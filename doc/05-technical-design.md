# Technical design
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** proposed algorithms for revised business scope; no application delivered

## 1. Selected runtime and invariants
Use TypeScript/Hono and React Router/React/Vite/SSR on Workers, Drizzle with node-postgres (pg), and Render PostgreSQL Frankfurt through Hyperdrive with application query caching disabled. Keep transaction-scoped connections and explicit SQL locks/constraints; no assumptions about session state surviving the pool. Use Render's external TLS endpoint and reviewed source-IP restrictions. Migrations are reviewed SQL applied separately from requests/jobs. The [stack comparison](18-cloudflare-vs-render.md) remains useful runtime research, not the current commercial contract.

The new business invariants are ownership/consent isolation, correct active plan entitlement, no free-plan allowance overrun, unique billing-period effects, bounded principal return and deletion-safe recovery. An unlimited subscription removes only the numerical monthly information cap. Only free and low-price subscription services remain: the founder canceled the legacy offer fee, so offer publication has no separate payment dependency. The exact five-item policy remains to be decided.

## 2. Identity and authorization
Preserve Better Auth company verified-email/password and staff invite-only password plus mandatory TOTP/one-use recovery codes. Password reset preserves MFA; role grants require fresh assurance and may not elevate an existing password-only session. Qualify adapter atomicity, immediate session revocation and concurrent recovery-code reuse.

Guest task creation, confirmation and recovery use short-lived, one-use hashed task-scoped credentials. Initial unpublished drafts require narrowly scoped draft authority; do not mistake a public task ID for ownership. Guest ownership remains separate from company/staff accounts and is not inferred by email matching.

Every read first checks actor, board, ownership or allowed audience, current record state/deadline and safety holds. Then evaluate plan/allowance as appropriate. Contact access additionally requires current selected-field consent and eligible task participation; no plan grants full postcode or rival offers.

## 3. Proposed allowance algorithm
The following assumes the recommended company-wide, shared-board, unique-item-per-month policy. It must not become live terms until counting, period/timezone, repeat/cross-month access, mail and plan-transition rules are confirmed.

1. Public/allowed preview GET returns only the safe preview. Prefetch and HEAD do not authorize detail disclosure or mutate usage.
2. An explicit authenticated POST requests detail access with an operation key. Validate the target's allowed board, visibility/deadline and the requester's authority before allocating quota.
3. In one PostgreSQL transaction, lock the company entitlement/period state in a consistent order. Re-read current membership and policy version. Own-content/own-reply/billing/rights management follows separately authorized paths without consuming the proposed information allowance.
4. If an applicable access grant already exists, return its current safe result without incrementing again. For a free company, atomically allocate only when usage is below five; insert a unique company/item/period grant and increment the period counter together. A subscriber uses the active entitlement without a numerical cap, not a fabricated huge counter limit.
5. Commit the grant and return the allowlisted projection. Lost responses replay the same operation/grant; do not claim to prove a human read the response. Failed/forbidden/expired lookups do not consume quota. A supported quota correction is reasoned and idempotent.
6. Subsequent detail/media/contact reads validate current authorization, grant/entitlement and expiry. A grant is not a permanent bearer URL. Deletion, withdrawal or consent change cannot be bypassed by an old grant/cache.

Use unique constraints and conditional updates, not a client-maintained counter or a scheduled destructive monthly reset. Store explicit period keys/bounds and policy versions. Two tabs requesting the fifth/sixth new item must not both succeed. Test mixed-board requests, retry, plan changes and month boundaries. Historical paid reads versus free usage on downgrade is an unresolved policy input; do not silently reset allowance or preserve unlimited access.

## 4. Three board services
Tasks retain private offers, material revisions and clear old-scope labels; expiry never renews on edits. Close stops new offers/alerts/callbacks; reopening before original expiry resets consent. Final withdrawal cannot be undone by task reopening or a stale event. Authorized offer submission, publication and editing carry no separate charge; the legacy payment-publication dependency is removed from v0.4.

Cooperation listings/work packages are independent company-owned information items. Validate a parent's visibility and the publisher's right to reference it, but copy no client contact/address/photos or rival offers automatically. Access to a package does not grant parent access. Interested-company records are private to the appropriate parties, not a public bidder list or automatic subcontract award.

Trade listings use explicit item, condition, quantity/unit, price/tax basis, area and availability fields. The current proposal is inquiries and direct agreement between companies; orders, delivery/payment orchestration and escrow need separate requirements. Until new-board geography/retention is configured, live publication stays disabled.

## 5. Membership and Mollie
Subscription price, period, renewal/consent and cancellation/refund/grace policy must be fixed before real checkout. Proposed application states include FREE, PAYMENT_PENDING, ACTIVE, CANCEL_AT_PERIOD_END, PAST_DUE and EXPIRED; their exact entitlement effects are policy inputs. Membership-period start/end and free calendar-month bounds are separate concepts.

