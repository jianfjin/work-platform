# Work Platform — local home-maintenance marketplace
**Version:** 0.3 · **Updated:** 22 October 2026 · **Status:** founder-approved implementation plan with confirmed scope/stack/budget; not implemented

The founder approved the [implementation plan](17-implementation-plan.md) on 22 October; it consolidates the confirmed decisions into qualification, delivery slices and launch gates. The infrastructure planning budget is **€100/month excluding VAT**, covering selected services, required backups/basic monitoring and necessary staging/test usage; human work, domains, payment fees/refund reserve, advertising and future AI hosting are separate. Start with synthetic-data runtime qualification; GDPR evidence precedes real personal-data use.

## Current direction
**Clients pay no platform fee. Companies pay the platform.** The confirmed pilot charge is **€1.00 including VAT per company per task when its first rough offer is successfully published**, not to read a task and not a percentage of the construction contract. The founder confirmed the event and gross amount during the implementation-planning interview on 21 October 2026: reading and edits are free, a client reply is not guaranteed, and failed publication receives a full refund. See [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md).

Collection is confirmed as individual Mollie-hosted checkout for each €1 publication fee. Prepaid bundles, wallets and subscriptions are deferred until after MVP. Mollie is the sole current provider, explicitly selected on 22 October, with iDEAL/Wero and cards confirmed as the only MVP method targets. Each method requires merchant approval and successful €1 payment, full-refund and phone/PWA return tests before activation; live approval/testing remains launch work.

Confirmed incident refunds: a substantiated fake, duplicate or materially misrepresented task qualifies for a full €1 refund, with a seven-day reporting target from payment and review of credible later evidence. Each approval needs a recorded reason and necessary case evidence. No reply or losing the work alone remains non-refundable. Submit confirmed refunds to Mollie within a one-business-day target after verification or staff approval, automatically submit verified failed-publication/duplicate-capture cases, and escalate overdue/queued/failed obligations. Submission and bank arrival are separate. See [payment policy](08-payments-and-economics.md).

Confirmed task alerts: explicit company opt-in, one daily digest by default, optional instant emails, category/radius matching and easy unsubscribe. Frequency selection alone does not subscribe a company; both modes share delivery history and recheck eligibility before dispatch. See [operations](15-operations-and-growth.md).

The founder requests **Cloudflare app hosting** and has confirmed **paid Render PostgreSQL in Frankfurt**, connected through Hyperdrive with application query caching disabled. See [database ADR 0009](../docs/adr/0009-use-render-postgresql-in-frankfurt.md) and the [Cloudflare versus Render comparison](18-cloudflare-vs-render.md). PostgreSQL-backed jobs with Cloudflare Cron are confirmed, initially using a one-minute recovery poll and optional immediate attempts after durable commit. Request/scheduled Workers handlers share application modules; private Cloudflare R2 photo storage with explicit EU jurisdiction and authorized Worker reads is confirmed. Framework integration, database plan/version and exact job/media configuration remain open; Render app hosting is the comparison baseline. No deployment has run.

**TypeScript with Hono on Cloudflare Workers is explicitly confirmed for the backend**, following the language comparison and subsequent HTTP-framework decision. Expected runtime overhead and shared tooling inform the language choice; throughput/latency remain unmeasured. See [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md), [architecture](04-architecture.md) and [the backend assessment](19-backend-options.md). **React Router framework mode with React, Vite and SSR is confirmed for the Web/PWA**; see [ADR 0010](../docs/adr/0010-use-react-router-for-web-pwa.md). **Drizzle ORM with node-postgres (pg) is confirmed for typed database access and reviewed SQL migrations**, retaining explicit locks/constraints and runtime qualification. **Better Auth is confirmed for company/staff accounts**, with records in Render PostgreSQL; see [ADR 0011](../docs/adr/0011-use-better-auth-for-company-and-staff-accounts.md). Company sign-in is confirmed as **verified email and password**, with email verification and password recovery. Staff access is confirmed as **invite-only email/password with mandatory authenticator-app TOTP and one-use recovery codes**; password recovery preserves MFA. Exact package versions, operating provisioning/recovery procedures and runtime qualification remain open; no build has run.

The founder confirmed the pilot covers **the whole provinces of Groningen and Friesland/Fryslân, explicitly including Drachten**. Company-selected service radii control matching within this area; they do not expand task-publication eligibility outside the two provinces. Validate the postcode/municipality mapping against that scope before launch. Support Dutch and English, desktop Web, Android PWA and iPhone PWA. Dedicated mobile apps come after maturity and a separate approval.

