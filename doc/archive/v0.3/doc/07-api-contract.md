# API contract proposal
**Version:** 0.3 · **Updated:** 22 October 2026 · **Base:** /api/v1 · **Status:** design only

## 1. Conventions
HTTPS, JSON, UTC ISO timestamps, UUID IDs and integer EUR gross cents. Company/account authority and guest job scope come from secure server sessions, never a supplied owner ID. Cookie-authenticated writes require CSRF/origin validation.

All private HTML, serialized route data, API and media responses are private/no-store and excluded from PWA/CDN caches. Cursor default 20/max 50; every page repeats authorization. No rival response totals in company DTOs.

Financial operations use actor+operation+Idempotency-Key and canonical request hash. Same key/different payload -> 409; repeated successful effect -> original resource, no new payment. Job writes take expected version/material revision. Authentication links exchange one-use tokens through a controlled POST; no permanent guest key in a public task URL.

Errors: 400 malformed, 401 login/session, 403 own-account restriction, 404 absent/inaccessible resource, 409 state/revision/idempotency conflict, 422 validation, 429 rate limit, 503 unavailable. An unavailable private object never leaks its owner or offer data. Authenticated owner may receive 410 TASK_EXPIRED after deletion; anonymous probes remain generic.

## 2. Routes
| Method/path | Principal | Result |
|---|---|---|
| POST /guest/jobs | Prospective guest, rate limited | Draft from short description/postcode/city/email; initiate email confirmation; no account required |
| POST /guest/access-requests | Guest requester | Generic success; email a fresh scoped link if eligible; no existence enumeration |
| POST /guest/token-exchanges | One-use valid token | Consume hash once, set scoped session, redirect safely |
| POST /companies | Company owner account | Self-declared profile and confirmed login email workflow, not trade verification |
| PATCH /companies/me | Owner | Services/base postcode/radius/contacts/optional website |
| GET/PATCH /companies/me/alerts | Owner | Explicit subscription and categories/radius/locale; DAILY_DIGEST default or INSTANT option; frequency update alone cannot opt in |
| POST /alerts/unsubscribe | Signed purpose-scoped unsubscribe token | Disable alerts without requiring a password; grant no task access |
| PATCH /jobs/{id} | Scoped guest task owner | Draft or material edit, expected version; preview/deletion timestamp |
| POST /jobs/{id}/publish | Confirmed job owner | Region/privacy/media readiness; first publish sets immutable delete_at |
| POST /jobs/{id}/close | Job owner | Stop responses/alerts/callback access and durably invalidate unpublished payment attempts; reconcile any captures |
| POST /jobs/{id}/reopen | Job owner | Voluntary closed task only, before delete_at; retain offers/fees/deadline, no re-charge, CLIENT_INITIATED pending fresh opt-in, old canceled attempts stay invalid |
| DELETE /jobs/{id} | Job owner | Immediate access denial, queued early erasure and minimal accounting separation |
| GET /jobs | Active company | Safe open task feed, category/region/approximate radius; free reading |
| GET /jobs/{id} | Active company or owner | Company-safe brief or owner DTO; never all company offers via a company principal |
| POST /jobs/{id}/response-drafts | Active company | Own rough bid/introduction/contacts; free draft; unique company/task record; 409 if that response was finally withdrawn |
| PATCH /responses/{id} | Own company | Edit eligible own draft/submitted response without second fee; WITHDRAWN is read-only, no edit-to-republish |
| POST /responses/{id}/publication-payments | Own company | Explicit EUR 1.00 intent, price/policy/material revision; no client fee; new purchase on WITHDRAWN returns 409 without PSP call |
| GET /me/payments/{id} | Paying company | Server-verified pending/published/refund state and receipt |
| GET /me/payments | Paying company | Recover owned pending attempts after browser/app termination |
| GET /me/responses | Own company | Own responses only, no rival IDs/counts |
| GET /jobs/{id}/responses | Job owner only | Eligible published offers plus company-name/withdrawn markers without offer text/company contacts, private cursor pagination |
| POST /responses/{id}/withdraw | Own company | Final published-offer withdrawal for company/task in MVP; client marker only, callback access ends, original fee/no automatic refund, no restoration/republishing/new checkout |
| GET/PATCH /jobs/{id}/contact-preference | Job owner | CLIENT_INITIATED / ALLOW_RESPONDERS and exact allowed fields/version |
| GET /jobs/{id}/client-contact | Own eligible published-response company | Dynamic callback consent read; no contact in default mode or after close/expiry/withdraw |
| POST /jobs/{id}/photos | Job owner | Private quarantine upload; enforce confirmed six-photo/task and 10 MB/file limits server-side, including concurrent in-flight uploads; real parent scope |
| POST /photos/{id}/complete | Upload owner | Queue scan/transform, not “safe” by client assertion |
| GET /photos/{id}/download | Authorized task reader | Worker-authorized sanitized bytes with current permission/expiry checks; no public or reusable direct-object capability bypass |
| POST /reports | Scoped reporter; bounded public illegal-content intake separately | Case/category/target, minimal evidence, status/correction route |
| POST /me/privacy-requests | Reauthenticated owner/verified guest | Scoped export/delete request with third-party minimization |
| POST /webhooks/mollie | Bounded classic form body with payment id; no browser session or signed event envelope | 400 malformed; 200 after durable verification scheduling for known IDs, generic 200 for unknown well-formed IDs; 503 if known work cannot be durably accepted; no payment decision from body |
| POST /webhooks/resend | Bounded raw body with verified Resend signature/Svix headers; no browser session | 400 invalid; 200 after minimal durable inbox acceptance/deduplication; 503 if persistence fails; effects resolve only platform delivery references, including later association |

