# Source register and provenance

**Current scope:** Version 0.4 is defined by the [redesign](22-platform-redesign.md) and [ADR 0013](../docs/adr/0013-expand-to-three-boards-and-free-subscription-services.md). Earlier sections record dated v0.3 decisions/research; per-offer €1 economics and unlimited-free-reading statements there are historical, not the current tier specification.

**Version:** 0.3 · **Research:** 20–22 October 2026; Mollie selection supplied 22 October.
Public pages support only narrow cited observations. Product rules and budget decisions come from the recorded user confirmations; unvalidated cost/capacity examples remain planning assumptions.

## 1. User-authoritative direction
User direction confirms client pays no platform commission; company pays. Conflicting client-€1 examples remain analysis only. Northern pilot, broader maintenance trades, guest posting, private rough offers, contact choice, opt-in SME notices, three-month deletion and an information-intermediary role replace incompatible 0.2 requirements. Earlier confirmed desktop/Android/iPhone PWA-first direction remains. On 22 October the user explicitly selected Mollie as the sole current provider; previous provider proposals are superseded, not fallback options.

During the implementation-planning interview on 21 October 2026, the user confirmed:

- €1 including VAT per company/task for first successful private rough-offer publication, free reading/edits, no guaranteed reply and a full refund for failed publication; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md).
- The whole provinces of Groningen and Fryslân, including Drachten, with company-selected service radii controlling matches.
- Email confirmation before publication, short-lived task-scoped email access and fresh-link recovery without registration; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md).
- Client contacts private by default, with explicit selected-field access for every published responder while open; revocation stops future disclosure but cannot recall copied details; [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md).
- Immutable task-content expiry three calendar months from first task availability to companies, without renewal by later activity or paid offer publication; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).
- Immediate expiry access cutoff, live task data/every photo version purged within 24h, isolated backup roll-off within 30 days and deletions reapplied before restored service; recorded in the same [task-retention ADR](../docs/adr/0004-expire-tasks-from-first-publication.md).
- Narrow separate retention for minimum platform-fee/invoice records, active company/alert settings and strictly necessary case evidence; holds need a named case, restricted access and a review/deletion date, with exact schedules subject to professional review; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).
- Optional client accounts, multi-task account dashboards and task linking deferred until after MVP; company/staff accounts remain in scope and client management uses task-scoped email access/recovery; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md).
- Material task changes create a new scope revision; captured payments for unpublished old-scope offers refund, while already published offers stay visible with scope-change labels/free edits and no automatic refund or extra fee; [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md).
- Voluntary reopening before original expiry retains offers/fees with no re-charge, resets contacts to private pending fresh opt-in and never revives canceled payment attempts; [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).
- Published-offer withdrawal is final for the company/task in MVP: client company-name/withdrawn marker only, offer/contact fields hidden, callback access stopped, original fee retained without automatic refund, and no restoration/republishing/new checkout; [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).
- Individual hosted checkout per €1 publication fee, with prepaid bundles, wallets and subscriptions deferred until after MVP; provider and live merchant/method decisions remain separate; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md).

On 22 October 2026 the founder explicitly selected Mollie-hosted checkout/Payments API as the sole current integration, without alternative/fallback evaluation; [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md). The preceding 21 October collection decision did not choose a provider; this later instruction resolves that choice.

The founder then confirmed iDEAL/Wero and cards as the only MVP method targets, with merchant approval and successful €1 payment, full-refund and phone/PWA return tests required before activating each method. This is a product decision, not a claim that the merchant is already eligible or tested; see the same [Mollie ADR](../docs/adr/0007-use-mollie-for-current-payment-integration.md).

The founder also confirmed full €1 refunds for substantiated fake, duplicate or materially misrepresented tasks, with a seven-day reporting target from payment, review of credible later evidence and a recorded reason for each approval. No reply or losing the work alone remains non-refundable. The founder then confirmed submission to Mollie within one business day of verification or staff approval, automatic submission for verified failed-publication/duplicate-capture cases, and overdue/queued/failed escalation. Submission is separate from provider completion/bank arrival. These product decisions are recorded in [payments](08-payments-and-economics.md).

The founder subsequently confirmed explicit task-alert opt-in with one daily digest as the default and optional instant emails, retaining category/radius matching and easy unsubscribe; see [operations](15-operations-and-growth.md). Frequency selection alone is not subscription consent.

The founder then asked whether the proposed design is a monolith and where PostgreSQL would be deployed. This clarification is not approval of the architecture or a hosting provider. The proposed answer is a modular monolith deployed as separate web/worker processes with an independently managed database; Render Frankfurt is the concrete hosting candidate in [architecture](04-architecture.md), pending selection and review.

