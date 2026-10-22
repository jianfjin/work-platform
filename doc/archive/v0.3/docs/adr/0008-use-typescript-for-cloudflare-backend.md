---
status: accepted
---

# Use TypeScript for the Cloudflare Workers backend

On 22 October 2026 the founder asked the assistant to choose between TypeScript and FastAPI for performance, latency and concurrency. Select TypeScript for the MVP backend on the requested Cloudflare Workers platform: JavaScript runs directly in the Workers runtime, while Python Workers adds the Pyodide execution environment. Expected lower runtime overhead and shared frontend/backend tooling favor TypeScript here; this is an architectural judgment, not a measured claim that every TypeScript endpoint outperforms FastAPI.

Keep one modular backend with PostgreSQL transactions, durable job handlers and Mollie-only payments. FastAPI remains technically viable; no project benchmark or maintainer-language preference was supplied. Qualify a thin HTTP adapter, exact dependencies and deployed-runtime behavior during foundation work, measuring real database-backed routes and existing acceptance targets. This decision selects the backend language/execution direction, not a frontend framework, database provider, production capacity or deployment account. See the [backend assessment](../../doc/19-backend-options.md).

The founder subsequently explicitly confirmed TypeScript after reviewing possible AI customer-service and knowledge/Wiki features. Keep TypeScript as the platform backend for those integrations; optional Python model-development or inference components remain a future proposal, not an approved second backend or AI MVP scope.
