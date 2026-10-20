# Test strategy, acceptance scenarios and launch checklist

**Status:** proposed verification plan. No application tests have been run because this delivery contains documentation only.

## 1. Test layers and environments

- **Unit/policy:** state transitions, eligibility, fee/restoration deadlines, rounding, DTO projection, quote comparisons and authorization predicates. Use a controllable clock and boundary values.
- **Database integration:** real PostgreSQL with production-equivalent constraints/isolation; concurrent unlock, spend, refund, grant, cancellation and award. Do not replace row locks with an in-memory mock.
- **Contract:** API/DTO/error schema, provider adapters, signed-link authorization and generic email payloads; generated OpenAPI checked against route tests.
- **End-to-end:** client publishes → moderator approves → business verifies/funds/unlocks → private response/shared clarification → quote/award/complete/review; both Dutch and English, mobile/desktop.
- **Security/privacy:** full role/resource matrix including raw HTML/JSON/RSC, media, notification, export, cache and staff-tool surfaces.
- **Operational:** worker crash/retry, PSP timeout/reordering, provider downtime, failed migrations, restore/PITR and post-restore financial reconciliation.

Local/test environments use generated identities and synthetic home images. Staging uses PSP test mode and isolated storage/email. Production testing uses explicitly approved internal accounts and accounted-for nominal transactions only after G2 controls exist.

## 2. Requirement-to-acceptance mapping

| Scenario | Requirements | Given / When / Then |
|---|---|---|
| AT-01 Client verification | FR-01/02 | Given unverified client, when publishing, then blocked without publication; after both verifications and full brief, submission enters review |
| AT-02 Business authority | FR-01/05 | Given pending/suspended business or job owned by same user, when unlocking, then denied, no balance/slot change |
| AT-03 Safe preview | FR-03, FR-04, FR-06 | Given unpaid eligible firm, when reading list/preview/raw render payload, then only approved structured fields appear; address, body, client identity and media URLs absent |
| AT-04 Brief completeness | FR-02/03 | Given unknown dimensions/materials, when submitting honest unknowns, then accepted for review with measurement-needed indicator; invalid units/ranges return field errors |
| AT-05 Exactly one purchase | FR-05/11 | Given sufficient funds, when same key is replayed and then new keys target same business/job, then exactly one entitlement/debit and original receipt; no new slot |
| AT-06 Lifetime cap race | FR-05; NFR-01 | Given two existing lifetime firms and 50 funded contenders, when calls race, then one additional success, total three, one new debit; refunded/withdrawn slots never open a fourth sale |
| AT-07 Atomic failure | FR-05; NFR-01 | Given insufficient funds, injected constraint failure or filled/expired job, when purchasing, then no partial debit, allocation, entitlement, slot or outbox; post-commit timeout can recover receipt |
| AT-08 Verified funding | FR-11 | Given pending/forged/wrong amount/currency/environment callback or browser success redirect, when processed, then no credit; authenticated correct paid observation grants once |
| AT-09 PSP race/recovery | FR-11; NFR-05 | Given callback duplicates/reordering/missing delivery and early browser return, when worker/reconciliation resumes, then correct state and one lot; top-up never reserves/unlocks a filled job |
| AT-10 Media access | FR-06 | Given unauthorized/cross-business/quarantined/deleted attachment, when requesting read link, then no content; clean media has stripped metadata and expires capability within policy |
| AT-11 Conversation isolation | FR-06/08 | Given three entitled firms, when one probes another's channel/message/quote/attachment/export, then 404/no private data; client sees only their own job's channels |
| AT-12 Shared scope | FR-07, FR-13 | Given private commercial message, when client answers shared Q&A, then private content is not auto-published; both composer audience labels and generic email payload verified |
| AT-13 Quote and award | FR-08, FR-09 | Given two simultaneous valid selections, when awarded, then one winner; stale scope, expired quote or wrong owner rejected; losing channels read-only |
| AT-14 Review evidence | FR-10 | Given unawarded or third-party user, when reviewing, then denied; awarded completed job has one client review and accurate unilateral/bilateral evidence label |
| AT-15 Lifecycle races | FR-03/05/09 | Given unlock racing cancel/pause/expiry/material edit, when actions serialize, then result follows locked state; no purchase after effective expiry; material change requires acknowledgment, never re-charge |
| AT-16 Restoration policy | FR-12 | Given cancel-before-participant-response or confirmed invalid job, when evaluated/repeated, then original spend reversed once, provenance preserved, entitlement read-only and slot retained |
| AT-17 No-response boundaries | FR-12/13 | Given first response at <=24h and no qualifying client reply/outcome by response+72h, when evaluated, then restore once; test 24h/72h just before/exact/after and late-worker execution |
| AT-18 Cash refund/dispute | FR-11/12 | Given refund/spend race, partial refund or PSP ambiguous timeout/chargeback, when reconciled, then source cap/reservation respected, no double payout, wallet nonnegative, exposure separately recorded |
| AT-19 Operations privilege | FR-14 | Given normal support, when accessing private chat or executing unauthorized finance action, then denied; break-glass requires separate privilege/case/reason and immutable audit |
| AT-20 Rights/retention | FR-14; NFR-08 | Given export/deletion request, when processed, then scoped export, third-party redactions, expired media inaccessible, minimum lawful finance retained; restored backups reapply tombstones |
| AT-21 Matching transparency | FR-15 | Given category/radius/date filters, when results rendered, then explain declared fit, city-based distance and availability uncertainty; no paid ranking |
| AT-22 Cross-platform parity/install | FR-16; NFR-09 | Given supported desktop, real Android and real iPhone devices, when using browser and installed mobile modes, then both user roles complete core flows, icons/launch/back navigation work, no install/push permission is required and an existing entitlement is not repurchased on another device |
| AT-23 Offline/cache isolation | FR-17; NFR-08, NFR-09 | Given installed PWA after private activity, when offline, account-switched or storage-inspected, then only allowlisted public static assets/offline page persist; no private content/media/balance/token or queued mutation exists; stale private views are hidden until authorized online revalidation |
| AT-24 Mobile checkout recovery | FR-11, FR-16, FR-17; NFR-09 | Given bank-app/browser switches, terminated PWA, cancelled/pending/late-paid outcome or return to another browser, when reauthenticated, then owned server payment status recovers one verified grant; no automatic job unlock, inferred success or second purchase |
| AT-25 Device input and resume | FR-08, FR-16, FR-17; NFR-09 | Given mobile keyboard/safe areas, denied camera permission, ordinary phone photos and app suspension, when composing/uploading/resuming, then controls remain usable, unsupported media has a safe alternative, saved server drafts resume, unsent memory-only text is not falsely saved/sent and permissions/state revalidate |
| AT-26 PWA update/rollback | FR-17; NFR-09 | Given old installed worker and a waiting new deployment, when composing/in checkout or rolling back, then no forced destructive reload, financial/message replay or private cache exposure; compatible assets remain and approved reload updates safely |

