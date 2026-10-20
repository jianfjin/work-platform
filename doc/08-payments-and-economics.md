# Payments, refunds and unit economics

**Status:** proposed paid-beta policy; requires founder, accountant/legal and PSP sign-off before collecting money.

## 1. What exactly costs €1

- One approved business explicitly unlocks one OPEN job thread for **EUR 1.00 gross including VAT** under pilot price P1.
- This permits the full work brief, approved attachments, shared scope clarification and a private client conversation. It does not expose competitor offers or automatically disclose client contact details.
- Only the first successful unlock is charged. Repeat reads, replies, quote versions, award and completion have no additional fee. There is no construction success commission or compulsory subscription.
- Three lifetime paying businesses maximum. Never resell a released/restored slot. No guaranteed project, earnings, reply or refund solely for losing a fair selection.
- The purchase snapshots the price and policy versions. Any future pricing experiment is prospective and requires fresh approval; no hidden dynamic fees.

## 2. Funding model and alternatives

**Recommended paid-beta mechanism:** one €10.00 gross prepaid access top-up, then €1.00 deducted per thread. No automatic refill, no credit transfer, no expiry during the pilot, no third-party redemption and no construction payments. “Access balance” is a product label, not a conclusion about legal classification.

The carpenter sees the €10 commitment before checkout. This may reduce conversion despite a low per-thread price. Validate willingness to prepay. A top-up never reserves a particular job and never automatically unlocks it when the buyer returns from the PSP. If the job fills, the buyer keeps unused funds and can use the unused-funds refund route.

Alternative A: direct €1 checkout per thread is simpler to explain and avoids a prepaid balance but has disproportionate PSP cost and a harder capacity/payment race. Alternative B: monthly postpaid billing reduces checkout friction but adds credit risk and collections. Do not silently switch models: revise the PRD/transaction design before implementing either alternative.

## 3. Payment boundary and fulfillment

HoutMatch sells its own access service; the client pays the carpenter separately for carpentry work. Use hosted checkout and never collect card/bank credentials in our application. Merchant onboarding and prepaid-service acceptance must be confirmed with the PSP.

