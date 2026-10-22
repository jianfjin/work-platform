# Data model and retention design
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** logical schema; no migrations or database deployed

Use Drizzle/pg with reviewed SQL, UUID/opaque identifiers, UTC instants and integer monetary minor units. Plan periods, free allowance periods and listing lifetimes are separate. The old response-paid-publication schema is [archived](archive/v0.3/doc/06-data-model.md).

## Entities
| Entity | Scope and key constraints |
|---|---|
| principals/accounts/sessions | Company users and scoped staff; verified identity, status, MFA and revocation; no automatic guest ownership |
| companies/company_members | ZZP/SME participant and authorized members; plan/allowance scope is proposed at company level, not per login |
| guest_task_owners/tokens | One client task per authority scope; one-use hashed credentials, purpose, expiry and revocation |
| information_items | Board discriminator, opaque ID, publisher type/ID, safe preview, state, first publication, explicit deadline and policy version |
| tasks/task_revisions | Client brief/category, scope revision, region proof, closure and immutable three-month deadline |
| task_private/contact_grants | Restricted postcode/coordinates/selected client fields, consent version and allowed audience; no generic JSON export |
| task_offers/offer_versions | Unique task/company response, own text/contacts and scope revision; private owner view, final withdrawal marker; no offer-publication payment or payment-fulfillment FK |
| cooperation_listings/work_packages | Company-owned scope/skills/capacity/timing/area, independent visibility, optional authorized source reference; no inherited client-data grants |
| cooperation_interest | Company/package interest and minimal private communication/status; uniqueness and audience constraints |
| trade_listings/trade_inquiries | Tool/material kind/specification/condition/quantity/unit/price/area/availability, publisher and scoped inquiry; no platform order/payment principal by assumption |
| media_assets | Typed information parent, owner, bounded upload reservation, source/variant keys, real bytes/type/pixels, processing state and erasure progress |
| plan_definitions | Versioned tier, price/currency/tax/period/terms and numeric allowance semantics; live paid plan disabled until terms selected |
| memberships/entitlement_periods | Company, current status, coverage bounds, policy/terms version, provider references and entitlement version |
| allowance_periods/access_grants | Company/period counter and unique company/item/period grant for the proposed rule; operation key/payload binding, reasoned corrections, minimal retention |
| billing_operations/payments | Stable local key/hash/time/credential version, expected company/plan/period/amount/currency, merchant/environment and distinct provider payment/subscription/customer IDs |
| billing_effects/refunds/journal/documents | Unique provider/period/business effects, captured/returned/reversed amounts, immutable corrections and purpose-minimal billing evidence |
| alert_preferences/deliveries | Board/trade/item-category/radius/locale/frequency and explicit opt-in; stable logical delivery identity independent of mode; safe preview delivery does not imply detail entitlement |
| inbox/outbox/jobs | Minimal IDs and work kind, due time, claim/lease generation, attempt/backoff and state; preserve updates while leased; no ordinary plaintext access-token archive |
| reports/holds/rights_requests | Board/record/case scope, minimum evidence, staff reason, review/deletion date and request-response deadlines |
| audit/deletion_tombstones | Minimal actor/reason/opaque target/action/time and progress; recoverable independently of restored content snapshots |

## Atomic constraints
Unique access/grant identity and a conditional counter update share one transaction; a naive count-then-insert permits overrun. Resolve current ownership/visibility/expiry before allowance changes. Serialize entitlement-version changes with free access decisions; exact upgrade/downgrade/month reset behavior remains a policy input.

Use one response per task/company and enforce final withdrawal without deleting/recreating the response to restore it. Separate work-package/source relations from authorization; foreign-key presence grants no parent access. Reject mismatched board/detail types and owners.

Unique provider resource identities are merchant/environment-scoped. Billing-period activation/journal effects must not duplicate across callbacks/reconciliation; the exact period identity must follow approved subscription terms, not an assumed calendar month. Refund reservations and reversals share principal controls. Keep the financial ledger independent of content erasure.

## Retention
Client-task deadlines and <=24h live/<=30-day backup erasure remain confirmed. New boards, access history, membership events, invoices, suppression and evidence require purpose/field/schedule review. No quota or financial row may reconstruct an expired client task or confidential cooperation project.

Optional parent references must be nullable/opaque or otherwise deletion-safe without extending parent retention. Sold/closed trade listings and removed work packages need explicit visibility/erasure semantics before live use. Restore applies independent tombstones, expiry and membership/billing reconciliation before enabling any board.
