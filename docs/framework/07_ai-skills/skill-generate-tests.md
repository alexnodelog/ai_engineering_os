# skill-generate-tests.md

**Status:** Active
**Layer:** 7 — AI Skills
**Tier:** 1 (Critical)
**Skill Name:** `generate-tests`
**Purpose:** Define the full, seven-field specification (SK-002; `FRAMEWORK_BLUEPRINT.md`, Section 10.3, correction C-08) of the `generate-tests` Skill — the second Skill of the Development Workflow Phase (`FRAMEWORK_BLUEPRINT.md`, Section 8), responsible for producing comprehensive test coverage — Unit Tests, Integration Tests, End-to-End Test Plans, Regression Tests, Edge Cases, and Negative Test Cases — for a unit of business, application, or infrastructure logic, positioned within the Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4), in a manner that is directly executable by an AI agent without requiring further clarification beyond what this document and the layers it inherits from already state.
**Primary Authority (highest priority, in order):** `FRAMEWORK_BLUEPRINT.md` → `SKILLS.md` → `global_rules_revisionfinal_v10.md`. This document introduces no architectural decision of its own and MUST NOT be read as amending any of the three. Where any statement below appears to conflict with one of them, the higher-priority document wins and this document MUST be corrected (Section 17).
**Reference materials (informational inspiration only, no authority):** `README_andrej-karpathy-skills.md`, `README_Claude Forge.md`, `README_agentmemory.md`. Consulted solely for general patterns in test workflow shape, metadata framing, and Skill organization (Section 2 below). No implementation-specific mechanism, vendor tooling, or runtime behavior from any of the three is adopted anywhere in this document. Where a reference pattern and the Primary Authority would ever conflict, the Primary Authority prevails without exception.
**Inherits from:** `global_rules_revisionfinal_v10.md` (Layer 2 — the TDD Boundary and quality gates this Skill's output must satisfy, cited by reference and not restated), `global_technology_stack_v10.md` (Layer 3 — the approved test runner and tooling this Skill's `steps` may introduce), and, at the point this Skill actually executes, whichever Layer 4 Project Rule document matches the current project's archetype (e.g., `project-pc-app_v04.md` for a Desktop/Electron project) — per `FRAMEWORK_BLUEPRINT.md`, Section 2.7, "Inputs."
**Governs:** Any Layer 8 Prompt Library entry that invokes `generate-tests` (which MUST reference this document rather than duplicate its logic, per `FRAMEWORK_BLUEPRINT.md` Section 11.2), and any Layer 9 Template that declares `generate-tests` as a mandatory Skill reference (per `FRAMEWORK_BLUEPRINT.md` Section 2.9), per the downward authority rule (KA-002, `FRAMEWORK_BLUEPRINT.md` Section 6).
**Supersedes:** None. This is the first version of this document. Its manifest entry in `SKILLS.md`, Section 12, previously recorded a **Document Status** of `Pending — not yet generated` with a provisional `gate` value; this document's creation resolves that entry (Section 17 below).
**Read order:** Read only after an AI agent has (a) completed the AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7), (b) read the Layer 4 Project Rule document matching the current project's archetype, and (c) read `SKILLS.md` and selected `generate-tests` via its two-part discovery filter — role match, then Workflow Phase fit (`SKILLS.md`, Section 6). This document MUST NOT be read, or its `steps` executed, as a substitute for any of those three prerequisites.
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
                            skill-review-code.md     (Active)
                            skill-generate-tests.md  ← this document
        ↓
