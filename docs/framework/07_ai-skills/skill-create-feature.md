# skill-create-feature.md

**Status:** Active
**Layer:** 7 — AI Skills
**Tier:** 1 (Critical)
**Skill Name:** `create-feature`
**Purpose:** Define the full, seven-field specification (SK-002; `FRAMEWORK_BLUEPRINT.md`, Section 10.3, correction C-08) of the `create-feature` Skill — the entry-point Skill of the Development Workflow Phase (`FRAMEWORK_BLUEPRINT.md`, Section 8), responsible for implementing a scoped feature's business/domain logic and, where applicable, its presentation layer, within a single existing project, in a manner that is directly executable by an AI agent or a human developer without requiring further clarification beyond what this document and the layers it inherits from already state.
**Primary Authority (highest priority, in order):** `FRAMEWORK_BLUEPRINT.md` → `SKILLS.md` → `global_rules_revisionfinal_v10.md`. This document introduces no architectural decision of its own and MUST NOT be read as amending any of the three. Where any statement below appears to conflict with one of them, the higher-priority document wins and this document MUST be corrected (Section 16).
**Reference materials (informational inspiration only, no authority):** `README_andrej-karpathy-skills.md`, `README_Claude Forge.md`, `README_agentmemory.md`. Consulted solely for general patterns in Skill organization, workflow structure, metadata framing, and engineering execution discipline (Section 2 below). No implementation-specific mechanism, vendor tooling, or runtime behavior from any of the three is adopted anywhere in this document. Where a reference pattern and the Primary Authority would ever conflict, the Primary Authority prevails without exception.
**Inherits from:** `global_rules_revisionfinal_v10.md` (Layer 2 — the rules this Skill's execution must follow, cited by reference and not restated), `global_technology_stack_v10.md` (Layer 3 — the technologies this Skill's `steps` may introduce), and, at the point this Skill actually executes, whichever Layer 4 Project Rule document matches the current project's archetype (e.g., `project-pc-app_v04.md` for a Desktop/Electron project) — per `FRAMEWORK_BLUEPRINT.md`, Section 2.7, "Inputs."
**Governs:** Any Layer 8 Prompt Library entry that invokes `create-feature` (which MUST reference this document rather than duplicate its logic, per `FRAMEWORK_BLUEPRINT.md` Section 11.2), and any Layer 9 Template that declares `create-feature` as a mandatory Skill reference (per `FRAMEWORK_BLUEPRINT.md` Section 2.9), per the downward authority rule (KA-002, `FRAMEWORK_BLUEPRINT.md` Section 6).
**Supersedes:** None. This is the first version of this document. Its manifest entry in `SKILLS.md`, Section 12, previously recorded a **Document Status** of `Pending — not yet generated` with a provisional `gate` value; this document's creation resolves that entry (Section 16 below).
**Read order:** Read only after an AI agent has (a) completed the AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7), (b) read the Layer 4 Project Rule document matching the current project's archetype, and (c) read `SKILLS.md` and selected `create-feature` via its two-part discovery filter — role match, then Workflow Phase fit (`SKILLS.md`, Section 6). This document MUST NOT be read, or its `steps` executed, as a substitute for any of those three prerequisites.
**RFC 2119:** The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in RFC 2119. They apply to every rule below without exception unless the rule itself states otherwise.

---

## 0. Scope and Position in the Knowledge Architecture

```
Layer 1 — Constitution                         AI_DEVELOPMENT_PHILOSOPHY_v2.0.md
        ↓
Layer 2 — Framework Rules                       global_rules_revisionfinal_v10.md
        ↓
Layer 3 — Technology Standards                  global_technology_stack_v10.md
        ↓
Layer 4 — Project Rules                         project-pc-app_v04.md (Active)
                                                 project-personal-full-stack_v01.md (pending)
                                                 project-monolithic_v04.md (pending)
                                                 project-mobile_v01.md (pending, v10.1)
        ↓ (inherited by reference, never restated)
Layer 5 — Developer Manuals
Layer 6 — Reference Implementations
        ↓
Layer 7 — AI Skills        SKILLS.md (Manifest)
                            skill-create-feature.md  ← this document
                            skill-generate-tests.md  (pending)
                            skill-review-code.md     (pending)
        ↓
Layer 8 — Prompt Library
Layer 9 — Project Templates
```