Clients prepare a short description, postcode/city and up to six optional photos (maximum 10 MB each) without creating an account. Photo inputs are confirmed as JPEG, PNG, WebP and iPhone HEIC, converted to sanitized JPEG/WebP for display; Cloudflare Images through its Workers binding is confirmed for upload-time conversion, with runtime/privacy qualification still required. The founder confirmed that tasks remain unpublished until email confirmation; short-lived links provide task-scoped management and fresh-link recovery. Optional client accounts, multi-task account dashboards and task linking are deferred until after MVP; company and staff accounts remain in scope. See [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md). Companies register, declare their services/radius and submit private rough offers with website/contact details. The client sees all submitted offers; companies cannot see competitors or competitor counts. There is **no three-company commercial cap**.

Confirmed contact model: client details stay private by default and the client initiates contact using company details. An explicit choice shares selected client email/phone fields with every published responder while the task is open, with no fixed audience cap. Revoking permission or closing stops future disclosure; copied details cannot be recalled. See [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md). Site visits, final prices, contracts and construction payments are arranged directly.

Confirmed material-edit model: changes to requested work create a new scope revision. Captured payments for unpublished offers on the old revision are refunded; published offers stay visible with a scope-change label and free edits, without automatic refund or another fee. See [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md).

Confirmed reopening model: a voluntarily closed task may reopen before its original expiry, preserving existing offers and fee history without another fee for previously published companies. Client contact resets to private until callbacks are explicitly enabled again; canceled payment attempts stay invalid. See [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).

Confirmed offer withdrawal: final for that company/task in MVP. The client sees the company name and a withdrawn marker, without offer text or company contacts; callback access ends, the original fee remains without automatic refund, and no republishing or new checkout is permitted. Restoration is deferred; see the same [lifecycle ADR](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).

The service is an **information intermediary**, not a workmanship certifier, contractor or construction-payment processor. No routine quality/credential checking or construction monitoring is promised. Email ownership, content security, platform-fee accounting, privacy, abuse reports and applicable legal duties still need operating controls.

**PDOK Location API is confirmed for Dutch location lookup**, using minimum server-side postcode/city queries and approximate radius matching in our application/database. API/dataset terms, CRS, province accuracy and privacy qualification remain work; Google remains unselected comparison context.

