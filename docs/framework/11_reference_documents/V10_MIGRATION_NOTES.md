# V10_MIGRATION_NOTES.md

Version: 10.0

Status: Migration Guide

Purpose: Defines the official migration path from Framework v09 to v10.

This document supersedes conflicting decisions found in previous rule documents.

---

# 1. Purpose

Framework v10 is not a simple revision of v09.

It represents a transition from a rule collection to an AI-Native Engineering Framework.

This document records the architectural decisions that guide this migration.

When conflicts exist between previous rule files and this document, **this document takes precedence** until the corresponding v10 documents are finalized.

---

# 2. Migration Goals

The objectives of v10 are:

* Establish AI-First engineering standards.
* Create a unified Source of Truth.
* Improve long-term maintainability.
* Reduce duplicated rules.
* Increase vendor independence.
* Standardize development environments.
* Support agentic AI workflows.

---

# 3. Philosophy Changes

## v09

* Rule-oriented
* Technology-oriented
* Project-specific decisions scattered across documents

## v10

* Philosophy-oriented
* Architecture-oriented
* Technology-independent
* AI-First
* Engineering Operating System

---

# 4. Confirmed Technology Decisions

## Python Package Management

### Previous

Primary: uv

### v10

Primary: Poetry

Alternative: uv

Reason:

* Mature ecosystem
* Better documentation
* Better AI familiarity
* Stable dependency management

---

## Database

### Previous

PostgreSQL-centric

### v10

Primary

SQLite

Optional

PostgreSQL

Reason:

* Zero configuration
* Personal development
* MVP
* PoC
* Local AI
* Cross-platform

Business logic MUST remain database-independent.

---

## ORM

Primary

SQLAlchemy

Reason:

Database independence.

---

## Migration Tool

Alembic

Status

Optional

Reason:

Migration tooling should not be mandatory for every project.

---

## Vector Store

Primary

FAISS

Optional

* sqlite-vec
* pgvector
* Qdrant

Applications MUST depend on Vector Store interfaces.

---

## Development Environment

Primary

Docker Compose

Recommended

Dev Container

Native execution remains optional.

---

## Desktop Framework

Primary

Electron

Alternative

Tauri

Tauri remains a comparison technology and is not the default.

---

## Hosting

Frontend

Vercel

Backend

* Oracle Cloud Always Free
* Self-host

Cloud providers MUST remain replaceable.

---

# 5. Architecture Changes

v09 contained project-specific architectural rules.

v10 introduces a layered architecture:

AI_DEVELOPMENT_PHILOSOPHY

↓

FRAMEWORK_HANDOVER

↓

Global Rules

↓

Technology Stack

↓

Project Rules

↓

Development Manuals

↓

Current Project

---

# 6. Documentation Changes

The following documents become first-class framework components:

* AI_DEVELOPMENT_PHILOSOPHY.md
* FRAMEWORK_HANDOVER.md
* global_rules_revisionfinal_v10.md
* global_technology_stack_v10.md

Documentation becomes part of the framework architecture.

---

# 7. AI Development Changes

The framework now explicitly supports:

* ChatGPT
* Claude
* Claude Code
* Cursor
* GitHub Copilot
* Codex
* Future AI systems

Rules should maximize AI readability and minimize ambiguity.

---

# 8. Engineering Workflow Changes

Recommended workflow:

Clone Repository

↓

Open in Dev Container (recommended)

↓

docker compose up

↓

Develop

↓

Test

↓

Commit

↓

Deploy

Native execution remains optional.

---

# 9. Vendor Independence

Applications MUST avoid direct dependency on:

* SQLite
* PostgreSQL
* FAISS
* pgvector
* OpenAI
* Ollama
* Gemini
* Cloud provider APIs

Repository and Provider abstractions MUST isolate infrastructure.

---

# 10. Rule Consolidation

Duplicated rules found across previous project documents should be migrated into:

global_rules_revisionfinal_v10.md

Project-specific documents should inherit rather than redefine common rules.

---

# 11. Migration Checklist

The following documents should be updated to align with v10:

* global_rules_revisionfinal_v10.md
* global_technology_stack_v10.md
* project-personal-full-stack_v01.md
* project-mobile_v01.md
* project-pc-app_v04.md
* project-monolithic_v04.md
* AI_DEVELOPMENT_MANUAL.md
* PROJECT_BOOTSTRAP_GUIDE.md
* COMMANDS.md
* PROJECT_STRUCTURE.md

---

# 12. Migration Priority

Priority 1

* AI_DEVELOPMENT_PHILOSOPHY.md
* FRAMEWORK_HANDOVER.md

Priority 2

* global_rules_revisionfinal_v10.md
* global_technology_stack_v10.md

Priority 3

* Project rule documents

Priority 4

* Manuals
* Templates
* Prompt Packs

---

# 13. Compatibility Policy

Framework v10 aims to preserve architectural compatibility where practical.

Technology choices may change.

Philosophy and architecture should remain stable.

---

# 14. Final Rule

When any previous rule document conflicts with the decisions recorded in this document, **v10 Migration Notes take precedence** until the new Source of Truth documents are completed.

This document serves as the official bridge between Framework v09 and Framework v10.
 
