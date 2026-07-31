# global_rules_revisionfinal_v10.md

**Status:** Active
**Layer:** 2 — Framework Rules
**Framework Version:** v10
**Purpose:** Translate the principles of `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` into universal, enforceable, technology-agnostic engineering rules that apply to every project created under this framework, regardless of application archetype (Layer 4) or technology selection (Layer 3).
**Authority:** This is the single Layer 2 document (`FRAMEWORK_BLUEPRINT.md`, Section 2.2). It is the framework's highest-priority Tier 1 deliverable (DI-001); until this document exists, no Layer 4 Project Rule document may be treated as fully load-bearing, because every Layer 4 document inherits from it (`FRAMEWORK_BLUEPRINT.md`, Section 17).
**Inherits from:** `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` (Layer 1 — Constitution). No other document is an input to this layer.
**Governs:** Layers 3–11, per the downward authority rule (KA-002, `FRAMEWORK_BLUEPRINT.md` Section 6). A Project Rule (Layer 4) cannot override a rule stated in this document. An AI Skill (Layer 7) cannot contradict it. This precedence is absolute and admits no exception based on context, urgency, or convenience.
**Supersedes:** `global_rules_revisionfinal_v09_(Source of Truth).md` and `global_rules_revisionfinal_v09_(prompt).md`, both `Deprecated` as of the v10 freeze (DL-002).
**RFC 2119:** The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in RFC 2119. They apply to every rule below without exception unless the rule itself states otherwise.

---

## 0. Scope and Position in the Knowledge Architecture

This document is the **only** Layer 2 artifact in Framework v10 (`FRAMEWORK_BLUEPRINT.md`, Section 2.3 of the Document Hierarchy). It sits directly beneath the Constitution and directly above the Technology Standards:

```
Layer 1 — Constitution
        ↓ (this document inherits from Layer 1 only)
Layer 2 — Framework Rules  (this document)
        ↓ (Layers 3–11 inherit from this document by reference, never by restatement)
Layer 3 — Technology Standards
Layer 4 — Project Rules
Layer 5 — Developer Manuals
Layer 6 — Reference Implementations
Layer 7 — AI Skills
Layer 8 — Prompt Library
Layer 9 — Project Templates
Layer 10 — Knowledge Base
Layer 11 — Reference Documents
```

**What this document contains.** RFC-level rules for architecture, the TDD boundary, Git workflow, environment and dependency management principles, code quality gates, documentation standards, security baselines, and the definition of "done" for a commit — all stated independently of any specific technology.

**What this document does not contain, by design.** Per its Layer 2 prohibited responsibilities (`FRAMEWORK_BLUEPRINT.md`, Section 2.2):

- It **MUST NOT** name a specific framework, library, testing tool, or package manager. Those selections, and their approved alternatives, belong to `global_technology_stack_v10.md` (Layer 3).
- It **MUST NOT** define a project-folder layout, naming convention for files/classes, or archetype-specific technical decision. Those belong to the relevant Layer 4 Project Rule document.

A rule stated here that appears to require a specific technology has been misapplied and MUST be corrected; the correct location for a technology-bound version of that rule is Layer 3 or Layer 4.

**How an AI agent uses this document.** This is the layer an AI agent consults for any engineering decision that is not technology-specific — whether to catch a broad exception, whether a commit message is well-formed, whether a comment explains *why* rather than merely *what*. It **MUST** be read in full at the start of any coding task (Section 14 / Section 7 of the session-initialization sequences in `FRAMEWORK_BLUEPRINT.md` / `FRAMEWORK_README.md`) and referenced continuously during execution, not read once and discarded.

---

## 1. Relationship to the Constitution

