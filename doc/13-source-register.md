# Source register and provenance
**Version:** 0.3 · **Research:** 20–21 October 2026; revised requirements supplied 21 October.
Public pages support only narrow cited observations. Product rules, budgets and cost assumptions are proposed design, not external findings.

## 1. User-authoritative direction
Latest instruction confirms client pays no platform commission; company pays. Conflicting client-€1 examples remain analysis only. Northern pilot, broader maintenance trades, guest posting, private rough offers, contact choice, opt-in SME notices, three-month deletion and an information-intermediary role replace incompatible 0.2 requirements. Earlier confirmed desktop/Android/iPhone PWA-first direction remains. The user's later suggestion to consider stripe-node is recorded as a preferred payment candidate, not final merchant/provider approval.

Charge event (first successful offer publication), €1 gross treatment, direct checkout, exact province boundary, guest email security and calendar/purge details are explicit recommendations requiring approval, not secretly “confirmed” facts.

## 2. Public product/design references
| Source | Use / limitation |
|---|---|
| [Profiman professionals](https://profiman.nl/vakman/) | Public lead/credit/competition/check/support statements; no purchase or proof of service outcome |
| [Werkspot professional help](https://support.werkspot.nl/s/article/How-do-I-get-in-contact-with-a-consumer) | Historical first-party contact-charge flow read previously; 21 October fetch gave a loading/CSS-error page, not a current verified price |
| [DeGoedkoopsteNotaris](https://www.degoedkoopstenotaris.nl/) | New user reference: service/postcode discovery, comparison and free tailored quote requests; business-side charging not established |
| [awesome-design-md](https://github.com/VoltAgent/awesome-design-md) | Requested agent-readable DESIGN.md method |
| [Notion DESIGN example](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/notion/DESIGN.md) | Token/component/state specification structure, not copied brand identity |
| [Linear DESIGN example](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/linear.app/DESIGN.md) | Restrained hierarchy/specification discipline |

The user's €10–15 competitor-cost concern is a market hypothesis; do not assert a universal live tariff or guaranteed savings. No search ad tracking query parameters from the notary reference are needed in the documents.

## 3. Technical/provider sources
| Source | Supports |
|---|---|
| [Stripe official Node.js SDK](https://github.com/stripe/stripe-node) | Server SDK named stripe-node; npm package `stripe`, supported runtimes/versioning, webhook helper and request options; pin stable versions at implementation |
| [Stripe hosted Checkout](https://docs.stripe.com/checkout/quickstart) | Hosted one-time-payment setup; no custom card form needed for pilot |
| [Stripe Checkout fulfillment](https://docs.stripe.com/checkout/fulfillment?payment-ui=stripe-hosted) | Server retrieval, repeated/concurrent fulfillment and delayed payment events |
| [Stripe webhooks](https://docs.stripe.com/webhooks) | Raw-body signature verification, delivery handling and endpoint secrets |
| [Stripe API idempotency](https://docs.stripe.com/api/idempotent_requests) | API v1 saved responses including errors and >=24h key pruning; local business uniqueness still required |
| [Stripe refunds](https://docs.stripe.com/refunds) | PaymentIntent/charge refund references, original-method refund and pending/failed lifecycle |
| [Stripe Netherlands pricing](https://stripe.com/nl/pricing) | Checked 21 October: iDEAL €0.29, standard EEA cards 1.5% + €0.25; indicative, actual contract/rounding/other products differ |
| [Stripe iDEAL / Wero](https://docs.stripe.com/payments/ideal) | EUR Dutch bank-payment flow, transition context, merchant eligibility and refund window; recheck before launch |
| [Mollie Dutch pricing](https://www.mollie.com/nl/pricing) | Public indicative local payment rate checked 21 October; merchant agreement/method/refund fees still required |
| [Mollie payment flow](https://docs.mollie.com/docs/accepting-payments) | Hosted checkout, webhook notification and authenticated status fetch |
| [PostgreSQL locks](https://www.postgresql.org/docs/current/explicit-locking.html) | Real row-lock mechanism, not proof our future code is correct |
| [Next.js data security](https://nextjs.org/docs/app/guides/data-security) | Server-only DAL/authorization/minimal DTO pattern |
| [Next.js PWA guide](https://nextjs.org/docs/app/guides/progressive-web-apps) | Manifest/framework integration, not a complete private-cache policy |
| [MDN PWA installation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable) | Platform install differences |
| [Apple iPhone web apps](https://support.apple.com/guide/iphone/open-as-web-app-iphea86e5236/ios) | Safari home-screen installation |
| [WebKit Home Screen push](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/) | Original supported push/permission mechanism, not a current release test |
| [MDN worker lifecycle](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers) | Install/update/cache concepts |
| [WCAG 2.2](https://www.w3.org/TR/WCAG22/) | Accessibility target and whole-flow assessment |

## 4. Location and advertising
| Source | Supports / limitation |
|---|---|
| [PDOK Location API](https://www.pdok.nl/location-api1) | Official open/free Dutch geocoding candidate; inspect current API/dataset licensing/CRS/limits before use; not bulk-export permission or a service SLA |
| [Google Geocoding policy](https://developers.google.com/maps/documentation/geocoding/policies) | Storage/cache/attribution restrictions and terms |
| [Google Maps pricing](https://developers.google.com/maps/billing-and-pricing/pricing) | SKU-based cost review; no perpetual free-credit assumption |
| [Google Geocoding usage/billing](https://developers.google.com/maps/documentation/geocoding/usage-and-billing) | Quota/billing integration review |
| [Google Ads daily budgets](https://support.google.com/google-ads/answer/6385083?hl=en) | Average daily versus spending-limit behavior, not a guaranteed CPC |
| [Smallingerland](https://www.smallingerland.nl/) | Municipal context for the Drachten trial label; final region mapping must be validated |

## 5. Legal/accounting review sources
| Source | Review topic, not a legal clearance |
|---|---|
| [GDPR authoritative text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) | Purpose, lawful basis, security, rights and retention assessment |
| [Commission DSA](https://digital-strategy.ec.europa.eu/en/policies/digital-services-act) | Intermediary/trader role and size-dependent duties; “information only” not automatic immunity |
| [Commission P2B](https://digital-strategy.ec.europa.eu/en/policies/platform-business-trading-practices) | Current legislation and proposed-change context; recheck in force at launch |
| [Commission DAC7](https://taxation-customs.ec.europa.eu/taxation/tax-transparency-cooperation/administrative-co-operation-and-mutual-assistance/dac7_en) | Reporting scope assessment for service facilitation, not automatic exemption from off-platform payment |
| [Belastingdienst business administration](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/) | Financial retention differs from task content; accountant classifies records/start dates |
| [Belastingdienst VAT explanation](https://stichtingenvereniging.belastingdienst.nl/belastingplicht/omzetbelasting-btw/btw-ondernemer-wat-nu/) | General VAT background; 21% is illustrative until specific service classified |
| [ACM ConsuWijzer spam information](https://consument.acm.nl/veilig-online/hoe-voorkom-ik-spam) | Consent/unsolicited-email background available as indexed first-party text; direct fetch timed out, and an older ACM path returned 404; recheck current guidance at launch |

## 6. Not established
No customer interviews, provider/merchant contracts, legal/tax opinion, current authenticated competitor pricing, domain/trademark clearance, paid Ads experiment, real geocoder dataset audit, implementation, browser/device tests or production deployment. All numerical operating estimates and conversion/CPC/refund examples are hypotheses, not promised performance.
