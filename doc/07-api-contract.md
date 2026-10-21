# API contract proposal
**Version:** 0.3 · **Base:** /api/v1 · **Status:** design only

## 1. Conventions
HTTPS, JSON, UTC ISO timestamps, UUID IDs and integer EUR gross cents. Company/account authority and guest job scope come from secure server sessions, never a supplied owner ID. Cookie-authenticated writes require CSRF/origin validation.

All private HTML/RSC/API/media responses are private/no-store and excluded from PWA/CDN caches. Cursor default 20/max 50; every page repeats authorization. No rival response totals in company DTOs.

Financial operations use actor+operation+Idempotency-Key and canonical request hash. Same key/different payload -> 409; repeated successful effect -> original resource, no new payment. Job writes take expected version/material revision. Authentication links exchange one-use tokens through a controlled POST; no permanent guest key in a public task URL.

Errors: 400 malformed, 401 login/session, 403 own-account restriction, 404 absent/inaccessible resource, 409 state/revision/idempotency conflict, 422 validation, 429 rate limit, 503 unavailable. An unavailable private object never leaks its owner or offer data. Authenticated owner may receive 410 TASK_EXPIRED after deletion; anonymous probes remain generic.

## 2. Routes
| Method/path | Principal | Result |
|---|---|---|
| POST /guest/jobs | Prospective guest, rate limited | Draft from short description/postcode/city/email; initiate email confirmation; no account required |
| POST /guest/access-requests | Guest requester | Generic success; email a fresh scoped link if eligible; no existence enumeration |
| POST /guest/token-exchanges | One-use valid token | Consume hash once, set scoped session, redirect safely |
| POST /account/link-guest-job | Authenticated account + proven guest scope | Optional ownership link, no TTL extension |
| POST /companies | Company owner account | Self-declared profile and confirmed login email workflow, not trade verification |
| PATCH /companies/me | Owner | Services/base postcode/radius/contacts/optional website |
| GET/PATCH /companies/me/alerts | Owner | Opt-in categories/radius/frequency/locale |
| POST /alerts/unsubscribe | Signed purpose-scoped unsubscribe token | Disable alerts without requiring a password; grant no task access |
| PATCH /jobs/{id} | Guest/account job owner | Draft or material edit, expected version; preview/deletion timestamp |
| POST /jobs/{id}/publish | Confirmed job owner | Region/privacy/media readiness; first publish sets immutable delete_at |
| POST /jobs/{id}/close | Job owner | Stop responses/alerts/callback access |
| POST /jobs/{id}/reopen | Job owner | Voluntary closed task only, before delete_at; no renewal of deadline |
| DELETE /jobs/{id} | Job owner | Immediate access denial, queued early erasure and minimal accounting separation |
| GET /jobs | Active company | Safe open task feed, category/region/approximate radius; free reading |
| GET /jobs/{id} | Active company or owner | Company-safe brief or owner DTO; never all company offers via a company principal |
| POST /jobs/{id}/response-drafts | Active company | Own rough bid/introduction/contacts; free draft; unique company/task record |
| PATCH /responses/{id} | Own company | Edit own draft/submitted response without second fee |
| POST /responses/{id}/publication-payments | Own company | Explicit EUR 1.00 intent, price/policy/material revision; no client fee |
| GET /me/payments/{id} | Paying company | Server-verified pending/published/refund state and receipt |
| GET /me/payments | Paying company | Recover owned pending attempts after browser/app termination |
| GET /me/responses | Own company | Own responses only, no rival IDs/counts |
| GET /jobs/{id}/responses | Job owner only | All eligible published offers, private cursor pagination |
| POST /responses/{id}/withdraw | Own company | Withdraw offer and end callback eligibility, no new purchase slot |
| GET/PATCH /jobs/{id}/contact-preference | Job owner | CLIENT_INITIATED / ALLOW_RESPONDERS and exact allowed fields/version |
| GET /jobs/{id}/client-contact | Own eligible published-response company | Dynamic callback consent read; no contact in default mode or after close/expiry/withdraw |
| POST /jobs/{id}/photos | Job owner | Bounded private quarantine upload, real parent scope |
| POST /photos/{id}/complete | Upload owner | Queue scan/transform, not “safe” by client assertion |
| GET /photos/{id}/download | Authorized task reader | Clean download capability expiring no later than delete_at |
| POST /reports | Scoped reporter; bounded public illegal-content intake separately | Case/category/target, minimal evidence, status/correction route |
| POST /me/privacy-requests | Reauthenticated owner/verified guest | Scoped export/delete request with third-party minimization |
| POST /webhooks/mollie/payments | Bounded PSP notification | Durable verify task; 200 accepted/duplicate, 503 if cannot durably ingest |

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

Example checkout host is illustrative, not a production allowlist. A PUBLISHED result means the server verified payment and committed fulfillment. REFUND_PENDING means no client-visible response on that attempt, not a wallet credit. Same-key replay returns original outcome reference; a previously fulfilled response with a new key must not create a second fee.

## 4. Consent/edit/closure semantics
A callback-mode write includes expected consent/job version, chosen email/phone fields and acknowledged audience wording. Never default to callback because a phone exists. Contact endpoint checks current mode/fields and published fee-backed own response each request. Previously copied details cannot be revoked remotely.

Material scope edits increment revision. Pending payment snapshots must match at fulfillment or refund. Already published offers show scope-changed status to client/company and remain editable without re-charge. Close/reopen does not reset retention or paid uniqueness. Deleted task response bodies are not recoverable from payment APIs.

## 5. Cross-platform and resilience
Same endpoints for desktop/Android/iPhone PWA. Browser cookies need not transfer across installed contexts; recover via authenticated /me/payments and owned status. No background/offline write queue or payment-success inference from redirect.

Initial tunable bounds: three guest drafts/email-hour, five access-link requests/hour/target plus network limits, 60 safe feed reads/minute/company, ten response/payment attempts/minute/company and five photos per upload batch within six/task. Adaptive abuse controls may suspend actors; these are not a commercial three-company participant cap. Reject unknown fields, arbitrary PSP redirect URLs, unsupported media and oversized input.
