# Cross-platform delivery: Web/PWA now, dedicated apps later

**Version:** 0.2 · **Decision date:** 20 October 2026
**Status:** platform direction explicitly confirmed by the user; implementation and remaining launch gates are not approved/completed

## 1. Confirmed scope and phases

The platform must run on **desktop, Android and iPhone**. The user selected mobile option 2 for now—browser access plus an installable progressive web application—and option 1, dedicated Android/iPhone applications, after the product matures.

| Phase | Delivery | Boundary |
|---|---|---|
| Current MVP/paid beta | Responsive desktop website; Android browser + home-screen PWA; iPhone Safari + home-screen PWA | One Web/PWA codebase and shared backend; installation optional; no App Store/Google Play submission or desktop binaries |
| After beta, if justified | Improve PWA usability; optional Web Push after separate approval | Browser capability detection and email/in-app fallback; no mandatory notification permission |
| Later, maturity-gated | Dedicated Android/iPhone clients alongside the desktop website | Separate G4 decision, architecture/store/payment assessment, budget and release acceptance; not an automatic date-based rollout |

All client/professional core tasks must work in the ordinary browser: sign-in, job publishing, photo/file upload, safe previews, funding/unlock, shared/private conversations, quotes, selection, review, billing history and support. Staff operations can remain desktop-first but must not weaken authorization on mobile requests.

This decision changes delivery/testing scope, not the commercial agreement. The €1 gross one-time unlock, €10 funding proposal, three lifetime businesses and refund/consent rules remain authoritative in [PRD](02-product-requirements.md) and [Payments](08-payments-and-economics.md).

## 2. Platform support contract

These are **release targets**, not verified compatibility claims. At T16, QA must record exact OS/browser versions and devices used, publish the supported-version policy and recheck at each release. “Latest” is evaluated at release time, not permanently fixed to this document's date.

| Surface | Initial support target | Release proof |
|---|---|---|
| Windows/Linux desktop | Chrome and Firefox current/previous stable releases on declared supported OS versions; Edge on Windows in the same browser-version window | Keyboard/mouse, upload/download, complete financial/user flows; record OS coverage rather than assuming all combinations |
| macOS desktop | Safari on current/previous supported macOS major releases; current/previous stable Chrome | Core flows and payment/return; optional browser installation where available |
| Android | Chrome current/previous stable releases on declared supported Android versions | Real phone in browser and installed modes; normal/denied camera permission, bank-app switch, resume/offline/worker lifecycle |
| iPhone | Safari on current/previous iOS major releases supported at launch | Real iPhone in Safari and Add to Home Screen mode; installation, keyboard/safe areas, file selection, checkout/session recovery |

The exact Android minimum OS and physical-device inventory are G0/T16 decisions based on audience/device access, not guessed from a browser feature table. Include at least one small-screen phone and one less capable target device. Emulators and Playwright viewport profiles assist regression testing but do not replace real iPhone/Android installation and bank-app return tests. Other browsers receive a usable standards-based fallback where possible; do not advertise untested install/push capabilities.

Installation, notifications and device capture are progressive enhancements. If an install prompt is unavailable or dismissed, keep the browser flow fully functional. If the browser blocks required cookies/network access, explain the limitation; do not bypass secure authentication.

## 3. Delivery architecture and installation