Every rule in this document **MUST** be consistent with the value-priority ordering defined in `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 4. This document does not restate that ordering — restating a parent-layer rule instead of referencing it violates the framework's inheritance model (`FRAMEWORK_BLUEPRINT.md`, Section 5) — but every rule below is traceable to it. Where a rule in this document could be read two ways, the reading that better serves maintainability over trend **MUST** be preferred, consistent with the Constitution's Final Principle.

If a future engineering need appears to conflict with a rule in this document, that conflict **MUST NOT** be resolved by silently ignoring the rule. It **MUST** be raised as a Gate 2 (Architecture Approval) proposal and, if approved, recorded in `DECISIONS.md` per the procedure in Section 13 of this document.

---

## 2. Vendor Independence (EP-001)

Vendor independence is stated in the Constitution as a philosophical value. In this document it becomes a **binding rule**, not an aspiration:

1. Business logic **MUST NOT** depend directly on a specific cloud provider, database engine, vector store, LLM provider, embedding provider, or storage provider's SDK or API surface.
2. Every external dependency on the categories listed above **MUST** be accessed through an interface (abstraction / port) owned by the application, not through a vendor's concrete client type leaking into domain code.
3. Replacing a vendor in any of the categories above **MUST** require changes confined to the infrastructure/adapter layer. If replacing a vendor requires changes to business logic, the abstraction boundary has failed and **MUST** be corrected before the change is considered complete.
4. A project **MAY** ship with only one implementation of a given interface. The rule requires the *seam* to exist, not that every alternative be built in advance.
5. An AI agent introducing a new external dependency in any of the categories above **MUST** place it behind an interface as part of the same change, not as a follow-up task.

---

## 3. Architecture Rules

### 3.1 Separation of Concerns

1. Domain/business logic **MUST** be separable from framework code, transport code (HTTP/CLI/UI), and persistence code. It **MUST** be possible to unit-test business logic without a database, network, or UI framework running.
2. Each module, class, or function **SHOULD** have a single, clearly stated responsibility (Single Responsibility Principle). A unit that requires the word "and" to describe its purpose is a candidate for splitting.
3. Dependencies **MUST** point from concrete/infrastructure code toward abstract/domain code, never the reverse (Dependency Inversion Principle). Domain code **MUST NOT** import infrastructure code.

### 3.2 Clean Architecture Boundaries

1. A project **MUST** maintain a discernible boundary between (a) domain/business rules, (b) application/use-case orchestration, and (c) infrastructure/framework/delivery mechanisms. The exact folder realization of this boundary is a Layer 4 concern; the *existence* of the boundary is a Layer 2 requirement.
2. Code on the inner (domain) side of this boundary **MUST NOT** reference code on the outer (infrastructure) side. Communication outward **MUST** occur through interfaces defined on the inner side and implemented on the outer side.
3. New abstractions **MUST** solve a real, currently existing problem. Introducing an interface, factory, or layer of indirection with only one plausible implementation and no anticipated second implementation **SHOULD NOT** be done merely for architectural symmetry.

### 3.3 Extensibility Without Premature Complexity

1. Extension points (interfaces, plugin seams, configuration hooks) **SHOULD** be added when a second variant is known or reasonably anticipated, not preemptively for every component.
2. Enterprise patterns (e.g., generic repository frameworks, elaborate factory hierarchies, speculative plugin systems) **MUST NOT** be introduced unless they clearly reduce long-term maintenance cost for the project's actual, stated scope.

---

## 4. Test-Driven Development Boundary (EP-002)

Framework v10 does not require universal, unconditional TDD. It defines a boundary for where test-first development is required and where it is not:

| Zone | TDD Requirement |
|---|---|
| Business/domain logic (rules, calculations, state transitions, validation) | **MUST** be developed test-first. A failing test **MUST** exist before the corresponding implementation is written. |
| Application/use-case orchestration (coordinating domain logic and ports) | **SHOULD** be developed test-first; **MUST** have test coverage before a commit is considered done (Section 10). |
| Infrastructure adapters (database access, external API clients) | **SHOULD** have integration-level tests; test-first is **RECOMMENDED** but not mandatory. |
| UI/presentation code, CLI wiring, and framework glue | **MAY** be developed without a strict test-first cycle. Tests, where they exist, **SHOULD** favor behavior over implementation detail. |
| Exploratory prototypes, throwaway scripts, generated scaffolding not intended to reach production | **MAY** be written without tests. Such code **MUST NOT** be merged into a project's production path without first being brought inside the applicable zone above. |

This boundary exists so that testing rigor is concentrated where correctness risk and long-term maintenance cost are highest (business logic), consistent with the Constitution's priority of maintainability over premature optimization of effort.

---

## 5. Git Workflow Rules

These rules are the Layer 2 rules that `CONTRIBUTING.md` (Layer 5) operationalizes procedurally; `CONTRIBUTING.md` **MUST** reference these rules rather than restate them.

1. Commit messages **MUST** follow the Conventional Commits format (`type(scope): description`), so that history remains machine-parseable and human-scannable across many years of a project's life.
2. A commit **MUST** represent one logical, coherent change. Unrelated changes **MUST NOT** be combined into a single commit.
3. Direct commits to a project's primary integration branch **MUST NOT** occur for non-trivial changes; work **MUST** proceed on a separate branch and be integrated through a reviewed change (pull/merge request).
4. A change that alters or introduces an architectural decision **MUST** be accompanied, in the same change, by a corresponding `DECISIONS.md` (Layer 10) entry, per the mandatory co-update rule (PR-001).
5. A change **MUST NOT** be merged into the primary integration branch until it satisfies the Definition of Done in Section 10 and, where applicable, has passed Gate 4 (Implementation Approval), per `FRAMEWORK_BLUEPRINT.md` Section 9.2.
6. Branch names **SHOULD** be descriptive of intent (e.g., the change type and a short subject) so that an AI agent or human scanning branch history can infer purpose without opening each branch.

---

## 6. Environment and Dependency Management Principles

These are principles, not tool selections; the specific package manager, virtualization approach, and lockfile format are Layer 3 decisions.

1. Every project **MUST** declare its dependencies explicitly and completely. Undeclared, ambient, or "works on my machine" dependencies **MUST NOT** be relied upon.
2. Dependency versions **MUST** be pinned or lock-filed such that a fresh environment build is reproducible byte-for-byte in behavior, not merely "close enough."
3. Development environments **MUST** be reproducible across machines (per the Constitution's Development Environment principle) — a project **MUST NOT** require undocumented manual setup steps that exist only in one contributor's memory.
4. Adding a new dependency **MUST** be a deliberate decision weighed against the cost of maintaining it; a dependency that duplicates functionality already available **SHOULD NOT** be added without justification.
5. Dependencies **SHOULD** be reviewed periodically for known vulnerabilities and unmaintained status; an unmaintained dependency in a security-relevant path **MUST** be flagged for replacement rather than left in place indefinitely.

---

## 7. Code Quality Gates

1. Code **MUST** pass automated linting and, where the language supports it, static type checking before being considered done.
2. Generic exception handling is restricted (see Section 8, Exception Handling Rule) as a specific, binding instance of this quality gate.
3. Dead code — code with no reachable caller and no documented future purpose — **MUST NOT** be left in the codebase; it **MUST** be removed rather than commented out.
4. Every non-trivial change **MUST** pass a code review step before merge, whether performed by a human or by an AI agent operating under the `review-code` Skill (Layer 7), with human confirmation remaining authoritative at Gate 4.
5. Code **SHOULD** be written for the reader first and the machine second: a clear, explicit implementation is preferred over a clever, compact one, consistent with the Constitution's Simplicity value.

---

## 8. Documentation Standards

1. Documentation **MUST** explain *why* a decision was made before, or in addition to, explaining *how* the code works. Comments that restate what the code already makes obvious **SHOULD** be avoided; comments that capture non-obvious intent **MUST** be preserved.
2. Every project **MUST** contain sufficient documentation for another developer — or another AI session with no prior context — to continue development without requiring a live explanation from the original author.
3. An architectural decision **MUST** be recorded with its context, the options considered, the decision made, and the rationale — not merely the conclusion — consistent with the required fields of a `DECISIONS.md` entry (PR-002).
4. Public interfaces (functions, classes, modules intended for reuse) **SHOULD** carry documentation describing their contract: inputs, outputs, and side effects.
5. Documentation **MUST** be updated in the same change that makes it inaccurate. Documentation debt **MUST NOT** be deferred as a separate, unscheduled task.

---

## 9. Security Baselines

1. Secrets (credentials, API keys, tokens, connection strings) **MUST NOT** be committed to version control in any form, including in comments, test fixtures, or commit history.
2. Every project **MUST** provide an example/template for required environment configuration (e.g., an environment-variable template) that contains no real secret values.
3. Input from outside the trust boundary of the application (user input, external API responses, file uploads) **MUST** be validated before use in business logic.
4. Components **SHOULD** be granted the minimum privilege necessary to perform their function (least privilege), rather than broad, undifferentiated access.
5. A known vulnerability in a dependency that is reachable from a security-relevant code path **MUST** be treated as a defect requiring remediation, not as background noise to be tolerated indefinitely.

---

## 10. Definition of Done (Commit-Level)

A change is **not** done — regardless of what the author believes — until every applicable item below is satisfied. This definition is intentionally independent of any specific technology stack.

| # | Requirement |
|---|---|
| 1 | The change addresses one coherent, logical unit of work (Section 5.2). |
| 2 | Business/domain logic touched by the change has test coverage consistent with the TDD Boundary (Section 4). |
| 3 | Automated linting and, where applicable, type checking pass (Section 7.1). |
| 4 | No generic/broad exception handling has been introduced without explicit justification (Section 11). |
| 5 | No dead code has been introduced (Section 7.3). |
| 6 | No secret, credential, or sensitive value has been introduced into version control (Section 9.1). |
| 7 | Documentation affected by the change has been updated in the same change (Section 8.5). |
| 8 | The commit message follows Conventional Commits (Section 5.1). |
| 9 | If the change introduces or alters an architectural decision, a corresponding `DECISIONS.md` entry has been appended in the same change (Section 5.4). |
| 10 | The change has passed code review, and — where its output requires it per the Skill's declared `gate` field — the applicable HITL Gate approval (`FRAMEWORK_BLUEPRINT.md`, Section 9.3). |

An AI agent **MUST** self-check a change against this table before presenting it as complete. A human reviewer **MAY** still reject a change that satisfies every item here for reasons specific to the project; this table is a floor, not a ceiling.

---

## 11. Exception Handling Rule

1. Code **MUST NOT** catch a generic or base exception type (e.g., a bare `Exception`/`BaseException`-equivalent in the implementation language) without either (a) immediately re-raising after performing a narrowly scoped action such as logging or resource cleanup, or (b) an explicit, in-code justification comment explaining why no narrower exception type is appropriate at that point.
2. Exception handling **SHOULD** target the narrowest exception type that correctly describes the failure being handled.
3. Domain-specific error conditions **SHOULD** be represented by custom, named exception types rather than reused generic ones, so that failure modes are self-documenting and machine-distinguishable.
4. Silently swallowing an exception (catching it and taking no action, including no logging) **MUST NOT** be done.

---

## 12. Authority and Conflict Resolution

1. Per the framework's downward authority rule (KA-002), this document takes precedence over every Layer 3–11 artifact in the event of conflict. A Layer 4 Project Rule, a Layer 7 Skill, or any other lower-priority (higher-numbered) artifact that conflicts with a rule in this document is in error and **MUST** be corrected, or a Gate 2 proposal **MUST** be raised to amend this document instead.
2. This document itself **MUST** remain consistent with `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` (Layer 1). If a future need appears to require a rule that conflicts with the Constitution, the conflict **MUST** be escalated as a Gate 2 decision against the Constitution, not resolved by quietly deviating from it here.
3. The full conflict-resolution procedure, including same-layer conflicts and deprecated-document handling, is defined in `FRAMEWORK_BLUEPRINT.md`, Section 6, and is not restated here.

---

## 13. Change Control

1. This document **MUST NOT** be edited silently. A change to any rule in this document is an architectural decision and **MUST** follow the amendment procedure defined in `FRAMEWORK_BLUEPRINT.md`, Section 18.2: proposal → Gate 2 (Architecture Approval) → amendment → `DECISIONS.md` entry → `FRAMEWORK_HANDOVER.md` update in the same commit.
2. Upon this document's own creation or any subsequent amendment, `FRAMEWORK_README.md` Sections 4–6 **MUST** be updated in the same change to reflect the new state, per that document's Section 9 rule that it is a mirror of the framework's actual state, not an independent source of truth.
3. Once this document exists and is `Active`, `V10_MIGRATION_NOTES.md` (Layer 11) **MUST NOT** be treated as a substitute source of binding rule authority for any question this document resolves, even though `V10_MIGRATION_NOTES.md` may remain `Active` for other, still-unresolved gaps until every Tier 1 and Tier 2 document exists (DL-003).

---

## Closing Statement

This document is the single Layer 2 artifact of Framework v10. It resolves the framework's previous highest-priority gap: with this document `Active`, `project-pc-app_v04.md` (Layer 4) and every subsequent Layer 4 Project Rule document may now be authored on a correct foundation, since each inherits its Layer 2 rules from here by reference rather than restatement. No architectural decision has been introduced by this document beyond what `FRAMEWORK_BLUEPRINT.md` Section 2.2 specifies as this layer's responsibility; every rule above is a technology-agnostic operationalization of the Constitution's principles, scoped strictly to what Layer 2 owns.