The founder also asked whether Supabase is an option. Add Supabase Frankfurt PostgreSQL to the candidate comparison, retaining backend-owned domain/payment rules and explicitly reviewing connectivity and recovery costs. Supabase Auth/Storage remain optional; no provider, architecture or network-boundary decision is approved by this question.

The founder subsequently stated that the app should deploy on Cloudflare and requested a comparison with the recommended Render solution. Cloudflare is now the requested app-hosting direction; [the comparison](18-cloudflare-vs-render.md) proposes shared application modules with request/scheduled handlers and external EU PostgreSQL. Database, framework integration, media and exact job configuration remain proposals. No account, deployment, runtime qualification or live charge has occurred.

The founder then asked to consider FastAPI, Go and Rust/Tokio for the backend. These are candidates, not a language selection; [backend options](19-backend-options.md) distinguishes Workers execution from conventional container servers. Maintainer language preference has been requested and is not inferred. Cloudflare app hosting does not restrict the backend to TypeScript or approve a Containers deployment.

The founder subsequently asked the assistant to choose between TypeScript and FastAPI using performance, low latency and high concurrency. Under that delegation the assistant selected TypeScript on Workers, based on expected runtime overhead and shared tooling rather than a claimed project benchmark. Record the selected direction in [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md); an unanswered maintainer-preference question is not a blocker or an assumed answer. Exact frameworks/providers and real-route performance qualification remain open.

