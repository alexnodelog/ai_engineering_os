# global_technology_stack_v10.md

Version: 10.0

Status: Source of Truth

---

# 1. Purpose

This document defines the official technology stack for the AI-Native Personal Development Framework.

Project-specific rules inherit from this document.

Technology choices are based on:

* Maintainability
* Simplicity
* AI Readability
* Vendor Independence
* Long-term Sustainability
* Free & Commercial-friendly Licensing

---

# 2. Technology Selection Principles

Priority:

1. Maintainability
2. Simplicity
3. Consistency
4. Developer Experience
5. AI Readability
6. Extensibility
7. Performance
8. Trend

Technology trends MUST NOT outweigh maintainability.

---

# 3. Core Technology Stack

| Category        | Primary             | Alternative                    | Notes                      |
| --------------- | ------------------- | ------------------------------ | -------------------------- |
| Frontend        | Next.js             | React                          | TypeScript required        |
| Language        | TypeScript          | JavaScript                     | TypeScript preferred       |
| Styling         | Tailwind CSS        | CSS Modules                    | Tailwind first             |
| UI              | shadcn/ui           | MUI                            | Prefer shadcn/ui           |
| State           | Zustand             | Redux Toolkit                  | Keep state simple          |
| Backend         | FastAPI             | None                           | Standard backend framework |
| Python          | Python 3.12+        | Latest LTS                     |                            |
| Validation      | Pydantic            | None                           |                            |
| ORM             | SQLAlchemy          | SQLModel                       | SQLAlchemy first           |
| Database        | SQLite              | PostgreSQL                     | SQLite by default          |
| Migration       | Alembic             | None                           | Optional                   |
| Vector DB       | FAISS               | sqlite-vec / Qdrant / pgvector | FAISS default              |
| Desktop         | Electron            | Tauri                          | Electron first             |
| Mobile          | React Native + Expo | Flutter (comparison only)      | Expo first                 |
| Node Package    | pnpm                | npm                            | pnpm required              |
| Python Package  | Poetry              | uv                             | Poetry primary             |
| Version Control | Git                 | None                           | GitHub recommended         |
| Documentation   | Markdown            | MkDocs                         | Markdown required          |
| API Spec        | OpenAPI             | None                           | Auto-generated             |
| Dev Environment | Docker Compose      | Native                         | Docker first               |
| IDE Environment | Dev Container       | Native                         | Recommended                |
| CI              | GitHub Actions      | Gitea Actions                  | GitHub first               |

---

# 4. Database Policy

## Primary Database

SQLite

Reason:

* Zero configuration
* Cross-platform
* Easy backup
* AI-friendly
* Perfect for personal development
* Suitable for MVP and PoC

---

## Optional Database

PostgreSQL

Use PostgreSQL only when:

* Multi-user production
* High concurrency
* PostgreSQL-specific features
* External deployment requires it

Projects MUST NOT depend directly on SQLite-specific features.

Repository Pattern and SQLAlchemy MUST isolate database implementation.

---

# 5. Vector Store Policy

Primary:

FAISS

Reasons:

* Local-first
* Fast
* No server required

Optional:

* sqlite-vec
* pgvector
* Qdrant

Applications MUST depend on Vector Store interfaces rather than implementations.

---

# 6. Python Package Management

Primary

Poetry

Reasons:

* Mature ecosystem
* Better documentation
* Better AI familiarity
* Stable lock file
* Excellent dependency management

Alternative

uv

May be used when:

* Performance is important
* Existing project already uses uv

---

# 7. Development Environment

Primary

Docker Compose

Standard command:

```bash
docker compose up
```

Native execution is optional.

---

# 8. Dev Container

Projects SHOULD include:

```
.devcontainer/

devcontainer.json
```

Purpose:

* Identical development environments
* Reproducible tooling
* Same VS Code extensions
* Same SDK versions

---

# 9. Hosting

Frontend

* Vercel

Backend

* Oracle Cloud Always Free
* Self-host
* Existing Linux Server

Applications MUST NOT depend on hosting provider APIs.

---

# 10. Architecture Requirements

Business Logic MUST NOT depend on:

* SQLite
* PostgreSQL
* FAISS
* Qdrant
* OpenAI
* Ollama
* Gemini
* Cloud Provider

Use abstraction layers for:

* Repository
* LLM Provider
* Embedding Provider
* Vector Store
* Storage

---

# 11. Free Software Policy

Preferred technologies MUST:

* Be free
* Allow commercial usage
* Be usable on company-owned PCs
* Have active maintenance
* Avoid vendor lock-in

Always-Free services are preferred over time-limited free tiers.

---

# 12. Future Evolution

Technology choices may evolve.

Architecture principles should remain stable.

Future revisions MUST prioritize long-term maintainability over adopting new trends.
 
