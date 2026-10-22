# Delivery plan and to-do list
**Version:** 0.3 · **Updated:** 22 October 2026
**Boundary:** documents only. All implementation boxes remain unchecked; current scope supersedes earlier task definitions.

## 1. Gates
| Gate | Evidence and owner |
|---|---|
| G0 scope and qualification baseline | Confirmed product rules, stack and €100/month ex-VAT infrastructure target recorded in the [decision register](12-decisions-and-risks.md); align implementation contracts and qualification work. Synthetic-data development can proceed while launch prerequisites are completed |
| Before any real personal-data use | NFR-09: identify controller/operator, reviewed purposes/notices/retention, rights workflow, vendor contracts/transfers, DPIA/DPO screening and breach readiness; applies at any development/alpha/beta gate |
| G1 internal alpha | Synthetic data by default; NFR-09 evidence required before any real personal-data use. Tech/QA prove guest ownership, rival/contact isolation, payment/refund races, alert consent, deletion and responsive flow in test mode |
| G2 bounded paid beta | Legal/tax/terms and merchant/method readiness, controlled production fee/refund reconciliation, real Android/iPhone tests, deletion/restore drill, named operations owners, actual operating budget/refund reserve and genuine supply; NFR-09 evidence already required before real personal data |
| G3 scale | Actual positive contribution after variable operations/acquisition, useful response rates and fixed-cost plan; no unresolved finance/privacy/deletion defects |
| G4 dedicated mobile apps | Mature usage/economics plus validated native-only need; separately approved native auth/payment/store/release design and budget |

## 2. Backlog
Engineering estimates are person-days, not calendar promises. Support/recruitment/legal waiting and some QA/design work are separate.
| ID | Priority | Responsibility | Dependencies | Days | Acceptance/output |
|---|---|---|---|---:|---|
| T00 | P0 | Product/founder | — | 2–3 | Resolve G0 decisions, ten client/ten SME interviews and five usability tests planned |
| T01 | P0 | Tech lead | T00 | 2–3 original baseline; re-estimate | Qualify selected TypeScript/Workers backend: frontend/API contract, framework/auth, PostgreSQL connection/transactions/cache settings, durable callback, job recovery and private image path; then repo/CI, isolation, OpenAPI/scripts and secrets |
| T02 | P0 | Identity/backend | T01 | 3–4 baseline | Guest tokens/sessions and recovery, company/staff auth, no client signup/linking; AT-01/02/25; re-estimate after runtime qualification |
| T03 | P0 | Full-stack | T02 | 4–6 | Lightweight tasks/photos, safe projections and geocoder/region; AT-03/04/19 |
| T04 | P0 | Full-stack/mail | T02,T03 | 3–5 | Company profile/radius, explicit opt-in, daily digest default/optional instant emails, unsubscribe/dedup across frequency changes; AT-10 |
| T05 | P0 | Full-stack | T02,T03 | 3–4 | Own rough offer and client-private all-response list; AT-05 |
| T06 | P0 | Backend/finance | T05 | 4–6 baseline | Mollie-hosted €1 checkout limited to validated iDEAL/Wero/card methods, classic payment-ID notification inbox, authenticated retrieval, unique fulfillment/refunds and reconciliation beyond one-hour credential-scoped cache; AT-06/07/18/20/24/26; re-estimate after runtime/payment qualification |
| T07 | P0 | Full-stack | T03,T05,T06 | 2–3 | Contact preference, close/reopen/material-edit races; AT-08/09 |
| T08 | P0 | Backend/privacy | T03–T07 | 3–5 original baseline; re-estimate | Immutable calendar TTL, token/media/content purge, backup tombstones and scoped data-rights actions; AT-11/12/23/27 |
| T09 | P0 | Operations/full-stack | T02,T06,T08 | 3–4 original baseline; re-estimate | Reports, holds/notices/appeals, staff roles, finance/deletion exceptions and verified rights-request/deadline handling; AT-13/14/27 |
| T10 | P0 | Frontend | T03,T05,T06 | 3–4 | PWA manifest/install/static-only worker, safe return/update behavior; AT-15/20/21 |
| T11 | P0 | Frontend/design | T03,T05,T07 | 2–3 | Dutch/English core copy, accessible comparison/permission/fee UI; AT-16 |
| T12 | P0 | Tech/QA | T02–T11 | 4–6 | Real-DB race/security suite, real phones, restore and incident drills; AT-01–AT-27 |
| T13 | P0 | Product/operations | G2,T12,T14 | 2–3 eng + beta operations | Bounded genuine-job beta, measured replies, fees/refunds/costs, G3 decision |
| T14 | P0 | Growth/tech | T00,T01,T04,T11 | 1–2 eng + outreach | Generic regional SEO, consented SME recruitment/referrals, optional separately approved ads; AT-17 |
| T15 | Later | Mobile/product | G4 | Separate estimate | Dedicated Android/iPhone apps alongside Web/PWA |
| T16 | Optional later | Finance/product | G3 | Separate estimate | Company bundles/subscription only if validated and separately designed; no compulsory wallet now |
| T17 | After MVP | Product/identity | MVP delivered | Separate estimate | Optional client accounts, multi-task account dashboard and proven task linking only if prioritized; preserve guest ownership and original expiry |

