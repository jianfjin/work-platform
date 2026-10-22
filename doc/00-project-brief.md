# Project brief
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** revised product direction; detailed commercial policies pending

## Mission and audience
A semi-public-benefit, low-fee information platform primarily serving Dutch ZZP and small/medium businesses. Help participants find maintenance work, cooperate on work packages and reuse/trade construction tools and materials. The mission is not a claim of registered charity, tax exemption, certification or guaranteed work.

Clients continue to publish maintenance requests free. Companies choose a free service with at most five information items viewed per month or a paid subscription with no numerical monthly information limit. The subscription price is not selected.

## Three boards
| Board | Publisher | Purpose |
|---|---|---|
| Client tasks | Client, including the existing confirmed-email guest flow | Publish a maintenance request and compare private rough offers |
| Cooperation | Company/ZZP | Split a project into work packages and find collaborating companies |
| Tools and materials | Company/ZZP | Publish tool/material trading information and contact a counterpart |

The boards share identity, plan entitlements, matching, media, notifications and operations. They have separate ownership, detail projections and lifecycle rules. A work-package relationship never automatically shares the original client's private data.

Expand trades to roof repair, new roofing, renovation, insulation, window/frame replacement, plumbing maintenance, electrical maintenance and installation, alongside existing carpentry, painting, plastering, flooring, tiling, facade and outdoor maintenance. See the [full bilingual category design](22-platform-redesign.md#2-工种目录).

## Decisions and boundaries
The founder's four new requirements and subsequent cancellation of the former €1 offer-publication fee are confirmed in [ADR 0013](../docs/adr/0013-expand-to-three-boards-and-free-subscription-services.md). Only free and low-price subscription services remain; there is no separate per-offer charge or checkout. The old unlimited-free-reading and deferred-subscription rules are superseded.

Quota counting, shared versus per-board allowance, monthly boundary, subscription amount/period/renewal/refunds and new-board retention need explicit policy. The proposed first design shares five unique detail accesses per company/month across boards, counts repeat access once, and keeps owner management free of quota. These are recommendations, not additional founder confirmations.

Direct agreements/payment between work or trading parties are the proposed initial boundary; platform orders, escrow, shipping and trade commissions are not selected. The client-task pilot remains the whole Groningen/Fryslân provinces, with company radii controlling matches; new-board geography must be specified before activation.

## Preserved constraints
Cloudflare/TypeScript/Hono, React Router SSR, Render PostgreSQL Frankfurt/Hyperdrive, Drizzle/pg, Better Auth, Mollie only, Resend, private EU R2/Images, PDOK and PostgreSQL/Cron jobs remain selected. GDPR/AVG, private contacts/offers, client-task expiry and deletion-safe recovery remain requirements. The €100/month ex-VAT infrastructure planning budget remains the sizing target; no expanded-scope cost or delivery date is proven.

The [v0.4 redesign](22-platform-redesign.md) and [implementation plan](17-implementation-plan.md) are current. The [v0.3 archive](archive/v0.3/README.md) preserves the previously approved design; its per-offer checkout implementation is not the current billing specification. No application has been implemented or deployed.
