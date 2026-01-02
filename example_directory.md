# Project Structure Analysis: Medical Q&A Agent System

## Project Overview

This is a **Medical Q&A Agent System** built as a sophisticated AI backend with a Streamlit frontend. It implements a Retrieval-Augmented Generation (RAG) pipeline using LangGraph for agentic workflows and integrates with the Upstage Solar LLM ecosystem.

---

## Directory Organization

```
/Users/sunwoong/dev/agent_test/
├── app/                          # Backend application core
│   ├── agents/                   # LangGraph workflow orchestration
│   │   ├── subgraphs/           # Individual agent subgraphs
│   │   │   ├── answer_gen.py    # Answer generation agent
│   │   │   ├── evaluator.py     # Response evaluation agent
│   │   │   ├── info_extractor.py # Medical info extraction agent
│   │   │   └── knowledge_augmentor.py # Knowledge enhancement agent
│   │   ├── state.py             # Agent state definitions (TypedDict)
│   │   ├── tools.py             # Agent tools and utilities
│   │   └── workflow.py          # Main workflow graph orchestration
│   │
│   ├── api/                      # API layer
│   │   └── route/
│   │       └── agent_routers.py  # FastAPI endpoints
│   │
│   ├── core/                     # Core infrastructure
│   │   ├── db.py                # ChromaDB connection (singleton pattern)
│   │   ├── llm.py               # LLM client initialization
│   │   ├── logger.py            # Logging configuration
│   │   └── seed.py              # Database seeding
│   │
│   ├── models/                   # Data models
│   │   ├── schemas/             # API request/response models (Pydantic)
│   │   │   └── agent.py         # Chat, Knowledge, Stats schemas
│   │   └── entities/            # Internal entities
│   │       ├── medical_qa.py    # Medical QA entity
│   │       └── session.py       # Session entity
│   │
│   ├── repository/               # Data access layer
│   │   ├── client/              # External clients
│   │   │   ├── base.py          # Abstract base class
│   │   │   ├── llm_client.py    # Upstage LLM client
│   │   │   └── search_client.py # Search/web client
│   │   └── vector/              # Vector database layer
│   │       └── vector_repo.py   # ChromaDB repository
│   │
│   ├── service/                  # Business logic layer
│   │   ├── agent_service.py      # Main orchestration service
│   │   ├── embedding_service.py  # Embedding generation
│   │   ├── vector_service.py     # Vector DB operations
│   │   └── agents/              # Agent-specific services
│   │       ├── answer_gen_service.py
│   │       ├── evaluator_service.py
│   │       ├── info_extractor_service.py
│   │       └── knowledge_augmentor_service.py
│   │
│   ├── deps.py                   # Dependency injection setup
│   ├── exceptions.py             # Custom exceptions
│   └── __init__.py
│
├── frontend/                     # Frontend application
│   └── ui.py                    # Streamlit UI
│
├── docs/                         # Documentation
│   └── PROJECT_ROADMAP.md       # 4-week curriculum roadmap
│
├── main.py                       # FastAPI application entry point
├── pyproject.toml               # Project dependencies (uv-based)
├── uv.lock                      # Lock file (uv package manager)
├── .env.example                 # Environment configuration template
├── start.sh                     # Server startup script
└── README.md                    # Project documentation
```

---

## Key Files and Their Purposes

### 1. Entry Point

- **main.py** (53 lines): FastAPI app initialization with lifespan management, exception handlers, and router registration

### 2. Agents (LangGraph Workflows)

- **workflow.py** (169 lines): Master workflow orchestrating 4-step medical Q&A pipeline
  - Step 1: Medical Information Extraction (RAG search in vector DB)
  - Step 2: Knowledge Augmentation (Google Search integration)
  - Step 3: Answer Generation (Medical Consultant)
  - Step 4: Response Evaluation (Quality scoring)
  - Includes loop control (max 2 iterations) and domain validation

- **state.py** (26 lines): TypedDict definitions for workflow state management
  - MainState: user_query, build_logs, augment_logs, extract_logs, answer_logs, eval_logs, loop_count

