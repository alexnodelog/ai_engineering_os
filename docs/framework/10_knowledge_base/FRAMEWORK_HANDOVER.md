
# FRAMEWORK_HANDOVER.md

Version: 1.0

Status: Living Document

Purpose: AI Session Handover & Framework Context

---

# 1. Project Identity

Project Name

AI-Native Personal Software Engineering Framework

Purpose

Create a long-term engineering framework that enables AI-assisted software development across Full Stack, Desktop, Mobile, Monolithic, and RAG applications.

This framework serves as the Engineering Operating System for future projects.

---

# 2. Current Development Goals

Primary Goals

* AI-First Development
* Agentic Engineering
* AI Orchestration
* Long-term Maintainability
* Vendor Independence
* Personal Development Framework
* Reproducible Development Environment
* Free & Commercial-friendly Tooling

---

# 3. Framework Philosophy

The framework prioritizes:

1. Maintainability
2. Simplicity
3. Consistency
4. Developer Experience
5. AI Readability
6. Vendor Independence
7. Long-term Sustainability

Technology trends MUST NOT override these principles.

---

# 4. Confirmed Technical Decisions

## Development Environment

Primary

Docker Compose

Recommended

Dev Container

Native execution is optional.

---

## Python Package Management

Primary

Poetry

Alternative

uv

Reason

* Better ecosystem maturity
* Better documentation
* Better AI familiarity
* Stable dependency management

---

## Database

Primary

SQLite

Optional

PostgreSQL

Reason

SQLite satisfies the requirements of:

* Personal development
* Local AI
* RAG
* MVP
* PoC

Applications MUST remain database-independent through SQLAlchemy and Repository Pattern.

---

## ORM

Primary

SQLAlchemy

---

## Vector Store

Primary

FAISS

Optional

* sqlite-vec
* pgvector
* Qdrant

Applications MUST depend on interfaces rather than concrete implementations.

---

## Desktop

Primary

Electron

Alternative

Tauri

---

## Mobile

Primary

React Native

Expo

TypeScript

---

## Frontend

Next.js

TypeScript

Tailwind CSS

shadcn/ui

Zustand

---

## Backend

FastAPI

Python

Pydantic

SQLAlchemy

---

# 5. Architecture Principles

Business Logic MUST NOT depend directly on:

* Database Engine
* Vector Store
* LLM Provider
* Embedding Provider
* Cloud Provider

Infrastructure MUST remain replaceable.

---

# 6. Framework Documents

Current planned hierarchy

AI_DEVELOPMENT_PHILOSOPHY.md

↓

FRAMEWORK_HANDOVER.md

↓

global_rules_revisionfinal_v10.md

↓

global_technology_stack_v10.md

↓

project-specific rules

↓

development manuals

---

# 7. Working Rules for AI

When improving the framework:

* Preserve philosophy.
* Improve architecture.
* Reduce duplication.
* Prefer inheritance over repetition.
* Explain architectural trade-offs before major redesigns.
* Treat uploaded rule files as references, not immutable specifications.

---

# 8. Documents Completed

Current

* AI Development Philosophy (draft)
* Technology Stack (v10)
* Framework Handover

In Progress

* Global Rules v10
* Project Rules v10
* Development Manuals

---

# 9. Next Recommended Tasks

1. Finalize AI_DEVELOPMENT_PHILOSOPHY_v2.0.md
2. Finalize global_rules_revisionfinal_v10.md
3. Update all project rule documents
4. Create AI development manuals
5. Create project templates
6. Create AI prompt packs

---

# 10. Important Notes

If previously uploaded rule files conflict with this document:

This document takes precedence.

If this document conflicts with AI_DEVELOPMENT_PHILOSOPHY.md:

The philosophy document takes precedence.

Future framework evolution MUST preserve philosophy while allowing technology replacement.

---

# 11. Session Continuity

This document is intended to be updated after significant architectural decisions.

It functions as the persistent memory of the framework across AI sessions.

Always review this document before modifying any rule, technology stack, or development manual.
 
