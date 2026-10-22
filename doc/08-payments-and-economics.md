# Free/subscription services and platform economics
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** tier direction confirmed; price/collection/refund terms unresolved

## 1. Confirmed service model
| Participant/plan | Information service |
|---|---|
| Client | Continue free maintenance-task publication and own private comparison |
| Free company | At most five information items viewed per month |
| Active subscriber | No numerical monthly information limit; matching, privacy, expiry and abuse controls still apply |

The platform is semi-public-benefit and low-fee, primarily serving ZZP/SMEs. Only the free plan and a low-price subscription remain; no separate offer-publication fee applies. No subscription amount, billing period, tax-inclusive price or automatic-renewal policy has been selected. There is no default paid plan or automatic charge after the fifth free item.

## 2. Former €1 fee canceled
On 22 October 2026 the founder explicitly canceled the former €1 first private offer-publication fee and confirmed only free and low-price subscription services. Authorized offer submission, publication and editing have no separate per-offer charge or checkout. The new tier model also supersedes unlimited-free reading and the subscription deferral; it does not relax access, privacy or offer-lifecycle rules.

The [archived publication/refund rules](archive/v0.3/doc/08-payments-and-economics.md) are historical, not subscription terms. No assumed new price, per-listing charge, wallet, commission, prepaid bundle or hidden offer fee is authorized. The old seven-day incident-refund target does not silently become a subscription refund policy. The earlier one-business-day submission target can inform operations, but its new trigger/eligibility scope must be specified.

## 3. Subscription decisions before activation
Choose and publish price/tax basis, period, manual renewal versus explicit automatic renewal, period start/end, cancellation timing, failed-payment/grace behavior, partial periods, upgrades/downgrades, refunds/reversals and their effect on allowance. Separate optional alert consent from payment consent. Explain no guarantee of work, collaboration or sale.

Mollie remains the sole PSP. The [recurring-payment guide](https://docs.mollie.com/docs/recurring-payments) distinguishes first-payment mandate setup from later recurring collection. iDEAL first payment may establish directdebit, which requires SEPA Direct Debit activation; this is not approval to add that method or automatically renew. Card recurring paths also need merchant/method/consent qualification. Retain test/live isolation and requalify phone/PWA return, renewal/cancellation, refund and reconciliation for the actual selected policy rather than reusing €1 test evidence.

Provider-created renewal payment IDs may be new to the application when a classic callback arrives. Intake/association must verify the resource belongs to a known local subscription/customer/merchant/environment and expected billing period. See [technical design](05-technical-design.md).

## 4. Financial correctness
Keep local operation IDs, immutable request hashes, non-secret credential-version context, provider payment/subscription/refund references and unique period/effect identities. Apply current authenticated payment observations; never grant unlimited access from a redirect, request body, pending charge or unverified webhook.

Mollie's one-hour credential-scoped idempotency window does not replace durable local uniqueness or permit a new create after timeout/rotation. Reconcile an unknown outcome before another charge/refund. Refund/reversal accounting never exceeds captured unreturned principal; provider acceptance, provider completion and bank receipt remain distinct. Do not collect construction or trading principal merely because the platform contains cooperation/trading boards.

## 5. Economics and funding
Price remains open. Evaluate subscription revenue excluding applicable VAT, minus actual PSP/refund/reversal costs and variable mail/media/support costs, against fixed infrastructure and human operating work. Use conversion, retained paying companies and actual board utility; no old per-€1 contribution example proves subscription viability.

The confirmed infrastructure planning budget is €100/month excluding VAT, including selected services, backups/basic monitoring and necessary staging. Engineering, operations, legal/accounting, domains, payment fees/reserve, ads and future AI are separate. Semi-public-benefit positioning still needs a funded cost/complaint/refund/deletion operating model. Exact tax, invoice/correction and retention classification needs professional review before charging.
