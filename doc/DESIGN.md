---
version: "0.1-proposal"
name: HoutMatch-workshop-daylight
description: "An approachable carpentry marketplace: warm workshop neutrals, deep forest actions, clear work-order hierarchy, and explicit shared/private conversation boundaries."
colors:
  canvas: "#F7F5F0"
  surface: "#FFFFFF"
  surface-warm: "#EEE8DC"
  ink: "#202821"
  ink-muted: "#596357"
  primary: "#24553C"
  primary-hover: "#193D2B"
  on-primary: "#FFFFFF"
  accent-wood: "#9A5A2A"
  border: "#D7D9D2"
  control-border: "#737D72"
  focus: "#1E6048"
  success-ink: "#20543A"
  success-surface: "#EAF3EC"
  warning-ink: "#784817"
  warning-surface: "#FFF1D6"
  danger-ink: "#A12E28"
  danger-surface: "#FFF0EE"
typography:
  display: "Georgia, serif; 48/52 desktop; 34/39 mobile; weight 400"
  heading: "system-ui, sans-serif; 30/38; weight 650"
  subheading: "system-ui, sans-serif; 22/30; weight 650"
  body: "system-ui, sans-serif; 16/25; weight 400"
  label: "system-ui, sans-serif; 14/20; weight 600"
  caption: "system-ui, sans-serif; 13/19; weight 400"
spacing:
  unit: 4px
  scale: "4, 8, 12, 16, 24, 32, 48, 64, 96"
rounded:
  control: 8px
  card: 12px
  dialog: 16px
  badge: 999px
layout:
  content-max: 1200px
  reading-max: 720px
  form-max: 680px
  mobile-gutter: 16px
  desktop-gutter: 32px
motion:
  feedback: 120ms
  panel: 180ms
  reduced-motion: "remove nonessential transitions"
---

# HoutMatch — Workshop Daylight

**Status:** original proposed design system; implementation has not been built or accessibility certified.

## 1. Reference method, not a brand clone

