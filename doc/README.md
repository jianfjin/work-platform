# Work Platform — semi-public-benefit network for ZZP and SMEs
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** revised design; detailed entitlement/billing policies pending; documentation only

The founder revised the platform to a low-fee, semi-public-benefit service with three boards:

1. Clients publish home-maintenance projects and compare private rough offers.
2. Companies split projects into work packages and find cooperating companies.
3. Companies trade construction tools and materials.

Expand trades to roof repair/new roofing, renovation, insulation, windows, simple plumbing/electrical maintenance, installation and the existing maintenance categories. Free companies may view at most five information items per month; active subscribers receive information without a numerical monthly limit. The founder canceled the old €1 offer-publication fee: only free and low-price subscription services remain, with no separate per-offer charge or checkout. Subscription price/period and exact counting rules remain to be chosen.

Read the [Chinese v0.4 redesign](22-platform-redesign.md) for confirmed requirements versus recommendations. [ADR 0013](../docs/adr/0013-expand-to-three-boards-and-free-subscription-services.md) records the change; the [v0.3 archive](archive/v0.3/README.md) preserves the previously approved design. Its old commercial rules and implementation approval do not approve the new billing details or expanded delivery estimate.

## Preserved architecture and constraints
TypeScript/Hono and React Router/React/Vite/SSR on Cloudflare Workers; Render PostgreSQL Frankfurt through cache-disabled Hyperdrive; Drizzle/pg; Better Auth company/staff identity; Mollie only; Resend with dedicated subdomain; private EU R2/Images; PDOK; durable PostgreSQL/Cron jobs. Keep the €100/month ex-VAT infrastructure planning budget and GDPR/AVG requirements. Human work, domains, legal/accounting, payment fees/reserve, ads and future AI remain separate.

Client-task private offers/contact consent, confirmed-email guest management, three-calendar-month expiry, <=24h live purge and <=30-day isolated backups remain in force. Cooperation/trade privacy, geography, retention and settlement boundaries need explicit design. A subscription bypasses neither data ownership nor client consent. No application, cloud resources or deployment have been created, and no application tests have run.

## Documents
| Document | Purpose |
|---|---|
| [Brief](00-project-brief.md) / [research](01-research-and-positioning.md) | Mission, audience, scope and validation work |
| [Requirements](02-product-requirements.md) / [UX](03-ux-and-service-blueprint.md) | Current three-board requirements and flows |
| [Architecture](04-architecture.md) / [technical design](05-technical-design.md) | Selected stack, entitlements, jobs and billing correctness |
| [Data model](06-data-model.md) / [API contract](07-api-contract.md) | Typed information, plans, quota and scoped operations |
| [Services/economics](08-payments-and-economics.md) | Free/subscription policy and unresolved subscription price/renewal/refund decisions |
| [Security/privacy](09-security-privacy-and-trust.md) | GDPR, permissions, deletion and operating evidence |
| [Delivery](10-delivery-plan-and-todo.md) / [tests](11-test-and-launch-plan.md) | Expanded backlog, planned acceptance and gates |
| [Decisions](12-decisions-and-risks.md) / [sources](13-source-register.md) | Current decisions, risks and historical/new research |
| [PWA](14-cross-platform-and-pwa.md) / [operations](15-operations-and-growth.md) | Device/entitlement behavior and three-board operations |
| [Chinese revision summary](16-requirement-revision.md) / [implementation plan](17-implementation-plan.md) | Scope change and revised delivery sequence |
| [Cloudflare comparison](18-cloudflare-vs-render.md) / [backend options](19-backend-options.md) | Preserved stack research with historical product context |
| [Future AI](20-ai-service-and-knowledge-options.md) / [Resend review](21-cloud-mail-and-resend-review.md) | Future-scope assessment and verified existing email integration |
| [UI design](DESIGN.md) / [glossary](../CONTEXT.md) | Shared product design and vocabulary |
