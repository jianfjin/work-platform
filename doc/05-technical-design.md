# Technical design

**Status:** proposed; algorithms and SQL fragments are design contracts, not delivered application code.

## 1. Invariants

1. `(job_id, business_id)` identifies at most one lifetime entitlement; it can never incur a second debit.
2. A job has at most three lifetime entitlements. The lifetime counter never decreases on withdrawal/restoration.
3. Successful unlock commits a debit, its source allocations, entitlement, slot and outbox event together; otherwise none commit.
4. Spendable balance is never negative. Disputed-payment debt is a separate, explicit record, not an unexplained negative access balance.
5. Each PSP payment grants one top-up lot at most. A redirect or callback-supplied status cannot authorize a grant.
6. Every money movement has a unique business reference, balanced journal and source-lot provenance. Historical entries are append-only.
7. Each entitlement can have at most one service restoration. Refunded access cannot be resold to the same business or free a fourth slot.
8. A private conversation belongs to one entitlement and its client; another paid participant is still unauthorized.
9. A job has at most one awarded business. Quote acceptance checks latest scope revision, validity and concurrency version.
10. External delivery failures cannot undo a committed financial or message transaction.

## 2. Authentication and authorization

Use secure, HttpOnly, SameSite cookies; enforce CSRF/origin checks for cookie-authenticated mutations. Re-derive principal, roles, active business membership and suspension state on the server; do not accept a business ID as proof of authority.

