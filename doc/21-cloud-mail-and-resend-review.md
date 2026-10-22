# Existing cloud-mail and Resend integration

**v0.4 scope note:** The selected technology/provider findings below remain reference material. Current product scope is the [semi-public-benefit three-board free/subscription redesign](22-platform-redesign.md); v0.3 €1 publication flows and their estimates are historical. Do not infer current billing, unlimited free reading or task-only MVP scope from comparison/inspection history.

**Reviewed:** 22 October 2026. **Status:** inspection complete; the founder subsequently selected Resend for direct platform sending with application-specific credentials; see [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md). A dedicated sending subdomain is also confirmed; the parent domain, subdomain label, sender/Reply-To addresses, plan, credentials and integration remain work. No email was sent and no cloud-mail code or deployment was changed.

## Verified existing setup

The founder supplied the [production cloud-mail dashboard](https://dash.cloudflare.com/52fdf946227d32558d000f6208377c87/workers/services/view/cloud-mail/production) and [Resend introduction](https://resend.com/docs/introduction). The dashboard page was not directly readable through web browsing; inspection used authenticated Cloudflare API reads for the explicit account in that URL, the local cloud-mail checkout and a read-only Resend domain lookup.

| Evidence | Observation |
|---|---|
| Production deployment API | Latest listed deployment is 8 October 2026 at 20:55 UTC, with version c0dd4d5a-e7b4-44e8-b25e-8adc9f8ca6ad at 100% |
| Live Worker settings | cloud-mail has D1, R2, KV, static assets and AI bindings; no email-sending binding is present |
| Runtime-authoritative KV settings | Sending is enabled and a Resend credential is configured for datawego.nl; credential values were not displayed or copied into this repository |
| Resend Domains API | datawego.nl is verified and configured for eu-west-1 sending |
| Downloaded deployed JavaScript | Resend send path, JSON webhook handler and no-op mailing-list queue consumer match the relevant local implementation; SHA-256: 96d12add44ee24e2680806d099902bcb67c590e1d78958697a631289c3749711 |

These reads establish configured integration, not a new end-to-end delivery test, a capacity result or approval to use the DataWeGo sender for this platform. No private mailbox contents were queried.

The reference checkout is /home/jianfeng/projects/cloud-mail. Relevant source references below are relative to that repository:

- mail-worker/src/service/email-service.js:368 chooses the sender domain and configured Resend token. External sending prefers a Cloudflare email binding when present; production has none, so the configured Resend path applies.
- mail-worker/src/service/email-service.js:575 builds the Resend request and calls the SDK. The package declares resend ^6.4.1 and Hono 4.12.16; these are reference-project dependencies, not version selections for work-platform.
- mail-worker/src/api/email-api.js:58 exposes the mailbox send action. mail-worker/src/security/security.js requires mailbox authentication/permissions for that action; it is not a dedicated platform notification API.
- mail-worker/src/service/setting-service.js:23 reads the runtime settings from KV. The Resend credentials are per-domain settings rather than an existing RESEND_API_KEY binding to copy into a new app.
- docs/adr/0002-in-place-rebrand-to-datawego.md records the DataWeGo cutover and retention of the Resend path. Its decision belongs to cloud-mail, not this platform's identity or task-retention model.

## Selected integration direction

The founder selected Resend after this review; use the existing provider experience/account as the integration starting point instead of introducing Brevo. Resend documents both [Cloudflare Workers sending](https://resend.com/docs/send-with-cloudflare-workers) and [Better Auth email hooks](https://resend.com/docs/send-with-better-auth).

Use a small server-only Resend adapter inside work-platform, with an application-specific sending credential and the confirmed dedicated sending-subdomain pattern. Choose the parent domain, subdomain label and sender address after platform naming; `notifications@notify.<platform-domain>` is illustrative. Verify the selected subdomain and configure its required DNS authentication before activation. Store the credential in Workers secrets. Keep PostgreSQL authoritative for notification obligations, task-alert subscriptions, delivery identity and eligibility. Bounded job handlers send due mail; delivery events update the platform's own records. Guest task links remain distinct from company/staff identity, and necessary account mail remains distinct from opt-in task alerts.

This reuses the email provider and operating experience. It does not require mailbox-user sessions, a second D1 database for platform state or copies of guest credentials/task content in cloud-mail's ordinary sent-mail archive. Human support replies could use an approved mailbox separately; the support/Reply-To mailbox, parent domain, subdomain label and sender address remain open.

## Adaptation required before reuse

1. **Durability and retries:** the inspected send method calls Resend before saving its sent-mail row and supplies no application idempotency key. For platform notifications, retain the durable PostgreSQL outbox, stable delivery identity and immutable request payload/key. Resend's [idempotency window is 24 hours](https://resend.com/docs/dashboard/emails/idempotency-keys); local history must outlive it. An ambiguous result after that window needs an explicit reconciliation/exception policy rather than blind resend or a claim of permanent exactly-once delivery. Expired access links and stale task alerts must be suppressed before dispatch.
2. **Delivery-event trust:** the inspected /webhooks handler parses JSON without verifying a signature, and the authentication middleware excludes that path. The platform implementation should verify the raw body and Resend signature headers, then durably deduplicate/process events; see [webhook verification](https://resend.com/docs/webhooks/verify-webhooks-requests). Resend acceptance, recipient-server delivery and a human reading the message are different states. No forged webhook was sent during this review.
3. **Queue reuse:** mail-worker/src/index.js:33 acknowledges and drops mailing-list queue messages, as documented by cloud-mail's ADR 0001. It is not a reusable durable notification consumer. Keep work-platform's PostgreSQL outbox and bounded handlers; changing the existing service or its consumer registration would be separate work.
4. **Purpose and retention:** qualify sender authentication, actual quota/budget, bounce/complaint suppression, alert unsubscribe behavior, token-safe logging and message/log/backup retention. Disable unnecessary tracking for access links. Generic SDK or auth-hook examples do not replace the platform's durability and privacy requirements.

These are reuse boundaries discovered by inspection, not fixes applied to the existing service.

## Storage-location tradeoff

The verified domain sends from Ireland, but Resend states that sending-region selection does not control storage: account data, including metadata, logs and API records, is stored in the United States. See [Resend regions and residency](https://resend.com/docs/dashboard/domains/regions). Do not describe the email integration as EU-only storage because eu-west-1 is configured.

The application database and confirmed private EU-jurisdiction R2 photo storage retain their EU placement requirements. The founder explicitly reaffirmed project-wide GDPR/AVG compliance on 22 October; complete the [NFR-09 vendor/transfer evidence](09-security-privacy-and-trust.md#8-compliance-launch-checklist) before sending real personal data through Resend. Provider selection does not waive that gate. Resend introduces a separate processor-storage/transfer boundary to review before activation; selecting it must not silently reinterpret the existing privacy requirements. If all provider-held email records must remain in the EU, the standard documented Resend service does not establish that property; Brevo remains the previously researched alternative. No legal clearance, contract, exception or new retention period is inferred from the existing DataWeGo setup.

## Sending identity decision

Resend and a dedicated sending subdomain are selected for direct platform sending with application-specific credentials. Finalize the parent domain, subdomain label and sender/Reply-To addresses after platform naming; the existing datawego.nl verification is reference evidence, not a platform branding decision. No domain was registered and no DNS or existing sender configuration was changed. The documented US account-data boundary remains part of processor review; provider selection does not constitute a completed integration, deployment or delivery test.
