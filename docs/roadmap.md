
```markdown
# Development Roadmap

## Phase 1: Core Engine (MVP) — Current
- Tree Builder with Docling + LLM
- Simple query engine with page citations
- CLI + basic FastAPI
- Vision mode (page images)
- Unit tests + evaluation harness

## Phase 2: Production Backend
- Async Celery workers
- PostgreSQL + Redis
- Full LangGraph agent (iterative tree navigation)
- Caching & cost tracking
- Evaluation suite (FinanceBench, custom datasets)

## Phase 3: Enterprise Features
- Multi-tenancy + RBAC + SSO
- Kubernetes deployment (Helm charts)
- Audit logs + compliance
- Advanced UI (Next.js + PDF viewer with highlights)
- MCP support for Claude/Cursor

## Phase 4: Polish & Scale
- Load testing
- On-prem / air-gapped version
- Public open-source core + paid enterprise
- Benchmarks & blog post