Layer 8 — Prompt Library
Layer 9 — Project Templates
```

**What this document contains.** The complete seven-field specification of `generate-tests` (Section 3), plus the surrounding context — purpose, scope, preconditions, detailed input/output description, the full test-generation workflow, a taxonomy of the six required test categories, completion criteria, failure conditions, and cross-references — needed for an AI agent to execute this Skill without requiring further clarification, per `SKILLS.md` Section 3's distinction between a manifest entry (a pointer) and a full Skill document (the executable specification).

**What this document does not contain, by design.** Per the Layer 7 prohibited responsibilities (`FRAMEWORK_BLUEPRINT.md`, Section 2.7) and the Skill scope boundary (SK-003, `FRAMEWORK_BLUEPRINT.md` Section 10.4):

- It **MUST NOT** modify, restate as if authoritative, or contradict any Layer 1–4 document. Every rule this Skill's output must satisfy is cited by reference to its governing layer, never reproduced as if invented here.
- It **MUST NOT** define a new HITL Gate, a new agent role, a new Skill Metadata field, or any schema element beyond the seven fields fixed by `FRAMEWORK_BLUEPRINT.md`, Section 10.3. The Test Category Taxonomy introduced in Section 10 below is a documentation lens organizing already-required test coverage — not a new Gate, field, or architectural mechanism (see the Scope note opening that section).
- It **MUST NOT** perform, or describe this Skill as capable of, any cross-project operation.
- It **MUST NOT** restate the canonical technology table (Layer 3) or the archetype's directory layout and naming conventions (Layer 4) — it applies them, by reference, at the point they become relevant to a step.
- It **MUST NOT** implement, or generate, the business/application logic under test. Implementation is the responsibility of `create-feature`, not of this Skill (Section 5.2).
- It **MUST NOT** perform the formal code-quality and rule-conformance review that gates merge, nor render a Gate 4 recommendation. That belongs to `review-code` (Section 5.2).

---

## 1. Relationship to the Skill Manifest (`SKILLS.md`)

This document is the full Skill document that `SKILLS.md`, Section 12, points to under the **Skill Document** column for the `generate-tests` row. Per `SKILLS.md` Section 3, the manifest entry is a pointer, not a substitute — an agent that has only read the manifest's one-line description MUST open this document before executing any `steps` below.

`SKILLS.md`, Section 12, Footnote 1, provisionally assigned `generate-tests` a `gate` value of `none`, pending this document's authoritative declaration, on the same reasoning already applied to `create-feature`: within a single Development Workflow Phase, both Skills feed forward into `review-code` without an intervening human checkpoint, since it is the *combined* output of the phase, not either Skill's output in isolation, that Gate 4 (Implementation Approval) reviews. Section 3 below confirms this value as this document's own authoritative declaration, reconciling rather than overriding the manifest's provisional footnote, per `SKILLS.md` Section 18.2.

Per `SKILLS.md`, Section 7 (Skill Dependencies), `generate-tests`'s declared input is ordinarily `create-feature`'s output — the business/application logic requiring test coverage. That same section states explicitly that this ordering is not fixed: "a project **MAY** invoke `generate-tests` before `create-feature` completes... under the TDD Boundary's requirement that business logic be developed test-first," in which case the practical dependency direction is reversed for that invocation. This document therefore defines its Preconditions (Section 6) and Input (Section 7) to accommodate **both** the typical ordering (this Skill executing after `create-feature`'s output exists, to extend coverage beyond what test-first development strictly required during Implementation) and the reversed ordering (this Skill executing ahead of, or concurrently with, `create-feature`'s Implementation stage, to supply the failing test-first tests that stage requires). Neither ordering is preferred by this document over the other; `SKILLS.md`, Section 7, is explicit that the table records the *typical* relationship for manifest-lookup convenience, not a binding execution order.

`create-feature`'s own scope boundary already draws the line this Skill inherits: per `skill-create-feature.md`, Section 5.2, item 1, "Test generation for coverage beyond what test-first development strictly requires during Implementation... belongs to `generate-tests`." This Skill's Purpose (Section 4) and Scope (Section 5) below are written directly against that boundary — `generate-tests` never duplicates the minimal test-first tests `create-feature` writes inline for TDD-mandatory zones (`skill-create-feature.md`, Section 9.4, item 1); it extends coverage to the full breadth the Owner's requirements name: Unit, Integration, End-to-End, Regression, Edge Case, and Negative Test Cases.

---

## 2. Cross-Project Design Inspiration (Informational Only)

Per the Owner's instruction, general patterns from three external, non-framework projects are reflected in this document's *test workflow shape*, *metadata framing*, and *Skill organization* — never in its architecture, mechanisms, or runtime behavior. Each pattern below is named, its general source noted, and mapped to the specific Blueprint- or `global_rules`-defined mechanism that already governs it in Framework v10.

| Requested pattern | Illustrated (informationally) by | Realized in this Skill exclusively through |
|---|---|---|
| **Transforming an imperative task into a verifiable, test-first goal** | The "Goal-Driven Execution" principle (`README_andrej-karpathy-skills.md`) — e.g., "Fix the bug" → "Write a test that reproduces it, then make it pass" | The Regression Test Generation stage (Section 9.6), which generates a test asserting the correct, fixed behavior at the exact point a prior defect occurred, grounded in the Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4) rather than in any external tooling |
| **A dedicated, staged test-generation step distinct from implementation and from review** | The `/tdd` step of the `/plan → /tdd → /code-review → /handoff-verify → /commit-push-pr` pipeline shape, and the `/test-coverage` and `/e2e` commands (`README_Claude Forge.md`) | The Workflow Phase concept already fixed by `FRAMEWORK_BLUEPRINT.md`, Section 8, and this Skill's own nine-stage `steps` sequence (Section 9), which sits between `create-feature` and `review-code` exactly as documented in `SKILLS.md`, Section 8 |
| **Organizing test output by category rather than as an undifferentiated suite** | The general practice, illustrated across contemporary test-tooling documentation, of naming distinct test kinds (unit, integration, end-to-end, regression) rather than treating "tests" as one unstructured artifact | The Test Category Taxonomy (Section 10 below), which organizes this Skill's declared `output` field (Section 8) against the already-existing Layer 2 TDD Boundary and the matching Layer 4 archetype's test-directory conventions — no new schema field is introduced (see Section 10's Scope note) |
| **Durable, attributable provenance linking a test back to the defect or decision it guards against** | The session-continuity and durable-context framing (`README_agentmemory.md`) | The requirement that a Regression Test cite the specific `DECISIONS.md` entry or defect reference it guards against (Section 9.6, item 3), and the flagging (not writing) of any new `DECISIONS.md` candidate surfaced during test generation (Section 8, item 8), consistent with the same discipline already established in `skill-create-feature.md`, Section 9.6.3, and `skill-review-code.md`, Section 8, item 5 |

**Explicit non-adoptions.** No plugin/marketplace installation mechanism, no hook or lifecycle-event engine, no MCP server or tool-connector protocol, no persistent memory-server process, and no slash-command or CLI runtime from any of the three reference materials is a component of this Skill or of Framework v10. No externally-sourced test-generation formula, coverage-scoring mechanism, or auto-repair loop is adopted — this Skill produces test artifacts for a human and for `review-code` to evaluate; it does not itself run, score, or auto-repair anything by runtime mechanism. What is carried forward is the *pattern* named in the left-hand column above, mapped to a mechanism the Blueprint or `global_rules_revisionfinal_v10.md` already defines — nothing more. Adopting any concrete mechanism from a reference material would be an architectural decision requiring its own Gate 2 (Architecture Approval) proposal (`FRAMEWORK_BLUEPRINT.md`, Section 18.2), and is explicitly out of scope for this document.

---

## 3. The Seven Required Fields — Summary

Per SK-002 (`FRAMEWORK_BLUEPRINT.md`, Section 10.3, correction C-08), every Skill document MUST populate all seven of the following fields. This table is the authoritative, at-a-glance summary; each field is expanded in the sections that follow.

| Field | Value |
|---|---|
| `skill-name` | `generate-tests` |
| `primary-agent-role` | Test Agent (`FRAMEWORK_BLUEPRINT.md`, Section 10.5; `SKILLS.md`, Section 12) — a single, non-context-dependent role, consistent with `review-code`'s single-role framing and unlike `create-feature`'s two-role framing. |
| `gate` | `none` (Section 1 above; confirmed, not provisional, as of this document, consistent with `SKILLS.md`, Section 12, Footnote 1) |
| `input` | Section 7 |
| `output` | Section 8 |
| `steps` | Section 9 |
| `framework-alignment` | Section 13 |

A Skill document with any of these seven fields unpopulated is not valid and MUST NOT be added to the manifest as `Active` (`FRAMEWORK_BLUEPRINT.md`, Section 10.3). All seven are fully populated in this document.

---

## 4. Purpose

`generate-tests` produces test coverage for a unit of business, application, or infrastructure logic, positioned within the Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4). It is the second Skill of the Development Workflow Phase (`FRAMEWORK_BLUEPRINT.md`, Section 8): it consumes `create-feature`'s output (or, where the TDD Boundary requires test-first development, `create-feature`'s Specification and Implementation Plan artifacts ahead of its Implementation stage, per Section 1 above), and its own output feeds forward, jointly with `create-feature`'s output, into `review-code` (`SKILLS.md`, Section 7).

`generate-tests`'s scope is deliberately bounded against `create-feature`'s own test-first obligation. Per `skill-create-feature.md`, Section 9.4, item 1, `create-feature` itself writes the minimal failing tests required by the TDD Boundary for zones that MUST be developed test-first, as an inseparable part of its own Implementation stage. `generate-tests` does not repeat that minimal coverage; it produces the comprehensive coverage the Owner's requirements name in full: **Unit Tests, Integration Tests, End-to-End Test Plans, Regression Tests, Edge Cases, and Negative Test Cases** — across every TDD Boundary zone the feature touches, not only the zone(s) requiring test-first development.

`generate-tests`'s `primary-agent-role` is **Test Agent** (`FRAMEWORK_BLUEPRINT.md`, Section 10.5), a single role for every invocation of this Skill, regardless of which TDD Boundary zone or test category the invocation addresses.

---

## 5. Scope

### 5.1 In Scope

1. Generating **Unit Tests** for business/domain logic and application/use-case orchestration, consistent with the Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4).
2. Generating **Integration Tests** for infrastructure adapters, consistent with the TDD Boundary's recommendation that such adapters have integration-level test coverage.
3. Generating **End-to-End Test Plans** — structured, documented behavioral scenarios exercising the feature as a user would experience it, together with automated end-to-end test stubs where the matching Layer 4 archetype and the current development environment support them.
4. Generating **Regression Tests** that assert correct, fixed behavior at the specific point a previously recorded defect occurred, guarding against its recurrence.
5. Generating **Edge Case** tests exercising boundary conditions of the feature's inputs and state transitions.
6. Generating **Negative Test Cases** exercising invalid input and the feature's expected error paths.
7. Extending test coverage beyond what `create-feature`'s own test-first Implementation stage strictly required (Section 4), without duplicating it.
8. Updating any test-related documentation made inaccurate by the newly generated tests, in the same change (`global_rules_revisionfinal_v10.md`, Section 8.5).
9. Flagging any architectural-decision candidate surfaced during test generation (e.g., an untestable design revealing a missing seam), for a human or a subsequent process step to record in `DECISIONS.md`.

### 5.2 Out of Scope

1. **Implementing the business/application logic under test.** That is `create-feature`'s responsibility (`skill-create-feature.md`, Section 4). `generate-tests` MUST NOT write, modify, or "fix" the implementation code itself, even where a generated test reveals a defect — it MUST report the finding rather than remediate it (Section 12).
2. **Performing the formal code-quality and rule-conformance review that gates merge, or rendering any recommendation on Gate 4 readiness.** That belongs to `review-code` (`SKILLS.md`, Section 12; `global_rules_revisionfinal_v10.md`, Section 7.4).
3. **Approving or rejecting the change at any HITL Gate.** `generate-tests`'s output is a set of test artifacts; it is not itself a Gate approval decision, which belongs to the human Engineering CEO (`FRAMEWORK_BLUEPRINT.md`, Section 1.2, Level 1).
4. **Defining or redefining the feature's scope.** Scope was fixed at Gate 3 (Scope Approval); `generate-tests` MUST NOT expand, narrow, or reinterpret that scope when deciding what to test (Section 9.1).
5. **Executing the generated test suite as part of a continuous-integration or release pipeline.** Running tests as a pipeline step is a Layer 5 / `CONTRIBUTING.md` concern; this Skill generates test artifacts, it does not operate a test-execution runtime.
6. Any cross-project operation, or any modification to a Layer 1–4 document (SK-003, `FRAMEWORK_BLUEPRINT.md` Section 10.4).
7. Selecting or changing the project's archetype, technology stack, or Layer 4 structural conventions, including the approved test runner. Those are already fixed by the time this Skill executes (Section 7).

---

## 6. Preconditions

`generate-tests` MUST NOT begin execution until all of the following are satisfied:

1. **Session initialization is complete.** The AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7) has been completed at least once in the current session.
2. **The project is bootstrapped.** Per `PROJECT_BOOTSTRAP_GUIDE.md`, Section 3, the project has passed Gate 1 (Plan Approval) and its scaffold has been validated against the applicable Layer 9 specification or `PROJECT_STRUCTURE.md`.
3. **A scoped feature definition exists.** Gate 3 (Scope Approval) has produced a feature scope definition (`FRAMEWORK_BLUEPRINT.md`, Section 9.2) that bounds what this invocation of `generate-tests` may test.
4. **Either of the two orderings named in Section 1 is satisfied:**
   - (a) **Typical ordering** — `create-feature`'s declared output already exists for the feature (`skill-create-feature.md`, Section 8); **or**
   - (b) **Reversed, test-first ordering** — `create-feature`'s Specification (its Stage 2) and, where already produced, Implementation Plan (its Stage 3) exist, so that this Skill can generate the failing test-first tests the TDD Boundary requires ahead of implementation.
   `generate-tests` MUST determine which ordering applies at Stage 1 (Section 9.1) rather than assume one.
5. **The matching Layer 4 Project Rule document is `Active`.** The document corresponding to the project's archetype (e.g., `project-pc-app_v04.md`) has been read in full for this task.
6. **`SKILLS.md` selection has occurred.** This Skill was selected through the two-part discovery filter — `primary-agent-role` match (Test Agent) and current Workflow Phase fit (`SKILLS.md`, Section 6) — not by name-matching alone.

If any precondition above is not satisfied, `generate-tests` MUST NOT execute. The gap MUST be reported per Section 12 (Failure Conditions) rather than worked around.

---

## 7. Input

Per `SKILLS.md`, Section 7, `generate-tests`'s declared input is ordinarily the business/application logic produced by `create-feature`, plus the applicable Layer 2 TDD Boundary zone(s) — with the ordering reversal described in Section 1 accommodated explicitly. The full input set is:

1. **`create-feature`'s declared output** for the feature under test (typical ordering), **or** **`create-feature`'s Specification and, where available, Implementation Plan artifacts** (reversed, test-first ordering) — per Section 6, item 4.
2. **The scoped feature definition** produced at Gate 3 (Scope Approval), bounding what this invocation may test (Section 5.2, item 4).
3. **The applicable Layer 2 TDD Boundary zone classification** (`global_rules_revisionfinal_v10.md`, Section 4) for every zone the feature touches — business/domain, application/use-case, infrastructure, and UI/framework-glue, as applicable.
4. **The matching Layer 4 Project Rule document** for the current project's archetype, supplying the test-directory layout and file-naming conventions this Skill's output must conform to (e.g., `project-pc-app_v04.md`, Sections 4–5, 9).
5. **`global_technology_stack_v10.md`**, applied through the matching Layer 4 document, for the approved test runner and testing tooling — `generate-tests` MUST NOT assume or introduce a test runner not already approved at Layer 3, consistent with the same discipline `project-pc-app_v04.md`, Section 8, already applies to test-runner selection.
6. **Any test artifacts already embedded in `create-feature`'s own test-first Implementation stage** (`skill-create-feature.md`, Section 9.4, item 1), so that this Skill extends coverage rather than duplicates it (Section 9.2, item 3).
7. **Any prior `DECISIONS.md` entries or recorded defect history** relevant to the feature's domain, required for Regression Test generation (Section 9.6) and to avoid silently re-litigating an already-settled architectural decision.

---

## 8. Output

Per `SKILLS.md`, Section 7 and Section 8, `generate-tests`'s output is, jointly with `create-feature`'s output, the natural input to `review-code`. The full output set is:

1. **Unit Tests** for business/domain logic and application/use-case orchestration (Section 9.3).
2. **Integration Tests** for infrastructure adapters (Section 9.4).
3. **End-to-End Test Plans**, together with automated end-to-end test stubs where applicable (Section 9.5).
4. **Regression Tests** guarding against the recurrence of a previously recorded defect, where one exists for the feature under test (Section 9.6).
5. **Edge Case tests** exercising boundary conditions (Section 9.7).
6. **Negative Test Cases** exercising invalid input and error paths (Section 9.7).
7. **Updated test-related documentation**, reflecting the newly generated coverage (Section 9.8, item 4).
8. **A flagged architectural-decision candidate**, where applicable (Section 9.8, item 5), for a human or a subsequent process step to record in `DECISIONS.md`. Per SK-003, `generate-tests` itself MUST NOT write to `DECISIONS.md` or any Layer 1–4 document — flagging it clearly is sufficient to satisfy PR-001 downstream, exactly as `create-feature` and `review-code` do for their own output.

`generate-tests`'s output is explicitly **not** considered ready for Gate 4 review on its own. Per `SKILLS.md`, Section 8, it is one of two inputs `review-code` consumes, the other being `create-feature`'s output.

---

## 9. Workflow

`generate-tests`'s `steps` field consists of nine ordered stages. Per `FRAMEWORK_BLUEPRINT.md`, Section 10.3, this Skill's `steps` describe only its own execution — they consume `create-feature`'s declared output or specification artifacts as input, but MUST NOT describe that Skill's internal procedure, and they produce output for `review-code` without describing that Skill's internal procedure either.

### 9.1 Stage 1 — Intake and Test Scope Analysis

1. The agent MUST determine, per Section 6, item 4, which ordering applies to this invocation: the typical ordering (`create-feature`'s output already exists) or the reversed, test-first ordering (this invocation runs ahead of, or concurrently with, `create-feature`'s Implementation stage).
2. The agent MUST identify which Layer 2 TDD Boundary zone(s) (`global_rules_revisionfinal_v10.md`, Section 4) the feature spans, mirroring the classification `create-feature` itself performs at its own Stage 1 (`skill-create-feature.md`, Section 9.1, item 1).
3. The agent MUST retrieve the original Gate 3 scope definition (Section 7, item 2) to bound the test scope — a test that exercises behavior outside the scoped feature's boundary is out of scope for this invocation (Section 5.2, item 4).
4. Where a required input from Section 7 is unavailable under either ordering, the agent MUST halt and report per Section 12 rather than proceed with a partial or assumed input set.

### 9.2 Stage 2 — Test Strategy and TDD Boundary Mapping

1. For every TDD Boundary zone identified in Stage 1, the agent MUST determine the required test rigor per `global_rules_revisionfinal_v10.md`, Section 4: business/domain logic MUST be covered test-first; application/use-case orchestration SHOULD be test-first and MUST have coverage; infrastructure adapters SHOULD have integration-level coverage; UI/framework-glue MAY be covered, with behavior-level tests preferred.
2. The agent MUST determine which of the six required test categories (Section 10) apply to which zone, so that the remaining stages (9.3–9.7) each know their scope precisely.
3. The agent MUST identify any test already embedded in `create-feature`'s own test-first Implementation stage (Section 7, item 6) and MUST plan this invocation's output to extend, not duplicate, that coverage.
4. The agent SHOULD state the strategy as a short, ordered list of planned test artifacts, each paired with its zone and category, so that the strategy itself is checkable rather than only aspirational — consistent with the goal-driven pattern named in Section 2.

### 9.3 Stage 3 — Unit Test Generation

1. The agent MUST generate unit tests for domain/business logic consistent with the TDD Boundary's test-first requirement for that zone (`global_rules_revisionfinal_v10.md`, Section 4, Zone 1).
2. The agent MUST generate unit tests for application/use-case orchestration consistent with that zone's SHOULD-test-first / MUST-coverage requirement (Zone 2).
3. Each unit test MUST exercise a single unit of behavior in isolation, without a database, network, or UI framework running, consistent with the Layer 2 requirement that business logic be unit-testable in isolation (`global_rules_revisionfinal_v10.md`, Section 3.1.1).
4. Test files MUST follow the matching Layer 4 archetype's test-naming convention (e.g., `project-pc-app_v04.md`, Section 5: a test file mirrors the file under test, suffixed `.test` or `.spec` per the Layer 3 test runner).
5. A unit test MUST NOT assert on an implementation detail that is not part of the unit's documented contract, consistent with the Constitution's Simplicity value and the reader-first principle (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 8; `global_rules_revisionfinal_v10.md`, Section 7.5).

### 9.4 Stage 4 — Integration Test Generation

1. The agent MUST generate integration-level tests for infrastructure adapters, consistent with the TDD Boundary's SHOULD-level recommendation for that zone (`global_rules_revisionfinal_v10.md`, Section 4, Zone 3) — for example, a persistence adapter tested against a real, temporary, file-based instance rather than a mock, consistent with `project-pc-app_v04.md`, Section 9's mapping of infrastructure tests to `tests/infrastructure/`.
2. An integration test MUST verify the adapter's conformance to its declared port/interface (`global_rules_revisionfinal_v10.md`, Sections 2.2–2.3), not the internal implementation detail of the underlying vendor SDK.
3. Where exercising a real external vendor dependency is not practical in the current development environment, the agent MUST substitute a narrowly-scoped, explicitly documented test double or a local/ephemeral instance, consistent with the vendor-independence rule (`global_rules_revisionfinal_v10.md`, Section 2) — the substitution itself MUST be stated in the test's accompanying documentation (Section 9.8, item 4) rather than left implicit.

### 9.5 Stage 5 — End-to-End Test Plan Generation

1. The agent MUST produce a structured End-to-End Test Plan documenting the user-facing behavioral scenarios the feature must satisfy, consistent with the TDD Boundary's stated preference for behavior-level tests in the UI/framework-glue zone (`global_rules_revisionfinal_v10.md`, Section 4).
2. Each scenario in the End-to-End Test Plan MUST state its preconditions, the user action sequence, and the expected observable outcome. A scenario MUST NOT assert on internal implementation state that is not observable through the feature's actual delivery surface.
3. Where the matching Layer 4 archetype defines an end-to-end test directory convention (e.g., `project-pc-app_v04.md`, Section 4: `tests/e2e/`), the agent SHOULD also produce automated end-to-end test stubs conforming to that location, to the extent the current development environment supports automated end-to-end execution. Where it does not, the documented test plan alone satisfies this stage.

### 9.6 Stage 6 — Regression Test Generation

1. Where the feature under test corresponds to, or is materially adjacent to, a previously recorded defect (e.g., referenced in a prior `DECISIONS.md` entry or an existing regression suite, Section 7, item 7), the agent MUST generate a regression test asserting the correct, fixed behavior at the specific point the original defect occurred, so that a future change reintroducing the same failure is caught automatically.
2. Where no defect history is retrievable for the feature under test — i.e., it is genuinely new functionality with no prior recorded regression — the agent MUST state this explicitly in the output rather than fabricate a defect history to justify a regression test that does not apply.
3. A generated regression test SHOULD cite, in an accompanying comment or equivalent documentation, the specific `DECISIONS.md` entry or defect reference it guards against, consistent with the durable-provenance pattern named in Section 2.

### 9.7 Stage 7 — Edge Case and Negative Test Case Generation

1. The agent MUST generate Edge Case tests exercising boundary conditions of the feature's inputs and state transitions (e.g., empty collections, minimum/maximum numeric bounds, boundary dates, or concurrent-access conditions, where relevant to the feature under test).
2. The agent MUST generate Negative Test Cases exercising invalid input and the feature's expected error paths, consistent with the Layer 2 requirement that input from outside the application's trust boundary be validated before use in business logic (`global_rules_revisionfinal_v10.md`, Section 9.3).
3. A Negative Test Case MUST verify that an invalid input produces a well-defined, documented failure mode — e.g., a specific domain exception, or a structured error response consistent with `project-pc-app_v04.md`, Section 6, for the Desktop archetype's IPC error-handling pattern where applicable — rather than an unhandled crash or a silently swallowed failure (`global_rules_revisionfinal_v10.md`, Section 11.4).
4. The agent MUST NOT generate an edge case or negative test that falls outside the feature's actual scoped boundary (Section 7, item 2). A hypothetically interesting but out-of-scope case MUST be reported as an observation rather than encoded as a required test.

### 9.8 Stage 8 — Self Review and Documentation Update

1. Before declaring its output complete, the agent MUST self-check the generated test suite against every applicable item of the Layer 2 Definition of Done (`global_rules_revisionfinal_v10.md`, Section 10).
2. The agent MUST verify that no generated test duplicates a test already embedded in `create-feature`'s own test-first implementation (Section 9.2, item 3).
3. The agent MUST verify that every generated test is deterministic and does not depend on unmanaged external state (e.g., wall-clock time, network availability, ambient environment configuration) unless that dependency is itself the documented subject of the test.
4. The agent MUST update any documentation made inaccurate, or newly required, by the generated tests (e.g., a contract document's stated behavior, or a note on a substituted test double per Section 9.4, item 3) in the same change (`global_rules_revisionfinal_v10.md`, Section 8.5).
5. If, during Stages 3–7, the agent identified a point where an architectural-decision candidate was surfaced (e.g., an untestable design revealing a missing seam), the agent MUST flag this explicitly as a `DECISIONS.md` candidate in its output. Per SK-003, `generate-tests` MUST NOT modify `DECISIONS.md` or any Layer 1–4 document itself.

### 9.9 Stage 9 — Gate Check and Handoff

1. The agent MUST perform a final self-verification that the combined output of Stages 1–8 is complete, internally consistent, and ready to be handed to `review-code` jointly with `create-feature`'s output, consistent with the reference Skill Composition in `SKILLS.md`, Section 8.
2. This "Gate Check" step is an internal readiness check, not the Skill-level `gate` metadata field. Because `generate-tests`'s `gate` field is `none` (Section 3), completing this step does **not** itself trigger a human approval pause. The actual HITL checkpoint for this Workflow Phase occurs later, at Gate 4 (Implementation Approval), against the *combined* output of `create-feature` → `generate-tests` → `review-code`, per `SKILLS.md`, Section 8. This distinction MUST be preserved by any agent executing this Skill and MUST NOT be described, reported, or logged as if Gate 4 had already occurred.
3. If Stage 8 (Self Review) or this stage surfaces a Definition-of-Done failure, the agent MUST return to the relevant earlier stage (most commonly Stage 3, 4, 6, or 7, depending on where the failure traces to) rather than presenting incomplete or non-conforming output as finished.
4. Only once this stage passes without an open failure MUST the agent present its output as the completed result of `generate-tests` and proceed to hand off, jointly with `create-feature`'s output, to `review-code`.

---

## 10. Test Category Taxonomy

**Scope note.** Framework v10 does not define a new test-category schema, a new HITL Gate, or a new Skill Metadata field for test artifacts. `BLUEPRINT_INPUT_FREEZE.md` contains no such decision, and none is introduced here. What follows is a documentation taxonomy organizing the six test categories the Owner's requirements name (Unit, Integration, End-to-End, Regression, Edge Case, Negative) against the already-existing Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4) and the matching Layer 4 archetype's already-existing test-directory conventions (e.g., `project-pc-app_v04.md`, Sections 4 and 9). It changes nothing about the binding force of any MUST/SHOULD/MAY rule cited elsewhere in this document; it only organizes this Skill's declared `output` field (Section 8) for legibility.

| Category | Definition | TDD Boundary zone(s) primarily addressed | Typical location (per the matching Layer 4 archetype's test-directory convention) |
|---|---|---|---|
| **Unit Tests** | Tests exercising a single unit of business/domain or application/use-case logic in isolation, without a database, network, or UI framework running. | Business/domain (MUST test-first); Application/use-case (SHOULD test-first, MUST coverage) | `tests/domain/`, `tests/application/` (e.g., `project-pc-app_v04.md`, Section 4) |
| **Integration Tests** | Tests exercising an infrastructure adapter against a real or realistic dependency instance, verifying conformance to its declared port. | Infrastructure adapters (SHOULD integration-level) | `tests/infrastructure/` |
| **End-to-End Test Plans** | Structured, documented behavioral scenarios — plus automated stubs where the environment supports them — exercising the feature as a user would experience it through its actual delivery surface. | UI/presentation, CLI wiring, framework glue (MAY, behavior-level preferred) | `tests/e2e/` |
| **Regression Tests** | Tests asserting correct, fixed behavior at the specific point a previously recorded defect occurred, guarding against its recurrence. | Whichever zone the original defect occurred in | Co-located with that zone's other tests, cross-referenced to the defect or `DECISIONS.md` entry (Section 9.6, item 3) |
| **Edge Case Tests** | Tests exercising boundary conditions of a feature's inputs or state transitions. | Any zone with a defined boundary condition | Co-located with that zone's other tests |
| **Negative Test Cases** | Tests exercising invalid input and a feature's expected error or failure paths. | Any zone receiving input across a trust boundary | Co-located with that zone's other tests |

A single generated test file MAY satisfy more than one category simultaneously (for example, a unit test that is also, incidentally, an edge case test); this taxonomy classifies test *intent*, not a mandatory one-file-per-category structure. Where a category genuinely does not apply to a given invocation (most commonly Regression Tests, where no defect history exists per Section 9.6, item 2), the agent MUST state this explicitly in the output rather than omit the category silently, consistent with the same completeness discipline `review-code` applies to its own review dimensions (`skill-review-code.md`, Section 9.8, item 4).

---

## 11. Completion Criteria

`generate-tests`'s output is complete if and only if every item below holds. This list operationalizes the Layer 2 Definition of Done (`global_rules_revisionfinal_v10.md`, Section 10) for this Skill specifically; it does not replace or narrow it.

| # | Criterion |
|---|---|
| 1 | The generated tests address exactly the feature scoped at Gate 3 — no more, no less (Sections 5.2, 9.1). |
| 2 | Every TDD Boundary zone the feature touches has been mapped to the applicable test categories, per Section 10 (Section 9.2). |
| 3 | Unit Tests, Integration Tests, End-to-End Test Plans, Regression Tests (or an explicit "not applicable" statement), Edge Cases, and Negative Test Cases have each been addressed (Sections 9.3–9.7). |
| 4 | No generated test duplicates a test already embedded in `create-feature`'s own test-first implementation (Section 9.2, item 3; Section 9.8, item 2). |
| 5 | Every generated test is deterministic and does not depend on unmanaged external state without that dependency being the documented subject of the test (Section 9.8, item 3). |
| 6 | Every Negative Test Case verifies a well-defined, documented failure mode rather than an unhandled crash or a silently swallowed failure (Section 9.7, item 3). |
| 7 | Documentation affected by the newly generated tests has been updated in the same change (Section 9.8, item 4). |
| 8 | Any architectural-decision candidate surfaced during test generation is explicitly flagged in the output, not silently absorbed (Section 9.8, item 5). |
| 9 | Stage 9 (Gate Check) passed without an unresolved Definition-of-Done failure (Section 9.9). |
| 10 | The output is structured such that `review-code` can consume it directly, jointly with `create-feature`'s output, without requiring the human or the next agent to reconstruct missing context (Section 8). |

An agent executing `generate-tests` MUST self-check its output against this table before presenting it as complete, consistent with the equivalent self-check requirement already stated at Layer 2 (`global_rules_revisionfinal_v10.md`, Section 10, closing paragraph) and already applied by `create-feature` and `review-code` to their own output.

---

## 12. Failure Conditions and Escalation

`generate-tests` MUST NOT attempt to silently work around any of the following. Each is a stop condition requiring the agent to report the gap and halt this Skill's execution rather than proceed on an assumption:

1. **A precondition in Section 6 is not satisfied** (e.g., neither the typical nor the reversed ordering's required input exists for the feature under test). The agent MUST report which precondition is unmet and MUST NOT begin Stage 1.
2. **The TDD Boundary zone classification cannot be determined for part of the feature** (Section 9.1, item 2). The agent MUST report this rather than guess a zone and apply the wrong rigor level to it.
3. **A generated test reveals a defect in the implementation under test.** This is not a condition this Skill remediates (Section 5.2, item 1); the agent MUST report the finding — including the specific failing test and the behavior it exposes — for `create-feature` to address in a subsequent invocation, rather than modify the implementation itself.
4. **The matching Layer 4 Project Rule document is not `Active`** (e.g., the project's archetype is Full-Stack or Monolithic, whose Layer 4 documents are `Pending` per `FRAMEWORK_README.md`, Section 4). The agent MUST report this as a standing Tier gap and MUST NOT improvise test-directory structure or naming conventions from a deprecated legacy document.
5. **A Regression Test appears warranted but no defect history can be retrieved, and it is unclear whether the feature is genuinely new or an undocumented bug fix.** The agent MUST report this ambiguity explicitly (Section 9.6, item 2) rather than fabricate a defect history in either direction.
6. **The feature, or a test scenario it implies, appears to require modifying a Layer 1–4 document, or performing a cross-project operation.** Per SK-003, this is not a valid `generate-tests` execution. It MUST be escalated as a Gate 2 (Architecture Approval) proposal and, if approved, recorded in `DECISIONS.md` — never worked around by narrowing the test's scope silently.
7. **Stage 8 (Self Review) or Stage 9 (Gate Check) surfaces a Definition-of-Done failure that cannot be resolved by returning to an earlier stage within this same invocation.** The agent MUST report the specific failing criterion from Section 11 rather than present non-conforming output as complete.

In every case above, escalation means: the agent reports the gap plainly, states which layer, Skill, or gate the resolution belongs to, and stops — consistent with the same escalation discipline `PROJECT_BOOTSTRAP_GUIDE.md` (Section 5), `SKILLS.md` (Section 6), `skill-create-feature.md` (Section 11), and `skill-review-code.md` (Section 12) already apply elsewhere in this framework.

---

## 13. `framework-alignment` (SK-002, Required Field)

This Skill's execution is bound to the following governing-layer rules, cited by reference and not restated:

| Layer | Document | Sections this Skill's execution is bound by |
|---|---|---|
| 1 | `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | Core value ordering (Section 4); Simplicity (Section 8); AI Readability (Section 16) |
| 2 | `global_rules_revisionfinal_v10.md` | Test-Driven Development Boundary (Section 4); Code Quality Gates (Section 7); Documentation Standards (Section 8); Security Baselines (Section 9, for Negative Test Case input-validation checks); Definition of Done (Section 10); Exception Handling Rule (Section 11, for well-defined negative-path failure modes) |
| 3 | `global_technology_stack_v10.md` | The approved test runner and testing tooling, applied through whichever Layer 4 document is in effect — this Skill introduces no test-tooling selection of its own |
| 4 | The matching Project Rule document for the current archetype (e.g., `project-pc-app_v04.md`) | Test-directory layout and naming conventions (Sections 4–5), the TDD-Boundary-to-directory mapping (Section 9), and the archetype's error-handling pattern relevant to Negative Test Cases (e.g., Section 6, IPC error handling, for the Desktop archetype) |

