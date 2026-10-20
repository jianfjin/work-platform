# Decisions, assumptions and risk register

**Status:** proposed defaults; no human approval implied · **Review date:** 20 October 2026

## 1. Architecture/product decision records

| ADR | Proposed decision | Alternatives rejected for MVP | Trade-off / revisit trigger |
|---|---|---|---|
| ADR-01 | Carpentry only; one local pilot region | All trades nationwide | Lower initial liquidity; widen only after category-level coverage/repeat demand |
| ADR-02 | €1 gross explicit one-time thread access | Variable lead cost; success fee; subscription | Charges before contact/winning; validate willingness and contribution; do not assume lower price equals fairer |
| ADR-03 | €10 prepaid own-service funding | €1 direct checkout; monthly invoicing | Better fee amortization, worse upfront friction/regulatory complexity; G0 approval required |
| ADR-04 | Three **lifetime** distinct firms/job | Unlimited entrants; rotating slots | Strong resale limit but may strand threads; reconsider only with disclosed policy change and evidence, never retroactively |
| ADR-05 | Shared non-commercial scope, private commercial channels | Public price bidding; fully private duplicate clarifications | More UI/moderation states; measure audience comprehension and scope reuse |
| ADR-06 | No automatic contact disclosure on payment | Sell contact as the paid artifact | Client control; may require more in-app interaction; monitor conversion without weakening privacy silently |
| ADR-07 | Modular monolith + PostgreSQL locks/outbox | Microservices, broker-first design | Shared transactional DB is intentional; split only with measured independent scale needs |
| ADR-08 | Coarse city/radius discovery and explainable rules | Precise address maps; opaque ML matching | Less exact distance but less location exposure; PostGIS later only if needed |
| ADR-09 | Hosted PSP only for platform access | Construction escrow, split payouts | Cannot verify construction payment; label reviews honestly; revisit via separate scoped project |
| ADR-10 | Automatic earned no-response restoration with lot provenance | Manual-only support; no response safeguard | Risk of gaming and more liabilities; tune prospectively after fraud/economics review |
| ADR-11 | Original Workshop Daylight system; DESIGN.md contract | Clone an existing brand; generic dashboard theme | Requires prototype testing; visual references inform specification discipline only |

## 2. Open decisions and accountable roles

| Decision | Proposed default | Owner | Deadline/gate |
|---|---|---|---|
| Name/domain/trademark | HoutMatch working name | Founder/legal | Before public identity |
| Pilot geography/categories | Utrecht + ~25 km; four non-structural categories | Product/trust | G0 |
| Permanent versus promotional €1 | Pilot-only P1 | Founder/finance | G0; publish end/change notice policy before sale |
| Prepaid classification and refund rights | Non-transferable own-service access, no expiry | Legal + PSP | G0, hard block on collection |
| VAT, invoice event and rounding | Illustrative 21%; accountant-approved actual treatment | Accountant | G0, before billing implementation finalization |
| Exact restoration language | access-v1 table; <=24h first response, +72h reply deadline | Product/trust/legal | G0 |
| Applicable DSA/P2B/DAC7 duties | Scope assessment; no blanket exemption assumed | Legal | G0/G2; recheck current legislation |
| Providers/regions/subprocessors | EU primary region, private storage; named vendors TBD | Tech/privacy | G0 before contracts |
| Auth and phone verification method | Maintained auth + OTP method TBD | Tech/privacy | T01/T03 |
| Content/finance retention and legal holds | Purpose-separated proposed schedule | Legal/privacy/finance | G0/G2 |
| Support staffing and business-hour promises | Named trust/finance owner, two-day decision target | Founder/trust | G2 |
| Paid-beta budget and refund reserve | Bounded experiment; no scaling at negative contribution | Founder/finance | G0/G2 |

If a blocking decision cannot be approved, keep the product in prototype/test mode. Do not create production accounts or change the business model as a silent fallback.

## 3. Risk register

Likelihood/impact are qualitative planning judgments, not measured probabilities.

| Risk | Likelihood / impact | Mitigation / evidence | Owner / stop condition |
|---|---|---|---|
| €1 economics unviable | High / critical | Actual per-job contribution; cheap acquisition assumptions tested; no hidden breakage revenue | Founder; stop growth if contribution negative |
| Too few local suitable jobs/firms | High / high | Seed real supply first; category/area coverage; response cohorts | Product; pause paid sales into unsupported market |
| Prepay friction hides the advertised price | Medium / high | Explicit €10 amount; prototype and actual conversion; unused-funds refund | Product; no launch if users confuse €1 with total commitment |
| Three lifetime firms all inactive | Medium / high | Track slot-to-response conversion, notifications and fit; do not recycle undisclosed slots | Product; reconsider policy prospectively if liquidity target fails |
| Duplicate debit or cash refund | Low / critical | Row locks, unique references, source lots/reservations, fault-injection tests | Tech/finance; freeze affected purchase flow on first confirmed incident |
| Private content/address leak | Medium / critical | Separate DTOs, scoped channels/media, explicit contact grants, cache tests | Tech/privacy; block release on any confirmed cross-user exposure |
| Phantom jobs / restore gaming | Medium / high | Contact checks, publication review, evidence-coded policy, rate controls | Trust; suspend affected accounts, audit restorations |
| Regulatory/accounting assumptions wrong | Medium / critical | Signed legal/PSP/accounting decisions, current-source recheck | Founder/legal; no paid launch before clearance |
| Quote ambiguity creates disputes | High / medium | Versioned scope, exclusions, gross price/VAT basis and site-visit labels | Product/trust; track disputed assumptions |
| Verification mistaken for workmanship guarantee | Medium / high | Exact check labels and evidence date, no shield/guarantee marketing | Trust; correct misleading copy before launch |
| Processor/region or support access exposes data | Medium / high | Contracts/subprocessor assessment; restricted telemetry/support | Privacy; no unreviewed processor access |
| Payment/email outage or lost worker | Medium / medium | Pending states, durable inbox/outbox, retries, reconciliation | Tech; fail closed for unverified funds |
| Backup restore loses paid events | Low / critical | Post-restore PSP reconciliation, idempotent replay, controlled reopening | Tech/finance; no purchase writes until reconciled |
| Unsupported all-platform superiority claims | Medium / medium | Evidence-based comparisons and interview validation | Product; remove claims beyond research scope |

## 4. Change management

Before changing fee semantics, top-up minimum, lifetime cap, shared/private visibility or restoration rules: record why and supporting cohort evidence; get product/legal/finance/tech review as relevant; version the policy; update PRD/API/tests/copy; communicate prospective changes. Preserve existing purchase terms and lawful privacy controls. A change that needs construction payments, multiple countries, business seats or algorithmic ranking deserves a separate scoped design.

## 5. Current completion status

This pack proposes the product, design system, architecture, technical rules and delivery backlog. It does not claim interviews, business validation, regulatory approval, independent security review, implemented features, test execution or production readiness. The next human decision is G0 scope/discovery approval, not a production deploy.
