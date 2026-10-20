# API contract proposal

**Base:** `/api/v1` · **Format:** JSON over HTTPS except multipart/presigned media and PSP form callbacks
**Status:** endpoint-level design; generate and validate an OpenAPI artifact during implementation task T02.

## 1. Shared conventions

- Secure session authentication; all mutations validate origin/CSRF as applicable. Actor and business membership come from the session, not body fields.
- UUID resource IDs; ISO-8601 UTC timestamps; integer cents; currency `EUR`; explicit `null` for unknown values rather than fabricated zeroes.
- Cursor pagination `{items, nextCursor}`; default 20, max 50. Every page repeats authorization checks.
- Financial mutations require `Idempotency-Key` (16–128 opaque characters). Scope: actor + operation + key; hash canonical body and route resource ID. Same key/different request returns 409.
- Retain operation lookup for at least 90 days as a proposed operational default; permanent entitlement/payment/reference uniqueness prevents duplicate effects after key cleanup.
- Mutable job/award actions require expected `version`. Return 409 if changed; no last-writer-wins award.
- Authenticated responses use `Cache-Control: private, no-store`. Error payloads never include an inaccessible object's attributes.

```json
{
  "error": {
    "code": "JOB_CAPACITY_REACHED",
    "message": "This job already has three participating businesses. Nothing was charged.",
    "retryable": false,
    "requestId": "req_example"
  }
}
```

Status taxonomy: 400 malformed; 401 sign-in required; 403 known account/verification restriction; 404 missing or inaccessible object; 402 insufficient access balance; 409 state/version/terms/idempotency conflict; 422 field validation; 429 rate limit; 503 temporarily unavailable. UI localizes stable codes, not arbitrary provider errors.

## 2. Endpoints

| Method/path | Authorized audience | Effect / response |
|---|---|---|
| `GET /me` | Signed-in actor | Minimal roles, business verification/hold flags, locale |
| `POST /businesses` | Eligible user | Create single-owner business, KVK and craft/service-area declaration; pending verification |
| `PATCH /businesses/me` | Business owner | Edit profile/availability; identity changes trigger recheck |
| `POST /jobs` | Verified client | Create draft; returns ID/version |
| `PATCH /jobs/{id}` | Owning client | Save draft or material revision using expected version |
| `POST /jobs/{id}/submit` | Owning client | Confirm genuine scope and preview; enter review queue |
| `POST /jobs/{id}/confirm-open` | Owning client | Record freshness; resumption subject to moderation/expiry rules |
| `POST /jobs/{id}/pause` | Owning client | Stop new unlocks, retain existing active conversations |
| `POST /jobs/{id}/cancel` | Owning client | Lock/update lifecycle; schedule policy evaluation for existing entitlements |
| `GET /jobs?category=&city=&radiusKm=&timing=` | Approved carpenter | Safe feed previews only; no private free-text search |
| `GET /jobs/{id}/preview` | Approved carpenter or owning client | Safe preview, revision/price/policy versions, capacity and current actor entitlement summary |
| `POST /jobs/{id}/unlocks` | Approved carpenter | Atomic €1 debit + entitlement; 201 new / 200 existing |
| `GET /jobs/{id}` | Owning client / permitted entitlement | Authorized full brief; contact fields still excluded |
| `GET /jobs/{id}/scope-questions` | Owning client / entitled participant | Shared non-commercial clarification only |
| `POST /jobs/{id}/scope-questions` | Active entitled participant | Add scope question; no commercial/contact data |
| `POST /scope-questions/{id}/answer` | Owning client | Publish shared answer with explicit audience |
| `GET /me/conversations` | Client or business owner | Only own private channels |
| `GET /conversations/{id}/messages?after=` | Channel participant | Authorized paginated messages |
| `POST /conversations/{id}/messages` | Writable channel participant | Deduplicated text/attachment message; 201/200 |
| `POST /jobs/{id}/quotes` | Active entitled business | New immutable quote version; expected brief revision |
| `GET /jobs/{id}/quotes` | Client: all own offers; business: only own | No competing offers for businesses |
| `POST /jobs/{id}/award` | Owning client | Atomic one-winner selection, expected job version and quote version |
| `POST /jobs/{id}/complete` | Client; winner may confirm afterward | Record client completion and independent winner confirmation |
| `POST /jobs/{id}/reviews` | Owning client of completed awarded job | One review with accurate evidence label |
| `POST /jobs/{id}/contact-grants` | Owning client | Explicit field-scoped grant to one entitled business |
| `DELETE /contact-grants/{id}` | Grant owner | Revoke future display/download; retain minimal consent audit |
| `GET /jobs/{id}/contact` | Client / specifically consented business | Selected fields only, no implicit payment-based sharing |
| `POST /uploads` | Authorized parent owner/participant | Presigned quarantine upload; typed parent and limits |
| `POST /uploads/{id}/complete` | Upload owner | Queue server-side scan; does not mark it safe |
| `GET /attachments/{id}/download` | Authorized parent reader | Safe short-lived download capability for clean media only |
| `GET /me/access-balance` | Business owner | Available/pending/reserved/held amounts, owned pending top-up references and journal cursor; permits checkout recovery after switching browser/PWA context |
| `POST /me/topups` | Approved business owner | Fixed €10 intent and hosted checkout URL; idempotent |
| `GET /me/topups/{id}` | Intent owner | Local pending/credited/refund/dispute status |
| `GET /me/operations/{key}?operation=unlock` | Original actor only | Operation receipt/resource/status, never a private body cache |
| `POST /entitlements/{id}/restoration-requests` | Entitlement owner | Open case or return existing restoration; cannot edit balances |
| `POST /me/cash-refund-requests` | Business owner | Request eligible unused paid value; status/reservation reference |
| `POST /reports` | Signed-in user; separate public illegal-content form | Case intake with typed target and bounded explanation |
| `POST /me/export-requests` / `POST /me/deletion-requests` | Reauthenticated user | Queued, identity-checked rights request |
| `POST /webhooks/mollie/payments` | Bounded external callback | Durable verification task; 200 on accepted/duplicate, 503 if durable ingest unavailable |

