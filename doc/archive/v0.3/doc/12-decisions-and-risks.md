# Decisions and risk register
**Version:** 0.3 · **Updated:** 22 October 2026

## 1. Confirmed direction
| Decision | Source / effect |
|---|---|
| Implementation plan | Founder approved the consolidated plan on 22 October after confirming scope, stack and infrastructure budget; synthetic-data runtime qualification is the first implementation milestone; implementation and launch evidence remain pending; [plan](17-implementation-plan.md) |
| Client platform fee zero; companies pay | User's explicit latest instruction overrides contradictory client-€1 examples |
| €1 including VAT for first private rough-offer publication per company/task | Confirmed during planning interview on 21 October 2026: free reading/edits, no guaranteed reply, full refund if publication fails; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md) |
| Individual hosted collection | Confirmed: separate Mollie-hosted checkout per €1 publication fee; prepaid bundles, wallets and subscriptions deferred until after MVP; live merchant/method activation separate; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md) |
| Payment provider | Explicitly selected on 22 October 2026: Mollie-hosted checkout and Payments API only for the current stage; no current alternative/fallback evaluation; [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md) |
| MVP payment methods | Confirmed on 22 October 2026: iDEAL/Wero and cards only; each needs merchant approval and successful €1 payment, full-refund and phone/PWA return tests before activation; other methods excluded; [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md) |
| Incident refunds and reporting | Confirmed on 22 October 2026: full €1 refund for substantiated fake, duplicate or materially misrepresented tasks; seven-day reporting target from payment with credible later evidence reviewed; recorded approval/reason and minimum case evidence; no reply/lost work alone does not qualify; [payment policy](08-payments-and-economics.md) |
| Refund submission target | Confirmed on 22 October 2026: submit to Mollie within one business day of verification or staff approval; automatic submission for verified failed-publication/duplicate-capture cases; overdue/queued/failed escalation; submission is separate from provider completion and bank arrival; [payment policy](08-payments-and-economics.md) |
| Material scope changes | Confirmed: new revision; captured payments for unpublished old-scope offers refunded; published offers stay visible with scope-change labels and free edits, without automatic refund or extra fee; [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md) |
| Voluntary reopening | Confirmed: before original expiry, retain offers/fees without re-charge, contacts private pending fresh opt-in, canceled attempts stay invalid; client cannot clear a safety hold; [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md) |
| Published-offer withdrawal | Confirmed: final per company/task in MVP; client company-name/withdrawn marker only, no offer body/company contacts/callback access; original fee retained without automatic refund, no restoration/republishing/new checkout; [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md) |
| Northern pilot, Dutch/English | Whole Groningen and Friesland/Fryslân provinces, including Drachten, confirmed during planning on 21 October 2026; company-selected radii control matches within that area |
| Lightweight guest posting | Confirmed on 21 October 2026: email before publication, short-lived task-scoped access and fresh-link recovery without registration; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md) |
| Client-account scope | Confirmed: optional client accounts, multi-task account dashboards and task linking deferred until after MVP; company/staff accounts remain in scope; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md) |
| Private rough offers and direct contact | Client sees all eligible responders; companies see only themselves; no commercial three-firm cap |
| Client contact permission | Confirmed on 21 October 2026: private by default; explicit selected email/phone access for every published responder while open; revoke/close stops future disclosure, copied details cannot be recalled; [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md) |
| Information intermediary | No routine workmanship certification/site monitoring/construction payment handling |
| Three-calendar-month task expiry and erasure | Confirmed on 21 October 2026: immutable from first task publication to companies, no extension by later activity or offer publication; immediate access denial, all live task/photo versions purged within 24h, isolated backups within 30 days, deletion-safe restores; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md) |
| Narrow separate retention | Confirmed: minimum billing records, active company/alert settings and strictly necessary case evidence; holds need named case, restricted access and review/deletion date; exact purposes/fields/schedules need professional review; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md) |
| Web/PWA now, dedicated apps later | Earlier user-confirmed cross-platform direction retained |
| Task-alert frequency | Confirmed on 22 October 2026: explicit opt-in, one daily digest by default, optional instant emails, category/radius matching and easy unsubscribe; [operations](15-operations-and-growth.md) |
| App-hosting direction | Founder requests Cloudflare on 22 October 2026; compare against the earlier Render proposal and qualify the actual runtime; no services provisioned; [comparison](18-cloudflare-vs-render.md) |
| Backend language/execution | TypeScript on Cloudflare Workers selected on 22 October under the delegated choice, then explicitly confirmed by the founder after discussing possible AI integrations; expected runtime overhead/shared tooling, no measured performance claim; [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md) |
| Backend HTTP framework | Hono confirmed on 22 October for the TypeScript/Workers HTTP API; application/domain services remain shared, with exact version and integration still to qualify |
| Frontend framework | React Router framework mode with React/Vite/SSR confirmed on 22 October for the Web/PWA; [ADR 0010](../docs/adr/0010-use-react-router-for-web-pwa.md) |
| Company/staff authentication library | Better Auth with account/session records in Render PostgreSQL confirmed on 22 October; guest task-scoped access remains separate; [ADR 0011](../docs/adr/0011-use-better-auth-for-company-and-staff-accounts.md) |
| Company sign-in | Verified email plus password confirmed on 22 October, with email verification and password recovery |
| Email provider | Resend confirmed on 22 October for direct platform sending with application-specific credentials and a dedicated sending subdomain; exact domain/address and qualification remain open; [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md) |
| Staff sign-in/MFA | Invite-only email/password, mandatory authenticator-app TOTP and one-use recovery codes confirmed on 22 October; password recovery preserves MFA; implementation and operating recovery procedures still to qualify |
| Geocoding provider | Confirmed on 22 October: PDOK Location API for minimum server-side Dutch location lookup, with approximate radius matching in our application/database; exact API/dataset/terms/CRS and GDPR qualification remain work |
| Pilot infrastructure budget | Confirmed on 22 October: €100/month excluding VAT as the infrastructure planning budget for selected services plus required backups/basic monitoring/staging; human work, domains, payment fees/reserve, ads and future AI are separate; actual sizing/costs still need qualification |
| Database-access tooling | Confirmed on 22 October: Drizzle ORM with node-postgres (pg), typed schema/queries and reviewed SQL migrations; explicit SQL locks/constraints and Workers/Hyperdrive/Better Auth qualification retained; no dependency installed |
| Background-job execution | Confirmed on 22 October: Durable PostgreSQL inbox/outbox/jobs plus bounded Cloudflare Cron handlers, initially one-minute recovery polling and optional bounded immediate attempts after durable commit; shared leases/idempotency, backlog monitoring and qualification remain work; Queues unselected |
| Image processor | Confirmed on 22 October: Cloudflare Images through its Workers binding for upload-time conversion, with sanitized JPEG/WebP variants in private EU R2; metadata/runtime/budget and NFR-09 qualification remain work |
| Photo formats | Confirmed on 22 October: JPEG/PNG/WebP and iPhone HEIC inputs converted to sanitized JPEG/WebP for display; exact decoder/metadata/orientation behavior and processor qualification remain work |
| Photo upload limits | Confirmed on 22 October: Optional photos, maximum six per task and 10 MB per file; server-side count/byte enforcement includes concurrent in-flight uploads; format and decoder qualification remain open |
| Photo storage/access | Confirmed on 22 October: Private Cloudflare R2 with explicit EU jurisdiction, disabled public bucket access and authorized Worker reads enforcing current permissions/expiry; image processing and deletion qualification remain work |
| GDPR/AVG requirement | Explicitly reaffirmed on 22 October for the whole project; NFR-09 requires ongoing technical and operating evidence, with required safeguards before real personal-data processing; [privacy requirements](09-security-privacy-and-trust.md#8-compliance-launch-checklist) |
| Database provider/region | Confirmed on 22 October: paid Render PostgreSQL in Frankfurt, accessed from Cloudflare through Hyperdrive; exact plan/version, connection and recovery qualification remain open; [ADR 0009](../docs/adr/0009-use-render-postgresql-in-frankfurt.md) |

## 2. Remaining decisions and qualification work
| Topic | Required decision or evidence | Owner / gate |
|---|---|---|
| Name | Work Platform working label; no final brand selected | Founder, before public branding |
| Stack qualification | TypeScript/Hono and React Router framework mode with React/Vite/SSR confirmed on Workers; qualify exact versions, validation/auth/SQL packages, durable jobs, media and NFR-03 load behavior | Tech, foundation/before beta |
| Staff provisioning and exceptional recovery | Sign-in/MFA policy settled; document initial privileged-user provisioning and loss-of-all-factors recovery, preserving role separation and avoiding an email-only MFA bypass | Founder/ops/tech, before beta |
| Email sender and readiness | Resend and a dedicated sending subdomain selected; choose parent domain, subdomain label, sender/Reply-To addresses and plan, qualify durable outbox/idempotency/signed events, suppression and US account-data/retention boundary; existing datawego.nl verification does not select the platform brand/domain | Founder/tech, foundation/before activation |
| Database and runtime configuration | Render PostgreSQL Frankfurt confirmed; select compute/storage/version and recovery-qualified budget, qualify Hyperdrive with query caching disabled and Render external TLS/IP controls; auth-method/media settings separate | Founder/tech, before provisioning |
| GDPR readiness | Complete purpose/data-flow records, notices/rights workflow, provider-role/contracts/subprocessor/transfer review including Resend US account data, DPIA/DPO screening and breach readiness; EU storage alone is insufficient | Founder/privacy/legal/tech, before real personal-data use |
| Job qualification | Execution strategy settled; qualify immediate/Cron claim races, interruption/lease recovery, stale-owner completion, retry/backoff, priority/fairness, backlog age and purge/refund capacity within runtime budgets | Tech, foundation/before beta |
| Database tooling qualification | Choice settled; pin compatible Drizzle/pg/Better Auth versions, qualify transaction/adapter atomicity, pooling/cleanup and reviewed migration apply/upgrade behavior through the actual Workers/Hyperdrive path | Tech, foundation |
| Infrastructure sizing and costs | €100/month ex-VAT planning budget settled; qualify actual service/workspace plans, storage/egress/usage, staging, recovery/privacy controls and billing alerts/limits; track invoice currency and metered charges | Tech/founder, foundation/before provisioning |
| Media qualification | R2 EU/private access, six-photo/10 MB limits and JPEG/PNG/WebP/iPhone HEIC inputs with sanitized JPEG/WebP output settled; Cloudflare Images Workers binding selected for upload-time processing; qualify metadata/orientation, safe variants, decoder/pixel limits, quarantine/late-output erasure, budget and NFR-09 processing/retention/transfer controls | Founder/tech/privacy, foundation/before live uploads |
| Tax classification | Event, €1 VAT-inclusive price, collection and refund eligibility/reporting/submission target settled; resolve VAT treatment, invoicing, correction and exact financial-retention policy | Founder/accountant, before paid beta |
| Mollie live readiness | Provider and iDEAL/Wero-plus-cards scope settled; validate legal merchant/profile, eligible target-method allowlist, actual fees, reserve, credentials and per-method €1/full-refund/device evidence | Founder/tech/finance, before paid beta |
| Exact separate retention schedules | Exception boundary confirmed; classify purposes, minimum fields, lawful bases, retention periods and start dates, with no blanket task archive | Privacy/legal/finance, before launch |
| Merchant/bank | Actual platform legal operator, approved business settlement account; no assistant/personal shared merchant | Founder/accountant/PSP, before money |
| Geocoder qualification | PDOK selected; validate current API/collections, coarse-coordinate and province mapping, CRS, terms/attribution/limits and minimum-query/privacy behavior; Google remains unselected comparison context | Tech/privacy, foundation/before live location processing |
| Native maturity | Stable economics/usage plus demonstrated needs; separate G4 design/budget | Founder/product, later |
| Possible AI support and knowledge/Wiki | Future-scope assessment only: retain TypeScript integration, optional Python model work; define models, corpus, automation authority, hosting and budget before adding scope; [assessment](20-ai-service-and-knowledge-options.md) | Founder/product/tech, separate scope decision |

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
| Incorrect region/radius match or geocoder data misuse | Validate PDOK source metadata/CRS, postcode/province accuracy, terms/limits and minimal queries; unresolved matches remain drafts | Tech/privacy |
| Alert promotion becomes spam | Affirmative subscription, purpose/frequency controls, unsubscribe/bounce/dispatch checks | Mail operations/legal |
| “Only intermediary” treated as legal immunity | Role-specific legal assessment and reviewed terms | Founder/legal |
| PWA bank return/update retries payment | Owned attempt recovery, no auto replay, real-device tests | Tech/QA |
| Google Ads spends beyond simple daily multiplication | Understand average-vs-cap, approval/monitoring/pause and actual margin-based CAC | Founder/growth |

## 5. Change control and current status
This revision replaces incompatible 0.2 assumptions; Git preserves historical documents. The repository is documentation only: no user/job/PSP data exists to migrate here. Any separately started implementation needs an inventory and an explicit migration plan, not blind deletion or repurposing of its payment records.

Confirmed rules include GDPR/AVG throughout delivery, private EU-jurisdiction R2 photo storage, fee payer/event/€1 price, individual hosted collection, deferred bundles/wallets/subscriptions, whole Groningen/Fryslân province scope, guest-only MVP client management, deferred client accounts/linking, contact/lifecycle/retention boundaries and PWA direction. Mollie is the sole provider selected on 22 October 2026, with iDEAL/Wero and cards confirmed as the only MVP method targets; live merchant/profile/method activation remains launch work. Region mapping and technical timezone/DST handling need validation; exact separate retention schedules require professional review. Detailed refund/tax policy, merchant details and budgets remain open. No customer interviews, profitability, legal clearance, application tests or deployment completion is claimed.
