# BLUEPRINT_INPUT_FREEZE.md

**Status:** Active  
**Layer:** 5 — Developer Manuals  
**Purpose:** Immutable input for Framework v10 Blueprint Generation  
**Authority:** All decisions herein are frozen. No modifications are permitted without an Owner decision recorded in DECISIONS.md.

---

## Preamble

This document is the final, validated, and frozen input for Framework v10 blueprint generation. It supersedes `4_Blueprint_Input_Freeze.md` and incorporates the results of the Final Validation Review together with three additional editorial corrections that do not alter any of the 38 frozen architectural decisions.

**Editorial corrections applied beyond the original validation pass:**

| ID | Type | Description |
|---|---|---|
| C-08 | Editorial | Skill document structure field count corrected from six to seven |
| C-09 | Editorial | Engineering Workflow pattern formalized in Layer 5 as alternating Workflow Phase → HITL Gate sequence |
| C-10 | Editorial | Project Template Specification (`TEMPLATE_SPEC.md`) defined as a required Layer 9 meta-artifact; concrete templates inherit structural requirements from the specification |

No new architectural concepts are introduced. No new layers are added. No frozen decisions are modified.

---

## Validation Results

### Point 1 — No conflicting architectural decisions

**Result: Pass with one correction required.**

All 38 decisions are internally consistent with one exception. DI-001 Tier 2 states "Claude Code required" when specifying AI Prompt artifacts. This directly contradicts Point 8 (runtime independence) and EP-001 (vendor independence). Claude Code is an implementation runtime, not a framework component. No specific AI tool may be named as required at the framework architecture level.

**Correction C-01:** DI-001 Tier 2 entry for Layer 8 is corrected from "AI Prompts, minimum two tools (Claude Code required)" to "Prompt Library, minimum Prompt artifacts for two distinct AI tools." Tool selection is a developer and project-level decision.

---

### Point 2 — All dependencies between layers are clear

**Result: Pass with three unclassified documents requiring assignment.**

The governing layer chain (1 → 2 → 3 → 4 → 5 → 6) has explicit precedence via KA-002. The SK-002 `framework-alignment` field correctly binds operational Skills (Layer 7) to governing rules. The `gate` field in SK-002 correctly binds Skills to HE-002 gate positions.

Three documents had no layer assignment and would create ambiguity in blueprint generation.

**Correction C-02:** `CONTRIBUTING.md` is assigned to Layer 5 (Developer Manuals). It defines engineering workflow procedures, which are operational developer guidance, not framework rules.

**Correction C-03:** `FRAMEWORK_HANDOVER.md` is assigned to Layer 10 (Knowledge Base). It is session-continuity context and accumulated knowledge, not a governing rule or reference implementation.

**Correction C-04:** `V10_MIGRATION_NOTES.md` is assigned to Layer 11 (Reference Documents). It is a transition reference document — historically informative, not authoritatively governing. This also satisfies the KA-004 minimum artifact requirement for Layer 11 (see Point 4).

---

### Point 3 — Every document belongs to exactly one layer

**Result: Pass after C-02, C-03, C-04, C-10.**

Complete layer-to-document mapping after all corrections:

| Layer | Document(s) |
|---|---|
| 1 — Constitution | `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` |
| 2 — Framework Rules | `global_rules_revisionfinal_v10.md` (to be created) |
| 3 — Technology Standards | `global_technology_stack_v10.md` |
| 4 — Project Rules | `project-pc-app_v04.md`, `project-personal-full-stack_v01.md`, `project-monolithic_v04.md`, `project-mobile_v01.md` (v10.1) |
| 5 — Developer Manuals | `FRAMEWORK_README.md`, `PROJECT_BOOTSTRAP_GUIDE.md`, `COMMANDS.md`, `PROJECT_STRUCTURE.md`, `CONTRIBUTING.md`, `AI_DEVELOPMENT_MANUAL.md` (v10.1) |
| 6 — Reference Implementations | RAG chapters 1–13, `ai-agent-harness-guide.html` |
| 7 — AI Skills | `SKILLS.md` Skill Manifest + individual Skill documents |
| 8 — Prompt Library | Individual Prompt documents per AI tool |
| 9 — Project Templates | `TEMPLATE_SPEC.md` Project Template Specification + concrete template directories |
| 10 — Knowledge Base | `DECISIONS.md`, `FRAMEWORK_HANDOVER.md` |
| 11 — Reference Documents | `V10_MIGRATION_NOTES.md` (seed) |