**GDPR/AVG compliance is an explicit requirement throughout this project (NFR-09).** The [privacy plan](09-security-privacy-and-trust.md#8-compliance-launch-checklist) defines evidence for minimization/access, purposes and notices, individual rights, retention/recovery, vendor contracts/international transfers and breach response before real personal-data use. EU storage alone does not establish compliance; Resend US account data and Cloudflare global processing remain explicit review boundaries.

## Existing email integration review

The founder requested inspection of the existing cloud-mail Worker and Resend integration. Authenticated read-only checks confirm a configured Resend path and verified Ireland sending for datawego.nl. The founder has now selected **Resend** for a direct work-platform adapter with application-specific credentials, a durable PostgreSQL outbox and verified delivery events; see [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md). A dedicated sending subdomain is confirmed; the parent domain, subdomain label, sender/Reply-To addresses, plan and integration remain open; the existing domain is not selected for this platform, and Resend account-data storage in the US remains an explicit processor-review boundary. No cloud-mail code/deployment was changed or email sent.

## Payment decision — 22 October 2026
**Mollie-hosted checkout through the Payments API is the sole integration for the current stage**, with no alternative/fallback evaluation. Clients remain free; companies pay only the confirmed platform-publication fee, with no construction funds or contractor payouts. The design uses classic payment-ID notifications, authenticated retrieval, durable fulfillment/refund uniqueness and PWA recovery beyond the one-hour credential-scoped idempotency cache. Economics now use Mollie's public pricing. See [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md), [payments](08-payments-and-economics.md) and [technical mapping](05-technical-design.md). No account, credentials, package or live payment has been provisioned.

## Read the documents
| Document | Purpose |
|---|---|
| [Project brief](00-project-brief.md) | Confirmed requirements, assumptions and scope |
| [Research and positioning](01-research-and-positioning.md) | Competitors and notary-comparison reference |
| [PRD](02-product-requirements.md) | Authoritative product rules and acceptance requirements |
| [UX/service blueprint](03-ux-and-service-blueprint.md) | Guest posting, private offer list and contact choices |
| [DESIGN.md](DESIGN.md) | Original agent-readable visual system |
| [Architecture](04-architecture.md) | Shared Web/PWA backend and external services |
| [Technical design](05-technical-design.md) | Guest security, pay-to-publish races, matching, alerts and deletion |
| [Data model](06-data-model.md) | Entities, constraints and retention boundaries |
| [API](07-api-contract.md) | Guest/company/operator routes and DTO authorization |
| [Payments/economics](08-payments-and-economics.md) | Company versus client funding, merchant ownership and viability |
| [Privacy/security/responsibility](09-security-privacy-and-trust.md) | Contact permission, three-month deletion and abuse handling |
| [Delivery/to-do](10-delivery-plan-and-todo.md) | Dependency-aware backlog and approval gates |
| [Tests/launch](11-test-and-launch-plan.md) | Traceable tests and real-device/recovery evidence |
| [Decisions/risks](12-decisions-and-risks.md) | What is confirmed and what still needs approval |
| [Sources](13-source-register.md) | First-party evidence and research limits |
| [Cross-platform](14-cross-platform-and-pwa.md) | PWA now, dedicated apps later |
| [Operations/growth](15-operations-and-growth.md) | Staff work, opt-in alerts, low-cash-cost launch and Google Ads |
| [Revision summary](16-requirement-revision.md) | Chinese change summary and migration from version 0.2 |
| [Implementation plan](17-implementation-plan.md) | Confirmed scope/stack/budget, first qualification milestone, staged delivery and remaining launch prerequisites |
| [Domain glossary](../CONTEXT.md) | Agreed marketplace terminology |
| [Publication-fee ADR](../docs/adr/0001-charge-for-first-offer-publication.md) | Accepted charge event and its rationale |
| [Guest-ownership ADR](../docs/adr/0002-confirm-email-for-guest-task-ownership.md) | Email confirmation before publication and account-free task management |
| [Contact-permission ADR](../docs/adr/0003-client-controlled-responder-contact.md) | Private defaults and explicit selected-field access for all published responders |
| [Task-expiry ADR](../docs/adr/0004-expire-tasks-from-first-publication.md) | Immutable three-calendar-month clock from first task publication |
| [Material-edit ADR](../docs/adr/0005-version-material-task-scope-changes.md) | Revision changes, unpublished-payment refunds and scope labels on existing offers |
| [Reopening ADR](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md) | Preserve offers/fees/expiry, require fresh callback permission and keep canceled attempts invalid |
| [Mollie provider ADR](../docs/adr/0007-use-mollie-for-current-payment-integration.md) | Sole current provider, classic notifications and provider-specific recovery |
| [Cloudflare versus Render](18-cloudflare-vs-render.md) | Requested app-hosting direction, runtime/job changes, database options and indicative costs |
| [Backend options](19-backend-options.md) | FastAPI, Go, Rust/Tokio and TypeScript; Workers versus native containers; maintainer and qualification criteria |
| [AI service and knowledge options](20-ai-service-and-knowledge-options.md) | Future support/Wiki assessment: TypeScript integration, optional Python model work, Jev versus open weights and existing refund authority |
| [TypeScript backend ADR](../docs/adr/0008-use-typescript-for-cloudflare-backend.md) | Delegated TypeScript/Workers selection, expected runtime overhead and required performance validation |
| [Render database ADR](../docs/adr/0009-use-render-postgresql-in-frankfurt.md) | Confirmed paid PostgreSQL in Frankfurt, Hyperdrive connection boundary and remaining configuration/recovery work |
| [Frontend ADR](../docs/adr/0010-use-react-router-for-web-pwa.md) | Confirmed React Router with React/Vite/SSR, Hono API boundary and remaining build qualification |
| [Authentication ADR](../docs/adr/0011-use-better-auth-for-company-and-staff-accounts.md) | Confirmed Better Auth with PostgreSQL identity records; separate guest access and remaining login/MFA qualification |
| [Existing cloud-mail/Resend review](21-cloud-mail-and-resend-review.md) | Read-only production/source evidence, selected integration direction, implementation gaps and sending-region/storage distinction |
| [Resend provider ADR](../docs/adr/0012-use-resend-for-platform-email.md) | Confirmed direct Resend sending, separate application credentials and remaining sender/integration review |

## Data lifecycle
The founder confirmed that task content, photos, offers, task contact data and guest grants expire **three calendar months after first task publication**, when the task first becomes visible to companies. Closing, editing, registration or a later offer does not extend this deadline; paid offer publication does not start it. Access stops immediately at the deadline, live task data and all photo/object versions are purged within 24 hours, and isolated backups age out within 30 days; restores reapply deletions before serving users. These confirmed timings must be disclosed and tested; see [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).

The founder confirmed narrow separate retention for minimum platform-fee/invoice records, active company/alert settings and strictly necessary case evidence. Evidence holds need a named case, restricted access and a review/deletion date; there is no blanket expired-task archive. Exact purposes, fields and periods still receive accountant/legal review before launch. See the [retention ADR](../docs/adr/0004-expire-tasks-from-first-publication.md) and privacy document before publishing retention wording.

## What version 0.3 replaces
The former Utrecht/carpentry-only, verified-business, paid-thread-reading, three-firm cap, compulsory €10 wallet, in-app chat/award and 12-month content-retention design is superseded. History remains in Git, not as competing active requirements. No application, cloud resource, payment account or advertising campaign has been created. All engineering estimates, policy proposals and commercial assumptions remain subject to their stated gates.
