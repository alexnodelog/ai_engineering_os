# skill-review-code.md

**Status:** Active
**Layer:** 7 — AI Skills
**Tier:** 1 (Critical)
**Skill Name:** `review-code`
**Purpose:** Define the full, seven-field specification (SK-002; `FRAMEWORK_BLUEPRINT.md`, Section 10.3, correction C-08) of the `review-code` Skill — the terminal Skill of the Development Workflow Phase (`FRAMEWORK_BLUEPRINT.md`, Section 8), responsible for performing the AI-executed code-quality and rule-conformance review required before a non-trivial change may merge (`global_rules_revisionfinal_v10.md`, Section 7.4), in a manner that is directly executable by an AI agent without requiring further clarification beyond what this document and the layers it inherits from already state.
**Primary Authority (highest priority, in order):** `FRAMEWORK_BLUEPRINT.md` → `SKILLS.md` → `global_rules_revisionfinal_v10.md`. This document introduces no architectural decision of its own and MUST NOT be read as amending any of the three. Where any statement below appears to conflict with one of them, the higher-priority document wins and this document MUST be corrected (Section 17).
**Reference materials (informational inspiration only, no authority):** `README_andrej-karpathy-skills.md`, `README_Claude Forge.md`, `README_agentmemory.md`. Consulted solely for general patterns in review workflow shape, metadata framing, and reporting structure (Section 2 below). No implementation-specific mechanism, vendor tooling, or runtime behavior from any of the three is adopted anywhere in this document. Where a reference pattern and the Primary Authority would ever conflict, the Primary Authority prevails without exception.
**Inherits from:** `global_rules_revisionfinal_v10.md` (Layer 2 — the rules this Skill's review checks against, cited by reference and not restated), `global_technology_stack_v10.md` (Layer 3 — the technology conformance this Skill checks against), and, at the point this Skill actually executes, whichever Layer 4 Project Rule document matches the current project's archetype (e.g., `project-pc-app_v04.md` for a Desktop/Electron project) — per `FRAMEWORK_BLUEPRINT.md`, Section 2.7, "Inputs."
**Governs:** Any Layer 8 Prompt Library entry that invokes `review-code` (which MUST reference this document rather than duplicate its logic, per `FRAMEWORK_BLUEPRINT.md` Section 11.2), and any Layer 9 Template that declares `review-code` as a mandatory Skill reference (per `FRAMEWORK_BLUEPRINT.md` Section 2.9), per the downward authority rule (KA-002, `FRAMEWORK_BLUEPRINT.md` Section 6).
**Supersedes:** None. This is the first version of this document. Its manifest entry in `SKILLS.md`, Section 12, previously recorded a **Document Status** of `Pending — not yet generated`; this document's creation resolves that entry (Section 18 below).
**Read order:** Read only after an AI agent has (a) completed the AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7), (b) read the Layer 4 Project Rule document matching the current project's archetype, and (c) read `SKILLS.md` and selected `review-code` via its two-part discovery filter — role match, then Workflow Phase fit (`SKILLS.md`, Section 6). This document MUST NOT be read, or its `steps` executed, as a substitute for any of those three prerequisites.
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
                            skill-create-feature.md  (Active)
                            skill-generate-tests.md  (pending)
                            skill-review-code.md     ← this document
        ↓
