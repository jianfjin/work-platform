---
version: "0.3-proposal"
name: work-platform-local-workbench
description: "A practical local maintenance marketplace: free guest posting, private rough offers, explicit company response fees and client-controlled contact."
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

# Work Platform — Local Workbench
**Version:** 0.3 · **Updated:** 21 October 2026 · **Status:** original proposed visual system, not implemented or certified

## 1. Reference method and brand
Follow [awesome-design-md](https://github.com/VoltAgent/awesome-design-md): semantic tokens, typography, spacing, component anatomy and all important states in one agent-readable contract. The inspected Notion/Linear examples informed specification discipline, not copied logos, fonts, copy or brand palettes.

“Work Platform” is a placeholder until founder selects/clears a name. Replace previous HoutMatch public branding because the revised scope includes painting, roof and other maintenance, not carpentry alone. Use daylight neutrals, forest actions and restrained wood accent; make a practical local service, not a price-auction dashboard.

## 2. Content hierarchy
Public hero leads with **Post your task free**, region/trade fit and optional account, not a client €1 price. Company page clearly says task reading is free and the proposed first successfully published offer costs €1.00 incl. VAT. Never hide the charge event in small print or display a compulsory €10 top-up.

Native/system body fonts, at least 16px inputs/body; public editorial heading may use Georgia. App sections use system sans. Target 4.5:1 normal text, 3:1 essential control boundaries, visible focus and complete-flow WCAG 2.2 AA evaluation. Decorative borders cannot be the sole input boundary. Token contrast still needs actual component/state verification.

Use the frontmatter's semantic colors, 4px spacing scale, 8px control/12px card radius, 1200px max content and 680px form width. Mobile 16px gutters, 44–48px touch controls, keyboard-safe placement. Check 320/375/768/1024/1440px, zoom, Dutch/English expansion and portrait/landscape. No untested dark mode.

## 3. Essential components
| Component | Contract |
|---|---|
| Free-posting entry | “No account required; email link to manage securely.” No payment/registration wall |
| Lightweight task form | Description, postcode/city, optional few photos; email management step; budget/timing optional; truthful unknowns |
| Company task card | Category, city/PC4, safe description/photos, approximate radius fit, status/deletion date; free reading; no competitor identities/count or remaining slots |
| Rough-offer composer | Own introduction, indicative range or visit-needed, availability, own website/contact details; private to client |
| Fee confirmation | €1 gross, publication event, no work/reply guarantee, all eligible companies can respond, refund policy; deliberate confirm/cancel |
| Client private offer list | Every eligible submitted offer via pagination; own company contacts/website and assumptions; default received order, not lowest-price-wins |
| Contact preference | Two distinct choices; CLIENT_INITIATED default; ALLOW_RESPONDERS explicit fields/audience and copied-data limitation |
| Close task | Client control at any time; explain no further offers/alerts/callback disclosure; existing history only until delete_at |
| Retention notice | Actual resolved deletion date; linked live-purge/backup/finance exceptions; edits/reopen never reset |
| Report/case view | Reason/status and correction/appeal, factual safety action; no automatic public accusation or “quality verified” shield |
| Payment status | Pending/published/refund-pending/refunded; no inferred success, wallet-credit substitution or double-pay prompt |

A company sees only its own response/status. Do not reuse the client list component with hidden competitor CSS/data: server must return no rival records. There is no shared commercial board, in-app chat/award screen, verified-completion review widget or commercial three-firm cap in current scope.

## 4. Contact audience design
Default label: **I will contact the companies**. Show company contacts to client, not client contacts to all companies.
Alternative: **Companies whose offers are published may contact me**. Explain audience can exceed three, select email and/or phone separately, and require clear confirmation before enabling. Do not precheck because a phone field is populated.

Revoking/closing blocks future disclosure, not contacts already copied. Full postcode/address is not an ordinary company card field. Keep mode and audience readable in both languages; critical information never tooltip-only.

## 5. Interaction states and copy
All actions have idle/hover/focus/disabled/pending/error/success states. Pending button prevents repeat taps without replacing server idempotency. Preserve width and meaningful error labels. An expired guest token offers fresh-link recovery, not mandatory account creation.

Dutch examples: “Plaats gratis je klus”, “Lezen is gratis”, “Offerte indienen: €1,00 incl. btw”, “Ik neem zelf contact op”, “Reagerende bedrijven mogen contact opnemen”, “Sluit mijn klus”. Native-speaker review before release.

Closed/expired/scope-changed tasks explain why no offer can publish; a captured failed attempt shows a cash-refund status, not a success confetti animation. No fake scarcity, invented testimonial/counter, guaranteed work or “all companies verified” claim.

## 6. Photos, icons and geography
Use actual consented images with clear captions; never imply stock work is a platform result. Private task photos do not become marketing assets. Strip metadata, scan and provide report/remove; a visible address in a photo needs privacy handling, not a false assurance that EXIF removal solves all leaks.

Consistent SVG icons, not emoji controls. Area/distance labels say approximate straight-line, not driving time. No exact-home map pin or mandatory GPS. Generic regional illustrations must not identify a client.

## 7. PWA and resilience
Optional install help is contextual; browser usage works fully. Standalone mode has own back/home navigation, safe-area insets and software-keyboard-safe offer fields. File picker works if camera declined; unsupported image types have an honest alternative.

Offline page is public and non-personal. No cached job/photos/offers/contact/payment data or offline transaction queue. In-memory unsent text may be lost on reload/OS termination; do not claim “saved offline.” Resume/login/account switch hides stale private content pending authorization. Updates defer reload until saved online/discarded; bank-app return uses verified server state, not window appearance.

## 8. Handoff checks
Implement fee/contact/deletion/guest/close/refund states before visual sign-off. Test raw data and all audiences, not just screenshots. Reject a polished handoff with client charging, rival data, ambiguous callback permission, misleading quality claims or private offline storage. See [UX](03-ux-and-service-blueprint.md), [PRD](02-product-requirements.md) and [tests](11-test-and-launch-plan.md).