**What this document contains.** The complete seven-field specification of `create-feature` (Section 3), plus the surrounding context — purpose, scope, preconditions, detailed input/output description, the full workflow, completion criteria, failure conditions, and cross-references — needed for an AI agent to execute this Skill without requiring further clarification, per `SKILLS.md` Section 3's distinction between a manifest entry (a pointer) and a full Skill document (the executable specification).

**What this document does not contain, by design.** Per the Layer 7 prohibited responsibilities (`FRAMEWORK_BLUEPRINT.md`, Section 2.7) and the Skill scope boundary (SK-003, `FRAMEWORK_BLUEPRINT.md` Section 10.4):

- It **MUST NOT** modify, restate as if authoritative, or contradict any Layer 1–4 document. Every rule this Skill's execution must follow is cited by reference to its governing layer, never reproduced as if invented here.
- It **MUST NOT** define a new HITL Gate, a new agent role, a new Skill Metadata field, or any schema element beyond the seven fields fixed by `FRAMEWORK_BLUEPRINT.md`, Section 10.3.
- It **MUST NOT** perform, or describe this Skill as capable of, any cross-project operation.
- It **MUST NOT** restate the canonical technology table (Layer 3) or the archetype's directory layout and naming conventions (Layer 4) — it applies them, by reference, at the point they become relevant to a step.

---

## 1. Relationship to the Skill Manifest (`SKILLS.md`)

This document is the full Skill document that `SKILLS.md`, Section 12, points to under the **Skill Document** column for the `create-feature` row. Per `SKILLS.md` Section 3, the manifest entry is a pointer, not a substitute — an agent that has only read the manifest's one-line description MUST open this document before executing any `steps` below.

`SKILLS.md`, Section 12, Footnote 1, provisionally assigned `create-feature` a `gate` value of `none`, pending this document's authoritative declaration. Section 3 below makes that declaration formally and confirms the provisional value: `create-feature`'s own output does not, by itself, require a human approval pause, because — per the reference Skill Composition documented in `SKILLS.md`, Section 8 — it is the *combined* output of the Development Workflow Phase (`create-feature` → `generate-tests` → `review-code`) that Gate 4 (Implementation Approval) reviews, not `create-feature`'s output in isolation. This document's creation therefore reconciles, rather than overrides, the manifest's provisional footnote, per `SKILLS.md` Section 18.2.

---

## 2. Cross-Project Design Inspiration (Informational Only)

Per the Owner's instruction, general patterns from three external, non-framework projects are reflected in this document's *organization* and *workflow discipline* — never in its architecture, mechanisms, or runtime behavior. Each pattern below is named, its general source noted, and mapped to the specific Blueprint- or `global_rules`-defined mechanism that already governs it in Framework v10.

| Requested pattern | Illustrated (informationally) by | Realized in this Skill exclusively through |
|---|---|---|
| **Explicit reasoning before implementation; no silent assumptions** | The "Think Before Coding" principle (`README_andrej-karpathy-skills.md`) — state assumptions, present interpretations, ask rather than guess | The Requirement Analysis step (Section 9.1) and the ambiguity-escalation rule in Section 11 |
| **Verifiable, goal-driven execution** | The "Goal-Driven Execution" principle (`README_andrej-karpathy-skills.md`) — define success criteria, loop until verified | The Completion Criteria mapping to the Layer 2 Definition of Done (Section 10) and the Self Review / Gate Check steps (Sections 9.5, 9.7) |
| **Minimum, surgical changes; no drive-by refactoring** | The "Simplicity First" and "Surgical Changes" principles (`README_andrej-karpathy-skills.md`) | The Implementation step's explicit boundary (Section 9.4), already grounded in Layer 2's Single Responsibility Principle (`global_rules_revisionfinal_v10.md`, Section 3.1.2) and Simplicity value (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 8) |
| **A staged pipeline of named phases feeding forward, ending in a review/verification checkpoint** | The `/plan → /tdd → /code-review → /handoff-verify → /commit-push-pr` pipeline shape (`README_Claude Forge.md`) | The Workflow Phase concept already fixed by `FRAMEWORK_BLUEPRINT.md`, Section 8, and this Skill's own seven-stage `steps` sequence (Section 9), which feeds `generate-tests` and `review-code` exactly as documented in `SKILLS.md`, Section 8 |
| **Documentation and decision history as durable, not incidental, artifacts** | The session-continuity and durable-context framing (`README_agentmemory.md`) | The Layer 10 Knowledge Base — `DECISIONS.md` and `FRAMEWORK_HANDOVER.md` — which the Documentation Update step (Section 9.6) points to for any architectural decision this Skill's execution surfaces |