No document appears in more than one layer. Deprecated documents hold DL-002 status and are not assigned to any active layer.

---

### Point 4 — Knowledge Architecture is complete and minimal

**Result: Fail on two layers. Contradictions resolved by C-04 and C-05.**

KA-004 requires each operational layer (Layers 7 through 11) to contain at least one valid artifact for v10 completion. Two violations existed.

**Layer 11** had no artifact. **C-04** resolves this by assigning `V10_MIGRATION_NOTES.md`.

**Layer 9** had all content deferred to v10.1, creating a direct KA-004 contradiction. A minimum seed artifact is required to satisfy KA-004 without violating the deferral.

**Correction C-05:** Layer 9 (Project Templates) is seeded in v10 with the Project Template Specification (`TEMPLATE_SPEC.md`) and a single `template-fastapi-sqlite/` directory scaffold that conforms to the specification. This satisfies KA-004 with the minimum viable artifacts. Full template population remains a v10.1 objective. This is a classification decision, not a new architectural concept.

All other operational layers satisfy KA-004 after corrections C-04 and C-05.

The eleven layers are minimal: no layer is redundant, no two layers serve the same purpose, and the governing/operational split in KA-003 is clean.

---

### Point 5 — Engineering Workflow in Developer Manuals

**Result: Pass with pattern clarification added (C-09).**

The eleven-layer Knowledge Architecture (KA-001) contains no separate Engineering Workflow layer. All workflow documents — `CONTRIBUTING.md` (C-02), `PROJECT_BOOTSTRAP_GUIDE.md`, `AI_DEVELOPMENT_MANUAL.md` — are correctly assigned to Layer 5 (Developer Manuals). The Engineering Workflow concept is fully contained within Layer 5. No structural risk of layer proliferation exists.

**Correction C-09:** Engineering workflows in this framework follow an alternating structure of automated execution phases and human approval checkpoints. The canonical pattern is:

```
Workflow Phase → HITL Gate → Workflow Phase → HITL Gate → ...
```

Each **Workflow Phase** is a sequence of engineering tasks that an AI agent executes within the constraints defined by the framework's governing layers. Each **HITL Gate** is one of the five canonical gate positions defined in HE-002, at which AI execution pauses and the human — acting as Engineering CEO per AA-003 — reviews and approves the output before the next phase begins.

Layer 5 documents define which phases and gates apply to which workflow type. Skills (Layer 7) are the reusable execution units within Workflow Phases. Gate annotations in Skill documents (HE-004) declare which gate applies to each Skill's output, binding the Skill to the correct stopping point in the workflow sequence. No workflow may skip a declared HITL Gate.

---

### Point 6 — Skills use a Skill Manifest concept

**Result: Fail. Missing relationship identified.**

SK-001 through SK-004 define individual Skill documents. No Skill Manifest existed. This is a missing relationship with direct consequences for agentic discoverability: an AI agent executing future orchestration would need to enumerate all Layer 7 files to discover available skills rather than reading a single index. This is a structural gap that makes AA-003 (Planner Agent and orchestration extensibility) fragile in practice even if it does not technically block individual skill execution in v10.

**Correction C-06:** A `SKILLS.md` Skill Manifest is defined as a required Layer 7 index artifact. It is not a new concept but the formalized relationship between the Skill collection and its consumers. The Skill Manifest contains one entry per defined skill, each entry recording: skill name, primary agent role, gate position, one-line description, and file path to the full skill document. The Skill Manifest is the first file an AI agent reads when entering Layer 7.

