# Product requirements
**Version:** 0.4 · **Updated:** 22 October 2026

## 1. Confirmed direction
The platform is semi-public-benefit and low-fee, serving ZZP and SMEs. It includes client maintenance tasks, company cooperation/work packages and tool/material trade. Free companies may view at most five information items per month; active subscribers receive information without a numerical monthly limit. Client posting remains free.

Trades include roof repair, new roofing, renovation, insulation, windows/frames, plumbing/electrical maintenance and installation, plus the existing maintenance trades; see [category design](22-platform-redesign.md#2-工种目录). The selected stack, GDPR, €100/month ex-VAT infrastructure target and client-task Groningen/Fryslân pilot are retained.

## 2. Commercial policy status
The founder confirmed cancellation of the former €1 offer-publication fee: only free and low-price subscription services remain. Authorized offer submission, publication and editing have no separate per-offer charge or checkout. Subscription price, period, renewal consent, cancellation/refunds and exact quota semantics remain open; archived per-offer payment/refund rules do not supply these terms.

Proposed quota defaults: one company-wide pool of five unique detail accesses per calendar month across all boards; repeat reads within the month do not re-count; own-content management and rights/billing functions never consume the allowance. Preview versus detail, cross-month rereading, timezone, notification counting and mid-period plan changes must be settled before enforcing a live commercial entitlement. Technical work may use an explicit test policy without presenting it as approved terms.

## 3. Functional requirements
| ID | Requirement | Current acceptance boundary |
|---|---|---|
| FR-01 | Guest client tasks | Confirm email before publication; short-lived task-scoped access/recovery; no mandatory client account; company identity grants no automatic guest ownership |
| FR-02 | Briefs, categories and photos | Expanded categories; postcode/city validation; optional six photos/task, 10 MB each; JPEG/PNG/WebP/HEIC to sanitized JPEG/WebP; no full postcode/contact in company projection |
| FR-03 | Company identity | Serve ZZP/SMEs with verified-email/password accounts, self-declared services/base/radius and company-scoped plan rights; no quality certification claim |
| FR-04 | Information discovery | Separate board filters and safe previews; current authorization and plan entitlement for details; approximate radius, not mandatory GPS |
| FR-05 | Private task offers | Client sees own task offers; company sees own only; no rival identities/counts/ordering; no commercial responder cap; active edits free |
| FR-06 | Free and subscription access | Only free and low-price subscription tiers; free maximum five items/month and active subscription no numerical monthly information cap; no separate offer-publication charge; publish exact approved counting/period/plan terms before live use |
| FR-07 | Client contact choice | Default private; explicit selected email/phone access only for eligible published responders while task is open; subscription alone never grants contacts; revocation/closure stops future display |
| FR-08 | Task lifecycle | Version material edits; scope labels on old offers; close/reopen before original expiry resets contacts; final offer withdrawal is marker-only and cannot silently republish; no inference of work award |
| FR-09 | Opt-in matching alerts | Separate marketing/task-alert opt-in from paid subscription; daily digest default, optional instant, easy unsubscribe; no full-detail email bypass of plan/privacy rules |
| FR-10 | Retention and erasure | Client task first publication + three calendar months, immediate denial, <=24h live purge, <=30-day isolated backups and deletion-safe restore; explicit separate new-board policy before live publication |
| FR-11 | Reports and trust | Case-scoped reports, reasons, holds/removal and correction/appeal for all boards; address client confidentiality and misleading/unsafe/stolen goods reports |
| FR-12 | Staff operations | Invite-only staff password/TOTP or one-use recovery code; scoped roles; member/quota support, moderation, finance exceptions and erasure monitoring |
| FR-13 | Security | Server-owned authorization, real-type media validation, redacted logs, bounded abuse controls, hashed guest tokens, no private offline cache |
| FR-14 | Web/PWA | Desktop/Android/iPhone browser and installed flows; quota-safe navigation and payment recovery; native apps later |
| FR-15 | Dutch/English and accessibility | Critical board, quota, plan, fee, consent and retention copy localized; user content not silently translated |
| FR-16 | Low-cost community growth | Genuine ZZP/SME participants, generic regional/trade content and opted-in outreach; no fabricated reviews or scraped job emails |
| FR-17 | Platform billing | Mollie only; price/terms/method policy must be approved before activation; verified charges and period-level membership effects, bounded refunds/reversals and reconciliation; no task-detail retention in finance records |
| FR-18 | Scope and visibility versions | Task/work-package edits preserve historical context without renewing expiry or automatically disclosing a parent project; stale entitlement/consent decisions rejected |
| FR-19 | Cooperation board | Company creates work packages, publishes partner needs and receives scoped private expressions of interest; no automatic original-task access or subcontract award |
| FR-20 | Tools/materials board | Company publishes typed trading information, condition/specification/quantity/price/area and availability; initial contact/settlement boundary remains an explicit design proposal |
| FR-21 | Shared access accounting | Enforce the approved allowance atomically and idempotently across all full-detail paths; previews/prefetch/unauthorized/expired reads cannot consume or bypass it; live policy must define plan transitions |

FR-06/17 replace the v0.3 publication-fee implementation contract. Version 0.3 requirements/acceptance cases are [archived](archive/v0.3/README.md); no implemented code or live billing is being migrated.

## 4. Non-functional requirements
| ID | Target / evidence |
|---|---|
| NFR-01 | No cross-client/company disclosure, consent bypass, duplicate retained billing effect or quota overrun |
| NFR-02 | Client-task calendar deadline and <=24h live/<=30-day backup deletion; new-board retention explicit; restores cannot resurrect erased data |
| NFR-03 | Retain initial p95 safe reads <500ms and local entitlement/business transaction <1s excluding external PSP/network at 100 active users/10,000 information items; revised workload must be qualified |
| NFR-04 | WCAG 2.2 AA target, 320px reflow, keyboard/screen reader, clear free/subscription and three-board comprehension |
| NFR-05 | Real Android/iPhone browser/installed media, session, subscription return and update tests; no private persistence/offline mutation replay |
| NFR-06 | Durable notification dedup and dispatch eligibility, opt-out and allowance rules; no inbox-arrival guarantee |
| NFR-07 | EU primary storage, processor/transfer review, secure sessions/secrets; RPO <=1h/RTO <=4h proven with billing/deletion reconciliation |
| NFR-08 | 99.5% monthly pilot availability target; versioned prices/terms/allowance policies; alerts for privacy, finance, quota and purge failures |
| NFR-09 | GDPR/AVG evidence before real personal data: purposes/roles/notices, rights, contracts/transfers, DPIA/DPO screening, retention and breach readiness |
| NFR-10 | Concurrent free accesses cannot exceed the approved five-item monthly rule; all APIs/mail/media/export paths honor the same entitlement policy without stale cached grants |

No performance, recovery, legal, pricing or capacity result is claimed by the design. No unconditional access follows from the word unlimited.
