# Decisions and risk register
**Version:** 0.3 · **Updated:** 21 October 2026

## 1. Confirmed direction
| Decision | Source / effect |
|---|---|
| Client platform fee zero; companies pay | User's explicit latest instruction overrides contradictory client-€1 examples |
| Northern pilot, Dutch/English | Groningen, Friesland/Fryslân and Drachten labels; exact boundary still open |
| Lightweight guest posting | Optional registration; proposed email ownership for safe management |
| Private rough offers and direct contact | Client sees all eligible responders; companies see only themselves; no commercial three-firm cap |
| Information intermediary | No routine workmanship certification/site monitoring/construction payment handling |
| Three-month task data deletion | Clock/technical purge/finance exceptions need precise approved wording |
| Web/PWA now, dedicated apps later | Earlier user-confirmed cross-platform direction retained |

## 2. Proposed decisions needing approval
| Topic | Current recommendation | Owner / gate |
|---|---|---|
| Name | Work Platform working label; no final brand selected | Founder, before public branding |
| Charge event | €1 gross first successfully published company/task offer; free reading; no construction-success percentage | Founder/product/accountant, G0 |
| Direct payment vs prepaid | Direct hosted €1 checkout; optional bundles only later | Founder/finance/PSP, G0 |
| Geographic interpretation | Groningen and Fryslân provinces; Drachten priority inside Fryslân; confirm city-only alternative | Founder, G0 |
| Client security minimum | Email confirmation/scoped one-use management session; no compulsory account or phone | Product/security, G0 |
| Callback audience | Every eligible published responder, selected email/phone fields; default client-initiated | Product/privacy/legal, G0 |
| Deletion clock | Three calendar months from first publication, no reset by edit/closure | Product/privacy/legal, G0 |
| Purge/backup exception | Access cutoff at deadline, proposed live purge <=24h, backups <=30 days; minimal finance separate | Tech/legal/finance, G0/G2 |
| Merchant/bank | Actual platform legal operator, approved business settlement account; no assistant/personal shared merchant | Founder/accountant/PSP, before money |
| Geocoder | PDOK Location API first candidate; Google optional after cost/terms review | Tech/privacy, G0 |
| Alerts | Confirmed opt-in, daily digest default, optional instant, easy unsubscribe | Product/mail operations, G0 |
| Native maturity | Stable economics/usage plus demonstrated needs; separate G4 design/budget | Founder/product, later |

## 3. Architecture decisions
- One responsive Web/PWA and modular backend; SQL transaction for payment-to-publication; server-only authorization/DTOs.
- One response per company/task, no published rival list/count and no participant slots/counter.
- Direct platform-fee payments/refunds, no mandatory wallet and no construction payout/escrow.
- Guest job sessions, not compulsory client-account onboarding.
- Coarse postcode-based radius, not mandatory GPS or exact-address map.
- Opt-in safe metadata emails, not forwarding full task/client data to arbitrary businesses.
- Task content expires after three calendar months, minimal finance and strictly justified evidence purpose-separated.
- No chat/award/verified-completion/review machinery in current minimum information-platform scope.

## 4. Risks and stop conditions
| Risk | Mitigation / evidence | Accountable owner |
|---|---|---|
| €1 cannot cover PSP, operations and acquisition | Actual cohort margin, founder-funded bounded experiment; no silent client charges | Founder/finance; stop growth at negative contribution |
| Too few relevant northern firms/jobs | Recruit genuine supply, monitor category/area coverage, no fake inventory | Product |
| Unlimited responders overwhelm client | Opt-in callback warning, relevance/rate controls, private paginated list and voluntary close; never hide a commercial cap | Product/trust |
| No routine quality checks attracts abuse | Honest labels, secure accounts, reports/holds/notices, legal required checks separately scoped | Trust/legal |
| Callback consent exposes more than intended | Default private, named audience/fields, revoke/close and disclosure tests | Privacy/tech; block release on bypass |
| Guest link takeover/enumeration | One-use hashed credential, scoped secure session, generic recovery and abuse limits | Tech |
| Paid-after-close/material-change orphan payment | Locking, unique fulfillment, refund obligation, reconciliation and mobile recovery | Tech/finance; no unresolved captured-unfulfilled funds |
| Rival offers/count leak | Distinct DTO/query/API tests including search/cursors/exports | Tech; block release on leak |
| “Three months” incorrectly interpreted as all data vanish | Explicit calendar clock, cutoff/purge/backup tests, finance/evidence separation and plain-language notice | Privacy/finance |
| Regional/Google geocoding cost or license misuse | Source metadata, provider terms, quota/budget controls, lawful coarse fallback | Tech/privacy |
| Alert promotion becomes spam | Affirmative subscription, purpose/frequency controls, unsubscribe/bounce/dispatch checks | Mail operations/legal |
| “Only intermediary” treated as legal immunity | Role-specific legal assessment and reviewed terms | Founder/legal |
| PWA bank return/update retries payment | Owned attempt recovery, no auto replay, real-device tests | Tech/QA |
| Google Ads spends beyond simple daily multiplication | Understand average-vs-cap, approval/monitoring/pause and actual margin-based CAC | Founder/growth |

## 5. Change control and current status
This revision replaces incompatible 0.2 assumptions; Git preserves historical documents. The repository is documentation only: no user/job/PSP data exists to migrate here. Any separately started implementation needs an inventory and an explicit migration plan, not blind deletion or repurposing of its payment records.

Fee payer and PWA direction are user-confirmed. The proposed charge event, exact geography, guest security/retention/merchant details and budgets are not silently approved. No interviews, profitability, legal clearance, application tests or deployment completion is claimed.