---

### Point 7 — Prompt Library terminology is consistent

**Result: Fail. Three names found for one concept.**

Layer 8 was named "AI Prompts" in KA-001. The same layer was called "Prompts (Layer 8)" in AA-002 and "AI Prompt Pack" in DI-001. Three different names for the same layer across three decisions is a blueprint-blocking inconsistency.

**Correction C-07:** Layer 8 is renamed "Prompt Library" across all decisions. This is the canonical term. "AI Prompts" and "AI Prompt Pack" are retired. The name "Prompt Library" accurately describes the layer as a reusable collection of invocation artifacts for AI tools, paralleling "AI Skills" at Layer 7 conceptually. All references in KA-001, AA-002, DI-001, and LP-001 use "Prompt Library" exclusively.

---

### Point 8 — Runtime independence preserved

**Result: Fail before C-01. Pass after C-01.**

The only runtime dependency found was in DI-001 ("Claude Code required"), resolved by C-01. The remaining decisions are runtime-clean:

- AA-001 names agent roles, not AI tools
- SK-002 references framework rules, not runtime tools
- HE-002 defines gate positions with no runtime mechanism
- EP-001 prohibits concrete infrastructure dependencies in business logic
- TD-002 through TD-007 specify development tooling standards, which are framework conventions, not runtime AI tool dependencies

Claude, Cursor, Codex, Gemini, MCP, and Harness do not appear as framework components in any decision after C-01. They are implementation runtimes that consume framework artifacts, not components of the framework itself.

---

### Point 9 — Vendor independence preserved

**Result: Pass.**

EP-001 governs infrastructure vendor independence for LLM providers, embedding providers, vector stores, databases, and cloud services. Technology decisions TD-002 through TD-007 specify development tooling standards. These are distinct categories. Choosing Electron over PyQt, or pnpm over npm, is a framework convention decision about development tooling, not a business logic infrastructure dependency. EP-001 governs the latter. Technology Standards (Layer 3) govern the former. The distinction is architecturally sound and is not a vendor independence violation.

TD-006 (FAISS default, Qdrant supported alternative) and TD-007 (SQLite default, PostgreSQL upgrade path) both correctly specify defaults with replaceable alternatives, which is consistent with EP-001.

---

### Point 10 — Future multi-agent orchestration addable without changing framework architecture

**Result: Pass after C-06.**

Before C-06, a Planner Agent entering Layer 7 would need to scan all Skill files to build its task inventory. This is not a structural failure but an inefficiency that would require workaround tooling. After C-06, the Skill Manifest provides a stable single-read discovery point. The Planner Agent reads `SKILLS.md`, selects skills by agent role tag (AA-002) and gate annotation (HE-004), then loads individual skill documents.

The framework architecture requires no structural changes to accommodate orchestration. The eleven-layer hierarchy is stable. The agent role reservations (AA-001) are in place. Gate positions (HE-002) are named and agent-facing. The Skill Manifest (C-06) makes skill discovery O(1) rather than O(n). No layer needs to be added or modified.

---

## Correction Summary

Ten corrections applied in total. C-01 through C-07 were identified in the original validation pass. C-08, C-09, and C-10 are editorial corrections applied during finalization. None introduce new layers, new architectural concepts, or changes to any of the 38 frozen architectural decisions. All resolve contradictions, missing relationships, naming inconsistencies, field count inaccuracies, or editorial gaps identified before blueprint generation.

