# HoutMatch implementation plan and to-do list

> **Execution boundary:** this is a proposed backlog, not authorization to implement it. After G0 approval, execute sequentially using the approved plan; respect repository instructions and do not delegate or create infrastructure without authorization. All boxes below intentionally remain unchecked.

**Goal:** validate and deliver a privacy-safe carpentry marketplace with one-time paid thread access on desktop Web and Android/iPhone browser plus installed PWA.
**Architecture:** modular monolith with server-only domain/DAL, PostgreSQL financial transactions and a restartable worker.
**Proposed stack:** TypeScript, Next.js/React, PostgreSQL, private object storage, hosted PSP checkout, real-DB integration and browser tests.

## 1. Gates, staffing and sequence

| Gate | Decision owner | Evidence needed to pass |
|---|---|---|
| G0 — approve scope before development | Founder + legal/accountant + tech lead | Interviews; categories/region; €1 gross/€10 funding; three lifetime firms; restoration rules; PSP/prepayment classification; privacy scope; build/beta budget |
| G1 — internal alpha | Tech lead + QA + product | Real-DB money/capacity tests; private-data authorization; coherent prototype; no outstanding critical/high-risk findings |
| G2 — capped paid beta | Founder + finance + trust | PSP production approval; terms/invoices ready; restoration/cash refund/reconciliation working; restore drill; real Android/iPhone browser and installed-PWA acceptance; staffed moderation/support; seeded real supply |
| G3 — public growth | Founder | At least 100 eligible jobs/four weeks directional evidence; liquidity and comprehension targets; positive post-variable-CAC contribution and fixed-cost funding plan |
| G4 — later dedicated mobile apps | Founder + product + tech/finance/legal | Maturity and validated native-only needs; new store/payment/auth/release design and separately approved budget; not part of current implementation |

Suggested team: two engineers, fractional product/design/QA, named trust/finance operator, external legal/accountant. A role is not an assigned person; name real owners at G0. Workstreams below are dependencies, not instructions to spawn parallel agents.

Proposed timing after G0: weeks 1–2 foundation and jobs; weeks 3–4 funding/atomic unlock and media; weeks 5–6 messaging/quotes/trust; weeks 7–9 PWA hardening, real-device checkout/recovery and alpha; weeks 10–13 capped beta/learning. Discovery/G0 may add 1–2 weeks beforehand. These are revised estimates including PWA work, not deadlines or a commitment that all gates will pass. G4 has no calendar commitment.

## 2. Proposed code ownership map (nothing scaffolded yet)

| Proposed repository path | Responsibility |
|---|---|
| `src/app/` | Pages, localized UI and API route adapters only |
| `src/modules/identity/` | Principals, sessions, business verification and role checks |
| `src/modules/jobs/` | Briefs, revisions, previews, lifecycle and scope Q&A |
| `src/modules/billing/` | PSP integration, lots, journal, refunds and reconciliation |
| `src/modules/entitlements/` | Atomic unlock and entitled-access policy |
| `src/modules/conversations/` | Private channels/messages and attachment authorization |
| `src/modules/offers/` | Quote versions, award, completion and review eligibility |
| `src/modules/trust/` | Reports, moderation cases, appeals and audit |
| `src/components/` / `src/styles/` | Accessible shared components and DESIGN.md tokens |
| `src/app/manifest.ts`, `public/icons/`, `public/offline.html` | Web manifest, reviewed install artwork and non-personal offline fallback |
| `public/sw.js`, `src/components/pwa/` | Narrow cache allowlist, safe update lifecycle and optional installation guidance |
| `src/workers/` / `db/migrations/` | Durable processing and reviewed schema |
| `tests/unit/`, `tests/integration/`, `tests/e2e/` | Policy, real database and user-flow verification |

Paths are proposed repo-relative implementation locations, not links to delivered files. Confirm the actual structure at kickoff. Do not create placeholder code just to make this table true.

## 3. Dependency-aware backlog

Effort is estimated **engineering person-days**, excluding external waiting, discovery and much of UI/QA/support labor. P0 = paid-beta prerequisite; P1 = post-beta candidate. Acceptance references point to [Test plan](11-test-and-launch-plan.md).

