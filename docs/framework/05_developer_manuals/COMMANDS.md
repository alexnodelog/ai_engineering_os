# COMMANDS.md

**Status:** Active
**Layer:** 5 — Developer Manuals
**Framework Version:** v10
**Tier:** 2 (Required)
**Purpose:** Serve as the single canonical command reference for Framework v10 (`FRAMEWORK_BLUEPRINT.md`, Section 2.5) — the lookup document a human developer or an AI agent consults for the exact command-line invocation of an operation already authorized by Layers 1–4, so that command syntax is looked up once, correctly, rather than reconstructed or guessed at each time it is needed.
**Authority:** Structural derivative of `FRAMEWORK_BLUEPRINT.md`, Sections 2.5 and 17. This document introduces no architectural decision of its own. Every command below operationalizes a tool, workflow, or convention already named and frozen at Layer 1 (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`), Layer 2 (`global_rules_revisionfinal_v10.md`), Layer 3 (`global_technology_stack_v10.md`), or a Layer 4 Project Rule document. Where a specific tool selection has not yet been confirmed in a document available to this generator (e.g., the specific test runner, linter, or Electron packaging tool), this document states that plainly and defers to `global_technology_stack_v10.md` rather than inventing a selection.
**Inherits from:** `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` (Layer 1 — the Development Environment principle, Section 10; the Dev Container philosophy, Section 11; the Package Management philosophy, Section 14); `global_rules_revisionfinal_v10.md` (Layer 2 — the Git Workflow Rules, Section 5; the Environment and Dependency Management Principles, Section 6; the TDD Boundary, Section 4); `global_technology_stack_v10.md` (Layer 3 — the canonical technology table, including the confirmed package-manager defaults this document operationalizes); and `project-pc-app_v04.md`, `project-personal-full-stack_v01.md`, and `project-monolithic_v04.md` (Layer 4 — the three currently `Active` archetype documents, supplying the directory layouts these commands are run against). Every rule and tool name drawn from these documents is referenced below by name; none is restated or reinterpreted.
**Governs:** Nothing below it. This is an operational Layer 5 document; it does not create new obligations for Layers 6–11 beyond what they already owe to Layers 1–4. It supplies command syntax only — it does not itself authorize any operation it documents.
**Supersedes:** None. This is the first version of this document.
**Read order:** Read after `FRAMEWORK_README.md`, `PROJECT_BOOTSTRAP_GUIDE.md`, and the Layer 4 Project Rule document matching the current project's archetype, whenever a developer or AI agent needs the canonical command-line syntax for an operation those documents have already authorized. Per `FRAMEWORK_BLUEPRINT.md`, Section 2.5's AI interaction model, this document — together with `PROJECT_STRUCTURE.md` — is "referenced continuously as a lookup table during execution," not read once and set aside.
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
Layer 4 — Project Rules                         project-pc-app_v04.md               (Active)
                                                 project-personal-full-stack_v01.md  (Active)
                                                 project-monolithic_v04.md           (Active)
                                                 project-mobile_v01.md               (Tier 3 / v10.1, pending)
        ↓ (this document inherits from all four layers above, by reference)
Layer 5 — Developer Manuals   FRAMEWORK_README.md          (Active)
                              PROJECT_BOOTSTRAP_GUIDE.md   (Active)
                              CONTRIBUTING.md              (Active)
                              COMMANDS.md                  ← this document
                              PROJECT_STRUCTURE.md         (pending, Tier 2)
                              AI_DEVELOPMENT_MANUAL.md     (pending, Tier 3 / v10.1)
        ↓
Layer 6 — Reference Implementations
Layer 7 — AI Skills
Layer 8 — Prompt Library
Layer 9 — Project Templates
```

**What this document contains.** The canonical command-line reference for every operation Layers 1–4 already authorize: environment startup, version control, package management, testing, and linting/type-checking — stated once, cross-archetype where a command is archetype-agnostic, and per-archetype where the directory layout or toolchain differs (`FRAMEWORK_BLUEPRINT.md`, Section 2.5, "Responsibilities": "Define the canonical command reference").

**What this document does not contain, by design.** Per the Layer 5 prohibited responsibilities (`FRAMEWORK_BLUEPRINT.md`, Section 2.5):

- It **MUST NOT** introduce a new architectural rule, tool selection, or workflow. Every command below is the literal invocation of a tool or convention Layers 1–4 have already named.
- It **MUST NOT** restate the canonical directory layout of any archetype. It assumes the reader has already consulted the matching Layer 4 document (or, once `Active`, `PROJECT_STRUCTURE.md`) for *where* a command is run; this document states only *what* the command is.
- It **MUST NOT** define reusable, directly-executable AI Skills or their invocation. Those are Layer 7 (`SKILLS.md`) and Layer 8 (Prompt Library) concerns; Section 10 below draws this boundary explicitly.
- It **MUST NOT** name a specific test runner, linter, static type-checker, or Electron packaging/installer tool where that selection has not been confirmed in a document available to this generator. Per `project-pc-app_v04.md`, Section 8, and `project-personal-full-stack_v01.md`, Section 8, these selections are deliberately left to `global_technology_stack_v10.md` "at the time a project is bootstrapped," precisely so that Layer 4 (and, by the same logic, this Layer 5 document) does not drift out of sync with Layer 3 as that table evolves. This document follows the same restraint rather than inventing a name.

**How an AI agent uses this document.** An agent that has already read the matching Layer 4 Project Rule document and needs to *run* an operation that document authorizes (initialize a repository, install a dependency, run a test suite) consults this document for the exact command, rather than guessing at CLI syntax or inventing a convention. Where this document defers to `global_technology_stack_v10.md` for a specific tool name, the agent **MUST** consult that document directly before running the command, per Section 8 and Section 11 below.

---

## 1. What This Document Is and Is Not

This document answers exactly one question: **"What is the exact command for an operation Layers 1–4 have already authorized?"** It does not answer *whether* an operation is authorized (that is Layers 1–4's question, and `PROJECT_BOOTSTRAP_GUIDE.md`'s procedural question) or *where* in the project structure it applies (that is the matching Layer 4 document's, and eventually `PROJECT_STRUCTURE.md`'s, question).

Per the framework's inheritance model (`FRAMEWORK_BLUEPRINT.md`, Section 5), this document does not restate a Layer 1–4 rule — it operationalizes one, at the single point where "what is the rule" becomes "what do I type." If any statement below appears to state a rule rather than a command, that statement is a defect and **MUST** be corrected to a reference, consistent with the same discipline `PROJECT_BOOTSTRAP_GUIDE.md`, Section 1, already applies to itself.

---

## 2. Inheritance Declaration

Per the inheritance rule that a Layer 5 document does not restate a governing-layer rule (`FRAMEWORK_BLUEPRINT.md`, Section 5), this document inherits, by reference and without restatement:

| From | Document | What is inherited |
|---|---|---|
| Layer 1 | `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` | The Development Environment principle — Clone → Open Project → Docker Compose → Start Development (Section 10); the Dev Container philosophy (Section 11); the Package Management philosophy — Poetry primary, uv alternative (Section 14). |
| Layer 2 | `global_rules_revisionfinal_v10.md` | The Git Workflow Rules — Conventional Commits format, one logical change per commit, no direct commits to the primary integration branch for non-trivial changes, descriptive branch names (Section 5); the Environment and Dependency Management Principles — explicit, pinned/lock-filed dependencies (Section 6); the TDD Boundary, which Section 7 below maps to concrete test-invocation commands (Section 4). |
| Layer 3 | `global_technology_stack_v10.md` | The canonical technology table in full, including the confirmed package-manager defaults this document operationalizes (Poetry/uv for Python backends, pnpm for Node.js/frontend code) and the technology selections this document does **not** name because they remain a Layer 3 lookup at bootstrap time (test runner, linter, static type-checker, Electron packaging tool). |
| Layer 4 | `project-pc-app_v04.md`, `project-personal-full-stack_v01.md`, `project-monolithic_v04.md` | The directory layout each archetype's commands are run against (Section 9 below), and each document's own confirmed Layer 3 technology application (its own Section 8). This document does not restate any of the three documents' directory layouts; Section 9 below states only which command runs where, by reference. |

Where this document states a command, it is either (a) the literal CLI invocation of a tool already named at Layer 1 or Layer 3 (e.g., `poetry`, `pnpm`, `docker compose`, `git`), or (b) a generic invocation pattern (e.g., "run the approved test runner via `poetry run <test-runner>`") used precisely because the specific tool name is not yet confirmed in a document available to this generator. It is never a new architectural decision.

---

## 3. Command Categories Overview

| # | Category | Defined in | Archetype scope |
|---|---|---|---|
| 1 | Environment and Session Commands | Section 4 | Cross-archetype |
| 2 | Version Control Commands | Section 5 | Cross-archetype |
| 3 | Package Management Commands | Section 6 | Cross-archetype, split by language/toolchain |
| 4 | Testing Commands | Section 7 | Cross-archetype pattern, mapped to the Layer 2 TDD Boundary |
| 5 | Linting and Type-Checking Commands | Section 8 | Cross-archetype pattern |
| 6 | Archetype-Specific Command Reference | Section 9 | Per archetype (Desktop, Full-Stack, Monolithic; Mobile deferred) |

No category above introduces a new tool or workflow. Each is a lookup table over what Layers 1–4 already authorize.

---

## 4. Environment and Session Commands

These commands operationalize the Constitution's Development Environment principle (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 10): Clone Repository → Open Project → Docker Compose → Start Development, reproducible across Home PC, Office PC, and laptop.

| Operation | Command | Notes |
|---|---|---|
| Clone the repository | `git clone <repository-url>` | The first step of the standard workflow (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 10). |
| Start the containerized development environment | `docker compose up` | Primary workflow per the Constitution. **MUST** be preferred over native execution for the project's non-native development activities (Section 9 below states, per archetype, which activities remain native — e.g., Electron shell packaging). |
| Start the containerized environment in the background | `docker compose up -d` | Equivalent to the above; **MAY** be used where the developer needs the terminal free for other commands in this document. |
| Stop the containerized development environment | `docker compose down` | Tears down containers started by `docker compose up`. |
| Rebuild containers after a Dockerfile or Dev Container configuration change | `docker compose up --build` | **SHOULD** be run whenever a project's Dev Container configuration changes (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 11: "Tool versions, SDKs, extensions, and runtimes should be reproducible"). |
| Open a shell inside a running container | `docker compose exec <service> sh` (or `bash`, where available) | Used to run any Section 6–8 command inside the reproducible environment rather than on the host, consistent with the containerized workflow being primary. |

Native, host-level execution **MAY** be used instead of the above (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 10: "Native execution MAY be used but is not the primary workflow"). Where an archetype requires native execution for a specific activity (e.g., the Electron shell, per `project-pc-app_v04.md`, Section 2, Rule 3), Section 9.1 below states this explicitly.

---

## 5. Version Control Commands

These commands operationalize the Git Workflow Rules already fixed at Layer 2 (`global_rules_revisionfinal_v10.md`, Section 5). This document does not restate those rules; it states their command-line form.

| Operation | Command | Governing rule |
|---|---|---|
| Create a new branch for a change | `git checkout -b <type>/<short-subject>` | Branch names **SHOULD** be descriptive of intent (`global_rules_revisionfinal_v10.md`, Section 5.6). `<type>` **SHOULD** mirror the Conventional Commits type of the work it contains (e.g., `feat/`, `fix/`). |
| Stage changes for a commit | `git add <path>` (or `git add -p` for partial staging) | Supports the "one logical, coherent change per commit" rule (`global_rules_revisionfinal_v10.md`, Section 5.2). |
| Commit a change | `git commit -m "<type>(<scope>): <description>"` | **MUST** follow the Conventional Commits format (`global_rules_revisionfinal_v10.md`, Section 5.1). |
| Push a branch to the remote | `git push -u origin <branch-name>` | — |
| Open a reviewed change (pull/merge request) | Performed through the Git hosting platform's own interface or CLI, per `CONTRIBUTING.md` | Direct commits to the primary integration branch **MUST NOT** occur for non-trivial changes (`global_rules_revisionfinal_v10.md`, Section 5.3). This document does not restate `CONTRIBUTING.md`'s procedure; it points to it. |
| Pull the latest changes from the remote | `git pull` | — |

Where a change introduces or alters an architectural decision, the corresponding `DECISIONS.md` entry **MUST** be appended in the same commit (`global_rules_revisionfinal_v10.md`, Section 5.4; PR-001). This document does not define how to author that entry — see `DECISIONS.md`, Section 1, for the required entry format.

---

## 6. Package Management Commands

These commands operationalize the Constitution's Package Management philosophy (`AI_DEVELOPMENT_PHILOSOPHY_v2.0.md`, Section 14) and the Layer 3 package-manager defaults each Active Layer 4 document confirms for its own toolchain.

### 6.1 Backend / Python (Poetry primary, uv alternative)

Applicable to the backend of the Full-Stack archetype (`project-personal-full-stack_v01.md`, Section 8) and the Monolithic archetype (`project-monolithic_v04.md`, Section 8). Not applicable to the PC App archetype, whose runtime is Node.js/Electron (`project-pc-app_v04.md`, Section 1).

| Operation | Poetry (primary) | uv (alternative) |
|---|---|---|
| Install all declared dependencies | `poetry install` | `uv sync` |
| Add a new dependency | `poetry add <package>` | `uv add <package>` |
| Add a development-only dependency | `poetry add --group dev <package>` | `uv add --dev <package>` |
| Run a command inside the managed environment | `poetry run <command>` | `uv run <command>` |
| Update dependencies within declared version constraints | `poetry update` | `uv lock --upgrade` |

A project **MUST** declare its dependencies explicitly and completely, and dependency versions **MUST** be pinned or lock-filed for a byte-for-byte reproducible build (`global_rules_revisionfinal_v10.md`, Section 6.1–6.2) — both commands above satisfy this via their respective lockfiles (`poetry.lock` / `uv.lock`), which **MUST** be committed to version control.

### 6.2 Frontend / Node.js (pnpm)

Applicable to the PC App archetype's renderer (`project-pc-app_v04.md`, Section 8), the Full-Stack archetype's frontend (`project-personal-full-stack_v01.md`, Section 8), and the Monolithic archetype's React frontend (`project-monolithic_v04.md`, Section 8). npm and yarn **MUST NOT** be used as a project's package manager, per `FRAMEWORK_README.md`, Section 6, Consequence 2.

| Operation | Command |
|---|---|
| Install all declared dependencies | `pnpm install` |
| Add a new dependency | `pnpm add <package>` |
| Add a development-only dependency | `pnpm add -D <package>` |
| Run a package-defined script | `pnpm run <script-name>` |
| Update dependencies within declared version constraints | `pnpm update` |

The `pnpm-lock.yaml` file **MUST** be committed to version control, for the same reproducibility reason stated in Section 6.1 above.

---

## 7. Testing Commands

The Layer 2 TDD Boundary (`global_rules_revisionfinal_v10.md`, Section 4) is defined once and is not restated here. This section states only the command pattern for invoking tests in each zone; it does not name a specific test runner, since that selection remains a Layer 3 lookup at bootstrap time (`project-pc-app_v04.md`, Section 8; `project-personal-full-stack_v01.md`, Section 8; `project-monolithic_v04.md`, Section 8).

| TDD Boundary zone (Layer 2 definition) | Backend (Python) invocation pattern | Frontend (Node.js) invocation pattern |
|---|---|---|
| Business/domain logic (MUST be test-first) | `poetry run <approved-test-runner> tests/domain/` | `pnpm run test -- <domain-or-unit-test-path>` |
| Application/use-case orchestration (SHOULD test-first, MUST coverage) | `poetry run <approved-test-runner> tests/application/` | `pnpm run test -- <application-test-path>` |
| Infrastructure adapters (SHOULD have integration coverage) | `poetry run <approved-test-runner> tests/infrastructure/` | `pnpm run test -- <infrastructure-test-path>` |
| UI/presentation, framework glue, end-to-end (MAY, behavior-level preferred) | `poetry run <approved-test-runner> tests/api/` (or `tests/e2e/`, per archetype) | `pnpm run test:e2e` (or the project's declared end-to-end script) |
| Run the full suite | `poetry run <approved-test-runner>` | `pnpm run test` |

`<approved-test-runner>` **MUST** be resolved from `global_technology_stack_v10.md` before this pattern is invoked. This document deliberately does not substitute a specific name, consistent with the same restraint every Active Layer 4 document already applies to this exact selection (Section 2 above). An AI agent invoking a test command **MUST** confirm the approved runner via Layer 3 first rather than assume one.

The exact test directory a command targets (`tests/domain/`, `backend/tests/domain/`, etc.) **MUST** be taken from the matching Layer 4 archetype document's directory layout (Section 9 below), not assumed from this table, since the path differs by archetype.

---

## 8. Linting and Type-Checking Commands

Code **MUST** pass automated linting and, where the language supports it, static type checking before being considered done (`global_rules_revisionfinal_v10.md`, Section 7.1; Section 10, Definition of Done, item 3). As with Section 7 above, the specific linter and type-checker are Layer 3 selections not yet confirmed in a document available to this generator, and this document does not invent one.

| Operation | Backend (Python) invocation pattern | Frontend (Node.js) invocation pattern |
|---|---|---|
| Run the approved linter | `poetry run <approved-linter>` | `pnpm run lint` |
| Run the approved static type-checker (where applicable) | `poetry run <approved-type-checker>` | `pnpm run type-check` (or equivalent declared script) |
| Auto-fix lint violations where the tool supports it | `poetry run <approved-linter> --fix` (flag varies by tool) | `pnpm run lint -- --fix` (or equivalent declared script) |

`<approved-linter>` and `<approved-type-checker>` **MUST** be resolved from `global_technology_stack_v10.md` before this pattern is invoked, for the same reason stated in Section 7 above.

---

## 9. Archetype-Specific Command Reference

The commands in Sections 4–8 above are archetype-agnostic. This section states only what differs *by archetype*: which directory a command in those sections targets, and any command an archetype requires that the cross-archetype sections do not cover. It does not restate any archetype's directory layout in full — see the referenced Layer 4 document for that.

### 9.1 Desktop / PC App (Electron) — `project-pc-app_v04.md`

| Operation | Command | Reference |
|---|---|---|
| Install dependencies | `pnpm install` (project root) | `project-pc-app_v04.md`, Section 8 |
| Run the application in development mode | `pnpm run dev` (or the project's declared development script) | — |
| Run domain/application tests | `pnpm run test -- src/domain` / `pnpm run test -- src/application` | `project-pc-app_v04.md`, Section 4 (directory layout) |
| Build the Electron application | `pnpm run build` (or the project's declared build script) | The specific packaging/installer tool (replacing the deprecated Inno Setup) is a Layer 3 selection not named here (`project-pc-app_v04.md`, Section 8). |
| Package/produce an installable artifact | `pnpm run package` (or the project's declared packaging script) | **MUST** be run natively, not inside the Dev Container, per `project-pc-app_v04.md`, Section 2, Rule 3 — window chrome, OS-level integrations, and installer output cannot be meaningfully exercised inside a container. |

### 9.2 Full-Stack Application — `project-personal-full-stack_v01.md`

| Operation | Command | Reference |
|---|---|---|
| Install backend dependencies | `poetry install` (run inside `backend/`) | `project-personal-full-stack_v01.md`, Section 4 |
| Install frontend dependencies | `pnpm install` (run inside `frontend/`) | `project-personal-full-stack_v01.md`, Section 4 |
| Run the backend development server | `poetry run <asgi-server-invocation>` (e.g., an ASGI server command declared by the project) | The specific ASGI server invocation is a Layer 3/bootstrap-time detail; FastAPI itself is confirmed (`project-personal-full-stack_v01.md`, Section 2, Rule 1). |
| Run the frontend development server | `pnpm run dev` (run inside `frontend/`) | — |
| Run backend tests by TDD zone | `poetry run <approved-test-runner> backend/tests/domain/` (and similarly for `application/`, `infrastructure/`, `api/`) | `project-personal-full-stack_v01.md`, Sections 4 and 9 |
| Run frontend tests | `pnpm run test` (run inside `frontend/`) | `project-personal-full-stack_v01.md`, Section 4 |

### 9.3 Monolithic Application — `project-monolithic_v04.md`

| Operation | Command | Reference |
|---|---|---|
| Install backend dependencies | `poetry install` (run inside `backend/`, per project convention) | `project-monolithic_v04.md`, Section 4 |
| Install frontend dependencies | `pnpm install` (run inside `frontend/`) | `project-monolithic_v04.md`, Section 4 |
| Run the backend development server | `poetry run <asgi-server-invocation>` | FastAPI confirmed (`project-monolithic_v04.md`, Section 2, Rule 1). |
| Run the frontend development server | `pnpm run dev` (run inside `frontend/`) | — |
| Run backend tests by TDD zone | `poetry run <approved-test-runner> backend/domain/` (per the project's own test directory, mirrored under `tests/`, per `project-monolithic_v04.md`, Section 4) | `project-monolithic_v04.md`, Sections 4 and 9 |
| Build the frontend for production serving | `pnpm run build` (run inside `frontend/`) | The compiled output is served by the FastAPI application in production, per `project-monolithic_v04.md`, Section 3, Rule 5. |

### 9.4 Mobile Application — Deferred (Tier 3 / v10.1)

`project-mobile_v01.md` is `Pending (Tier 3 / v10.1)` (`FRAMEWORK_README.md`, Section 4.3; `FRAMEWORK_STATUS.md`). Per `PROJECT_BOOTSTRAP_GUIDE.md`, Section 5.3, a Mobile-archetype bootstrap request **MUST** be declined at the framework level entirely, not merely deferred, until v10.1 scope begins. This document accordingly defines no Mobile-archetype command reference. An agent asked for Mobile-archetype commands **MUST** report this as an out-of-scope request for the current framework version, consistent with `PROJECT_BOOTSTRAP_GUIDE.md`, Section 5.3, rather than improvise a command set.

---

## 10. Relationship to Layer 7 (AI Skills) and Layer 8 (Prompt Library) — Boundary Clarification

This document is easily confused with two adjacent, but distinct, concerns. Neither is in scope here:

1. **Skill invocation (Layer 7) is not a shell command.** Selecting and executing `create-feature`, `generate-tests`, or `review-code` (`SKILLS.md`, Section 12) is an AI-agent workflow action governed by `SKILLS.md`'s discovery procedure (`SKILLS.md`, Section 6) and each Skill's own `steps` field — it is not invoked via a CLI command this document could define. This document's commands are the *mechanical* operations (install, test, lint, commit) a Skill's `steps` may direct an agent to run; it is not itself a list of Skills.
2. **Tool-specific AI invocation (Layer 8) is a separate artifact.** The Prompt Library (`Active` — `CLAUDE_CODE_PROMPTS.md`, `OPENAI_PROMPTS.md`) wraps a Skill for a specific AI tool (`FRAMEWORK_BLUEPRINT.md`, Section 11). That wrapper is not a shell command either, and this document does not anticipate or duplicate its content.

An agent that needs to run a mechanical operation as part of executing a Skill's `steps` consults this document for the command; it consults `SKILLS.md` and the individual Skill document for *what to do and when*.

---

## 11. Current Applicability at Framework v10's Mid-Migration State

Section 4 above states the canonical command set independent of which documents currently exist. As of this revision, the following gaps apply, consistent with `FRAMEWORK_README.md`, Section 4: "An AI agent MUST treat any document not listed as `Active`... as unavailable for new work."

1. **Specific test runner, linter, and static type-checker names.** Sections 7 and 8 above deliberately do not name these tools. An agent **MUST** consult `global_technology_stack_v10.md` directly before running a Section 7 or Section 8 command, rather than assume a name from general knowledge of the ecosystem. This is not a gap introduced by this document; it is the same restraint already applied by every Active Layer 4 document to this identical selection (Section 2 above). This gap remains open — it is not resolved by any document reaching `Active` status, since it reflects a deliberate, ongoing Layer 3 lookup requirement rather than a missing document.
2. **Electron packaging/installer tooling.** Section 9.1 above does not name the tool replacing the deprecated Inno Setup, for the same reason. This gap likewise remains open.
3. **`PROJECT_STRUCTURE.md` (Resolved).** `PROJECT_STRUCTURE.md` is now `Active`. Section 9 above has been reviewed for consistency with its cross-archetype canonical reference, per the mirroring obligation this section establishes (Section 14 below); Section 13 below reflects this document's current relationship to it.
4. **Mobile archetype.** Per Section 9.4 above, no command reference is defined, consistent with `PROJECT_BOOTSTRAP_GUIDE.md`, Section 5.3. This gap remains open pending `project-mobile_v01.md`'s v10.1 generation.
5. **Prompt Library (Resolved — `Active`).** Both required tool-specific documents, `CLAUDE_CODE_PROMPTS.md` and `OPENAI_PROMPTS.md`, now exist. Section 10 above continues to state the boundary between this document and the Prompt Library — this document still does not define AI-tool invocation syntax, since that remains Layer 8's responsibility, not because Layer 8 is unpopulated.

An agent encountering one of the two gaps still open above (items 1 and 2) **MUST** report it plainly and consult `global_technology_stack_v10.md`, rather than invent a substitute command or tool name.

---

## 12. Prohibited Responsibilities — Explicit Boundary

Consistent with `FRAMEWORK_BLUEPRINT.md`, Section 2.5, this document explicitly does **not** do the following, and any future edit that adds one of these is out of scope for this document and **MUST** be redirected to the correct layer instead:

| Not defined here | Correct layer |
|---|---|
| Whether an operation is authorized at all | Layers 1–4 (the rule itself); `PROJECT_BOOTSTRAP_GUIDE.md` (the bootstrap procedure) |
| Canonical directory structure across all archetypes | Layer 5 (`PROJECT_STRUCTURE.md`, Active) |
| Archetype-specific directory layout in full | Layer 4 (the matching Project Rule document) |
| The canonical technology table itself, including specific test runner/linter/packager names | Layer 3 (`global_technology_stack_v10.md`) |
| Reusable, directly-executable AI Skill definitions | Layer 7 (`SKILLS.md` and individual Skill documents) |
| Tool-specific AI invocation wrappers | Layer 8 (Prompt Library, pending) |
| Commit/Pull Request procedural workflow in full | Layer 5 (`CONTRIBUTING.md`) |
| Project bootstrap procedure in full | Layer 5 (`PROJECT_BOOTSTRAP_GUIDE.md`) |

---

## 13. Relationship to Other Layer 5 Documents

| Document | Relationship to this document |
|---|---|
| `FRAMEWORK_README.md` | Read *before* this document, every session. Supplies the current document-status tables this document's Section 11 depends on. |
| `PROJECT_BOOTSTRAP_GUIDE.md` | Governs *whether* and *when* an operation in this document may be run (e.g., no command in Section 9 may be run before Gate 1 — Plan Approval — per that guide's Section 4.6). This document supplies the *syntax*; that guide supplies the *sequencing*. |
| `CONTRIBUTING.md` | Governs the procedural detail of opening a reviewed change (pull/merge request) that Section 5 of this document points to rather than restates. |
| `PROJECT_STRUCTURE.md` (Active) | Supplies the cross-archetype canonical directory reference. Section 9 of this document points to each Layer 4 document's own directory layout directly rather than duplicating `PROJECT_STRUCTURE.md`'s consolidated reference; the two remain consistent per `PROJECT_STRUCTURE.md`, Section 15. |
| `AI_DEVELOPMENT_MANUAL.md` (Tier 3, v10.1, pending) | Will house the comprehensive AI-agent operational manual; this document covers only command syntax, not the broader operational guidance that document will eventually provide. |

---

## 14. Authority and Conflict Resolution

1. Per the downward authority rule (KA-002, `FRAMEWORK_BLUEPRINT.md`, Section 6), this document **MUST NOT** contradict `AI_DEVELOPMENT_PHILOSOPHY_v2.0.md` (Layer 1), `global_rules_revisionfinal_v10.md` (Layer 2), `global_technology_stack_v10.md` (Layer 3), or any Active Layer 4 Project Rule document. Where a command in this document appears to conflict with any of those, the higher-priority document wins and this document **MUST** be corrected.
2. Where `global_technology_stack_v10.md` confirms a specific test runner, linter, static type-checker, or Electron packaging tool not yet named in this document, Sections 7, 8, and 9.1 **MUST** be updated to name it, in the same change that confirms the selection, per the mirroring obligation this document places on itself (Section 15 below).
3. The full conflict-resolution procedure, including same-layer conflicts and deprecated-document handling, is defined in `FRAMEWORK_BLUEPRINT.md`, Section 6, and is not restated here.

---

## 15. Change Control

1. This document **MUST NOT** be edited silently to introduce a new command category, tool, or workflow. A change of that kind is an architectural change and **MUST** follow the amendment procedure defined in `FRAMEWORK_BLUEPRINT.md`, Section 18.2: proposal → Gate 2 (Architecture Approval) → amendment → `DECISIONS.md` entry → `FRAMEWORK_HANDOVER.md` update in the same commit.
2. This document **MAY** be updated, without a Gate 2 event, to fill in a placeholder this document itself already anticipates (e.g., naming the approved test runner once `global_technology_stack_v10.md` confirms it) — this is a transcription of an already-existing Layer 3 decision into this document's lookup tables, not a new decision, consistent with the same distinction `DECISIONS.md`, Section 5, Rule 2, draws for its own append-only gap-filling.
3. Section 11 of this document **MUST** be updated in the same change as any transition of a document it names from `Pending` to `Active` (most directly `PROJECT_STRUCTURE.md` and the Prompt Library), so that this document never claims a gap that no longer exists or stays silent about one that does — mirroring the identical obligation `PROJECT_BOOTSTRAP_GUIDE.md`, Section 7, and `TEMPLATE_SPEC.md`, Section 17, already place on their own equivalent sections.
4. Section 9 of this document **MUST** be extended with a Mobile-archetype subsection in the same change that `project-mobile_v01.md` transitions from `Pending (Tier 3 / v10.1)` to `Active`, rather than being left silent about an archetype the framework has since adopted.
5. Upon this document's creation, `FRAMEWORK_README.md`, Sections 4–6, and `FRAMEWORK_STATUS.md` **MUST** be updated in the same change to reflect its new `Active` status and its removal from the "Pending (Tier 2)" lists, per `FRAMEWORK_README.md`, Section 9, and the AI Session Instructions in `FRAMEWORK_STATUS.md`.

---

## Closing Statement

This document is the canonical command reference for Framework v10 (`FRAMEWORK_BLUEPRINT.md`, Section 2.5) — the single lookup table an agent or developer consults for the exact command-line syntax of an operation Layers 1–4 have already authorized: environment startup, version control, package management, testing mapped to the Layer 2 TDD Boundary, linting/type-checking, and the archetype-specific commands for the three currently `Active` Layer 4 archetypes. No architectural decision has been introduced beyond what `FRAMEWORK_BLUEPRINT.md`, Section 2.5, assigns to this layer's responsibility; every command above operationalizes a tool, workflow, or convention already named at Layers 1–3 or confirmed in a Layer 4 document, and every place where a specific tool name is not yet confirmed (test runner, linter, type-checker, Electron packager) is stated as an open, honestly-reported gap deferring to `global_technology_stack_v10.md`, rather than filled with an invented name. `PROJECT_STRUCTURE.md` and the Prompt Library, previously listed here as pending Tier 2 targets, are now both `Active`; the sole remaining Tier 2 target is `template-fastapi-sqlite/`, per `FRAMEWORK_STATUS.md`, "Current Work."
