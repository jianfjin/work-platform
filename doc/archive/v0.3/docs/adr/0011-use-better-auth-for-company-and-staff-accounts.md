---
status: accepted
---

# Use Better Auth for company and staff accounts

On 22 October 2026 the founder confirmed Better Auth with account/session records in Render PostgreSQL, fitting the TypeScript/Hono stack and keeping identity records in the application database. Companies use verified email/password with verification and password recovery; staff use invite-only email/password with mandatory authenticator-app TOTP and one-use recovery codes, and password recovery preserves MFA. Guest clients retain separate task-scoped email access without accounts or automatic task ownership through company/staff identity; exact versions, runtime integration and operating provisioning/exceptional-recovery procedures remain work; see [technical design](../../doc/05-technical-design.md).