**Explicit non-adoptions.** No plugin/marketplace installation mechanism, no hook or lifecycle-event engine, no MCP server or tool-connector protocol, no persistent memory-server process, and no slash-command or CLI runtime from any of the three reference materials is a component of this Skill or of Framework v10. What is carried forward is the *pattern* named in the left-hand column above, mapped to a mechanism the Blueprint or `global_rules_revisionfinal_v10.md` already defines — nothing more. Adopting any concrete mechanism from a reference material would be an architectural decision requiring its own Gate 2 (Architecture Approval) proposal (`FRAMEWORK_BLUEPRINT.md`, Section 18.2), and is explicitly out of scope for this document.

---

## 3. The Seven Required Fields — Summary

Per SK-002 (`FRAMEWORK_BLUEPRINT.md`, Section 10.3, correction C-08), every Skill document MUST populate all seven of the following fields. This table is the authoritative, at-a-glance summary; each field is expanded in the sections that follow.

| Field | Value |
|---|---|
| `skill-name` | `create-feature` |
| `primary-agent-role` | Backend Agent **or** Frontend Agent, selected per invocation according to which side of the Clean Architecture boundary the feature's work sits on (Section 4 below) — the two-role framing itself is inherited verbatim from `FRAMEWORK_BLUEPRINT.md`, Section 10.5, and `SKILLS.md`, Section 12; this document does not introduce a third option. |
| `gate` | `none` (Section 1 above; confirmed, not provisional, as of this document) |
| `input` | Section 6 |
| `output` | Section 7 |
| `steps` | Section 9 |
| `framework-alignment` | Section 12 |

A Skill document with any of these seven fields unpopulated is not valid and MUST NOT be added to the manifest as `Active` (`FRAMEWORK_BLUEPRINT.md`, Section 10.3). All seven are fully populated in this document.

---

## 4. Purpose

`create-feature` implements a single, previously scoped feature's business/domain logic and, where the feature has a presentation surface, its presentation-layer wiring, entirely within one existing project. It is the entry point of the Development Workflow Phase (`FRAMEWORK_BLUEPRINT.md`, Section 8): the phase begins when Gate 3 (Scope Approval) has produced a scoped feature definition, and this Skill is the first unit of AI-executable work that acts on that definition.

`create-feature`'s `primary-agent-role` is context-dependent between **Backend Agent** and **Frontend Agent** (`FRAMEWORK_BLUEPRINT.md`, Section 10.5): a given invocation of this Skill MUST resolve to exactly one of the two roles, determined by which side of the Clean Architecture boundary (`global_rules_revisionfinal_v10.md`, Section 3.2) the feature's work primarily sits on — domain/application-layer work resolves to Backend Agent; presentation-layer work resolves to Frontend Agent. A feature that spans both is executed as two `create-feature` invocations, one per role, rather than as a single invocation with an ambiguous role.

---

## 5. Scope

### 5.1 In Scope

1. Implementing business/domain logic for a single, already-scoped feature within a single project (SK-003).
2. Implementing application/use-case orchestration that coordinates the new domain logic through existing or newly declared ports (`global_rules_revisionfinal_v10.md`, Section 2).
3. Implementing presentation-layer wiring (UI components, view state, delivery-layer glue) strictly to the extent the scoped feature requires it, when this Skill is invoked under the Frontend Agent role.
4. Declaring any new port/interface required by the vendor-independence rule (`global_rules_revisionfinal_v10.md`, Section 2.2) as part of the same change.
5. Updating documentation made inaccurate by the change, in the same change (`global_rules_revisionfinal_v10.md`, Section 8.5).

### 5.2 Out of Scope

1. Generating the feature's test suite. Test generation for coverage beyond what test-first development strictly requires during Implementation (Section 9.4) belongs to `generate-tests` (`SKILLS.md`, Section 7).
2. Performing the formal code-quality and rule-conformance review that gates merge. That belongs to `review-code` (`SKILLS.md`, Section 12; `global_rules_revisionfinal_v10.md`, Section 7.4).
3. Defining the feature's scope itself. Scope definition is the output of Gate 3 (Scope Approval) and precedes this Skill; `create-feature` MUST NOT redefine or expand scope on its own authority (Section 11).
4. Any cross-project operation, or any modification to a Layer 1–4 document (SK-003, `FRAMEWORK_BLUEPRINT.md` Section 10.4).
5. Selecting the project's archetype, technology stack, or Layer 4 structural conventions. Those are already fixed by the time this Skill executes (Section 6).