| ID | Type | Description |
|---|---|---|
| C-01 | Contradiction | Remove runtime dependency from DI-001; rename to tool-agnostic language |
| C-02 | Classification | `CONTRIBUTING.md` → Layer 5 |
| C-03 | Classification | `FRAMEWORK_HANDOVER.md` → Layer 10 |
| C-04 | Classification | `V10_MIGRATION_NOTES.md` → Layer 11 (resolves KA-004 violation) |
| C-05 | Minimum artifact | `TEMPLATE_SPEC.md` and `template-fastapi-sqlite/` seed Layer 9 (resolves KA-004 violation) |
| C-06 | Missing relationship | `SKILLS.md` Skill Manifest defined as required Layer 7 index artifact |
| C-07 | Naming | Layer 8 renamed "Prompt Library" across all decisions |
| C-08 | Editorial | Skill document structure field count corrected from six to seven |
| C-09 | Editorial | Engineering Workflow pattern formalized as Workflow Phase → HITL Gate → Workflow Phase → HITL Gate in Layer 5 |
| C-10 | Editorial | `TEMPLATE_SPEC.md` Project Template Specification defined as required Layer 9 meta-artifact; concrete templates inherit structural requirements from the specification |

---

## Blueprint Input Frozen.

---

## Frozen Blueprint Input Checklist

The following inputs are confirmed consistent and complete for Blueprint Generation. Every item in this checklist is derived from the 38 frozen architectural decisions and the 10 corrections above. No item introduces content beyond what has already been established.

---

### Architecture Foundation

- [ ] Framework vision: Engineering Operating System for Agentic Software Development (FD-001)
- [ ] Tri-level operating model: Human → Framework → AI; AI agents operate exclusively at Level 3 (FD-002)
- [ ] Agentic design assumption: reserve extension points without implementing orchestration; no structural changes required when orchestration is introduced (FD-003)

---

### Knowledge Architecture — 11 Layers

- [ ] **Layer 1 — Constitution:** single document, highest authority, changed only by Owner decision recorded in DECISIONS.md
- [ ] **Layer 2 — Framework Rules:** global rules, inheritable by all lower layers; child layers do not restate content from this layer
- [ ] **Layer 3 — Technology Standards:** technology stack; referenced by Project Rules and Skills
- [ ] **Layer 4 — Project Rules:** inherit from Layers 2 and 3; do not restate global content
- [ ] **Layer 5 — Developer Manuals:** operational guidance including engineering workflow; workflow is organized as an alternating sequence of Workflow Phases and HITL Gates: **Workflow Phase → HITL Gate → Workflow Phase → HITL Gate**; Skills (Layer 7) are the execution units within Workflow Phases; gate annotations in Skill documents (HE-004) bind each Skill to the appropriate HITL Gate stopping point; no workflow may skip a declared gate (C-09)
- [ ] **Layer 6 — Reference Implementations:** vendor-specific and implementation-specific examples, not framework mandates; every document in this layer carries a status header declaring its Layer 6 classification
- [ ] **Layer 7 — AI Skills:** `SKILLS.md` Skill Manifest as required index + individual Skill documents; Skill Manifest is the first artifact read by any AI agent entering this layer (C-06)
- [ ] **Layer 8 — Prompt Library:** individual Prompt documents per AI tool; tool selection is a developer and project-level decision, not a framework mandate (C-07)
- [ ] **Layer 9 — Project Templates:** `TEMPLATE_SPEC.md` Project Template Specification as required meta-artifact defining mandatory structure, mandatory folders, mandatory documents, mandatory Skills, and mandatory Prompt Library artifacts; concrete template directories (e.g., `template-fastapi-sqlite/`) inherit structural requirements from the specification and MUST conform to it; full template set deferred to v10.1 (C-05, C-10)
- [ ] **Layer 10 — Knowledge Base:** `DECISIONS.md` + `FRAMEWORK_HANDOVER.md`
- [ ] **Layer 11 — Reference Documents:** `V10_MIGRATION_NOTES.md` as v10 seed (C-04)
- [ ] Layer authority rule: higher-numbered layers never override lower-numbered layers; Layer 1 always has final authority (KA-002)
- [ ] Governing layers (1–6) MUST be written in English; operational layers (7–11) MAY be written in the developer's primary language when the content targets human developers, and MUST be written in English when the content is executed by an AI agent (LP-001)

