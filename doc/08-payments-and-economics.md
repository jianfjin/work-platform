# Fees, payment ownership and sustainability
**Version:** 0.3 · **Updated:** 21 October 2026 · **Status:** proposed company-fee model; accountant/merchant approval required

## 1. Resolve the conflicting draft
**Active requirement: client platform fee = €0; companies pay the platform.** The supplied prose also asks whether a client-funded or company-funded €1 model is sustainable. Compare them here, but do not turn that question into a client checkout feature.

Recommended pilot event: company pays **€1.00 gross including VAT per task for its first successfully published rough offer**. Task reading is free. Same company/task edits and ordinary contact have no further fee. This is a flat response-publication service charge, not a success percentage on final work. The event and gross amount require founder approval; no app or production charges exist.

## 2. Model comparison
| Model | Advantages | Cost/risk | Current decision |
|---|---|---|---|
| Client €1 posting, companies free | Simple single charge/job, may deter some casual spam | Adds demand friction; one €1 revenue per job regardless of company demand; does not ensure genuine jobs | Comparison only; **not enabled**, conflicts with payer rule |
| Company €1 first published response, client free | Low-friction demand; companies read fit before deciding; revenue follows professional interest | Firms may not win; PSP cost large relative to fee; no fixed competitor limit, so expectations must be explicit | **Recommended**, subject to event/economics approval |
| Company success commission | Fee follows reported outcome | Off-platform work/payment is not reliably observable without monitoring, disputes and collection complexity | Not MVP |
| Company subscription/optional bundles | Better fee amortization/recurring workflow value | Upfront commitment, prepaid/refund or recurring obligations; no demonstrated willingness yet | Future separate design, not compulsory €10 wallet |
| Both sides free | Useful bounded market-learning subsidy | No service revenue; ongoing costs still exist | Only an explicitly budgeted trial, not default long-term economics |

Company funding is more aligned with the current requirement and demand acquisition, not automatically profitable. Do not increase spam/participant volume or hide competition solely to make revenue look better.

## 3. Whose payment account?
Use **a PSP merchant account owned by the actual platform legal operator**, settling to its approved dedicated business bank account. It is not an assistant-owned account and not an informal shared/personal account. If an existing founder business will operate the platform, verify legal ownership, bookkeeping separation, merchant approval and account permissions; otherwise establish the operator first.

Required owner decisions: legal entity/trading name, authorized directors/admins, business IBAN, PSP onboarding/merchant checks, VAT/tax status, refund/dispute authority, finance contact and reserve. These concern the platform merchant, not routine workmanship certification of listed firms. No account has been opened or connected in this task.

Hosted checkout via a provider such as Mollie; initially use merchant-approved local payment methods and show actual available methods. Do not collect card/bank credentials ourselves. Construction payments never pass through this merchant account.

## 4. Charge and refund rules (proposed R1)
- Drafting/reading is free. Show €1 gross, no guaranteed job/reply, all interested companies may respond and contact visibility conditions before checkout.
- Only server-verified correct paid payment plus current eligible scope publishes first response.
- Failed/cancelled payment: no visible response or fee service fulfillment.
- Paid after close/expiry/safety hold/material edit, system unable to publish, or accidental extra capture: no second listing; full €1 cash-refund obligation tracked once to original method.
- Successfully published offer later loses/receives no reply/client closes/company withdraws: no automatic refund. This service is publication, not a verified connection or completed sale.
- Substantiated fake/duplicate/misrepresented task: proposed seven-day reporting window from payment, with later genuine evidence reviewed; approved incident refunds use recorded reason. No general 72-hour no-response restoration, because calls/messages happen off-platform.
- Edits and return visits never create another fee. A failed refunded attempt can be retried only deliberately after resolution; never retain two successful fees for one company/task.
- Refunds/chargebacks are verified, capped to unreturned captured funds and reconciled. Ambiguous timeout stays pending, not blindly resent.
- Target initiating confirmed platform-failure refund within one business day of verification; provider bank-arrival time varies and is not promised. Maintain a refund reserve.

