# API contract proposal
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** proposed contract; no running API

## Common rules
Prefix /api/v1. Server-side sessions, ownership, board/type, current visibility/expiry, plan/allowance and selected-field consent are authoritative. No caller-supplied subscription flag, balance, fee or consumed-count claim. Use explicit request keys for replay-sensitive mutations and safe allowlisted DTOs. Return generic unauthorized/not-found outcomes without rival or owner enumeration.

Quota counting and subscription terms are pending; live endpoints depend on configured approved policy. The founder canceled the €1 offer-publication fee; version 0.3 publication-payment endpoints are historical and must not be exposed in v0.4. Paid checkout is solely for the selected low-price subscription.

## Endpoints
| Endpoint | Actor | Contract |
|---|---|---|
| POST /tasks/drafts | Narrow draft authority | Create private guest draft; no implicit company feed publication |
| POST /tasks/{id}/confirm-email | Task-scoped one-use credential | Confirm ownership and publish only eligible validated task |
| POST /tasks/{id}/recover-access | Generic bounded input | Fresh task-scoped link without account enumeration |
| GET/PATCH/DELETE /tasks/{id}/manage | Task owner | Own content/lifecycle/contact controls; immutable expiry and erasure |
| GET /information?board=... | Allowed requester | Typed minimal previews and board/category/area filters; no quota mutation or sensitive details |
| POST /information/{id}/access | Active company | Explicit, idempotent detail-access request; atomically evaluate approved plan/allowance and current visibility |
| GET /information/{id}/details | Currently entitled actor | Safe current detail; no anonymous/prefetch grant or stale cached entitlement |
| GET /me/plan /me/allowance | Own company | Current tier, coverage and published allowance policy/usage; no editing counters via request |
| GET /plans | Any allowed viewer | Approved plan catalog only; no invented live price or default renewal consent |
| POST /me/billing/checkout | Reauthenticated company | Deliberate purchase for versioned selected price/terms; local operation before provider call; recover unresolved attempts |
| GET /me/billing/payments/{id} | Paying company | Verified server-owned payment/entitlement status and permitted receipt fields |
| POST /me/membership/cancel | Authorized company | Apply chosen cancellation/renewal policy; replay-safe, with no assumed immediate refund |
| POST /tasks/{id}/offers | Eligible company | Own private draft/publication under current access and task-lifecycle rules; no per-offer charge, checkout dependency or automatic client contact |
| GET /tasks/{id}/offers | Task owner | All own-task eligible private offers; no company/rival access |
| PATCH /offers/{id} / POST /offers/{id}/withdraw | Own company | Eligible edits and final withdrawal; owner management not treated as a new information item |
| GET /tasks/{id}/client-contact | Eligible responder | Current entitlement/access plus task state and explicit selected-field callback permission |
| POST /cooperation-listings | Company | Publish own validated package/partner need under configured geography/lifecycle; no source-project data inheritance |
| PATCH/DELETE /cooperation-listings/{id} | Publishing company | Manage only own item; dependent interest/media/deletion handled explicitly |
| POST /cooperation-listings/{id}/interest | Eligible company | Private interest for currently accessible listing; no automatic contract |
| POST /trade-listings | Company | Typed tool/material listing; configured audience/lifecycle and safe price/condition/area |
| PATCH/DELETE /trade-listings/{id} | Publishing company | Own edit/withdraw/availability state; not evidence of settled platform funds |
| POST /trade-listings/{id}/inquiries | Eligible company | Private counterpart contact/inquiry; no exact-address disclosure by default |
| POST /information/{id}/photos / POST /photos/{id}/complete | Scoped publisher | Bounded private quarantine and queued conversion; client cannot mark a file safe |
| GET /photos/{id}/download | Current authorized reader | Sanitized bytes through Worker; same board/entitlement/consent/deadline rules as detail; no public original URL |
| POST /reports /me/privacy-requests | Scoped reporter/verified requester | Rights/report functions available independently of paid tier or exhausted information allowance |
| POST /webhooks/mollie | Bounded classic payment-ID hint | Durable accepted work, authenticated association/verification, no body-paid trust; accommodate validated unseen renewal IDs if recurring is enabled |
| POST /webhooks/resend | Raw body with verified signature/Svix headers | Minimal durable event inbox, dedup and platform-delivery association |

Both provider callback routes are narrowly documented CSRF/session exceptions with their own intake controls. Do not disable CSRF/origin protection globally or assume the Mollie classic body is signed like Resend events.

## Detail grant and media
Prefer an explicit POST so GET previews/prefetch cannot spend free allowance. Binding an operation to company/item/policy prevents replay with a different payload. Repeated successful access follows the approved counting period; the same operation cannot increment twice. HTTP retry is not proof of a new human view.

Forbidden/expired/removed records expose no protected fields and consume no quota. A prior access grant cannot survive revoked ownership/consent or revive deleted content. Owner management, own offers/billing and rights requests are separately scoped; free allowance exhaustion cannot disable these functions.

## Error and device behavior
Use bounded localized validation errors, quota-exhausted/upgrade guidance, current plan status and explicit payment-pending/processing states. No offline queue, automatic purchase retry, raw-contact response or company-visible rival count. A PWA/bank return on another browser recovers the existing owned operation after appropriate login; callbacks reconcile independently.
