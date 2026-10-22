# AI customer service and knowledge-base options

**Updated:** 22 October 2026. **Status:** assessment of a possible future extension. After reviewing how support agents, knowledge/Wiki features and self-hosted models affect language choice, the founder explicitly confirmed TypeScript for the backend. This does not add AI features to the approved MVP scope. No model, agent framework, inference host, Python component or AI budget is selected, and no model evaluation has run.

## Language recommendation

Keep **TypeScript on Cloudflare Workers for the platform backend and model integration**. Prefer **Python for custom model inference, fine-tuning, evaluation and document-processing jobs when those are needed**. Calling a self-hosted model over HTTP does not require the caller to use the model server's language. This recommendation extends [the backend assessment](19-backend-options.md); it does not supersede [ADR 0008](../docs/adr/0008-use-typescript-for-cloudflare-backend.md).

| Responsibility | Recommended starting point | Reason |
|---|---|---|
| Platform API, ownership, company/staff permissions and Mollie rules | Existing TypeScript backend | Keep authoritative domain and financial rules in one implementation |
| Support chat, streamed replies, model calls and tool coordination | TypeScript | Fits the selected application runtime; Python is also viable |
| Knowledge/Wiki UI, approved document search and retrieval-based answers | TypeScript | Retrieval and generation can use HTTP services; Python is not required merely to add a knowledge base |
| Complex document ingestion, custom embeddings/reranking and evaluation jobs | Python when its libraries help | Separate offline processing from interactive request handling; simple Markdown ingestion can stay in TypeScript |
| Open-weight model adaptation or a model-specific inference implementation | Python | Direct access to the model's Python/PyTorch/Transformers implementation and training tools |

Both languages have agent frameworks: [LangGraph JavaScript](https://docs.langchain.com/oss/javascript/langgraph/overview) and [LangGraph Python](https://docs.langchain.com/oss/python/langgraph/overview) document orchestration, streaming, persistence and human review. Cloudflare also provides [Agents SDK examples in TypeScript](https://developers.cloudflare.com/agents/getting-started/quick-start/) and [multi-model pipelines](https://developers.cloudflare.com/agents/concepts/calling-llms/). These are candidates, not selected dependencies; their runtime, persistence and processing-location behavior still require qualification.

Python is my preferred model-development environment because of tools such as [Transformers fine-tuning](https://huggingface.co/docs/transformers/training). FastAPI supplies an HTTP interface around custom Python logic; it is not the inference engine. A model server such as [vLLM already exposes HTTP APIs](https://docs.vllm.ai/en/latest/serving/online_serving/), so a separate FastAPI wrapper is unnecessary unless it adds required behavior. Exact architecture/model support must be checked, especially for custom decision heads.

If one language is mandatory, choose TypeScript for a platform that integrates existing models through APIs. Choose Python/FastAPI instead if developing/adapting models becomes a central, near-term part of the product. The latter would be an explicit reconsideration of the backend decision, not a conclusion that ordinary AI features require a rewrite. Team expertise is still unknown.

## Model roles and Jev

An LLM can write conversational replies, summarize complaints, answer from retrieved approved documents and draft Wiki articles. A decision model can select among defined intents, score a case against a rubric or recommend escalation. A structured answer or probability does not establish that the judgment is correct, and a decision model alone does not write a conversational knowledge-base answer.

Assuming “Jev” means TypeSafe AI's model, its [official introduction](https://docs.typesafe.ai/introduction) describes typed choices, scores and yes/no judgments. Its [quick start](https://docs.typesafe.ai/introduction/quickstart) provides Python, JavaScript and TypeScript clients for a hosted API. The reviewed official material does not establish a downloadable Jev-weight release or local serving path; do not plan self-hosting Jev from the name alone. A clarification request about the intended project remains unanswered.

As an example of the separate open-weight category, Oz Labs publishes [Typical weights](https://huggingface.co/OzLabs/typical-small) and a [Python inference implementation](https://github.com/ozlabsai/typical). This is a candidate for evaluation, not a model recommendation or a claim of production accuracy. Verify the exact artifact/license, language coverage, hardware/runtime support and task performance before selection. TypeSafe's [model documentation](https://docs.typesafe.ai/models) says English performs best; evaluate Dutch and English separately for this platform.

## Proposed deployment and domain boundary

The Web/PWA would call the TypeScript backend, which authenticates the user, retrieves permitted knowledge and coordinates model calls. A self-hosted model runs behind an authenticated inference endpoint on hardware suitable for that model. Custom Python logic can use FastAPI there; an existing model server can be called directly. Running model weights and calling inference from Workers are separate deployment decisions. Managed inference is also a separate option and does not mean the platform has self-deployed its own weights.

Keep the marketplace's identity, task, offer and billing modules together. A separately deployed model server does not require splitting those domain modules into microservices. It also does not require adding a second financial database. Any agent-specific state store would need an explicit lifecycle and access design; a framework's persistence example does not replace the platform's financial journal or durable outbox.

For this platform, clients pay no platform fee. A refund assistant would normally help a company with its €1 publication fee, while clients can report a task/service complaint. It must distinguish platform fees from off-platform construction payments. Proposed initial tools are authorized status lookup, complaint intake and refund-request preparation. Existing staff approval remains necessary for incident refunds, and verified failed-publication/duplicate-capture refunds keep their existing automatic workflow. A model proposal goes through the same server-side permission, eligibility, principal-limit and durable idempotency checks as other callers; it does not directly call Mollie or redefine refund policy.

A first knowledge-base corpus should contain approved platform instructions and policies, with source links, document revisions and publish/review controls. Public FAQ/Wiki content must be distinct from private company/task/case material. Indexing, conversation history, derived embeddings, caches and model logs need a defined lifecycle before private data is included; they must not become a blanket archive of expired tasks. LLM-written Wiki drafts need an explicit publication workflow. Customer-to-platform support chat is a separate capability from the deferred client-to-company marketplace chat.

## Validation before adding scope

Start with a small read-only support experiment over approved Dutch/English policies, with complaint handoff and existing refund-status lookup. Compare answer correctness, source support, intent routing, escalation and cost per resolved interaction using representative cases. Include stale policy, unknown answer, unauthorized case lookup and misleading instructions embedded in messages/documents. No financial write access is required to evaluate these questions.

Measure time to first token, full response time, retrieval time, inference queue time, model throughput and errors under concurrent conversations. Model size, hardware, input/output lengths, batching and network placement can outweigh the application language's request overhead; this is an expectation to measure, not a project benchmark. Existing safe-read/publication targets remain separate from proposed AI response targets. Protect ordinary platform capacity with bounded model concurrency and request/job limits.

Only after that experiment should the founder select AI scope, model/hosting strategy, document ownership, automation authority and an operating budget. Re-estimate separately; the original engineering estimate does not include these features or model-serving costs.