---

## 6. Preconditions

`create-feature` MUST NOT begin execution until all of the following are satisfied:

1. **Session initialization is complete.** The AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7) has been completed at least once in the current session.
2. **The project is bootstrapped.** Per `PROJECT_BOOTSTRAP_GUIDE.md`, Section 3, the project has passed Gate 1 (Plan Approval) and its scaffold has been validated against the applicable Layer 9 specification or `PROJECT_STRUCTURE.md`.
3. **A scoped feature definition exists.** Gate 3 (Scope Approval) has produced a feature scope definition (`FRAMEWORK_BLUEPRINT.md`, Section 9.2) that this invocation of `create-feature` is executing against.
4. **The matching Layer 4 Project Rule document is `Active`.** The document corresponding to the project's archetype (e.g., `project-pc-app_v04.md`) has been read in full for this task.
5. **`SKILLS.md` selection has occurred.** This Skill was selected through the two-part discovery filter — `primary-agent-role` match and current Workflow Phase fit (`SKILLS.md`, Section 6) — not by name-matching alone.

If any precondition above is not satisfied, `create-feature` MUST NOT execute. The gap MUST be reported per Section 11 (Failure Conditions) rather than worked around.

---

## 7. Input

Per `SKILLS.md`, Section 7, `create-feature`'s declared input has no natural predecessor Skill within Layer 7 — it is the entry point of the Development Workflow Phase, and its input originates from a HITL Gate, not from another Skill's output. The full input set is:

1. **The scoped feature definition** produced at Gate 3 (Scope Approval) — a statement of what the feature must do, its boundaries, and any constraints the human (Engineering CEO) attached at approval time.
2. **The matching Layer 4 Project Rule document** for the current project's archetype, supplying the directory layout, naming conventions, and archetype-specific technical decisions this Skill's output must conform to.
3. **The current state of the project's codebase** relevant to the feature — the existing domain, application, and (where applicable) presentation code the new feature must integrate with, without unnecessary disruption (Section 9.4).
4. **Any prior `DECISIONS.md` entries** relevant to the feature's domain, so that this invocation does not silently re-litigate or contradict a settled architectural decision (`FRAMEWORK_BLUEPRINT.md`, Section 2.10, AI interaction model).

---

## 8. Output

Per `SKILLS.md`, Section 7, `create-feature`'s declared output is the natural input to `generate-tests` (business/application logic requiring test coverage) and, jointly with `generate-tests`'s output, to `review-code`. The full output set is:

1. **Working business/domain logic** implementing the scoped feature, conforming to the Clean Architecture boundary (`global_rules_revisionfinal_v10.md`, Section 3.2) and the matching Layer 4 directory layout.
2. **Application/use-case orchestration** wiring the new domain logic to existing or newly declared ports.
3. **Presentation-layer wiring**, where the feature has one and this invocation runs under the Frontend Agent role.
4. **Any newly declared port/interface** required by the vendor-independence rule, together with its concrete adapter placed in the archetype's infrastructure boundary.
5. **Updated documentation** reflecting the change (Section 9.6).
6. **A flagged architectural-decision candidate**, where applicable (Section 9.6), for a human or a subsequent process step to record in `DECISIONS.md` — `create-feature` itself MUST NOT write to `DECISIONS.md`, since that is a Layer 10 artifact outside this Skill's single-project execution scope in the sense that recording it is a framework-level bookkeeping act, not a feature-implementation act; flagging it clearly is sufficient to satisfy PR-001 downstream.

`create-feature`'s output is explicitly **not** considered ready for Gate 4 review on its own. Per `SKILLS.md`, Section 8, it is one of two inputs `review-code` consumes, the other being `generate-tests`'s output.

---

## 9. Workflow

`create-feature`'s `steps` field consists of seven ordered stages. Per `FRAMEWORK_BLUEPRINT.md`, Section 10.3, this Skill's `steps` describe only its own execution — they reference `generate-tests`'s and `review-code`'s outputs and inputs where relevant, but MUST NOT describe those Skills' internal procedures.

### 9.1 Stage 1 — Requirement Analysis