### 3. API Routes

- **agent_routers.py** (160 lines): RESTful endpoints
  - `POST /agent/chat`: Execute full workflow
  - `POST /agent/chat/stream`: Streaming response with Server-Sent Events
  - `POST /agent/knowledge`: Add documents to knowledge base
  - `GET /agent/stats`: Collection statistics
  - `DELETE /agent/knowledge/{doc_id}`: Document removal
  - `POST /agent/{name}`: Run individual agents
  - `GET /agent/health`: Health check

### 4. Service Layer

- **agent_service.py** (103 lines): Orchestrates agent execution with:
  - Multiple graph support (super, extractor, augmentor, answerer, evaluator)
  - Session management via thread_id
  - Synchronous and asynchronous execution modes
  - Vector service injection

- **vector_service.py**: Vector database operations
- **embedding_service.py**: Upstage embedding generation
- **Agent-specific services** (4 files): Each agent's business logic

### 5. Data Access Layer

- **vector_repo.py** (79 lines): Abstract repository pattern
  - ChromaDBRepository implementation
  - Methods: add_documents, query, delete_documents, get_collection_info

- **llm_client.py** (26 lines): Upstage integration
  - ChatUpstage (chat model: solar-pro2)
  - UpstageEmbeddings (embedding model: solar-embedding-1-large)

### 6. Database

- **db.py** (73 lines): ChromaDB singleton connection manager
  - Supports local (PersistentClient) and server (HttpClient) modes
  - Environment-driven configuration
  - Collection auto-creation

### 7. Data Models

- **schemas/agent.py** (40 lines):
  - AddKnowledgeRequest, KnowledgeResponse, StatsResponse
  - ChatRequest, ChatResponse with message logging
  - AgentRunRequest for individual agent execution

