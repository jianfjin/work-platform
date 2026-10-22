# Fees, payment ownership and sustainability
**Version:** 0.3 · **Updated:** 22 October 2026 · **Status:** fee event/gross price, collection, Mollie/method scope and refund eligibility/reporting/submission target confirmed; live activation, operational readiness and accountant/merchant work remain open

## 1. Resolve the conflicting draft
**Active requirement: client platform fee = €0; companies pay the platform.** The supplied prose also asks whether a client-funded or company-funded €1 model is sustainable. Compare them here, but do not turn that question into a client checkout feature.

Confirmed pilot event: company pays **€1.00 gross including VAT per task for its first successfully published private rough offer**, using individual Mollie-hosted checkout. Reading and edits are free; a client reply is not guaranteed, and failed publication receives a full refund. The event, gross amount and collection model were confirmed on 21 October; Mollie was selected on 22 October 2026. See [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md) and [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md). Live merchant/method activation, operational refund details and tax classification remain separate launch work.

## 2. Model comparison
| Model | Advantages | Cost/risk | Current decision |
|---|---|---|---|
| Client €1 posting, companies free | Simple single charge/job, may deter some casual spam | Adds demand friction; one €1 revenue per job regardless of company demand; does not ensure genuine jobs | Comparison only; **not enabled**, conflicts with payer rule |
| Company €1 first published response, client free | Low-friction demand; companies read fit before deciding; revenue follows professional interest | Firms may not win; PSP cost large relative to fee; no fixed competitor limit, so expectations must be explicit | **Selected** event and gross price; economics still require validation |
| Company success commission | Fee follows reported outcome | Off-platform work/payment is not reliably observable without monitoring, disputes and collection complexity | Not MVP |
| Company subscription/optional bundles | Better fee amortization/recurring workflow value | Upfront commitment, prepaid/refund or recurring obligations; no demonstrated willingness yet | Confirmed deferred until after MVP; any later option needs separate design |
| Both sides free | Useful bounded market-learning subsidy | No service revenue; ongoing costs still exist | Only an explicitly budgeted trial, not default long-term economics |

Company funding is more aligned with the current requirement and demand acquisition, not automatically profitable. Do not increase spam/participant volume or hide competition solely to make revenue look better.

## 3. Whose payment account?
Use **a PSP merchant account owned by the actual platform legal operator**, settling to its approved dedicated business bank account. It is not an assistant-owned account and not an informal shared/personal account. If an existing founder business will operate the platform, verify legal ownership, bookkeeping separation, merchant approval and account permissions; otherwise establish the operator first.

Required owner decisions: legal entity/trading name, authorized directors/admins, business IBAN, PSP onboarding/merchant checks, VAT/tax status, refund/dispute authority, finance contact and reserve. These concern the platform merchant, not routine workmanship certification of listed firms. No account has been opened or connected in this task.

**Mollie-hosted checkout through the Payments API is the sole selected integration for this stage**, by the founder's explicit instruction on 22 October 2026. Provider selection does not activate a merchant contract. Use only merchant-approved, implemented payment methods and display actual availability. Do not collect card/bank credentials ourselves. Construction payments never pass through this merchant account.