The client is never asked to fund refunds or pay a platform commission. Valid construction contracts/prices remain between parties; the platform does not arbitrate workmanship or collect final invoices.

## 5. Direct payment now, bundles later
Propose direct €1 checkout for simplest initial UX/accounting. This replaces the old compulsory €10 access balance, not a new confirmed user preference. An optional company bundle could reduce PSP fees later, but requires explicit prepaid-service classification, source/refund accounting and onboarding validation before implementation.

PWA browser distribution uses web hosted checkout; same server records on all devices. Bank-app return to another context requires ownership/session recovery, not another automatic purchase. Native apps later need a new storefront/payment/receipt/fee assessment; physical carpentry does not by itself settle rules for paid digital publication.

## 6. Costs and arithmetic
[Mollie's Dutch pricing](https://www.mollie.com/nl/pricing), checked 21 October 2026, publicly lists €0.32 for iDEAL/Wero payments. This is indicative, not a merchant contract; other methods/refund/dispute costs differ. At an **illustrative** 21% VAT on this service, €1 gross yields 1/1.21 = €0.8264 before costs, or €0.5064 after a €0.32 payment fee. Accountant confirms actual tax classification/tax point/rounding.

Illustrative cohort model, not observed forecasts:
- s = original paid response attempts per job; cases shown at 1, 3, 5, not a required/maximum number.
- 5% cash refunds, assumed lost gross service revenue; PSP fees conservatively not returned.
- Variable email/support €0.10 and hosting/media/geocoding allocation €0.03 per paid attempt.
- Basic platform operations allocation €0.20/job; not manual certification/monitoring.
- Combined client/company acquisition cost and fixed overhead excluded from first contribution column.

```text
Net service revenue/job = s * 0.95 / 1.21
Contribution before CAC/fixed overhead =
    s * (0.95/1.21 - 0.32 - 0.10 - 0.03) - 0.20
```

| Paid attempts/job | Net revenue after assumed refunds | Contribution before CAC/fixed | With €2 combined CAC/job |
|---|---:|---:|---:|
| 1 | €0.785 | €0.135 | -€1.865 |
| 3 | €2.355 | €0.805 | -€1.195 |
| 5 | €3.926 | €1.476 | -€0.524 |

A client-funded single €1 posting would produce roughly the one-attempt contribution under the same simplified inputs, even if many companies respond. Company funding has more revenue potential but a fragile margin and no guarantee of sufficient responses.

At three attempts the acquisition ceiling is only about €0.805/job before fixed costs. With assumed CPC €0.50 and click-to-eligible-post conversion 10%, demand CAC alone is €5, well above this ceiling. These CPC/conversion numbers are examples, not Google quotes or measured local demand. Do not spend based on them as facts.

A later fully used ten-response bundle could amortize a single €0.32 payment to €0.032/response, but that is not the current direct-checkout design or permission to implement stored funds. Track actual method fees, refunds, support time, genuine responses and both sides' acquisition cost.

## 7. Accounting and retained evidence
Before fulfillment, captured funds are a refundable service obligation; fulfillment records fee revenue under the accountant-approved policy. Refunds append correction/credit-note records, never edit history. No withdrawal balance, contractor payout or job-value commission ledger.

Separate minimum statutory platform-fee records from task data. [Belastingdienst](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/) explains that basic business records generally have a seven-year retention period, with other categories differing. Accountant must classify the actual records and start dates. This does not justify retaining every client photo, postcode or offer for years.

Daily reconciliation: PSP paid/refunded/disputed amounts versus attempts, one response fulfillment, journal and billing documents. Flag paid-but-unpublished unresolved attempts. Preserve enough finance-safe identifiers after task deletion to resolve refunds without resurrecting client content.

## 8. Sustainability gate
Free client posting remains the rule even if margins disappoint. Before scale, approve actual positive contribution after variable operations/acquisition plus a credible fixed-cost plan. If it fails, improve fit/organic acquisition and reconsider prospective company pricing or optional tools with explicit approval; do not silently charge clients, guarantee leads, introduce paid ranking or infer final-work success fees.