- **entities/**: Internal domain models (medical_qa.py, session.py)

### 8. Frontend

- **ui.py** (150+ lines): Streamlit interface
  - Chat interface with streaming support
  - Session persistence (UUID-based)
  - Knowledge base stats display
  - Agent reasoning log visualization

---

## Technology Stack

### Backend Framework
- **FastAPI** (>= 0.122.0): Modern async web framework
- **Uvicorn** (>= 0.38.0): ASGI server

### AI/ML & Agent Orchestration
- **LangGraph**: LLM application state management and workflow graphs
- **LangChain Core & Community**: LLM abstraction and tools
- **Upstage Solar LLM**:
  - Chat model (solar-pro2) for text generation
  - Embedding model (solar-embedding-1-large) for semantic search
- **Transformers**: HuggingFace transformer models

### Vector Database
- **ChromaDB** (== 1.3.7): In-memory/persistent vector database for embeddings
- Supports local persistent storage and server-based modes

### Frontend
- **Streamlit**: Interactive web UI for medical Q&A
- **Pandas**: Data manipulation

### Data Validation & ORM
- **Pydantic** (>= 2.0.0): Request/response schema validation

### API Client
- **httpx** (== 0.27.1): Async HTTP client (used for streaming)
- **OpenAI client library** (== 1.52.2): For API compatibility layer

### Configuration & Environment
- **python-dotenv** (>= 1.0.0): Environment variable management

### Testing
- **pytest** (>= 8.0.0): Test framework
- **pytest-asyncio** (>= 0.23.0): Async test support
- **pytest-mock** (>= 3.12.0): Mocking utilities
- **LangSmith**: LangChain observability

### Development
- **ruff** (>= 0.14.10): Python linter and formatter
- **uv**: Fast Python package manager (replaces pip/conda)

---

## Overall Architecture

### Multi-Layered Architecture Pattern

```
┌─────────────────────────────────────────┐
│         Frontend (Streamlit)             │ <- UI Layer
│  Chat Interface, Session Management     │
└────────────┬────────────────────────────┘
             │ HTTP/WebSocket
┌────────────▼────────────────────────────┐
│         FastAPI Routes                   │ <- API Layer
│  /agent/chat, /agent/knowledge, etc.    │
└────────────┬────────────────────────────┘
             │
┌────────────▼────────────────────────────┐
│      AgentService (Orchestrator)         │ <- Service Layer
│  Routes to correct workflow/agent        │
└────────────┬────────────────────────────┘
             │
┌────────────▼────────────────────────────┐
│  LangGraph Workflow (Super Graph)        │ <- Agent Layer
│  ┌──────────────────────────────────┐   │
│  │ Step 1: Info Extraction (RAG)    │   │
│  │ Step 2: Knowledge Augmentation   │   │
│  │ Step 3: Answer Generation        │   │
│  │ Step 4: Evaluation               │   │
│  └──────────────────────────────────┘   │
└────────────┬────────────────────────────┘
             │ (loops back on insufficient)
┌────────────▼────────────────────────────┐
│  Repository Layer                        │ <- Data Access
│  ┌──────────────┐  ┌────────────────┐   │
│  │VectorService │  │UpstageClient   │   │
│  │- embedding   │  │- LLM           │   │
│  │- vector ops  │  │- embedding gen │   │
│  └──────────────┘  └────────────────┘   │
└────────────┬────────────────────────────┘
             │
┌────────────▼────────────────────────────┐
│  External Services                       │ <- External
│  ┌──────────┐  ┌──────────┐  ┌────────┐ │
│  │ ChromaDB │  │ Upstage  │  │ Google │ │
│  │ (Vector) │  │ (LLM)    │  │(Search)│ │
│  └──────────┘  └──────────┘  └────────┘ │
└──────────────────────────────────────────┘
```

### Workflow Logic

1. **User Query Received** → /agent/chat endpoint
2. **Info Extraction Agent**: Searches vector DB for relevant medical information
3. **Conditional Branch**:
   - If sufficient info found → proceed to answer generation
   - If insufficient → trigger knowledge augmentation
   - If out-of-domain → provide guidance message
   - If max iterations (2) reached → proceed with available info
4. **Knowledge Augmentation** (if needed): Google Search integration for external knowledge
5. **Answer Generation**: Medical consultant generates contextual answer
6. **Evaluation Agent**: Scores answer quality and confidence
7. **Response Streaming**: Server-Sent Events for real-time feedback

### Dependency Injection Pattern (deps.py)

```python
get_agent_service() depends on get_vector_service()
get_vector_service() depends on:
  - get_vector_repository() → ChromaDBRepository
  - get_embedding_service() → EmbeddingService
```

---

## Configuration Management

### Environment Variables (.env.example)

- **UPSTAGE_API_KEY**: Authentication for Upstage LLM
- **SERPER_API_KEY**: Search API key
- **CHROMA_MODE**: local or server
- **CHROMA_PERSIST_PATH**: Vector DB storage location
- **CHROMA_HOST/PORT**: Server connection details
- **CHROMA_COLLECTION_NAME**: Default collection name

---

## Key Features

1. **RAG Pipeline**: Semantic search in vector DB using Upstage embeddings
2. **Agent Orchestration**: Multi-step workflow with conditional branching
3. **Session Management**: Thread-based conversation continuity
4. **Streaming Responses**: Real-time agent reasoning via SSE
5. **Domain Validation**: Out-of-domain detection for medical QA
6. **Iterative Refinement**: Loop control for knowledge gaps
7. **Vector Store**: Persistent embeddings for fast retrieval
8. **Quality Evaluation**: Automated response scoring

---

## Development & Deployment

- **Package Manager**: uv (modern, fast Python package manager)
- **Startup Script**: start.sh for process management
- **Testing Framework**: pytest with async support
- **Code Quality**: Ruff formatter (88-char lines, double quotes, LF endings)
- **Main Server**: Runs on port 1234 (configurable in main.py)
- **Streamlit UI**: Connects to FastAPI backend at localhost:1234

---

## Summary

This is a **production-ready medical Q&A system** demonstrating advanced patterns including LangGraph workflows, dependency injection, layered architecture, and streaming APIs. It represents a complete 4-week curriculum project for building enterprise-grade AI services.