1. The agent MUST review the scoped feature definition (Section 7, item 1) in full and MUST identify which Layer 2 TDD Boundary zone(s) (`global_rules_revisionfinal_v10.md`, Section 4 — business/domain, application/use-case, infrastructure, or UI/framework-glue) the feature spans.
2. The agent MUST identify which Layer 4 directory locations (per the matching Project Rule document) the feature will touch.
3. The agent MUST NOT silently resolve an ambiguity in the scope definition by picking one plausible interpretation and proceeding. Where the scope definition is ambiguous or underspecified in a way that would materially change the implementation, the agent MUST surface the ambiguity and the candidate interpretations rather than guess (Section 11).
4. The agent MUST confirm that no part of the feature, as scoped, would require modifying a Layer 1–4 document or performing a cross-project operation (Section 5.2, item 4). If it would, execution MUST stop and the gap MUST be escalated per Section 11 rather than performed.

### 9.2 Stage 2 — Specification

1. The agent MUST produce an explicit, written specification of the feature's business rules, inputs, outputs, and edge cases before any implementation planning begins.
2. The specification MUST be expressed in technology-agnostic domain language consistent with the Layer 2 Architecture Rules (`global_rules_revisionfinal_v10.md`, Section 3), not in terms of a specific library or framework call.
3. The specification MUST NOT introduce an architectural pattern, abstraction, or extension point beyond what Layers 1–4 already authorize for this project. New abstractions MUST solve a real, currently existing problem, per `global_rules_revisionfinal_v10.md`, Section 3.2.3 and Section 3.3 — a new interface, factory, or indirection layer introduced merely for symmetry is out of scope for this stage.
4. Where the specification, once written, appears to require an architectural decision Layers 1–4 do not already resolve, the agent MUST escalate per Section 11 rather than resolve it unilaterally within the specification.

### 9.3 Stage 3 — Implementation Planning

1. The agent MUST map the specification onto the current archetype's directory layout and naming conventions, as defined by the matching Layer 4 document.
2. The agent MUST identify, before writing any implementation code, every new port/interface the vendor-independence rule requires (`global_rules_revisionfinal_v10.md`, Section 2.2 and 2.5) — a new external dependency MUST be planned to sit behind an interface from the outset, not as a follow-up task.
3. The agent MUST state which units of the plan fall into the "MUST be developed test-first" zone of the Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4) and in what order they will be built, since this determines the handoff sequencing with `generate-tests`.
4. The agent SHOULD state the plan as a short, ordered list of steps, each paired with a concrete verification check (e.g., "implement `X` → verify: unit test `Y` passes"), so that the plan itself is checkable rather than only aspirational.

### 9.4 Stage 4 — Implementation

1. The agent MUST implement domain/business logic test-first wherever the TDD Boundary requires it (`global_rules_revisionfinal_v10.md`, Section 4, Zone 1).
2. The agent MUST respect the Clean Architecture boundary throughout: domain code MUST NOT import infrastructure code, and dependencies MUST point from concrete/infrastructure code toward abstract/domain code (`global_rules_revisionfinal_v10.md`, Sections 3.1.3, 3.2.2).
3. The agent MUST NOT introduce a new external vendor dependency without placing it behind an interface owned by the application, in the same change (`global_rules_revisionfinal_v10.md`, Section 2.5).
4. The agent MUST follow the Exception Handling Rule (`global_rules_revisionfinal_v10.md`, Section 11) and the Code Quality Gates (`global_rules_revisionfinal_v10.md`, Section 7) throughout implementation, not only as a final check.
5. The agent MUST confine changes to what the scoped feature requires. Adjacent code, comments, or formatting that are not part of the feature's implementation MUST NOT be modified as a side effect of this Skill's execution; where an unrelated defect is noticed, it MUST be reported rather than silently fixed in the same change. This is a direct application of the Single Responsibility Principle (`global_rules_revisionfinal_v10.md`, Section 3.1.2) and the Constitution's Simplicity value (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 8), not a new rule.
6. Where the agent's own changes make an import, variable, or function unused, the agent MUST remove it. The agent MUST NOT remove pre-existing dead code unrelated to this change (that is the concern of a future, separate refactoring Skill, per `FRAMEWORK_BLUEPRINT.md`, Section 10.5's roadmap).

### 9.5 Stage 5 — Self Review