This Skill's own `steps` (Section 9) MUST NOT be read as adding to, narrowing, or reinterpreting any rule in the table above, and the Test Category Taxonomy (Section 10) MUST NOT be read as altering the RFC 2119 force of any rule it references. Where a step above appears to state a rule not already present in one of these documents, that step is in error and MUST be corrected to a reference (per the framework's inheritance model, `FRAMEWORK_BLUEPRINT.md`, Section 5).

---

## 14. Related Skills

Per `SKILLS.md`, Sections 7 and 8:

| Skill | Relationship to `generate-tests` |
|---|---|
| `create-feature` (Active) | Typical predecessor — `generate-tests` consumes its output as declared input (Section 7). Under the reversed, test-first ordering (Section 1), `generate-tests` instead consumes `create-feature`'s Specification/Implementation Plan artifacts ahead of that Skill's own Implementation stage, supplying the failing tests that stage's test-first requirement depends on. |
| `review-code` (Active) | Successor. Consumes the combined output of `create-feature` and `generate-tests` as its declared input (`skill-review-code.md`, Section 7). A rejected Gate 4 review returns control to `create-feature`, not to `generate-tests` directly, per `SKILLS.md`, Section 8. |

---

## 15. Related Documents

| Document | Relevance |
|---|---|
| `FRAMEWORK_BLUEPRINT.md` | Master structural authority for this Skill's classification, metadata schema, Workflow Phase concept, and HITL Gate positions (Sections 8–10). |
| `SKILLS.md` | The Layer 7 Manifest this document's entry belongs to; the source of the Skill Dependencies (Section 7) and Skill Composition (Section 8) this document's Sections 1 and 14 apply, including the explicit ordering-reversal note this document accommodates. |
| `global_rules_revisionfinal_v10.md` | Layer 2 source of the TDD Boundary (Section 4) this Skill's entire workflow is organized against, and of every other binding rule this Skill's output must satisfy. |
| `global_technology_stack_v10.md` | Layer 3 source of the approved test runner and testing tooling this Skill applies through the Layer 4 document. |
| The matching Layer 4 Project Rule document (e.g., `project-pc-app_v04.md`) | Supplies the test-directory layout, naming conventions, and archetype-specific error-handling pattern this Skill's output must conform to. |
| `skill-create-feature.md` | The full specification of this Skill's typical predecessor, including the precise boundary (Section 5.2, item 1; Section 9.4, item 1) between what `create-feature` tests inline and what `generate-tests` is responsible for. |
| `skill-review-code.md` | The full specification of this Skill's successor, including the Test Coverage Verification stage (`skill-review-code.md`, Section 9.6) that checks this Skill's output. |
| `PROJECT_BOOTSTRAP_GUIDE.md` | Governs the preconditions in Section 6 — a project must be bootstrapped before this Skill may execute. |
| `DECISIONS.md` | The Layer 10 destination for any architectural-decision candidate this Skill's execution flags (Section 8, item 8), and the source of defect/decision history this Skill's Regression Test generation consults (Section 9.6). |
| `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | Layer 1 source of the value ordering that resolves any ambiguity in how this Skill's rules should be read (Section 13). |

---

## 16. Authority and Conflict Resolution

1. Per the Primary Authority ordering stated in the header, and per the downward authority rule (KA-002), this document MUST NOT contradict `FRAMEWORK_BLUEPRINT.md`, `SKILLS.md`, or `global_rules_revisionfinal_v10.md`, in that order of precedence. Where this document appears to conflict with any of the three, the higher-priority document wins and this document MUST be corrected.
2. None of the reference materials named in the header (Section 2) carries any authority over this document. A pattern drawn from them MUST NOT be cited as justification for a rule that conflicts with any Primary Authority document.
3. Where a future amendment to `skill-create-feature.md` or `skill-review-code.md`, once authored, appears to require this document's `steps` to change, that change MUST follow the amendment procedure in Section 17 rather than being made unilaterally in either document.
4. The full conflict-resolution procedure, including same-layer conflicts between two operational-layer artifacts, is defined in `FRAMEWORK_BLUEPRINT.md`, Section 6, and is not restated here.

---

## 17. Change Control

1. This document MUST NOT be edited silently to change what `generate-tests` does. A change to its `input`, `output`, `steps`, `gate`, `framework-alignment`, or Test Category Taxonomy (Section 10) is a change to a frozen Skill specification and MUST follow the amendment procedure defined in `FRAMEWORK_BLUEPRINT.md`, Section 18.2: proposal → Gate 2 (Architecture Approval) → amendment → `DECISIONS.md` entry → `FRAMEWORK_HANDOVER.md` update in the same commit.
2. Upon this document's creation, `SKILLS.md`, Section 12, MUST be updated in the same change: the **Document Status** for `generate-tests` MUST move from `Pending — not yet generated` to `Active`, and Footnote 1's provisional `gate: none` reasoning for `generate-tests` MUST be marked as reconciled and authoritative per Section 3 of this document, per `SKILLS.md` Section 18.2.
3. `SKILLS.md`, Section 9 (Skill Lifecycle), MUST be updated in the same change to reflect that `generate-tests` has entered the `Active` state, per the same section's stated diagram.
4. **Layer 7 completion.** Per `FRAMEWORK_BLUEPRINT.md`, Section 2.7, Layer 7 is `Active` "once the manifest and three starter Skills exist." With `skill-create-feature.md` and `skill-review-code.md` already `Active` and this document now completing the third and final Tier 1 starter Skill, all four required Layer 7 artifacts — `SKILLS.md`, `skill-create-feature.md`, `skill-review-code.md`, and `skill-generate-tests.md` — now exist. Layer 7 as a whole therefore transitions to `Active` upon this document's creation. This is a status transition mechanically determined by an already-frozen criterion (`FRAMEWORK_BLUEPRINT.md`, Section 2.7), not a new architectural decision. `FRAMEWORK_README.md`, Sections 4.1, 4.6, and 5, and `FRAMEWORK_STATUS.md` MUST be updated in the same change to reflect this — including resolving the "SKILLS.md + 3 starter Skills" bundle line item that Flags 3, 5, and 6 in `FRAMEWORK_STATUS.md` have tracked across the framework's mid-migration state.

---

## Closing Statement

This document is the full, seven-field specification of `generate-tests`, the second Skill of Framework v10's Development Workflow Phase. It defines a nine-stage workflow — Intake and Test Scope Analysis, Test Strategy and TDD Boundary Mapping, Unit Test Generation, Integration Test Generation, End-to-End Test Plan Generation, Regression Test Generation, Edge Case and Negative Test Case Generation, Self Review and Documentation Update, and Gate Check and Handoff — producing the full breadth of test coverage the Owner's requirements name (Unit, Integration, End-to-End, Regression, Edge Case, and Negative Test Cases), organized by the Test Category Taxonomy of Section 10, itself scoped explicitly as a documentation lens over the already-frozen Layer 2 TDD Boundary rather than new architecture. It resolves `SKILLS.md`'s provisional `gate: none` assignment for `generate-tests` into an authoritative declaration, and explicitly accommodates the dependency-ordering reversal `SKILLS.md`, Section 7, already anticipates between this Skill and `create-feature`. With this document's creation, all four artifacts `FRAMEWORK_BLUEPRINT.md`, Section 2.7, requires for Layer 7 completeness — the Manifest and all three starter Skill documents — now exist, and Layer 7 transitions to `Active`.
