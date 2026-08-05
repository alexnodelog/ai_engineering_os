# DECISIONS.md

**Status:** Active
**Layer:** 10 — Knowledge Base
**Tier:** 1 (Critical)
**Purpose:** The framework's single, append-only, chronological record of architectural decisions. Every entry MUST record: date, decision identifier, context, options considered, decision made, and rationale (`global_rules_revisionfinal_v10.md`, Section 8.3; `FRAMEWORK_BLUEPRINT.md`, Section 13.1, PR-002).
**Authority:** Structural derivative of `FRAMEWORK_BLUEPRINT.md`, Sections 2.10 and 13. This document introduces no architectural decision of its own. It records decisions frozen elsewhere (`BLUEPRINT_INPUT_FREEZE.md`) and, from this point forward, decisions approved at Gate 2 — Architecture Approval (`FRAMEWORK_BLUEPRINT.md`, Section 18.2) or at Gate 1 — Plan Approval, where a Gate 1 event itself constitutes the decision being recorded (`FRAMEWORK_BLUEPRINT.md`, Section 18.4).
**Inherits from:** Every layer may produce a decision worth recording here (`FRAMEWORK_BLUEPRINT.md`, Section 2.10, "Inputs"). This document does not inherit rules to operationalize — it inherits *events* to record.
**Governs:** Nothing. Per `FRAMEWORK_BLUEPRINT.md`, Section 2.10 ("Prohibited Responsibilities") and Section 4 ("Dependency Hierarchy," load-bearing property 1), Layer 10 **MUST NOT** be treated as a governing layer. An entry below documents that some governing-layer document was changed; it does not itself change Layer 1–4 rules.
**Companion document:** `FRAMEWORK_HANDOVER.md` (Layer 10) — the living session-continuity document, updated alongside every entry below per the mandatory co-update rule (PR-001).
**Read order:** Read as Step 5 of the AI Session Initialization Sequence, after `FRAMEWORK_README.md`, the Constitution, `global_rules_revisionfinal_v10.md`, and `global_technology_stack_v10.md`, and before `FRAMEWORK_HANDOVER.md` (`FRAMEWORK_BLUEPRINT.md`, Section 14). An agent **MUST** read this document before proposing or making any architectural decision, to avoid re-litigating a settled question.
**RFC 2119:** The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in Sections 0–2 below are to be interpreted as described in RFC 2119. They govern how this document is maintained; they do not retroactively alter the historical entries in Section 3.

---

## 0. How to Use This Document

This document answers exactly one question: **"has this question already been decided, and if so, what was decided and why?"** It does not contain rules to follow directly — for that, consult the governing layer the entry points to (Layer 1–4). It contains the *history* that produced those rules, so that a future session, human or AI, does not reopen a question the framework has already settled without a Gate event to justify reopening it.

An AI agent **MUST NOT** treat an entry in this document as itself a source of binding rules. If an entry and a governing-layer document appear to disagree, the governing-layer document is authoritative and the discrepancy **MUST** be raised for correction — either the entry mis-recorded the decision, or the governing document has drifted from what was actually decided (`FRAMEWORK_BLUEPRINT.md`, Section 6).

---

## 1. Entry Format

Every entry in Section 3, and every entry appended after this document's creation, **MUST** populate the following fields in full. An entry missing a required field is not valid and **MUST NOT** be appended as-is.

| Field | Description |
|---|---|
| **Date** | The date the decision was made (not the date it was transcribed into this log, where the two differ — see Section 2.1). |
| **Decision ID(s)** | The frozen identifier(s) this entry records, using the prefixes defined in `FRAMEWORK_BLUEPRINT.md` (`FD-`, `TD-`, `DL-`, `KA-`, `DI-`, `LP-`, `CC-`, `PR-`, `EP-`, `AA-`, `SK-`, `HE-`) or, for a decision made after the original freeze, a new sequential `DEC-` identifier in this same log's own numbering. |
| **Context** | The real engineering need or inconsistency that made a decision necessary. |
| **Options Considered** | The alternatives that were weighed, including the one(s) rejected. |
| **Decision** | The specific choice made, stated as a conclusion, not a discussion. |
| **Rationale** | Why this choice was preferred, traceable to the Constitution's value ordering (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 4) wherever possible. |
| **Supersedes / Superseded By** | Cross-reference to any entry this one reverses or is reversed by (Section 2.2). Stated as "None" where not applicable. |

---

## 2. Append-Only Constraint (Binding)

### 2.1 No Silent Edits

Entries in Section 3, once appended, **MUST NOT** be edited or deleted. A decision that is later reversed **MUST** be recorded as a **new** entry that explicitly references the entry it supersedes, per `FRAMEWORK_BLUEPRINT.md`, Section 13.2. This preserves a complete, tamper-evident history of how the framework's architecture evolved.

