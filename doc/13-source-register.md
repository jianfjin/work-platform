# Source register and research provenance

**Accessed:** 20 October 2026. Public pages can change. Sources support the narrow observations/technical mechanics cited; original requirements, cost assumptions and recommendations are this proposal's own design.

## 1. Competitor evidence

| ID | First-party source | Use / limitation |
|---|---|---|
| C1 | [Profiman client page](https://profiman.nl/) | Client flow, free requests, regional/verified matching, up to three quotations; self-description, not measured service delivery |
| C2 | [Profiman professional page](https://profiman.nl/vakman/) | Credit-funded lead access, displayed starting price, competitor cap and invalidity-support window; no purchase performed |
| C3 | [Werkspot homepage](https://www.werkspot.nl/) | Public client workflow, reviews and messaging/app positioning; no need to reproduce mutable aggregate counters |
| C4 | [Werkspot: contact with a client](https://support.werkspot.nl/s/article/How-do-I-get-in-contact-with-a-consumer) | Public description of interest, client selection and contact-sharing charge trigger; help article is dated December 2024 |
| C5 | [Werkspot: not selected but charged](https://support.werkspot.nl/s/article/I-wasn-t-selected-for-the-job-So-why-do-I-pay-for-the-lead) | Lead access versus guaranteed work; narrow refund explanation, not an exhaustive current refund-policy audit |

Some help results were accessible as indexed first-party text rather than authenticated product UI. Their published dates and public-view limitations are preserved here. No Trustpilot/Reddit anecdotes were used as proof of platform defects or current price ranges.

## 2. Requested design references

| ID | Source | Use |
|---|---|---|
| D1 | [VoltAgent awesome-design-md collection](https://github.com/VoltAgent/awesome-design-md) | DESIGN.md as an agent-readable visual contract |
| D2 | [Notion DESIGN.md example](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/notion/DESIGN.md) | Token hierarchy, component/state detail and specification structure; no copied branding |
| D3 | [Linear DESIGN.md example](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/linear.app/DESIGN.md) | Restrained hierarchy/surfaces and component specification discipline; original palette/fonts chosen for HoutMatch |

The actual Markdown files were inspected through their raw GitHub versions as well as catalogue descriptions. No brand assets or substantive source text are vendored into this repository. Reference does not imply endorsement or affiliation.

## 3. Technical and pricing references

| ID | Primary source | Supports |
|---|---|---|
| T1 | [Mollie Netherlands pricing](https://www.mollie.com/nl/pricing) | Indicative local transaction fee used for arithmetic; not a contractual rate |
| T2 | [Mollie accepting payments](https://docs.mollie.com/docs/accepting-payments) | Hosted checkout, callback, API status retrieval and delayed callback/return handling |
| T3 | [Mollie create-payment API](https://docs.mollie.com/reference/create-payment) | Payment intent/checkout URL/metadata/amount contract |
| T4 | [Mollie payment statuses](https://docs.mollie.com/docs/handling-payment-status) | Paid/pending/failed/etc. distinction; don't infer success from browser redirect |
| T5 | [PostgreSQL explicit locking](https://www.postgresql.org/docs/current/explicit-locking.html) | Row-lock/concurrency mechanism, not proof that our future code is correct |
| T6 | [Next.js data-security guidance](https://nextjs.org/docs/app/guides/data-security) | Server-only DAL, authorization and minimal DTO boundary |
| T7 | [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/) | Accessibility target, contrast/focus and full-flow evaluation reference |
| T8 | [MDN installable PWA guidance](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable) | Browser-specific installation/standalone behavior; do not assume one automatic install prompt works everywhere |
| T9 | [Apple: website as an iPhone web app](https://support.apple.com/guide/iphone/open-as-web-app-iphea86e5236/ios) | Current Safari home-screen installation instructions; recheck UI wording per supported release |
| T10 | [WebKit: iOS/iPadOS Web Push](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/) | Home-screen/permission requirements and original iOS 16.4 introduction; not a current-device support guarantee |
| T11 | [Next.js PWA guide](https://nextjs.org/docs/app/guides/progressive-web-apps) | Manifest and PWA framework integration; not a substitute for our restrictive cache/transaction policy |
| T12 | [MDN service-worker lifecycle](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers) | Browser worker install/activate/update/cache concepts, distinct from server job workers |

Stack choices are recommendations, not independent feature/version audits. Pin maintained supported releases and test actual provider APIs at implementation kickoff. The design does not depend on guessed webhook signatures or unverified provider idempotency support; verify the chosen product contract.

## 4. Legal/accounting scope-assessment references

| ID | Authority/source | Review topic, not a compliance conclusion |
|---|---|---|
| L1 | [Belastingdienst VAT explanation](https://stichtingenvereniging.belastingdienst.nl/belastingplicht/omzetbelasting-btw/btw-ondernemer-wat-nu/) | General tariff reference only; accountant determines access/voucher treatment and invoices |
| L2 | [GDPR authoritative text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) | Purposes, lawful bases, rights, security and privacy-by-design assessment |
| L3 | [European Commission DSA](https://digital-strategy.ec.europa.eu/en/policies/digital-services-act) | Role/size-dependent intermediary/marketplace scope and transparency duties |
| L4 | [European Commission P2B](https://digital-strategy.ec.europa.eu/en/policies/platform-business-trading-practices) | Current rules and proposed repeal context; recheck enacted law before launch |
| L5 | [European Commission DAC7](https://taxation-customs.ec.europa.eu/taxation/tax-transparency-cooperation/administrative-co-operation-and-mutual-assistance/dac7_en) | Personal-services platform reporting scope assessment |
| L6 | [DNB electronic-money authorization exceptions](https://www.dnb.nl/en/sector-information/open-book-supervision/open-book-supervision-sectors/electronic-money-institutions/authorisation-as-an-electronic-money-institution-overview/services-not-requiring-authorisation/) | Prepaid access-balance classification, conditional exclusions/exemptions |

## 5. What was not established

No customer interviews or transaction experiments; no market-size/revenue forecast validation; no authenticated competitor audit; no actual Werkspot fee quote; no provider commercial agreements; no legal/tax opinion; no live implementation/performance/accessibility results. Target repository inspection found an empty repository with `main` as its configured default branch at the start of this work.

All financial examples, capacity policy, geographic pilot, implementation effort, retention defaults and SLOs are explicitly proposed assumptions. Revalidate claims and mutable sources before external marketing or a paid launch.

**User requirement provenance:** the user explicitly selected mobile option 2 (PWA) for the current stage and option 1 (dedicated Android/iPhone applications) after maturity. This is a confirmed delivery direction, not a conclusion inferred from any external source. Desktop means browser delivery in the current phase, with optional supported-browser installation; no desktop binaries were requested or created.