Layer 8 — Prompt Library
Layer 9 — Project Templates
```

**What this document contains.** The complete seven-field specification of `review-code` (Section 3), plus the surrounding context — purpose, scope, preconditions, detailed input/output description, the full review workflow, a severity classification for findings, completion criteria, failure conditions, and cross-references — needed for an AI agent to execute this Skill without requiring further clarification, per `SKILLS.md` Section 3's distinction between a manifest entry (a pointer) and a full Skill document (the executable specification).

**What this document does not contain, by design.** Per the Layer 7 prohibited responsibilities (`FRAMEWORK_BLUEPRINT.md`, Section 2.7) and the Skill scope boundary (SK-003, `FRAMEWORK_BLUEPRINT.md` Section 10.4):

- It **MUST NOT** modify, restate as if authoritative, or contradict any Layer 1–4 document. Every rule this Skill's review checks against is cited by reference to its governing layer, never reproduced as if invented here.
- It **MUST NOT** define a new HITL Gate, a new agent role, a new Skill Metadata field, or any schema element beyond the seven fields fixed by `FRAMEWORK_BLUEPRINT.md`, Section 10.3. The Severity Classification introduced in Section 10 below is a reporting lens over existing, already-frozen rules — not a new Gate, field, or architectural mechanism (see the Scope note opening that section).
- It **MUST NOT** perform, or describe this Skill as capable of, any cross-project operation.
- It **MUST NOT** restate the canonical technology table (Layer 3) or the archetype's directory layout and naming conventions (Layer 4) — it applies them, by reference, at the point they become relevant to a review check.
- It **MUST NOT** implement, rewrite, or remediate the code under review. Remediation is the responsibility of `create-feature` and `generate-tests` upon a rejected Gate 4 review, not of this Skill (Section 5.2).

---

## 1. Relationship to the Skill Manifest (`SKILLS.md`)

This document is the full Skill document that `SKILLS.md`, Section 12, points to under the **Skill Document** column for the `review-code` row. Per `SKILLS.md` Section 3, the manifest entry is a pointer, not a substitute — an agent that has only read the manifest's one-line description MUST open this document before executing any `steps` below.

`SKILLS.md`, Section 12, Footnote 2, already assigned `review-code` a `gate` value of `Gate 4 — Implementation Approval` on direct textual grounds, citing `global_rules_revisionfinal_v10.md`, Section 7.4 ("human confirmation remaining authoritative at Gate 4") and `FRAMEWORK_BLUEPRINT.md`, Section 9.2 (Gate 4 "Reviews completed implementation," "Blocks: Merge"). Unlike the provisional `gate: none` values assigned to `create-feature` and `generate-tests` (`SKILLS.md`, Section 12, Footnote 1), this assignment was not provisional. Section 3 below restates it as this document's own authoritative declaration, confirming rather than reconciling the manifest's existing value, per `SKILLS.md` Section 18.2.

Per `SKILLS.md`, Section 7 (Skill Dependencies), `review-code`'s declared input is the combined output of `create-feature` and `generate-tests` — it is the last Skill in the Development Workflow Phase before Gate 4, and it has no successor Skill within Layer 7 (Section 14 below). Per `SKILLS.md`, Section 8 (Skill Composition), a rejected Gate 4 review returns control to `create-feature`, not to `review-code` itself, since remediation of the underlying implementation or tests is out of this Skill's scope (Section 5.2).

---

## 2. Cross-Project Design Inspiration (Informational Only)

Per the Owner's instruction, general patterns from three external, non-framework projects are reflected in this document's *review workflow shape*, *metadata framing*, and *reporting structure* — never in its architecture, mechanisms, or runtime behavior. Each pattern below is named, its general source noted, and mapped to the specific Blueprint- or `global_rules`-defined mechanism that already governs it in Framework v10.

| Requested pattern | Illustrated (informationally) by | Realized in this Skill exclusively through |
|---|---|---|
| **A dedicated review stage, distinct from implementation, that gates merge** | The `/code-review` step of the `/plan → /tdd → /code-review → /handoff-verify → /commit-push-pr` pipeline shape (`README_Claude Forge.md`) | The pre-existing Layer 2 requirement that "every non-trivial change MUST pass a code review step before merge" (`global_rules_revisionfinal_v10.md`, Section 7.4), and the Gate 4 position already fixed by `FRAMEWORK_BLUEPRINT.md`, Section 9.2 |
| **Findings organized by severity rather than as an undifferentiated list** | The tiered "security/quality checks" framing found in review-oriented tooling generally, and specifically the halt-until-resolved framing of a quality gate | The Severity Classification in Section 10 below, which reuses the Critical / Major / Minor terminology already present elsewhere in this framework's own document set (`FRAMEWORK_STATUS.md`, "Frozen Architecture" section) for cross-document consistency (Rule 5), mapped entirely to Layer 2 rules already in force — no new gate or field is introduced (see Section 10's Scope note) |
| **Explicit, checkable completion criteria rather than an unverified "looks fine"** | The "Goal-Driven Execution" principle (`README_andrej-karpathy-skills.md`) — define success criteria, loop until verified | The Completion Criteria table (Section 11), which operationalizes the existing Layer 2 Definition of Done (`global_rules_revisionfinal_v10.md`, Section 10) for review specifically |
| **Surfacing scope creep or unrelated changes rather than silently accepting them** | The "Surgical Changes" principle (`README_andrej-karpathy-skills.md`) — touch only what you must, flag but don't fix adjacent issues | The Scope Conformance check in Stage 3 (Section 9.3), grounded in the Single Responsibility and coherent-change rules already fixed at Layer 2 (`global_rules_revisionfinal_v10.md`, Sections 3.1.2, 5.2) |
| **A durable, attributable record of what was checked and why a finding was raised** | The provenance/audit-trail framing of session and observation records (`README_agentmemory.md`) | The requirement that every finding in this Skill's output cite the specific governing-layer rule it traces to (Section 9, every stage), and the flagging (not writing) of any `DECISIONS.md` candidate surfaced during review (Section 8, item 5), consistent with the same discipline already established in `skill-create-feature.md`, Section 9.6.3 |

**Explicit non-adoptions.** No plugin/marketplace installation mechanism, no hook or lifecycle-event engine, no MCP server or tool-connector protocol, no persistent memory-server process, and no slash-command or CLI runtime from any of the three reference materials is a component of this Skill or of Framework v10. No externally-sourced quality-gate mechanism, scoring formula, or automated-repair loop is adopted — this Skill produces a findings report for human confirmation at Gate 4; it does not itself gate, block, or auto-repair anything by runtime mechanism. What is carried forward is the *pattern* named in the left-hand column above, mapped to a mechanism the Blueprint or `global_rules_revisionfinal_v10.md` already defines — nothing more. Adopting any concrete mechanism from a reference material would be an architectural decision requiring its own Gate 2 (Architecture Approval) proposal (`FRAMEWORK_BLUEPRINT.md`, Section 18.2), and is explicitly out of scope for this document.

---

## 3. The Seven Required Fields — Summary

Per SK-002 (`FRAMEWORK_BLUEPRINT.md`, Section 10.3, correction C-08), every Skill document MUST populate all seven of the following fields. This table is the authoritative, at-a-glance summary; each field is expanded in the sections that follow.

| Field | Value |
|---|---|
| `skill-name` | `review-code` |
| `primary-agent-role` | Code Review Agent (`FRAMEWORK_BLUEPRINT.md`, Section 10.5; `SKILLS.md`, Section 12) — a single, non-context-dependent role, unlike `create-feature`'s two-role framing. |
| `gate` | `Gate 4 — Implementation Approval` (Section 1 above; confirmed, not provisional, as of this document, consistent with `SKILLS.md`, Section 12, Footnote 2) |
| `input` | Section 7 |
| `output` | Section 8 |
| `steps` | Section 9 |
| `framework-alignment` | Section 13 |

A Skill document with any of these seven fields unpopulated is not valid and MUST NOT be added to the manifest as `Active` (`FRAMEWORK_BLUEPRINT.md`, Section 10.3). All seven are fully populated in this document.

---

## 4. Purpose

`review-code` performs the AI-executed code-quality and rule-conformance review required, per `global_rules_revisionfinal_v10.md`, Section 7.4, before a non-trivial change may merge — with human confirmation remaining authoritative at Gate 4 (Implementation Approval), per that same section and `FRAMEWORK_BLUEPRINT.md`, Section 9.2. It is the terminal Skill of the Development Workflow Phase (`FRAMEWORK_BLUEPRINT.md`, Section 8): the phase's other two Skills, `create-feature` and `generate-tests`, feed their combined output forward into this Skill, and this Skill's output is what Gate 4 evaluates before a merge may proceed (`SKILLS.md`, Section 8).

`review-code` does not implement, rewrite, or remediate code. It examines the implementation and tests already produced, checks them against every applicable governing-layer rule, classifies any deviation by severity (Section 10), and compiles a structured report that either recommends the change is ready for Gate 4 approval or identifies what must change before it can be. Where a finding requires remediation, that remediation is performed by a subsequent invocation of `create-feature` and/or `generate-tests`, not by this Skill, consistent with the rejected-Gate-4 loop-back already documented in `SKILLS.md`, Section 8.

Maintainability is not one review dimension among the ten checked in Section 9 below — it is the lens through which every one of the ten is weighed, consistent with the Constitution's value ordering (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 4: "Performance and trends MUST NEVER outweigh maintainability") and its Final Principle (Section 22). A finding that is technically correct but immaterial to long-term maintainability SHOULD be classified no higher than Minor (Section 10); a finding that threatens long-term maintainability MUST be classified at least Major regardless of how small the affected code surface is.

---

## 5. Scope

### 5.1 In Scope

1. Reviewing the combined output of `create-feature` and, where it has already executed, `generate-tests` for a single, already-implemented feature within a single project (SK-003).
2. Checking that output against every dimension enumerated in Section 9 (Workflow): Blueprint compliance, Framework Rule compliance, Architecture compliance, Technology compliance, Code Quality, Security, Documentation impact, Test coverage, and RFC 2119 compliance where applicable — with Maintainability applied as the weighing lens throughout (Section 4).
3. Classifying every deviation found by severity (Section 10), with an explicit citation to the governing-layer rule each finding traces to.
4. Compiling a structured findings report suitable for direct human review at Gate 4 (Section 8), including an explicit recommendation (ready for Gate 4 approval, or not) grounded in whether any Critical finding remains open (Section 10).
5. Flagging any architectural-decision candidate surfaced during review, for a human or a subsequent process step to record in `DECISIONS.md` (Section 8, item 5) — without writing to it directly.

### 5.2 Out of Scope

1. **Implementing, rewriting, or remediating the code or tests under review.** Where a finding requires a code or test change, `review-code` MUST report the finding; it MUST NOT perform the fix itself. Remediation is the responsibility of a subsequent `create-feature` and/or `generate-tests` invocation, following the rejected-Gate-4 loop-back documented in `SKILLS.md`, Section 8.
2. **Generating new tests.** Where a coverage gap is found (Section 9.6), `review-code` reports the gap; it MUST NOT author the missing test itself — that is `generate-tests`'s responsibility (`SKILLS.md`, Section 7).
3. **Approving or rejecting the change at Gate 4.** Per `global_rules_revisionfinal_v10.md`, Section 7.4, "human confirmation remain[s] authoritative at Gate 4." `review-code`'s output is a recommendation and a findings report; it is not itself the Gate 4 approval decision, which belongs to the human Engineering CEO (`FRAMEWORK_BLUEPRINT.md`, Section 1.2, Level 1).
4. **Defining or redefining the feature's scope.** Scope was fixed at Gate 3 (Scope Approval) and elaborated by `create-feature`; `review-code` MUST NOT expand, narrow, or reinterpret that scope — it checks conformance to it (Section 9.3).
5. Any cross-project operation, or any modification to a Layer 1–4 document (SK-003, `FRAMEWORK_BLUEPRINT.md` Section 10.4).
6. Selecting or changing the project's archetype, technology stack, or Layer 4 structural conventions. Those are already fixed by the time this Skill executes (Section 6).

---

## 6. Preconditions

`review-code` MUST NOT begin execution until all of the following are satisfied:

1. **Session initialization is complete.** The AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7) has been completed at least once in the current session.
2. **The project is bootstrapped.** Per `PROJECT_BOOTSTRAP_GUIDE.md`, Section 3, the project has passed Gate 1 (Plan Approval) and its scaffold has been validated against the applicable Layer 9 specification or `PROJECT_STRUCTURE.md`.
3. **`create-feature`'s output exists** for the feature under review, per that Skill's own Completion Criteria (`skill-create-feature.md`, Section 10).
4. **Test coverage consistent with the Layer 2 TDD Boundary exists** for the feature under review (`global_rules_revisionfinal_v10.md`, Section 4), whether produced by a completed `generate-tests` invocation or, in a project where `generate-tests` has not yet been separately invoked, embedded in `create-feature`'s test-first implementation (Section 9.4 of `skill-create-feature.md`). `review-code` MUST verify which of these applies at Stage 1 (Section 9.1) rather than assume one.
5. **The matching Layer 4 Project Rule document is `Active`.** The document corresponding to the project's archetype (e.g., `project-pc-app_v04.md`) has been read in full for this task.
6. **`SKILLS.md` selection has occurred.** This Skill was selected through the two-part discovery filter — `primary-agent-role` match (Code Review Agent) and current Workflow Phase fit (`SKILLS.md`, Section 6) — not by name-matching alone.

If any precondition above is not satisfied, `review-code` MUST NOT execute. The gap MUST be reported per Section 12 (Failure Conditions) rather than worked around.

---

## 7. Input

Per `SKILLS.md`, Section 7, `review-code`'s declared input is the combined output of `create-feature` and `generate-tests` — it is the last Skill in the Development Workflow Phase, consuming both prior Skills' outputs. The full input set is:

1. **`create-feature`'s declared output** for the feature under review: working business/domain logic, application/use-case orchestration, presentation-layer wiring where applicable, any newly declared ports/interfaces and their adapters, and updated documentation (`skill-create-feature.md`, Section 8).
2. **`generate-tests`'s declared output** for the same feature, where it has separately executed, or the test artifacts embedded in `create-feature`'s test-first implementation where it has not (Section 6, item 4).
3. **The scoped feature definition** produced at Gate 3 (Scope Approval), so that Scope Conformance (Section 9.3) can be checked against the original boundary, not against `create-feature`'s own restatement of it.
4. **The matching Layer 4 Project Rule document** for the current project's archetype, supplying the directory layout, naming conventions, and archetype-specific technical decisions the output must conform to.
5. **`global_technology_stack_v10.md`**, for Technology compliance checks (Section 9.5) against the canonical stack as applied through the Layer 4 document.
6. **Any prior `DECISIONS.md` entries** relevant to the feature's domain, so that this invocation does not raise a finding that in fact merely reflects an already-recorded, approved architectural decision.

---

## 8. Output

Per `SKILLS.md`, Section 8, `review-code`'s output is what Gate 4 (Implementation Approval) evaluates before a merge may proceed. `review-code` has no successor Skill within Layer 7 (Section 14); its output feeds forward only to a human checkpoint. The full output set is:

1. **A structured findings report**, organized by the review dimensions of Section 9 (Blueprint compliance, Framework Rule compliance, Architecture compliance, Technology compliance, Code Quality, Security, Documentation impact, Test coverage, RFC 2119 compliance where applicable), with every finding classified by severity (Section 10) and citing the specific governing-layer rule it traces to.
2. **An explicit recommendation**: ready for Gate 4 approval, or not — determined mechanically by whether any Critical finding remains open (Section 10), and stated as a conclusion, not left for the human to infer from the raw findings list alone.
3. **A Definition of Done cross-check** (`global_rules_revisionfinal_v10.md`, Section 10), stating explicitly which items of that table were verified and which, if any, remain unsatisfied.
4. **A Test Coverage summary**, stating which TDD Boundary zone(s) (`global_rules_revisionfinal_v10.md`, Section 4) the reviewed change touches and whether coverage in each zone meets that zone's requirement level (MUST / SHOULD / MAY, per Section 9.6 below).
5. **Any flagged architectural-decision candidate** surfaced during review, for a human or a subsequent process step to record in `DECISIONS.md`. Per SK-003, `review-code` itself MUST NOT write to `DECISIONS.md` — flagging it clearly is sufficient to satisfy PR-001 downstream, exactly as `create-feature` does for its own output (`skill-create-feature.md`, Section 8, item 6).
6. **A statement of scope reviewed**, naming precisely which artifacts (which files, ports, or modules) were examined, so that the human at Gate 4 can verify completeness of the review itself, not only its conclusions.

`review-code`'s output does not itself constitute Gate 4 approval (Section 5.2, item 3). It is the artifact the human Engineering CEO reviews in order to approve or reject at that Gate.

---

## 9. Workflow

`review-code`'s `steps` field consists of nine ordered stages, mapping directly to the ten review dimensions required of this Skill (Maintainability being the cross-cutting lens described in Section 4, rather than an isolated stage). Per `FRAMEWORK_BLUEPRINT.md`, Section 10.3, this Skill's `steps` describe only its own execution — they consume `create-feature`'s and `generate-tests`'s declared outputs as input, but MUST NOT describe those Skills' internal procedures.

### 9.1 Stage 1 — Intake and Review Scope Confirmation

1. The agent MUST confirm which of `create-feature`'s and, where applicable, `generate-tests`'s outputs are present and available for review, per Section 6, item 4.
2. The agent MUST identify the precise set of files, modules, ports, and documentation artifacts that constitute the change under review (Section 8, item 6), so that the review's own completeness is later verifiable.
3. The agent MUST retrieve the original Gate 3 scope definition (Section 7, item 3), not merely `create-feature`'s restatement of it, as the reference point for Scope Conformance (Section 9.3).
4. Where the input set required by Section 7 is incomplete (e.g., `create-feature`'s output exists but no test artifact of any kind is present, in violation of Section 6, item 4), the agent MUST halt and report per Section 12 rather than proceed to a partial review.

### 9.2 Stage 2 — Blueprint and Framework Rule Compliance Review

1. The agent MUST verify that the change respects the downward authority rule (KA-002, `FRAMEWORK_BLUEPRINT.md`, Section 6): that no artifact in the change modifies, contradicts, or restates in a conflicting way any Layer 1–4 document.
2. The agent MUST verify that the change does not perform a cross-project operation and does not require modifying a Layer 1–4 document (SK-003) — if it does, this is not a defect to report as a normal finding but a Failure Condition requiring escalation (Section 12, item 3), since `create-feature` itself should have halted at this condition per its own Section 11.
3. The agent MUST verify Vendor Independence (`global_rules_revisionfinal_v10.md`, Section 2): every new external dependency on a cloud provider, database engine, vector store, LLM provider, embedding provider, or storage provider is accessed only through an owned interface, with no vendor SDK type leaking into domain code.
4. The agent MUST verify the Git Workflow Rules relevant to this stage (`global_rules_revisionfinal_v10.md`, Section 5): the change represents one coherent logical unit (Section 5.2, cross-referenced against Scope Conformance in Section 9.3 below), and the commit message, where already drafted, follows Conventional Commits format (Section 5.1).
5. The agent MUST verify the Environment and Dependency Management Principles (`global_rules_revisionfinal_v10.md`, Section 6): any new dependency is declared explicitly and is a deliberate, justified addition rather than a duplication of existing functionality (Section 6.4).

### 9.3 Stage 3 — Architecture and Technology Compliance Review

1. The agent MUST verify the Clean Architecture boundary (`global_rules_revisionfinal_v10.md`, Section 3.2): domain code does not import infrastructure code, and dependencies point from concrete/infrastructure code toward abstract/domain code.
2. The agent MUST verify Separation of Concerns and the Single Responsibility Principle (`global_rules_revisionfinal_v10.md`, Section 3.1): each touched module has a single, clearly stated responsibility.
3. The agent MUST verify **Scope Conformance**: that the change addresses exactly the feature scoped at Gate 3 (Section 7, item 3) — no unrelated adjacent refactoring, no drive-by changes to code, comments, or formatting outside the feature's boundary, consistent with `skill-create-feature.md`, Section 9.4, item 5. Any unrelated defect noticed during review MUST be reported as a separate observation, not folded into this feature's findings as if in scope.
4. The agent MUST verify that no new abstraction, interface, or extension point was introduced without solving a real, currently existing problem (`global_rules_revisionfinal_v10.md`, Section 3.2.3, 3.3).
5. The agent MUST verify Technology compliance against the canonical stack (`global_technology_stack_v10.md`) as applied through the matching Layer 4 document (e.g., `project-pc-app_v04.md`, Section 8): that no technology outside the approved table, or outside that document's stated defaults and alternatives, was introduced without being flagged as a Layer 3 gap.
6. The agent MUST verify conformance to the matching Layer 4 archetype's directory layout and naming conventions (e.g., `project-pc-app_v04.md`, Sections 4–5, for a Desktop/Electron project).

### 9.4 Stage 4 — Code Quality Review

1. The agent MUST verify that the change would pass automated linting and, where the language supports it, static type checking (`global_rules_revisionfinal_v10.md`, Section 7.1).
2. The agent MUST verify the Exception Handling Rule (`global_rules_revisionfinal_v10.md`, Section 11): no generic or base exception type is caught without either immediate re-raising after a narrowly scoped action or an explicit, in-code justification; no exception is silently swallowed (Section 11.4).
3. The agent MUST verify that no dead code — code with no reachable caller and no documented future purpose — has been introduced (`global_rules_revisionfinal_v10.md`, Section 7.3).
4. The agent MUST verify that the code is written for the reader first and the machine second, consistent with the Constitution's Simplicity value (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 8) and `global_rules_revisionfinal_v10.md`, Section 7.5.

### 9.5 Stage 5 — Security Review

1. The agent MUST verify that no secret, credential, API key, token, or connection string has been introduced into version control in any form, including comments, test fixtures, or commit history (`global_rules_revisionfinal_v10.md`, Section 9.1).
2. The agent MUST verify that an example/template exists for any newly required environment configuration, containing no real secret values (`global_rules_revisionfinal_v10.md`, Section 9.2).
3. The agent MUST verify that input from outside the application's trust boundary is validated before use in business logic (`global_rules_revisionfinal_v10.md`, Section 9.3) — including, for a Desktop/Electron project, IPC payloads crossing from renderer to main (`project-pc-app_v04.md`, Section 6.1).
4. The agent MUST verify that components are granted the minimum privilege necessary to perform their function (`global_rules_revisionfinal_v10.md`, Section 9.4).
5. The agent MUST verify that any known vulnerability in a newly introduced or newly reachable dependency has been identified and addressed rather than left as background noise (`global_rules_revisionfinal_v10.md`, Section 9.5).

### 9.6 Stage 6 — Test Coverage Verification

1. The agent MUST identify which Layer 2 TDD Boundary zone(s) (`global_rules_revisionfinal_v10.md`, Section 4) the change touches: business/domain logic, application/use-case orchestration, infrastructure adapters, or UI/framework-glue.
2. The agent MUST verify that business/domain logic has test coverage consistent with a test-first requirement (MUST) and that application/use-case orchestration has coverage consistent with its own requirement level (SHOULD test-first, MUST have coverage before the commit is done).
3. The agent MUST verify that infrastructure adapters have integration-level test coverage where the TDD Boundary recommends it (SHOULD), and MUST NOT treat its absence as a Critical finding where the boundary itself only recommends, rather than requires, it (Section 10).
4. The agent MUST verify that tests exercise behavior rather than only implementation detail, consistent with the TDD Boundary's stated preference for UI/framework-glue code (`global_rules_revisionfinal_v10.md`, Section 4).

### 9.7 Stage 7 — Documentation Impact Review

1. The agent MUST verify that documentation made inaccurate by the change has been updated in the same change, per `global_rules_revisionfinal_v10.md`, Section 8.5 — documentation debt MUST NOT be deferred as a separate, unscheduled task.
2. The agent MUST verify that any new or changed public interface carries documentation describing its contract — inputs, outputs, and side effects (`global_rules_revisionfinal_v10.md`, Section 8.4).
3. The agent MUST verify that documentation explains *why* a non-obvious decision was made, not only *how* the code works (`global_rules_revisionfinal_v10.md`, Section 8.1).
4. Where documentation includes a stated contract using RFC 2119 keywords (e.g., a public interface's documented obligations), the agent MUST perform the RFC 2119 Compliance check of Stage 8 (Section 9.8) against that same documentation, rather than treating the two checks as unrelated.

### 9.8 Stage 8 — RFC 2119 Compliance Review (Where Applicable)

1. This check applies only where the change under review itself produces or modifies text that uses, or should use, RFC 2119 keywords — most commonly, documented contracts for public interfaces (Section 9.7, item 2) or any project-level rule text the feature happens to introduce. It does not apply to ordinary application code that contains no such normative text.
2. Where applicable, the agent MUST verify that **MUST** / **MUST NOT** is reserved for genuinely mandatory requirements, **SHOULD** / **SHOULD NOT** for strong recommendations admitting a justified exception, and **MAY** / **OPTIONAL** for genuinely discretionary behavior — consistent with the usage already established throughout this framework's own governing documents.
3. Where applicable, the agent MUST verify that a normative statement is not weakened or strengthened relative to what the underlying governing-layer rule (Layer 1–4) actually requires — e.g., a contract comment MUST NOT state "MUST" for a behavior that the governing layer only states as "SHOULD."
4. Where this check does not apply (no RFC 2119 text is present in the change), the agent MUST state this explicitly in the output ("N/A — no normative text introduced") rather than omit the dimension silently, so that the report in Section 8 remains a complete accounting of all nine review dimensions of Section 9.2–9.9.

### 9.9 Stage 9 — Severity Classification, Findings Compilation, and Gate Handoff

1. The agent MUST classify every finding surfaced in Stages 2–8 by severity, per Section 10, citing the specific governing-layer rule each finding traces to.
2. The agent MUST determine the overall recommendation mechanically: if any Critical finding remains open, the recommendation MUST be "not ready for Gate 4 approval"; otherwise, the recommendation MUST state readiness, together with a list of any open Major or Minor findings for the human's awareness.
3. The agent MUST compile the full output set of Section 8 into a single structured report.
4. The agent MUST perform a final self-check that every review dimension named in Section 9.2–9.8 was in fact addressed in the report — including an explicit "N/A" statement where a dimension does not apply (Section 9.8, item 4) — before presenting the output as complete.
5. Only once this stage passes without an incomplete dimension MUST the agent present its output as the completed result of `review-code` and hand off to Gate 4 (Implementation Approval) for human confirmation.

---

## 10. Severity Classification

**Scope note.** Framework v10 does not define a Skill-specific severity schema, a new HITL Gate, or a new Skill Metadata field for review findings. `BLUEPRINT_INPUT_FREEZE.md` contains no such decision, and none is introduced here. What follows is a reporting classification applied to findings already produced by Stages 2–8 (Section 9) against rules already fixed at Layer 1–4 — it changes nothing about the binding force of any MUST/SHOULD/MAY rule cited elsewhere in this document; it only organizes findings for legibility at Gate 4. The three-level naming (Critical / Major / Minor) reuses terminology already present elsewhere in this framework's own document set (`FRAMEWORK_STATUS.md`, "Frozen Architecture" section: "If inconsistencies are discovered, classify them as: Critical, Major, Minor"), consistent with the requirement to keep terminology consistent across documents, rather than inventing a new vocabulary in isolation.

| Severity | Definition | Effect on the Section 9.9 Recommendation |
|---|---|---|
| **Critical** | A finding that a MUST/MUST NOT-level governing-layer rule has been violated in a way that materially threatens correctness, security, vendor independence, or the Clean Architecture boundary. Includes, without limitation: a secret committed to version control (Section 9.5.1); a Clean Architecture boundary violation, i.e., domain code importing infrastructure code (Section 9.3.1); missing test-first coverage for business/domain logic (Section 9.6.2); a silently swallowed exception (Section 9.4.2); an external dependency not placed behind an owned interface (Section 9.2.3); unvalidated input from outside the trust boundary reaching business logic (Section 9.5.3). | The overall recommendation MUST be "not ready for Gate 4 approval" while any Critical finding remains open (Section 9.9.2). |
| **Major** | A finding that a MUST-level rule of lesser materiality, or a SHOULD-level rule, has been violated without an explicit, stated justification. Includes, without limitation: documentation not updated in the same change (Section 9.7.1) where the affected interface is not itself safety- or security-relevant; a naming-convention violation against the matching Layer 4 archetype (Section 9.3.6); dead code present (Section 9.4.3); missing integration-level test coverage for an infrastructure adapter, where the TDD Boundary only recommends it (Section 9.6.3), and no justification is stated. | The recommendation MAY still state readiness for Gate 4 approval, but every open Major finding MUST be listed explicitly for the human's awareness (Section 9.9.2) — approval with an open Major finding requires the human's explicit, informed acceptance of it, not silent omission from the report. |
| **Minor** | A finding that is stylistic, non-blocking, or a SHOULD-level rule violated with a stated and reasonable justification. Includes, without limitation: a readability suggestion; a comment that could better capture non-obvious intent (Section 9.7.3) without currently violating the rule; a discretionary (MAY-level) convention not followed. | The recommendation MAY state readiness for Gate 4 approval without qualification; Minor findings are informational and MUST still be listed in the output (Section 8, item 1) but do not themselves block or condition the recommendation. |

A finding MUST NOT be classified below the severity its triggering rule's own RFC 2119 keyword implies — a MUST-level violation MUST NOT be reported as Minor merely because the affected code surface is small (Section 4). Where a finding's classification is genuinely ambiguous between two adjacent severities, the agent MUST classify at the higher of the two and state the ambiguity explicitly in the report, rather than silently resolve it downward.

---

## 11. Completion Criteria

`review-code`'s output is complete if and only if every item below holds. This list operationalizes the Layer 2 Definition of Done (`global_rules_revisionfinal_v10.md`, Section 10) for this Skill specifically; it does not replace or narrow it.

| # | Criterion |
|---|---|
| 1 | The review scope (Section 9.1) is stated explicitly and matches the full set of artifacts produced by `create-feature` and, where applicable, `generate-tests` for the feature under review. |
| 2 | Every review dimension in Section 9.2–9.8 has been addressed, including an explicit "N/A" statement for RFC 2119 Compliance where it does not apply (Section 9.8.4). |
| 3 | Every finding is classified by severity per Section 10 and cites the specific governing-layer rule it traces to. |
| 4 | The overall recommendation is stated as a conclusion (ready / not ready for Gate 4 approval) and is mechanically consistent with the presence or absence of open Critical findings (Section 9.9.2). |
| 5 | Every open Major finding is listed explicitly, even where the overall recommendation states readiness (Section 10). |
| 6 | The Definition of Done cross-check (Section 8, item 3) and the Test Coverage summary (Section 8, item 4) are both present and specific, not generic restatements of Section 4/10 of `global_rules_revisionfinal_v10.md`. |
| 7 | Any architectural-decision candidate surfaced during review is explicitly flagged in the output, not silently absorbed (Section 8, item 5). |
| 8 | The output does not itself claim to constitute Gate 4 approval (Section 5.2, item 3). |
| 9 | Stage 9's final self-check (Section 9.9.4) passed without an incomplete review dimension. |
| 10 | The output is structured such that the human Engineering CEO can render a Gate 4 decision directly from it, without needing to re-derive missing context (Section 8, item 6). |

An agent executing `review-code` MUST self-check its output against this table before presenting it as complete, consistent with the equivalent self-check requirement already stated at Layer 2 (`global_rules_revisionfinal_v10.md`, Section 10, closing paragraph) and already applied by `create-feature` to its own output (`skill-create-feature.md`, Section 10).

---

## 12. Failure Conditions and Escalation

`review-code` MUST NOT attempt to silently work around any of the following. Each is a stop condition requiring the agent to report the gap and halt this Skill's execution rather than proceed on an assumption:

1. **A precondition in Section 6 is not satisfied** (e.g., `create-feature`'s output does not exist, or no test artifact of any kind is present for the feature under review). The agent MUST report which precondition is unmet and MUST NOT begin Stage 1.
2. **The input set required by Section 7 is incomplete in a way that would make the review partial rather than complete** (Section 9.1.4). The agent MUST report exactly which input is missing rather than review only the available portion and present it as a complete result.
3. **The change under review appears to modify a Layer 1–4 document or to perform a cross-project operation** (Section 9.2.2). This is not a normal finding to classify and report under Section 10; per SK-003, it MUST be escalated as a Gate 2 (Architecture Approval) proposal and, if approved, recorded in `DECISIONS.md`, never merged via the ordinary Gate 4 path.
4. **The matching Layer 4 Project Rule document is not `Active`** (e.g., the project's archetype is Full-Stack or Monolithic, whose Layer 4 documents are `Pending` per `FRAMEWORK_README.md`, Section 4). The agent MUST report this as a standing Tier gap and MUST NOT improvise directory or naming-convention checks against a deprecated legacy document.
5. **The original Gate 3 scope definition cannot be retrieved**, making Scope Conformance (Section 9.3.3) unverifiable. The agent MUST report this rather than assume the feature's own restatement of scope is authoritative.
6. **A finding's severity classification is genuinely ambiguous between Critical and Major** in a way that materially changes the overall recommendation (Section 9.9.2). The agent MUST classify at the higher severity and state the ambiguity explicitly (Section 10), rather than silently resolve it in either direction.
7. **The review surfaces a defect that appears to require a new architectural pattern not already authorized by Layers 1–4**, discovered only during review rather than during `create-feature`'s own Stage 2 (Specification). The agent MUST escalate per this section rather than approve, reject, or silently accept the pattern within the review's own recommendation.

In every case above, escalation means: the agent reports the gap plainly, states which layer or gate the resolution belongs to, and stops — consistent with the same escalation discipline `PROJECT_BOOTSTRAP_GUIDE.md` (Section 5), `SKILLS.md` (Section 6), and `skill-create-feature.md` (Section 11) already apply elsewhere in this framework.

---

## 13. `framework-alignment` (SK-002, Required Field)

This Skill's execution is bound to the following governing-layer rules, cited by reference and not restated:

| Layer | Document | Sections this Skill's execution is bound by |
|---|---|---|
| 1 | `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | Core value ordering (Section 4, the Maintainability lens of Section 4 above); Simplicity (Section 8); AI Readability (Section 16) |
| 2 | `global_rules_revisionfinal_v10.md` | Vendor Independence (Section 2); Architecture Rules (Section 3); TDD Boundary (Section 4); Git Workflow Rules (Section 5); Environment and Dependency Management Principles (Section 6); Code Quality Gates (Section 7); Documentation Standards (Section 8); Security Baselines (Section 9); Definition of Done (Section 10); Exception Handling Rule (Section 11) |
| 3 | `global_technology_stack_v10.md` | The canonical technology table, applied through whichever Layer 4 document is in effect — this Skill introduces no technology selection of its own |
| 4 | The matching Project Rule document for the current archetype (e.g., `project-pc-app_v04.md`) | Directory layout, naming conventions, and archetype-specific technical decisions (e.g., the IPC error-handling pattern in `project-pc-app_v04.md`, Section 6, checked at Stage 5, Section 9.5.3, where the archetype is Desktop) |

