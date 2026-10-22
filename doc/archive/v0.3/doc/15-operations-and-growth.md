# Platform operations and low-cash-cost growth
**Version:** 0.3 · **Updated:** 22 October 2026 · **Status:** refund policy and task-alert cadence confirmed; remaining operating plan proposed; no campaign/account created

## 1. Necessary jobs even for an information intermediary
The platform does not monitor construction or certify workmanship. It still needs named people for its own service. One founder can cover several roles in a small pilot, but access and review responsibilities remain distinct.

| Function | Minimum work | Suggested rhythm |
|---|---|---|
| Platform owner | Scope/terms/merchant/provider approvals, budget, incidents and operating hours | Before launch; weekly decisions |
| Support/report handler | Guest-link/posting help, client closure/contact issues, misleading content/harassment/privacy notices, correction/appeal | Staffed business-day queue; urgent exposure escalation |
| Finance | Paid-publication/refund exceptions, PSP reconciliation, invoices/taxes and dispute reserve | Automatic verified-failure refunds; one-business-day submission target after verification/approval; automated checks and human exception review |
| Privacy/security | Deletion lag/object failures, rights requests, secret/access/log review, legal evidence scope | Automated deadlines; daily alert review; scheduled audits |
| Mail/delivery | Opt-in settings, bounces, unsubscribe suppression, stale-job/send failures, sender reputation | Automated dispatch checks and daily health |
| Supply/growth | Real local company enrollment, coverage and client education; no fake jobs/reviews | Before demand recruitment and weekly |
| Technical operations | Deployments, workers/backups, PWA/browser regressions, incident recovery | Alerts plus scheduled maintenance/drills |

Propose reserving 30–60 minutes/day of human attention for a small pilot, then measure actual workload; this is not a service guarantee or a promise that all categories fit that capacity. The economics model's lean €0.20/job operations allocation is a test assumption and may be too low. Record labor rather than calling it free.

No recurring manual visit, trade certification or mandatory review of every job/company. Automated security/privacy checks do not prove genuineness. Exceptions/reports still need a reasoned, proportionate decision and an appeal/correction path. Determine any legally required trader identification separately.

Confirmed incident-refund policy: companies may request a full €1 publication refund for a task substantiated as fake, duplicate or materially misrepresented. Ask for reports within seven days of payment and still review credible later evidence. Authorized staff record the decision/reason, retain only necessary case evidence with the existing access and review/deletion controls, and pass an approved obligation to the finance workflow. An allegation, no reply or losing the work alone does not approve a refund. Refunds do not restore withdrawn offers or reset publication history. Submission timing is confirmed: automatically submit verified failed-publication/duplicate-capture refunds and target Mollie submission within one business day of verification or staff approval. Escalate overdue, queued or failed refunds and track submission separately from completion. The operating calendar, named staffing, monitoring and funded reserve must support that target before paid beta; bank arrival is not guaranteed by it. See [payment policy](08-payments-and-economics.md).

## 2. Daily/weekly dashboard and incidents
Daily: unresolved captured-unpublished payments/refunds, overdue refund submissions and queued/failed obligations, duplicate effects, email opt-outs/bounces, report/privacy queue, due deletion failures and service availability. Alert immediately on rival/contact leakage, unpaid visibility, duplicate retained fee or expired task still accessible. Stop the affected action/exposure, preserve minimal evidence, assess notice duties, repair/reconcile/test and document reopening.

Weekly: eligible tasks by category/area, genuine registered companies, relevant response rate, per-task response distribution, client contact clicks, complaints, refunds, actual PSP/mail/storage/labor and acquisition costs. Contact clicks or user feedback are not verified construction completion/payment.

Do not increase unwanted callback volume to meet revenue targets. Clients may close/revoke contacts; inactive companies may stop alerts. No paid ranking or unsubstantiated public blacklist.

## 3. Automatic notifications to SMEs
**Confirmed on 22 October 2026:** registered companies explicitly opt in to matched task alerts, with one daily digest as the default and optional instant emails. Keep category, base area/radius and language settings, confirm the recipient, show what will arrive and provide an easy unsubscribe. Choosing a frequency or creating an account does not subscribe the company. Security/login/payment emails have separate necessary-service purposes.

Flow: task published -> safe matching event -> category/radius/allowlist match -> due digest/instant queue -> recheck current frequency/OPEN/not expired/company active/opt-in -> deduplicated send. Use shared delivery history across both modes so a frequency change does not send the same event again. Suppress empty digests. A changed radius affects future sends. Unsubscribe, closure, suspension or deletion suppresses queued notifications. Already accepted external emails cannot be recalled. Instant alerts run through the worker; no fixed delivery-time promise is established by this decision.

Email contains category/city/PC4, optional coarse timing and authenticated task link, never raw description/photos/client contacts/full postcode/guest credentials. Firms read the task free and decide whether to pay for a rough offer. Do not scrape websites and forward full jobs to every discovered business address. Recruit opt-ins via permissioned partnerships, self-registration and approved outreach methods; legal reviews precise communication rules.

The founder selected Resend after inspecting the existing cloud-mail production service; see [ADR 0012](../docs/adr/0012-use-resend-for-platform-email.md) and [the review](21-cloud-mail-and-resend-review.md). Production has a verified datawego.nl sender domain in Ireland, while this platform will use a dedicated sending subdomain whose parent domain and exact address remain open. Use a direct platform adapter with application-specific sending credentials and PostgreSQL outbox/delivery records; the existing mailbox send action and no-op mailing-list consumer are not the platform notification service. Keep verification/recovery/guest-access mail separate from task-alert opt-in. Provider acceptance is not proof of inbox delivery.