For AT-17: a reply/award committed at or before the deadline defeats no-response eligibility; later events do not erase an already-earned restoration. Evaluate due entitlements under the job lock before accepting late messages/award actions; do not let worker lag change policy outcomes.

### Non-functional traceability

| Requirement | Planned evidence |
|---|---|
| NFR-01 | AT-05–AT-11, AT-13, AT-16–AT-19; real-DB fault/concurrency and cross-principal tests |
| NFR-02 | Complete-flow automated and manual accessibility matrix in section 4 |
| NFR-03 | Realistic dataset/100-user load profile with p95 reads and transaction timing in section 4 |
| NFR-04 | End-to-end synthetic availability monitor, monthly report and dependency-separated outage log |
| NFR-05 | Verified-paid-to-credited timestamp histogram, missing-callback recovery and five-minute alert drill |
| NFR-06 | Lab budgets initially, then real-user p75 LCP/INP/CLS; exclude no user cohort silently |
| NFR-07 | G2 timed DB/media restore and post-restore PSP reconciliation; measured RPO/RTO |
| NFR-08 | Processor/region register, access audit, rights/retention scenarios AT-19/20 and approved legal purpose review |
| NFR-09 | Version-recorded real-device support matrix and AT-22–AT-26 in normal-browser/installed modes; browser emulation cannot substitute |

## 3. Adversarial financial test details

- Same idempotency key across different payloads returns 409; same key under a different actor cannot reveal another receipt.
- Two tabs spend the last €1 on different jobs: one succeeds, other returns insufficient funds; balances and source lots agree.
- Crash before commit rolls back all writes; crash after commit/before HTTP response replays one receipt.
- Callback repeated 100 times grants one lot; caller-selected `paid` strings and unknown IDs grant none.
- PSP payment-creation timeout remains ambiguous until reconciled; no new payment ID by blind retry.
- Refund request reserves funds while provider response is lost; reservation is not freed until verified failed/cancelled.
- Cash refund followed by duplicate dispute observation never removes the same principal twice; spend exposure is visible separately.
- Service restoration racing cash refund or payment dispute preserves source-lot equation and cannot become a second refund.
- All posting transactions balance; lifetime counter equals entitlement count; no direct operator DB edits needed for recovery.

