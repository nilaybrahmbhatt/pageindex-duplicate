# System Architecture

## High-Level Overview

```mermaid
flowchart TD
    Clients["Clients<br/>(Web/Mobile/App)"] -->|HTTPS + Auth| Gateway["API Gateway"]
    Gateway --> Ingestion["Ingestion Service<br/>(FastAPI + Celery)"]
    Gateway --> Storage["Storage Layer"]
    Gateway --> Query["Query Service<br/>(LangGraph Agent)"]
    Gateway --> LLM["LLM Router + Cost Router"]
    Gateway --> Obs["Observability & Security"]

    subgraph Ingestion
        Parser["Parser Workers<br/>(Docling + Marker + Vision)"]
        TreeBuilder["Tree Builder + Metadata Scorer"]
        GraphBuilder["Memory Graph Builder<br/>(NetworkX / Neo4j)"]
        LeafEmbed["Optional Leaf Embedder<br/>(only at leaves)"]
    end
    Ingestion --> Parser & TreeBuilder & GraphBuilder & LeafEmbed

    subgraph Storage
        S3["Object Store (S3/MinIO)"]
        DB["PostgreSQL 16 + JSONB"]
        GraphDB["Memory Graph<br/>(NetworkX or Neo4j)"]
        Vector["Optional Vector Store<br/>(pgvector / FAISS) — leaves only"]
        Cache["Redis"]
    end

    subgraph Query
        Router["Cost-Optimized Router<br/>(cheap for nav, premium for synth)"]
        Agent["Tree + Graph Navigation Agent"]
        Hybrid["Hybrid Retriever<br/>(Tree + Graph + Leaf Vector)"]
        Synth["Synthesis + Citation Engine"]
    end
    
## High-Level Overview

```mermaid
flowchart TD
    Clients["Clients<br/>(Web/Mobile/App)"] 
        -->|HTTPS + Auth| Gateway["API Gateway<br/>(Kong / AWS API GW / Traefik)"]

    Gateway --> Ingestion["Ingestion Service<br/>(FastAPI + Celery/RQ)"]
    Gateway --> Storage["Storage Layer"]
    Gateway --> Query["Query Service<br/>(LangGraph Agent)"]
    Gateway --> LLM["LLM Router<br/>(LiteLLM)"]
    Gateway --> Obs["Observability & Security"]

    subgraph IngestionDetails ["Ingestion Service"]
        direction TB
        Parser["Parser Workers<br/>(Docling + Marker/Unstructured + Vision LLM)"]
        TreeBuilder["Tree Builder Workers<br/>(LLM calls → hierarchical index)"]
    end
    Ingestion --> Parser
    Ingestion --> TreeBuilder

    subgraph StorageDetails ["Storage Layer"]
        direction TB
        S3["Object Store<br/>(S3/MinIO) → PDFs, page images, extracted text"]
        DB["Metadata DB<br/>(PostgreSQL 16 + JSONB) → docs, trees, users, audit logs"]
        Cache["Cache<br/>(Redis) → frequent trees + query results"]
    end
    Storage --> S3
    Storage --> DB
    Storage --> Cache

    subgraph QueryDetails ["Query Service"]
        direction TB
        Agent["Tree Navigation Agent<br/>(iterative reasoning + LangGraph)"]
        Tools["Tools: get_node_children(), get_node_content(), evaluate_relevance()"]
        Synth["Synthesis + Citation Engine"]
    end
    Query --> Agent
    Query --> Tools
    Query --> Synth

    subgraph LLMDetails ["LLM Router"]
        Providers["Multiple providers<br/>(OpenAI, Anthropic, Groq, Grok, local vLLM/Ollama) + fallback"]
    end
    LLM --> Providers

    subgraph ObsDetails ["Observability & Security"]
        direction TB
        O1["LangSmith/LangFuse + OpenTelemetry"]
        O2["Auth (Keycloak/OAuth2 + RBAC + SSO)"]
        O3["Audit + Compliance<br/>(SOC2, GDPR, HIPAA, ISO 27001 ready)"]
        O4["Monitoring<br/>(Prometheus + Grafana + Sentry)"]
    end
    Obs --> O1
    Obs --> O2
    Obs --> O3
    Obs --> O4