Client registration, multi-task account dashboards and `/account/link-guest-job` are deferred until after MVP and are not implemented as active MVP routes. Company/staff accounts grant no automatic guest-task ownership. See [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md).

The selected provider route is `/api/v1/webhooks/mollie`, using classic Payments API notifications configured through `webhookUrl`. Treat `id=tr_...` as an untrusted lookup hint: only known locally associated payments schedule verification, and authenticated Mollie retrieval validates mode/profile/amount/attempt and current payment/refund facts. Unknown well-formed IDs get generic 200 without disclosure or arbitrary external fetches; create-timeout reconciliation recovers a provider ID not yet saved locally. Repeated IDs may represent later refund/chargeback changes, so they are not permanently deduplicated. Exempt this exact Mollie notification route from cookie CSRF; keep company purchase CSRF/ownership checks. Neither payment IDs nor return URLs grant access or prove payment. See [Mollie mapping](05-technical-design.md) and [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md).

The selected email event route is /api/v1/webhooks/resend. Verify raw bytes and the endpoint signing secret against svix-id, svix-timestamp and svix-signature before durable acceptance; do not copy cloud-mail's JSON-only handler. Deduplicate event IDs, allow later event types for the same email and reconcile events that precede the local send-result association. Exempt only this exact Resend event route from cookie CSRF within the email integration; user account/task/offer actions retain their own controls. Delivery events never authorize task access or change payment fulfillment. See [Resend mapping](05-technical-design.md) and [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md).

Operator routes under /ops/v1 need staff MFA and action-specific permission: report/hold/notice/appeal, fee exception/refund, delivery suppression, deletion retry and reconciliation. No “verify workmanship” action or arbitrary balance editor.

## 3. DTO examples
```json
{
  "id": "00000000-0000-4000-8000-000000000001",
  "category": "windows_doors",
  "city": "Drachten",
  "postcodeArea": "9203",
  "description": "Replace two internal doors; site measurement preferred.",
  "approxDistanceKm": 18,
  "state": "OPEN",
  "materialRevision": 1,
  "deleteAt": "2027-01-21T09:00:00Z",
  "readingFeeGrossCents": 0,
  "firstPublishedResponseFee": {"grossCents": 100, "currency": "EUR", "vatIncluded": true},
  "photoIds": []
}
```

