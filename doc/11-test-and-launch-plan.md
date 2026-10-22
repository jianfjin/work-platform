# Acceptance and launch evidence
**Version:** 0.4 · **Updated:** 22 October 2026 · **Status:** planned cases; no application tests run

Version 0.4 revises the former fee-specific AT-06/07/18/20/26 meanings and adds three-board/allowance scenarios. Version 0.3 cases are [archived](archive/v0.3/doc/11-test-and-launch-plan.md), not simultaneously active requirements. Policy-dependent cases need fixed expected terms before they can qualify live behavior.

## Planned acceptance
| ID | Scope | Expected evidence |
|---|---|---|
| AT-01 | Guest ownership | Unconfirmed drafts stay private; one-use scoped confirmation/recovery; no account requirement or enumeration |
| AT-02 | Company/ZZP account | Verified email/password and own company authority; no claim of workmanship verification or guest/staff ownership |
| AT-03 | Safe task projection | Allowed details/photos omit full postcode/client contacts/credentials; R2 stays private and Worker checks current entitlement |
| AT-04 | Trades/region | New trade categories; whole Groningen/Fryslân client-task eligibility and approximate radius; invalid/ambiguous geocode stays draft |
| AT-05 | Private offers | Client sees own offers; companies see only own; no rival identities/count/order; withdrawn marker omits body/contacts |
| AT-06 | Free/subscription tiers | Only free and low-price subscription tiers; free maximum five items per approved month policy and active subscription no numerical item cap; authorized offer submission/publication/editing has no separate charge or checkout; no auto-charge after fifth item |
| AT-07 | Access replay | Same authorized company/item/period/operation does not double-spend under the chosen rule; mismatched payload/actor rejected |
| AT-08 | Task edit/close/reopen | Scope revisions/labels, original deadline, fresh callback opt-in on reopen, no stale event republishing |
| AT-09 | Client contact | Default private; exact opted-in fields only to eligible current responders; plan/payment alone never grants contacts |
| AT-10 | Alerts | Explicit opt-in/digest/instant/unsubscribe and shared logical identity; email/media links cannot reveal unentitled details or unexpected quota consumption |
| AT-11 | Task purge | Immediate deadline denial even with worker off; all ordinary task/live photo versions purge <=24h |
| AT-12 | Retention separation | Minimum finance, active company and named evidence purpose-separated; no task reconstruction from billing/quota/audit |
| AT-13 | Reports | Board-aware case/reason/hold/correction with minimal evidence; false reports do not publish accusations |
| AT-14 | Staff MFA/roles | Invite-only staff with required assurance; role-change/password-reset/recovery-code races preserve MFA and case/finance boundaries |
| AT-15 | PWA | No private offline cache/write replay or allowance-consuming prefetch; safe resume/update/account switch |
| AT-16 | Languages/accessibility | Dutch/English board/plan/quota/renewal/consent copy, keyboard/screen-reader/reflow and comprehension |
| AT-17 | Growth | Generic pages only, no private project/photo indexing or scraped automated outreach |
| AT-18 | Billing effects | Exact company/plan/amount/currency/environment/period; verified payment and unique effect; bounded refund/reversal; ambiguous create reconciled |
| AT-19 | Media | Six/10 MB client-task boundaries including concurrent slots; real type/pixels/HEIC, sanitized JPEG/WebP without identifying metadata; late/deleted-parent output purged |
| AT-20 | Payment return | Real Android/iPhone browser/PWA checkout recovery for selected membership policy; no activation from redirect or automatic second charge |
| AT-21 | Cross-device | Recover own state and allowance consistently; no requirement that cookies transfer or another company's grants become usable |
| AT-22 | Restore | Independent deletions/expiry before exposure, <=30-day ordinary backup roll-off, reconciled membership/financial effects, no free-counter reset exploit |
| AT-23 | Calendar policy | Original three-calendar-month client-task clamp; quota/billing periods tested separately with chosen timezone, boundaries and DST |
| AT-24 | No outcome guarantee | Subscription/interest/inquiry does not mean work award, subcontract, quality certification, sale or extra commission |
| AT-25 | Guest scope | Client account/dashboard/linking still deferred; company account grants no task ownership |
| AT-26 | Merchant/terms | Actual operator/merchant and approved selected price/tax/renewal/methods; no invented subscription price or unapproved recurring SEPA activation |
| AT-27 | GDPR rights | Usable verified request without forced client signup; scoped access/correction/erasure/objection, deadlines and evidence; expired allowance cannot block rights |
| AT-28 | Cooperation publication | Company publishes independent work packages/needs; current ownership and chosen geography/lifetime required |
| AT-29 | Source confidentiality | Link/copy/interest attempts never inherit original client contacts/photos/address/rival offers; parent deletion cannot be undone by package data |
| AT-30 | Trade listing | Item/category/condition/quantity/unit/price/area validated; no full pickup/home address by default |
| AT-31 | Trade lifecycle | Owner edit/remove/unavailable states, reports and chosen retention; no implicit cart/escrow or settled-money claim |
| AT-32 | Mixed-board allowance | Under the proposed shared pool, two tasks + two packages + one trade item consume five total; per-board/account workarounds denied; revise if founder chooses another pool |
| AT-33 | Fifth/sixth race | Independent DB connections/tabs/API/media paths cannot exceed the chosen five-item cap; forbidden/expired/prefetch reads consume zero |
| AT-34 | Plan transition | Upgrade, expiry, cancellation, refund/reversal and downgrade use approved coverage/usage policy; no stale unlimited access, surprise reset or deletion of own posts/offers |
| AT-35 | New renewal IDs | If recurring enabled, unknown local payment IDs are bounded and authenticated, matched to known subscription/customer/merchant/period; forged or unrelated IDs never activate entitlement |
| AT-36 | Renewal/cancel recovery | Ambiguous subscription creation, repeated callbacks, period failures, out-of-band cancellation and credential/cache expiry reconcile without duplicate future subscriptions/charges |

