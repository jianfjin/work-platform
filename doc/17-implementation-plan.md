# Implementation plan — revised platform scope
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** revised plan for confirmed new direction; detailed commercial/lifecycle policies pending

## 1. Product baseline and change control
Deliver a semi-public-benefit low-fee platform for ZZP/SMEs with three boards: client maintenance tasks, company cooperation/work packages, and building-tool/material trade. Expand trades as specified in [the redesign](22-platform-redesign.md). Free companies may view at most five information items monthly; active subscribers receive information without a numerical monthly cap. Client posting remains free.

This changes the scope and commercial baseline of the [previously approved v0.3 plan](archive/v0.3/doc/17-implementation-plan.md). The founder explicitly canceled the legacy €1 offer-publication fee, leaving only free and low-price subscription services; authorized offer submission, publication and editing require no separate charge or checkout. Exact quota scope/unit/period/transition, subscription price/renewal/refund, and new-board audience/lifecycle/settlement are not silently approved by this plan. Implement synthetic policies for qualification, not unapproved live billing.

## 2. Preserved architecture, budget and privacy
Keep the modular monolith: Workers TypeScript/Hono and React Router/React/Vite/SSR; Render PostgreSQL Frankfurt/Hyperdrive cache disabled; Drizzle/pg with reviewed SQL; Better Auth company/staff policies and separate guest authority; Mollie only; Resend dedicated subdomain; private EU R2/Images; minimum PDOK queries; durable PostgreSQL jobs with bounded Cron recovery initially every minute and optional post-commit immediate attempts.

Retain the €100/month ex-VAT infrastructure planning budget for selected services/backups/basic monitoring/staging. Human work, domains, legal/accounting, payment fees/reserve, ads and future AI are separate. Exact sizing and expanded-scope capacity remain qualification evidence.

GDPR/AVG precedes real personal-data use on every board. Preserve original task guest/privacy/contact/expiry rules; cooperation never inherits parent-client access. New-board retention and trading boundaries must be explicit. Resend US records and Images processing/temporary copies remain review items; semi-public-benefit positioning or EU primary storage does not establish compliance.

## 3. Delivery sequence
| Slice | Deliverable | Completion evidence / dependencies |
|---|---|---|
| 1. Policy and contracts | Fix live allowance/subscription terms and board lifecycle/settlement decisions; baseline v0.4 data/API/acceptance contracts | Distinguish confirmed requirements from open choices; canceled per-offer fee remains excluded |
| 2. Runtime foundation | Selected framework/DB/auth/jobs/media/provider qualification with synthetic data; shared typed catalog and board ownership | Deployed-runtime build, real transactions/rollback/claims, private sanitized HEIC and no cross-board/parent leak; exact versions/costs documented |
| 3. Membership and allowance | Current plan entitlements, atomic/idempotent free quota and Mollie billing adapter | Fifth/sixth mixed-board races, repeat/device/month/transition rules; authenticated period effects, unseen renewal-ID recovery and no duplicate charges; real billing waits for terms/merchant gate |
| 4. Client tasks | Expanded trades and guest task publication, safe detail access, private offers/contacts/lifecycle | Original privacy/expiry behavior preserved under the new entitlement gate; no separate offer-publication charge or checkout |
| 5. Cooperation | Company work packages, independent ownership/reference, matching and private interest | No source-client data/grant inheritance; agreed lifecycle and privacy-safe DTOs |
| 6. Tools/materials | Typed trade listings, safe item/condition/quantity/price/area, inquiry and availability | Agreed audience/settlement/lifecycle, pickup privacy and report handling; no implicit escrow/cart/shipping |
| 7. Unified operations | Board-aware alerts, quota/member support, rights/cases, billing exceptions and erasure/recovery | Opt-in and no email quota bypass, scoped human roles, reviewed retention and deletion-safe financial restore |
| 8. Qualification and beta | Full Web/PWA/NL/EN/accessibility, real phones, load, failure and recovery evidence | Applicable AT-01–AT-36 and NFRs, selected price/method/renewal evidence, named operations/funding and actual useful participants |

All three boards are in the requested scope. Order the slices to reuse foundations; do not quietly drop cooperation/trade as optional future features. Privacy, accessibility, minimal audit and relevant tests accompany each slice rather than starting only at slice 7.

## 4. First implementation milestone
Use synthetic data and explicit test policy to qualify a company seeing previews and requesting a detail grant through Workers/Hyperdrive/Drizzle. Exercise a shared fifth/sixth race across board types, replay, invalid target and a plan change. Prove guest/staff authority isolation, current consent/expiry, durable job claim recovery and one HEIC-to-private-R2 path.

Qualify Mollie resource/period association using provider test contracts, including unseen renewal IDs if auto-renew is chosen. Do not substitute the archived known-ID-only, €1 publication algorithm. Resend test contracts retain signed event intake, token-safe durable delivery and suppression.

Produce a reproducible versions/configuration/evidence report, actual proposed service costs and a revised estimate for all eight slices. Routine technical choices can be qualified within the selected stack; a required change to confirmed business/provider/budget constraints must be surfaced before dependent live use.

## 5. Activation gates
- Synthetic engineering can proceed with clearly labeled test policies while founder/professional/operating decisions are completed.
- Real personal data requires NFR-09 evidence, operator/notices/contracts/transfers/rights/retention and breach readiness, including new-board data flows.
- Live paid membership requires exact quota/price/period/renewal/refund terms, actual merchant/method/mandate approval, reconciliation and funded operations; only free and low-price subscription services are permitted.
- Each new board requires its audience/geography/lifecycle, source-data boundary and trading/interest behavior before activation.
- Beta/growth requires the [release evidence](11-test-and-launch-plan.md), useful real participants and actual low-fee economics. Native apps, AI/model authority, platform orders/escrow and additional payment providers remain separate later decisions.

No new schedule or performance result is proven. The old 41–61 person-day/8–10-week assumptions are historical; the revised scope must be estimated after policy/runtime qualification. No implementation, purchase or deployment is part of this design update.