A top-up grants balance only after authenticated server-side retrieval verifies the correct paid transaction, merchant/environment, amount, currency and intent. Browser redirects, screenshots, callback fields and “authorized” are not sufficient. [Mollie's official flow](https://docs.mollie.com/docs/accepting-payments) describes hosted checkout, callbacks and API verification; our idempotent grant/reservation design is specified in [Technical design](05-technical-design.md).

Payment pending or failed creates no spendable funds. Late verified success grants the lot once and updates the owner; a requested job may no longer have capacity. Provider refunds and chargebacks are tracked separately from original paid status.

## 4. Restoration policy (access-v1 proposal)

“Restoration” means reversing one €1 service spend into the original paid source lot. It is not an automatic cash transfer; restored eligible unused paid value can subsequently enter the cash-refund process. Each entitlement gets at most one restoration, and then becomes read-only; lifetime capacity is unchanged.

| Case | Rule | Result |
|---|---|---|
| Duplicate tap, network retry or second unlock key | There must never be a second successful charge | Return original entitlement/receipt, no second debit |
| Confirmed accidental duplicate debit | Finance incident, not discretionary customer goodwill | Compensating entry for duplicate; investigate invariant breach |
| Job invalid, duplicated, fabricated or materially misrepresented | Operator confirms; submission window 7 days from unlock, with later substantiated reports reviewed for exceptions | Restore affected entitlement(s) once; remove unsafe job, redact content as needed |
| Client cancels before this business's first private response | Compare locked cancellation timestamp and that participant's first response | Automatically restore that participant; others follow their own facts |
| Client does not reply | Business sent first substantive private response within 24h of unlock; no qualifying client private reply/interaction in that channel and no legitimate job award/completion at or before 72h after that response | Automatically restore; read-only afterward; timestamped eligibility, not email-delivery proof |
| Service unavailable after successful unlock | Verified incident prevented full-brief access for >=24 consecutive hours during the eligible OPEN/PAUSED opportunity and business did not access the brief successfully | Restore through incident-linked operator batch |
| Client selects someone else after a genuine opportunity | Access service delivered | No automatic restoration |
| Carpenter changes mind, withdraws, never responds or sends first response after 24h | Access delivered; no-response safeguard not earned | No automatic restoration |
| Client replies but job is not won | Normal commercial risk | No automatic restoration unless independent invalidity evidence applies |
| Job expires after valid access | Expiry is visible before purchase | No automatic restoration; an already-earned no-response rule still evaluates unless an award/completion outcome occurred before its deadline |
| Payment reversal / fraud suspension | Separate financial dispute, not a free second service | Freeze/reconcile related lots; revoke unsafe access; prevent restoration into spendable disputed value |

“Substantive” first response is text with an actual introduction/question/proposal, not an empty placeholder, emoji or automated receipt; flag suspected gaming for review. Shared-board posts do not count as a private business/client response. A client visit/read receipt is not a reply. Shared Q&A alone does not defeat the private-reply safeguard. A genuine client private reply, visit arrangement recorded in that channel, or selection outcome counts; attach evidence for an operator correction, not guessed intent.

Batch policy evaluation checks job/entitlement facts under the same locks as cancellation and award. Replies/outcomes at or before the deadline defeat eligibility; later events cannot erase an already-earned restoration, even if the worker ran late. Private write/award actions evaluate due policies under that lock before acting. Once restoration is granted, a later reply cannot silently re-debit it. Restored firms cannot repurchase or continue writing; they retain permitted history until retention deletion. A legitimate still-open job can therefore have fewer than three active responders and no refill: an explicit pilot trade-off.

Request intake and restoration decisions target two business days during staffed hours. Appeals target five business days. These are proposed operating targets, not published guarantees. No-response restorations are automatic once objectively eligible, with anomalies routed to support and audit.

## 5. Unused-funds cash refund and account closure

Proposed policy: on verified request/closure, refund eligible unspent paid principal to its original payment method, without a platform processing deduction; do not promise a bank-arrival time before PSP confirmation. No promotional funds exist in MVP. A refundable ceiling for one payment is its captured gross amount minus net consumed service value, prior confirmed cash refunds and unrecovered reversals. Source-lot provenance prevents returning more than was paid.

Reserve refundable value transactionally before contacting the PSP, so it cannot be spent concurrently. Never let an ambiguous timeout release money that may already have been refunded. Multi-payment balance refunds are separate tracked source refunds. If the original method is unavailable, finance follows a PSP-approved verified alternative; do not collect arbitrary bank changes through chat.

Pending disputes freeze only affected value where feasible; already-consumed disputed funds produce separately recorded exposure/debt. Never make the wallet negative or silently offset unrelated new funds. Account closure resolves balances, pending refunds and disputes before deletion of non-required identity data. See [Data model](06-data-model.md) and [Security](09-security-privacy-and-trust.md).

## 6. Ledger and accounting boundaries

Maintain a balanced append-only **gross service subledger**, with per-payment funding lots. Illustrative entries:

| Event | Debit | Credit |
|---|---|---|
| Verified €10 top-up | PSP clearing 1,000 cents | User prepaid-service liability 1,000 cents |
| €1 unlock | User prepaid-service liability 100 cents | Access service consumed 100 cents |
| Restore one unlock | Access service consumed 100 cents | Original prepaid liability 100 cents |
| Cash refund unused €3 | Original prepaid liability 300 cents | PSP clearing/refund payable 300 cents |

These gross postings track access economics and refund provenance; they are **not** a finalized statutory general ledger or a determination of VAT recognition. Accountant must decide voucher/prepayment classification, tax point, invoice/credit-note timing, rounding and legal retention. Generate each billing document once and link corrections; never edit an issued invoice in place.

The Netherlands general VAT rate is 21% according to [Belastingdienst's tariff explanation](https://stichtingenvereniging.belastingdienst.nl/belastingplicht/omzetbelasting-btw/btw-ondernemer-wat-nu/). Its application to this specific access service is an assumption requiring accountant approval. Contractor work quotes have their own tax treatment; do not force the platform-access tax assumption onto construction work.

Reconcile daily: provider successful charges/refunds/disputes ↔ top-ups/source lots ↔ journal ↔ billing documents; available balances ↔ materialized wallet; entitlement count ↔ lifetime slot counter. Any mismatch blocks new affected purchases until repaired with auditable compensating entries.

## 7. Why €1 needs testing

[Mollie's Dutch pricing page](https://www.mollie.com/nl/pricing), observed 20 October 2026, advertises €0.32 per iDEAL/Wero payment. This is an indicative public rate, not a merchant quote; verify method availability, fees and refund/dispute costs at onboarding.

Assuming 21% VAT and that fee:

- €1 gross service revenue before costs = `1 / 1.21 = €0.8264` net.
- Direct €1 checkout leaves approximately `€0.8264 - €0.32 = €0.5064` before every other cost and restoration.
- A fully consumed €10 top-up spreads a €0.32 fee across ten original €1 uses: €0.032 per original use. It improves fees, not underlying demand acquisition economics.
- Maximum original revenue with three participating firms = €3.00 gross, approximately €2.4793 net before restorations/costs. Never increase the cap solely to rescue revenue.

## 8. Illustrative per-job model

All operating inputs below are assumptions, not observed results. Let `s` be original paid unlocks/job (1–3), `r=10%` restoration rate, `p=€1` gross price. Assume restored value remains unspent in the measurement cohort; it stays a liability, not revenue. Top-up fees are conservatively allocated to original purchases; later re-spends are tracked in later cohorts, not double-counted as fresh cash acquisition.

```text
Net consumed access revenue = s × p × (1-r) / 1.21
PSP allocation              = s × €0.032
Variable hosting/media      = s × €0.03
Support/restoration work    = s × €0.10
Moderation                  = €1.00/job (2 minutes at €30/hour)
Acquisition                 = combined client+carpenter CAC allocated per job
Contribution                = revenue - all above costs
```

| Original unlocks/job | Net revenue after 10% restoration | Contribution before CAC/fixed overhead | Contribution with €2 combined CAC/job |
|---|---:|---:|---:|
| 1 | €0.744 | -€0.418 | -€2.418 |
| 2 | €1.488 | €0.164 | -€1.836 |
| 3 | €2.231 | €0.745 | -€1.255 |

Even a fully subscribed job cannot tolerate €2 acquisition cost under these assumptions. At three unlocks the pre-fixed-overhead CAC ceiling is only about €0.745/job. Manual moderation alone can erase the margin. Top-up cash received but not consumed is not profit; avoid breakage/expiry assumptions.

These figures exclude engineering salaries, legal costs, fixed infrastructure and extra refund/chargeback fees. Do not present them as profit forecasts. Track actual support time, consumption, restored balances and both sides' acquisition costs weekly.

## 9. Pricing and scale decision gate

G3 requires a positive observed contribution after variable acquisition and operations, an acceptable liquidity/reply rate, understandable fee terms and a viable fixed-cost plan. Discovery and beta can run at an explicit approved subsidy; scaling cannot hide it.

If €1 fails: improve brief quality/organic acquisition/automation first; then test a higher flat prospective fee or an optional professional workflow subscription, with explicit consent. Preserve the three-firm cap and private-message boundary. Do not implement a pricing change until its own approval, updated economics and terms are complete. The architecture records price versions for safety, not permission to add dynamic pricing to the MVP.