## Non-functional evidence
| Requirement | Evidence |
|---|---|
| NFR-01/10 | Actual PostgreSQL concurrency, constraints, allowance ledger, cross-board DTO/network tests and billing races |
| NFR-02/07 | Explicit per-board schedules, measured purge/backup roll-off and timed RPO <=1h/RTO <=4h recovery with tombstones/provider reconciliation |
| NFR-03 | Defined three-board route mix at 100 active users/10,000 information items; p50/p95/p99, throughput/errors, CPU, DB round trips/locks/pools and backlog; no benchmark claimed |
| NFR-04/05 | Whole-flow accessibility and real Android/iPhone browser/installed sessions, photo selection/HEIC and chosen payment-return policy |
| NFR-06/08 | Interrupted/overlapping Cron and immediate claims, lease fencing, fairness, stale-send suppression, alerts and backlog-age evidence |
| NFR-09 | Processing register, reviewed notices/contracts/transfers, rights/retention and breach drills before real data; new-board confidentiality included |

Mollie one-hour and Resend 24-hour idempotency windows do not establish permanent exactly-once delivery/charging; test unresolved outcomes and explicit reconciliation after expiry/rotation. Resend events require raw-body signature verification and association to platform mail, not payment facts.

## Release checklist
- [ ] Only free and low-price subscription tiers implemented; no per-offer charge or publication-payment endpoint; price/period/renewal/refund and exact quota/transition semantics settled and translated.
- [ ] New-board audience/geography/lifetime and trade settlement boundaries chosen.
- [ ] Selected runtime, actual transactions/claims, media and provider contracts qualified with synthetic data first.
- [ ] NFR-09 evidence complete before real personal data, including Resend US and Images processing/temporary-copy review.
- [ ] Actual merchant/method/mandate and controlled charge/refund/cancel/renew evidence before paid membership.
- [ ] Applicable AT-01–AT-36/NFR evidence passes, with no critical privacy/allowance/finance/purge issue.
- [ ] Named human support/finance/privacy owners, operations funding and refund reserve; measured costs and genuine participants.

No production qualification, legal clearance or purchase follows from this checklist alone.
