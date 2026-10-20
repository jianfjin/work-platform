# HoutMatch — carpenter marketplace design pack

**Repository:** work-platform · **Working name:** HoutMatch (not trademark/domain cleared)
**Version:** 0.2 · **Date:** 20 October 2026 · **Status:** proposal for review, not an implemented product

**Confirmed platform direction:** desktop Web plus Android/iPhone progressive web application (PWA) now; dedicated mobile applications after product maturity and a separate release decision. This confirms the delivery approach, not the remaining business/legal launch gates.

## The proposal in one minute

A Netherlands-first platform where clients post carpentry jobs and verified carpentry businesses pay **€1 including VAT, once per job thread**, to read the full brief and respond. A free, privacy-safe preview supports the purchase decision. A payment never buys another business's messages, a client's phone number, or a guaranteed job.

The proposed advantage is a **quote-ready carpentry brief + shared scope clarification + private comparable quotes + auditable access/refund history**. Limited competition and verification are important but are not novel on their own; see the competitor evidence.

€1 is a **pilot hypothesis**, not a validated sustainable price. With three paying firms per thread, revenue is tightly capped. Do not launch paid acquisition or promise this price indefinitely before the economics gate passes.

## Read in this order

| Document | Purpose |
|---|---|
| [Project brief](00-project-brief.md) | Vision, assumptions, alternatives, boundaries |
| [Research and positioning](01-research-and-positioning.md) | Observed competitors versus hypotheses and experiments |
| [Product requirements](02-product-requirements.md) | Personas, scope, requirements and acceptance criteria |
| [UX and service blueprint](03-ux-and-service-blueprint.md) | Journeys, screens, wireframes, operational handoffs |
| [DESIGN.md](DESIGN.md) | Agent-readable original visual system and interaction rules |
| [Architecture](04-architecture.md) | Context, containers, modules, deployment and trade-offs |
| [Technical design](05-technical-design.md) | Transactions, authorization, background processing, failure handling |
| [Data model](06-data-model.md) | Entities, constraints, indexes, retention boundaries |
| [API contract](07-api-contract.md) | Endpoints, DTOs, idempotency and error semantics |
| [Payments and economics](08-payments-and-economics.md) | €1 rules, top-ups, refunds, ledger and viability model |
| [Security, privacy and trust](09-security-privacy-and-trust.md) | Threats, access matrix, compliance gates and moderation |
| [Delivery plan and to-do list](10-delivery-plan-and-todo.md) | Prioritized, owned, dependency-aware implementation backlog |
| [Test and launch plan](11-test-and-launch-plan.md) | Requirement traceability, adversarial cases, release gates |
| [Decisions and risks](12-decisions-and-risks.md) | ADRs, open decisions, owners and stop conditions |
| [Source register](13-source-register.md) | Primary references, access date and research limitations |
| [Cross-platform and PWA delivery](14-cross-platform-and-pwa.md) | Desktop/Android/iPhone support, installation, offline/privacy boundaries and later native-app gate |

## Document contract

- This delivery is **documentation only**. No application, cloud resources, paid accounts or production configuration have been created.
- English is the working document language; Dutch is the default proposed UI language, with English supported from the pilot.
- Requirements using **MUST/SHALL** are proposed implementation contracts, not claims of existing capability.
- Policies in [Product requirements](02-product-requirements.md) and [Payments](08-payments-and-economics.md) are authoritative. Technical documents implement them; resolve any conflict before development.
- All dates, costs, volumes and schedules outside cited observations are estimates or explicit assumptions. Weeks start after scope approval and team availability, not on this document's date.
- The requested reference collection informs the **format and depth** of DESIGN.md. This is an original carpentry identity, not a copy of a competitor or reference brand.
- Approving or publishing these documents does not approve their unresolved legal, pricing or infrastructure decisions. Gate G0 in the backlog must be signed off first.

## Proposed first release

One responsive Web/PWA codebase supporting desktop browsers, Android browser/home-screen use and iPhone Safari/home-screen use; one local pilot market; client jobs; verified single-owner carpentry businesses; free previews; paid unlocks; private conversations; shared non-commercial Q&A; structured quotes; client selection; job-linked reviews; operations dashboard; refunds and reconciliation. Installation is optional; core flows work in the browser. Private job data and financial actions require connectivity; an installed icon is not a promise of offline functionality.

No construction payments, escrow, commissions on building work, employment matching, native desktop/mobile binaries, generative-AI dependencies or nationwide rollout in the MVP. Dedicated Android/iPhone apps remain a later phase rather than a rejected platform requirement.
