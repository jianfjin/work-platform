# Work Platform — local home-maintenance marketplace
**Version:** 0.3 · **Updated:** 21 October 2026 · **Status:** documentation proposal, not implemented

## Current direction
**Clients pay no platform fee. Companies pay the platform.** The recommended pilot charge is **€1.00 including VAT per company per task when its first rough offer is successfully published**, not to read a task and not a percentage of the construction contract. This charge event is a proposed interpretation requiring founder approval; the payer direction is explicit user instruction.

Start in **Groningen and Friesland/Fryslân, explicitly including Drachten**; final province-versus-city boundaries must be confirmed. Support Dutch and English, desktop Web, Android PWA and iPhone PWA. Dedicated mobile apps come after maturity and a separate approval.

Clients post a short description, postcode/city and a few optional photos without creating an account. An email-confirmed job-management link provides ownership/security; registration remains optional. Companies register, declare their services/radius and submit private rough offers with website/contact details. The client sees all submitted offers; companies cannot see competitors or competitor counts. There is **no three-company commercial cap**.

Default: the client initiates contact using company details. A separate explicit choice lets responding companies see selected client contact fields. Site visits, final prices, contracts and construction payments are arranged directly.

The service is an **information intermediary**, not a workmanship certifier, contractor or construction-payment processor. No routine quality/credential checking or construction monitoring is promised. Email ownership, content security, platform-fee accounting, privacy, abuse reports and applicable legal duties still need operating controls.

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

## Data lifecycle
Task content, photos, offers, task contact data and guest grants expire **three calendar months after first publication**. Closing, editing, registration or a later offer does not extend this deadline. Access stops at the deadline; the proposed live-store purge target is within 24 hours and rolling backups age out within 30 days. These implementation timings must be disclosed and tested.

Three months is not a promise to erase every financial record: minimum platform-fee accounting records, narrowly justified legal evidence and active company/account/alert preferences have separate lawful purposes and schedules. See the privacy document before publishing retention wording.

## What version 0.3 replaces
The former Utrecht/carpentry-only, verified-business, paid-thread-reading, three-firm cap, compulsory €10 wallet, in-app chat/award and 12-month content-retention design is superseded. History remains in Git, not as competing active requirements. No application, cloud resource, payment account or advertising campaign has been created. All engineering estimates, policy proposals and commercial assumptions remain subject to their stated gates.