### Mollie scope
Use a small server-only adapter for payment creation/retrieval, refund creation/retrieval and reconciliation. Desktop, Android and iPhone PWA redirect to the same provider-hosted checkout; no custom card form or dedicated mobile SDK is required. Use classic payment-ID notifications plus authenticated API retrieval, not a signed-event assumption. See the [payment flow](https://docs.mollie.com/docs/accepting-payments) and [technical mapping](05-technical-design.md).

Collect only the platform's own publication fee. No connected-merchant onboarding/routing, construction payouts, escrow, recurring charges, subscriptions or wallet is planned. Any tax configuration must preserve the approved €1 gross total and receive the existing accountant review. No payment account, credentials or package has been created by this documentation work.

Confirmed on 22 October 2026: target both iDEAL/Wero and cards, with all other payment methods outside MVP. Before enabling each method, obtain merchant approval and validate the actual Mollie profile, current method identifier, €1 minimum/maximum, fees, successful €1 payment, full refund and bank/card return on Android and iPhone, including installed PWA contexts. Restrict hosted checkout through an explicit server-maintained allowlist of these validated targets; do not inherit every method enabled in the merchant dashboard. See [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md). The selected provider has no current-stage alternative/fallback evaluation.

## 4. Charge and refund rules (R1; eligibility, reporting and submission target confirmed)

The confirmed core is €1 including VAT for first publication, free reading/edits, no guaranteed reply and a full refund if publication fails. Material task changes create a new scope revision: captured payments for unpublished old-scope offers refund, while already published offers remain visible with scope-change labels and free edits, without automatic refund or extra fee; see [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md). On 22 October 2026 the founder also confirmed full €1 refunds for substantiated fake, duplicate or materially misrepresented tasks, with a seven-day reporting target from payment and review of credible later evidence. The founder confirmed a one-business-day submission target after verification or staff approval; provider recovery algorithms and operational readiness still need implementation and validation.
- Drafting/reading is free. Show €1 gross, no guaranteed job/reply, all interested companies may respond and contact visibility conditions before checkout.
- Only server-verified correct paid payment plus current eligible scope publishes first response.
- Failed/cancelled payment: no visible response or fee service fulfillment.
- Paid after close/expiry/safety hold/material edit, system unable to publish, or accidental extra capture: no second listing; full €1 cash-refund obligation tracked once to original method.
- Successfully published offer later loses/receives no reply/client closes/company withdraws: no automatic refund. This service is publication, not a verified connection or completed sale.
- Confirmed: substantiated fake, duplicate or materially misrepresented task qualifies for a full €1 publication refund. Ask companies to report within seven days of payment; this is a reporting target, not an automatic rejection cutoff, and credible later evidence is still reviewed. An allegation alone does not approve a refund: authorized staff record the decision/reason and retain only necessary case evidence under the existing evidence-hold rules. No reply or losing the work alone does not qualify; there is no general 72-hour no-response restoration because calls/messages happen off-platform.
- An approved incident refund corrects the financial record without erasing publication history or restoring a withdrawn offer. A later legitimate scope change is governed by the separate revision policy, not automatically treated as original misrepresentation.
- Edits and return visits never create another fee. A failed refunded attempt can be retried only deliberately after resolution; never retain two successful fees for one company/task.
- Confirmed reopening preserves existing offers and paid-fee history without another fee, while old canceled attempts remain invalid: a late capture for such an unpublished attempt refunds even if the task is open again. See [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).
- Confirmed withdrawal of a published offer is final for that company/task in MVP: original fee retained without automatic refund, no restoration/republishing/new checkout. It does not erase financial facts or turn a fulfilled publication into an unpublished failed attempt. The same [lifecycle ADR](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md) records the client marker/contact consequences.
- Refunds/chargebacks are verified, capped to unreturned captured funds and reconciled. Ambiguous timeout stays pending, not blindly resent.
- Confirmed submission policy: automatically submit verified failed-publication/duplicate-capture refunds, and target submission to Mollie within one business day of automatic verification or staff incident-refund approval; escalate overdue, queued or failed obligations. Persist the original verification/approval time and track submission attempts, provider acceptance and provider completion separately; retries do not restart the target. This is a platform submission target, not a bank-arrival promise: Mollie tracks refund processing separately and funds may still be on their way at its `refunded` status. See [Mollie refund statuses](https://docs.mollie.com/docs/refunds). Maintain a refund reserve and document the operating calendar/timezone before paid beta.

The client is never asked to fund refunds or pay a platform commission. Valid construction contracts/prices remain between parties; the platform does not arbitrate workmanship or collect final invoices.

## 5. Confirmed direct payment now, bundles later
The founder confirmed individual hosted €1 checkout for MVP, with prepaid bundles, wallets and subscriptions deferred. This avoids upfront commitment and prepaid accounting, accepting one checkout and processing costs per small payment. An optional company bundle could amortize PSP fees later, but requires separate service classification, source/refund accounting and onboarding validation before implementation. Mollie is selected; live merchant/method activation remains separate. See [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md) and [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md).

PWA browser distribution uses web hosted checkout; same server records on all devices. Bank-app return to another context requires ownership/session recovery, not another automatic purchase. Native apps later need a new storefront/payment/receipt/fee assessment; physical carpentry does not by itself settle rules for paid digital publication.

## 6. Costs and arithmetic
[Mollie's Dutch pricing](https://www.mollie.com/nl/pricing), checked 22 October 2026, lists **€0.32 per iDEAL | Wero payment**, **1.8% + €0.25 for EEA consumer cards** and **2.9% + €0.25 for EEA commercial cards**. These are public indicative rates, not our merchant agreement; actual method mix, rounding, optional products, refunds, disputes and currency conversion can differ.

| €1 gross payment scenario | Indicative PSP fee | Before other operating costs, after illustrative 21% VAT and PSP fee |
|---|---:|---:|
| Mollie iDEAL/Wero baseline | €0.32 | €0.5064 |
| Mollie EEA consumer card | €0.268 arithmetic, before PSP rounding | €0.5584 arithmetic, before PSP rounding |
| Mollie EEA commercial card | €0.279 arithmetic, before PSP rounding | €0.5474 arithmetic, before PSP rounding |

At an **illustrative** 21% VAT, €1 gross yields 1/1.21 = €0.8264 before costs. Accountant confirms tax classification/tax point/rounding. The baseline below uses Mollie iDEAL/Wero €0.32; replace assumptions with actual payment mix and settled fees before scale.

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

At three attempts the acquisition ceiling is only about €0.805/job before fixed costs and any omitted method-specific charges. With assumed CPC €0.50 and click-to-eligible-post conversion 10%, demand CAC alone is €5, well above this ceiling. These CPC/conversion numbers are examples, not Google quotes or measured local demand. Do not spend based on them as facts.

A later fully used ten-response bundle could amortize a single €0.32 payment to €0.032/response, but bundles remain outside MVP and this is not permission to implement stored funds. Track actual method fees, refunds, support time, genuine responses and both sides' acquisition cost. Additional refund fees and dispute/exception losses are excluded pending the actual method/merchant terms; even rare chargebacks can dominate €1 revenue. Maintain a method-specific refund reserve, including enough available Mollie balance to avoid queued refunds, and include measured losses before scale.

## 7. Accounting and retained evidence
Before fulfillment, captured funds are a refundable service obligation; fulfillment records fee revenue under the accountant-approved policy. Refunds append correction/credit-note records, never edit history. No withdrawal balance, contractor payout or job-value commission ledger.

Separate minimum statutory platform-fee records from task data. [Belastingdienst](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/) explains that basic business records generally have a seven-year retention period, with other categories differing. Accountant must classify the actual records and start dates. This does not justify retaining every client photo, postcode or offer for years.

Daily reconciliation: PSP paid/refunded/disputed amounts versus attempts, one response fulfillment, journal and billing documents. Flag paid-but-unpublished unresolved attempts. Preserve enough finance-safe identifiers after task deletion to resolve refunds without resurrecting client content.

## 8. Sustainability gate
Free client posting remains the rule even if margins disappoint. Before scale, approve actual positive contribution after variable operations/acquisition plus a credible fixed-cost plan. If it fails, improve fit/organic acquisition and reconsider prospective company pricing or optional tools with explicit approval; do not silently charge clients, guarantee leads, introduce paid ranking or infer final-work success fees.
