# Product and interface design
**Version:** 0.4 · **Updated:** 22 October 2026

## Product character
A practical, welcoming, low-fee community platform for ZZP and SMEs. State the semi-public-benefit mission in plain language; do not imply government approval, charity registration, guaranteed work or certified participants. Favor useful content, readable forms and transparent service terms over pressure-driven upselling.

## Information architecture
| Destination | Core interface |
|---|---|
| Client tasks / Opdrachten | Expanded trade selector, free client posting, safe company discovery and client-private offer list |
| Cooperation / Samenwerken | Work-package scope/time/skills/area, publisher and private expressions of interest |
| Tools/materials / Gereedschap & materialen | Item/specification/condition/quantity/price/area, safe photos, inquiry and availability |
| Free / Subscription | Clear five-item monthly allowance versus unlimited numerical access; actual price/renewal terms must be selected |
| My account | Own posts/replies, plan/allowance, alert preferences, billing, support and privacy requests |

Board tabs and labels must remain distinct on mobile. Do not mix a client offer with an item selling price or label an expression of interest as a completed contract/sale.

## Shared components
- Board-aware information cards show minimal category/area/status previews without sensitive client/project/pickup details.
- A free-plan allowance indicator explains used/remaining items and period; exact counting copy follows the approved policy, not hidden implementation behavior.
- Detail access is deliberate; list prefetch and back navigation cannot unexpectedly spend allowance. Show a useful quota-exhausted state and an explicit subscription action.
- Owner editing, own replies, receipts and privacy requests remain available without consuming information allowance under the proposed scope.
- Work-package publishing warns about original-client confidentiality; a parent reference never auto-copies restricted fields.
- Trade forms label item condition, quantity/unit, price/tax basis and pickup/delivery separately; no fake stock, review or sold count.

## Privacy and original task behavior
Client task contact is private by default. Callback opt-in names fields and the all-eligible-responders audience, and can be revoked. Offers remain private to the task client; companies see their own only. No rival count, slot sales or artificial scarcity.

Client tasks expire three calendar months after first publication; show their deadline. New-board lifecycle/retention needs a separate policy. Optional client-task media remains six photos, 10 MB each, JPEG/PNG/WebP/HEIC to sanitized JPEG/WebP; propose reuse for new boards. Do not equate metadata removal with removal of visible faces/addresses.

## Language, accessibility and PWA
Dutch/English critical copy, keyboard/screen-reader operation, 320px reflow and real Android/iPhone tests. Plain shapes/spacing/color contrast should make board, plan, visibility and status legible without relying on color. Browser operation must work without installation.

Cache only public static assets. No private detail/offer/contact/media/payment data, durable personal draft or offline quota-consuming write queue. Resume/reconnect revalidates server state; checkout return can require login and never implies payment success.

The active business scope is [v0.4](22-platform-redesign.md). The [v0.3 design](archive/v0.3/doc/DESIGN.md) is historical; its free-unlimited reading and €1 offer call-to-action must not reappear as current copy.