T00–T14 original baseline: approximately **41–61 engineering person-days**, before optional client accounts/linking were deferred and Cloudflare hosting was requested. T15/T16/T17 are excluded from MVP. Include explicit GDPR evidence and rights-handling work in the revised estimate. The earlier two-engineer planning assumption was roughly 8–10 implementation weeks after G0 and then at least four weeks bounded beta; this is not a revised estimate. Re-estimate against the confirmed decisions, discovery results and Cloudflare runtime qualification, including final scope and dependencies; no production date is promised.

## 3. Suggested code boundaries, not created files
| Proposed location | Responsibility |
|---|---|
| src/modules/identity/ | Guest task-scoped ownership/recovery and company/staff identity; no client signup/linking in MVP |
| src/modules/tasks/ | Brief, region, revision, close/delete deadline and contact preference |
| src/modules/responses/ | Rough offers and client/own-company authorization |
| src/modules/billing/ | Platform fee intent/fulfillment/refund/journal plus small server-only Mollie Payments API adapter, not construction payments |
| src/modules/matching/ and alerts/ | Licensed geocoder adapter, radius/preferences, opt-in mail |
| src/modules/privacy/ and operations/ | Purge/tombstones, reports/holds, minimal audit |
| src/app/ and components/ | Localized responsive views/API adapters |
| public/sw.js, public/offline.html, app manifest/icons | Public-static-only PWA shell and safe updates |
| db/migrations/, tests/unit/, tests/integration/, tests/e2e/ | Schema and reproducible tests |

