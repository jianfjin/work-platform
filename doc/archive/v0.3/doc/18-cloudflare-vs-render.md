# Cloudflare versus Render deployment comparison

**Updated:** 22 October 2026. **Status:** Cloudflare requested for app hosting; TypeScript/Hono backend and paid Render PostgreSQL in Frankfurt confirmed. React Router with React/Vite/SSR is confirmed for the frontend; exact package versions, database plan/version, exact mail domain/address/plan, media and job configuration remain open; Resend with a dedicated sending subdomain is confirmed for platform email. Better Auth is confirmed for company/staff accounts with records in Render PostgreSQL; company verified-email/password sign-in, verification and password recovery are confirmed. Staff invite-only password/TOTP/one-use recovery-code access is confirmed; runtime integration and operating recovery procedures remain work. No deployment or compatibility spike has run.

This comparison describes the TypeScript/Workers deployment. After considering FastAPI, Go and Rust/Tokio, the founder delegated a performance-focused TypeScript-versus-FastAPI choice; TypeScript/Workers is now selected. See [the backend assessment](19-backend-options.md) and [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md). Container alternatives remain comparison context and are not parallel MVP implementations. The Workers-only price examples below do not price them, and no performance result has been measured.

## Recommendation

Use Cloudflare Workers as the application-hosting direction requested by the founder. Keep one TypeScript application with internal domain modules and PostgreSQL transactions. Replace the earlier always-running background process with bounded scheduled handlers sharing the application services and durable PostgreSQL inbox/outbox. This is a change to execution and deployment, not a requirement to split tasks, offers and billing into independently owned services.

