# Product requirements document (PRD)

**Version:** 0.2 · **Status:** proposed; Web/PWA-first platform direction user-confirmed · **Primary owner:** product lead

## 1. Users and jobs to be done

| Persona | Need | Main constraint |
|---|---|---|
| Client: homeowner or authorized resident | Explain a carpentry job and choose a suitable business | Does not know measurements/material terminology; wants privacy |
| Carpenter: verified sole trader/small business owner | Find suitable local work and control acquisition spend | Often on mobile, has limited time, wants predictable fees |
| Trust/support operator | Review jobs, businesses, abuse and restoration requests | Must act without unrestricted browsing of private conversations |
| Finance operator | Reconcile access sales, PSP transactions and refunds | Must not repair balances with unaudited manual edits |

Adults only. A user may have client and business-owner roles, but a business cannot unlock its owner's job. MVP supports one owner per business; multi-user business seats are out of scope.

## 2. MVP scope

**P0:** desktop Web plus Android/iPhone browser and installable PWA delivery; sign-in; verified client contact before publishing; manual business/KVK checks; four initial carpentry templates; moderated job publication; safe previews; geographic/category filtering; €10 top-up; atomic €1 unlock; shared scope Q&A; private messages and structured quotes; client selection; completion confirmation; job-linked reviews; billing history; abuse reports; support restoration; operations audit and reconciliation. Installation is optional; neither an app-store account nor notification permission is required for core use.

**P1 after beta:** saved searches/digests, richer portfolio verification, optional language assistance, finer availability matching and optional permission-based Web Push where supported. No AI or push subscription is necessary to complete the core workflow. Dedicated Android/iPhone apps belong to a later maturity-gated phase, not an automatic P1 commitment.

**Not MVP:** other trades, employment vacancies, cross-border business billing, construction escrow/payments, success commissions, native mobile apps, bidding auctions, paid placement, subscriptions, transferable credits, automatic top-ups or automatic price optimization.

## 3. Canonical commercial and thread rules

1. A job has a free sanitized preview and an entitled full brief. Browsing previews is free for registered approved carpenters. Anonymous visitors see categories, example jobs clearly labeled as examples, and opted-in public profiles only; live jobs are not SEO pages.
2. **€1.00 including VAT per business per job thread**, once, at explicit unlock confirmation. It buys the full brief, approved attachments, shared scope Q&A and one private conversation with the client. It does not buy competitors' messages/offers, client contact details or a work award.
3. Access uses €10 gross prepaid service top-ups. No auto-refill, no expiry in the pilot, no transfer. Top-up does not reserve a slot or automatically spend on a job. This design is conditional on G0 legal/accounting and PSP approval.
4. Maximum **three distinct businesses over the entire lifetime of a job**. The fourth cannot buy access. Withdrawals, inactivity, refunds and suspension do not reopen or recycle slots. This conservative rule avoids repeatedly reselling one inquiry; measure its liquidity cost.
5. Reopening the same page, retries, sending replies, editing a quote, or client selection never charge again. Refunded entitlements cannot be purchased again. Original purchase terms remain attached to the entitlement.
6. Paying does not reveal phone, email, street address or exact location. The client may explicitly share selected fields with one business; may revoke future disclosure; previously seen information cannot be “unseen.” Do not place contact information in shared Q&A.
7. Offers, prices and private messages are visible only to the client and that particular business. The shared Q&A is for non-commercial scope questions and client answers, visible only to the client and entitled firms. A client must deliberately publish a summarized private clarification; never auto-publish a private message.
8. Normal access continues until retention deletion, subject to lawful removal/suspension. Write rights follow job/entitlement status. Restored entitlements become read-only and keep their historical receipt; removed/unsafe jobs additionally redact sensitive brief/attachments. Earned time-based restoration is evaluated before late private writes or awards, so worker delay cannot change eligibility.
9. Paid-access restoration rules are specified in [Payments](08-payments-and-economics.md). Losing a valid job, choosing not to respond or withdrawing does not automatically earn restoration.
10. One job is one genuine scope. A material edit pauses new purchases and notifies existing participants; no extra charge. Moderator approval resumes publication. Duplicate reposting to generate fresh paid access is prohibited.

