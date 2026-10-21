# Technical design
**Version:** 0.3 · **Updated:** 21 October 2026 · **Status:** proposed algorithms, no delivered application code

## 1. Invariants
1. Client platform fee is zero. Only the company initiates the proposed €1 response payment.
2. One company/task response; at most one retained successful publication fee for it. Retries/edits/withdrawals do not create another charge.
3. No response is visible without a verified payment or a separately approved, auditable promotion (none enabled in MVP).
4. Paid but ineligible/unpublished attempts must reach a tracked full-refund outcome; an ambiguous external result cannot be retried blindly.
5. Company can read safe tasks free, never rivals' responses or counts. Client job owner sees all its eligible submitted offers.
6. Client contact is disclosed only through current explicit mode/field permission plus an eligible published response on an OPEN task.
7. Every task read/send/write/download checks delete_at; a late worker does not extend access.
8. Photos/text/offers/contact grants/guest access and derived task location are purged by policy; financial records contain no unnecessary task/client content.
9. No private PWA cache or queued financial mutation.

## 2. Guest publishing and optional accounts
A guest provides email for management, not a compulsory account. Use a random high-entropy one-use token; store only its hash, purpose, job ID and short expiry (proposed 15 minutes). Resend is rate limited and generic; no email/job enumeration. Exchange token via a controlled POST into an HttpOnly/Secure/SameSite job-scoped session; remove token from visible URL and exclude query/fragment credentials from logs/analytics/referrers. Do not turn email into a permanent privileged link.

Confirm email before publication; phone verification is not required for default client-initiated mode. Optional registration links jobs only after proving the same email/job ownership, never by a caller-supplied address string. Session revocation, deletion and account switch must work across open browser contexts. Company accounts use separate server-derived company ownership; an email-confirmed company is not certified.

## 3. Task and response authorization
A server-only DAL returns different DTOs for company task view, company own response, client offer list, contact disclosure and finance. Never serialize a raw ORM job/response relation and hide fields in React. Personal HTML/RSC/API responses are private/no-store, excluded from CDN/service-worker caches.

Company task view omits full postcode, client name/email/phone, management identifiers and all bidder metadata. Client listing query uses the owner principal/job ID and pagination without leaking cursor authority. Other companies' IDs/offer counts return no data, including aggregates, errors, exports and attachment parents.

A callback-contact read requires active account + PUBLISHED own response with valid retained fee + OPEN, unexpired, non-held job + current ALLOW_RESPONDERS grant for exactly the returned fields. Default CLIENT_INITIATED returns no client contact even after payment. Closing, withdrawal or revocation immediately prevents new disclosure.

Store/render descriptions and introductions as plain text or a strictly sanitized subset, escape outputs, parameterize queries and reject unknown mutation fields. Validate contact/website URL schemes; never execute HTML/JavaScript supplied in a rough bid. External website links use safe browser attributes and do not trigger automatic server-side preview fetching of arbitrary URLs. Apply CSP, CSRF/origin checks, request size/rate bounds and per-owner media authorization. No SVG/HTML/executable upload types.

## 4. Company payment to first-publication protocol
Draft offer is free. Fee is server-derived EUR 1.00 gross under fee policy R1; body includes expected material revision and accepted policy version, not an authoritative amount.

1. Authenticate company; validate safe contact/profile/rough offer. Claim actor+operation+idempotency key and canonical payload hash.
2. In job -> response lock order, ensure OPEN, now < delete_at, same material revision, not own task/held account. Find/create unique response row. If already fulfilled, return existing result at zero new charge.
3. Reuse any unresolved payment attempt. Create one pending attempt/reference and commit. Outside locks, create hosted PSP payment with supported stable idempotency; if outcome unknown, reconcile that attempt before creating another.
4. Persist provider IDs/checkout URL. For Stripe, accept only a signature-verified webhook with a bounded body; durably queue minimal event/object references for verification, not a publication decision. See the Stripe mapping below.
5. Fetch payment via authenticated PSP API. Verify merchant/environment, EUR, 100 gross cents and stored attempt reference. Redirect/body-supplied “paid” is not proof.
6. In job -> response -> payment-attempt lock order, evaluate current state/deadline/material revision and ensure the attempt has no reserved/pending/completed refund or reversal. If valid and not previously fulfilled, atomically mark offer PUBLISHED, create unique fulfillment/fee journal and minimal outbox event. Receipt is recoverable after timeout.
7. If task closed/deleted/held/materially changed, or another valid attempt already fulfilled, do not publish this payment; create one full-refund obligation. Never substitute a wallet balance or auto-pay for another task.
8. Repeated callback/return/retry gets the existing result. Reopening a voluntarily closed job before delete_at does not recharge an existing fulfilled response. After a definitive refunded failed attempt, a new deliberate reviewed attempt is possible; no two retained successful fees.

