# Security, privacy and intermediary responsibilities
**Version:** 0.3 · **Updated:** 22 October 2026

Cloudflare is the requested application host; see [the deployment comparison](18-cloudflare-vs-render.md). On 22 October the founder confirmed private Cloudflare R2 photo storage with explicit EU jurisdiction and authorized Worker endpoints, and explicitly required GDPR/AVG compliance throughout the project. Extend private-cache checks to Hyperdrive and Cloudflare CDN/framework layers, retain server-owned authorization, and review global processing/logging separately from EU database/object storage. The founder also confirmed JPEG/PNG/WebP/iPhone HEIC input with sanitized JPEG/WebP display; media sanitization and complete erasure still require qualification. EU storage is one control, not evidence of overall compliance; no processor clearance, implementation or deployment is claimed.

## 1. Information platform does not mean no platform duties
The platform connects parties and processes only its own company response fee. It does not certify workmanship, inspect sites, supervise work, negotiate final prices, hold construction money or guarantee identity/performance/payment between parties.

Terms must describe that actual role, not say every legal responsibility disappears. Protect our own data/payment systems, handle illegal-content/privacy reports, administer platform-fee refunds and determine applicable trader/platform duties before paid launch. Required legal identity checks, if applicable, are distinct from optional quality certification. [Commission DSA guidance](https://digital-strategy.ec.europa.eu/en/policies/digital-services-act) describes role/size-dependent rules; legal must classify this concrete service.

## 2. Data minimization and access
| Data/action | Public | Active company | Job client/guest owner | Scoped staff |
|---|---|---|---|---|
| Generic region/trade information | Read | Read | Read | Read |
| Live safe task/approved photos | No index/feed | Free authenticated read before deadline | Own | Case/operation scope |
| Full postcode/client email/phone | No | Only selected callback fields under current permission; no full postcode | Own | Necessary case only |
| All job responses/companies' offers/contacts | No | No; own response only | Own task private list | Case-scoped, audited |
| Competitor participation/count | No | No | Own job list as needed | Necessary case only |
| Company fee/receipt | No | Own | No client billing flow | Finance scope |
| Guest management token | No | No | One-use exchange only | Never raw token lookup |
| Task past delete_at | No | No | No content | Minimal authorized legal/finance exception only |

Company websites/contact details are self-submitted with the offer and shown to that client. They are not a guarantee that the company is qualified or trustworthy. A checked email means email control only. No quality badges, invented reviews or completed-work claims.

## 3. Client contact choice
The founder confirmed the contact model on 21 October 2026; see [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md). Default CLIENT_INITIATED discloses no client contacts to companies. ALLOW_RESPONDERS requires a clear separate action naming selected email/phone fields and the audience: every eligible currently published responder while the task is open, with no individual company selection or fixed cap. Phone is optional if email-only callbacks are selected. Do not hide consent in platform terms or preselect it because a user entered a phone.

Every disclosure checks current task state/deadline, own valid response, account status and allowed fields. Close, withdrawal, suspension or revocation blocks future display; previously copied contact details cannot be recalled. Full postcode/street address never enters the standard company task view. Client can share an address directly for a site visit.

Confirmed: reopening a voluntarily closed task resets contacts to private and requires a fresh explicit callback choice; the previous permission does not resume automatically. Advance consent version to reject stale permission writes and keep canceled payment attempts invalid even after reopening. See [ADR 0006](../docs/adr/0006-reopen-without-renewing-fees-or-consent.md).

No raw contacts in notifications, analytics, search indexes, PSP metadata, service-worker caches or export to other companies. Image metadata removal is necessary but cannot detect every visible house number/face; warn before posting and support rapid removal.

## 4. Three-month deletion and exceptions
Confirmed on 21 October 2026: published task content, photos/versions, rough offers and their contact snapshots, full postcode/client contacts, derived task coordinates, consent grants and guest credentials have an immutable expiry three calendar months after the task first becomes visible to companies. Closing/reopening/editing/registering later does not extend it; paid offer publication neither starts nor resets it. See [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md).

Confirmed: at the deadline all reads/downloads/callback/alert actions immediately deny access; live primary data and every photo/object version are purged within 24 hours with retries/alerts. Isolated backups roll off within 30 days and cannot be restored into service without applying deletion records and expiry filters. Validate those records remain available independently of the restored snapshot. State these technical timings in the privacy notice and verify them before launch.

The founder confirmed these narrow exceptions, each requiring a separate purpose, restricted access as appropriate and a deletion schedule; exact fields, lawful bases, periods and start dates receive accountant/legal review before launch:
- Minimal platform-fee accounting/billing/payment evidence, classified by accountant. [Belastingdienst](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/) describes basic-record retention generally lasting years.
- Active company account/profile/opt-in alert preferences while needed for service; no automatic preservation of old task details through that account.
- Strictly necessary legal/complaint evidence only for a named case, with restricted access and a review/deletion date. Preserve the necessary evidence separately; the ordinary task still expires and cannot regain company/client access through the hold.

No blanket “keep all data for disputes” or analytics exception. On early erasure request deny access promptly, resolve financial obligations separately and delete eligible content. We cannot delete recipients' screenshots/external email copies or promises in contractors' own systems; minimize what gets forwarded.

## 5. Guest, account and payment security
Guest links are short-lived one-use credentials stored hashed, purpose/job-scoped, rate-limited and excluded from logs/referrers. Client registration, dashboards and account association are deferred until after MVP; company/staff accounts do not automatically own guest tasks. Any future account association must prove ownership. Never expose all jobs from an unverified email lookup. Staff MFA and scoped permissions do not authorize unrestricted browsing of offers/contact.

Confirmed company sign-in uses verified email and password, with email verification and password recovery through Better Auth. Require verified ownership and current company permissions; verification/reset credentials are short-lived, purpose-scoped and excluded from logs. Guest task access remains separate. Staff access is confirmed as invite-only email/password with mandatory authenticator-app TOTP and one-use recovery codes; password reset preserves MFA, and staff permissions require completed MFA. Initial staff provisioning and loss-of-all-factors recovery need a documented operating process.

Company €1 publication requires authenticated Mollie retrieval, exact amount/currency/profile/mode/attempt validation, unique fulfillment and refund-safe races. Classic webhook payment IDs are untrusted lookup hints, not signed payment proof; forged calls cannot change financial facts. Use known-payment lookup, bounded/rate-controlled ingestion, durable verification work and business-effect uniqueness; repeated payment IDs must still permit later refund/chargeback updates. Store server-only Mollie credentials in the secret manager with separate test/live configuration, least privilege and rotation; record only a non-secret credential version for recovery because provider idempotency is credential-scoped and lasts one hour. Never expose keys or raw personal payloads in PWA assets/logs. Processor review covers Mollie's actual processing/subprocessor/transfer terms rather than inferring them from the application's EU database. See [Mollie mapping](05-technical-design.md).

## 6. Content/report operations, not construction monitoring
Automated type/scan/privacy/rate checks and exception handling are required even without routine manual approval. Reports may concern misleading offers, harassment, privacy, duplicate/fake jobs or unlawful content. Record case, evidence minimum, decision/reason, notice and correction/appeal. Urgent data exposure can trigger a temporary hold; a complaint is not proof of guilt.

After reasoned review, issue proportionate factual warnings/removal/suspension where justified and lawful. Do not automatically publish unsubstantiated accusations or a “bad company” list. Clients remain responsible for evaluating providers and agreeing scope/prices directly; the platform handles its own fee/service complaints and reports, not the building contract as arbitrator.

## 7. Alerts and promotion
Company job-alert subscriptions are affirmative, recipient-confirmed, category/radius/frequency controlled and easy to stop. Do not scrape business websites and automatically email every posted job to those addresses. [ACM ConsuWijzer spam information](https://consument.acm.nl/veilig-online/hoe-voorkom-ik-spam) provides first-party background; legal reviews the precise categories/permissions, while our product defaults to opt-in. Research access limitations are recorded in the source register.

Unsubscribe/close/deletion is checked before dispatch; an already accepted external email cannot be recalled. Use authenticated sending, bounce suppression and no sensitive body/media. Promotional newsletters/ads are separate from security/account service emails. Non-essential tracking/cookies need appropriate consent and disclosure; no session replay on guest links, offers or billing.

## 8. Compliance launch checklist
GDPR/AVG compliance is a confirmed project requirement, tracked as NFR-09. The following evidence is required before processing real personal data in an alpha or beta, as well as for public launch; synthetic-data development can proceed. Maintain the evidence when purposes, vendors or features change. This document records delivery requirements, not a completed legal assessment.

| Area | Required project evidence | Owner |
|---|---|---|
| Accountability and purposes | Identify the legal operator/controller and privacy contact. Maintain a processing register and data-flow map covering guest/company/staff identity, task location/photos, contact grants, offers, fees, alerts, support cases, logs and backups; record necessary fields, purpose, lawful basis, recipients and retention for each. Review DPIA and DPO applicability, recording reasons and completing either where required. | Founder/privacy/legal |
| Privacy by design | Implement minimum collection, private contacts/offers/media, scoped staff MFA, encrypted transport/storage and redacted telemetry. Keep public R2 access disabled; every photo request checks current authorization and task expiry. Qualify sanitization and deletion of originals, variants and quarantine copies. Reuse AT-03/05/09/14/19 as evidence. | Tech/privacy |
| Transparency and choice | Publish reviewed Dutch/English notices before collection, including recipients/transfers and retention. Separate callback permission and alert opt-in from necessary service processing. Record the appropriate lawful basis per purpose; accepting terms or choosing an EU region is not blanket consent. Preserve withdrawal and required tracking/cookie controls. | Product/privacy/legal |
| Individual rights | Provide a usable request route without requiring a client account. Proportionately verify identity, scope access/export to the requester without leaking rival/third-party data, and handle correction, erasure, restriction, objection and applicable portability. Track receipt and the one-month response deadline; document valid extensions/refusals and communicate them in time. Record recipient/provider follow-up where required. | Support/privacy/tech |
| Vendors and transfers | Review the role of each service, processor contracts where applicable, subprocessors, processing/support/log/backup locations, deletion assistance and any international-transfer mechanism or assessment required. Cover Cloudflare/Hyperdrive/R2, Render, Resend, Mollie, the confirmed PDOK location service and any later media, analytics or AI service; do not assume all vendors act only as processors. | Privacy/legal/tech |
| Retention and recovery | Prove immediate task-expiry denial, <=24h live purge, <=30-day isolated backup roll-off and deletion-safe restore. Keep minimum billing and named-case evidence separately under reviewed schedules; early rights requests do not wait for ordinary task expiry, and justified exceptions need a recorded reason. Include provider copies and derived data in the inventory. | Tech/privacy/finance |
| Breach response | Name an accountable incident owner and backup, maintain a breach register, assess risk promptly, and drill containment and notification. Where required, notify the competent authority without undue delay and within 72 hours of awareness; notify affected people without undue delay where high risk requires it. Record the assessment even when notification is not required. | Security/privacy/operations |

The controls follow the EDPB's guidance on [accountability and privacy by design](https://www.edpb.europa.eu/sme/be-compliant/be-compliant_en), [controller/processor responsibilities](https://www.edpb.europa.eu/sme/learn-the-basics/data-controller-or-data-processor_en), [individual rights](https://www.edpb.europa.eu/sme/be-compliant/respect-individuals-rights_en) and [breach response](https://www.edpb.europa.eu/system/files/2025-03/edpb_summary_092022-012021_data_breach_en.pdf). Exact lawful bases, contracts, exceptions and required assessments remain review work.

Resend remains the selected provider, but its [documented US account-data storage](https://resend.com/docs/dashboard/domains/regions) must be covered by the vendor/transfer review before live personal-data sending. Ireland sending does not establish EU-only storage. Cloudflare's global Worker execution, support access and logging also need review independently of R2's EU jurisdiction. An unresolved required safeguard blocks the affected live processing; selecting a vendor does not waive this requirement. No particular transfer mechanism or contract approval is assumed.

Future AI customer service or knowledge/Wiki processing requires an updated purpose/data-flow/vendor review and DPIA screening before personal data is introduced. The current planning discussion does not authorize private-task indexing, model training or new automated decision authority.

Cloudflare Images through a Workers binding is confirmed for upload-time photo conversion; provider selection does not establish compliance readiness. Add the raw-photo-to-transformer flow, processing/temporary-copy/log locations, retention/deletion and applicable contract/subprocessor/transfer evidence to the register before live photos are processed. Private EU R2 storage does not extend its jurisdiction setting to the transformer. Serving remains behind current Worker authorization, with no public cache or raw-image fallback.

Other launch duties remain:
- Classify DSA/trader disclosure duties and any P2B/DAC7 scope under current law; no automatic exemption merely because construction payments are external.
- Accountant approves platform fee VAT, invoice/correction, refund and retention schedules. Merchant PSP onboarding is required; not a contractor-quality service.
- PDOK is the confirmed geocoder: minimum server-side queries, necessary coarse coordinates, postcode/province accuracy and provider terms/privacy/attribution must be qualified. The geocoder flow remains in the processing inventory; public source data does not make task-linked locations anonymous. Google remains unselected comparison context.
- Exact separate retention purposes/fields/schedules professionally reviewed; technical timezone/DST details validated; the confirmed narrow exceptions, task-expiry, 24h live purge/30-day backup limits, pre-publication email ownership, account-free recovery and contact audience/selected-field/revocation rules implemented, reviewed and translated.
- Disclaimers reviewed for actual role and non-waivable duties; no claim of zero responsibility for our own misconduct/data/payment operations.

## 9. Operational security
Encrypt transport/storage, use managed secrets and isolated environments, restrict DB/media access and keep logs redacted/purpose-limited. PWA only caches public allowlisted static assets; logout/resume/account switch revalidate, no private durable draft or offline write queue.

Critical incidents: rival/client data leakage, consent bypass, duplicate retained fee, unpaid visible offer or missed deletion access deadline. Stop affected exposure/purchases, preserve minimal evidence, assess notification obligations, reconcile/fix/test and record reopening approval. Test backups with tombstone/PSP replay before paid beta.