Persist an operation and expected company/plan/amount/currency/terms/environment before creating a payment or subscription. Use stable keys and request hashes. Authenticate provider reads and verify the actual merchant/profile, mode, customer/subscription association, expected charge and period before applying a unique entitlement/journal effect. Pending payment, provider subscription creation, callback body or return URL is not settled payment proof. Do not tie new membership activation to publishing a particular offer.

For [classic Mollie callbacks](https://docs.mollie.com/reference/webhooks), accept bounded form payment-ID hints and durably schedule verification before acknowledging accepted work; persist failure returns 503 and malformed input 400. Known local payments can be queued directly. If automatic subscriptions are selected, their provider-generated payment IDs may first appear at callback: bounded admission/retrieval under the configured merchant credentials must associate them to an already known local subscription/customer and expected period before creating local billing effects. Reject unrelated resources without disclosing local existence. Do not permit arbitrary URLs, unlimited provider lookups or the old permanent-ignore-unknown-ID rule. Scheduled reconciliation of known subscriptions/payments recovers missed intake and state changes; exact admission/rate and recurring-event tests are qualification work.

A later callback on the same payment can carry refund/reversal changes; do not permanently deduplicate payment IDs. Coalesced work preserves updates arriving during a lease. Local payment, subscription, refund and period-effect references remain distinct and environment-scoped.

Mollie's [idempotency cache](https://docs.mollie.com/reference/api-idempotency) lasts one hour and is credential-scoped. Keep local operation history beyond it; inspect/list/reconcile ambiguous charge/subscription/refund creation before retry after timeout/cache expiry/rotation. Duplicate subscriptions can cause repeated future charges, so unresolved creation cannot be replaced casually. Refund plus reversal cannot exceed captured unreturned principal; queued/accepted/processing is not completed. New refund eligibility and subscription cancellation rules remain to be decided, not inherited from €1 task-publication cases.

The [recurring-payment/mandate guide](https://docs.mollie.com/docs/recurring-payments) and [subscription API](https://docs.mollie.com/reference/create-subscription) need qualification for the chosen policy. The previous iDEAL/Wero/card initial-checkout selection does not silently approve a new recurring SEPA method or auto-renew consent.

## 6. Media and matching
Use private EU R2 and the selected Images Workers binding. Client-task limits remain six photos, 10 MB each, JPEG/PNG/WebP/HEIC input to sanitized JPEG/WebP. Propose the same limits for the new boards. Enforce actual bytes and atomic in-flight slots, validate real type/pixels, apply orientation/color, remove identifying metadata and reject unsupported/corrupt input clearly. Streams and bounded concurrency avoid buffering whole batches.

Quarantined originals are never ordinary downloads. Persist validated variants and conditionally mark ready only while the parent listing remains eligible; late/orphan output after deletion must be cleaned. Reads through authorized Workers check board entitlement/consent/deadline; no public R2/original fallback or unreviewed shared cache. Processor temporary-copy/log/retention locations remain NFR-09 work.

PDOK receives minimum postcode/city queries, not project text or contacts. Qualify coarse coordinates/CRS and Groningen/Fryslân task mapping. A missing/ambiguous location stays a draft, not zero distance. New-board geography must be configured explicitly.

## 7. Notifications and durable jobs
Keep PostgreSQL inbox/outbox/jobs with bounded Cron, initially one-minute recovery polling and optional post-commit immediate attempts claiming the same work. Use expiring leases, stale-owner fencing, bounded concurrency/backoff, fair priority and visible failed-work exceptions. Never call external providers while holding domain locks. `waitUntil` is not durable storage; Queues remains unselected.

Resend sends through a dedicated subdomain and app-specific secret. Record a stable logical delivery identity independent of digest/instant frequency. Recheck opt-in, current plan, listing visibility/expiry, match and suppression before send. Default to safe previews/links so email does not leak full details or spend free quota unexpectedly; exact receipt/view counting still needs policy.

Preserve Resend 24-hour idempotency-window handling, durable local history, verified raw-body/Svix delivery-event inbox and callback-before-association recovery. Access-link delivery must not create an ordinary plaintext-token archive. Do not confuse paid membership with marketing consent or provider acceptance with human receipt.

## 8. Erasure and recovery
Client tasks retain the confirmed three-calendar-month clock from first publication, immediate denial, <=24h live purge and <=30-day isolated backup roll-off. Purge all task revisions, photos, grants, location and credentials; minimal financial records and named-case evidence stay purpose-separated under reviewed schedules.

New-board listing lifetimes, sold/closed states, parent/package deletion and usage/billing/audit retention require explicit schedules before live data. Billing and access grants cannot archive task/project content. Deletion tombstones remain available independently of restored snapshots; reapply current expiry/deletions, entitlement policy and provider reconciliation before users regain access. Keep no private PWA cache or offline write replay.