Use at least 20 repetitions of each synchronized capacity/spend race in CI/nightly as appropriate, with independent connections. Include deterministic barriers/fault injection; “50 sequential promises” is not a real contention test. Record test isolation/connection pool settings.

## 4. Performance and accessibility evidence

Load dataset: 10,000 jobs, 100,000 messages and representative media metadata; 100 concurrent active users including discovery, messages and competing unlocks. Measure p50/p95/p99 latency, transaction lock wait, error rates, DB CPU and worker backlog. Pass NFR-03 budgets with no correctness regression; list excluded PSP/network time explicitly.

Target WCAG 2.2 AA across sign-in, job wizard, preview/unlock, top-up/return, conversation, quote comparison and rights requests. Automated checks plus manual keyboard, focus order, screen reader, zoom/reflow, validation errors and reduced motion. Check 320/375/768/1,024/1,440px. No critical fee or audience information may be hidden by truncation/sticky UI.

Prototype tests: at least five representative users complete the purchase/audience tasks; >=80% correctly explain €1 unlock, €10 funding commitment, no guaranteed award, no automatic contact disclosure and shared/private boundary. A visually polished screen does not waive these comprehension gates.

Cross-platform scope: record exact versions at each release against [the support matrix](14-cross-platform-and-pwa.md). Test core flows on desktop Chrome/Edge/Firefox and macOS Safari within the stated support window, plus real Android Chrome and real iPhone Safari in browser and home-screen modes. Include cold/warm launch, small screens, portrait/landscape, denied permissions, offline/slow network, storage eviction, session expiry, checkout return and worker replacement. No PWA/browser implementation has been tested as part of this documentation update.

## 5. Release checklist

### Before internal alpha (G1)

- [ ] All P0 functional acceptance scenarios implemented and passing; failing fixtures prove tests detect unauthorized access/duplicate effects.
- [ ] Core end-to-end flow works without manual database edits.
- [ ] Critical/high-risk security findings resolved; privacy DTO and media authorization reviewed.
- [ ] DESIGN.md state inventory and language checks complete; prototype comprehension gate passed.

### Before accepting money (G2)

- [ ] G0 legal/accounting/PSP decisions signed; current Dutch/English terms and privacy notice published.
- [ ] Production PSP merchant approval, correct webhook reachability and verified paid fulfillment proven.
- [ ] One controlled top-up, unlock, restoration, partial unused-funds cash refund and reconciliation completed end to end with receipts.
- [ ] Backup restore measures RPO <=1h and RTO <=4h; reconciles PSP activity before re-enabling purchase writes.
- [ ] AT-22–AT-26 pass on real Android/iPhone browser and installed PWA; exact OS/browser versions, private-storage inspection and bank-app checkout return evidence recorded.
- [ ] Staff MFA, secrets, incident owner/backup, scoped roles, alerts and support hours verified.
- [ ] Fraud holds, report/appeal routes and data rights operations tested.
- [ ] Genuine supply/demand coverage confirmed; beta budget/refund reserve approved.

### Before growth (G3)

- [ ] >=70% of eligible jobs receive a relevant response within 48h; >=30% receive two comparable offers within seven days.
- [ ] No unresolved duplicate debit/grant, oversold thread or confirmed private-content exposure.
- [ ] No-response/invalidity restoration rates and operator time understood; cost model uses actual cohorts, not top-up cash as profit.
- [ ] Positive contribution after variable acquisition and operations; fixed-cost funding plan approved.
- [ ] At least 100 eligible jobs and four weeks of learning, whichever takes longer; report sample limitations rather than claiming statistical certainty.

## 6. Monitor, stop and recover

P0 alerts: ledger imbalance, duplicate business effect, capacity >3, private data accessible by wrong principal, or grant without verified funding. Stop affected purchase operations immediately and page the named owner. PSP verification >5 minutes, failed refunds or worker lag >5 minutes trigger investigation; do not manufacture success states.

For data exposure: revoke affected access/capabilities, identify scope, preserve minimal audit, assess notification duties and fix/test before reopening. For financial inconsistency: freeze affected wallets/purchases, compare PSP and subledger, append reviewed corrections and preserve receipts. For deployment failure: roll back application safely, not financial history; replay/reconcile pending external effects.

Weekly review: liquidity, response/award/completion evidence, acquisition costs, restoration cohorts, remaining paid liabilities, support minutes, verification freshness, security incidents and new-business exposure. Use “iterate/stop” as valid results; do not increase competitor count or obscure fees to force a growth chart upward.
