# Decisions and risk register
**Version:** 0.4 · **Updated:** 22 October 2026

## Confirmed direction
| Decision | Effect |
|---|---|
| Mission/audience | Semi-public-benefit, low-fee platform primarily for ZZP and SMEs; no implied charity/tax/quality status |
| Expanded trades | Roof repair/new roofing, renovation, insulation, windows, simple plumbing/electrical maintenance and installation alongside previous maintenance trades |
| Three boards | Client tasks, company cooperation/work packages, company tools/materials trade |
| Free service | Companies may view at most five information items per month |
| Subscription service | No numerical monthly information limit during active subscription; price/terms not selected |
| Charging model | Only free and low-price subscription tiers; former €1 offer-publication fee explicitly canceled, with no per-offer checkout |
| Client tasks | Free client posting, email-confirmed guest authority, private offers and selected-field callback consent; no rival visibility or commercial participant cap |
| Task lifecycle | Three calendar months from first publication, immediate denial, <=24h live purge and <=30-day isolated backups; narrow reviewed finance/company/evidence exceptions |
| Original pilot | Whole Groningen/Fryslân client-task scope and company-controlled approximate service radius retained |
| Runtime/data | Cloudflare TypeScript/Hono, React Router/React/Vite/SSR, Render PostgreSQL Frankfurt/Hyperdrive cache disabled, Drizzle/pg and explicit SQL constraints/locks |
| Identity | Better Auth company email/password; invited staff mandatory TOTP/recovery assurance; guest authority separate |
| Providers | Mollie only, Resend dedicated subdomain, private EU R2/Images and minimum server-side PDOK queries |
| Jobs | Durable PostgreSQL inbox/outbox/jobs, bounded Cron initial one-minute recovery, optional immediate attempts after commit |
| GDPR/budget | GDPR/AVG across the project; €100/month ex-VAT infrastructure planning target retained with original exclusions |

[ADR 0013](../docs/adr/0013-expand-to-three-boards-and-free-subscription-services.md) revises the previously approved [v0.3 plan](archive/v0.3/doc/17-implementation-plan.md). The new product direction is confirmed; detailed new policies below are not approved by the old plan acceptance.

## Pending decisions and delivery evidence
| Topic | Recommendation/status | Gate |
|---|---|---|
| Allowance unit/scope | Recommend five unique details per company/month shared across boards; own records/rights/billing excluded and same-month repeats count once | Confirm policy before live enforcement |
| Month and transitions | Define timezone/calendar, cross-month rereads, upgrades/downgrades, refunds/expiry, mail versus viewing | Before live entitlements |
| Subscription terms | Set low price/tax basis, period, manual versus explicit auto-renew, cancellation/refund/grace and changes | Before real checkout |
| Recurring methods | Mollie retained; qualify actual mandates/methods; iDEAL first payment does not approve subsequent SEPA automatically | Before recurring collection |
| Cooperation | Define new-board geography, visibility, work-package relations, interest and retention; no automatic client-data sharing | Before live cooperation data |
| Trade | Propose listings/private inquiry/direct settlement; decide audience, sale/wanted/rental scope, orders/escrow/delivery and sold/closed retention | Before live trade activation |
| Operator/name/sender | Legal operator, brand/domain, From/Reply-To and platform/vendor notices/contracts still required | Before relevant real-data/sender use |
| Runtime/sizing | Compatible versions, actual transactions, quota races, media and new billing callbacks; cost/capacity against €100 infrastructure target | Foundation and release |
| Operations/legal | Separate tax/invoice/retention review, data rights, reports, support/finance/privacy owners, renewal/refund coverage and funding | Before applicable live use/beta |

## Main risks and controls
| Risk | Control/evidence |
|---|---|
| Old fee design reappears | Clear v0.4 authority, archived v0.3, no old publication-payment API assumed active |
| Five-item cap bypass or unfair consumption | Server transaction/unique grant, all-board/path tests, explicit detail action, no quota for previews/forbidden/own-rights management under chosen policy |
| Subscriber treated as all-data access | Same ownership, consent, expiry and safety checks for both tiers |
| Confidential parent project copied | Work-package fields independently authorized/minimized; no inherited grants; parent-deletion and DTO tests |
| Trade scams/unsafe goods/address exposure | Structured listings, reasoned reports/holds, audience/pickup privacy and actual legal/trader-role review |
| Subscription double charge or stale access | Period-effect uniqueness, finite-cache reconciliation, validated unseen renewal IDs and tested expiry/cancel/refund transitions |
| Low-fee promise cannot fund operations | Select price from measured utility/costs; budget for human work/refunds/legal separately; no false cost or work guarantee |
| Scope expansion invalidates schedule | Re-estimate all three boards and entitlement/operations after qualification; old estimates are historical |
| Retention/backup resurrection | Explicit per-board policies, no indefinite new-board storage, independent tombstones and provider/billing restore replay |

All existing unverified market, tax, runtime, recovery and compliance results remain unverified. No app or paid resources were created.
