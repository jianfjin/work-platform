# Test strategy and release gates
**Version:** 0.3 · **Updated:** 21 October 2026
No application tests have run in this documentation delivery. Cases below are planned implementation evidence.

## 1. Layers
Unit tests for policy/time/normalization; real PostgreSQL integration for fee/publication/closure/refund and uniqueness; contract tests for scoped DTOs; browser E2E and accessibility; real Android/iPhone browser/PWA tests; worker/PSP/backup failure drills. Synthetic data and PSP test mode only before approved production testing.

## 2. Requirement traceability
| Scenario | Requirements | Given / when / then |
|---|---|---|
| AT-01 Guest management | FR-01, FR-13 | Guest posts without account; one-use valid emailed token creates only that job session; expired/reused/other-job token denied; resend does not enumerate |
| AT-02 Company account | FR-03, FR-13 | Confirmed login email and declared profile allow own actions; no quality-verified claim; suspended/non-owner cannot submit |
| AT-03 Safe brief/photos | FR-02, FR-04, FR-13 | Registered company reads safe task/photos free; no full postcode/client contacts/management credential; private storage authorization tested |
| AT-04 Region/radius | FR-02, FR-04 | Allowlisted Groningen/Fryslân job, category and chosen approximate radius match; Drachten not a separate province; invalid postcode/city/outside area/failed geocode never fake a match |
| AT-05 Private all-offer list | FR-05, FR-13 | Client sees every eligible own-task offer via pagination; company sees only its own; probing IDs/cursors/counts/analytics/exports reveals no rival information; no three-firm commercial cap |
| AT-06 Fee payer/event | FR-06, FR-17 | Client never charged; company reads/drafts free; correct verified €1 payment publishes first offer; edits/read/new key for fulfilled response cost zero |
| AT-07 Duplicate effect | FR-06, FR-17 | Concurrent same/different keys, callback replay and timeout -> one retained fulfilled fee/response and original receipt; mismatched idempotency payload conflicts |
| AT-08 Close/edit race | FR-08, FR-18 | Close/deadline/material edit serializes with callback; invalid pending paid offer never publishes and gets unique refund; existing valid visible offer not silently recharged |
| AT-09 Contact preference | FR-07, FR-13 | Default private even after payment; explicit allowed fields go only to current eligible responders; close/revoke/withdraw stops future disclosure; no automatic full postcode |
| AT-10 Opt-in alerts | FR-09 | Matching subscribed company receives safe metadata once per delivery key; unsubscribe/close/delete at dispatch suppresses; no photos/contact or guest credential in email |
| AT-11 Automatic purge | FR-10, FR-13 | At first-publish + three calendar months all access stops even with worker off; live text/photos/versions/offers/location/credentials purge with retries/progress |
| AT-12 Finance separation | FR-10, FR-17 | Task erasure leaves only lawful minimum fee records; finance API cannot reconstruct task/client content; company account retention does not preserve old jobs |
| AT-13 Misbehavior notice | FR-11 | Report creates scoped case/hold/reason/appeal; false report does not automatically publish accusations or imply work inspection |
| AT-14 Staff permissions | FR-12, FR-13 | Support cannot inspect arbitrary offers or execute finance-only actions; reason/case/MFA and immutable minimal audit enforced |
| AT-15 PWA lifecycle | FR-14, FR-13 | Desktop plus real Android/iPhone browser/installed flows work; no private cache or offline replay; safe worker update/rollback and keyboard/back navigation |
| AT-16 Languages/accessibility | FR-15 | Dutch/English critical fee/consent/expiry/mail copy and keyboard/screen-reader/reflow pass; task text not silently translated |
| AT-17 Growth/privacy | FR-16 | Generic regional SEO only, no live job/contact/photo indexing; opt-in outreach; no unapproved paid campaign or fake reviews |
| AT-18 Refund/reconciliation | FR-06, FR-17 | Paid-unpublished/duplicate capture produces one bounded original-method refund; lost provider response stays pending and reconciles; no wallet substitute |
| AT-19 Hostile media | FR-02, FR-13 | MIME spoof/oversized pixels/unsafe upload rejected/quarantined; EXIF stripped, unsupported HEIC has clear alternative; signed read cannot outlive delete_at |
| AT-20 Phone payment recovery | FR-14, FR-17 | Bank-app return in different browser, cancelled/pending payment and terminated PWA recover authenticated owned attempt; no inferred success/automatic second checkout |
| AT-21 Cross-device ownership | FR-01, FR-06, FR-14 | Same owner/response on another device recovers existing server state; cookies need not transfer, no second fee or other client's job access |
| AT-22 Restore | FR-10, FR-17 | Restore old DB/media, apply tombstones/expiry and PSP replay before access; no resurrected job or duplicate fulfillment/refund |
| AT-23 Calendar boundaries | FR-10 | Jan 31 -> Apr 30, leap dates, local DST gap/overlap and month lengths resolved by approved timezone policy; edit/close/register/reopen does not extend deadline |
| AT-24 No outcome commission | FR-06, FR-08, FR-17 | A valid visible response losing/no reply/off-platform sale creates no extra fee or automatic no-response refund; no construction amount/payment monitored |
| AT-25 Optional registration | FR-01, FR-10 | Client can keep using scoped guest ownership; optional account link proves owner, cannot enumerate/claim other jobs or reset deletion clock |
| AT-26 Merchant/terms gate | FR-12, FR-17 | Money uses platform operator-approved merchant/account and truthful terms; no assistant/personal shared account, client checkout or quality certification shortcut |

