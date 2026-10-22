# Project brief and revised scope
**Version:** 0.3 · **Updated:** 22 October 2026 · **Owner role:** founder/product

## 1. Aim
An economical local information platform connecting small and medium home-maintenance/carpentry businesses directly with clients. Reduce the cost and friction of finding relevant jobs without claiming to guarantee work or workmanship.

**Client promise:** post free, without mandatory account creation; compare all private rough offers; decide whom to contact and when to close.
**Company promise:** browse suitable tasks free; choose service radius and opt-in alerts; know the confirmed €1 publication fee before payment; competitors cannot inspect the offer.

## 2. Confirmed versus proposed
| Item | Requirement or interpretation |
|---|---|
| Fee payer | **Confirmed:** client pays no platform commission; construction company pays the platform. The draft's contrary client-€1 statements are alternatives for analysis, not launch rules |
| Fee event | **Confirmed on 21 October 2026:** €1 including VAT once per company/task for first successful private rough-offer publication; free reading and edits, no guaranteed reply, full refund if publication fails; [ADR 0001](../docs/adr/0001-charge-for-first-offer-publication.md) |
| Collection | **Confirmed:** individual Mollie-hosted checkout for each €1 publication fee; Mollie selected as sole current provider on 22 October, prepaid bundles/wallets/subscriptions deferred; live merchant/method approval separate |
| Payment methods | **Confirmed on 22 October 2026:** iDEAL/Wero and cards only; merchant approval and €1 payment, full-refund and phone/PWA return tests required per method before activation; [ADR 0007](../docs/adr/0007-use-mollie-for-current-payment-integration.md) |
| Material task edits | **Confirmed:** new scope revision; captured payments for unpublished old-scope offers refunded; published offers remain visible with a scope-change label and free edits, without automatic refund or extra fee; [ADR 0005](../docs/adr/0005-version-material-task-scope-changes.md) |
| Geography | **Confirmed on 21 October 2026:** the whole provinces of Groningen and Fryslân, including Drachten; company-selected radii control matches within this task-publication area |
| Trades | Carpentry, windows/doors, non-structural alterations, insulation, internal walls, painting and roof maintenance; category-specific safety/information fields, not trade certification |
| Client identity | **Confirmed on 21 October 2026:** email confirmation before publication, short-lived task-scoped management links and fresh-link recovery; optional client accounts/dashboards/task linking deferred until after MVP, company/staff accounts in scope; [ADR 0002](../docs/adr/0002-confirm-email-for-guest-task-ownership.md) |
| Company participation | Registered company profile, declared specialties, approximate base area/radius, rough offer and company contacts; website optional if none exists |
| Contact | **Confirmed on 21 October 2026:** private by default; explicit selected email/phone access for every published responder while open; revocation/close stops future disclosure, copied details cannot be recalled; [ADR 0003](../docs/adr/0003-client-controlled-responder-contact.md) |
| Visibility | Client sees all eligible published offers; company sees only its own response; no competitor list/count or fixed commercial cap |
| Platform role | Information/connection only; no routine workmanship certification, site monitoring, construction-contract/payment handling |
| Retention | **Confirmed on 21 October 2026:** immutable three-calendar-month task expiry, immediate access cutoff, live purge within 24h, isolated backups within 30 days and deletion-safe restores; narrow separate billing/active-company/case-evidence retention accepted, exact schedules need professional review; [ADR 0004](../docs/adr/0004-expire-tasks-from-first-publication.md) |
| Platforms/languages | Desktop Web and Android/iPhone PWA now; Dutch/English; dedicated apps later |

Name remains undecided. “Work Platform” is a repository/working label, not a registered brand. Creative naming candidates may be evaluated later for meaning, availability and trademark clearance; do not buy domains or publish a name without approval.

## 3. Product alternatives
1. **Selected:** company-funded first rough-offer publication, free client posting and free company reading. The fee event and €1 gross price are confirmed; commercial viability remains a hypothesis.
2. Client-funded €1 posting, free company response: simple one-sale-per-task model, but reduces demand conversion and **conflicts with the current payer instruction**. Evaluate on paper only.
3. Company success percentage or subscription: may improve revenue but needs outcome visibility/collections or recurring value. Not adopted: construction agreements happen off-platform and the platform does not monitor completion.

Confirmed: direct individual €1 hosted checkout in MVP, with prepaid bundles, wallets and subscriptions deferred until after MVP. This avoids upfront prepayment and prepaid accounting, accepting checkout friction and processing costs on each small transaction. Any later company bundle needs its own accounting, refund and product review.

## 4. Boundaries
The rough bid is an indication for client selection, subject to a site visit and final agreement. No platform booking, contract acceptance, construction escrow, contractor payout, success tracking obligation, public price auction, competitor feed, mandatory chat or public verified-completion review system.

User-declared company details are not a quality guarantee. No routine manual inspection of every job/company is promised. Automated email ownership, media/privacy checks and a complaint queue are platform safety controls, not workmanship verification. Applicable trader-identification duties cannot be removed by a disclaimer; determine them before launch.

## 5. Pilot and success
Recruit genuine participating companies in the trial area before inviting paid responses. Test a mix of the listed categories without pretending supply exists everywhere. Directional pilot targets: 80% of tested users understand payer/charge event/contact visibility; 70% of eligible jobs receive a relevant rough offer in 48 hours; no duplicate company/task charge or rival-offer exposure; deletion/access deadlines pass tests.

Track client posting completion, matched alerts, offer conversion, client contact clicks/self-reported site visits, complaints, refund cost and contribution per task. A contact click is not proof of a call, completed job or construction payment. No invented testimonials, leads or competitor-price claims.

## 6. Approval
Publishing this revision updates design documents only. The charge event, €1 VAT-inclusive price, individual hosted collection with Mollie, two-province pilot scope, guest-only MVP client management, contact-permission model and retention boundary/timings are confirmed. Before collecting money validate the region mapping, obtain professional review of exact separate retention schedules, and resolve: legal Mollie merchant/profile and business settlement account, live methods and detailed refund/tax policy, privacy/trader duties, staffing and beta budget. Implement and translate the confirmed rules; optional client accounts/linking are deferred until after MVP.

The working app direction is already Web/PWA-first. Dedicated apps remain later. See [Decisions](12-decisions-and-risks.md) and [revision summary](16-requirement-revision.md).
