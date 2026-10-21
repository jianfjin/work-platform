# Product requirements document
**Version:** 0.3 · **Date:** 21 October 2026 · **Status:** revised design; not implemented

## 1. Authoritative interpretation
Client posting and comparison are **free**. Companies fund the platform. Recommended pilot: **€1.00 including VAT once per company/task when its first rough offer is successfully published to the client**. Reading tasks is free. This flat platform-response fee is not a percentage commission on a construction contract, and is not conditional on winning or completing the work. Founder must confirm the charge event before development.

The draft's client-€1 alternative is analyzed in [Payments](08-payments-and-economics.md), not enabled. No compulsory €10 wallet, three-company cap, KVK/workmanship certification, in-app contract/award system or construction-payment processing.

## 2. Users, region and scope
- Clients: adults authorized to request work, as secure guests or optional registered accounts.
- Companies: registered platform accounts with confirmed email ownership and self-declared business/service/contact details; one owner per company in MVP. This is not a workmanship or credential check.
- Operators: platform administration, content/report handling, platform-fee support, security and deletion; not building-site supervision.

Pilot labels are Groningen, Friesland/Fryslân and Drachten. Planning assumption: the two provinces, with Drachten explicitly prioritized; final municipality/postcode allowlist requires approval. Categories include carpentry, windows/doors, non-structural alterations, insulation, internal walls, painting and roof maintenance. No platform-issued engineering/safety advice or certification.

Dutch and English; desktop Web plus Android/iPhone browser and installed PWA. Native apps later.

## 3. Workflow and commercial rules
1. Client supplies short description, postcode/city, email and optional few photos; budget, timing and measurements are optional. Email confirmation enables publishing without an account/password.
2. Client reviews exactly what companies can see: city/PC4 area, description and sanitized photos. Full postcode, email, phone and street address are not part of that shared brief.
3. Company signs in, reads suitable open tasks free and drafts an introduction, rough bid/range or “site visit needed,” own website if available, email/phone and availability.
4. The first visible response requires explicit €1 checkout. One response per company/task, versioned edits without a second fee. Duplicate requests/callbacks must not double-charge. Payment failure publishes nothing; payment succeeds after close/delete/material scope change -> no publication and full platform-fee refund.
5. All eligible paid responses appear in the client's private paginated list. No fixed participant limit. Companies see only their own response and cannot retrieve competitor identities, prices, response counts or ordering information.
6. Default CLIENT_INITIATED: client contacts a company directly. Company contact fields are in its offer; client contact fields are not revealed.
7. Optional ALLOW_RESPONDERS: an explicit client choice permits currently published responding companies to view the selected client email/phone fields. Explain that the audience is every eligible responder, not only a winner and not a limited three firms. Client can switch back/revoke future disclosure; previously copied details cannot be recalled.
8. Street address/full postcode stays private; sharing it for a site visit is a direct client action outside the brief. No contact details in free description/photos.
9. Client may close at any time. Closing stops new responses, alert sends and future callback disclosure. Existing submitted offers remain client-readable until the deletion deadline unless withdrawn/removed; closure does not prove award/completion or automatically refund a valid earlier response.
10. The task's immutable deletion deadline is first publication plus **three calendar months**, using Europe/Amsterdam calendar arithmetic and UTC storage. Edit, close, new offer or account registration never resets it. Access is denied at deadline even if purge is delayed.
11. Companies edit/withdraw their own offer without a new fee. Withdrawal is visible as withdrawn to the client and ends callback access; it does not erase finance history or create another purchase opportunity.
12. No mandatory on-platform negotiation, site visit booking, final agreement, construction invoice/payment, performance monitoring or work-success verification.