---

### Document Status System

- [ ] Four statuses are defined and MUST appear in every framework document header: `Constitution`, `Active`, `Legacy`, `Deprecated` (DL-001)
- [ ] Deprecated document list in DL-002 is applied immediately upon framework v10 freeze (DL-002)
- [ ] `V10_MIGRATION_NOTES.md` remains `Active` until all v10 Tier 1 and Tier 2 documents are complete, then transitions to `Deprecated` (DL-003)

---

### Document Inventory for Blueprint Generation

**Tier 1 — Critical (v10 is not releasable without these):**

- [ ] `global_rules_revisionfinal_v10.md` — Layer 2
- [ ] `project-pc-app_v04.md` (Electron-based) — Layer 4
- [ ] `PROJECT_BOOTSTRAP_GUIDE.md` — Layer 5
- [ ] `FRAMEWORK_README.md` (designated session entry point for all AI agents and new developers) — Layer 5
- [ ] `DECISIONS.md` with initial entries — Layer 10
- [ ] `SKILLS.md` Skill Manifest — Layer 7
- [ ] Minimum three Skill documents: `create-feature`, `review-code`, `generate-tests` — Layer 7
- [ ] `TEMPLATE_SPEC.md` Project Template Specification — Layer 9

**Tier 2 — Required after Tier 1 is complete:**

- [ ] `project-personal-full-stack_v01.md` — Layer 4
- [ ] `project-monolithic_v04.md` — Layer 4
- [ ] `COMMANDS.md` — Layer 5
- [ ] `PROJECT_STRUCTURE.md` — Layer 5
- [ ] Minimum Prompt documents for two distinct AI tools — Layer 8
- [ ] `template-fastapi-sqlite/` scaffold conforming to `TEMPLATE_SPEC.md` — Layer 9

**Tier 3 — Deferred to v10.1:**

- [ ] `AI_DEVELOPMENT_MANUAL.md` — Layer 5
- [ ] `project-mobile_v01.md` — Layer 4
- [ ] Full Project Template set (all types, all conforming to `TEMPLATE_SPEC.md`) — Layer 9
- [ ] Full Prompt Library expansion covering all five AI tools — Layer 8

---

### Technology Decisions

- [ ] Desktop framework: Electron. PyQt, PySide, PyInstaller, and Inno Setup are Deprecated (TD-002)
- [ ] JavaScript package manager: pnpm. npm and yarn are Deprecated in all v10 framework documents (TD-003)
- [ ] State management: Zustand is the sole approved library. Recoil is removed from the approved list (TD-004)
- [ ] Python package manager: Poetry is primary; uv is a formally supported alternative (TD-005)
- [ ] Vector store default: FAISS for new projects requiring in-process vector search. Qdrant is the formally supported alternative when persistent, filterable, server-based search is required; no project rule mandates Qdrant as a default (TD-006)
- [ ] Database default: SQLite for all project types including full-stack. PostgreSQL is the upgrade path for multi-user concurrency, high write throughput, or PostgreSQL-specific features; no project rule implies PostgreSQL as the starting point (TD-007)

---

### Agentic Architecture

- [ ] 13 named agent roles reserved as architectural designations, not implementations: Planner Agent, PM Agent, Architect Agent, Backend Agent, Frontend Agent, Desktop Agent, Mobile Agent, Database Agent, Documentation Agent, Test Agent, Code Review Agent, DevOps Agent, Release Agent (AA-001)
- [ ] Every Skill document and every Prompt Library entry declares a primary agent role from the reserved list (AA-002)
- [ ] Human role is defined as Engineering CEO: defines vision, approves HITL Gates, makes architectural decisions; does not perform engineering execution within the delegation scope of a framework-defined agent role (AA-003)
- [ ] Framework role is defined as orchestrator: the knowledge substrate that enables consistent, autonomous AI execution; framework quality is measured by how little human clarification is required for an AI agent to produce correct output using only framework documents (AA-004)

