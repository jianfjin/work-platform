# Delivery plan and to-do list
**Version:** 0.3 · **Updated:** 21 October 2026
**Boundary:** documents only. All implementation boxes remain unchecked; current scope supersedes earlier task definitions.

## 1. Gates
| Gate | Evidence and owner |
|---|---|
| G0 scope/operating approval | Founder approves company-paid event/gross fee, exact trial boundary, guest email minimum, callback audience, retention clock/exceptions, merchant entity/account, provider and budgets; legal/accountant review |
| G1 internal alpha | Tech/QA prove guest ownership, rival/contact isolation, payment/refund races, alert consent, deletion and responsive flow in test mode |
| G2 bounded paid beta | Merchant approved, production fee/refund reconciliation demonstrated, real Android/iPhone tests, deletion/restore drill, named operations owner and genuine supply |
| G3 scale | Actual positive contribution after variable operations/acquisition, useful response rates and fixed-cost plan; no unresolved finance/privacy/deletion defects |
| G4 dedicated mobile apps | Mature usage/economics plus validated native-only need; separately approved native auth/payment/store/release design and budget |

## 2. Backlog
Engineering estimates are person-days, not calendar promises. Support/recruitment/legal waiting and some QA/design work are separate.
| ID | Priority | Responsibility | Dependencies | Days | Acceptance/output |
|---|---|---|---|---:|---|
| T00 | P0 | Product/founder | — | 2–3 | Resolve G0 decisions, ten client/ten SME interviews and five usability tests planned |
| T01 | P0 | Tech lead | T00 | 2–3 | Repo/CI, supported runtime, isolation, OpenAPI/test scripts and deployment secrets |
| T02 | P0 | Identity/backend | T01 | 3–4 | Guest tokens/sessions, company auth, optional account linking; AT-01/02/25 |
| T03 | P0 | Full-stack | T02 | 4–6 | Lightweight tasks/photos, safe projections and geocoder/region; AT-03/04/19 |
| T04 | P0 | Full-stack/mail | T02,T03 | 3–5 | Company profile/radius, opt-in preferences/digests, unsubscribe/dedup; AT-10 |
| T05 | P0 | Full-stack | T02,T03 | 3–4 | Own rough offer and client-private all-response list; AT-05 |
| T06 | P0 | Backend/finance | T05 | 4–6 | Stripe Checkout + `stripe` server SDK candidate: direct €1, signed inbox, unique fulfillment, refunds and reconciliation; AT-06/07/18/20/24/26 |
| T07 | P0 | Full-stack | T03,T05,T06 | 2–3 | Contact preference, close/reopen/material-edit races; AT-08/09 |
| T08 | P0 | Backend/privacy | T03–T07 | 3–5 | Immutable calendar TTL, token/media/content purge, backup tombstones; AT-11/12/23 |
| T09 | P0 | Operations/full-stack | T02,T06,T08 | 3–4 | Reports, holds/notices/appeals, staff roles and finance/deletion exception queues; AT-13/14 |
| T10 | P0 | Frontend | T03,T05,T06 | 3–4 | PWA manifest/install/static-only worker, safe return/update behavior; AT-15/20/21 |
| T11 | P0 | Frontend/design | T03,T05,T07 | 2–3 | Dutch/English core copy, accessible comparison/permission/fee UI; AT-16 |
| T12 | P0 | Tech/QA | T02–T11 | 4–6 | Real-DB race/security suite, real phones, restore and incident drills; AT-01–AT-26 |
| T13 | P0 | Product/operations | G2,T12,T14 | 2–3 eng + beta operations | Bounded genuine-job beta, measured replies, fees/refunds/costs, G3 decision |
| T14 | P0 | Growth/tech | T00,T01,T04,T11 | 1–2 eng + outreach | Generic regional SEO, consented SME recruitment/referrals, optional separately approved ads; AT-17 |
| T15 | Later | Mobile/product | G4 | Separate estimate | Dedicated Android/iPhone apps alongside Web/PWA |
| T16 | Optional later | Finance/product | G3 | Separate estimate | Company bundles/subscription only if validated and separately designed; no compulsory wallet now |

T00–T14 total: approximately **41–61 engineering person-days**. T15/T16 are excluded. Two engineers plus fractional design/QA/operations might plan roughly 8–10 implementation weeks after G0 and then at least four weeks bounded beta, subject to dependencies/provider/legal work. Re-estimate after discovery; no production date is promised.