## 4. Functional requirements
| ID | Requirement | Acceptance criteria |
|---|---|---|
| FR-01 | Guest ownership and optional account | No required client account/password; email-confirmed scoped job-management session; resend/recovery generic and rate limited; optional account linking proves ownership |
| FR-02 | Lightweight job and photos | Description/postcode/city/email; optional photos and timing/budget; sanitized company preview; postcode-city validation; no publication outside approved region |
| FR-03 | Company self-registration | Confirmed login email, declared name/services/base area/radius/contact details and optional website; no “quality verified” claims; suspend abusive accounts |
| FR-04 | Free task reading and geographic fit | Registered active companies see safe open task brief/media free; category/radius filtering; approximate distance explained; full postcode/contact never leaked |
| FR-05 | Private rough offers | Introduction, own contacts, website, indicative amount/range or site-visit-needed; client sees all live eligible responses with pagination; company sees only its own, not rivals/count |
| FR-06 | Company-paid first publication | Explicit €1 gross confirmation; one successful fee per company/task; first visibility only after server-verified paid state; edits/read/retries are not charged |
| FR-07 | Client contact preference | Default client-initiated; explicit selected-field responder callback consent; disclosure checks response/job/consent every time; revocation/close stops future access |
| FR-08 | Client close and task lifecycle | Close whenever desired; no new response or alert after close; paid-after-close compensates; no inferred award, completion or construction payment |
| FR-09 | Opt-in matched email alerts | Company category/radius/frequency settings, confirmed recipient and unsubscribe; notifications contain only safe metadata and an authenticated link; stale/closed/deleted tasks suppressed |
| FR-10 | Three-month task deletion | Immutable calendar deadline; job text/media/offers/contact grants/guest tokens and derived task location deleted; live access stops at deadline; auditable purge/retry |
| FR-11 | Feedback and misbehavior handling | Report intake, evidence/reason/status, safety hold and appeal/correction; no automatic defamatory public badge or claimed construction investigation |
| FR-12 | Minimal operations | Staff role separation, fee refund/reconciliation, moderation/report queue, mail deliverability, deletion health and platform support; no routine workmanship checks |
| FR-13 | Security and scoped data access | Hash one-use guest tokens; server-side ownership and company/response ACL; clean media; safe logs; finance effects deduplicated; no private offline cache |
| FR-14 | Web/PWA parity | Both client/company flows usable on desktop, Android and iPhone browser/home-screen modes; optional installation and safe checkout return; native apps deferred |
| FR-15 | Dutch/English | Core forms, legal/fee/contact/deletion copy and transactional mail localized; user task text not silently auto-translated |
| FR-16 | Responsible low-cost acquisition | Generic region/category landing pages; no private job SEO index or scraped email blast; separately controlled advertising budget and honest attribution |
| FR-17 | Platform-fee accounting and refunds | Operator-owned merchant/settlement configuration; paid/publication/refund audit; refund failed fulfillment once; statutory minimal finance separated from task deletion |
| FR-18 | Scope/version safety | Material job edits version and notify; pending paid offer on old material scope refunds instead of silently publishing; paid visible offers get a scope-change label, not an automatic re-charge |

## 5. States
Task: DRAFT -> EMAIL_PENDING -> OPEN -> CLOSED; safety hold SUSPENDED can restrict it; at delete_at it becomes EXPIRED_FOR_ACCESS -> PURGED. Client may resume a voluntary CLOSED task before delete_at without extending the clock; no additional per-company fee. Suspended tasks need an operator decision. No old 14-day commercial expiry or recurring three-day reconfirmation rule.

Response: DRAFT -> PAYMENT_PENDING -> PUBLISHED -> WITHDRAWN/REMOVED; purge deletes content. Keep an opaque finance-effect key, not the proposal text, where lawfully required. A refunded failed response cannot be re-published on that same payment; a deliberate new attempt needs an explicitly confirmed new intent and must not yield two retained successful fees.

## 6. Retention contract
At delete_at, all reads, downloads, consent grants and new sends stop. Proposed physical live-store/object-version purge completes within 24 hours; retry/alert on failure. Backups are isolated, inaccessible to ordinary users and age out within 30 days; restore reapplies deletion tombstones before serving.

Minimum platform-fee tax/accounting records, active company profiles/alert settings and strictly justified legal evidence have separate schedules. Disclose these exceptions; do not promise all records disappear at three months. No retained task photos/text under a blanket “analytics” excuse. External emails/screenshots/contact copies are outside remote recall.

## 7. Non-functional requirements
| ID | Target / verification |
|---|---|
| NFR-01 | Zero tolerated duplicate retained successful company/task fee, payment without fulfillment/refund resolution, cross-client/rival-offer exposure or consent bypass |
| NFR-02 | Deadline access denial independent of worker; purge target <=24h; backup roll-off <=30 days; month-end/DST/restore tests |
| NFR-03 | Initial p95 safe reads <500ms, local publish transaction <1s excluding PSP/network, at 100 concurrent active users and 10,000 tasks; test, not SLA |
| NFR-04 | WCAG 2.2 AA target, 320px reflow, keyboard/screen reader; Dutch/English fee/contact/deletion comprehension |
| NFR-05 | Recorded real Android/iPhone browser+installed checkout/lifecycle tests; no private PWA persistence or offline write replay |
| NFR-06 | Durable alert delivery/deduplication; close/unsubscribe rechecked before sending; no reply-time or mail-delivery guarantee |
| NFR-07 | EU primary storage with processor/transfer review, secure sessions/secrets, minimal telemetry; RPO <=1h/RTO <=4h tested with PSP reconciliation |
| NFR-08 | Monthly 99.5% pilot availability target; price/payment/consent/retention changes versioned; alerts for financial/deletion/privacy failures |

## 8. Metrics and non-goals
Measure posting success, relevant response rate, fee conversion, client contact clicks, opt-out/complaint rates, refund incidents, deletion lag and cost per eligible job. Label site visits/awards/completion as self-reported if voluntarily supplied. Not required: public reviews, chat, shared Q&A, escrow, subscriptions, full offline use, automatic contractor verification or paid ranking.
