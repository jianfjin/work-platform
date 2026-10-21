# Cross-platform delivery: Web/PWA first
**Version:** 0.3 · **Updated:** 21 October 2026
User-confirmed delivery direction retained; commercial workflow revised to free guest clients and company-paid published responses.

## 1. Phases
| Phase | Delivery |
|---|---|
| Current | Desktop responsive Web; Android browser/home-screen PWA; iPhone Safari/home-screen PWA; Dutch/English |
| After useful beta | Improve PWA, optionally evaluate Web Push with consent and fallback |
| Mature product, G4 | Dedicated Android/iPhone clients plus existing Web/PWA; separate technology/store/payment/security/budget decision |

Installation is optional. Both client and company can complete posting/reading/rough-offer/payment/contact/closure/support flows in an ordinary supported browser. No desktop binaries, store wrapper, native receipt infrastructure or mandatory notification permission now.

## 2. Support target and installation
Record exact tested versions per release. Initial target: current/previous stable Chrome/Edge/Firefox on declared desktop OS versions, macOS Safari within its supported current/previous major OS window; Android Chrome on declared devices/OS; iPhone Safari current/previous supported major iOS releases in browser and installed modes. Test actual coverage rather than advertise every combination.

Use manifest with stable ID/start/scope, standalone display and reviewed normal/maskable/Apple touch artwork. Android install action is feature-detected; iPhone help follows [Apple's Safari home-screen guidance](https://support.apple.com/guide/iphone/open-as-web-app-iphea86e5236/ios). No universal automatic install prompt: [MDN documents differences](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable). Keep browser fallback if installation unavailable/declined.

Working name is not a confirmed brand. Do not manufacture icons/binaries or publish store accounts as part of this documentation revision.

## 3. Same backend, secure guest contexts
Server records own tasks, guest/account scopes, company offers and fees. No device-local “paid access” or balance. Guest session is job-scoped; company session is account-scoped. Optional client registration does not reset task deletion.

Cookies need not transfer between Safari, installed PWA and bank return. A guest link may require a fresh one-use exchange in the chosen context. A returning company signs in and recovers owned pending payments/responses, not another automatic checkout.

## 4. Offline/privacy contract
Browser worker is not the backend deletion/mail worker. It may cache exact allowlisted public JS/CSS/fonts/icons and a non-personal offline page only. All task feeds/private pages/RSC/API/photos/offer/contact/billing/auth/payment content is network-only. No generic app navigation cache, private IndexedDB/localStorage/sessionStorage draft, or financial/message Background Sync queue.

In-memory unsaved text may survive only while the current view remains; explain reload/OS kill can lose it. Saved job drafts are server-side, not an offline replica. After resume/reconnect/history restore, hide stale private fields until ownership/current task/contact/delete_at state revalidates.

Three-month access cutoff applies on every device; uninstall/old worker cannot restore expired content. An explicit user download/screenshot outside app-managed storage cannot be remotely erased.

## 5. Company €1 checkout recovery
Read task and draft free -> explicit first-publication checkout -> server verifies paid plus current task/revision -> private client-visible response or tracked full refund.

When bank app opens another browser, require owned-session recovery and query /me/payments or own response. Navigation/redirect is not proof of payment. Closed/expired/materially changed task at fulfillment produces no offer and a refund, not a wallet credit. Reopening/editing/another device never recharges an already fulfilled response.

Future dedicated mobile apps must reassess then-current storefront digital-service fee/payment/receipt rules; web PSP economics are not presumed unchanged. No construction payments in either current scope or a silent native extension.

## 6. Mobile UX and permissions
Use visible standalone back/home, safe areas, keyboard-safe controls and private client/company audience labels. Offer optional camera with ordinary file selection fallback, scan/EXIF handling and clear unsupported HEIC alternative. Dutch/English fee, callback-audience and deletion copy must remain legible.

P0 notifications are opt-in company emails and generic client service notices plus in-app refresh. Background JavaScript is not a delivery guarantee. Optional later Web Push needs feature/permission checks, unsubscribe and generic lock-screen text. [WebKit's introduction](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/) describes iOS Home Screen permission support; it is not proof of our actual tested device matrix.

## 7. Updates and real-device evidence
Revalidatable worker script, versioned public caches, retained compatible assets and a safe “update available” action. Never force reload during unsaved work/upload/checkout or replay writes on activation. Reconcile unknown prior payment before retry. Logout/account switch clears current sensitive view and any accidental app-owned private cache, then reauthenticates appropriately.

FR-14/NFR-05 and AT-15/20/21 cover real Android+iPhone browser/installed launch, photo permissions, keyboard, bank return, process termination, offline/cache isolation and worker update/rollback. T10/T12 implement/test them. Emulated viewports alone cannot pass G2. No app/device test has been executed in this documents-only task.

## 8. Maturity for dedicated apps
Proposed G4 evidence: G3 useful liquidity/trust/margins; at least two stable 30-day cohorts; a demonstrated user need not reasonably met by improved PWA; funded release/support ownership. Then design native auth/deep links/media/notifications/accessibility and storefront payment/refund/accounting separately, choose technology and approve budget. Preserve server-owned users/fees/rights without charging a migration repurchase. No date or app-store approval is promised.
