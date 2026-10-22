---
status: accepted
---

# Use Mollie as the sole payment provider for the current stage

On 22 October 2026 the founder explicitly selected Mollie and excluded Stripe from consideration at the current stage, including fallback evaluation. Use Mollie-hosted checkout through the Payments API for the confirmed individual €1 platform-publication fee, keeping the existing publication, refund, privacy and retention rules. One provider integration limits implementation and operating work; any later provider change needs a separate decision.

The founder also confirmed iDEAL/Wero and cards as the MVP method targets on 22 October 2026. All other payment methods are outside MVP. Enable each target only after merchant approval and successful €1 payment, full-refund and phone/PWA return tests. This sets product scope; actual profile eligibility, current method identifiers, fees and activation evidence remain launch prerequisites.

The design uses classic payment-ID webhooks followed by authenticated resource retrieval, durable local financial uniqueness, and reconciliation beyond Mollie's one-hour credential-scoped idempotency cache. A server-maintained allowlist restricts hosted checkout to approved, validated targets. Merchant/profile activation and a funded refund reserve remain launch prerequisites. No account, API key, SDK installation or live charge is created by this planning decision.