Create a server-only data access layer returning allowlisted DTOs, following the [Next.js data-security guidance](https://nextjs.org/docs/app/guides/data-security). Middleware and hidden buttons are not sufficient. Route handlers and server actions must call the same domain authorization functions.

Queries must select only permitted columns. Never load a full job/private thread and rely on React to omit sensitive fields. Private routes send `Cache-Control: private, no-store`; no shared response, RSC, CDN, search, analytics or service-worker caches for private data.

Object-read policy evaluates account status, client ownership or entitlement, job safety hold, participant state, channel ownership and consent grant for each contact field. Use 404 for inaccessible job/conversation/attachment IDs to limit enumeration; report stable authorized business conflicts separately.

## 3. Atomic unlock protocol

Input: authenticated actor, job ID, expected brief revision, price version, policy version and idempotency key. Amount is server-derived: 100 EUR gross cents under pilot price version P1.

```text
1. Authenticate; require active verified business and allowed user role.
2. Begin transaction; claim actor+operation+idempotency key with payload hash.
   Same key + different payload -> 409 IDEMPOTENCY_CONFLICT.
   Same committed request -> return stored receipt (after current access checks).
3. SELECT job FOR UPDATE.
4. Look up unique business/job entitlement.
   If it exists, return its receipt/status without charging, even with a new key.
5. Require OPEN, not expired, recently client-confirmed, not own job,
   no safety restriction, same revision/price/policy and lifetime_slots_used < 3.
6. SELECT wallet FOR UPDATE; lock eligible source lots in stable FIFO order.
7. Require available_gross_cents >= 100 and account has no payment hold.
8. Create entitlement, allocate 100 cents from available paid lots, append
   balanced spend journal, reduce materialized wallet/lot available amounts.
9. Increment lifetime_slots_used; create private channel and outbox event;
   store idempotency result (receipt ID, not private message content).
10. Commit. Return receipt; fetch entitled content through normal authorized read.
```

An insufficient balance, filled job, stale terms, changed revision or closed state leaves no debit or entitlement. Post-commit timeout uses the same key to retrieve the result. Concurrent fresh keys still hit the business/job uniqueness guard. Do not use a distributed cache or Redis counter as the capacity authority.

Use PostgreSQL row locking as described in [official locking documentation](https://www.postgresql.org/docs/current/explicit-locking.html). READ COMMITTED with consistent explicit locks is adequate for this bounded invariant; verify actual migrations/query plans and concurrent behavior.

**Lock order:** sorted job rows → wallet → sorted source lots → child records. Job-related unlock/restoration paths follow this order. Top-up and cash-refund paths may lock wallet/lots only and must never then acquire a job lock. Payment dispute handlers place a wallet/account hold first; job revocations occur in a separate transaction/task. All administrative paths use these same services. Retry deadlocks/serialization failures a bounded three times with the same operation key; return retryable 503 after that.

## 4. Top-up and PSP protocol

1. Server creates a top-up intent for exactly EUR 10.00, currency EUR, authenticated business and immutable policy/billing details. Never accept an arbitrary client-submitted amount.
2. Persist the intent and a create-payment attempt token. Worker/API calls PSP using its idempotency facility when verified supported; otherwise retain an intent/attempt mapping and reconcile ambiguous calls before any new creation. Do not blindly retry payment creation with a new reference.
3. Save PSP payment ID and approved checkout URL, then redirect. Reference [Mollie's create-payment API](https://docs.mollie.com/reference/create-payment). No secret credentials or private job data in metadata/redirect URLs.
4. Callback receives a bounded payment identifier. Match against a stored intent. Persist an inbox/verification task before acknowledging receipt; unknown IDs have no grant effect. Enforce request size/rate limits.
5. Worker retrieves the payment using the authenticated PSP API. Validate ID, correct merchant/environment, EUR currency, EUR 10.00 amount and internal intent reference. Only verified `paid` can grant funds. Validate callback authenticity if the selected PSP product offers signatures; do not invent a signature requirement for a legacy endpoint that has none.
6. In one wallet-locked transaction, create the unique paid lot and grant journal, mark the top-up credited, and enqueue receipt delivery. Duplicate observations cannot grant again.
7. Return-page polling reads our authenticated top-up status, not PSP query parameters. An early browser return stays pending. Periodic reconciliation re-fetches unresolved intents and unsettled records, covering missing callbacks.

These verification decisions implement our service policy; PSP callback/hosted-checkout mechanics are described in [Mollie's payment flow](https://docs.mollie.com/docs/accepting-payments). Detailed payment states are in [official status documentation](https://docs.mollie.com/docs/handling-payment-status). A `paid` purchase can later have a refund or chargeback: track those separately rather than treating paid as immunity to reversal.

## 5. Financial restoration and cash refund

**Service restoration:** under a job lock, evaluate versioned policy facts; lock wallet/lots; insert a unique entitlement restoration; reverse the original 100-cent spend allocations/journal; restore the original source lots; set entitlement READ_ONLY_RESTORED; write audit and outbox. Repeated worker/operator attempts return the existing restoration. An original source lot already under payment dispute is restored into held value/debt resolution, not made newly spendable.

**Unused cash refund:** finance locks wallet/lots, calculates source-payment refundable value, reserves eligible funds and creates a refund intent. Funds become unavailable before the PSP request. Submit through the PSP refund API with stable attempt identity, then verify provider result. On success journal the cash refund and settle the reservation. On verified failure release it; on ambiguous timeout keep it reserved and reconcile. Never both release a reservation and retry an unknown refund blindly. Refunds can span multiple payments only as separately tracked provider refunds.

**Chargeback/reversal:** freeze the disputed source lot and block new purchases; remove unspent disputed value through an auditable reversal; record already-consumed exposure as a separate debt/dispute. Never subtract an already refunded or reversed cent twice. Reconciliation and finance decide recovery; no silent clawback from unrelated balances. Details and caps are in [Payments](08-payments-and-economics.md).

## 6. Job edits, award and expiry races

Publishing creates immutable revision 1 and an approved safe preview. Non-material typo corrections are logged. Changed dimensions/materials/budget/date/scope create a new immutable revision, pause new unlocks and notify existing participants. Approved resumption needs client reconfirmation; existing access is never charged again. A quote can only be selected after acknowledging the latest revision.

Award locks the job, evaluates any already-due entitlement restoration policies, validates expected job version and current quote from an ACTIVE entitlement, writes the selected business/quote once and marks the job AWARDED. Losing channels become read-only by authorization policy; the winner remains writable. A competing award or cancellation returns 409 with updated state. Ordinary selection/cancellation does not charge money; separately earned restorations may still settle during the locked policy evaluation.

The expiry worker and reconfirmation worker use the same job lock and current-time predicates as unlock. `expires_at = first_published_at + 14 days` is fixed. Unlock rejects overdue jobs even if the worker has not yet updated the state. No-response restoration timing uses stored UTC event timestamps and eligibility facts, not notification-delivery timestamps. Before accepting private writes or awards, evaluate already-due restorations under that job lock: a reply/selection at or before the deadline counts, a later one cannot undo eligibility. Worker lag must not change the purchased policy.

## 7. Conversations, quotes and media

- Create exactly one private channel for an entitlement. Message insert checks channel ownership and write state, then appends message/outbox in one transaction. Sender-generated message IDs deduplicate retries.
- Store text as plain text or a small sanitized markdown subset; never execute arbitrary HTML. Escape outputs and validate links. Shared board excludes commercial offers/contact data; automated flags assist, not replace, moderator review.
- Quote versions are immutable; server calculates totals in integer cents and checks VAT fields and rounding. A chosen quote references its exact version and brief revision. Corrections create a new version.
- Initially poll new messages with an authenticated cursor every 10–15 seconds while visible; pause in background. WebSockets can wait. Cursor authorization is independent of cursor possession.
- Allow JPEG/PNG/WebP and PDF, up to 10 MB each and ten files per job initially. Detect real media type; cap decoded image pixels and PDF pages; quarantine, scan, strip EXIF and derive sanitized variants. Serve PDFs as attachments with safe headers; no SVG/HTML/executable uploads.
- Authorized downloads use at most 60-second signed links after checking attachment parent ownership; for especially sensitive files proxy access if immediate revocation is required. A minted bearer link may remain usable until its short expiry; disclose this limitation.

## 8. Async work, scheduling and telemetry

Outbox fields include event ID, aggregate ID, sequence, type, minimal payload, attempts, available time and lease. Worker claims rows with leases/`SKIP LOCKED`; retries with backoff and dead-letters after ten failures. Each consumer records processed event IDs. Outbox does not contain message text, addresses or payment secrets. Notifications link to authenticated content.

Scheduled work: job freshness/expiry, no-response restoration, unresolved PSP/payment/refund reconciliation, retention deletion and daily financial checks. Processes must be restartable and idempotent. Financial rules use database time; user display uses Europe/Amsterdam.

Trace request → domain operation → journal/entitlement → outbox using non-secret correlation IDs. Log decisions/reason codes, never full bodies/contact data. Alert immediately on ledger imbalance, duplicate charge/grant or capacity breach; stop purchases until reviewed. See [Test plan](11-test-and-launch-plan.md) for proof obligations.

## 9. PWA implementation boundary

Implement one HTTPS Web/PWA with a stable manifest ID, scoped start URL, standalone display, appropriate icon variants and installation instructions; use [Next.js's PWA guidance](https://nextjs.org/docs/app/guides/progressive-web-apps) for framework integration, not as a security-complete cache recipe. No native binaries or store submission in this phase.

The browser service worker has an exact allowlist for versioned public CSS/JavaScript/fonts, icons and a non-personal offline HTML page. All APIs, personalized HTML/RSC, authentication/payment/return routes, media/signed URLs and mutations bypass its cache. A generic cache-first/stale-while-revalidate navigation strategy is forbidden. HTTP `no-store` is necessary but not sufficient: worker code must never put these responses in Cache Storage. No IndexedDB/localStorage copies of private threads, balances, tokens or media; unsent sensitive drafts remain in view memory only. Server-saved drafts resume online.

Offline mutations fail clearly; do not add Background Sync or an automatic financial/message replay queue. On reconnect, foreground or back-forward restore, hide stale private views until the current session, job state, balance and permissions are revalidated. A network/status failure never indicates successful payment or message send. Preserve a non-secret operation reference where needed, then query an owned intent/entitlement to resolve ambiguity before another purchase; the server's lifetime uniqueness/idempotency remains authoritative across devices.

Serve worker updates revalidatably, version public caches and delete obsolete app-owned caches after safe activation. Do not force `skipWaiting` plus reload while a user is composing or in checkout. Keep old/new API clients compatible during rollout, signal updates, and require save-online/discard confirmation before reload. Logout/account-switch clears current private UI, in-memory drafts and any accidental app-owned private caches across open same-origin contexts; refetch before displaying another account. Do not depend on cookie/session sharing between Safari, installed iPhone PWA and bank-app return browser.

No PWA-specific financial schema or duplicate API is needed. Optional future push subscriptions are a separate feature with permission, ownership, revocation/expiry and delivery security design; P0 notifications remain email plus in-app polling. Detailed release tests and recovery are in [Cross-platform delivery](14-cross-platform-and-pwa.md).