### 2.2 Superseding a Decision

A superseding entry **MUST** populate its own "Supersedes" field with the ID of the entry it reverses, and the original entry's "Superseded By" field **MAY** be updated to point forward — this is the one narrow exception to Section 2.1, since it is an addition of a cross-reference, not a rewrite of the original decision's content.

### 2.3 Mandatory Co-Update Rule (PR-001)

Any commit that introduces or changes an architectural decision **MUST** append an entry to this document in the same commit, and **MUST** update `FRAMEWORK_HANDOVER.md` in the same commit, per `global_rules_revisionfinal_v10.md`, Section 5.4, and `FRAMEWORK_BLUEPRINT.md`, Section 13.3. This is not optional and is not satisfied by a later, separate commit.

**Corollary — no entry without a decision.** The converse of PR-001 also holds: an entry **MUST NOT** be appended to Section 3 unless a genuine architectural decision was actually made. Every framework document generated since this log's seeding through `OPENAI_PROMPTS.md` (`skill-create-feature.md`, `skill-review-code.md`, `skill-generate-tests.md`, `TEMPLATE_SPEC.md`, `project-personal-full-stack_v01.md`, `project-monolithic_v04.md`, `COMMANDS.md`, `PROJECT_STRUCTURE.md`, `CLAUDE_CODE_PROMPTS.md`, `OPENAI_PROMPTS.md`) explicitly states, in its own closing text, that it introduces no new architectural decision beyond what `FRAMEWORK_BLUEPRINT.md` already assigns to its layer's responsibility. None of these, therefore, triggers PR-001, and none is recorded as a new entry below (see Section 3.1).

`template-fastapi-sqlite/`, by contrast, depended on a genuine architectural decision — which of two valid Layer 4 archetypes it targets — that no prior document had settled. That decision, together with a second, separate Gate 1 decision made in the same window, **is** recorded below, as `DEC-010` and `DEC-011` (Section 3, and Section 3.2's note on this revision).

---

## 3. Seed Entries — Framework v10 Foundational Decisions

### 3.0 Note on Seeding

This document did not exist at the time `BLUEPRINT_INPUT_FREEZE.md` froze the framework's 38 architectural decisions and 10 corrections. The entries below **retrospectively** record that freeze, now that Layer 10 exists to hold it, consistent with `PROJECT_BOOTSTRAP_GUIDE.md`, Section 5.6, which anticipates exactly this situation for a still-missing `DECISIONS.md`. Entries are grouped by the decision cluster they belong to, rather than one entry per individual ID, because the underlying decisions were made together as part of a single freeze event; each entry's "Decision ID(s)" field lists every specific identifier it covers, so no individual decision is left untraceable.

Not every decision identifier reserved in the freeze is independently reconstructed below. Specifically, **TD-001, TD-003, TD-004, and TD-005**, and the entire **`LP-`** series, are referenced in `FRAMEWORK_BLUEPRINT.md`'s traceability header but are not otherwise described in the documents available to this generator at seeding time. Rather than inventing their context, options, or rationale, this document records their existence as a known gap in this log (see Section 4). **`BLUEPRINT_INPUT_FREEZE.md` remains the sole authoritative source for those identifiers** until an entry for them can be transcribed accurately from that document.

### 3.1 Note on a Prior Revision — Verification Pass, No New Entries

This note records a prior revision of `DECISIONS.md`, generated to bring the document's currency in line with the framework's state at that time, following the Owner's request to update it alongside `FRAMEWORK_README.md`. Every document generated in this framework since the original seeding, through `OPENAI_PROMPTS.md` (the framework's most recent artifact as of that revision), was reviewed against the append-only, decision-triggered standard stated in Section 2.3 above. None qualified: each one explicitly stated, in its own text, that it introduced no architectural decision beyond what `FRAMEWORK_BLUEPRINT.md` already assigns to its own layer's responsibility — every directory layout, naming convention, wrapper structure, and reporting taxonomy each one defined was a direct application of an already-frozen Layer 1–3 rule to a new archetype, tool, or artifact, not a new decision at the Layer 1–3 level.

Two status transitions occurred during that same window that were worth noting explicitly, because they were *mechanical consequences* of already-frozen completeness criteria rather than decisions in their own right, and therefore likewise did not warrant a new Section 3 entry at that time:

- **Layer 7 (AI Skills) reached `Active` as a whole** upon `skill-generate-tests.md`'s generation, per the pre-existing criterion at `FRAMEWORK_BLUEPRINT.md`, Section 2.7 ("Active once the manifest and three starter Skills exist").
- **Layer 8 (Prompt Library) reached `Active` as a whole** upon `OPENAI_PROMPTS.md`'s generation, per the pre-existing criterion at `FRAMEWORK_BLUEPRINT.md`, Sections 2.8 and 11.3 ("Active once at least two tool-specific Prompt sets exist").