## 4. Functional requirements and acceptance criteria

| ID | Requirement | Acceptance criteria |
|---|---|---|
| FR-01 | Identity and verification | Publishing requires verified email and phone; purchasing requires an active approved business with recorded KVK-check date/result; a failed or suspended verification blocks new unlocks |
| FR-02 | Structured client brief | Category, city/approximate area, budget range or “unknown,” desired date range, material/finish preference or unknown, measurements or “site measurement needed,” access constraints, description, and optional photos; save draft and resume |
| FR-03 | Publish/moderate | Publication needs client confirmation that the job is genuine and still open, privacy review of preview, attachments scan and an operator approval; rejected jobs have a reason and correction route |
| FR-04 | Browse fit previews | Preview returns only approved title, category, city, coarse distance bucket, budget/date ranges, dimension-completeness indicators, number of used slots, fresh client-confirmation age and €1 gross price; no raw free text or image URLs |
| FR-05 | Explicit paid entitlement | After showing fee, rights, no-award guarantee and restoration policy version, one atomic transaction debits 100 gross cents, records one entitlement and consumes one lifetime slot; failure changes none of them |
| FR-06 | Safe full-thread reads | Every full brief, attachment, Q&A and private conversation request checks authorization on the server; a client-side blur is never an access boundary; unauthorized object IDs return no content |
| FR-07 | Shared clarification | Entitled active firms ask scope questions; the client answers; UI warns against contact and pricing content; report/hide route exists; private quotes never enter the shared board |
| FR-08 | Private response and quote | Exactly one client–business channel per entitlement; send text and approved attachments; quote versions record line items, gross totals, VAT basis, exclusions, earliest date, visit requirement, validity and brief revision |
| FR-09 | Compare/select/complete | Client compares their own offers; only a current, unexpired quote on the latest acknowledged brief revision is selectable; one winning business; selection is atomic; completion can be confirmed by both parties |
| FR-10 | Reviews and evidence labels | At most one client review per awarded job; label “both parties confirmed completion” only if both did; otherwise label client-reported completion; no claim of payment verification; professional may reply and report |
| FR-11 | Receipts and balances | A verified paid top-up produces one credit grant, never two; show gross amount, billing document and all spend/restore/refund movements; pending payment is not available balance |
| FR-12 | Restorations and cash refunds | Automatic rules and operator decisions use stable reason codes; each entitlement restored at most once; unused paid funds can be refunded to original payment method through the controlled lot-based workflow |
| FR-13 | Notifications | Transactional email contains a generic notification and authenticated link, never job body, address, photos or competitors' information; in-app status is authoritative; notifications do not grant access |
| FR-14 | Trust operations and rights | User can report job/message/profile/review, block harassment and request data export/deletion; operators record reason, audit entry and appeal route; break-glass private access needs separate permission and a case |
| FR-15 | Spend visibility and matching | Account shows total paid access costs, restorations and client-reported awards; matching uses declared craft, city-based distance and date fit, with explanations; no payment-based ranking |
| FR-16 | Desktop, Android and iPhone parity | Clients and carpenters complete all core flows on the supported desktop browsers, Android Chrome and iPhone Safari; Android/iPhone also support home-screen PWA use; manifest, icons, installation help and browser fallback work; accounts, balances and entitlements are server-owned and shared across devices |
| FR-17 | Safe PWA lifecycle and connectivity | Only explicit public static assets/offline page may be persistently cached; private content/API/render payloads/media and financial states are network-only; offline writes are not queued; reconnect/resume revalidates state and ambiguous purchase outcomes; logout clears local sensitive state and updates never auto-replay a mutation |

Quote comparisons show differences in assumptions; the platform does not invent a market price, recommend the cheapest provider or require a carpenter to honor an unmeasured estimate as a binding construction contract.