Region-mapping and technical timezone/DST validation remain implementation work; exact separate retention purposes/fields/schedules remain professional-review deliverables. Live merchant/method activation, tax classification and refund operational readiness remain launch work; the provider, method scope and refund eligibility/reporting/submission target are settled.

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
| [Mollie Dutch pricing](https://www.mollie.com/nl/pricing) | Checked 22 October: iDEAL/Wero €0.32; EEA consumer cards 1.8% + €0.25 and commercial cards 2.9% + €0.25; actual merchant terms/rounding still required |
| [Mollie payment flow](https://docs.mollie.com/docs/accepting-payments) | Checked 22 October: payment creation, hosted checkout URL/GET redirect, classic notification and authenticated status retrieval |
| [Mollie create payment](https://docs.mollie.com/reference/create-payment) | Payments API v2, exact decimal amount, configured redirect/webhook and opaque metadata; do not copy task data into provider fields |
| [Mollie get payment](https://docs.mollie.com/reference/get-payment) | Authenticated payment retrieval, environment handling and associated refund/chargeback reads |
| [Mollie classic webhooks](https://docs.mollie.com/reference/webhooks) | Form-encoded resource ID, repeated later status notifications, generic unknown-ID 200, authenticated retrieval and reachable HTTPS endpoint |
| [Mollie API idempotency](https://docs.mollie.com/reference/api-idempotency) | One-hour POST response cache, keys tied to API credential; local financial uniqueness and unknown-outcome reconciliation outlive the cache |
| [Mollie payment statuses](https://docs.mollie.com/docs/handling-payment-status) | Paid versus open/pending/authorized/failed/canceled/expired; no return-page payment proof |
| [Mollie refunds](https://docs.mollie.com/docs/refunds) | Separate refund states, balance-related queueing, callback gaps and reconciliation before retry after a timeout |
| [Mollie create refund](https://docs.mollie.com/reference/create-refund) | Refund belongs to a recorded payment; exact amount/opaque refund reference |
| [Mollie get refund](https://docs.mollie.com/reference/get-refund) | Authenticated refund retrieval with parent payment and test/live scope |
| [PostgreSQL locks](https://www.postgresql.org/docs/current/explicit-locking.html) | Real row-lock mechanism, not proof our future code is correct |
| [Render regions](https://render.com/docs/regions) | Checked 22 October: Frankfurt, Germany is a deployment region; the founder later selected this database location, with configuration and recovery qualification still open |
| [Render private networking](https://render.com/docs/private-network) | Same-workspace/region private connectivity and internal PostgreSQL URL; worker can initiate private connections |
| [Render background workers](https://render.com/docs/background-workers) | Continuously running separate worker process; PostgreSQL queue polling is our proposed application design |
| [Render PostgreSQL setup](https://render.com/docs/postgresql-creating-connecting) | Separate managed database service, colocated-region recommendation, private connection and paid PITR availability |
| [Render PostgreSQL recovery](https://render.com/docs/postgresql-backups) | Continuous paid-instance backups; three-/seven-day workspace-dependent PITR windows and restore into a new instance; no application RPO/RTO or retention qualification implied |
| [Supabase regions](https://supabase.com/docs/guides/platform/regions) | Checked 22 October: exact Frankfurt `eu-central-1` project region; a general Europe grouping is not an EU jurisdiction guarantee |
| [Supabase PostgreSQL connections](https://supabase.com/docs/guides/database/connecting-to-postgres) | Persistent-backend direct/session-pooler options, IP-version differences and TLS configuration |
| [Supabase network restrictions](https://supabase.com/docs/guides/platform/network-restrictions) | IP controls cover PostgreSQL/poolers, not Auth/Storage/Data API HTTPS endpoints |
| [Supabase pricing](https://supabase.com/pricing) | Checked 22 October: Pro starts at $25/month; paid configuration, compute/add-ons/usage still need a full quote |
| [Supabase backups](https://supabase.com/docs/guides/platform/backups) | Daily backup cadence, PITR add-on from about $100/month and Small-compute prerequisite; database backups exclude Storage objects |
| [Render pricing](https://render.com/pricing) | Checked 22 October: small paid web/worker instances $7 each and PostgreSQL compute from $6/month; illustrative floor, not capacity/usage/workspace quote |
| [Cloudflare Next.js guidance](https://developers.cloudflare.com/workers/framework-guides/web-apps/nextjs/) | Checked 22 October: vinext recommended for new apps, Next.js API reimplementation and beta label; compatibility qualification still required |
| [Cloudflare OpenNext guidance](https://developers.cloudflare.com/workers/framework-guides/web-apps/opennext/) | Separate build-adapter path and supported-feature matrix; no automatic framework selection |
| [Workers limits](https://developers.cloudflare.com/workers/platform/limits/) | Invocation/CPU/memory constraints and request-scoped `waitUntil()` lifetime; no persistent-process assumption |
| [Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/) | Paid subscription minimum $5/month plus metered usage; database/media/additional services separate |
| [Hyperdrive pricing](https://developers.cloudflare.com/hyperdrive/platform/pricing/) | Pooling included with Workers Paid; external database billed separately |
| [Hyperdrive query caching](https://developers.cloudflare.com/hyperdrive/concepts/query-caching/) | Query caching enabled by default; application proposal disables it for current private/domain state |
| [Hyperdrive connection lifecycle](https://developers.cloudflare.com/hyperdrive/concepts/connection-lifecycle/) | Transaction pooling rather than persistent per-request session ownership |
| [Hyperdrive Supabase integration](https://developers.cloudflare.com/hyperdrive/examples/connect-to-postgres/postgres-database-providers/supabase/) | Use the direct database origin behind Hyperdrive rather than a nested Supabase pooler |
| [Hyperdrive firewall configuration](https://developers.cloudflare.com/hyperdrive/configuration/firewall-and-networking-configuration/) | Cloudflare source ranges and public-ACL/private-tunnel networking options; no assumed Render private-network membership |
| [R2 data location](https://developers.cloudflare.com/r2/reference/data-location/) | Explicit EU jurisdiction versus best-effort location hints; primary object storage placement control |
| [R2 public buckets](https://developers.cloudflare.com/r2/buckets/public-buckets/) | Buckets are private by default; custom-domain and r2.dev public access must be explicitly enabled |
| [Workers regional processing](https://developers.cloudflare.com/data-localization/how-to/workers/) | Regional processing requires separate localization configuration; EU storage alone does not establish EU-only Workers execution |
| [FastAPI on Python Workers](https://developers.cloudflare.com/workers/languages/python/packages/fastapi/) | Checked 22 October: official FastAPI/ASGI entrypoint; no ordinary Uvicorn listener assumed inside an isolate |
| [Python Hyperdrive drivers](https://developers.cloudflare.com/hyperdrive/examples/python-workers/) | Documented Python PostgreSQL drivers including asyncpg; async SQLAlchemy restriction differs from support for async SQL drivers |
| [Workers languages](https://developers.cloudflare.com/workers/languages/) | TypeScript/Python/Rust first-class language paths and additional Wasm languages including Go |
| [Rust Workers](https://developers.cloudflare.com/workers/languages/rust/) | workers-rs/Wasm handler model and event-loop integration rather than an ordinary native socket listener |
| [Cloudflare Containers](https://developers.cloudflare.com/containers/) | Native runtimes/container images controlled through Workers; separate lifecycle/billing qualification needed |
| [Containers Go example](https://developers.cloudflare.com/containers/get-started/) | Official example deploys a Go web server in a container |
| [FastAPI features](https://fastapi.tiangolo.com/features/) | OpenAPI/JSON Schema and validation capabilities, not measured project productivity |
| [Go HTTP library](https://pkg.go.dev/net/http) | Standard HTTP client/server facilities |
| [Axum](https://docs.rs/axum/latest/axum/) | Rust HTTP routing/request handling; candidate alongside Tokio, not a selected framework |
| [Tokio](https://docs.rs/tokio/latest/tokio/) | Async runtime role and restricted/experimental Wasm feature support |
| [Workers runtime model](https://developers.cloudflare.com/workers/reference/how-workers-works/) | Checked 22 October: V8 isolate execution; architectural evidence rather than an application throughput result |
| [Python Workers internals](https://developers.cloudflare.com/workers/languages/python/how-python-workers-work/) | Pyodide/CPython-in-Wasm execution and deployment-time snapshot startup optimizations; no project-specific latency ratio implied |
| [FastAPI async concurrency](https://fastapi.tiangolo.com/async/) | Async I/O/concurrency support; no basis for assuming Python cannot serve concurrent I/O |
| [Hono on Workers](https://hono.dev/docs/getting-started/cloudflare-workers) | Official Workers deployment path; Hono subsequently confirmed by the founder, with exact version/integration still to qualify |
| [React Router on Workers](https://developers.cloudflare.com/workers/framework-guides/web-apps/react-router/) | Confirmed frontend's React/Vite/SSR integration; current plugin excludes framework SPA mode and prerendering; no project build has run |
| [Better Auth Hono integration](https://better-auth.com/docs/integrations/hono) | Selected auth library; documented Hono mounting and Workers compatibility flags, not project qualification |
| [Better Auth PostgreSQL](https://better-auth.com/docs/adapters/postgresql) | Auth-record storage and schema support; exact Hyperdrive/driver/version integration remains untested |
| [Better Auth 2FA](https://better-auth.com/docs/plugins/2fa) | TOTP/recovery-code primitives for the selected staff policy; sign-in method, recovery and role-change coverage still require explicit enforcement |
| [Brevo sending API](https://developers.brevo.com/docs/send-a-transactional-email) | Earlier email candidate: HTTP message submission and sender setup; not selected or tested |
| [Brevo data location](https://help.brevo.com/hc/en-us/articles/360001005510-Data-storage-location) | Provider states EU database hosting; not proof of every subprocessor/transfer or the platform retention configuration |
| [Brevo delivery events](https://developers.brevo.com/docs/transactional-webhooks) | Earlier candidate's bounce/complaint/delivery events; not selected or tested |
| [Resend introduction](https://resend.com/docs/introduction), [Workers](https://resend.com/docs/send-with-cloudflare-workers), [Better Auth](https://resend.com/docs/send-with-better-auth) | Selected email provider compatible with the chosen stack; examples do not establish the platform's durability or runtime qualification |
| [Resend idempotency](https://resend.com/docs/dashboard/emails/idempotency-keys) | Optional stable-key suppression lasts 24 hours; local delivery identity and ambiguous-result policy remain necessary |
| [Resend webhook verification](https://resend.com/docs/webhooks/verify-webhooks-requests) | Raw-body/signature verification for platform events; not present in the inspected cloud-mail handler |
| [Resend sending regions/residency](https://resend.com/docs/dashboard/domains/regions) | Ireland sending is distinct from US account-data/log/API-record storage; provider review remains necessary |
| [Better Auth email/password](https://better-auth.com/docs/authentication/email-password), [magic links](https://better-auth.com/docs/plugins/magic-link) | Both methods are documented; the founder selected verified-email/password company sign-in, with verification and password recovery |
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
| [PDOK Location API](https://www.pdok.nl/location-api1) | Confirmed provider on 22 October; official open/free geocoding, with API/dataset licensing/CRS/limits still to qualify; not bulk-export permission or a service SLA |
| [PDOK Location API specification](https://api.pdok.nl/kadaster/location-api/v1/api) | Checked 22 October: current documented Location API entry point, separate from legacy Locatieserver; exact integration and source-data terms still to qualify |
| [PDOK Location API collections](https://api.pdok.nl/kadaster/location-api/v1/collections) | Checked 22 October: current address/place/municipality/province source metadata; existence is not proof of a complete permitted coarse-postcode lookup or correct task matching |
| [Google Geocoding policy](https://developers.google.com/maps/documentation/geocoding/policies) | Storage/cache/attribution restrictions and terms |
| [Google Maps pricing](https://developers.google.com/maps/billing-and-pricing/pricing) | SKU-based cost review; no perpetual free-credit assumption |
| [Google Geocoding usage/billing](https://developers.google.com/maps/documentation/geocoding/usage-and-billing) | Quota/billing integration review |
| [Google Ads daily budgets](https://support.google.com/google-ads/answer/6385083?hl=en) | Average daily versus spending-limit behavior, not a guaranteed CPC |
| [Smallingerland](https://www.smallingerland.nl/) | Municipal context for the Drachten trial label; final region mapping must be validated |

## 5. Legal/accounting review sources
| Source | Review topic, not a legal clearance |
|---|---|
| [GDPR authoritative text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) | Purpose, lawful basis, security, rights and retention assessment |
| [EDPB privacy by design/accountability](https://www.edpb.europa.eu/sme/be-compliant/be-compliant_en) | Checked 22 October: embed minimization/default safeguards, maintain processing records and assess DPIA applicability; delivery evidence rather than regional-hosting claims |
| [EDPB controller/processor responsibilities](https://www.edpb.europa.eu/sme/learn-the-basics/data-controller-or-data-processor_en) | Checked 22 October: role-specific accountability, processor contracts/subprocessor controls and international-transfer duties |
| [EDPB individual rights](https://www.edpb.europa.eu/sme/be-compliant/respect-individuals-rights_en) | Checked 22 October: applicable rights, proportionate identity checks, one-month response/extension notice and recipient follow-up |
| [EDPB breach-response summary](https://www.edpb.europa.eu/system/files/2025-03/edpb_summary_092022-012021_data_breach_en.pdf) | Checked 22 October: breach records, risk assessment, required authority notification within 72h of awareness and high-risk individual notification |
| [Commission DSA](https://digital-strategy.ec.europa.eu/en/policies/digital-services-act) | Intermediary/trader role and size-dependent duties; “information only” not automatic immunity |
| [Commission P2B](https://digital-strategy.ec.europa.eu/en/policies/platform-business-trading-practices) | Current legislation and proposed-change context; recheck in force at launch |
| [Commission DAC7](https://taxation-customs.ec.europa.eu/taxation/tax-transparency-cooperation/administrative-co-operation-and-mutual-assistance/dac7_en) | Reporting scope assessment for service facilitation, not automatic exemption from off-platform payment |
| [Belastingdienst business administration](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/) | Financial retention differs from task content; accountant classifies records/start dates |
| [Belastingdienst administration setup and retention](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/een_administratie_opzetten) | Planning check on 21 October: record categories and retention periods differ; does not classify this platform's exact records |
| [AP personal-data retention guidance](https://autoriteitpersoonsgegevens.nl/nl/over-privacy/persoonsgegevens/bewaren-van-persoonsgegevens) | Planning check on 21 October: necessity by purpose, applicable legal periods, documented justification and destruction when no longer needed; not a blanket task archive |
| [Belastingdienst VAT explanation](https://stichtingenvereniging.belastingdienst.nl/belastingplicht/omzetbelasting-btw/btw-ondernemer-wat-nu/) | General VAT background; 21% is illustrative until specific service classified |
| [ACM ConsuWijzer spam information](https://consument.acm.nl/veilig-online/hoe-voorkom-ik-spam) | Consent/unsolicited-email background available as indexed first-party text; direct fetch timed out, and an older ACM path returned 404; recheck current guidance at launch |

## 6. Possible AI extension

On 22 October the founder also asked whether possible AI support (chat, refund and complaint handling), knowledge/Wiki and Jev-like or self-hosted open-weight models change the language recommendation. The [assessment](20-ai-service-and-knowledge-options.md) retains TypeScript for platform integration and proposes Python where custom model/processing work needs it. This conditional question does not approve AI MVP scope, a model, an inference service, automated incident-refund authority or a backend rewrite. “Jev” is provisionally interpreted as TypeSafe AI's model pending clarification.

The founder then explicitly instructed “Choose TypeScript for the backend” and requested the next interview question. This confirms the platform language after the AI assessment; it does not select a Python component, model, AI delivery scope or database provider. The next question concerns PostgreSQL hosting.

The founder subsequently confirmed the recommendation of paid Render PostgreSQL in Frankfurt, connected to Cloudflare through Hyperdrive; see [ADR 0009](../docs/adr/0009-use-render-postgresql-in-frankfurt.md). Render is now the selected database provider, while Supabase and Render app hosting remain comparison context. Compute/storage/version, exact framework, network/driver qualification, recovery evidence and operating budget remain open; no resources have been provisioned.

The founder then confirmed Hono for the TypeScript/Workers backend HTTP API. This selects API routing/middleware, with shared application/domain rules retained; it does not select a frontend framework, authentication package or exact dependency version. React Router framework mode with React/Vite/SSR was the recommendation for the next frontend question at that point.

The founder then confirmed React Router framework mode with React/Vite/SSR for the Web/PWA; see [ADR 0010](../docs/adr/0010-use-react-router-for-web-pwa.md). Hono remains the HTTP API; exact versions/build/cookie/PWA integration remain untested. Better Auth for company/staff accounts with records in Render PostgreSQL was the next recommendation at that point.

The founder subsequently confirmed Better Auth and PostgreSQL account/session storage; see [ADR 0011](../docs/adr/0011-use-better-auth-for-company-and-staff-accounts.md). This selects the authentication library, while company/staff sign-in methods, MFA/recovery rules and exact runtime integration remain open. Verified email/password for company sign-in was the next recommendation at that point.

The founder then confirmed company sign-in through verified email and password, including email verification and password recovery. This does not select staff sign-in/MFA/recovery methods or change guest task ownership/client-account deferral. Invite-only staff email/password with mandatory authenticator-app TOTP and one-use recovery codes was the next recommendation at that point.

The founder subsequently confirmed that staff-access policy, including preservation of MFA through password recovery. Initial staff provisioning, loss-of-all-factors operating recovery and runtime enforcement still need implementation/qualification. Brevo was the next email recommendation at that point; it was not accepted.

The founder then supplied the cloud-mail production dashboard and Resend documentation for inspection. Authenticated read-only Cloudflare API/source/settings and Resend domain reads confirm the existing DataWeGo integration; see [the review](21-cloud-mail-and-resend-review.md) for deployment evidence, source references and limits. The recommendation at that point was direct Resend reuse from work-platform with a separate sending credential/sender and its own durable outbox, pending the provider decision and review of US account-data storage. No email was sent, existing service changed or new platform resource provisioned.

Primary AI references checked on 22 October:

| Source | Supports / limitation |
|---|---|
| [LangGraph JavaScript](https://docs.langchain.com/oss/javascript/langgraph/overview), [Python](https://docs.langchain.com/oss/python/langgraph/overview) | Agent orchestration is available in both ecosystems; neither framework/version is selected |
| [Cloudflare Agents quick start](https://developers.cloudflare.com/agents/getting-started/quick-start/), [model pipelines](https://developers.cloudflare.com/agents/concepts/calling-llms/) | TypeScript agent/model integration examples; not deployment qualification or financial-authority approval |
| [Transformers fine-tuning](https://huggingface.co/docs/transformers/training) | Python model-adaptation tooling; no requirement to change the application language |
| [vLLM HTTP serving](https://docs.vllm.ai/en/latest/serving/online_serving/) | Model serving through HTTP; exact model and decision-head compatibility still needs verification |
| [TypeSafe introduction](https://docs.typesafe.ai/introduction), [quick start](https://docs.typesafe.ai/introduction/quickstart), [models](https://docs.typesafe.ai/models) | Typed decisions, hosted API and both language clients; reviewed pages do not establish a downloadable Jev-weight release; test Dutch independently |
| [Typical model card](https://huggingface.co/OzLabs/typical-small), [publisher repository](https://github.com/ozlabsai/typical) | Example open-weight decision artifact with Python inference; not selected or evaluated for this platform |

The founder then explicitly instructed “use Resend,” confirming direct platform sending with application-specific credentials; see [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md). The founder subsequently instructed “use a dedicated sending subdomain,” confirming that pattern while leaving the parent domain, subdomain label and sender/Reply-To addresses open until platform naming. The actual plan/budget, runtime integration and processor/retention qualification remain work. The next question concerns private photo storage. The R2 data-location and public-bucket documentation was checked on 22 October: an explicit EU jurisdiction controls object-storage placement, and public access requires explicit enablement; private authorized Worker access remains the recommendation pending confirmation. No key, domain or message has been created for work-platform and no existing DNS/sender configuration was changed.

The founder then confirmed private Cloudflare R2 photo storage with explicit EU jurisdiction and authorized Worker reads, and explicitly instructed to keep GDPR compliance throughout the project. This settles the storage/provider/access direction and establishes NFR-09 as a delivery constraint; it does not establish completed compliance or approve a vendor transfer mechanism. The privacy plan now specifies purpose/data-flow and lawful-basis records, notices/rights handling, provider roles/contracts/transfers, DPIA/DPO screening and breach readiness before real personal-data use, with AT-27 for evidence. Current EDPB guidance was readable; the EUR-Lex ELI and HTML requests encountered a JavaScript verification page during this check. The interview then turned to PDOK Location API for Dutch location lookup; its official current introduction confirms an open/free geocoding service, while exact dataset/endpoint/terms and matching accuracy still require qualification. No application or cloud resource was created.

The founder subsequently confirmed PDOK Location API for minimum server-side Dutch location lookup, with approximate radius matching in our application/database. Provider selection is settled; the current introduction/specification/collections were checked on 22 October, while exact integration, source-data terms, coarse-coordinate/province accuracy and NFR-09 review remain work. No task/contact data was sent to the service. The interview then turned to the proposed photo-upload policy: a maximum of six photos per task and 10 MB per file.

The founder then confirmed the maximum of six optional photos per task and 10 MB per file. The requirements, upload contract and acceptance cases now include actual-byte checks, concurrent in-flight quota and idempotent completion; the proposed exact byte/reservation mapping still needs implementation qualification. The interview then turned to the proposed JPEG/PNG/WebP/iPhone HEIC input policy with sanitized JPEG/WebP output. On 22 October [Apple's HEIF guidance](https://support.apple.com/en-gb/116944) confirmed iPhone HEIF capture/conversion behavior, and [Cloudflare Images limits/formats](https://developers.cloudflare.com/images/get-started/limits/) listed HEIC input support; these are feasibility references, not processor selection or a GDPR/runtime qualification result. No application, upload or cloud resource was created.

The founder then confirmed JPEG/PNG/WebP and iPhone HEIC photo input, converted to sanitized JPEG/WebP for display. This settles formats without selecting a processor. The interview then considered the [Cloudflare Images Workers binding](https://developers.cloudflare.com/images/optimization/binding/) for upload-time raw-byte conversion and private EU R2 variants; the [official R2 tutorial](https://developers.cloudflare.com/images/tutorials/optimize-user-uploaded-image/) supports the basic transform-and-store integration, not our authorization/retention controls. Documentation checked on 22 October states binding responses are not automatically cached and distinguishes high-fidelity remote from limited local behavior. [Metadata controls](https://developers.cloudflare.com/images/optimization/features/#metadata) and [current transformation pricing](https://developers.cloudflare.com/images/pricing/) inform qualification/budget work. Exact metadata removal through the binding, live HEIC behavior, runtime limits and processing-location/retention/transfer review remain unverified. No user photos, transformations, bindings or cloud resources were created.

The founder subsequently confirmed Cloudflare Images through its Workers binding for upload-time conversion with sanitized JPEG/WebP variants stored in private EU R2 and served by authorized Worker endpoints. The processor direction is settled; metadata/orientation, actual HEIC, runtime/deletion races, pricing and NFR-09 processing/retention/transfer qualification remain work. The interview then considered PostgreSQL-backed durable jobs with bounded Cloudflare Cron handlers, initially a one-minute recovery poll; optional immediate attempts may run only after durable commit. [Cron Triggers](https://developers.cloudflare.com/workers/configuration/cron-triggers/) and [runtime limits](https://developers.cloudflare.com/workers/platform/limits/) were checked on 22 October: minute-based schedules, UTC execution/configuration propagation and bounded invocation budgets require resumable/recoverable work, not a one-minute delivery guarantee. At that point the job-execution decision was pending; no trigger, binding, paid plan, deployment or cloud resource was created.

The founder then confirmed PostgreSQL-backed inbox/outbox/jobs with bounded Cloudflare Cron handlers, an initial one-minute recovery poll and optional immediate attempts after durable commit. Shared claims/leases, retries/idempotency, current eligibility and backlog monitoring remain implementation evidence; Queues is unselected. The interview then considered Drizzle ORM with node-postgres (pg). On 22 October the [Cloudflare Drizzle/Hyperdrive guide](https://developers.cloudflare.com/hyperdrive/examples/connect-to-postgres/postgres-drivers-and-libraries/drizzle-orm/) documented pg and stated its Drizzle/Postgres.js pairing is unsupported; [Better Auth](https://better-auth.com/docs/adapters/drizzle) documented its Drizzle adapter, and Drizzle documented [transactions](https://orm.drizzle.team/docs/transactions) and [generated SQL migrations](https://orm.drizzle.team/docs/drizzle-kit-generate). These support the proposed tooling direction without proving this app's transaction/adapter/runtime behavior or performance. No package, migration, trigger or cloud resource was created.

The founder subsequently confirmed Drizzle ORM with node-postgres (pg), typed schema/queries and reviewed SQL migrations, retaining explicit SQL locks/database constraints and runtime/auth-adapter qualification. The main stack choices are now settled; exact compatible versions and tuning are implementation qualification work. The interview then considered a €100/month pilot infrastructure planning target excluding VAT, covering the selected services plus required backups/basic monitoring/staging, with human work/domains/payment fees/reserve/ads/future AI separate. This is a project recommendation, not a verified vendor quote or hard billing cap. Current [Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/) and [Resend pricing](https://resend.com/pricing) were checked on 22 October; the fresh Render pricing HTML read exposed client-rendered placeholders and the Resend agent-pricing link was not readable. Exact Render/Resend plans, current charges, currency/taxes and usage still need a complete quote and qualification. No package, migration, purchase or cloud resource was created.

The founder then confirmed the €100/month infrastructure planning budget excluding VAT, covering the selected services plus required backups/basic monitoring and necessary staging/test usage. Human work, legal/accounting, domain registration, payment fees/refund reserve, advertising and future AI hosting remain separate. This settles the planning budget, not a service quote or provider-enforced billing cap. The implementation plan now consolidates the confirmed scope/stack/budget into a synthetic-data qualification milestone, staged delivery and explicit real-personal-data/paid-beta gates; exact sizing, professional review, identity/sender/merchant setup and operating readiness remain deliverables. No application, service, purchase or deployment was created.

The founder subsequently confirmed the consolidated implementation plan. Its status is now approved, with synthetic-data runtime qualification as the first implementation milestone. This records acceptance of the plan; runtime, performance, recovery, GDPR and launch evidence remain pending. No implementation, provisioning, tests or deployment occurred during this approval-record update.

## 7. Not established

During the planning retention check, the direct EUR-Lex page required a JavaScript bot check; AP and Belastingdienst guidance supplied the readable first-party retention context. Exact platform schedules still require the existing professional review.

No customer interviews, provider/merchant contracts, legal/tax opinion, current authenticated competitor pricing, domain/trademark clearance, paid Ads experiment, real geocoder dataset audit, implementation, browser/device tests or production deployment. All numerical operating estimates and conversion/CPC/refund examples are hypotheses, not promised performance.


## 8. Version 0.4 redesign evidence and unresolved policies
The founder explicitly requested semi-public-benefit/low-fee positioning for ZZP and SMEs; expanded home-maintenance trades; client-project, cooperation/work-package and tools/materials boards; and free companies viewing at most five items monthly versus unlimited-number information for subscribers. These four directions are authoritative. The founder subsequently answered “取消，仅保留免费与低价订阅两档”, explicitly canceling the former €1 offer-publication fee and confirming only free and low-price subscription services; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md) is superseded by [ADR 0013](../docs/adr/0013-expand-to-three-boards-and-free-subscription-services.md). Exact price/renewal, quota unit/scope, new-board lifecycle and settlement remain design decisions.

The previously approved v0.3 documents were snapshotted in [the archive](archive/v0.3/README.md) before active requirements, contracts, tests and delivery were revised. The selected technical stack, GDPR requirements and €100/month ex-VAT infrastructure planning budget remain in force; old delivery/fee-economics estimates do not validate the expanded scope.

| Source checked 22 October | Narrow finding |
|---|---|
| [KVK: ZZP/freelance registration](https://www.kvk.nl/starten/kan-ik-me-bij-kvk-inschrijven-als-zzper-of-freelancer/) | ZZP is a way of working, not a legal form; the product audience does not establish operator or participant legal status |
| [Belastingdienst: ANBI conditions](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/bijzondere_regelingen/goede_doelen/algemeen_nut_beogende_instellingen/aan_welke_voorwaarden_moet_een_anbi_voldoen/) | Formal ANBI designation has conditions; no such designation or exemption is established for this project |
| [Mollie recurring payments](https://docs.mollie.com/docs/recurring-payments) | First-payment mandates differ from later collection; iDEAL can lead to directdebit; renewal payment IDs can first arrive via callbacks; no new method/renewal approval is inferred |
| [Mollie create subscription](https://docs.mollie.com/reference/create-subscription) | Provider subscription configuration requires policy and integration qualification; creating it is not an application period-settlement rule |
| [Mollie classic webhooks](https://docs.mollie.com/reference/webhooks) | Payment-ID hints require authenticated lookup; subscription-created payments need validated association rather than the old permanent-ignore-unknown-ID assumption |
| [Mollie idempotency](https://docs.mollie.com/reference/api-idempotency) | Duplicate subscription/recurring creation can duplicate charges; finite one-hour, credential-scoped cache requires longer local reconciliation/uniqueness |

No new merchant plan, subscription, charge, user data flow, application test or cloud deployment was created by this documentation revision.