## 4. Implementation checklist
- [x] Planning approval on 22 October: Founder confirmed the consolidated [implementation plan](17-implementation-plan.md); synthetic-data runtime qualification is the first implementation milestone. Implementation and launch-evidence items remain open.
- [x] T00 decision: Record confirmed payer, first-publication event and €1 VAT-inclusive price in [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md).
- [x] T00 decision: Confirm whole Groningen and Fryslân provinces, including Drachten, with company-selected matching radii.
- [x] T00 decision: Confirm email before publication, task-scoped short-lived email access and fresh-link recovery without registration; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md).
- [x] T00 decision: Confirm private-default contact and explicit selected-field access for every published responder while open, with revocation stopping future disclosure; [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md).
- [x] T00 decision: Confirm immutable task-content expiry three calendar months from first task publication, independent of paid offer publication and later activity; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).
- [x] T00 decision: Confirm immediate expiry access cutoff, live task/all photo-version purge within 24h and isolated backups within 30 days, with deletions reapplied before restored service; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).
- [x] T00 decision: Confirm separate minimum billing, active company/alert and strictly necessary case-evidence retention; evidence holds require a named case, restricted access and review/deletion date; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).
- [x] T00 decision: Defer optional client accounts, multi-task account dashboards and task linking until after MVP; company/staff accounts remain in scope; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md).
- [x] T00 decision: Confirm material scope revisions, full refund of captured unpublished old-scope offers, and continued visibility/free edits with scope-change labels for published offers; [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md).
- [x] T00 decision: Allow voluntary reopening before original expiry with offers/fees intact, no new fee, private contact pending fresh opt-in, and no revival of canceled attempts; [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).
- [x] T00 decision: Make published-offer withdrawal final for company/task in MVP; client company-name/withdrawn marker only, offer/contact fields hidden, callback access stopped, original fee retained without automatic refund and no restoration/republishing/new checkout; [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).
- [x] T00 decision: Confirm individual hosted checkout for each €1 publication fee and defer prepaid bundles/wallets/subscriptions until after MVP; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md).
- [x] T00 decision on 22 October: Select Mollie as the sole current provider, without a current-stage alternative/fallback evaluation; [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md).
- [x] T00 decision on 22 October: Target iDEAL/Wero and cards only; require merchant approval and successful €1 payment, full-refund and phone/PWA return tests before enabling each; [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md).
- [x] T00 decision on 22 October: Full €1 refund for substantiated fake/duplicate/materially misrepresented tasks; seven-day reporting target from payment with credible later evidence reviewed; recorded approval/reason and minimum case evidence; no reply/lost work alone does not qualify; [payment policy](08-payments-and-economics.md).
- [x] T00 decision on 22 October: Target refund submission to Mollie within one business day of verification or staff approval; automatically submit verified failed-publication/duplicate-capture cases; escalate overdue/queued/failed obligations and track submission separately from completion; [payment policy](08-payments-and-economics.md).
- [x] T00 decision on 22 October: Explicit task-alert opt-in, daily digest default with optional instant emails, category/radius matching and easy unsubscribe; [operations](15-operations-and-growth.md).
- [x] T00 hosting direction on 22 October: Founder requests Cloudflare for the app; retain Render as comparison baseline and qualify the exact Workers framework/database/job/media design; [comparison](18-cloudflare-vs-render.md).
- [x] T00 delegated backend choice on 22 October: Select TypeScript/Workers over FastAPI for expected runtime overhead and shared tooling under the founder's performance/latency/concurrency priorities; actual performance remains to validate; [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md).
- [x] T00 database decision on 22 October: Paid Render PostgreSQL in Frankfurt confirmed, accessed from Cloudflare through Hyperdrive with application query caching disabled; exact plan/version, connection and restore qualification remain work; [ADR 0009](../docs/adr/0009-use-render-postgresql-in-frankfurt.md).
- [x] T00 backend HTTP framework on 22 October: Hono confirmed for the TypeScript/Workers API; exact version and integration qualification remain implementation work; [architecture](04-architecture.md).
- [x] T00 frontend decision on 22 October: React Router framework mode with React/Vite/SSR confirmed; Hono remains the HTTP API and exact build/integration qualification remains work; [ADR 0010](../docs/adr/0010-use-react-router-for-web-pwa.md).
- [x] T00 authentication-library decision on 22 October: Better Auth confirmed for company/staff accounts with records in Render PostgreSQL; guest task-scoped links remain separate, and login/MFA/recovery details remain open; [ADR 0011](../docs/adr/0011-use-better-auth-for-company-and-staff-accounts.md).
- [x] T00 company sign-in on 22 October: Verified email/password confirmed, including email verification and password recovery; staff sign-in/MFA/recovery details remain separate; [technical design](05-technical-design.md).
- [x] T00 staff access on 22 October: Invite-only email/password, mandatory authenticator-app TOTP and one-use recovery codes confirmed; password recovery preserves MFA, with enrollment/recovery/role-change enforcement still to implement and verify; [technical design](05-technical-design.md).
- [x] T00 email-provider decision on 22 October: Resend confirmed for direct platform sending with application-specific credentials and a dedicated sending subdomain; exact domain/address, actual plan/budget, durable delivery/verified events and processor/retention qualification remain work; [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md).
- [x] T00 photo-storage decision on 22 October: Private Cloudflare R2 with explicit EU jurisdiction and authorized Worker reads confirmed; public access stays disabled, with sanitization/erasure and exact configuration still to qualify.
- [x] T00 privacy direction on 22 October: GDPR/AVG compliance explicitly confirmed as a project-wide requirement (NFR-09); this records the requirement, not completed compliance.
- [ ] T00/T01 before real personal-data use: Complete the processing/data-flow register, purpose/lawful-basis and notice review, provider-role/contracts/subprocessor/transfer review (including Resend US account data), DPIA/DPO screening and accountable rights/breach procedures; [required evidence](09-security-privacy-and-trust.md#8-compliance-launch-checklist).
- [x] T00 geocoding decision on 22 October: PDOK Location API confirmed, with minimum server-side queries and local approximate radius matching; dataset/terms/CRS/limits, province accuracy and GDPR qualification remain work.
- [x] T00 photo-upload limits on 22 October: Maximum six optional photos per task and 10 MB per file confirmed; enforce server-side count/byte limits and concurrent-upload quota, with format/decoder and processor qualification still open.
- [x] T00 photo formats on 22 October: JPEG/PNG/WebP and iPhone HEIC inputs, converted to sanitized JPEG/WebP for display, confirmed; real-type checks, HEIC fixtures, orientation/metadata and processor/runtime/privacy qualification remain work.
- [x] T00 image processor on 22 October: Cloudflare Images Workers binding confirmed for upload-time conversion with private EU R2 variants; metadata, runtime, budget and GDPR qualification remain work, with no resources provisioned.
- [x] T00 background jobs on 22 October: PostgreSQL-backed inbox/outbox/jobs and bounded Cloudflare Cron handlers confirmed, initially a one-minute recovery poll with optional immediate attempts after durable commit; shared leases/idempotency, interruption/recovery and backlog capacity remain qualification work.
- [x] T00 database-access tooling on 22 October: Drizzle ORM with node-postgres (pg), typed schema/queries and reviewed SQL migrations confirmed; explicit locks/constraints and runtime/auth-adapter qualification retained. No dependency or migration has been installed/applied.
- [x] T00 infrastructure planning budget on 22 October: €100/month excluding VAT confirmed for the selected services plus required backups/basic monitoring/staging; exact plans and usage costs remain qualification work. Human work, domains, payment fees/refund reserve, ads and future AI are separate.
- [ ] T00 remaining: Finalize the implementation/launch deliverables; finalize the sending subdomain/address after platform naming and document staff provisioning/loss-of-all-factors recovery, exact package versions, remaining service providers, Render database configuration/budget, name/merchant and operating decisions; document the business calendar and staffing for the confirmed refund target, validate live activation of the confirmed method targets, complete discovery planning and professional review of exact retention purposes/fields/schedules. Validate the confirmed regional scope's postcode/municipality mapping during T03.
- [ ] T00: Interview local clients/SMEs and test free posting versus paid publication comprehension; no new client-fee feature.
- [ ] T01/T02: Write failing guest-token/ownership/enumeration tests; implement short-lived session exchange/recovery and company/staff auth. Keep client account creation/linking outside MVP.
- [ ] T01 qualification: Prove the chosen framework/dependencies in Workers preview/deployed runtime, real PostgreSQL transaction/rollback, no stale Hyperdrive/application state, durable Mollie inbox, the confirmed PostgreSQL/Cron job path with immediate-attempt/lease-recovery races, and the selected Cloudflare Images-to-private-R2 path, including actual HEIC, metadata removal and interrupted/deletion-race behavior. Re-estimate delivery after results; no spike has run yet.
- [ ] T03: Test confirmed six-photo/10 MB limits at boundaries, actual byte counting, concurrent quota and retry/cleanup behavior, all four confirmed input formats including real HEIC fixtures, output metadata/orientation and task deletion during processing, plus no private postcode/contact in raw DTO/media; implement brief/photo pipeline and confirmed PDOK-based coarse region matching; validate source geometry/CRS, ambiguous postcodes and Groningen/Fryslân border cases without automatic provider fallback.
- [ ] T04: Test explicit opt-in, daily digest default/optional instant, frequency changes, opt-out/close/deletion before dispatch; implement verified-recipient alerts with shared delivery history and bounce/dedup handling.
- [ ] T05: Prove paid/unpaid firms cannot list rivals/counts; implement owner-only offer list and company-name/withdrawn markers with no withdrawn body/contacts. Enforce terminal withdrawal across edits, draft creation, payment replay and task reopening.
- [ ] T06: Validate Mollie merchant/profile/method/cost settings and a maintained server adapter. Test forged/repeated classic payment-ID notifications, later refund changes on the same ID, one-hour cache expiry/credential rotation, unknown-result recovery and close/edit/reopen races; verify one retained fee and refund reconciliation.
- [ ] T07: Test private defaults and responder callback audience; implement revoke/close, contact reset on reopening, stale-consent rejection and durable attempt invalidation across close/reopen.
- [ ] T08: Test month-end/DST, deadline access independent of worker, all object versions and restore; implement automatic purge.
- [ ] T09: Implement case/reason/appeal, staff permissions, fee reconciliation, one-business-day refund submission monitoring and overdue/queued/failed escalation, plus deletion health; no quality-verification badge.
- [ ] T10/T11: Implement install/help, offline privacy, Dutch/English/accessibility and safe bank-app return on real phones.
- [ ] T12: Run complete tests and timed restore/PSP reconciliation; verify no work data returns after expiry.
- [ ] T14: Recruit actual opt-in companies and real task demand, publish generic category/region pages; no scraped email blast.
- [ ] G2/T13: Approve bounded money/operations budget and refund reserve; run genuine paid beta and review weekly.
- [ ] G3: Decide iterate/stop/scale using actual contribution and useful responses, not website visits alone.
- [ ] G4/T15 later: Approve native-client needs/store payments/security/release plan separately.
- [ ] T16 later: Validate fee-amortization tools before adding prepaid/recurring accounting.

## 5. Critical transaction task example
For T06, use Mollie test mode with real hosted checkout and a controlled public HTTPS callback endpoint once implementation starts; localhost alone cannot receive provider callbacks. Separate test/live credentials and profile configuration. Include real notification delivery and authenticated retrieval, not only local HTTP fixtures. Record test-mode and controlled-live device evidence separately: simulated checkout alone cannot prove a real bank-app handoff. No package installation, public tunnel or merchant provisioning is performed by this document update. See [Mollie payment flow](https://docs.mollie.com/docs/accepting-payments) and [webhooks](https://docs.mollie.com/reference/webhooks).

For T06, first write a real-PostgreSQL integration test: same company/job receives simultaneous fresh-key payment/publication requests, a PSP callback repeats, and client close competes with fulfillment. Before implementation it must fail for missing behavior, not a broken environment.

Implement only the job->response->attempt locked use case. Assert one retained fulfilled fee; either visible valid response before close or tracked full refund if close wins, never a paid orphan or second successful listing. Repeat with lost HTTP response, changed material scope and late-paid callback. Run focused and full suites, inspect raw private DTOs, then commit the slice.

Proposed scripts such as npm run test:integration are implementation outputs of T01, not existing commands in this documents-only repo.

## 6. Budget and “zero cost”

Confirmed on 22 October: a **€100/month infrastructure planning budget, excluding VAT**. Cover Workers/Hyperdrive, paid Render PostgreSQL, Resend, private R2/Images, required backups, basic monitoring and necessary staging/test usage. Exclude engineering/operations labor, legal/accounting, domain registration, payment-processing fees/refund reserve, advertising and any future AI/model hosting. Select exact plans from measured load, current quotes and recovery/privacy requirements; this is not a capacity quote or a vendor-enforced spending cap. Track invoice currencies, metered usage and service limits/alerts; required deletion/refund/recovery work must remain funded. No service or spend has been created.

Illustrative build envelope, not a quote: 55 engineering days × €600 = €33,000; six product/design days × €600 = €3,600; eight QA days × €500 = €4,000; legal/accounting €2,500; pilot tools €500; initial refund reserve €100. Subtotal €43,700; 20% contingency = **€52,440**, excluding taxes, test-device hardware, ongoing staffing and optional advertising. Replace with actual staffing/vendor quotes; founder labor can reduce cash spending, not eliminate work or operating risk.

Zero paid advertising is the default initial growth plan. Any €100 Google Ads experiment is separate and needs explicit owner approval/monitoring; it is not automatically funded by this document.

## 7. Done means verified
Per feature: passing policy/real-DB/browser tests as relevant, safe DTO/consent/retention behavior, appropriate audit/telemetry, translated failure states and reviewed migrations/runbooks. No completion merely because a page renders. No paid beta before refund, abuse/privacy handling and deletion controls work.