## 5. Job lifecycle and write permissions

| State | Entry/exit | New unlocks | Existing messages |
|---|---|---|---|
| DRAFT | Client editing | No | None |
| PENDING_REVIEW | Submitted with verified contacts | No | None |
| OPEN | Approved, expiry in future | Yes if fewer than three firms | Active participants; shared Q&A allowed |
| PAUSED | Client pause, material edit or stale confirmation | No | Existing active conversations/Q&A continue, unless a safety hold denies them |
| AWARDED | Client selects one valid quote | No | Winner/client can continue privately; all others and shared Q&A read-only |
| COMPLETED | Client marks complete; optional business confirmation | No | Read-only |
| CANCELLED | Client withdraws | No | Read-only; cancellation restoration rule evaluated |
| EXPIRED | 14 days after first publication without an award | No | Read-only |
| REMOVED | Moderation takedown | No | Safe historical records only; sensitive content blocked |

Client reconfirms an OPEN job every 72 hours. Without confirmation it becomes PAUSED, never silently keeps selling. An operator-approved reconfirmation may resume it before expiry. Expiry never slides with reconfirmation. No automatic repost or post-expiry reopen in MVP; genuine follow-on scope requires review as a separate job.

Participant status is independent: ACTIVE, READ_ONLY_RESTORED, WITHDRAWN or REVOKED. Suspension denies access as needed even if the job is otherwise OPEN. Blocking stops private messages; a support workflow can record completion outside the blocked channel.

## 6. Non-functional requirements

| ID | Proposed target and measurement |
|---|---|
| NFR-01 | No double debit, fourth lifetime participant or private-content leak in required integration/concurrency/security tests; zero tolerated production incidents |
| NFR-02 | WCAG 2.2 AA target for complete core flows; keyboard and screen-reader manual tests, not just automated scoring |
| NFR-03 | At 100 concurrent active users: API p95 reads <500 ms and unlock transaction <1 s excluding internet/PSP time; load-test dataset 10,000 jobs and 100,000 messages |
| NFR-04 | Monthly core app availability target 99.5% during staffed pilot; monitor end-to-end, report PSP/email dependencies separately |
| NFR-05 | Paid fulfillment p95 <60 seconds after verified PSP paid state; alert at 5 minutes; provider outage may breach target without granting unverified funds |
| NFR-06 | Mobile p75 LCP <2.5 s, INP <200 ms, CLS <0.1 as initial performance budgets, validated with real-user measurements when sufficient traffic exists |
| NFR-07 | Recovery target RPO <=1 hour, RTO <=4 hours; prove via timed restore before paid launch |
| NFR-08 | EU primary hosting; encrypted transport/storage; least privilege, case-scoped support access; approved retention/deletion schedule and processor register |
| NFR-09 | Release records exact supported OS/browser versions and passes the cross-platform matrix on real Android and iPhone devices in browser and installed modes; includes bank-app checkout return, keyboard/safe-area layout, account switching, cache inspection, offline recovery and service-worker update/rollback; viewport emulation alone cannot pass this gate |

These are engineering/release targets, not an SLA offered to users. Final provider costs and operational coverage are reviewed before promising them.

## 7. Instrumentation and pilot decisions

North-star learning metric: **eligible published jobs with at least one relevant response and a client reply within 48 hours / all eligible published jobs**. Relevant means the client marks “matches my request,” supported by operator sampling, not keyword matching.

Track draft-to-publication completion; full-preview-to-confirmed-unlock conversion; slots used; first response within 24h; two comparable offers within seven days; client reply rate; confirmed versus client-reported awards/completions; repeat professional purchase in 30 days; restoration rate/reasons; support minutes/job; contribution/job. Use pseudonymous IDs, not message bodies or contact data.

Exclude staff/test/fraud jobs from marketplace metrics using explicit flags; include legitimate cancellations and unanswered jobs in cohort denominators to avoid inflated liquidity. Show cohort size and confidence limitations. See [Test and launch plan](11-test-and-launch-plan.md) for gates.
