# Backend language and runtime options

**v0.4 scope note:** The selected technology/provider findings below remain reference material. Current product scope is the [semi-public-benefit three-board free/subscription redesign](22-platform-redesign.md); v0.3 €1 publication flows and their estimates are historical. Do not infer current billing, unlimited free reading or task-only MVP scope from comparison/inspection history.

**Updated:** 22 October 2026. **Status:** TypeScript and Hono explicitly confirmed for the Cloudflare Workers backend, including the language direction for possible future AI integrations; React Router with React/Vite/SSR is confirmed for the frontend, while exact package versions remain open. No project benchmark or runtime qualification has run.

## Selected backend and performance assessment

Choose **TypeScript on Cloudflare Workers** for this MVP. The founder delegated the TypeScript-versus-FastAPI choice with performance, low latency and high concurrency as priorities; see [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md). The choice no longer waits for a maintainer-language answer and does not assume any particular team expertise.

The deployed code executes as JavaScript in the Workers runtime. Python Workers uses Pyodide/CPython compiled to WebAssembly inside an isolate. My expectation is that avoiding that Python runtime layer gives TypeScript a better starting point for low-overhead API execution; the runtime architecture alone does not establish a numerical throughput or tail-latency advantage for this application. Cloudflare also optimizes Python initialization using deployment-time snapshots, so do not assume ordinary container cold-start numbers apply. Sources: [Workers runtime](https://developers.cloudflare.com/workers/reference/how-workers-works/), [Python runtime and snapshots](https://developers.cloudflare.com/workers/languages/python/how-python-workers-work/).

| Criterion | TypeScript Workers assessment | FastAPI/Python assessment |
|---|---|---|
| API processing overhead | Preferred starting point: direct JavaScript execution and platform bindings | Additional Python execution environment; actual overhead depends on request/dependencies |
| User-visible latency | Expected runtime advantage is only one component; measure database/network/pool/lock waits | Can be competitive when external waits dominate; no application result is available |
| Concurrent I/O | Async handlers can overlap database/API waits; bound downstream load | Async handlers and compatible database clients can also overlap waits |
| CPU-heavy work | Avoid long synchronous processing in request handlers | Also needs bounded or separately executed heavy work; async alone does not add CPU parallelism |
| Project integration | One language for backend and frontend contracts/tooling | Requires a Python backend toolchain and cross-language contracts, still viable |

FastAPI supports [async concurrency](https://fastapi.tiangolo.com/async/). High concurrent-user counts do not establish a language winner: PostgreSQL capacity, transaction contention, connection/pool limits, query count, response size and external services constrain the result. Treat these as measurement dimensions rather than claiming the database is already a measured bottleneck. [Hono](https://hono.dev/docs/getting-started/cloudflare-workers) is confirmed as the thin Workers HTTP adapter; its exact version, validation/auth packages and integration still need qualification. The founder subsequently confirmed React Router framework mode with React/Vite/SSR for the frontend; this does not duplicate the Hono API's domain rules.

Before beta, load representative safe list/detail reads, owner-private offers, durable callback intake and local publication transactions against real PostgreSQL. Use the existing NFR-03 workload of 100 concurrent active users and 10,000 tasks, with a controlled ramp and an explicit request mix. Report achieved requests/second rather than equating user count with throughput, and record warm/first-request p50/p95/p99 latency, CPU, errors, database round trips, pool/lock waits and job backlog. Preserve the existing targets of p95 safe reads below 500ms and local publication below 1s excluding PSP/network time; label end-to-end measurements separately. These are validation targets, not achieved figures or an SLA. No synthetic hello-world comparison replaces the financial/privacy race tests.

## Assessment

FastAPI, Go and Rust can all implement this platform. Compare deployable stacks: FastAPI is a Python web framework; Go is a language with an HTTP server library; Tokio is Rust's asynchronous runtime, commonly paired with an HTTP layer such as Axum. Language choice and hosting execution model are separate decisions.

| Backend stack | Workers path | Native server/container path | Project assessment |
|---|---|---|---|
| TypeScript | Direct Workers application and platform bindings; same frontend/backend language | Ordinary Node server also possible | Selected for this MVP under the delegated choice: expected lower runtime overhead and simpler shared tooling, subject to real-route validation |
| Python + FastAPI | Official ASGI integration in Python Workers; documented PostgreSQL/Hyperdrive drivers | Conventional ASGI server in a container | Strong alternative if maintainers prefer Python; validate exact package, auth, SQL and media dependencies in the target runtime |
| Go HTTP API | Go-to-Wasm is possible, but not the same deployment as a normal `net/http` listener | Normal Go HTTP server and PostgreSQL client | My preferred additional candidate when a dedicated native backend is wanted and maintainers know Go; avoid choosing an unusual Wasm toolchain just to preserve Workers-only execution |
| Rust + Axum/Tokio | Rust Workers use `workers-rs`/Wasm and the Workers event model | Conventional Axum/Tokio server and native SQL driver | Appropriate with Rust expertise or demonstrated requirements; no measurements currently justify choosing it for throughput alone |

The alternatives remain documented for context. These are project judgments, not measured rankings of speed, memory use or developer productivity. Maintainer experience remains unknown; selection followed the founder's later request to choose using Cloudflare fit and performance priorities.

## Current platform facts

Cloudflare documents [FastAPI's ASGI entrypoint](https://developers.cloudflare.com/workers/languages/python/packages/fastapi/) in Python Workers. It also documents [Python Hyperdrive drivers](https://developers.cloudflare.com/hyperdrive/examples/python-workers/), recommending `asyncpg` for PostgreSQL and listing alternatives. That guide currently says async SQLAlchemy ORM support is unavailable because greenlet is unavailable, although synchronous SQLAlchemy is supported. Do not generalize this restriction to all Python async database access; qualify the actual driver/transaction implementation.

Cloudflare lists TypeScript, Python and Rust as first-class Workers languages; [Go is available through WebAssembly](https://developers.cloudflare.com/workers/languages/). Rust's documented Workers route uses [`workers-rs` and the JavaScript event loop](https://developers.cloudflare.com/workers/languages/rust/). A native socket-listening Tokio server is not a drop-in deployment in that environment. Tokio itself documents [restricted/experimental Wasm support](https://docs.rs/tokio/latest/tokio/#wasm-support). Some Rust components can be reused, but the runtime, networking and database crate compatibility must be checked.

[Cloudflare Containers](https://developers.cloudflare.com/containers/) provides a native-runtime option within Cloudflare. Containers are started and controlled through Workers; the [getting-started example runs a Go web server](https://developers.cloudflare.com/containers/get-started/). A conventional FastAPI or Axum/Tokio container can also be evaluated. Container lifecycle, routing, startup, placement, database connectivity, scheduled wake-up and total billing need their own qualification. The earlier Workers-only cost examples do not price this topology.

Framework/library references: [FastAPI validation/OpenAPI](https://fastapi.tiangolo.com/features/), [Go `net/http`](https://pkg.go.dev/net/http), [Axum](https://docs.rs/axum/latest/axum/) and [Tokio](https://docs.rs/tokio/latest/tokio/). Pin versions only after choosing the runtime.

## Application boundaries

Possible AI customer-service and knowledge/Wiki features are assessed separately in [AI service and knowledge options](20-ai-service-and-knowledge-options.md). The recommendation is to retain TypeScript for platform/model integration and add Python only where model development, custom inference or processing jobs benefit from it. This hypothetical extension does not add AI to the current MVP or replace the accepted backend choice.

Changing the backend language does not require domain microservices. One backend application can own identity, task, company, offer and billing modules, share its domain code with scheduled/job entrypoints, and use PostgreSQL transactions. A TypeScript frontend plus a Python/Go/Rust API is a separate frontend/backend deployment; the backend can still be a modular monolith.

If frontend and backend use different languages, define the API contract and generate/check client types rather than duplicating domain rules. The backend is the authority for guest ownership, staff/company permissions, payment/publication and private DTOs. Route authenticated API requests consistently, including server-rendered requests if used, and preserve cookie/origin/CSRF controls. A frontend login check does not replace backend authorization.

The selected TypeScript/Workers backend retains the confirmed durable PostgreSQL inbox/outbox/jobs with bounded Cloudflare Cron recovery, an initial one-minute poll and optional immediate attempts after durable commit, plus the cache-disabled Hyperdrive application binding. A native container backend may use its own PostgreSQL driver and bounded pool over a reviewed TLS/network path; do not assume a Workers binding or isolate connection recipe automatically applies inside a container. Keep one authoritative financial journal and publication/refund implementation.

## Foundation qualification and unchanged rules

Before selecting dependencies and estimating delivery, prove the chosen stack against real PostgreSQL transactions/rollback, concurrent first publication, durable Mollie callback intake, ambiguous external results, current consent/expiry reads, interrupted job recovery and private photo sanitization. In-memory FastAPI background tasks, Go goroutines or Tokio tasks alone do not provide the required durability. Container restarts and scale-down also require persisted obligations and an accountable wake-up/retry mechanism.

All candidates must preserve the current free/low-price-subscription model with no separate offer-publication charge, Mollie-only integration, guest-only client MVP, private offers, contact rules, alert preferences and erasure schedules. Subscription refund terms still require a decision; the former €1 policy is historical. Memory safety or a fast HTTP benchmark does not prove business idempotency or correct authorization. No language selection silently relaxes the existing recovery targets or approves Supabase, a database plan, Containers or another hosting service.

TypeScript/Hono on Workers and paid Render PostgreSQL in Frankfurt are selected; see [ADR 0009](../docs/adr/0009-use-render-postgresql-in-frankfurt.md) for the later database confirmation. React Router with React/Vite/SSR is also confirmed; qualify exact package versions, database plan/driver, job and media dependencies next. The earlier delivery estimate remains an unvalidated baseline until runtime qualification is complete; these choices alone do not establish capacity or latency.