| ID | Priority | Owner role | Depends on | Effort | Deliverable / acceptance |
|---|---|---|---|---:|---|
| T00 | P0 | Product/founder | — | 2–4 | Discovery and G0 decisions; signed decision register, no unresolved payment/privacy blocker |
| T01 | P0 | Tech lead | T00 | 2–3 | Repo/toolchain/CI/env boundaries; lint/typecheck/unit/integration commands work |
| T02 | P0 | Backend | T01 | 3–4 | Schema, migrations, DTOs, API/OpenAPI baseline; constraints and contract tests |
| T03 | P0 | Full-stack | T01,T02 | 3–4 | Sign-in, verified client contacts, business/KVK ops; AT-01/02 |
| T04 | P0 | Design/frontend | T00,T01 | 2–3 | Tokenized components and five-task prototype; accessibility and price/audience comprehension |
| T05 | P0 | Full-stack | T02,T03,T04 | 4–5 | Job wizard, revision/state model, review queue, safe previews and filtering; AT-03/04/15 |
| T06 | P0 | Backend | T02,T03 | 4–5 | Journal, source lots, PSP intents/verified funding; AT-08/09 and balanced-ledger assertions |
| T07 | P0 | Backend | T05,T06 | 3–4 | Atomic unlock, idempotency and lifetime capacity; AT-05/06/07; load/concurrency tests |
| T08 | P0 | Full-stack | T03,T05 | 2–3 | Private media quarantine/scan/download; AT-10 and hostile upload fixtures |
| T09 | P0 | Full-stack | T07,T08 | 3–4 | Private messages, shared board, notification outbox; AT-11/12 |
| T10 | P0 | Full-stack | T05,T09 | 3–4 | Quote versions, comparison, atomic award/complete/review; AT-13/14 |
| T11 | P0 | Backend + finance | T06,T07,T09 | 4–5 | Automatic/manual restorations, cash reservations/refunds, payment disputes/reconciliation; AT-16/17/18 |
| T12 | P0 | Full-stack + trust | T03,T09,T10,T11 | 3–4 | Scoped operations, reports, holds, appeals, export/deletion; AT-19/20 |
| T16 | P0 | Frontend + tech lead | T01,T04,T05 | 3–4 | Manifest/icons/install help, public-static-only worker/offline page, safe updates and no private persistence; AT-22/23/26 |
| T17 | P0 | Full-stack + QA | T06–T12,T16 | 2–3 eng + device QA | Android/iPhone installed/browser checkout, resume, camera/file selection and cross-device entitlement recovery; AT-22–AT-26 |
| T13 | P0 | Tech lead + QA | T05–T12,T16,T17 | 4–6 | End-to-end/adversarial/load/a11y suite, restore drill, observability/runbooks; G1/G2 evidence |
| T14 | P0 | Product + operations | G2,T13 | 1–2 eng + 4 weeks ops | Capped paid beta, real supply, support/economics dashboard; G3 review |
| T15 | P1 | Product | G3 | Re-estimate | Saved searches, workflow integrations and optional language assistance, only if validated |
| T18 | Later, gated | Product + mobile/tech lead | G4 | Separate estimate | Dedicated Android/iPhone clients; choose technology and store/payment/auth strategy only after maturity approval |

Range total for current P0 tasks T00–T14 plus T16/T17: approximately 51–70 engineering person-days, including an estimated 5–7 additional engineering days for PWA delivery/device recovery. P1 and later T18 are excluded. Two engineers have limited overlap and noncoding work, so person-days are not calendar days; real-device QA needs separately allocated time/hardware.

## 4. Executable checklist

### Chunk A — decisions and foundation

- [ ] T00: Run 10 client and 10 carpenter interviews; record anonymized evidence and purchase objections.
- [ ] T00: Approve market/category/budget and versioned fee/restoration terms; close G0 legal/accounting/PSP questions.
- [ ] T01: Choose maintained supported runtime/auth libraries; lock versions; set up CI and isolated local/staging/test PSP environments.
- [ ] T02: Write schema/DTO contract tests first; migrate an empty and populated test DB; generate reviewed OpenAPI.
- [ ] T03: Test failed verification, suspended account and own-job restrictions before implementing happy paths.
- [ ] T04: Implement semantic tokens and all critical component states; test Dutch/English and mobile purchase comprehension.
- [ ] T05: Test safe raw payloads before implementing previews; add wizard, moderation, freshness, revisions and expiry.
- [ ] T16: Define exact supported browser/OS versions; write cache/privacy and lifecycle tests before manifest/install/worker implementation; add icon variants and offline help.

### Chunk B — access sales and privacy boundary