1. Before declaring its output complete, the agent MUST self-check the implementation against every applicable item of the Layer 2 Definition of Done (`global_rules_revisionfinal_v10.md`, Section 10).
2. The agent MUST verify that no generic or broad exception handling was introduced without explicit justification (`global_rules_revisionfinal_v10.md`, Section 11.1).
3. The agent MUST verify that no dead code, no secret or credential, and no unjustified new dependency has been introduced (`global_rules_revisionfinal_v10.md`, Sections 7.3, 9.1, 6.4).
4. This Self Review is an internal check performed by this Skill and MUST NOT be represented as, or substituted for, the formal review performed by `review-code` at Gate 4. `create-feature`'s `gate` field remains `none` (Section 3); Self Review does not itself pause for human approval.

### 9.6 Stage 6 — Documentation Update

1. The agent MUST update any documentation made inaccurate by the change, in the same change (`global_rules_revisionfinal_v10.md`, Section 8.5). Documentation debt MUST NOT be deferred as a separate, unscheduled task.
2. The agent MUST add or update contract documentation (inputs, outputs, side effects) for any new or changed public interface (`global_rules_revisionfinal_v10.md`, Section 8.4).
3. If, in the course of Stages 1–5, the agent identified a point where an architectural decision was made or newly required (e.g., a new port introduced, a Layer 3 default applied to a case it did not previously cover), the agent MUST flag this explicitly as a `DECISIONS.md` candidate in its output. Per SK-003, `create-feature` MUST NOT modify `DECISIONS.md` or any Layer 1–4 document itself; recording the entry is a downstream, human-or-process action this Skill's output must make easy to perform correctly, per PR-001.

### 9.7 Stage 7 — Gate Check

1. The agent MUST perform a final self-verification that the combined output of Stages 1–6 is complete, internally consistent, and ready to be handed to `generate-tests` as its declared input (Section 8), consistent with the reference Skill Composition in `SKILLS.md`, Section 8.
2. This "Gate Check" step is an internal readiness check, not the Skill-level `gate` metadata field. Because `create-feature`'s `gate` field is `none` (Section 3), completing this step does **not** itself trigger a human approval pause. The actual HITL checkpoint for this Workflow Phase occurs later, at Gate 4 (Implementation Approval), against the *combined* output of `create-feature` → `generate-tests` → `review-code`, per `SKILLS.md`, Section 8 and Footnote 2. This distinction MUST be preserved by any agent executing this Skill and MUST NOT be described, reported, or logged as if Gate 4 had already occurred.
3. If Stage 5 (Self Review) or this stage surfaces a Definition-of-Done failure, the agent MUST return to the relevant earlier stage (most commonly Stage 4, Implementation, or Stage 2, Specification, if the failure traces to a specification gap) rather than presenting incomplete or non-conforming output as finished.
4. Only once this stage passes without an open failure MUST the agent present its output as the completed result of `create-feature` and proceed to hand off to `generate-tests`.

---

## 10. Completion Criteria

`create-feature`'s output is complete if and only if every item below holds. This list operationalizes the Layer 2 Definition of Done (`global_rules_revisionfinal_v10.md`, Section 10) for this Skill specifically; it does not replace or narrow it.

| # | Criterion |
|---|---|
| 1 | The implementation addresses exactly the feature scoped at Gate 3 — no more, no less (Sections 5.2, 9.1). |
| 2 | Business/domain logic touched has test coverage consistent with the TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4); units requiring test-first development were built test-first (Section 9.4). |
| 3 | The Clean Architecture boundary is intact: no domain-layer import of infrastructure code (Section 9.4.2). |
| 4 | Every new external dependency sits behind an owned interface (Section 9.3.2, 9.4.3). |
| 5 | No generic/broad exception handling was introduced without justification (Section 9.5.2). |
| 6 | No dead code, secret, or credential was introduced (Section 9.5.3). |
| 7 | Documentation affected by the change was updated in the same change (Section 9.6.1–9.6.2). |
| 8 | Any architectural-decision candidate surfaced during execution is explicitly flagged in the output, not silently absorbed (Section 9.6.3). |
| 9 | Stage 7 (Gate Check) passed without an unresolved Definition-of-Done failure (Section 9.7). |
| 10 | The output is structured such that `generate-tests` can consume it directly as declared input, without requiring the human or the next agent to reconstruct missing context (Section 8). |

An agent executing `create-feature` MUST self-check its output against this table before presenting it as complete, consistent with the equivalent self-check requirement already stated at Layer 2 (`global_rules_revisionfinal_v10.md`, Section 10, closing paragraph).

---