Before activation, qualify the sending domain, delivery/bounce/complaint handling, opt-out suppression, actual quotas/costs and provider message/log/backup retention. Review the approved purpose and provider rules for matched alerts rather than inferring permission from the name of an API. Resend documents US storage for account data, metadata, logs and API records regardless of sending region; review this boundary explicitly. Brevo remains unselected comparison context. The dedicated-subdomain pattern is confirmed; the parent domain, subdomain label, sender/Reply-To addresses and plan remain open. No DNS configuration was changed and no email was sent during the inspection.

## 4. Geography and location costs
The founder confirmed [PDOK Location API](https://www.pdok.nl/location-api1) on 22 October for Dutch location lookup, with minimum server-side queries and approximate radius matching in our application/database. Verify dataset terms, CRS, limits, province accuracy and retention rights; free API access does not mean free hosting or guaranteed availability. Keep personal task/contact fields out of queries and review provider logging and the location-data flow under NFR-09.

Google remains unselected comparison context. Any future provider change needs a separate decision and privacy/terms/budget review; an operator-owned Maps project, restricted server key, enabled SKUs only, quotas, alerts and approved spend would be required. [Geocoding policy](https://developers.google.com/maps/documentation/geocoding/policies) restricts storage/caching and requires attribution in applicable use; do not assume an unrestricted permanent postcode cache. Review [current SKU pricing](https://developers.google.com/maps/billing-and-pricing/pricing), not outdated blanket free-credit claims.

No exact address is needed for discovery: company-selected radius and coarse postcode/city coordinates support approximate straight-line fit. Validate city/postcode and membership in the confirmed whole Groningen/Fryslân province pilot region, including Drachten; do not trigger unrelated alerts if provider unavailable. A company radius does not expand the region eligible for task publication.

## 5. Start with zero paid advertising
An initial **€0 advertising-spend** test is realistic as a budget choice; a zero-cost business is not. Domain/hosting/email/PSP/legal/security/support still need funding or existing capacity, and founder time has an opportunity cost.

Suggested four-week plan after operating readiness:
1. Recruit a small genuine supply cohort through existing networks, local builders' merchants, professional associations and permissioned community contacts. Companies create their own profiles and opt in; aim for roughly 15–20 active firms spread over real category/area coverage rather than a fabricated directory.
2. Ask community/admin permission to explain the free client service in Groningen/Fryslân/Drachten groups, neighborhood networks and expat channels. Publish Dutch/English explanation and actual conditions; no mass unsolicited posts or fake endorsements.
3. Create useful static region/category pages and short posting/privacy/site-visit guidance. Encourage partners to link to those pages. Do not expose live home photos, postcode/contact or offer text to search engines.
4. Invite genuine clients in covered areas, ask for feedback/referrals with permission, track the path from eligible post to useful offer/contact. Avoid promising reply times where supply is thin.

The numbers are proposed recruiting goals, not current inventory or guaranteed performance. No introductory fee waiver is enabled implicitly; any founder-funded free trial needs explicit terms/budget and separate test accounting.

## 6. Google Ads: optional controlled experiment
Do not launch ads now. Only after genuine coverage, a working posting/payment/refund flow and an owner-approved loss budget, consider an exploratory **€100 total intended test budget**, not a profitability forecast. Founder can instead keep spend at zero.

Target only the approved geographic area and relevant maintenance categories; use truthful localized copy such as free posting, and no verified/guaranteed-work/lowest-price claims. Send to generic regional/category landing pages. Use exclusions for employment, training, irrelevant trades and unsupported locations; review actual search terms. Do not place client job/contact data in ad URLs, audience uploads or conversion payloads.

Measure eligible published tasks and ultimately useful responses/actual fee contribution, not clicks alone. Assumed CPC €0.50 and 10% publish conversion imply €5 demand CAC; this is an arithmetic example, not a Google/local-market quote. Compare against actual contribution from [Payments](08-payments-and-economics.md), which may be far below €5.

[Google documents](https://support.google.com/google-ads/answer/6385083?hl=en) that average daily budgets are not rigid daily caps; many campaigns can spend up to twice that average on a day, with a monthly limit based on 30.4 times the daily average. Do not assume €5/day for 20 days is a guaranteed €100 cap. Before approval verify the actual campaign's total-budget/limit options, set controls/alerts and name someone to monitor/pause well before the intended ceiling; reporting/delivery lag and applicable rules must be understood. No unattended spending workaround.

Stop if coverage/quality fails, privacy issues occur, budget risk rises or actual marginal contribution cannot justify acquisition. If the experiment is a deliberate research subsidy, record it as such; never call negative acquisition economics sustainable.

## 7. Zero-cost and responsibility answers in plain terms
- **Who pays?** Client €0; confirmed company €1 including VAT for its first successfully published private rough offer per task. Not a percentage of construction work.
- **Whose account?** The legal platform operator's approved merchant/business settlement account; not the assistant's or an informal shared personal account.
- **Can email be automatic?** Yes, opted-in matched SMEs, not arbitrary scraped addresses or full-client-data forwarding.
- **Do we verify/supervise work?** No routine workmanship service; still secure our platform, administer our fees and process lawful notices/reports.
- **Delete after three months?** Task content yes under the precise calendar/deletion schedule; minimal financial/legal records and active accounts have disclosed separate purposes.
- **Can launch cost zero?** Paid ads can start at zero; software operations, transaction fees and human time cannot be assumed zero.