Selected app/database direction: Cloudflare web/API Worker -> Hyperdrive -> paid Render PostgreSQL in Frankfurt; scheduled Worker handlers -> the same database. Private Cloudflare R2 photo storage with explicit EU jurisdiction and authorized Worker reads is confirmed. GDPR/AVG is an explicit project-wide requirement; satisfy [NFR-09 privacy evidence](09-security-privacy-and-trust.md#8-compliance-launch-checklist) before real personal-data use, reviewing global processing and Resend US account data separately from EU object/database storage. The founder confirmed Render after the Supabase comparison; see [ADR 0009](../docs/adr/0009-use-render-postgresql-in-frankfurt.md). Supabase and its optional Auth/Storage remain unselected alternatives. Mollie remains the sole payment provider.

## Comparison

| Concern | Earlier Render proposal | Cloudflare Workers proposal |
|---|---|---|
| Web runtime | Standard Next.js on a persistent Node.js web service | Confirmed Hono API and React Router SSR on Workers; dependency/build qualification needed |
| Background execution | Continuously running Node worker polls PostgreSQL | Scheduled handlers claim and process bounded batches from PostgreSQL; no infinite polling loop |
| Database | Render PostgreSQL can share the app's private network | Selected Render PostgreSQL Frankfurt through Hyperdrive using the external TLS endpoint; network/firewall configuration is a separate boundary |
| Business transactions | SQL locks, unique effects and a durable outbox | Same SQL model; transaction-scoped connections and query caching disabled for application state |
| Private photos | Separate EU object service and ordinary Node-compatible processing | Confirmed private R2 EU bucket with authorized Worker reads and JPEG/PNG/WebP/iPhone HEIC input; confirmed Cloudflare Images Workers binding for upload-time sanitized JPEG/WebP variants, with runtime/privacy qualification required |
| Location | Web, jobs and database can be colocated in Frankfurt | Global application execution by default; EU database/object placement must be configured separately |
| Operations | Fewer runtime adaptations; pay for running web and worker services | Fewer always-running application processes; more platform-specific build, scheduling and connection settings |
| Overall fit | Simpler implementation baseline for the original persistent-process design | Viable for this pilot after runtime and recovery qualification; follows the founder's hosting preference |

The frontend choice is confirmed as React Router framework mode with React/Vite/SSR; see [ADR 0010](../docs/adr/0010-use-react-router-for-web-pwa.md). Cloudflare documents a [Workers integration](https://developers.cloudflare.com/workers/framework-guides/web-apps/react-router/). Qualify its build and request dispatch alongside the confirmed Hono API. SPA mode and prerendering are currently unsupported by that framework's Cloudflare Vite integration.

For the considered Next.js alternative, Cloudflare currently recommends **vinext**; its guide describes a Vite reimplementation of the Next.js API surface and still labels it beta. OpenNext is a separate adapter. Sources: [Next.js guidance](https://developers.cloudflare.com/workers/framework-guides/web-apps/nextjs/), [OpenNext guidance](https://developers.cloudflare.com/workers/framework-guides/web-apps/opennext/). Neither is selected, and no framework build has run.

## Application correctness on Workers

Keep known Mollie notification intake durable: commit the minimal PostgreSQL inbox/outbox work before acknowledging it. Scheduled handlers recheck and claim due work with leases, perform external calls outside domain locks, and preserve existing financial uniqueness/reconciliation. The same approach handles instant-alert delivery, daily digests, refund submission and erasure. An optional fast wake-up or future queue cannot replace the committed outbox.

Workers' `waitUntil()` only extends request-associated work for up to 30 seconds after response/disconnect. Paid Cron invocations scheduled more frequently than hourly have a 30-second CPU budget and a 15-minute wall-time limit. Use small resumable batches and lease recovery, with enough capacity and monitoring for the confirmed refund/purge targets. The founder confirmed PostgreSQL-backed jobs with Cron and an initial one-minute recovery poll, plus optional bounded immediate attempts after durable commit; the cadence is not an email-arrival guarantee. Exact lease/concurrency/backoff and backlog capacity still need qualification. See [Workers limits](https://developers.cloudflare.com/workers/platform/limits/).

Hyperdrive pools connections; it does not host PostgreSQL. Its query cache is on by default. Disable it for the application binding so task expiry, ownership, callback revocation, withdrawn offers, payment state and reads after writes use current database facts. Use explicit SQL transactions without session-state assumptions. Sources: [query caching](https://developers.cloudflare.com/hyperdrive/concepts/query-caching/), [connection lifecycle](https://developers.cloudflare.com/hyperdrive/concepts/connection-lifecycle/).

For the selected Render database, configure Hyperdrive with the external hostname and TLS, use least-privilege server credentials and review Cloudflare source-IP restrictions plus migration/administration access. Render's private URL is not directly available to Workers. Sources: [Render connections](https://render.com/docs/postgresql-creating-connecting), [Hyperdrive firewall configuration](https://developers.cloudflare.com/hyperdrive/configuration/firewall-and-networking-configuration/).

For the unselected Supabase alternative, Cloudflare recommends Hyperdrive's origin use the database's **direct connection**, not a Supabase pooled URL. See [Supabase integration](https://developers.cloudflare.com/hyperdrive/examples/connect-to-postgres/postgres-database-providers/supabase/). This is comparison context, not an additional database deployment.

## Storage and processing location

For R2, select the **EU jurisdiction** explicitly; a Western Europe location hint is only a performance preference. Objects remain private and application-authorized; CDN/service-worker caching cannot override task access deadlines. Media implementation must prove MIME/pixel checks, metadata stripping, quarantine and complete object erasure; ordinary framework image optimization does not prove these controls. See [R2 data location](https://developers.cloudflare.com/r2/reference/data-location/).

EU primary storage remains the existing requirement. It does not imply all Workers processing occurs in the EU. Review processing/logging/support transfers under the existing processor review; if EU-only execution becomes a requirement, qualify Cloudflare's Regional Services and its commercial terms separately. See [Workers regional processing](https://developers.cloudflare.com/data-localization/how-to/workers/). No new legal classification is inferred here.

## Indicative monthly cost

The founder confirmed a **€100/month pilot infrastructure planning budget excluding VAT** on 22 October, covering the selected services, required backups/basic monitoring and necessary staging/test usage. Human work, domain registration, payment fees/refund reserve, advertising and future AI hosting are separate. Exact service quotes and provider billing controls remain work; exact Render/Resend plans, measured capacity, metered usage and recovery/privacy qualification still determine the real cost. The lower-bound comparisons below are not an estimate of that complete deployment.


Public USD list prices checked 22 October; these are small-instance illustrations, not capacity recommendations or a production quote.

| Configuration | Illustrative starting cost | What is excluded |
|---|---:|---|
| Earlier Render web + worker + smallest paid PostgreSQL | $7 + $7 + $6 = **$20/month** | Workspace upgrades, usage, larger instances, media, mail, taxes and other services |
| Workers Paid + same smallest Render PostgreSQL | $5 + $6 = **$11/month** | Usage, media/mail, larger database, workspace requirements and taxes; network configuration differs from the Render baseline |
| Workers Paid + Supabase Pro | $5 + $25 = **$30/month before recovery add-ons** | Additional compute, PITR, usage and other services; daily-only recovery does not meet the existing <=1h recovery-point target |

Sources: [Render compute pricing](https://render.com/pricing), [Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/), [Supabase pricing](https://supabase.com/pricing). Hyperdrive pooling is included in Workers Paid; the database remains separately billed. See [Hyperdrive pricing](https://developers.cloudflare.com/hyperdrive/platform/pricing/).

Supabase PITR starts around $100/month extra and requires at least Small compute, so $30 is not the cost of the proposed recovery-qualified Supabase deployment. Render includes PITR on paid databases, but neither provider's feature list proves application restore time or deletion-safe recovery. Sources: [Supabase backup prerequisites](https://supabase.com/docs/guides/platform/backups), [Render recovery](https://render.com/docs/postgresql-backups). Keep the existing <=1h RPO/<=4h RTO and <=30-day backup boundary until explicitly revised; do not weaken them to match an entry price.

## Plan changes before implementation

The first foundation deliverable is a small qualification slice: chosen SSR/framework build in the Workers runtime, company/guest session checks, real PostgreSQL transaction/rollback and Hyperdrive cache settings, durable Mollie intake, resumable scheduled jobs, and one private sanitized image path. Test preview/deployed-runtime behavior as well as local development. This planning update does not authorize provisioning or live payments.

Then re-estimate the existing delivery slices for the chosen deployment. Retain all accepted product, fee, privacy, contact, alert and retention rules. Render database compute/storage/version, exact Better Auth integration and staff provisioning/exceptional-recovery procedures, Resend sender/plan, media parameters, actual service costs within the confirmed infrastructure budget and exact runtime configuration remain to be qualified; app hosting, database provider/region, private R2 EU photo storage and the Cloudflare Images Workers binding are settled.