- [ ] T06: Write failing balanced-journal and duplicate-PSP-grant tests; implement source lots and verified funding.
- [ ] T06: Prove pending/forged/wrong-amount/wrong-environment callbacks cannot credit; reconcile missing callback.
- [ ] T07: Write concurrency tests on real PostgreSQL; prove failure before implementing unlock (example below).
- [ ] T07: Implement the minimum job/wallet transaction; no external API inside locks; verify timeout replay.
- [ ] T08: Test cross-owner attachments and hostile uploads; implement quarantine, safe derivatives and short-lived read links.

### Chunk C — conversations, decisions and finance operations

- [ ] T09: Write cross-business private-channel tests; implement distinct shared/private composers and message deduplication.
- [ ] T10: Test latest-scope/quote validity and simultaneous awards; implement comparison/selection, completion labels and reviews.
- [ ] T11: Test every policy boundary, restore-once rule, cash refund/spend race and ambiguous PSP outcome; implement reversals and holds.
- [ ] T12: Test support/finance privilege separation, legal holds and third-party redaction; implement case-based operations and rights workflows.
- [ ] T17: Execute Android/iPhone browser and installed-PWA scenarios on real devices: install, file/camera permissions, bank-app return, session loss, app termination, logout/account switch and stale worker update; resolve every incorrect purchase/replay outcome.

### Chunk D — verify, pilot and decide

- [ ] T13: Run all acceptance cases, real-DB races, load tests and manual accessibility/security reviews; fix critical findings.
- [ ] T13: Restore DB/media in staging and reconcile post-restore PSP activity; record measured RPO/RTO.
- [ ] T13: Approve runbooks, alert ownership, secret rotation, backup retention and incident responsibilities.
- [ ] T14: Recruit 15–20 verified businesses; establish adequate craft/area coverage; seed only real consented jobs.
- [ ] T14: Run bounded paid beta; monitor daily balance/capacity/privacy and weekly liquidity/economics.
- [ ] G3: Decide iterate/stop/scale from evidence; preserve original purchase terms in any pricing change.
- [ ] G4/T18 later: Only after maturity evidence and a separate decision, plan dedicated Android/iPhone apps, storefront/payment compliance, native auth/push and release operations; do not bundle them into the PWA budget.

## 5. Critical-task example: T07 atomic unlock

Implementation team should break each backlog item into similarly small test-first steps; this example is the financial-risk reference, not production code.

1. Add integration scenario in proposed `tests/integration/unlock-concurrency.test.ts`: create one OPEN job with two lifetime entitlements and fund 50 distinct eligible firms; start 50 unlock calls together.
2. Run proposed `npm run test:integration -- unlock-concurrency` against real PostgreSQL. Before implementation, verify it fails for the missing behavior, not a broken test environment.
3. Implement the locked use case in proposed `src/modules/entitlements/unlock.ts` and source allocation in billing; add unique constraints through reviewed migration.
4. Assert exactly one new entitlement, total lifetime count three, one 100-cent debit, 49 unchanged contender balances and balanced journal. Repeat at least 20 times with synchronized start and controlled connection pools.
5. Add same-business concurrent-new-key and post-commit-timeout cases; replay cannot create a second debit. Add award/cancel/expiry races.
6. Run the focused suite plus full integration suite; review diff for price/PII leaks; commit only that verified slice.

The suggested npm scripts are to be created by T01/T02; they do not exist in this documentation-only repository yet. Expected final outcomes and assertions, not merely exit codes, are required evidence.

## 6. Budget envelope and scope control

Illustrative revised planning budget, **not a quote**: 67 engineering days × €600 = €40,200; 10 product/design days × €600 = €6,000; 10 QA days × €500 = €5,000; external legal/accounting €2,500; pilot infrastructure/operations tools €500; acquisition experiment €1,000. Subtotal €55,200; 20% contingency → approximately €66,240 before applicable taxes, test-device purchase/rental and ongoing support staffing. This adds PWA delivery/recovery and extra device QA to the previous envelope; dedicated mobile apps and optional Web Push remain excluded. Rates and hours are assumptions to replace with real quotes.

Approve a separate beta spend ceiling and refund reserve. No public paid acquisition if contribution remains negative. Cut P1 first; never cut privacy, transaction consistency, financial reconciliation or cash-refund controls to meet a date.

## 7. Definition of done

Each delivered feature has passing unit/policy tests, real-DB integration where needed, authorized DTOs, relevant browser states, audit/telemetry without PII, updated documentation and reviewed migrations. No task is complete because a UI renders or a mock webhook test passes. Gate evidence is named, reproducible and attached to the release decision.
