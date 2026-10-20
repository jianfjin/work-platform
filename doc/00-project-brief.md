# Project brief and change proposal

**Status:** proposed · **Owner role:** founder/product lead · **Date:** 20 October 2026

## 1. Why this project

Carpentry estimates depend on dimensions, materials, finish, access and timing. A generic short description can leave clients with incomparable quotes and professionals spending acquisition time on unsuitable jobs. These are **problem hypotheses** requiring interviews, not conclusions established by competitor browsing.

**Vision:** make a small carpentry job as clear to commission as a well-written work order, without turning a homeowner's inbox into an auction.

**Client promise:** describe the work once, clarify scope once, compare up to three private proposals, choose freely.

**Carpenter promise:** see meaningful fit information before paying; explicitly spend €1 for one full thread; never pay again for that same thread; know the eligibility, refund rules and competition limit before confirming.

## 2. Proposed approach and alternatives

| Approach | Benefits | Trade-offs | Recommendation |
|---|---|---|---|
| A. Carpentry-only, fixed-price thread access | Clear positioning; measurable acquisition cost; domain-specific briefs | €1 may not cover acquisition/operations; lower liquidity than broad marketplaces | **Pilot this**, with a capped budget and an economics stop gate |
| B. Broad trades marketplace, variable lead prices | Larger addressable audience; pricing can follow job value | Weak initial focus; harder trust operations; directly competes with established networks | Defer until one carpentry region works |
| C. Concierge matching or success-fee marketplace | Stronger hands-on help or revenue aligned with awards | Manual costs; attribution disputes; escrow/payment scope if expanded | Use concierge only for research; no success fee in MVP |

The recommendation combines useful carpentry-specific work preparation with a simple price, not a claim that low price alone is defensible. A repeatable library of validated briefs and truthful outcomes may eventually be defensible; this remains unproven.

## 3. Assumptions to make the draft concrete

| ID | Assumption | Consequence if wrong | Validation owner |
|---|---|---|---|
| A01 | Netherlands; adults; independent registered carpentry businesses | Different verification, language and tax design | Product + legal |
| A02 | Start in Utrecht city and approximately 25 km surroundings | Supply density may be insufficient | Founder; choose final geography after interviews |
| A03 | €1 means **€1.00 gross**, not €1 plus VAT | Lower net revenue; explicit pricing everywhere | Founder + accountant |
| A04 | Paid beta uses €10 top-ups for ten €1 unlocks | Upfront commitment may suppress first purchase; prepaid-service classification requires review | Product + legal + PSP |
| A05 | Three distinct businesses maximum for the lifetime of one job | Less spam but possibly stranded threads if firms do not reply | Product; measure slot-to-response conversion |
| A06 | Clients prefer private offers over a public bidding contest | Shared knowledge must exclude pricing and private messages | UX interviews |
| A07 | A small team can operate a modular monolith | More runtime separation only if measurements justify it | Tech lead |
| A08 | Reviews can be verified as platform interactions, not as payments for construction | Completion claims need accurate labels | Trust lead |
| A09 | **User-confirmed:** desktop Web and Android/iPhone PWA first; dedicated mobile apps after maturity | MVP needs real mobile-browser and installed-PWA acceptance, not separate native builds | Product + tech lead; native release has a later approval gate |

Working name, launch geography, provider contracts, pricing duration and the public launch budget require approval. These are not facts inferred from the user request. Platform delivery A09 was explicitly confirmed by the user; detailed support/release policies are in [Cross-platform delivery](14-cross-platform-and-pwa.md).

## 4. Scope and non-goals

Initial categories: made-to-measure cabinets and shelving; internal doors and trim; non-structural carpentry repairs; simple wood installations. Templates allow “I do not know” and a site visit. Load-bearing changes, electrical/plumbing work and emergency repairs route to an out-of-scope message, not an unqualified quote funnel.

The platform connects clients and businesses. It does not employ carpenters, determine their working hours, guarantee workmanship or take the construction contract payment. An accepted platform quote records intent and scope; the parties finalize their construction agreement directly.

## 5. Success definition

Run discovery with 10 clients and 10 carpenters before implementation approval. Conduct at least five moderated mobile purchase tests. Run a bounded paid beta only after payment/privacy gates pass. Targets are proposed learning thresholds:

- At least 80% of tested professionals correctly describe what the €1 buys and when it is not refunded.
- At least 70% of eligible pilot jobs receive one relevant private response within 48 hours.
- At least 30% of eligible pilot jobs receive two comparable proposals within seven days.
- Zero confirmed double-debits, oversold threads or unauthorized private-content exposures.
- Paid-beta contribution per job is positive **after variable supply and demand acquisition and moderation costs** before scaling. See the explicit model in [Payments](08-payments-and-economics.md).

## 6. Approval and change control

This document proposes new capabilities; implementation is gated on G0. Before coding, the founder approves market, first categories, fee semantics, three-firm limit, refund policy, and budget. Legal/accounting sign off prepaid access, invoice timing and applicable platform obligations. The tech lead approves transaction and privacy boundaries.

After approval, changes to charge timing, gross price, refund eligibility, contact disclosure or participant visibility require a recorded decision, updated acceptance tests and versioned policies. Existing unlocks retain their purchase terms. No prospective pricing change may retroactively debit existing entitlements.

## 7. Impact

Affected domains: identity, client job publishing, matching, paid entitlements, billing, private messaging, shared clarification, quotes, reputation and operations. There is no pre-existing application to migrate in the target repository at research time. This proposal deliberately adds only files under `doc/`.