All writers, including staff tools, use the same lock order and uniqueness constraints. Close/edit/delete races serialize through the job row. Provider observation can precede a lock; the business decision uses state under that lock. Use bounded deadlock retry with the same operation identity. See [PostgreSQL locking](https://www.postgresql.org/docs/current/explicit-locking.html) and [Stripe Checkout fulfillment](https://docs.stripe.com/checkout/fulfillment?payment-ui=stripe-hosted); these references do not prove future code correct.

### Stripe implementation mapping (preferred candidate, not implemented)
- Use the official [stripe-node library](https://github.com/stripe/stripe-node), installed as `stripe`, only in the Node.js server/worker. At implementation, pin a supported stable SDK, maintained Node LTS and compatible non-preview API version; align webhook event version and test upgrades. Do not put secret keys in browser/PWA bundles.
- Create a hosted Checkout Session with `stripe.checkout.sessions.create`, `mode: payment`, one server-owned EUR 100-cent fee item/quantity one. Disable adjustable quantities, discounts and tax additions that could change the approved €1 gross total. VAT treatment/documentation is an accountant-approved policy, not an automatic Stripe Tax configuration. Approved company billing fields only; no client job/contact/photo content in Stripe metadata.
- Persist an internal attempt before the call; send its stable opaque key in SDK request options `idempotencyKey`. Store Checkout Session, PaymentIntent and later charge IDs separately. Session/PaymentIntent metadata use opaque attempt/response references, explicitly set where needed rather than assuming automatic propagation. Return URLs come from server configuration, not user input.
- On `POST /api/v1/webhooks/stripe`, read the unmodified raw body (e.g. `request.text()` in a Next.js Node route), then call `stripe.webhooks.constructEvent(rawBody, signature, endpointSecret)` with `Stripe-Signature`. Reject invalid/missing signatures with 400. Use the endpoint's environment-specific signing secret, not an API key or another endpoint's secret. Keep normal signature timestamp validation.
- After verification, persist minimal event ID/type/object ID/environment/version plus an outbox job atomically; unique merchant/environment/event ID handles delivery replay. Return 200 only after durable acceptance (also duplicates/irrelevant valid types); ingestion failure returns 503. Exempt only this signed server-to-server route from cookie CSRF, not company purchase routes. See [webhook guidance](https://docs.stripe.com/webhooks).
- `checkout.session.completed` and `checkout.session.async_payment_succeeded` schedule the same verification/fulfillment service. For this paid-only product require authenticated retrieval with `payment_status: paid`, matching `mode`, merchant/livemode, currency, total, fee item, owned attempt, successful PaymentIntent and unreversed/unrefunded captured funds; a completed-but-unpaid session is not enough. Delayed failure/expiration schedules reconciliation, never overwrites newer paid facts. Do not enable unimplemented payment methods.
- Browser return can request that same server verification only after owned-attempt authorization; the URL/session ID itself grants no access or payment proof. Background reconciliation recovers missed events without a browser visit. Local unique fulfillment/effect constraints handle concurrent events and returns.
- Stripe API v1 idempotency keys can be pruned after at least 24 hours; cached responses can include 500 errors. Preserve local attempt/effect uniqueness beyond that window. For unknown results, retrieve known objects or reconcile the merchant's recent records using opaque references before any new create; escalate unresolved ambiguity instead of recycling a key blindly. See [Stripe idempotency](https://docs.stripe.com/api/idempotent_requests).

## 5. Refund/accounting recovery
Use separate payment, publication and refund states. A captured payment is not earned income until the offer is fulfilled. A valid visible offer later losing work/no reply/client closure is not automatically refundable: the platform cannot observe private calls or construction success.

For failed publication/system duplicate/confirmed invalid listing under R1, reserve a unique refund amount <= captured less prior refund/reversal before calling PSP. Submit with stable attempt identity; verify provider result. Unknown timeout stays pending, not released then resent. Late payment/extra capture routes to refund rather than a second fulfillment. Chargebacks are separately recorded and reconciled; no silent negative wallet because there is no MVP wallet.

For Stripe, use `stripe.refunds.create` against the recorded successful PaymentIntent/charge and a stable refund-obligation `idempotencyKey`. Persist the Stripe refund ID; retrieve status and process relevant signed refund/dispute events through the same inbox. A created/pending refund is not a completed refund. Failed/canceled/requires-action outcomes need an accountable exception workflow; unknown outcomes keep their reservation while reconciled. Confirm the selected method's refund window and business refund policy before charging. See [Stripe refunds](https://docs.stripe.com/refunds).

Journal is append-only with unique effect references and balancing controls. Keep minimum company/payment/invoice/time/opaque task-reference evidence; no photos, raw client contact or rough-offer body in long-lived PSP metadata/finance logs. Accountant approves VAT/tax point/invoice corrections and legal retention.

## 6. Region and radius matching
Normalize Dutch postcode/city and validate against the approved pilot allowlist. Treat Groningen province versus city as a final owner decision. Drachten belongs within the Friesland/Fryslân trial definition, not a separate province.

Use provider-returned/licensed coarse postcode-area or city centroid coordinates, and explicit CRS transformation to WGS84 where needed. Match category + company radius using geodesic/straight-line distance (e.g. Haversine); label it approximate, not driving distance. Suggested radius options 10/25/50/100 km, with a proposed 1–200 km validated range. The company controls radius; no GPS permission is required.

PDOK Location API is the recommended first Dutch provider candidate; verify dataset terms, quotas and coordinate metadata at integration. Google Geocoding is an optional adapter with key restrictions, billing quotas/alerts and licensing-approved caching/attribution. Do not assume Google results can be copied into a permanent unrestricted postcode database. If geocoding fails, retain draft/show retry or manually confirmed coarse area; do not invent zero-distance coordinates or send unrelated alerts.

## 7. Opt-in task email notifications
On task publication or eligible material revision, enqueue minimal matching work. A confirmed subscribed company receives safe category/city/PC4/timing metadata and an authenticated link, not photos, full postcode, client contacts or guest tokens. Do not send the full post to arbitrary SME email lists.

Store category/radius/frequency/locale preferences and consent evidence. Proposed default daily digest, optional instant, editable/unsubscribe. Unique company+job+revision+notification-kind delivery key prevents repeated campaigns. At dispatch recheck task OPEN/not expired/not held, company active, match and subscription; unsubscribe/close suppress queued sends. Use bounded retries, delivery status and bounce suppression. External accepted email cannot be recalled; no job body in it limits residual exposure.

## 8. Three-calendar-month deletion
Compute immutable delete_at from first_published_at in Europe/Amsterdam, add three calendar months, clamp month-end to last valid date, convert to UTC. It is not “90 days after last activity.” A 31 January publication expires 30 April at the corresponding local time. Proposed DST rule: shift a nonexistent target local time forward by the clock-change gap; for a repeated local time choose the earlier instant. Store the resolved deadline once, show it before posting and test both cases. This calendar policy is part of the owner-approved retention wording.

At delete_at authorization denies all task reads/writes/downloads/contact/notifications. Idempotent worker marks inaccessible, revokes sessions/tokens/grants, removes response text/contact snapshots and task location, then deletes originals/variants/quarantine/object versions and any search/cache/provider copies permitted under contracts. Proposed live-store completion <=24h; failed deletes retry with alerts and per-target progress. Keep only minimal tombstone/reference needed for deletion and lawful finance.

A new offer, close/reopen or optional account creation never moves delete_at. No deletion job payload may contain the content being deleted. Rolling backups expire <=30 days; before restore opens, reapply tombstones and deadline filters. Legal holds are case-scoped, minimal, separately protected and reviewed, not an indefinite full-job archive.

## 9. Media, PWA and observability
Initial photo formats JPEG/PNG/WebP, up to six files and 10 MB/file; real MIME/pixel limits, quarantine/scan, EXIF removal, sanitized preview and clear HEIC conversion/file alternative. Metadata stripping cannot guarantee every picture lacks an address/face: client preview and report/remove path remain necessary. Signed/proxied reads must expire no later than delete_at; no public bucket URLs.

PWA caches exact allowlisted public build assets/icons/offline page only. No API/personalized HTML/RSC/media/offer/balance/auth/payment caching or Background Sync queue. Resume/reconnect revalidates session/task/consent/fee outcome; no automatic purchase replay. Persist only non-secret operation references if needed, not sensitive drafts; checkout can return to another browser and require login/recovery.

Inbox/outbox leased workers retry idempotently; notifications after source commit. Redact logs, guest tokens, postcode/contact and proposal bodies. Alert on duplicate retained fee, unpaid visibility, unresolved paid-unpublished/refund state, deletion lag or consent/data leak. See [PWA](14-cross-platform-and-pwa.md) and [Tests](11-test-and-launch-plan.md).