---

### Skill System

- [ ] A Skill is defined as a named, reusable engineering capability that can be executed by a human developer or assigned to an AI agent (SK-001)
- [ ] Skill document structure: **seven required fields** — `skill-name`, `primary-agent-role`, `gate`, `input`, `output`, `steps`, `framework-alignment`; no Skill document is valid without all seven fields populated (SK-002, C-08)
- [ ] Skill scope: executes within one project; does not perform cross-project operations; does not modify any framework document in Layers 1 through 4 (SK-003)
- [ ] `SKILLS.md` Skill Manifest: one entry per Skill; each entry records skill name, primary agent role, gate position, one-line description, and file path to the full Skill document (C-06)
- [ ] v10 starter Skills: `create-feature`, `review-code`, `generate-tests` (SK-004)

---

### Harness Engineering (HITL Gates)

- [ ] HITL gates are defined as canonical stopping points in engineering workflows at which AI execution pauses and human approval is required before the next Workflow Phase begins (HE-001)
- [ ] Five canonical gate positions are named and frozen: **Gate 1 — Plan Approval**, **Gate 2 — Architecture Approval**, **Gate 3 — Scope Approval**, **Gate 4 — Implementation Approval**, **Gate 5 — Release Approval** (HE-002)
- [ ] Gate implementation mechanism — how AI execution pauses, how human approval is captured, how execution resumes — is not specified in v10; gate positions are frozen, not their runtime mechanism (HE-003)
- [ ] Every Skill document MUST declare its applicable gate in the `gate` field; this gate annotation is required metadata per SK-002; a value of `none` is permitted when a Skill produces no artifact requiring human approval (HE-004)

---

### Engineering Principles

- [ ] Vendor independence: business logic MUST NOT import or depend on concrete infrastructure implementations including LLM providers, embedding providers, vector stores, database engines, and cloud services; the constraint is stated in the global rules; the implementation pattern is demonstrated in Reference Implementations (EP-001)
- [ ] TDD applies to business logic and domain services: the test MUST be written before the implementation for any service layer or domain function; integration tests are required for all API boundaries; no coverage percentage thresholds are specified (EP-002)
- [ ] Runtime independence: Claude, Cursor, Codex, Gemini, MCP, and Harness are implementation runtimes that consume framework artifacts; they are not components of the framework itself and MUST NOT be named as required framework dependencies (C-01, Point 8)

---

### Process Rules

- [ ] `CONTRIBUTING.md` is extended with a mandatory sixth step: when a commit introduces or changes an architectural decision, a record MUST be appended to `DECISIONS.md` in the same commit; this step is not optional (PR-001, C-02)
- [ ] `DECISIONS.md` is an append-only document; each entry records: date, decision identifier, context, options considered, decision made, and rationale; it is the authoritative chronological record of significant architectural choices (PR-002)

---

### Project Template Specification

- [ ] `TEMPLATE_SPEC.md` is a required Layer 9 meta-artifact and MUST be created before any concrete template directory; it is the parent specification from which all concrete templates inherit their structural requirements (C-10)
- [ ] The specification MUST define the following for any conforming template: required directory structure, mandatory folders, mandatory documents (e.g., `README.md`, `.env.example`, `.gitignore`), mandatory Skills that the template references or includes, and mandatory Prompt Library artifacts that the template references or includes
- [ ] All concrete template directories (e.g., `template-fastapi-sqlite/`) MUST conform to `TEMPLATE_SPEC.md`; a template that does not satisfy every requirement in the specification is invalid and MUST NOT be placed in Layer 9
- [ ] `TEMPLATE_SPEC.md` is placed at the root of Layer 9 and is the first artifact read by any AI agent or developer before creating or validating a project template
- [ ] The specification itself inherits its authority from Layer 5 Developer Manuals and Layer 2 Framework Rules; it does not introduce project-type-specific technology choices — those belong in Layer 4 Project Rules referenced by each concrete template