This Skill's own `steps` (Section 9) MUST NOT be read as adding to, narrowing, or reinterpreting any rule in the table above, and the Severity Classification (Section 10) MUST NOT be read as altering the RFC 2119 force of any rule it references. Where a step above appears to state a rule not already present in one of these documents, that step is in error and MUST be corrected to a reference (per the framework's inheritance model, `FRAMEWORK_BLUEPRINT.md`, Section 5).

---

## 14. Related Skills

Per `SKILLS.md`, Sections 7 and 8:

| Skill | Relationship to `review-code` |
|---|---|
| `create-feature` (Active) | Predecessor. `review-code` consumes `create-feature`'s output as part of its declared input (Section 7). A rejected Gate 4 review returns control to `create-feature` for remediation (`SKILLS.md`, Section 8), not to `review-code` itself. |
| `generate-tests` (Pending — not yet generated) | Predecessor. `review-code` consumes `generate-tests`'s output as the other part of its declared input, where that Skill has separately executed (Section 6, item 4). |

`review-code` has no successor Skill within Layer 7. Its output feeds forward only to Gate 4 (Implementation Approval), a human checkpoint, per `SKILLS.md`, Section 8 and Section 9.2 of `FRAMEWORK_BLUEPRINT.md`.

---

## 15. Related Documents