Internal operator endpoints live under `/ops/v1`, enforce MFA and per-action privileges, and append an audit record: approve/reject business/job, apply safety hold, decide restoration/refund, issue reasoned moderation decision, manage appeal and reconcile. An ordinary support role cannot grant itself finance or private-content access.

## 3. Free versus entitled DTOs

```json
{
  "id": "job_example",
  "approvedTitle": "Built-in shelving installation",
  "category": "cabinets_shelving",
  "city": "Utrecht",
  "distanceBucketKm": "5-15",
  "budgetRangeGrossCents": {"min": 80000, "max": 150000},
  "timing": "2-4 weeks",
  "measurementsProvided": true,
  "siteMeasurementRequested": true,
  "clientConfirmedAt": "2026-10-20T08:00:00Z",
  "lifetimeSlotsUsed": 2,
  "lifetimeSlotsMax": 3,
  "briefRevision": 1,
  "price": {"id": "P1", "grossCents": 100, "currency": "EUR", "vatIncluded": true},
  "policyVersion": "access-v1"
}
```

IDs in examples are placeholders; actual IDs must be UUIDs. Preview intentionally lacks description, client identity, photos, address, coordinates and raw source fields. Full DTO adds structured scope, safe attachments, shared Q&A access and the actor's private channel ID only. Contact is a separate consent-protected resource. Full DTO never includes a list of competitors' private channel IDs or offers.

## 4. Unlock contract

```http
POST /api/v1/jobs/{jobId}/unlocks
Idempotency-Key: client-generated-opaque-operation-key
Content-Type: application/json

{"expectedBriefRevision":1,"expectedPriceId":"P1","acceptedPolicyVersion":"access-v1"}
```

```json
{
  "entitlementId": "entitlement_example",
  "status": "ACTIVE",
  "chargedNowGrossCents": 100,
  "originalChargeGrossCents": 100,
  "availableBalanceGrossCents": 900,
  "receiptId": "receipt_example",
  "jobUrl": "/jobs/job_example"
}
```

An existing entitlement reached with a **new** key returns `chargedNowGrossCents: 0` and the original receipt. An identical-key replay returns the original operation receipt, labeled `replayed: true`; UI must not treat its historic charged amount as a new debit. Receipt lookup does not bypass present-day revocation/safety restrictions to expose job content.

Conflict codes: `JOB_NOT_OPEN`, `JOB_EXPIRED`, `JOB_CAPACITY_REACHED`, `STALE_BRIEF_REVISION`, `PRICE_CHANGED`, `POLICY_CHANGED`, `OWN_JOB`, `IDEMPOTENCY_CONFLICT`. Insufficient funds returns `INSUFFICIENT_ACCESS_BALANCE` plus the fixed permitted top-up offer; never auto-top-up.

## 5. Quote and message contracts

Quote input: expected brief revision; line descriptions; integer quantities/scaled decimal quantities; unit net cents; VAT rate/mode; exclusions; visit requirement; date range and validity. Server recomputes totals and rejects mismatches/negative values. Supported pilot VAT modes are explicitly displayed taxable rate or verified exemption/small-business scheme; the platform does not choose a contractor's tax treatment.

Message input includes `clientMessageId`, text up to 5,000 characters and clean attachment IDs owned by this actor/parent. Contact-sharing prompts are distinct from sending messages. A message on another provider's channel returns 404 even if the sender paid for the job.

## 6. Reliability/security envelope

Initial tunable rate limits: 60 preview reads/minute/account; 10 unlock attempts/minute/business; 30 message sends/minute/account; three new jobs/day/client; five top-up creations/hour/business. Use risk review rather than relying on IP alone for shared networks. Add independent callback bounds and worker deduplication. Return `Retry-After` where appropriate.

Bound query complexity, file sizes and list lengths. Reject unknown mutation fields. Validate external checkout redirect hosts against the PSP configuration; reject arbitrary user-supplied callback/return URLs. Integration tests must inspect raw JSON/HTML/RSC and storage URLs, not merely rendered UI.

## 7. Browser and installed-PWA contract

Desktop, Android browser/PWA and iPhone Safari/PWA call the same API using the authenticated session available in that context. They share server-side jobs, balance and entitlements, not necessarily browser cookies/local storage. No device-local balance or installed-mode authorization exception exists.

All private reads and mutations remain network-only. Do not defer POST requests into a service-worker queue; after an uncertain unlock, check the actor's preview/entitlement or operation receipt, and after checkout inspect the owned pending top-up reference and status. Reauthenticate in a new return context if necessary; never expose a receipt because a URL contains its ID. A fresh device retrieves server history instead of buying access again. Future native bearer authentication/push/store receipt endpoints are not added in this phase; their separate design must reuse the same domain authorization and business-effect uniqueness rules.
