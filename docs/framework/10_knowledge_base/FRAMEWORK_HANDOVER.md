
# FRAMEWORK_HANDOVER.md

Version: 1.1

Status: Living Document

Last Updated: 2026-08-03

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

These are the Layer 3 (`global_technology_stack_v10.md`) defaults, now fully operationalized across every currently `Active` Layer 4 archetype document (`project-pc-app_v04.md`, `project-personal-full-stack_v01.md`, `project-monolithic_v04.md`) and about to be applied to a fourth (`project-mobile_v01.md`, in progress — see Section 9).

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

Operationalized in `project-pc-app_v04.md` (Active).

---

## Mobile

Primary

React Native

Expo

TypeScript

Not yet operationalized into a Layer 4 Project Rule document. `project-mobile_v01.md` is the current in-progress target (Section 9) that will apply this default the same way `project-pc-app_v04.md` already applies Electron.

---

## Frontend

Next.js

TypeScript

Tailwind CSS

shadcn/ui

Zustand

Operationalized in `project-personal-full-stack_v01.md` and `project-monolithic_v04.md` (both Active); embedded directly in `template-fastapi-sqlite/`'s `frontend/package.json`.

---

## Backend

FastAPI

Python

Pydantic

SQLAlchemy

Operationalized in `project-personal-full-stack_v01.md` and `project-monolithic_v04.md` (both Active; the Monolithic archetype additionally layers SQLModel over SQLAlchemy as its ORM, per that document's own Section 8).

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

The six-document hierarchy this section originally sketched has since been superseded, in substance, by the full eleven-layer Knowledge Architecture (KA-001 through KA-004, `DECISIONS.md` entry `DEC-002`) defined in `FRAMEWORK_BLUEPRINT.md`, Section 2. This document does not reproduce that architecture — doing so would duplicate content that already has a single authoritative source, contrary to the framework's own inheritance model (`FRAMEWORK_BLUEPRINT.md`, Section 5).

For the current, authoritative document hierarchy, consult, in order:

1. `FRAMEWORK_README.md` — the mandatory first read of any session; current migration status, authoritative documents by domain, deprecated documents.
2. `FRAMEWORK_BLUEPRINT.md`, Section 2 — the full eleven-layer specification this hierarchy is built on.
3. `FRAMEWORK_STATUS.md` — implementation progress, per-document completion detail, and the Changelog / Open Inconsistency Flags this document's own summaries (Sections 8–9, below) intentionally do not duplicate.

What remains true, and is restated here because it is this document's own load-bearing precedence rule (Section 10, below): a lower-numbered layer always outranks a higher-numbered one, and `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` (Layer 1 — Constitution) sits above every other document in the repository, including this one.

---

# 7. Working Rules for AI

When improving the framework:

* Preserve philosophy.
* Improve architecture.
* Reduce duplication.
* Prefer inheritance over repetition.
* Explain architectural trade-offs before major redesigns.
* Treat uploaded rule files as references, not immutable specifications.
* Read `FRAMEWORK_README.md` first, every session, before any other framework document (CC-002).
* Read `DECISIONS.md` before proposing or making any architectural decision, to avoid re-litigating a settled question (PR-002).
* Never select a project archetype on the human's behalf; archetype selection is a Level 1 (Engineering CEO) decision (`PROJECT_BOOTSTRAP_GUIDE.md`, Section 4.1).
* Update this document, alongside `DECISIONS.md`, in the same change as any commit that introduces or changes an architectural decision (PR-001) — this document's own currency is what makes it useful as session-continuity context; a stale handover document is exactly the kind of drift `FRAMEWORK_STATUS.md`'s Open Inconsistency Flags exist to catch.

---

# 8. Documents Completed

**Framework v10, Tier 1 and Tier 2 combined, is now fully complete.** Every governing layer (1–6) is `Active`, and every v10-scoped operational layer (7 — AI Skills, 8 — Prompt Library, 9 — Project Templates, 10 — Knowledge Base) is `Active` as a whole. This is a material change from earlier revisions of this document, which tracked the framework as still assembling its foundational rule and technology-stack layers; that assembly is now finished.

**Most recently completed:** `template-fastapi-sqlite/` (Layer 9) — the concrete seed template conforming to `project-personal-full-stack_v01.md` (Full-Stack), per the archetype-selection decision recorded as `DECISIONS.md` entry `DEC-010`. Its completion was the framework's final outstanding Tier 2 item, and had two further, purely mechanical consequences (neither a new decision in its own right, per `DECISIONS.md`, Section 3.2):

* Layer 9 (Project Templates) reached `Active` as a whole, satisfying the KA-004 minimum-artifact requirement in full.
* `V10_MIGRATION_NOTES.md` (Layer 11) transitioned from `Active` to `Deprecated`, per its own pre-scheduled DL-003 condition — every Tier 1 and Tier 2 v10 document now exists. It is no longer a valid conflict-resolution fallback for any question; the ordinary Authority Model (`FRAMEWORK_BLUEPRINT.md`, Section 6) now applies directly and without exception.

For the complete, document-by-document completion record — including every Skill, Prompt, Project Rule, and Developer Manual generated across v10 — consult `FRAMEWORK_STATUS.md`, "Completed Milestones." This document intentionally does not reproduce that full list, to avoid the two documents drifting out of sync with each other over time.

---

# 9. Next Recommended Tasks

The six tasks this section previously listed (finalizing the Constitution, Global Rules, Project Rules, Development Manuals, Templates, and Prompt Packs) are all complete — see Section 8, above, and `FRAMEWORK_STATUS.md` for full detail. They are superseded by the following, current queue:

1. **`project-mobile_v01.md`** (Layer 4, Mobile Application archetype) — **in progress.** This is a Tier 3 / v10.1 item, explicitly pulled forward ahead of the remainder of v10.1 scope by the Owner's Gate 1 (Plan Approval) decision, recorded as `DECISIONS.md` entry `DEC-011`. Its content is substantially pre-determined by the confirmed Layer 3 Mobile default (React Native + Expo, Section 4 above) and by Layer 4's own designated directory-layout/naming-convention responsibility, applied the same way it already has been for the Desktop, Full-Stack, and Monolithic archetypes.
2. `AI_DEVELOPMENT_MANUAL.md` (Layer 5) — v10.1, still deferred. Not part of the Owner's pull-forward approval; requires its own separate Gate 1 event before generation may begin.
3. Full Project Template set beyond `template-fastapi-sqlite/` (Layer 9) — v10.1, still deferred, same condition as above.
4. Full five-tool Prompt Library beyond Claude Code and OpenAI (Layer 8) — v10.1, still deferred, same condition as above.

Per `FRAMEWORK_BLUEPRINT.md`, Section 18.4, no item in this list beyond `project-mobile_v01.md` may be started without its own explicit Gate 1 decision recorded in `DECISIONS.md` — the `DEC-011` approval is scoped narrowly to the Mobile archetype and MUST NOT be read as authorizing the rest of v10.1.

---

# 10. Important Notes

If previously uploaded rule files conflict with this document:

This document takes precedence.

If this document conflicts with `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`:

The philosophy document takes precedence.

These two sentences are this document's own informal restatement of a rule that is now fully and formally specified elsewhere: the downward-authority rule (KA-002, `DECISIONS.md` entry `DEC-002`) defined in full in `FRAMEWORK_BLUEPRINT.md`, Section 6. Where any apparent tension exists between the informal statement here and that formal Authority Model, the formal model governs — this section is a convenience summary, not an independent source of precedence.

Future framework evolution MUST preserve philosophy while allowing technology replacement.

---

# 11. Session Continuity

This document is updated after every significant architectural decision, alongside `DECISIONS.md`, in the same change, per the mandatory co-update rule (PR-001, `DECISIONS.md` entry `DEC-004`).

It functions as the persistent memory of the framework across AI sessions — specifically, as the fast, narrative-form answer to "what has already happened and what comes next," complementing `DECISIONS.md`'s formal, dated, append-only record of *why* each decision was made. A session picking up framework-evolution work SHOULD read both: this document for orientation, `DECISIONS.md` for the specific rationale behind any decision this document summarizes.

Always review this document before modifying any rule, technology stack, or development manual.