## 11. Failure Conditions and Escalation

`create-feature` MUST NOT attempt to silently work around any of the following. Each is a stop condition requiring the agent to report the gap and halt this Skill's execution rather than proceed on an assumption:

1. **A precondition in Section 6 is not satisfied** (e.g., the project is not yet bootstrapped, or no scoped feature definition exists). The agent MUST report which precondition is unmet and MUST NOT begin Stage 1.
2. **The scope definition is ambiguous in a materially implementation-affecting way** (Section 9.1.3). The agent MUST present the ambiguity and candidate interpretations rather than silently choose one.
3. **The feature, as scoped or as specified, appears to require modifying a Layer 1–4 document, or performing a cross-project operation.** Per SK-003, this is not a valid `create-feature` execution. It MUST be escalated as a Gate 2 (Architecture Approval) proposal and, if approved, recorded in `DECISIONS.md` — never encoded as an ad hoc extension of this Skill's own scope.
4. **The matching Layer 4 Project Rule document is not `Active`** (e.g., the project's archetype is Full-Stack or Monolithic, whose Layer 4 documents are `Pending` per `FRAMEWORK_README.md`, Section 4). The agent MUST report this as a standing Tier gap and MUST NOT improvise directory structure or naming conventions from a deprecated legacy document.
5. **A required external dependency cannot be placed behind an interface within the same change** (e.g., because doing so would itself require an architectural decision Layer 3 has not resolved). The agent MUST escalate rather than introduce the dependency directly into business logic.
6. **Stage 5 (Self Review) or Stage 7 (Gate Check) surfaces a Definition-of-Done failure that cannot be resolved by returning to an earlier stage within this same invocation** (e.g., the specification itself was incomplete in a way only discovered during implementation). The agent MUST report the specific failing criterion from Section 10 rather than present non-conforming output as complete.
7. **The specification (Stage 2) would require a new architectural pattern not already authorized by Layers 1–4.** The agent MUST escalate per Section 9.2.4 rather than adopt the pattern unilaterally.

In every case above, escalation means: the agent reports the gap plainly, states which layer or gate the resolution belongs to, and stops — consistent with the same escalation discipline `PROJECT_BOOTSTRAP_GUIDE.md` (Section 5) and `SKILLS.md` (Section 6) already apply elsewhere in this framework.

---

## 12. `framework-alignment` (SK-002, Required Field)

This Skill's execution is bound to the following governing-layer rules, cited by reference and not restated:

| Layer | Document | Sections this Skill's execution is bound by |
|---|---|---|
| 1 | `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | Core value ordering (Section 4); Simplicity (Section 8); AI Readability (Section 16) |
| 2 | `global_rules_revisionfinal_v10.md` | Vendor Independence (Section 2); Architecture Rules (Section 3); TDD Boundary (Section 4); Code Quality Gates (Section 7); Documentation Standards (Section 8); Security Baselines (Section 9); Definition of Done (Section 10); Exception Handling Rule (Section 11) |
| 3 | `global_technology_stack_v10.md` | The canonical technology table, applied through whichever Layer 4 document is in effect — this Skill introduces no technology selection of its own |
| 4 | The matching Project Rule document for the current archetype (e.g., `project-pc-app_v04.md`) | Directory layout, naming conventions, and archetype-specific technical decisions (e.g., the IPC error-handling pattern in `project-pc-app_v04.md`, Section 6, where the archetype is Desktop) |

This Skill's own `steps` (Section 9) MUST NOT be read as adding to, narrowing, or reinterpreting any rule in the table above. Where a step above appears to state a rule not already present in one of these documents, that step is in error and MUST be corrected to a reference (per the framework's inheritance model, `FRAMEWORK_BLUEPRINT.md`, Section 5).

---

## 13. Related Skills

Per `SKILLS.md`, Sections 7 and 8:

| Skill | Relationship to `create-feature` |
|---|---|
| `generate-tests` (Pending — not yet generated) | Natural successor. Consumes `create-feature`'s output (business/application logic) as its declared input, positioned within the Layer 2 TDD Boundary. |
| `review-code` (Pending — not yet generated) | Downstream. Consumes the combined output of `create-feature` and `generate-tests`. Is the Skill whose output Gate 4 (Implementation Approval) reviews. |

`create-feature` has no predecessor Skill within Layer 7 (Section 7); its input originates from Gate 3 (Scope Approval), a human checkpoint, not from another Skill's output.

---

## 14. Related Documents

| Document | Relevance |
|---|---|
| `FRAMEWORK_BLUEPRINT.md` | Master structural authority for this Skill's classification, metadata schema, Workflow Phase concept, and HITL Gate positions (Sections 8–10). |
| `SKILLS.md` | The Layer 7 Manifest this document's entry belongs to; the source of the Skill Dependencies (Section 7) and Skill Composition (Section 8) this document's Sections 8 and 13 apply. |
| `global_rules_revisionfinal_v10.md` | Layer 2 source of every binding engineering rule this Skill's `steps` cite. |
| `global_technology_stack_v10.md` | Layer 3 source of the technology table this Skill applies through the Layer 4 document. |
| The matching Layer 4 Project Rule document (e.g., `project-pc-app_v04.md`) | Supplies the directory layout and naming conventions this Skill's output must conform to. |
| `PROJECT_BOOTSTRAP_GUIDE.md` | Governs the preconditions in Section 6 — a project must be bootstrapped before this Skill may execute. |
| `DECISIONS.md` | The Layer 10 destination for any architectural-decision candidate this Skill's execution flags (Section 9.6.3); this Skill does not write to it directly. |
| `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | Layer 1 source of the value ordering that resolves any ambiguity in how this Skill's rules should be read (Section 12). |

---

## 15. Authority and Conflict Resolution

1. Per the Primary Authority ordering stated in the header, and per the downward authority rule (KA-002), this document MUST NOT contradict `FRAMEWORK_BLUEPRINT.md`, `SKILLS.md`, or `global_rules_revisionfinal_v10.md`, in that order of precedence. Where this document appears to conflict with any of the three, the higher-priority document wins and this document MUST be corrected.
2. None of the reference materials named in the header (Section 2) carries any authority over this document. A pattern drawn from them MUST NOT be cited as justification for a rule that conflicts with any Primary Authority document.
3. Where a future `generate-tests` or `review-code` document, once authored, appears to require `create-feature`'s `steps` to change, that change MUST follow the amendment procedure in Section 16 rather than being made unilaterally in either document.
4. The full conflict-resolution procedure, including same-layer conflicts between two operational-layer artifacts, is defined in `FRAMEWORK_BLUEPRINT.md`, Section 6, and is not restated here.

---

## 16. Change Control

1. This document MUST NOT be edited silently to change what `create-feature` does. A change to its `input`, `output`, `steps`, `gate`, or `framework-alignment` fields is a change to a frozen Skill specification and MUST follow the amendment procedure defined in `FRAMEWORK_BLUEPRINT.md`, Section 18.2: proposal → Gate 2 (Architecture Approval) → amendment → `DECISIONS.md` entry → `FRAMEWORK_HANDOVER.md` update in the same commit.
2. Upon this document's creation, `SKILLS.md`, Section 12, MUST be updated in the same change: the **Document Status** for `create-feature` MUST move from `Pending — not yet generated` to `Active`, and Footnote 1's provisional `gate: none` reasoning MUST be marked as reconciled and authoritative per Section 3 of this document, per `SKILLS.md` Section 18.2.
3. `SKILLS.md`, Section 9 (Skill Lifecycle), MUST be updated in the same change to reflect that `create-feature` has entered the `Active` state, per the same section's stated diagram.
4. Layer 7 as a whole does not become `Active` on this document alone. Per `FRAMEWORK_BLUEPRINT.md`, Section 2.7, Layer 7 requires the Manifest *and* all three starter Skill documents. `skill-generate-tests.md` and `skill-review-code.md` remain outstanding.

---

## Closing Statement

This document is the full, seven-field specification of `create-feature`, the entry-point Skill of Framework v10's Development Workflow Phase. It resolves `SKILLS.md`'s provisional `gate: none` assignment into an authoritative declaration, defines a seven-stage workflow — Requirement Analysis, Specification, Implementation Planning, Implementation, Self Review, Documentation Update, and Gate Check — grounded entirely in rules already fixed by `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, `global_rules_revisionfinal_v10.md`, and the matching Layer 4 Project Rule document, and introduces no new Gate, role, schema field, or architectural pattern. Its final workflow stage, Gate Check, is explicitly distinguished from the Skill's own `gate: none` metadata field to avoid any reading of this document as introducing an undeclared human checkpoint. Per Section 16, Layer 7 remains incomplete until `skill-generate-tests.md` and `skill-review-code.md` are also generated.