| Document | Relevance |
|---|---|
| `FRAMEWORK_BLUEPRINT.md` | Master structural authority for this Skill's classification, metadata schema, Workflow Phase concept, and HITL Gate positions (Sections 8–10). |
| `SKILLS.md` | The Layer 7 Manifest this document's entry belongs to; the source of the Skill Dependencies (Section 7) and Skill Composition (Section 8) this document's Sections 1 and 14 apply. |
| `global_rules_revisionfinal_v10.md` | Layer 2 source of every binding engineering rule this Skill's review checks against, including the code-review requirement itself (Section 7.4). |
| `global_technology_stack_v10.md` | Layer 3 source of the technology table this Skill checks against, applied through the Layer 4 document. |
| The matching Layer 4 Project Rule document (e.g., `project-pc-app_v04.md`) | Supplies the directory layout, naming conventions, and archetype-specific patterns (e.g., IPC error handling) this Skill's review checks conformance to. |
| `skill-create-feature.md` | The full specification of `review-code`'s predecessor Skill and the source of the output this Skill consumes as input. |
| `skill-generate-tests.md` (pending) | The full specification of the other predecessor Skill; pending as of this document's generation. |
| `PROJECT_BOOTSTRAP_GUIDE.md` | Governs the preconditions in Section 6 — a project must be bootstrapped before this Skill may execute. |
| `DECISIONS.md` | The Layer 10 destination for any architectural-decision candidate this Skill's execution flags (Section 8, item 5); this Skill does not write to it directly. |
| `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | Layer 1 source of the value ordering — particularly the Maintainability priority — that governs how findings are weighed (Section 4). |
| `FRAMEWORK_STATUS.md` | Source of the Critical / Major / Minor terminology reused, for cross-document consistency, in this Skill's Severity Classification (Section 10). Not a governing-layer document; consulted for terminology consistency only, per the Scope note opening Section 10. |

---

## 16. Authority and Conflict Resolution

1. Per the Primary Authority ordering stated in the header, and per the downward authority rule (KA-002), this document MUST NOT contradict `FRAMEWORK_BLUEPRINT.md`, `SKILLS.md`, or `global_rules_revisionfinal_v10.md`, in that order of precedence. Where this document appears to conflict with any of the three, the higher-priority document wins and this document MUST be corrected.
2. None of the reference materials named in the header (Section 2) carries any authority over this document. A pattern drawn from them MUST NOT be cited as justification for a rule that conflicts with any Primary Authority document.
3. Where a future amendment to `skill-create-feature.md` or `skill-generate-tests.md`, once authored, appears to require this document's `steps` to change, that change MUST follow the amendment procedure in Section 17 rather than being made unilaterally in either document.
4. The full conflict-resolution procedure, including same-layer conflicts between two operational-layer artifacts, is defined in `FRAMEWORK_BLUEPRINT.md`, Section 6, and is not restated here.

---

## 17. Change Control

1. This document MUST NOT be edited silently to change what `review-code` does. A change to its `input`, `output`, `steps`, `gate`, `framework-alignment`, or Severity Classification (Section 10) is a change to a frozen Skill specification and MUST follow the amendment procedure defined in `FRAMEWORK_BLUEPRINT.md`, Section 18.2: proposal → Gate 2 (Architecture Approval) → amendment → `DECISIONS.md` entry → `FRAMEWORK_HANDOVER.md` update in the same commit.
2. Upon this document's creation, `SKILLS.md`, Section 12, MUST be updated in the same change: the **Document Status** for `review-code` MUST move from `Pending — not yet generated` to `Active`, and Footnote 2's `Gate 4 — Implementation Approval` assignment MUST be marked as confirmed and authoritative per Section 3 of this document, per `SKILLS.md` Section 18.2.
3. `SKILLS.md`, Section 9 (Skill Lifecycle), MUST be updated in the same change to reflect that `review-code` has entered the `Active` state, per the same section's stated diagram.
4. Layer 7 as a whole does not become `Active` on this document alone. Per `FRAMEWORK_BLUEPRINT.md`, Section 2.7, Layer 7 requires the Manifest and all three starter Skill documents. `skill-generate-tests.md` remains outstanding.

---

## Closing Statement

This document is the full, seven-field specification of `review-code`, the terminal Skill of Framework v10's Development Workflow Phase. It resolves `SKILLS.md`'s already-non-provisional `Gate 4 — Implementation Approval` assignment into this document's own authoritative declaration, defines a nine-stage workflow — Intake and Review Scope Confirmation, Blueprint and Framework Rule Compliance, Architecture and Technology Compliance, Code Quality, Security, Test Coverage Verification, Documentation Impact, RFC 2119 Compliance (where applicable), and Severity Classification with Gate Handoff — covering all ten review dimensions required of this Skill (Maintainability applied as the cross-cutting weighing lens per Section 4, rather than as an isolated stage), grounded entirely in rules already fixed by `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, `global_rules_revisionfinal_v10.md`, and the matching Layer 4 Project Rule document. It introduces a Severity Classification (Section 10) that is explicitly scoped as a reporting lens over already-frozen rules, reusing terminology already present elsewhere in this framework's document set rather than inventing new architecture, a new Gate, or a new metadata field. Per Section 17, Layer 7 remains incomplete until `skill-generate-tests.md` is also generated.
