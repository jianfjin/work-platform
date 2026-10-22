---
status: accepted
---

# Use Resend for platform email

On 22 October 2026 the founder selected Resend after inspection of the existing cloud-mail deployment, reusing that provider experience with the TypeScript/Workers and Better Auth stack instead of introducing Brevo. Use a direct server-only platform adapter with application-specific sending credentials and a dedicated sending subdomain, retaining the PostgreSQL outbox, stable delivery identity and verified delivery events. The documented US account-data storage boundary remains part of processor/retention review while application storage stays in the EU; the parent domain, subdomain label, sender address, exact plan, credentials and runtime qualification remain work, with no new resources provisioned or messages sent; see [the integration review](../../doc/21-cloud-mail-and-resend-review.md).