Reuse the Next.js UI, versioned `/api/v1` contracts, server-only domain services, PostgreSQL ledger/entitlements and outbox worker. Add a manifest, installation artwork/help and a narrowly scoped browser service worker. [Next.js PWA guidance](https://nextjs.org/docs/app/guides/progressive-web-apps) supports manifest integration; our cache/transaction restrictions below remain mandatory.

Proposed manifest contract: stable `id` `/`, `name` HoutMatch, short name HoutMatch, `start_url` `/`, `scope` `/`, `display` `standalone`, theme/background matching DESIGN.md, standard 192/512px icons plus a reviewed maskable variant and Apple touch icon. The root/start route performs an online session-aware redirect; it is not a cached authenticated app shell. Treat all paths/assets as proposed implementation choices, not currently existing files.

On Android, offer the supported browser install action or contextual browser instructions. On iPhone, use Safari Share → Add to Home Screen guidance and the web-app option if shown by that OS version, following [Apple's instructions](https://support.apple.com/guide/iphone/open-as-web-app-iphea86e5236/ios). Do not claim installation is Safari-only across all iOS releases. The custom `beforeinstallprompt` flow is not universal; [MDN documents platform differences](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable). Detect capabilities rather than faking an install button.

Do not introduce Capacitor/TWA/native wrappers, Electron, React Native or Flutter builds in the current phase. Future mobile technology is deliberately unselected; sharing TypeScript policy contracts is useful but does not imply React DOM screens can be reused unchanged in a native UI.

## 4. Offline and local-data policy

**Online-first, private-data network-only.** A PWA is not a promise that paid threads, chat or payments work offline. This limitation is intentional to keep home-interior media, contact data and access decisions out of durable device caches.

| Resource/action | Device policy |
|---|---|
| Explicitly allowlisted versioned public JS/CSS/fonts and icons | May cache as non-personal build assets; version and remove obsolete app-owned caches |
| Dedicated non-personal offline page | May cache; show explanation, help and retry; never fabricate a balance/status |
| Personalized HTML/RSC/navigation, safe live-job feed, full jobs, chat, quotes, profile/account data, billing and all APIs | Network-only; no offline replica, stale-while-revalidate private response or generic navigation precache |
| Job media, signed downloads, authentication and PSP/payment return responses | Never service-worker cached; no offline downloads managed by the app |
| Sensitive unsent composer/job text | Current-view memory only until saved to server online; no IndexedDB/localStorage/sessionStorage copy of sensitive content |
| Unlock, top-up, award, message send, refund or contact-sharing mutation | No offline queue, background sync or automatic replay after reconnect |
| Non-secret pending-operation reference | May retain a minimal scoped reference for recovery; it grants no access and contains no token, payment proof, private text or balance |

Use an explicit response allowlist in worker code; do not assume `Cache-Control: no-store` prevents a custom worker from caching a response. [Service-worker lifecycle guidance](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers) explains the browser mechanism; this table is our stricter application policy. Browser-managed history and memory also need tests; an absence from Cache Storage alone is not sufficient.

If connectivity is lost, disable new privileged actions and show honest unsent/pending states. Keep in-memory drafts only while the current view survives, with a warning that reload/OS termination loses them. A previously server-saved job draft resumes after online sign-in. Explicitly selected user downloads/screenshots outside app-managed storage cannot be remotely erased; do not promise otherwise.

On foreground/back-forward restore and account switch, hide stale private UI until an online session/permission/state check succeeds. Logout clears current sensitive UI/drafts and coordinates same-origin open contexts; it does not mean other devices were globally signed out unless the user explicitly requests account-wide session revocation.

## 5. Cross-device identity and mobile checkout

One account/business owns server-side jobs, offers, balance and lifetime entitlements. Android, iPhone and desktop do not create separate purchases or balances. Each context authenticates independently as needed; do not assume cookies/session state transfer from a browser to an installed PWA or vice versa.

1. Create the owned €10 intent on the server, with stable operation identity; use hosted checkout as already designed.
2. A bank/PSP can open another app/window and return somewhere other than the original PWA. Treat that return as navigation only, not evidence of successful payment.
3. If the returning context lacks a valid session, reauthenticate and verify the same intent owner; do not expose payment/job details in an unauthenticated return page.
4. Retrieve owned pending top-up references via the account balance response and query verified status. A terminated original view must not cause another top-up to be created by default.
5. Only authenticated PSP verification grants funds once. A paid top-up still does not reserve a job or automatically unlock it; refresh the preview and explicitly confirm €1.
6. If an unlock request's outcome is unknown, look up the owned entitlement/receipt or operation result before retrying. Cross-device lifetime uniqueness protects against a second job-access charge even with a different request key.

On resume/reconnect, refetch job availability, price/policy version, balance and write permissions. Never use an old on-screen balance as authority. Deep links are navigation, not credentials; use an allowlisted same-origin destination after sign-in. Optional installation must not turn hosted checkout into an embedded insecure payment form.

## 6. Phone UX, media and notifications

Keep mobile tabs, shared/private audience labels and back/home navigation visible in standalone mode. Respect device safe areas, landscape and software keyboard; large touch controls and quote/price disclosures must remain usable. Test camera/file picker cancellation, permission denial and a normal file-selection fallback. Accepted formats/scanning remain in technical design; if common HEIC/HEIF images are unsupported, provide a clear compatible-format/conversion path, never a silent failed upload or unsafe decoder bypass.

P0 notifications remain generic transactional email plus server-backed in-app polling. Refresh when visible; do not depend on continuous background JavaScript. Web Push is a possible later P1 enhancement, not required to make the PWA usable.

For a separately approved push feature, feature-detect support, request permission after an explicit user action, support denial/revocation and never include job/contact/quote text on a lock screen. [WebKit's documented introduction](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/) supports Home Screen Web Push on iOS/iPadOS 16.4+ subject to permission; this is not our minimum supported iOS version or a delivery guarantee. No subscription schema/endpoints, native APNs/FCM setup or mandatory push work are included now.

## 7. Update and recovery strategy

Serve worker code revalidatably, use versioned public caches and retain compatible prior public assets during rollout. A newly waiting worker shows “Update available”; allow a safe reload after the user saves work online or explicitly discards it. Never force a destructive reload in a composer, upload or checkout, and never replay pending writes as part of activation.

Support old/new frontend API compatibility during deployment. On an outdated-client conflict, explain refresh/retry after checking earlier transaction outcomes. Operations needs a tested worker/cache rollback or replacement procedure, monitoring for stuck installations and a way to disable a faulty worker's behavior safely. Rollback must not restore old balances or reverse external payment facts.

Server state is authoritative after browser storage eviction, uninstall/reinstall or a new phone sign-in. Uninstalling removes a shortcut/local app context; it does not cancel the account, refund funds or remove entitlements. Test those distinctions explicitly.

## 8. Acceptance and delivery impact

This decision adds **FR-16, FR-17 and NFR-09**, mapped to **AT-22–AT-26** in [Test plan](11-test-and-launch-plan.md). G2 requires real-device evidence for install/browser fallback, ordinary flows, offline/cache isolation, account changes, keyboard/media handling, bank-app return, app termination and worker update/rollback. Document actual tested versions and failures. No real-device/application test has been executed in this documentation delivery.

[Delivery plan](10-delivery-plan-and-todo.md) adds P0 T16/T17: estimated 5–7 engineering days plus separately allocated real-device QA, revising the illustrative current-phase engineering range to 51–70 days. The budget/timeline remain estimates; optional push and native clients are excluded.

## 9. Gate G4: what “mature enough for dedicated apps” means

User intent is to move to dedicated apps after maturity; the following evidence checklist is proposed, not a substitute for approval:

- G3 marketplace liquidity/trust/economics gates passed; at least two consecutive 30-day cohorts show stable usage and positive contribution after variable operations/acquisition, with explicit fixed-cost funding.
- Client/carpenter research identifies a measurable unmet need that a well-functioning PWA cannot reasonably meet; app-store presence alone is not assumed to improve conversion.
- Product defines expected mobile benefit, acceptance metrics, supportable OS/device targets and ongoing release/QA ownership.
- Tech/finance/legal review then-current storefront payment rules for digital thread access, regional options/fees, VAT/refunds, receipt validation and existing cross-platform entitlements; today's web-checkout margin is not presumed unchanged.
- Approve a separate architecture for native authentication/session security, deep links, notifications/media/storage, API compatibility, accessibility and release pipelines. Select a mobile framework or native approach at that point using current evidence.
- Approve budget, staged testing and store-release responsibilities. Keep the website/PWA available and preserve server-owned accounts, purchase history and rights; no paid repurchase migration.

Passing these checks permits planning dedicated Android/iPhone apps; it does not silently authorize implementation or guarantee app-store approval. Current work remains documentation and a Web/PWA-first proposal.
