<div align="center">

# 🗣️ CONVERSATION INTELLIGENCE

### AI Analytical Platform

*A RAG-powered analytical engine for extracting insights, tracking schemas, and querying conversation transcripts with AI.*

[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react)](https://react.dev/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)](https://www.docker.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-4169E1?style=for-the-badge&logo=postgresql)](https://postgresql.org/)
[![Gemini](https://img.shields.io/badge/Google_Gemini-AI-4285F4?style=for-the-badge&logo=google)](https://ai.google.dev/)

</div>

---

## 📌 Problem Statement

Analyzing thousands of conversation transcripts manually to uncover customer intents, track resolution schemas, and extract nuanced analytical insights is extremely inefficient. **This platform** solves this by leveraging a LangGraph-powered RAG agent running over a vector database of chunked transcripts. Users can upload transcripts, query over conversational histories using natural language, and seamlessly retrieve structured analytical intelligence directly backed by context citations.

---

## ✨ Key Features

| Feature | Description |
|---|---|
| **🧠 LangGraph RAG Agent** | Complex multi-stage reasoning to analyze queries, retrieve relevant transcript chunks, and generate accurate final answers. |
| **🔍 Multi-LLM Orchestration** | Agnostic support leveraging Google Gemini, OpenAI, and HuggingFace models for scalable intelligence. |
| **📅 Vector Search & Retrieval** | Transcripts are chunked, embedded via Sentence Transformers, and indexed into a PostgreSQL database with `pgvector` (`VECTOR(768)`). |
| **👤 Schema Tracking Engine** | Built-in domain schema agent to dynamically map transcripts to domains, intents, and issue categories automatically. |
| **💬 Threaded Memories** | Agent state retains long-term memory and contextual feedback across sessions, uniquely identifying past queries via `thread_id`. |
| **⚡ Real-time Interface** | A responsive React 19 SPA built on Vite, drawing inspiration from leading chat engines, served efficiently via Nginx. |
| **📦 Containerized Simplicity** | Out-of-the-box infrastructure featuring Docker Compose with auto-scaffolded database initializations. |

---

## 🏗️ Architecture

```text
┌──────────────────────────────────────────────────────────────────┐
│                        React 19 + Vite                          │
│               React Markdown │ @google/genai API                │
└────────────────────────────┬─────────────────────────────────────┘
                             │  REST API (JSON)
┌────────────────────────────▼─────────────────────────────────────┐
│                  FastAPI Backend Server                          │
│                                                                  │
│  ┌──────────────┐ ┌──────────────────┐ ┌──────────────────────┐  │
│  │ Chat/Agent   │ │ Transcript Loader│ │ Schema Analyzer      │  │
│  │ Endpoint     │ │ & Batch API      │ │ Service              │  │
│  └──────────────┘ └──────────────────┘ └──────────────────────┘  │
│  ┌──────────────────┐ ┌────────────────┐ ┌────────────────────┐  │
│  │ LangGraph RAG    │ │ External LLMs  │ │ Memory Saver       │  │
│  │ Orchestrator     │ │ (Gemini/OpenAI)│ │ (Checkpointer)     │  │
│  └──────────────────┘ └────────────────┘ └────────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
                             │
                      ┌──────▼───────┐
                      │  PostgreSQL  │
                      │  (pgvector)  │
                      └──────────────┘
```

### Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 19, Vite, TypeScript |
| **Backend** | Python 3.11+, FastAPI, Uvicorn |
| **AI / Graph Engine** | LangGraph, LangChain |
| **Vector DB** | PostgreSQL (`pgvector` extension) |
| **Embeddings** | Sentence Transformers (768d vectors) |
| **LLMs** | Google Gemini (`langchain-google-genai`), OpenAI |
| **Deployment** | Docker, Docker Compose, Nginx |

---

## 📁 Project Structure

```text
project-root/
├── backend/                        # Python FastAPI Backend
│   ├── agents/                     # LangGraph specialized agents (e.g. Schema Agent)
│   ├── llm/                        # LLM model configuration and integrations
│   ├── memory/                     # Agent state memories and long-term context
│   ├── nodes/                      # Logic nodes used in LangGraph compilation
│   ├── utils/                      # DB pools and external connection helpers
│   ├── data/                       # Stores the raw transcripts JSON inputs
│   ├── main.py                     # FastAPI entry point & API definition Router
│   ├── graph.py                    # LangGraph workflow definition & runtime compilation
│   ├── state.py                    # Global application state models definitions
│   ├── Dockerfile                  # Backend API Docker image configuration
│   └── pyproject.toml              # Python dependencies & package management
├── frontend/                       # React + Vite SPA
│   ├── src/
│   │   ├── components/            # Reusable UI React components
│   │   ├── services/              # Backend abstraction and API caller services
│   │   ├── App.tsx                # Main Application Shell and View Layer
│   │   └── types.ts               # Global TypeScript interface definitions
│   ├── Dockerfile                  # Multi-stage production Nginx build
│   └── package.json                # Node modules and build scripts
├── init.sql                        # PostgreSQL Initialization & Vector Field creation
└── docker-compose.yml              # Multi-container orchestration config
```

---

## 🚀 Quick Start

### Prerequisites

- Docker and Docker Compose installed on your host system.
- Valid API Keys for Google Gemini or OpenAI.

### 1. Clone the Source

```bash
git clone https://github.com/your-org/conversation_intelligence.git
cd conversation_intelligence
```

### 2. Configure Environment

The backend consumes standard LLM API credential environment variables. You must provide them contextually either by exporting in your shell or maintaining them within an env file.

```bash
export GOOGLE_API_KEY="your-google-api-key"
export OPENAI_API_KEY="your-openai-api-key"
```

### 3. Spin Up Services

Deploy the Postgres Database, Fast API server, and React UI automatically:

```bash
docker-compose up --build -d
```

Your services are now immediately mapped and available directly via:
- **Frontend Dashboard:** [http://localhost:8080](http://localhost:8080)
- **Backend API Docs:** [http://localhost:8010/docs](http://localhost:8010/docs)
- **Database Port:** `localhost:5434` (mapped inwards to 5432)

---

## 🔗 API Endpoints

| Prefix | Method | Description |
|---|---|---|
| `/chat` | `POST` | Core RAG conversational query endpoint. Evaluates `query` and maintains dialogue consistency leveraging the `thread_id` memory tracker. |
| `/transcript` | `POST` | Retrieve complete single transcript payload based on `transcript_id`. |
| `/transcripts/batch`| `POST` | Retrieve an arbitrary subset of transcript objects matching multiple IDs. |
| `/schema/stats` | `GET` | Fetches aggregate system-wide domain metadata, intent groupings, and tag vocabularies. |
| `/health` | `GET` | Rapid connectivity sanity-check probing backend uptime structure. |

---

## 🗄️ Database Initialization Framework

On initial bare-metal provisioning, Docker leverages `init.sql` executing automatically within the Postgres `nlp_postgres` container context enabling semantic features immediately:
1. `CREATE EXTENSION IF NOT EXISTS vector;`
2. Schemas instantiated inherently: `transcripts`, `transcript_chunks`, and `transcript_embeddings`.

*(Note: Data ingestion populating these tables and updating embeddings traditionally invokes custom ingest scripts feeding out of the `./data` root payloads.)*

---

## 📜 License

See `LICENSE` file for usage stipulations and distribution restrictions.
