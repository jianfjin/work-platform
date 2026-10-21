# Security, privacy and intermediary responsibilities
**Version:** 0.3 · **Updated:** 21 October 2026

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
Default CLIENT_INITIATED discloses no client contacts to companies. ALLOW_RESPONDERS requires a clear separate action naming fields and audience: every eligible currently published responder, with no fixed three-firm cap. Phone optional if email-only callbacks selected. Do not hide consent in platform terms or preselect it because a user entered a phone.

Every disclosure checks current task state/deadline, own valid response, account status and allowed fields. Close, withdrawal, suspension or revocation blocks future display; previously copied contact details cannot be recalled. Full postcode/street address never enters the standard company task view. Client can share an address directly for a site visit.

No raw contacts in notifications, analytics, search indexes, PSP metadata, service-worker caches or export to other companies. Image metadata removal is necessary but cannot detect every visible house number/face; warn before posting and support rapid removal.

## 4. Three-month deletion and exceptions
Published job content, photos/versions, rough offers and their contact snapshots, full postcode/client contacts, derived task coordinates, consent grants and guest credentials have an immutable deadline: first publication + three calendar months. Closing/editing/registering later does not extend it.

At the deadline all reads/downloads/callback/alert actions deny access. Proposed live primary/object purge completes within 24 hours with retries/alerts; backups roll off within 30 days and cannot be restored into service without applying tombstones. State these technical timings honestly rather than promising instantaneous physical erasure everywhere.

Exceptions require separate purpose, access and deletion schedule:
- Minimal platform-fee accounting/billing/payment evidence, classified by accountant. [Belastingdienst](https://www.belastingdienst.nl/wps/wcm/connect/bldcontentnl/belastingdienst/zakelijk/ondernemen/administratie/) describes basic-record retention generally lasting years.
- Active company account/profile/opt-in alert preferences while needed for service; no automatic preservation of old task details through that account.
- Specific legal/complaint evidence only when necessary, minimal, restricted, approved and review-dated.

No blanket “keep all data for disputes” or analytics exception. On early erasure request deny access promptly, resolve financial obligations separately and delete eligible content. We cannot delete recipients' screenshots/external email copies or promises in contractors' own systems; minimize what gets forwarded.

## 5. Guest and payment security
Guest links are short-lived one-use credentials stored hashed, purpose/job-scoped, rate-limited and excluded from logs/referrers. Optional account association proves ownership. Never expose all jobs from an unverified email lookup. Staff MFA and scoped permissions do not authorize unrestricted browsing of offers/contact.

Company €1 publication requires authenticated PSP retrieval, amount/currency/merchant/environment validation, one fulfillment and refund-safe races. Fake callbacks, duplicate captures and delayed paid-after-close outcomes cannot expose an unpaid/invalid response. Hosted checkout avoids handling payment credentials ourselves. For the Stripe candidate, enforce raw-body `Stripe-Signature` verification before webhook ingestion, deduplicate event and business effects separately, and retrieve current owned payment state before publication. Store server-only Stripe API/signing secrets in the deployment secret manager with separate test/live configuration, least privilege and rotation; never expose them in PWA assets or logs. Processor review must cover Stripe's actual processing/subprocessor/transfer terms, not infer EU-only processing from the application's EU database.

## 6. Content/report operations, not construction monitoring
Automated type/scan/privacy/rate checks and exception handling are required even without routine manual approval. Reports may concern misleading offers, harassment, privacy, duplicate/fake jobs or unlawful content. Record case, evidence minimum, decision/reason, notice and correction/appeal. Urgent data exposure can trigger a temporary hold; a complaint is not proof of guilt.

After reasoned review, issue proportionate factual warnings/removal/suspension where justified and lawful. Do not automatically publish unsubstantiated accusations or a “bad company” list. Clients remain responsible for evaluating providers and agreeing scope/prices directly; the platform handles its own fee/service complaints and reports, not the building contract as arbitrator.

## 7. Alerts and promotion
Company job-alert subscriptions are affirmative, recipient-confirmed, category/radius/frequency controlled and easy to stop. Do not scrape business websites and automatically email every posted job to those addresses. [ACM ConsuWijzer spam information](https://consument.acm.nl/veilig-online/hoe-voorkom-ik-spam) provides first-party background; legal reviews the precise categories/permissions, while our product defaults to opt-in. Research access limitations are recorded in the source register.

Unsubscribe/close/deletion is checked before dispatch; an already accepted external email cannot be recalled. Use authenticated sending, bounce suppression and no sensitive body/media. Promotional newsletters/ads are separate from security/account service emails. Non-essential tracking/cookies need appropriate consent and disclosure; no session replay on guest links, offers or billing.

## 8. Compliance launch checklist
- Determine controller/legal operator, privacy notice, processing purposes/lawful bases, processor/transfer contracts and rights handling under [GDPR](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679).
- Classify DSA/trader disclosure duties and any P2B/DAC7 scope under current law; no automatic exemption merely because construction payments are external.
- Accountant approves platform fee VAT, invoice/correction, refund and retention schedules. Merchant PSP onboarding is required; not a contractor-quality service.
- Geocoding provider terms/privacy/billing/attribution approved. Google result storage restrictions apply separately from user privacy deletion.
- Exact three-month interpretation/backup limits, callback audience wording and guest email requirement approved and translated.
- Disclaimers reviewed for actual role and non-waivable duties; no claim of zero responsibility for our own misconduct/data/payment operations.

## 9. Operational security
Encrypt transport/storage, use managed secrets and isolated environments, restrict DB/media access and keep logs redacted/purpose-limited. PWA only caches public allowlisted static assets; logout/resume/account switch revalidate, no private durable draft or offline write queue.

Critical incidents: rival/client data leakage, consent bypass, duplicate retained fee, unpaid visible offer or missed deletion access deadline. Stop affected exposure/purchases, preserve minimal evidence, assess notification obligations, reconcile/fix/test and record reopening approval. Test backups with tombstone/PSP replay before paid beta.