Follow the [awesome-design-md](https://github.com/VoltAgent/awesome-design-md) practice of a single agent-readable visual contract: semantic tokens, typography, component anatomy, states and explicit do/don't rules. The inspected [Notion example](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/notion/DESIGN.md) provides a detailed token/component specification; the [Linear example](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/linear.app/DESIGN.md) shows disciplined hierarchy, surfaces and restrained accents.

Borrow that specification discipline, not their logos, copy, proprietary fonts or brand palettes. HoutMatch uses its own forest/wood/daylight identity. The catalogue's brief summaries can differ from the actual files; the linked files, not inferred brand stereotypes, informed this document's structure.

## 2. Emotional and visual character

**Crafted, calm, candid, practical.** A client should feel comfortable admitting they do not know a measurement. A carpenter should scan fit, budget and cost without marketing noise.

- Public pages feel like a well-organized workshop notebook: warm background, honest photography, generous margins, editorial headings.
- Authenticated pages feel like a work order: compact hierarchy, strong labels, stable columns, clear status and timestamps.
- Forest green means a primary action or positive progress. Wood brown is a limited accent for category details and craftsmanship, not another competing CTA.
- Trust is demonstrated through verifiable facts and visible policy, not shield graphics everywhere.
- Light mode only in MVP. Do not add an untested dark theme that doubles the state/contrast matrix.

## 3. Typography and content hierarchy

Use native fonts initially to avoid external font requests and loading shifts. Georgia is reserved for public marketing headlines; all application, pricing and conversation UI uses the system sans stack. Fallback weights must remain legible if a platform does not support weight 650.

| Use | Desktop | Mobile | Rules |
|---|---|---|---|
| Public hero | 48px / 52px | 34px / 39px | Max 18 words; no all-caps paragraph |
| Page title | 30px / 38px | 26px / 34px | One H1 per page |
| Section title | 22px / 30px | 22px / 30px | Describes task, not marketing slogan |
| Body / input | 16px / 25px | 16px / 25px | Inputs never shrink below 16px |
| Control label | 14px / 20px | 14px / 20px | Visible, never placeholder-only |
| Metadata | 13px / 19px | 13px / 19px | Never the only place to disclose price or consent |

Use tabular numerals for balances, totals and dimensions. Keep reading lines approximately 60–75 characters. Dutch text expansion is expected; controls wrap without cutting off the euro/VAT label.

## 4. Color roles and contrast

Use tokens by meaning, not an arbitrary color per page. Normal text targets at least 4.5:1 contrast and essential control/focus boundaries at least 3:1 against adjacent surfaces; test actual pairings and states. These targets are based on [WCAG 2.2](https://www.w3.org/TR/WCAG22/); the complete AA evaluation is broader than color.

- `ink` for body text on canvas/surface; `ink-muted` for secondary readable text.
- `on-primary` only on `primary`/`primary-hover` actions.
- `border` is a decorative divider, **not** the sole boundary of an input. Inputs use `control-border`.
- Status chips use semantic ink on corresponding tinted surface, always with an icon or text label.
- Focus ring: 3px `focus`, 2px white offset, visible outside the component. Check it against every surrounding surface.
- Never use color alone for paid/locked/shared/private status. “Private conversation” must be text, not just a green dot.

## 5. Layout and spacing

12-column desktop grid inside 1,200px maximum width; 24px gaps. At 768–1,023px use a simpler two-column layout. Below 768px use one column and 16px gutters. At 320px, 200% text zoom and 400% browser zoom, core flows must reflow without page-wide horizontal scrolling.

Public vertical sections use 64–96px spacing on desktop and 40–48px on mobile. App sections use 24–32px. A job card has 24px desktop padding and 16px mobile padding. A primary control is at least 48px high; other touch controls at least 44px square. No primary action relies on hover.

Forms max out at 680px. Conversation and brief columns are approximately 7/5 on wide screens; use labeled tabs on mobile. Keep price confirmation close to the action, not in a floating footnote.

## 6. Component contracts

### Primary and secondary actions

Primary: forest fill, white text, 8px radius, 16px horizontal padding, 48px minimum height. Hover uses darker forest; pressed state has immediate visual feedback. Pending state shows a spinner and “Processing,” preserves width, and prevents duplicate submissions while still supporting server-side idempotency.

Secondary: white surface, `control-border`, `ink`; destructive actions use danger text with an explicit confirmation when irreversible. A disabled button has an adjacent explanation; do not make the explanation itself inaccessible.

### Job preview card

Order: category/status → approved title → city/coarse distance → budget/timing → measurement/material readiness → client-confirmation age → lifetime slots → €1 gross unlock action. Price and slot facts are not visually de-emphasized. Whole-card navigation must not accidentally purchase access.

No actual job photos in unpaid previews. A generic category illustration is acceptable and must not be presented as the client's work. All monetary examples use locale formatting.

### Access confirmation dialog

Dialog title names the action and job. Show price, existing balance, remaining balance, terms version, one-time/no-guarantee statement, access scope and restoration link. Buttons: Cancel / Confirm €1 unlock. If terms or price changed, return a conflict and require a fresh review; no silent consent upgrade. Trap focus while open and return focus to the trigger on close.

### Brief and measurement block

Separate desired dimensions from professionally verified measurements. Show units on each field; numeric input accepts Dutch decimal comma and converts to canonical units. “Unknown / site measurement needed” is a first-class value, not a validation failure. Photos have captions and a privacy reminder.

### Conversation composer

Private composer: explicit audience line names the client and business. Shared composer: separate location, amber information strip and “Shared scope question” heading. They must not share a mode-switching input. Message actions include report; private-message controls never offer “share all.” Client can create a new shared summary through a separate confirmation flow.

### Quote card

Show gross amount, VAT basis, line items, materials included/excluded, assumptions, start range, site-visit requirement, validity and scope revision. Quotes are ordered by received time by default, not cheapest price. Highlight incomplete comparisons without manipulating the winner choice.

### Balance and billing history

Call this “Access balance / Toegangstegoed,” not a banking wallet. Show available funds, pending payments, held refunds and an itemized history. Top-up has one clear €10 option in MVP, no “best value” decoy bundle. Never hide the €10 commitment behind a €1 headline.

### Trust badges

State exactly what was checked: “KVK registration checked · date,” not “quality guaranteed.” Completion labels distinguish both-party confirmation from client-reported completion. New businesses display “New on HoutMatch,” not fabricated ratings or a punitive zero-star graphic.

## 7. Screen composition

- **Landing:** client-first hero; category choices; how it works; consented real portfolio work; privacy explanation; separate professional pricing CTA. No unverified counters or testimonials.
- **Job wizard:** one main question group per step; progress label; persistent draft status; contextual measuring guidance; review preview before submission.
- **Carpenter feed:** filter bar and vertical cards; default list, not a precise-address map. Explain distance is city-based. Cap filters to category, radius, timing and budget initially.
- **Thread:** lifecycle header; brief revisions; attachments; shared board; clearly distinct private workspace; receipt/policy reference.
- **Client comparison:** consistent dimensions and price inclusions; decision summary; explicit contact sharing; confirmation before award.
- **Operations:** table-first queues with case ownership, reason codes and audit timeline. Sensitive fields remain masked until justified access.

## 8. Feedback, motion and resilience

120ms control feedback, 180ms panel transitions, opacity/transform only. No parallax, scroll-jacking, animated wood textures or payment celebration that obscures the receipt. Honor reduced motion.

Skeletons match final geometry; announce asynchronous success/error via appropriate live regions without reading the entire chat. Payment pending is not success. Display a stable receipt after unlock; on timeout, query operation status instead of asking users to pay again.

Sticky mobile action bars respect safe-area insets and cannot cover form errors, keyboard focus or message composers. Avoid shifting buttons when notification badges load.

## 9. Photography, iconography and voice

Use authentic consented work photos, natural light and detail shots that explain craft. Strip metadata and screen for identifiable people/addresses before publishing. Do not imply stock photos are completed platform projects.

Use consistent 20–24px outlined SVG icons with visible text for critical actions. No emoji as UI icons. Decorative woodgrain is limited to a subtle marketing illustration; never behind text or inputs.

Voice: direct and respectful, no “exclusive lead jackpot,” countdown pressure or guaranteed-income copy. Explain unknowns instead of pressuring users to guess. A professional payment buys access, not ownership of a person or their data.

## 10. Do / don't

| Do | Don't |
|---|---|
| Keep €1 incl. VAT and €10 funding commitment visible | Advertise €1 while hiding minimum top-up |
| Show actual lifetime slot usage | Use fake scarcity or replenishing sales slots |
| Use a server-returned safe preview | Blur sensitive DOM/JSON as a paywall |
| Keep shared scope and private offers separate | Expose competing quotes to create a bidding contest |
| Show honest verification/feedback labels | Suggest KVK checks guarantee workmanship |
| Explain expired, paused and restored states | Let an unavailable job remain purchasable |
| Preserve drafts and show send failures | Pretend an offline message was delivered |

## 11. Implementation handoff and review

Use these tokens as the source for eventual CSS variables and a component catalogue; no styling framework is mandated by this document. Implement empty/loading/error/disabled/focus/pending/restored states before visual sign-off. Test 320, 375, 768, 1,024 and 1,440px widths, Dutch and English copy, zoom, touch and keyboard.

Reject a handoff with unreadable price disclosure, ambiguous audiences, tiny controls, fabricated proof, unauthorized preview data or untested monetary flows even if it looks polished. Wireframes and user tasks are in [UX](03-ux-and-service-blueprint.md); objective release checks are in [Test plan](11-test-and-launch-plan.md).