Both criteria were fixed at the original freeze (DEC-002, DEC-006, DEC-009 below); their satisfaction was a status observation, not a new architectural decision, in exactly the sense `FRAMEWORK_STATUS.md` itself describes for both transitions. That revision was recorded as a verification action rather than as a new milestone.

The Section 4.2 known gap (`TD-001`, `TD-003`, `TD-004`, `TD-005`, the `LP-` series) remained open as of that revision: closing it requires direct transcription from `BLUEPRINT_INPUT_FREEZE.md`, which was not available to that generator either. The gap was not fabricated shut; it remained accurately reported in Section 4.2.

That revision also recorded, correctly for its time, that the archetype-disambiguation question for `template-fastapi-sqlite/` — whether it should target `project-personal-full-stack_v01.md` or `project-monolithic_v04.md` — was still open and pending explicit Owner confirmation, and that this document therefore correctly did not record it as a decision. That question has since been resolved; see `DEC-010` and Section 3.2, below.

---

### DEC-001 — Framework Vision and the Tri-Level Operating Model

**Date:** 2026-06 (exact day not separately recorded in source documents; the freeze predates this log's creation)
**Decision ID(s):** FD-001, FD-002, FD-003
**Context:** Prior to the freeze, the repository had no single stated purpose distinguishing this system from an ordinary style guide or a loose collection of best practices, and no explicit boundary existed between human direction, framework structure, and AI execution — creating a risk that these three responsibilities would collapse into one another as the framework grew.
**Options Considered:** A two-level model collapsing "Structure" and "Execution" into a single layer; leaving human/agent responsibility boundaries implicit and resolved case-by-case, rather than frozen as a named model. Both were rejected in favor of an explicit, named three-level separation. Full deliberation predates this log and is recorded in `BLUEPRINT_INPUT_FREEZE.md`.
**Decision:** FD-001 — the framework is chartered as an Engineering Operating System (EOS) for Agentic Software Development, not a style guide. FD-002 — three operating levels are established (1: Direction/Human, 2: Structure/Framework, 3: Execution/AI Agents) and **MUST NEVER** be collapsed into one another. FD-003 — multi-agent orchestration is explicitly out of scope for v10, but every artifact **MUST** be designed so a future orchestration layer can consume it without structural change.
**Rationale:** Per `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 7, and `FRAMEWORK_BLUEPRINT.md`, Section 1.1, the framework's success is measured by how little human clarification an AI agent requires to produce correct output — an explicit tri-level model is what makes that criterion enforceable rather than aspirational. Deferring orchestration *mechanism* while freezing orchestration *readiness* preserves future optionality without introducing complexity the framework does not yet need, consistent with the Constitution's Simplicity value.
**Supersedes / Superseded By:** None.

---

### DEC-002 — Eleven-Layer Knowledge Architecture and Document Lifecycle

**Date:** 2026-06
**Decision ID(s):** KA-001, KA-002, KA-003, KA-004, DL-001, DL-002, DL-003
**Context:** The pre-freeze document set was organized ad hoc — independently versioned files (`v02`, `v03`, `v04`), duplicate-purpose documents in different languages, and no stated rule for which document wins when two disagree.
**Options Considered:** Continuing with independently versioned documents and no formal precedence rule, relying on judgment case-by-case; versus adopting a single, numbered layer hierarchy with a strict, mechanical downward-authority rule. A partially-numbered hierarchy covering only governing documents (leaving Skills/Prompts/Templates unclassified) was also implicitly rejected in favor of classifying all eleven layers, including operational ones.
**Decision:** KA-001–004 — eleven layers are adopted: Layers 1–6 (Constitution, Framework Rules, Technology Standards, Project Rules, Developer Manuals, Reference Implementations) are governing; Layers 7–11 (AI Skills, Prompt Library, Project Templates, Knowledge Base, Reference Documents) are operational. Authority flows strictly downward by layer number; operational layers never override governing layers; each operational layer must contain at least one valid, classified artifact for framework completeness. DL-001–003 — every document carries exactly one of four statuses (`Constitution` / `Active` / `Legacy` / `Deprecated`); a named set of legacy documents is immediately deprecated at freeze; `V10_MIGRATION_NOTES.md` carries a pre-scheduled, condition-triggered transition from `Active` to `Deprecated` once every Tier 1/2 v10 document exists.
**Rationale:** A numbered, strictly-ordered hierarchy makes "which document governs this question" a mechanical lookup rather than a judgment call repeated in every session, directly serving the Constitution's Consistency and AI Readability values (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 4).
**Supersedes / Superseded By:** None.

---

### DEC-003 — Delivery Tiering and Cross-Cutting Reclassifications

**Date:** 2026-06
**Decision ID(s):** DI-001, DI-002, CC-001, CC-002
**Context:** Without an explicit generation order, lower-priority documents risked being generated before the foundational Layer 2 document every Layer 4 document depends on; separately, `CONTRIBUTING.md` and the Reference Implementation chapters lacked a settled layer classification, and no single document was designated as the mandatory entry point for a new session.
**Options Considered:** An unordered, best-effort generation approach versus a strict tiered order gating Tier 2 generation behind full Tier 1 completion. For CC-002, specifically: leaving session entry unstructured (any document could plausibly be read first) versus naming one document as the mandatory first read.
**Decision:** DI-001 — `global_rules_revisionfinal_v10.md` is designated the framework's single highest-priority Tier 1 deliverable. DI-002 — no Tier 2 document generation begins until every Tier 1 document exists. CC-001 — Reference Implementation vendor choices (HyperCLOVA X, BGE-M3, Qdrant, NextChat) are explicitly marked as implementation-specific examples, not Layer 3 framework defaults. CC-002 — `FRAMEWORK_README.md` is designated the mandatory first-read entry point for every new AI agent session and every new developer.
**Rationale:** Strict tiering prevents a Layer 4 document from being authored on an incomplete Layer 2 foundation (`FRAMEWORK_BLUEPRINT.md`, Section 17). The CC-001/CC-002 reclassifications remove ambiguity that would otherwise force every new session to rediscover — rather than simply be told — where to start and what is illustrative versus binding.
**Supersedes / Superseded By:** None.

---

### DEC-004 — Process Rules and Engineering Principles

**Date:** 2026-06
**Decision ID(s):** PR-001, PR-002, EP-001, EP-002
**Context:** An append-only decision log is only trustworthy if every architectural change is captured at the moment it happens, not reconstructed later from memory. Separately, engineering rigor (vendor independence, test-first development) needed a stated boundary, since unconditional, universal enforcement was judged disproportionate to the framework's personal/small-team scale.
**Options Considered:** Recording decisions retroactively or periodically, on a best-effort basis, versus requiring a same-commit co-update (PR-001). Universal, mandatory TDD across all code versus a zoned boundary concentrating rigor where correctness risk and maintenance cost are highest (EP-002). Treating vendor independence as aspirational guidance only versus a binding rule with a defined enforcement mechanism at the infrastructure boundary (EP-001).
**Decision:** PR-001 — any commit introducing or changing an architectural decision **MUST** append a `DECISIONS.md` entry in the same commit, with `FRAMEWORK_HANDOVER.md` updated alongside it. PR-002 — every entry **MUST** record date, decision identifier, context, options considered, decision, and rationale. EP-001 — vendor independence is binding: business logic **MUST NOT** depend directly on a specific vendor's SDK/API surface; replacing a vendor **MUST** be confined to the infrastructure/adapter layer. EP-002 — TDD is mandatory for business/domain logic, recommended-but-not-mandatory for infrastructure adapters, and optional for UI/framework-glue code.
**Rationale:** Same-commit recording (PR-001) is the only mechanism that prevents institutional memory from silently drifting away from decisions actually made. The TDD zoning in EP-002 concentrates cost where the Constitution's maintainability priority is most at stake, without imposing enterprise-grade rigor on throwaway or presentation code — consistent with the Constitution's rejection of premature complexity (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 8).
**Supersedes / Superseded By:** None.

---

### DEC-005 — Named Agent Role Architecture

**Date:** 2026-06
**Decision ID(s):** AA-001, AA-002, AA-003, AA-004
**Context:** DEC-001's FD-003 commits the framework to being orchestration-ready without implementing orchestration in v10. This requires some mechanism, available today, that a future Planner Agent could consume without requiring the framework to change shape later.
**Options Considered:** Leaving agent responsibility implicit within each Skill's free-text description, to be inferred by whichever tool executes it, versus formally reserving a fixed, named set of agent roles attached as metadata to every operational artifact.
**Decision:** AA-001 — a fixed set of thirteen reserved agent roles is established. AA-002 — every Skill and Prompt document **MUST** declare exactly one `primary-agent-role` drawn from that set. AA-003 — the human is formally named "Engineering CEO" within the tri-level model. AA-004 — framework success is defined by how little human clarification an AI agent requires to produce correct, consistent output.
**Rationale:** Named, reserved roles are what make the FD-003 orchestration-readiness guarantee concrete rather than aspirational — a future Planner Agent can route work by matching `primary-agent-role` metadata without any existing Layer 7/8 document needing to be rewritten (`FRAMEWORK_BLUEPRINT.md`, Section 18.5).
**Supersedes / Superseded By:** None.

---

### DEC-006 — Skill Architecture

**Date:** 2026-06
**Decision ID(s):** SK-001, SK-002, SK-003, SK-004
**Context:** The framework needed one atomic, discoverable unit of AI-executable engineering work, rather than leaving "what an AI agent actually does" to be reinvented in every project.
**Options Considered:** An unstructured, freeform task-description approach decided per project versus a formally specified Skill document type with a mandatory discovery manifest and a fixed, required field set.
**Decision:** SK-001 — a Skill is the atomic, named, reusable unit of AI-executable engineering work. SK-002 — every Skill document **MUST** populate seven required fields, including a `framework-alignment` field and a `gate` field. SK-003 — a Skill executes within exactly one project and **MUST NOT** modify a Layer 1–4 document or perform cross-project operations. SK-004 — three starter Skills (`create-feature`, `review-code`, `generate-tests`) are required for Tier 1 completion.
**Rationale:** A fixed field set plus a required manifest (`SKILLS.md`) make Skill discovery a single-read operation instead of a directory scan, directly serving both AI Readability and the orchestration-extensibility guarantee (`FRAMEWORK_BLUEPRINT.md`, Sections 10, 18.5).
**Supersedes / Superseded By:** None.

---

### DEC-007 — HITL Gate Architecture

**Date:** 2026-06
**Decision ID(s):** HE-001, HE-002, HE-003, HE-004
**Context:** Because the human is Engineering CEO (AA-003) and AI agents execute autonomously (Level 3 of the tri-level model, DEC-001), consequential engineering decisions need defined stopping points where AI execution pauses for human approval — without this, "autonomous execution" and "human direction" could not safely coexist.
**Options Considered:** An unstructured, ad hoc "ask the human when it seems necessary" approach, decided in the moment by whichever agent is executing, versus five named, frozen, universally-numbered gate positions with defined blocking effects. Also considered: specifying the technical pause/resume mechanism now versus deferring it to a future orchestration layer.
**Decision:** HE-001 — HITL Gates exist because consequential decisions require CEO approval before AI agents are permitted to proceed into the next phase. HE-002 — five canonical gates are named and frozen: Plan Approval, Architecture Approval, Scope Approval, Implementation Approval, Release Approval. HE-003 — the technical mechanism for pausing/resuming at a Gate is explicitly deferred and not specified at v10. HE-004 — every Skill **MUST** declare, via a `gate` field, which Gate its output requires, or the literal value `none`.
**Rationale:** Freezing the gate *positions* while deferring the pause/resume *mechanism* lets the framework state a binding human-approval requirement today without prematurely committing to a specific runtime implementation — consistent with the Constitution's Technology Evolution principle that architecture is long-lived while technology is temporary (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 18).
**Supersedes / Superseded By:** None.

---

### DEC-008 — Technology Defaults Selected at Freeze

**Date:** 2026-06
**Decision ID(s):** TD-002, TD-006, TD-007
**Context:** Several technology questions had accumulated conflicting legacy answers across superseded documents — a PyQt-based desktop stack, and unresolved vector-store and database defaults — requiring one authoritative resolution consistent with the Constitution's persistence and vendor-independence philosophy.
**Options Considered:** TD-002 — Electron versus continuing with PyQt/PySide for the application shell, and PyInstaller/Inno Setup for packaging. TD-006 — FAISS versus Qdrant as the default vector store. TD-007 — SQLite versus PostgreSQL as the default database.
**Decision:** TD-002 — Electron is the standing desktop-framework decision; PyQt, PySide, PyInstaller, and Inno Setup are deprecated for all new desktop work. TD-006 — FAISS is the default vector store, with Qdrant retained as a supported alternative for production-grade persistent search. TD-007 — SQLite is the default database, with PostgreSQL retained as a defined upgrade path.
**Rationale:** Each default follows the Constitution's stated priority of maintainability and zero-configuration simplicity for personal/small-scale development (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Sections 8–9, 12), while explicitly preserving a supported upgrade path — satisfying the vendor-independence rule (EP-001, DEC-004) that a default must remain replaceable rather than become a hard dependency.
**Supersedes / Superseded By:** None.

---

### DEC-009 — Ten Corrections Applied to the Freeze

**Date:** 2026-06
**Decision ID(s):** C-01, C-02, C-03, C-04, C-05, C-06, C-07, C-08, C-09, C-10
**Context:** The audit that produced `BLUEPRINT_INPUT_FREEZE.md` surfaced ten specific structural inconsistencies in the draft freeze — a document reclassified to the wrong layer, a naming collision across prior document sets, and an apparent contradiction between two frozen rules among them — requiring correction before the freeze could be considered internally consistent.
**Options Considered:** Leaving each inconsistency for a future framework version to resolve versus correcting each as part of the same freeze event, so that `FRAMEWORK_BLUEPRINT.md` would not need to knowingly reproduce an inconsistent decision set.
**Decision:** C-01 — no specific AI tool or runtime is a required framework dependency at the architecture level. C-02 — `CONTRIBUTING.md` is reclassified to Layer 5. C-03 — `FRAMEWORK_HANDOVER.md` is reclassified to Layer 10. C-04 — `V10_MIGRATION_NOTES.md` is reclassified to Layer 11. C-05 — the apparent contradiction between "full Layer 9 template population is deferred to v10.1" and "every operational layer must contain at least one valid artifact" is resolved: `TEMPLATE_SPEC.md` plus one seed template satisfies the v10 minimum. C-06 — `SKILLS.md` is required as the Layer 7 discovery manifest. C-07 — "Prompt Library" is adopted as the single canonical name for Layer 8, replacing three prior inconsistent names. C-08 — every Skill document's required field count is corrected to seven. C-09 — the alternating Workflow-Phase/HITL-Gate pattern is formalized as the framework's one canonical workflow structure. C-10 — `TEMPLATE_SPEC.md` is required as the Layer 9 specification-first parent artifact.
**Rationale:** Applying corrections at freeze time, rather than deferring them, ensures `FRAMEWORK_BLUEPRINT.md` — the master structural reference every future document must conform to — starts from an internally consistent state, rather than propagating a known defect forward into every derivative document.
**Supersedes / Superseded By:** None.

---

### DEC-010 — `template-fastapi-sqlite/` Archetype Target Confirmed

**Date:** 2026-08-03
**Decision ID(s):** DEC-010
**Context:** `template-fastapi-sqlite/` is the concrete Layer 9 seed template `TEMPLATE_SPEC.md`, Section 12, identifies as the artifact required, together with `TEMPLATE_SPEC.md` itself, to satisfy the Layer 9 KA-004 minimum-artifact requirement (correction C-05, `DEC-009`). Following `project-monolithic_v04.md`'s generation, two `Active` Layer 4 archetype documents were both capable of hosting a FastAPI-based project — `project-personal-full-stack_v01.md` (Full-Stack) and `project-monolithic_v04.md` (Monolithic) — and neither `TEMPLATE_SPEC.md`, Section 5, Rule 1, nor `PROJECT_STRUCTURE.md` stated which one `template-fastapi-sqlite/` was intended to conform to. This gap was tracked as `FRAMEWORK_STATUS.md`, Flag 10, and as this document's own Section 4.3.
**Options Considered:** (a) Target `project-personal-full-stack_v01.md` — a Full-Stack archetype composed of a separately deployable FastAPI backend and SPA frontend, communicating over a versioned HTTP/REST API boundary. (b) Target `project-monolithic_v04.md` — a Monolithic archetype composed of a single deployable unit, one FastAPI process serving both its own HTTP API and a compiled React frontend.
**Decision:** `template-fastapi-sqlite/` conforms to `project-personal-full-stack_v01.md` (the Full-Stack archetype).
**Rationale:** Archetype selection is a Level 1 (human Engineering CEO) decision that an AI agent MUST NOT make on the human's behalf, per `PROJECT_BOOTSTRAP_GUIDE.md`, Section 4.1, and `TEMPLATE_SPEC.md`, Section 5, Rule 1, itself requires this to be confirmed by the Owner rather than assumed. The Owner made this selection explicitly, in direct response to a targeted question, resolving the ambiguity `TEMPLATE_SPEC.md` and `PROJECT_STRUCTURE.md` both deliberately left open, since neither document is authorized to make archetype-selection decisions on Layer 9's behalf.
**Supersedes / Superseded By:** None.

---

### DEC-011 — Mobile Archetype (`project-mobile_v01.md`) Pulled Forward from v10.1

**Date:** 2026-08-03
**Decision ID(s):** DEC-011
**Context:** `project-mobile_v01.md` (Layer 4, Mobile Application archetype) is designated Tier 3 / v10.1 scope (`FRAMEWORK_BLUEPRINT.md`, Sections 3, 17, 18.4). Per `FRAMEWORK_BLUEPRINT.md`, Section 18.4, and `PROJECT_BOOTSTRAP_GUIDE.md`, Section 5.3, no Tier 3 item may be pulled forward ahead of Tier 1/Tier 2 completion without an explicit Gate 1 (Plan Approval) decision recorded in this log. With Framework v10 Tier 1 and Tier 2 both complete as of `DEC-010` (this same window — `template-fastapi-sqlite/` was the final outstanding Tier 2 item), the Owner requested `project-mobile_v01.md` specifically be generated next, ahead of the remainder of v10.1 scope.
**Options Considered:** (a) Leave all of v10.1 deferred until a future, separately-scoped v10.1 planning cycle begins, generating its items together in whatever order that cycle sets. (b) Pull `project-mobile_v01.md` forward in isolation now, ahead of `AI_DEVELOPMENT_MANUAL.md`, the remaining Layer 9 template set, and the remaining Layer 8 tool coverage, all of which remain deferred and untouched by this decision.
**Decision:** `project-mobile_v01.md` is approved for generation now, pulled forward from v10.1 scope ahead of every other Tier 3 item, which remains deferred.
**Rationale:** Explicit Owner request, constituting the Gate 1 (Plan Approval) event `FRAMEWORK_BLUEPRINT.md`, Section 18.4, requires before any v10.1 scope may be pulled forward ahead of the rest of that scope. The pull-forward is deliberately scoped to this one document; it does not reopen or advance any other Tier 3 item, consistent with the Constitution's priority of maintainability over premature scope expansion (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 4) and with `FRAMEWORK_BLUEPRINT.md`, Section 18.4's own concern for preventing the scope-creep pattern that produced the incomplete v09-to-v10 migration this framework's v10 generation itself resolved.
**Supersedes / Superseded By:** None.

---

## 3.2 Note on This Revision — `DEC-010` and `DEC-011` Appended

This revision appends the two entries above, both made in the same working session and both satisfying the "genuine architectural decision" threshold Section 2.3's corollary requires. Three further status changes occurred as **direct, mechanical consequences** of `DEC-010` and are recorded here narratively, exactly as Section 3.1 recorded the equivalent Layer 7/Layer 8 transitions previously — each is a status observation determined by an already-frozen completeness or lifecycle criterion, not an additional decision, and none of the three receives its own Section 3 entry:

- **`template-fastapi-sqlite/` was generated and validated** against all five `TEMPLATE_SPEC.md` categories, conforming to `project-personal-full-stack_v01.md` per `DEC-010`.
- **Layer 9 (Project Templates) reached `Active` as a whole**, per the pre-existing criterion at `FRAMEWORK_BLUEPRINT.md`, Section 12.3, and correction C-05 (`DEC-009`) — the specification (`TEMPLATE_SPEC.md`) now has its required conforming concrete instance.
- **`V10_MIGRATION_NOTES.md` (Layer 11) transitioned from `Active` to `Deprecated`**, per its own pre-scheduled DL-003 condition (`DEC-002`): "remains `Active` until every Tier 1 and Tier 2 v10 document exists, at which point it transitions to `Deprecated`." With `template-fastapi-sqlite/`'s completion, every Tier 1 and Tier 2 v10 document now exists, satisfying that condition.

Taken together, these three consequences mean **Framework v10, Tier 1 and Tier 2 combined, is now fully complete** — every governing layer (1–6) is `Active`, and every v10-scoped operational layer (7, 8, 9, 10) is `Active` as a whole.

This same window's second decision, `DEC-011`, is a Gate 1 (Plan Approval) event rather than a Gate 2 (Architecture Approval) event — it reorders which already-authorized future document is generated next; it does not amend any governing-layer rule. It is recorded here because `FRAMEWORK_BLUEPRINT.md`, Section 18.4, explicitly requires any v10.1 pull-forward to be recorded in this log, using the same append-only entry format as any other decision.

This document's Section 4.3 previously described the `DEC-010` question as still open, pending explicit Owner confirmation, and stated that once decided it would "constitute this log's next legitimate entry, `DEC-010`." That forward reference is now fulfilled; Section 4.3 below has been updated to reflect the resolution, consistent with that section's own stated purpose as a pointer to a *pending* decision rather than a historical entry in its own right (and therefore not itself subject to the Section 2.1 no-silent-edit constraint, which binds Section 3's dated entries specifically).

---

## 4. Entry Index and Known Gaps

### 4.1 Decision ID → Entry Lookup

| Decision ID(s) | Entry |
|---|---|
| FD-001, FD-002, FD-003 | DEC-001 |
| KA-001–004, DL-001–003 | DEC-002 |
| DI-001, DI-002, CC-001, CC-002 | DEC-003 |
| PR-001, PR-002, EP-001, EP-002 | DEC-004 |
| AA-001–004 | DEC-005 |
| SK-001–004 | DEC-006 |
| HE-001–004 | DEC-007 |
| TD-002, TD-006, TD-007 | DEC-008 |
| C-01–C-10 | DEC-009 |
| DEC-010 | DEC-010 |
| DEC-011 | DEC-011 |

### 4.2 Known Gap — Identifiers Not Yet Transcribed

`TD-001`, `TD-003`, `TD-004`, `TD-005`, and the `LP-` series are named in `FRAMEWORK_BLUEPRINT.md`'s traceability header as valid decision-ID families but are not described in sufficient detail in any document available to this generator, at seeding time or during any subsequent revision, to populate an honest Context/Options/Rationale entry. Rather than fabricate that detail, this document records the gap explicitly. **`BLUEPRINT_INPUT_FREEZE.md` remains authoritative for these identifiers** until a future session, with direct access to that document, transcribes them here as additional seed entries. This gap **MUST NOT** be treated as license to ignore those identifiers elsewhere in the framework — only as an accurate statement of what this log currently does and does not reproduce.

### 4.3 Previously Open Question — Resolved (see `DEC-010`)

This section previously tracked, as a standing open question, the choice of which `Active` Layer 4 archetype document (`project-personal-full-stack_v01.md` or `project-monolithic_v04.md`) `template-fastapi-sqlite/` should conform to (`FRAMEWORK_STATUS.md`, Flag 10). **That question is now resolved: the Owner confirmed `project-personal-full-stack_v01.md` (Full-Stack), recorded above as `DEC-010`.** `template-fastapi-sqlite/` has since been generated and validated against it, and Layer 9 has reached `Active` as a whole as a direct consequence (Section 3.2, above).

No further question is currently open in this log awaiting a future entry. The framework's current, narrowly-scoped work item — generating `project-mobile_v01.md` — is not an open *question* in the sense this section previously tracked; the decision authorizing it has already been made and is recorded above as `DEC-011`.

---

## 5. Change Control

1. This document **MUST NOT** be edited silently. Its Sections 0–2 (operating rules) follow the same amendment procedure as any other governing content: proposal → Gate 2 (Architecture Approval) → amendment → a new entry in Section 3 recording the amendment itself → `FRAMEWORK_HANDOVER.md` update in the same commit (`FRAMEWORK_BLUEPRINT.md`, Section 18.2).
2. Section 3 is append-only per Section 2.1 above. Correcting the Section 4.2 gap by transcribing `TD-001/003/004/005` or the `LP-` series is an **addition** of new seed entries, not an edit of existing ones, and is therefore permitted without a Gate 2 event — it is a transcription of an already-frozen decision, not a new decision.
3. A regeneration of this document that adds no new Section 3 entry — because no qualifying architectural decision occurred in the interval (Section 2.3's corollary) — **MAY** be performed as a verification pass, consistent with the precedent recorded in Section 3.1. Such a regeneration **MUST NOT** alter any existing entry's content and **SHOULD** record, in a dated note comparable to Section 3.1 or 3.2, which documents were reviewed and why none qualified (or, where entries were appended, what was appended and why).
4. Section 4.3 ("Previously Open Question") and equivalent forward-pointing status sections **MAY** be updated in place to reflect resolution of the question they track, without a Gate 2 event, since they are pointers to pending decisions rather than dated historical entries themselves — the underlying Section 3 entry they point to remains subject to the full Section 2.1 no-silent-edit constraint regardless.
5. Upon any status change to this document, `FRAMEWORK_README.md` Sections 4–6 and `FRAMEWORK_STATUS.md` **MUST** be reviewed for currency in the same change, per `FRAMEWORK_README.md` Section 9 and the AI Session Instructions in `FRAMEWORK_STATUS.md`.

---

## Closing Statement

This document is the `Active` Layer 10 Knowledge Base artifact of Framework v10. It transcribes, retrospectively, the 33 individually-evidenced decision identifiers and all 10 corrections from `BLUEPRINT_INPUT_FREEZE.md` into the append-only format this layer requires (`DEC-001` through `DEC-009`), while explicitly and honestly flagging the identifiers it does not yet reproduce (Section 4.2). In this revision, it appends the framework's next two genuine architectural decisions since that seeding: `DEC-010`, confirming `project-personal-full-stack_v01.md` (Full-Stack) as the archetype `template-fastapi-sqlite/` targets, and `DEC-011`, the Owner's Gate 1 approval pulling `project-mobile_v01.md` forward from v10.1 scope. `DEC-010`'s direct, mechanical consequences — `template-fastapi-sqlite/`'s generation, Layer 9 reaching `Active` as a whole, and `V10_MIGRATION_NOTES.md`'s DL-003-triggered deprecation — are recorded narratively in Section 3.2 rather than as separate entries, consistent with the same non-decision, criterion-triggered treatment this log has already applied to Layer 7's and Layer 8's earlier completions. Section 4.3, which had tracked the `DEC-010` question as open since it was first surfaced, is updated to record its resolution. From this point forward, every commit that introduces or changes an architectural decision **MUST** continue to append a new entry here, in the same commit, per PR-001 (`DEC-004`) — the next such entry now being whatever architectural question first arises during `project-mobile_v01.md`'s own generation, should one surface.