No full postcode, client identity/contact, guest credential, response count or competitor object appears. Registered company reading is free; photo authorization still checks task state/deadline.

```json
{
  "responseId": "00000000-0000-4000-8000-000000000002",
  "status": "PAYMENT_PENDING",
  "paymentId": "00000000-0000-4000-8000-000000000003",
  "feeGrossCents": 100,
  "currency": "EUR",
  "checkoutUrl": "https://psp-approved-checkout.example/opaque"
}
```

Example checkout host is illustrative, not a production allowlist. Obtain the Mollie URL from the server-created payment's `_links.checkout.href`, apply reviewed HTTPS/provider-host controls and redirect by GET, such as HTTP 303; reject caller-supplied return/checkout URLs. A PUBLISHED result means authenticated verification and committed fulfillment. REFUND_PENDING means no client-visible response on that attempt, not a wallet credit. Same-key replay recovers the original owned outcome reference; a fulfilled or finally withdrawn response cannot create another fee. Local idempotency and reconciliation outlive Mollie's one-hour credential-scoped request cache.

The server restricts payment creation to validated, merchant-approved iDEAL/Wero and card methods. Client input and dashboard-enabled methods cannot expand that MVP allowlist. Verify the actual method through authenticated retrieval before publication; unsupported-method capture follows the paid-unpublished refund workflow. Each method needs successful €1 payment, full-refund and phone/PWA return evidence before activation.

## 4. Consent/edit/closure semantics
A callback-mode write includes expected consent/job version, chosen email/phone fields and acknowledged audience wording. Never default to callback because a phone exists. Contact endpoint checks current mode/fields and published fee-backed own response each request. Previously copied details cannot be revoked remotely.

Confirmed reopening retains existing offers/fee history and original expiry, with no new fee for previously published companies. Reset contact mode to CLIENT_INITIATED with a new consent version; require a fresh explicit opt-in before any callback disclosure. Close invalidates unpublished attempts durably, so reopening does not let an old checkout/callback publish; a captured invalidated attempt refunds. Expired/deleted tasks and operator holds cannot be reopened by the client. See [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).

Confirmed published-offer withdrawal is terminal in MVP. No client DTO may serialize the withdrawn offer body, website or company contact fields; the client sees only company name and withdrawn status. Reopening a task, updating an offer or replaying an old payment cannot undo WITHDRAWN. A historical payment replay may recover the existing owned receipt with current response status, but never creates checkout or publication. Restoration has no active MVP route.

Confirmed material-edit policy: requested-work changes increment scope revision. At fulfillment, an unpublished offer's payment snapshot must match the current revision or any captured payment receives a full refund. Already published offers remain visible to the client with a scope-change label and are editable free, without automatic refund or another fee; company edits must acknowledge the current scope before presenting the offer as current. See [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md). Close/reopen does not reset retention or paid uniqueness. Deleted task response bodies are not recoverable from payment APIs.

## 5. Cross-platform and resilience
Same endpoints for desktop/Android/iPhone PWA. Browser cookies need not transfer across installed contexts; recover via authenticated /me/payments and owned status. No background/offline write queue or payment-success inference from redirect.

Initial tunable bounds: three guest drafts/email-hour, five access-link requests/hour/target plus network limits, 60 safe feed reads/minute/company, ten response/payment attempts/minute/company. Adaptive abuse controls may suspend actors; these are not a commercial three-company participant cap. Reject unknown fields, arbitrary PSP redirect URLs, unsupported media and oversized input.

Confirmed photo limits are six per task and 10 MB per file, independent of upload batching. Enforce against actual bytes and shared per-task capacity, including in-flight reservations, so overlapping uploads cannot exceed six. A lost response/repeated completion does not create another attachment; rejected/failed uploads have bounded cleanup. The confirmed input allowlist is JPEG, PNG, WebP and iPhone HEIC; only validated sanitized JPEG/WebP becomes readable. Decoder/pixel limits and processor/runtime qualification remain work. Upload completion queues processing and never trusts a client assertion that the file is safe; a file awaiting/failed conversion is not exposed as the original.