## 3. Suggested code boundaries, not created files
| Proposed location | Responsibility |
|---|---|
| src/modules/identity/ | Guest scoped ownership and company/optional client identity |
| src/modules/tasks/ | Brief, region, revision, close/delete deadline and contact preference |
| src/modules/responses/ | Rough offers and client/own-company authorization |
| src/modules/billing/ | Platform fee intent/fulfillment/refund/journal plus small server-only Stripe adapter, not construction payments |
| src/modules/matching/ and alerts/ | Licensed geocoder adapter, radius/preferences, opt-in mail |
| src/modules/privacy/ and operations/ | Purge/tombstones, reports/holds, minimal audit |
| src/app/ and components/ | Localized responsive views/API adapters |
| public/sw.js, public/offline.html, app manifest/icons | Public-static-only PWA shell and safe updates |
| db/migrations/, tests/unit/, tests/integration/, tests/e2e/ | Schema and reproducible tests |

## 4. Implementation checklist
- [ ] T00: Record confirmed payer rule and proposed charge event; clarify Groningen boundary/name/merchant/contact/retention decisions.
- [ ] T00: Interview local clients/SMEs and test free posting versus paid publication comprehension; no new client-fee feature.
- [ ] T01/T02: Write failing guest-token/ownership/enumeration tests; implement short-lived session exchange and optional account association.
- [ ] T03: Test no private postcode/contact in raw DTO/media; implement brief/photo pipeline and licensed coarse region matching.
- [ ] T04: Test opt-out/close/deletion before dispatch; implement verified-recipient opt-in alerts with bounce/dedup handling.
- [ ] T05: Prove paid/unpaid firms cannot list rivals/counts; implement all-response list only for job owner.
- [ ] T06: Confirm Stripe merchant/method/cost approval; pin stable SDK/API versions. Test raw-body signatures, duplicate/out-of-order/async events, unknown-result retry and close/edit races before coding publication/refund; verify one retained fee and refund reconciliation.
- [ ] T07: Test default private mode and explicit responder callback audience; implement revoke/close with no future disclosure.
- [ ] T08: Test month-end/DST, deadline access independent of worker, all object versions and restore; implement automatic purge.
- [ ] T09: Implement case/reason/appeal, staff permissions, fee reconciliation and deletion health; no quality-verification badge.
- [ ] T10/T11: Implement install/help, offline privacy, Dutch/English/accessibility and safe bank-app return on real phones.
- [ ] T12: Run complete tests and timed restore/PSP reconciliation; verify no work data returns after expiry.
- [ ] T14: Recruit actual opt-in companies and real task demand, publish generic category/region pages; no scraped email blast.
- [ ] G2/T13: Approve bounded money/operations budget and refund reserve; run genuine paid beta and review weekly.
- [ ] G3: Decide iterate/stop/scale using actual contribution and useful responses, not website visits alone.
- [ ] G4/T15 later: Approve native-client needs/store payments/security/release plan separately.
- [ ] T16 later: Validate fee-amortization tools before adding prepaid/recurring accounting.

## 5. Critical transaction task example
For T06, use Stripe test mode and the official CLI for real Checkout/webhook forwarding once implementation is approved; synthetic signed fixtures alone do not prove bank-return behavior. Test/live keys and webhook endpoint secrets must be distinct. No SDK/CLI installation or merchant provisioning is performed by this document update.

For T06, first write a real-PostgreSQL integration test: same company/job receives simultaneous fresh-key payment/publication requests, a PSP callback repeats, and client close competes with fulfillment. Before implementation it must fail for missing behavior, not a broken environment.

Implement only the job->response->attempt locked use case. Assert one retained fulfilled fee; either visible valid response before close or tracked full refund if close wins, never a paid orphan or second successful listing. Repeat with lost HTTP response, changed material scope and late-paid callback. Run focused and full suites, inspect raw private DTOs, then commit the slice.

Proposed scripts such as npm run test:integration are implementation outputs of T01, not existing commands in this documents-only repo.

## 6. Budget and “zero cost”
Illustrative build envelope, not a quote: 55 engineering days × €600 = €33,000; six product/design days × €600 = €3,600; eight QA days × €500 = €4,000; legal/accounting €2,500; pilot tools €500; initial refund reserve €100. Subtotal €43,700; 20% contingency = **€52,440**, excluding taxes, test-device hardware, ongoing staffing and optional advertising. Replace with actual staffing/vendor quotes; founder labor can reduce cash spending, not eliminate work or operating risk.

Zero paid advertising is the default initial growth plan. Any €100 Google Ads experiment is separate and needs explicit owner approval/monitoring; it is not automatically funded by this document.

## 7. Done means verified
Per feature: passing policy/real-DB/browser tests as relevant, safe DTO/consent/retention behavior, appropriate audit/telemetry, translated failure states and reviewed migrations/runbooks. No completion merely because a page renders. No paid beta before refund, abuse/privacy handling and deletion controls work.