## 3. Non-functional evidence
| Requirement | Planned evidence |
|---|---|
| NFR-01 | AT-05–AT-09, AT-18/20, real concurrent DB/PSP failures and raw DTO/network checks |
| NFR-02 | AT-11/12/19/22/23, due-access filter, purge lag/object versions and backup age evidence |
| NFR-03 | 100 active users/10,000 tasks; p95 safe read <500ms and local publication <1s excluding PSP/network; no privacy/correctness regression |
| NFR-04 | Whole-flow WCAG 2.2 AA target, 320/375/768/1024/1440px, keyboard/screen reader/zoom and localized comprehension |
| NFR-05 | AT-15/20/21 on real Android and iPhone, browser and installed modes, exact release versions recorded |
| NFR-06 | AT-10, durable queue dedup, bounce/opt-out and worker downtime/retry; accepted external email not falsely “recalled” |
| NFR-07 | Processor/region/access/secret review; timed RPO <=1h/RTO <=4h restore with deletion and PSP reconciliation |
| NFR-08 | Availability monitor, versioned fee/consent/retention, critical alerts, rollback/runbook drill |

## 4. Adversarial money tests
Use synchronized independent PostgreSQL connections, not sequential mocked promises. Repeat representative races at least 20 times:
- Same company/job attempts and callbacks: unique response/fulfillment, no second retained fee.
- Client close or material edit versus paid verification: valid first publication or full refund obligation, no orphan.
- Fake/wrong-amount/wrong-currency/other-merchant/test-mode callback: no publication.
- PSP create timeout: reconcile stable attempt before another external payment.
- Lost refund response, duplicate refund callback and later chargeback: no double principal payout/reversal.
- Crash before/after local commit: rollback everything or recover exactly one effect.
- Deletion while payment pending: no content republished; minimum financial refund continues independently.
- Withdraw/reopen/edit does not enable a re-charge; an explicitly new attempt only follows a definitively failed/refunded unpublished one.

## 5. Cross-platform/security checks
On real phones test ordinary photo selection/camera denial, keyboards/safe areas, cold launch, storage eviction, background/foreground, account switch, deep links, checkout return, offline and waiting-worker updates. Inspect Cache Storage, IndexedDB, local/session storage, history restore and raw HTML/JSON/RSC; UI hiding is not authorization.

Prototype at least five users; target 80% comprehension of client free/company €1 event, no guarantee of work, unlimited eligible responder audience, private offers, callback options and three-month deletion/exception wording. Record confusion and amend copy before beta.

## 6. Release checklist
- [ ] G0 decisions approved, legal/accountant/merchant/region/contact/retention wording complete in both languages.
- [ ] AT-01–AT-26 and critical non-functional evidence pass; no unresolved high-risk finance/privacy/retention bug.
- [ ] Controlled approved live €1 publication and failed-publication refund reconciled with receipts after G2 readiness.
- [ ] Deadline denial, <=24h live purge and <=30-day backup policy proven; restore applies tombstones before exposure.
- [ ] Real Android/iPhone browser and installed PWA checkout/session/update recovery pass.
- [ ] Genuine opt-in local company coverage and client jobs; no fabricated stock or spam.
- [ ] Named support/report/finance/deletion/mail/security owner and backup; incident and refund reserve approved.
- [ ] Beta metrics include sample size, actual costs, refunds and self-reported outcomes honestly.
- [ ] G3 growth requires useful responses and positive actual contribution plus fixed-cost funding, not a promise that €1 must work.

No dedicated apps, new payment accounts, advertising spend or production deployment is authorized by this checklist alone.
