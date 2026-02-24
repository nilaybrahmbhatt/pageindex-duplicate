# Tech Stack

## Philosophy
- Use **best-in-class open-source tools** (2026 standards)
- Prioritize **reliability, observability, and cost-efficiency**
- Keep it **Python-first** for rapid development and easy maintenance
- Design for **multi-cloud / on-prem / air-gapped** deployment

## Full Stack Breakdown

| Layer                  | Technology                              | Version / Reason |
|------------------------|-----------------------------------------|------------------|
| **Metadata Scoring**     | LLM + custom scorer (Pydantic models)        | Structured JSON scores per node |
| **Memory Graph**         | NetworkX (light) or Neo4j (enterprise)       | Cross-section relationships |
| **Hybrid Vector Layer**  | pgvector (in Postgres) or FAISS (leaves only)| Precision without full vector DB |
| **Cost Router**          | LiteLLM + custom routing logic in LangGraph  | Cheap navigation + premium synthesis |
| **LLM Router**           | LiteLLM (now with step-aware model selection)| Cost optimization |
| **Graph DB**             | NetworkX (default) / Neo4j / FalkorDB        | Memory graph |
| **Vector (optional)**    | pgvector 0.8+                                | Leaf-only embeddings |
| **Language**           | Python                                 | 3.12+ — mature ecosystem, async support |
| **Web Framework**      | FastAPI                                | High performance, auto OpenAPI docs, async |
| **Agent Framework**    | LangGraph (LangChain ecosystem)        | Stateful, multi-step reasoning, human-in-loop |
| **LLM Router**         | LiteLLM                                | One API for 100+ providers + fallbacks + cost tracking |
| **Document Parser**    | Docling (IBM) + Marker + pdfplumber    | Best hierarchical extraction + page numbers + tables |
| **Vision / OCR-free**  | GPT-4o / Gemini 2.0 / Claude-3.5 Vision| Direct page image → structured Markdown |
| **Database**           | PostgreSQL 16 + JSONB + pgvector (optional hybrid) | Native tree queries, full-text, audit-ready |
| **Cache / Queue**      | Redis + Celery / RQ                    | Reliable async ingestion, rate limiting |
| **Object Storage**     | MinIO / AWS S3                         | PDFs, page images, exports |
| **Frontend (Phase 1)** | Streamlit → Next.js 15 + shadcn/ui     | Fast prototyping → beautiful production UI |
| **PDF Viewer**         | PDF.js + custom highlights             | Side-by-side with citations |
| **Observability**      | LangSmith + OpenTelemetry + Prometheus + Grafana + Sentry | Full trace of every LLM call & cost |
| **Auth & Security**    | Keycloak (OAuth2/OIDC + RBAC + SSO)    | Enterprise SSO, roles, audit logs |
| **Deployment**         | Docker + Kubernetes + Helm             | Scalable, one-click on any cloud |
| **Infrastructure as Code** | Terraform                           | Reproducible environments |
| **CI/CD**              | GitHub Actions                         | Tests, lint, Docker build, deploy |
| **Testing**            | pytest + LangSmith evaluators          | Unit, integration, E2E, benchmark |

## LLM Strategy (Cost vs Quality)
- **Tree Building** → Claude-3.5-Haiku or Llama-3.3-70B (cheap & fast)
- **Tree Navigation** → Groq Llama-3.3-70B or Gemini-2.0-Flash
- **Final Synthesis** → GPT-4o / Claude-3.5-Sonnet / Grok-4
- **Vision** → GPT-4o or Gemini-2.0-Pro

## Optional / Future
- Hybrid mode with pgvector (for fuzzy search fallback)
- MCP (Model Context Protocol) support
- On-prem local models via vLLM + Ollama
- Air-gapped version (no internet)