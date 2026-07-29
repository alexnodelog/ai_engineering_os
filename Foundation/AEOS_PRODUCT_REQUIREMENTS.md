# AI Engineering Operating System

## AEOS — Product Requirements Document

*The product source of truth for the AEOS repository.*

| Field | Value |
| :--- | :--- |
| **Document** | Product Requirements Document (PRD) |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-PRD |
| **Version** | 1.1.1 |
| **Status** | Freeze candidate — awaiting owner approval |
| **Owner** | Product Owner, AEOS |
| **Author** | Chief Product Architect, AEOS |
| **Audience** | Product owner, engineering contributors, AI runtimes consuming this repository |
| **Suggested path** | `docs/product/PRD.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) |
| **Supersedes** | None |

> **Authority of this document.**
> This PRD defines *what* AEOS is and *what it must do as a product*.
> It does not define *how* AEOS is built. Architecture, interface contracts, data formats,
> and implementation belong to downstream documents that must trace back to the requirement
> identifiers defined here. Where any downstream document conflicts with this PRD, this PRD wins
> until the owner approves a revision.

---

## Table of Contents

1. [Overview](#1-overview)
2. [Executive Summary](#2-executive-summary)
3. [Scope and Applicability](#3-scope-and-applicability)
4. [Problem Statement](#4-problem-statement)
5. [Mission and Vision](#5-mission-and-vision)
6. [What AEOS Is and Is Not](#6-what-aeos-is-and-is-not)
7. [Product Philosophy](#7-product-philosophy)
8. [Users](#8-users)
9. [The Engineering Lifecycle](#9-the-engineering-lifecycle)
10. [The Interaction Model](#10-the-interaction-model)
11. [Environment Philosophy](#11-environment-philosophy)
12. [Product Capabilities](#12-product-capabilities)
13. [Repository Product Assets and Runtime State](#13-repository-product-assets-and-runtime-state)
14. [Platform Support](#14-platform-support)
15. [Distribution Strategy](#15-distribution-strategy)
16. [Runtime Strategy](#16-runtime-strategy)
17. [Product Scope](#17-product-scope)
18. [Product Requirements](#18-product-requirements)
19. [Quality Attributes](#19-quality-attributes)
20. [Safety and Trust Model](#20-safety-and-trust-model)
21. [Success Metrics](#21-success-metrics)
22. [Release Phases](#22-release-phases)
23. [Assumptions, Dependencies, and Risks](#23-assumptions-dependencies-and-risks)
24. [Glossary](#24-glossary)
25. [Document Governance](#25-document-governance)
26. [Appendix A — Recommendations for Future Releases](#appendix-a--recommendations-for-future-releases)
27. [Appendix B — Requirement Index](#appendix-b--requirement-index)

---

## 1. Overview

AEOS is an operating system for AI-assisted software engineering.

An operating system is what makes many independent capabilities usable together, consistently, so
that every program need not solve the same problems again. AEOS does that for engineering work
performed with AI. It holds the project's context, upholds the engineering discipline, sequences the
work, and keeps the person in control of what happens next.

> **On the metaphor.** *Operating system* describes what AEOS does for engineering activity, not how
> AEOS is built. AEOS is not a kernel. It does not schedule processes, abstract hardware, or replace
> the operating system a machine already runs. The metaphor names the orchestration of software
> engineering work, and nothing below that level.

AEOS does not perform language-model inference. It never has its own model, and it never becomes
one. Inference is delegated to external AI runtimes that the user chooses and controls. AEOS
supplies what those runtimes lack: durable project context, an enforced engineering process,
minimized and deliberate context delivery, a human approval gate in front of every consequential
action, and a repository that remains the single source of truth after the AI session ends.

The result is a product that turns ad-hoc AI assistance into a reproducible engineering practice.

| In one sentence |
| :--- |
| AEOS lets developers build software through AI-assisted, human-supervised engineering workflows, orchestrating any AI runtime across the full lifecycle without ever becoming a model, an IDE, or a framework. |

---

## 2. Executive Summary

**What AEOS is.** AEOS is an operating system for AI-assisted software engineering. It orchestrates
the complete engineering lifecycle — environment preparation, project initialization, requirement
analysis, architecture, TDD, agentic orchestration, code generation, review, refactoring, testing,
documentation, deployment, and maintenance — while performing no language-model inference of its
own. Inference belongs to external AI runtimes that the user chooses and controls. AEOS supplies
what those runtimes do not: durable project context, an enforced engineering discipline, deliberate
context minimization, a human approval gate in front of every consequential action, and a repository
that remains authoritative after the session ends.

**Who it is for.** Developers who build with AI and want the result to be verifiable rather than
merely fast. Engineering leads who need a team's practice to be explicit, versioned, and uniform
across people and tools. Platform engineers responsible for predictable behavior across Windows,
macOS, and Linux. Open-source maintainers who want a repository that teaches contributors — human
and AI — how the project is built. And the AI runtimes themselves, which read the repository to
learn how to act inside a project.

**Why it exists.** AI coding tools have become capable; AI engineering has not become disciplined.
Process improvised per session is not reproducible. Context thrown at a model raises cost and lowers
accuracy. Tools that act before they explain destroy trust. Tests written last leave generated code
unverified. A practice locked to one vendor must be rebuilt when the vendor changes. Environments
assumed rather than inspected break working machines. Knowledge left in a chat log dies with the
session. AEOS answers these as product behavior, not as advice.

**What it guarantees.** The human decides before AEOS acts. AEOS explains before it executes. Tests
come before implementation. The repository is the source of truth. Context is minimized on purpose.
The user's choice of vendor, runtime, model, platform, and distribution method is theirs alone, and
changing any of them does not change how the project is built.

| At a glance |  |
| :--- | :--- |
| **Product category** | Operating system for AI-assisted, human-supervised software engineering |
| **Performs inference** | Never — external AI runtimes do, under user control |
| **Core loop** | Inspect → Explain → Propose → Confirm → Execute → Report |
| **Capabilities** | Ten, spanning environment, project, workflow, runtime, TDD, documentation, rules, skills, prompts, and repository |
| **Platforms** | Windows, macOS, Linux — equal citizens, no primary |
| **Distribution** | GitHub Clone, Native Installer, MCP, Portable — with package managers, containers, and IDE marketplaces planned |
| **Source of truth** | The repository, as versioned Repository Assets |
| **Default posture** | Human-in-the-Loop, TDD-first, safe by default, fails closed |

---

## 3. Scope and Applicability

### 3.1 The Product Boundary

This document defines the **Product**. It does not define how the product is built.

That distinction is load-bearing. A product definition that leaks implementation constrains
decisions that have not been made yet, and ages the moment those decisions change. This PRD
therefore describes what AEOS is and what users can observe it doing — and stops there.

### 3.2 Layers and Their Owners

| Layer | Question it answers | Defined by | Role of this PRD |
| :--- | :--- | :--- | :--- |
| **Product** | What is AEOS, who is it for, and what must it do for them? | This document | **Defines it completely.** This is the Product Contract. |
| **Architecture** | How is AEOS structured so it can deliver the product? | Architecture documents | Defers entirely. Names no structure, no layering, no mechanism. |
| **Specification** | How must each defined behavior work, precisely and testably? | Specification documents | Defers entirely. States outcomes, never formats or interfaces. |
| **Runtime** | How does AEOS execute, in what environment, with what lifecycle? | Runtime documents | Defers entirely. States what users observe, never what executes. |
| **Repository** | What durable assets constitute the product? | This document names the asset kinds; architecture defines their form | Defines Repository Assets as product concepts only. |
| **Workflow** | What engineering sequence does AEOS drive, and where does the human decide? | This document defines the observable behavior; specification defines the mechanics | Defines the approval model and lifecycle coverage only. |
| **Implementation** | What code realizes all of the above? | The codebase and its tests | Defers entirely. Traces back to requirement identifiers defined here. |

### 3.3 What This Document Deliberately Avoids

| Not defined here | Because |
| :--- | :--- |
| Internal structure, components, layering, or module boundaries | These are architecture decisions and must stay revisable. |
| Interfaces, data formats, schemas, or file layouts | These are specification decisions and would freeze prematurely here. |
| Storage, transport, process, or execution mechanisms | These are runtime decisions and vary by platform and distribution. |
| Languages, libraries, frameworks, or technology choices | These are implementation decisions and must follow the product, not precede it. |
| How integration with any external system is achieved | Naming a mechanism today would outlive its usefulness and constrain future standards. |

### 3.4 The Reading Rule

> **If a statement in this document can be satisfied in only one way, it is too specific.**
> Report it as a defect in this PRD rather than treating it as an architectural instruction.
> Every requirement here should admit more than one honest implementation.

Conversely, a downstream document may not weaken, reinterpret, or quietly widen a product
requirement. Architecture decides *how*. It does not decide *whether*.

## 4. Problem Statement

AI coding tools have become capable. AI engineering has not become disciplined.

| # | Problem | Consequence |
| :--- | :--- | :--- |
| P1 | **Process lives in the developer's head.** The sequence of analysis, design, testing, and review is improvised in each chat session. | Output quality varies per session, per developer, and per day. Nothing is reproducible. |
| P2 | **Context is thrown at the model.** Whole repositories are pasted in the hope that relevance emerges. | Cost rises, accuracy falls, and the model attends to the wrong things. |
| P3 | **Tools act before they explain.** Files are written, dependencies installed, and branches rewritten before the developer understands the intent. | Trust erodes. Developers either disable automation or stop reviewing it. |
| P4 | **Tests come last, if at all.** AI generates implementation first because implementation is what was asked for. | Untested generated code accumulates faster than humans can verify it. |
| P5 | **Everything is vendor-locked.** Prompts, rules, skills, and workflows are trapped inside one vendor's tool. | Changing runtime means rebuilding the practice from zero. |
| P6 | **The environment is assumed.** Tools install, overwrite, and configure without first inspecting what already exists. | Working machines break. Existing projects get clobbered. |
| P7 | **Knowledge dies with the session.** Decisions, constraints, and rationale live in a chat log, not in the repository. | The next session — human or AI — starts uninformed. |

AEOS exists to solve these seven problems as a product, not as advice.

---

## 5. Mission and Vision

### 5.1 Mission

> **AEOS is an operating system that enables developers to build software through AI-assisted,
> human-supervised engineering workflows.**

AEOS orchestrates the complete engineering lifecycle — environment preparation, project
initialization, requirement analysis, architecture, TDD, agentic orchestration, code generation,
review, refactoring, testing, documentation, deployment, and maintenance — while performing no
language-model inference of its own.

### 5.2 Vision

A developer opens any project, on any operating system, with any AI runtime, and finds the same
engineering discipline waiting for them. The workflow is explicit. The rules are versioned. The
context is minimal and deliberate. Every consequential action is explained and approved before it
happens. When the runtime changes, the practice does not. When the developer leaves, the repository
still knows how the product is built.

### 5.3 Positioning

| Category | What it provides | Relationship to AEOS |
| :--- | :--- | :--- |
| AI models and APIs | Inference | Orchestrated by AEOS; the user chooses which |
| AI coding assistants and agentic runtimes | Interactive generation and tool use | Orchestrated by AEOS; never reimplemented by it |
| IDEs and editors | Editing surface | Environments AEOS operates alongside |
| Application frameworks | Runtime structure for the user's code | Unrelated; AEOS produces no application framework |
| Version control and CI/CD platforms | History, collaboration, automation | Orchestrated by AEOS, never replaced |
| **AEOS** | **Engineering process, project context, orchestration, and human supervision** | **The product that makes the others work together as an engineering practice** |

---

## 6. What AEOS Is and Is Not

| AEOS is | AEOS is not |
| :--- | :--- |
| An operating system for AI-assisted engineering work | An AI model or an inference engine |
| An orchestrator of external AI runtimes | A replacement for those runtimes |
| A product with a versioned repository as its core artifact | A library or an application framework |
| A human-supervised system with approval gates | An autonomous agent that acts on its own judgment |
| A process enforcer: TDD-first, explain-before-execute | A prompt collection or a template pack |
| Vendor, runtime, model, platform, and distribution independent | A wrapper around one vendor's ecosystem |
| An environment-aware system that inspects before it acts | An installer that assumes a clean machine |
| A manager of Repository Assets — rules, skills, prompts, workflows, and more — as versioned product artifacts | A hidden configuration store the user cannot read |
| An IDE-agnostic system usable from any editing surface | An IDE, an editor, or a UI toolkit |

---

## 7. Product Philosophy

These thirteen principles are mandatory. They constrain every capability, every requirement, and
every downstream design decision in this repository. A feature that violates a principle is not a
feature — it is a defect.

### 7.1 **1. Human-in-the-Loop by Default**

The human is the decision-maker. AEOS is the system that prepares decisions and executes them
faithfully once made.

Full automation is never the default and never inferred from context. It is granted explicitly, in
scope, by the owner of the project, and it can be withdrawn at any time. Where automation has been
granted, AEOS still records what it did so the human can audit the decision after the fact.

**Implication:** every workflow terminates in an approval gate before any consequential action.

### 7.2 **2. Explain Before Execute**

AEOS never performs a consequential action that the human has not first been able to understand.

Before execution, AEOS states what it found, what it intends to do, why it intends to do it, what
will change, and what happens if the action is declined. The explanation is written for a human
reader, not as a log dump.

**Implication:** an action with no explanation is not executable, regardless of who requested it.

### 7.3 **3. Incremental Execution**

Work advances in small, verifiable steps. Each step has a defined start state, a defined end state,
and a way to tell whether it succeeded.

Large operations are decomposed into a sequence of approvable steps rather than presented as a
single irreversible leap. A failed step stops the sequence; it does not cascade.

**Implication:** progress is always inspectable mid-flight, and interruption is always safe.

### 7.4 **4. TDD-first Development**

Tests are written before the implementation they verify. This applies to code AEOS helps generate,
and it applies to AEOS itself.

The cycle is explicit: define the behavior, write the failing test, confirm it fails for the right
reason, implement the minimum that passes, refactor under a green suite. AEOS treats a request to
skip this cycle as an exception requiring explicit human acknowledgment, not as a shortcut.

**Implication:** generated implementation without a preceding test is a process violation AEOS must surface.

### 7.5 **5. Repository as Product**

The repository is not where the product is stored. The repository *is* the product.

Requirements, rules, skills, prompts, workflows, profiles, decisions, and documentation are
Repository Assets: versioned artifacts living beside the code. What is not in the repository does
not exist. A session ends; the repository persists and remains authoritative for the next human and
the next AI.

**Implication:** no product-relevant state may live only in a chat session, a vendor account, or a machine-local cache.

### 7.6 **6. Context Minimization**

AEOS sends the smallest context that is sufficient for the task, and it knows why each piece was sent.

Context is selected deliberately and scoped to the active step, not accumulated by default. Less
context means lower cost, higher accuracy, better privacy, and a smaller blast radius when something
goes wrong. Bulk context transfer is a failure of design, not a feature.

**Implication:** every context selection must be explainable to the user on request.

### 7.7 **7. Vendor Independence**

No vendor is privileged. No vendor is required.

Product capabilities are defined in vendor-neutral terms. A vendor's absence reduces the runtimes
available to the user; it never disables AEOS itself.

**Implication:** no product capability may be defined, documented, or understood in terms of one vendor's offering.

### 7.8 **8. Runtime Independence**

Workflows are defined independently of the runtime that executes them. The same workflow runs on a
different runtime without being rewritten.

Runtimes differ in capability. AEOS reconciles this by declaring what a workflow needs and matching
it against what a runtime offers — not by encoding one runtime's assumptions into the workflow.

**Implication:** switching runtime is a configuration change, not a migration project.

### 7.9 **9. Model Independence**

AEOS is not tuned to one model family, one model size, or one generation of models.

Model selection belongs to the user. Models improve, deprecate, and change price; AEOS absorbs that
churn so the project does not have to. AEOS never assumes a specific model's quirks are universal.

**Implication:** no capability may depend on undocumented behavior of a specific model version.

### 7.10 **10. Platform Independence**

Windows, macOS, and Linux are equal citizens. Not one primary and two ports.

The same product behavior, the same workflows, and the same repository semantics apply on every
supported platform. Platform-specific handling exists to preserve identical behavior, not to justify
different behavior.

**Implication:** a capability that works on only one platform is incomplete, not shipped.

### 7.11 **11. Distribution Independence**

How AEOS was installed does not change what AEOS is.

Clone, native installer, MCP distribution, and portable distribution all deliver the same product
architecture, the same capabilities, and the same behavior. Distribution affects packaging,
discovery, and update mechanics — nothing else.

**Implication:** no capability may be exclusive to a distribution channel.

### 7.12 **12. Safety by Default**

The safe path is the default path, and the unsafe path requires a deliberate act.

AEOS inspects before it changes, prefers reversible operations, refuses to destroy without explicit
confirmation, keeps secrets out of context and out of logs, and fails closed when it is uncertain.
Uncertainty resolves toward asking, never toward proceeding.

**Implication:** "it probably would have been fine" is never a justification for an unconfirmed destructive action.

### 7.13 **13. Extensibility by Design**

AEOS is extended without being modified.

New runtimes, and new Repository Assets of every kind, are added as declared, versioned assets.
Extending AEOS must not require forking it, patching it, or understanding how it works inside.

**Implication:** if a common extension requires modifying AEOS itself, AEOS has failed to be extensible.

### 7.14 Principle Conflict Resolution

When two principles pull in opposite directions, resolve in this order:

1. **Safety by Default** — never trade safety for convenience.
2. **Human-in-the-Loop by Default** — when unsure, ask the human.
3. **Explain Before Execute** — never act faster than the human can understand.
4. **TDD-first Development** — never trade verification for speed.
5. **Repository as Product** — never let state escape the repository.
6. All remaining principles, weighed on the merits of the specific case.

---

## 8. Users

| User | Context | What AEOS must give them |
| :--- | :--- | :--- |
| **Solo developer** | Builds and maintains projects alone; uses AI heavily; has no process safety net. | A dependable process without a team to enforce it, fast setup on their machine, and confidence that nothing is changed behind their back. |
| **Engineering lead / architect** | Responsible for how a team builds, not only what it ships. | Rules, workflows, and standards expressed as versioned Repository Assets that apply uniformly to every developer and every runtime. |
| **Platform / DevOps engineer** | Owns environments, installation, and delivery across a heterogeneous fleet. | Predictable installation on all three platforms, non-destructive environment handling, and integration with existing repository and CI/CD systems. |
| **Open-source maintainer** | Accepts contributions from unknown contributors using unknown tools. | A repository that encodes the project's engineering practice so contributions and AI-assisted changes arrive already aligned. |
| **AI runtime** *(non-human consumer)* | Reads the repository to determine how to act within the project. | Unambiguous, minimal, machine-consumable definitions of rules, skills, prompts, and workflow state. |

---

## 9. The Engineering Lifecycle

AEOS orchestrates the complete engineering lifecycle. Every stage below is a first-class product
concern. Stages are ordered for readability; real projects re-enter them continuously.

| # | Stage | What AEOS does | Human decision point |
| :--- | :--- | :--- | :--- |
| 1 | **Environment preparation** | Inspects the machine, reports what exists, proposes only the missing or misaligned pieces. | Approve the environment plan. |
| 2 | **Project initialization** | Establishes or adopts a project, its profile, and its Repository Assets without overwriting existing work. | Approve initialization or adoption. |
| 3 | **Requirement analysis** | Turns intent into stated, traceable requirements; surfaces ambiguity as questions rather than assumptions. | Confirm requirements and resolve ambiguity. |
| 4 | **Architecture** | Captures structure, boundaries, and decisions as durable repository artifacts. | Approve architecture and decisions. |
| 5 | **TDD** | Drives the test-first cycle and refuses to advance to implementation without a failing test. | Approve test intent and accept the failing test. |
| 6 | **Agentic orchestration** | Sequences multi-step work across runtimes, holding each step to its approval gate. | Approve the plan; approve each consequential step. |
| 7 | **Code generation** | Delegates generation to the selected runtime with minimized, deliberate context. | Approve generated changes before they are applied. |
| 8 | **Review** | Evaluates changes against requirements, rules, and tests; classifies findings by severity. | Accept, reject, or request revision. |
| 9 | **Refactoring** | Improves structure under a green test suite, with behavior preservation as the stated goal. | Approve the refactoring scope. |
| 10 | **Testing** | Runs the project's own test tooling, reports results, and blocks progress on failure. | Decide how to respond to failures. |
| 11 | **Documentation** | Generates and maintains documentation from the repository's actual state. | Approve documentation changes. |
| 12 | **Deployment** | Orchestrates the project's existing delivery pipelines; never replaces them. | Explicit approval — always, without exception. |
| 13 | **Maintenance** | Supports ongoing change: drift detection, dependency and documentation currency, incremental improvement. | Approve maintenance actions. |

---

## 10. The Interaction Model

Every consequential AEOS action follows one loop. This loop is the product's defining behavior and
is not optional, not configurable away, and not shortened by urgency.

```text
        +-----------+
        |  INSPECT  |
        +-----+-----+
              |
              v
        +-----------+
        |  EXPLAIN  |
        +-----+-----+
              |
              v
        +-----------+
        |  PROPOSE  |
        +-----+-----+
              |
              v
        +-----------+
        |  CONFIRM  +------ declined ------+
        +-----+-----+                      |
              |                            |
           approved                        |
              |                            |
              v                            |
        +-----------+                      |
        |  EXECUTE  |                      |
        +-----+-----+                      |
              |                            |
              v                            |
        +-----------+                      |
        |  REPORT   |<---------------------+
        +-----------+
```

| Phase | Obligation |
| :--- | :--- |
| **Inspect** | Determine the actual current state before forming any intent. Never act on assumed state. |
| **Explain** | State what exists, in language a human can act on. Distinguish observed fact from inference. |
| **Propose** | State the intended action, its rationale, its effects, its reversibility, and the consequence of declining. Offer alternatives where they exist. |
| **Confirm** | Wait for explicit human approval. Silence is not approval. Ambiguity is not approval. A prior approval for a different action is not approval. |
| **Execute** | Perform exactly what was approved — no more. Scope expansion requires a new proposal. |
| **Report** | State what actually happened, including partial completion and failure, and record it in the repository. |

### 10.1 Action Classes

Not every action needs the same gate. Actions are classified by their effect on the user's system,
and the classification determines the approval required.

| Class | Definition | Examples | Approval |
| :--- | :--- | :--- | :--- |
| **Observation** | Reads state; changes nothing. | Inspecting the environment, reading Repository Assets, reporting status. | None required. |
| **Local change** | Changes state that is reversible within the repository. | Writing files, updating configuration, adding assets. | Explicit approval of the proposal. |
| **External effect** | Reaches outside the repository or the machine. | Installing software, invoking a runtime, pushing to a remote, triggering CI. | Explicit approval, with cost and scope stated. |
| **Destructive** | Loses information or is not reversible by AEOS. | Deleting files, overwriting uncommitted work, rewriting history, deploying. | Explicit, specific confirmation of that exact action. Never covered by a general approval. |

### 10.2 Automation Grants

Automation is a delegation of the human's authority, granted deliberately and always revocable.

- A grant is **explicit**: the human states it; AEOS never infers it from repetition or impatience.
- A grant is **scoped**: to specific action classes, a specific project, and a stated duration or session.
- A grant is **recorded**: it is a Repository Asset, not a hidden runtime setting.
- A grant is **bounded**: destructive actions are never automated by a general grant.
- A grant is **auditable**: automated actions are reported exactly as approved ones are.
- A grant is **revocable**: at any time, immediately, without justification.

---

## 11. Environment Philosophy

> **AEOS inspects before it acts. Always. On every machine. Every time.**

Before executing installation, configuration, generation, refactoring, or deployment, AEOS
determines what already exists. If something exists, AEOS explains the current state, proposes
actions, waits for confirmation, and only then executes.

| Finding | AEOS behavior |
| :--- | :--- |
| **Nothing exists** | Report the empty state. Propose creation. Execute after approval. |
| **Exists and is correct** | Report it. Propose no change. Do nothing. Reuse it. |
| **Exists and differs from expectation** | Report both states, state the difference and its consequence, propose reconciliation options including "leave as is". Never reconcile silently. |
| **Exists and is unrecognized** | Report the finding. Do not modify. Do not delete. Ask the human what it is. |
| **Exists and conflicts with a required action** | Stop. Explain the conflict. Propose resolution options with tradeoffs. Wait. Never force through. |
| **Cannot be determined** | Report the uncertainty explicitly. Never present an assumption as a finding. Fail closed. |

The user's machine belongs to the user. AEOS is a guest on it and behaves accordingly: it does not
uninstall what it did not install, does not reconfigure what it does not own, and does not treat a
pre-existing tool as a problem to be solved.

---

## 12. Product Capabilities

AEOS provides ten capabilities. Together they constitute the product. Each is defined here in
product terms; each is expressed as numbered requirements in [Section 18](#18-product-requirements).

| # | Capability | Purpose | ID prefix |
| :--- | :--- | :--- | :--- |
| C1 | **Environment management** | Know the machine; prepare it non-destructively. | `PR-ENV` |
| C2 | **Project management** | Establish, adopt, and describe projects. | `PR-PRJ` |
| C3 | **Workflow orchestration** | Define and drive engineering workflows with approval gates. | `PR-WFL` |
| C4 | **AI runtime orchestration** | Select, adapt, and coordinate external runtimes. | `PR-RUN` |
| C5 | **TDD workflow** | Enforce test-first development as the primary path. | `PR-TDD` |
| C6 | **Documentation generation** | Produce and maintain documentation from repository truth. | `PR-DOC` |
| C7 | **Rule management** | Express engineering constraints as versioned assets. | `PR-RUL` |
| C8 | **Skill management** | Package reusable engineering procedures. | `PR-SKL` |
| C9 | **Prompt management** | Manage prompts as versioned, portable, minimized assets. | `PR-PMT` |
| C10 | **Repository management** | Treat the repository as the product's source of truth, including version control and delivery integration. | `PR-REP` |

### 12.1 **C1 — Environment management**

AEOS understands the machine it runs on before it changes anything about it. It detects the
platform, the tooling relevant to the project, and the AI runtimes available for use. It reports
findings in human terms, proposes only what is missing or misaligned, and executes only after
approval. It never assumes a clean machine, never silently upgrades, and never removes what it did
not install. Environment state is a product concern because a workflow that cannot describe its
environment cannot be reproduced.

### 12.2 **C2 — Project management**

A project is the unit AEOS operates on. AEOS creates new projects and adopts existing ones — adoption
being the harder and more common case, and therefore the one that must be safest. Each project has a
profile describing what it is, how it is built and tested, which runtime it uses, and which rules
apply. The profile is a Repository Asset, readable by humans and by AI runtimes, and it is the reason
a new session can become useful immediately without re-explaining the project.

### 12.3 **C3 — Workflow orchestration**

Workflows encode how engineering work proceeds: the steps, their order, their preconditions, their
approval gates, and their success criteria. AEOS drives them incrementally, keeps observable state,
survives interruption, and can resume without losing position. Workflows are declared as versioned
assets rather than embedded in tool behavior, so a team's practice is inspectable, reviewable, and
portable across runtimes and machines.

### 12.4 **C4 — AI runtime orchestration**

AEOS coordinates external AI runtimes and performs no inference itself. It reports which runtimes
are available, lets the user select one and switch later without penalty, and automatically applies
the engineering capabilities appropriate to that choice. Where a selected runtime cannot support part
of the requested work, the user is told before the work begins rather than partway through. Where it
can, the workflow behaves as it would anywhere else. Runtime failure is handled as an ordinary
condition: reported clearly, never silently retried into cost, and never resolved by substituting a
different runtime without asking.

### 12.5 **C5 — TDD workflow**

TDD is a product capability, not a recommendation. AEOS drives the red-green-refactor cycle
explicitly: define behavior, write the failing test, verify the failure is for the right reason,
implement minimally, refactor green. It tracks cycle position, blocks implementation that has no
preceding test, and surfaces skipping as an explicit exception the human must acknowledge. This is
the primary means by which AI-generated code stays verifiable at the speed it is produced.

### 12.6 **C6 — Documentation generation**

Documentation is generated from what the repository actually contains, kept consistent with it, and
maintained as the project changes. AEOS produces documentation suitable for GitHub, for human
maintenance, and for AI consumption — the same artifact serving all three. It detects drift between
documentation and reality, proposes updates, and never publishes a document containing placeholders,
TODOs, or unfinished sections.

### 12.7 **C7 — Rule management**

Rules are the engineering constraints a project agrees to: standards, boundaries, required practices,
prohibitions. AEOS manages them as versioned Repository Assets with defined scope and precedence,
applies them during generation and review, reports violations with severity, and never applies a rule
the user cannot inspect. Rules are how an engineering lead's intent reaches every developer and every
runtime without being restated.

### 12.8 **C8 — Skill management**

Skills are packaged, reusable engineering procedures — a repeatable way to perform a specific kind of
work. AEOS discovers, versions, composes, and applies them, and makes clear which skill was applied
and why. Skills are runtime-independent so a team's accumulated know-how survives a change of vendor,
and they are additive so extending AEOS does not mean modifying it.

### 12.9 **C9 — Prompt management**

Prompts are engineering assets, not disposable text. AEOS manages them as versioned, parameterized,
portable artifacts, composes them from project context deliberately, and holds them to Context
Minimization: the smallest sufficient context, with the reason for each inclusion available on
request. Prompts remain inspectable before they are sent, because a prompt the user cannot read is a
decision the user did not make.

### 12.10 **C10 — Repository management**

The repository is the product. AEOS treats it as the single source of truth for code and for every
Repository Asset: requirements, rules, skills, prompts, workflows, profiles, decisions, and
documentation. Version control, branching,
history, and CI/CD integration are product capabilities: AEOS orchestrates the project's existing
Git and delivery systems, explains what it will do to them before doing it, and treats history
rewriting and deployment as destructive actions requiring specific confirmation. AEOS never replaces
the version control or CI/CD system it integrates with.

---

## 13. Repository Product Assets and Runtime State

The repository is the product. That principle only holds if it is clear what belongs in the
repository and what does not. This section draws that line as a product distinction — not as a
storage design.

### 13.1 Repository Product Assets

**Repository Assets** are the durable, versioned artifacts that together constitute the product.
They are what a project carries forward: the accumulated statement of what is being built, how it is
built, and why it is built that way.

Repository Assets include, but are not limited to:

| Asset | What it carries |
| :--- | :--- |
| **Rules** | The engineering constraints a project agrees to. |
| **Skills** | Reusable engineering procedures the project knows how to perform. |
| **Prompts** | Deliberate, minimized instruction and context used when work is delegated to a runtime. |
| **Workflows** | The engineering sequences a project follows, including where the human decides. |
| **Profiles** | What a project is, how it is built and tested, and which runtime and rules apply. |
| **Templates** | Reusable starting points for work the project performs repeatedly. |
| **Playbooks** | Established responses to recurring engineering situations. |
| **Recipes** | Known-good sequences for producing a specific, repeatable result. |
| **Specifications** | Precise statements of required behavior, traceable to product requirements. |
| **Architecture Documents** | The structural decisions that realize the product, and the reasoning behind them. |
| **Manuals** | Documentation for the humans and AI runtimes that will maintain the project. |

The list is open on purpose. New kinds of Repository Asset may be introduced without changing what a
Repository Asset *is*.

### 13.2 What Every Repository Asset Has in Common

These are observable product properties, not a definition of form.

| Property | What the user can rely on |
| :--- | :--- |
| **Durable** | It survives the session, the machine, and the runtime that produced it. |
| **Versioned** | It changes through the project's ordinary review and history, like code. |
| **Inspectable** | A human can read it and understand what it does before it takes effect. |
| **Consumable** | An AI runtime can act on it without a separate machine-only version existing. |
| **Portable** | It moves between machines, platforms, and distribution methods unchanged. |
| **Extensible** | Users add, modify, and remove their own without modifying AEOS. |

This document defines Repository Assets as product concepts only. It does not define their form,
their expression, their relationships to one another, or how they are organized. Those are
architecture and specification concerns.

### 13.3 Runtime State

**Runtime State** is everything AEOS produces or depends on while running that is *not* part of the
product. It is a consequence of execution, not a statement of what the product is.

Runtime State includes:

| Runtime State | Why it is not a product asset |
| :--- | :--- |
| **Cache** | An optimization. Losing it costs time, never meaning. |
| **Temporary execution state** | Belongs to a run in progress, not to the project. |
| **Credentials** | Belong to a person or an organization, never to a repository. |
| **Telemetry** | Describes usage, not the product being built. |
| **Generated temporary artifacts** | Reproducible from Repository Assets on demand. |
| **Machine-specific configuration** | True of one machine, and therefore not true of the project. |

### 13.4 The Distinction, Stated as a Test

> **If losing it costs only repeated work, it is Runtime State.**
> **If losing it costs product meaning, it is a Repository Asset.**

Runtime State is intentionally excluded from Repository Product Assets. A project must remain fully
understandable and reproducible without it. Nothing in this document specifies where Runtime State
lives, how long it persists, or how it is managed — those are runtime and architecture concerns.

---

## 14. Platform Support

Platform support is a core product capability, not a compatibility exercise.

| Platform | Status | Commitment |
| :--- | :--- | :--- |
| **Windows** | Officially supported | Full product capability. First-class citizen. |
| **macOS** | Officially supported | Full product capability. First-class citizen. |
| **Linux** | Officially supported | Full product capability. First-class citizen. |

**Commitments**

- Identical product capability on all three platforms. No platform-exclusive features.
- Identical workflow, rule, skill, prompt, and repository semantics across platforms.
- A project prepared on one platform is usable on another without modification.
- Platform differences are absorbed by the product, not exposed to the user or the workflow author.
- A capability is complete only when it works on all three. Partial platform support is not a release.

---

## 15. Distribution Strategy

AEOS supports multiple official distribution methods. **The product architecture is identical
regardless of installation method.**

### 15.1 Official at minimum

| Method | Description | Primary user |
| :--- | :--- | :--- |
| **GitHub Clone** | Clone the repository and use AEOS directly from source. | Contributors, teams standardizing on a pinned revision, users who want full transparency. |
| **Native Installer** | Platform-native installation for Windows, macOS, and Linux. | Developers who want a supported, updatable install with the least setup. |
| **MCP Distribution** | AEOS made available to MCP-capable AI runtimes and clients. | Users who work primarily inside an AI runtime or IDE and want AEOS available there. |
| **Portable Distribution** | Self-contained, relocatable, no system-level installation. | Locked-down machines, air-gapped environments, ephemeral and shared systems. |

### 15.2 Planned

| Method | Rationale |
| :--- | :--- |
| **Package Managers** | Native ecosystem installation and update paths per platform. |
| **Docker Images** | Reproducible, isolated environments for CI and containerized workflows. |
| **IDE Marketplace Distribution** | Discovery and installation from within the developer's editing surface. |

### 15.3 Distribution Invariants

1. Every distribution method delivers the same product architecture.
2. No capability is exclusive to a distribution method.
3. A project is portable across distribution methods without modification.
4. Installation method is a deployment detail, never a semantic difference.
5. Every method reports its version and origin so the user always knows what they are running.

---

## 16. Runtime Strategy

AEOS integrates external AI runtimes. It does not reimplement them, compete with them, or hide them.

> **AEOS remains independent from AI runtime implementation.**
> How that independence is achieved is an architecture concern, left open by this document on
> purpose. The product is defined by what the user can observe: any supported runtime, chosen by the
> user, running the same engineering work.

### 16.1 Runtime Examples

| Category | Examples |
| :--- | :--- |
| **Commercial AI services** | Claude · OpenAI · Gemini |
| **AI-assisted development environments** | Cursor · GitHub Copilot |
| **Open-source models** | Locally or privately hosted models of any size |
| **Interoperability standards** | MCP and comparable standards, present and future |
| **Extensions** | Plugins and integrations supplied by users or third parties |
| **Not yet released** | Runtime categories that do not exist at the time of writing |

This list is illustrative. Being named here confers no privilege; being absent implies no exclusion.
The final row is deliberate: AEOS is expected to outlive the current runtime landscape.

### 16.2 Integration Model

| Rule | Meaning |
| :--- | :--- |
| **Runtimes are integrations, not components** | An external runtime is never part of AEOS. AEOS remains whole and functional as runtimes come and go. |
| **Independent of runtime implementation** | AEOS is defined by what the user gets, never by how a runtime works inside. The means of integration belongs to architecture. |
| **Do not rebuild what exists** | Where a mature runtime already provides a capability well, AEOS orchestrates it rather than recreating it. |
| **Fit is reported, not assumed** | AEOS automatically applies the engineering capabilities a selected runtime supports, and states before work begins what it cannot support. |
| **The user chooses** | Runtime selection is the user's decision. AEOS may report suitability; it never overrides the choice or silently substitutes. |
| **Graceful degradation** | An unavailable runtime reduces available options. It never corrupts project state and never blocks non-inference capabilities. |
| **Cost and effect transparency** | Invoking a runtime is an external effect. The user knows before it happens, not after. |

### 16.3 Runtime Independence in Practice

A workflow authored against AEOS runs on a different runtime by changing the project's runtime
selection. The workflow file does not change. The rules do not change. The skills do not change. The
repository does not change. If any of those must change, runtime independence has been violated and
the violation is a defect in AEOS, not a limitation of the user's setup.

---

## 17. Product Scope

### 17.1 In Scope

All ten capabilities in [Section 12](#12-product-capabilities), plus explicitly:

| Capability | AEOS role |
| :--- | :--- |
| **Version control** | Product capability. AEOS understands and orchestrates the project's version control state. |
| **Git** | Product capability. AEOS integrates with Git operations under the standard approval gates. |
| **CI/CD integration** | Product capability. AEOS integrates with existing pipelines and delivery systems. |
| **Code generation** | Product capability, delegated to runtimes and governed by AEOS workflow, rules, and gates. |
| **Testing** | Product capability. AEOS orchestrates the project's test tooling and gates progress on results. |
| **Review** | Product capability. AEOS evaluates changes against requirements, rules, and tests with severity-classified findings. |
| **Deployment** | Product capability. AEOS orchestrates the project's delivery systems with explicit approval, always. |

### 17.2 Out of Scope

Only the following are outside the product. Everything else is in scope; implementation detail
belongs to downstream documents, not to exclusion.

| Excluded | Reason |
| :--- | :--- |
| **Language-model inference** | AEOS performs no inference. It orchestrates runtimes that do. |
| **Model training, fine-tuning, or hosting** | Belongs to model vendors and open-source model providers. |
| **Replacing version control, CI/CD, or hosting platforms** | AEOS integrates with these systems; it does not become one. |
| **Being an IDE, editor, or application framework** | AEOS operates alongside editing surfaces and produces no application framework. |
| **Implementation detail** | Architecture, interfaces, data formats, schemas, algorithms, file layout, and technology choices belong to downstream architecture and specification documents. |

---

## 18. Product Requirements

| Field | Meaning |
| :--- | :--- |
| **ID** | Stable identifier. Downstream documents, tests, and issues reference it. IDs are never reused or renumbered. |
| **Priority** | `P0` product cannot ship without it · `P1` required for a complete product · `P2` planned enhancement |
| **Phase** | Target release phase — see [Section 22](#22-release-phases). |

Requirements state product behavior as observable outcomes. They deliberately avoid prescribing
mechanism: each one should admit more than one honest implementation.

> **Requirement identifiers are immutable.**
> An identifier is bound to a requirement for the life of the product. Wording may be refined,
> priority may change, and phase may move — the identifier never does. It is never renumbered, never
> renamed, and never reassigned to different intent. This is what makes long-term traceability
> possible across architecture documents, specifications, tests, issues, and releases that will be
> written years apart.

---

### 18.1 Environment Management — `PR-ENV`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-ENV-001` | AEOS inspects the environment before proposing or performing any environment-affecting action. | P0 | 1 |
| `PR-ENV-002` | AEOS detects the host platform and adapts its behavior to preserve identical product semantics across Windows, macOS, and Linux. | P0 | 1 |
| `PR-ENV-003` | AEOS reports the discovered environment state to the user in human-readable form, distinguishing observed facts from inferences. | P0 | 1 |
| `PR-ENV-004` | AEOS detects AI runtimes available on the machine and reports which are usable for the current project. | P0 | 1 |
| `PR-ENV-005` | AEOS detects project-relevant tooling (build, test, version control, delivery) rather than assuming its presence. | P0 | 1 |
| `PR-ENV-006` | When a required component is missing, AEOS proposes an action and executes only after explicit approval. | P0 | 1 |
| `PR-ENV-007` | When a component already exists and is correct, AEOS reuses it and proposes no change. | P0 | 1 |
| `PR-ENV-008` | When a component exists but differs from expectation, AEOS reports both states and the difference, and proposes reconciliation options including taking no action. | P0 | 1 |
| `PR-ENV-009` | AEOS never modifies, replaces, or removes a component it did not install without explicit, specific confirmation. | P0 | 1 |
| `PR-ENV-010` | When environment state cannot be determined, AEOS reports the uncertainty and stops rather than assuming. | P0 | 1 |
| `PR-ENV-011` | AEOS provides an on-demand environment report the user can inspect at any time without changing state. | P1 | 1 |
| `PR-ENV-012` | AEOS records environment findings in the project so a workflow's environment assumptions are reproducible. | P1 | 2 |
| `PR-ENV-013` | AEOS detects when the environment has drifted from previously recorded findings and reports the drift. | P2 | 3 |

---

### 18.2 Project Management — `PR-PRJ`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-PRJ-001` | AEOS initializes a new project with the assets required for AEOS-governed work. | P0 | 1 |
| `PR-PRJ-002` | AEOS adopts an existing project without overwriting, relocating, or restructuring existing content. | P0 | 1 |
| `PR-PRJ-003` | Before initialization or adoption, AEOS inspects the target location and reports what it found. | P0 | 1 |
| `PR-PRJ-004` | AEOS maintains a project profile describing the project's identity, technology, build and test approach, selected runtime, and applicable rules. | P0 | 1 |
| `PR-PRJ-005` | The project profile is a versioned Repository Asset, readable and editable by humans and consumable by AI runtimes. | P0 | 1 |
| `PR-PRJ-006` | AEOS proposes profile values derived from inspection and requires confirmation before recording them. | P0 | 1 |
| `PR-PRJ-007` | AEOS reports current project status — profile, workflow position, runtime selection, and outstanding decisions — on demand. | P0 | 1 |
| `PR-PRJ-008` | A project is portable: it functions identically on any supported platform and under any distribution method. | P0 | 2 |
| `PR-PRJ-009` | Users can work on multiple independent projects on one machine; work in one never affects another. | P1 | 2 |
| `PR-PRJ-010` | AEOS detects that a project's recorded profile no longer matches the repository's actual state and reports the divergence. | P1 | 3 |
| `PR-PRJ-011` | AEOS supports removing its own project assets cleanly, leaving the user's project intact. | P1 | 2 |

---

### 18.3 Workflow Orchestration — `PR-WFL`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-WFL-001` | AEOS provides workflows covering every stage of the engineering lifecycle defined in Section 9. | P0 | 1–3 |
| `PR-WFL-002` | Workflows are declared as versioned Repository Assets, inspectable and reviewable by humans. | P0 | 1 |
| `PR-WFL-003` | Workflows are defined independently of any AI runtime and execute unchanged across runtimes. | P0 | 1 |
| `PR-WFL-004` | AEOS executes workflows incrementally, one verifiable step at a time. | P0 | 1 |
| `PR-WFL-005` | Every consequential step follows Inspect → Explain → Propose → Confirm → Execute → Report. | P0 | 1 |
| `PR-WFL-006` | AEOS classifies each action as observation, local change, external effect, or destructive, and applies the corresponding approval requirement. | P0 | 1 |
| `PR-WFL-007` | Users can see where a workflow currently stands, what has been completed, and what decisions remain outstanding. | P0 | 1 |
| `PR-WFL-008` | Users can safely pause an engineering workflow and resume it later without losing position or re-establishing context. | P0 | 2 |
| `PR-WFL-009` | A declined proposal halts the workflow without side effects and without penalty. | P0 | 1 |
| `PR-WFL-010` | A failed step halts the workflow, reports the failure, and does not proceed to dependent steps. | P0 | 1 |
| `PR-WFL-011` | AEOS reports what actually occurred after execution, including partial completion. | P0 | 1 |
| `PR-WFL-012` | AEOS supports agentic orchestration: multi-step work sequenced across runtimes, with each consequential step held to its approval gate. | P0 | 2 |
| `PR-WFL-013` | Users can define project-specific workflows without modifying AEOS itself. | P1 | 2 |
| `PR-WFL-014` | Automation grants are explicit, scoped, recorded, revocable, and never extend to destructive actions. | P1 | 2 |
| `PR-WFL-015` | AEOS records an auditable history of proposals, decisions, and executions within the project. | P1 | 2 |
| `PR-WFL-016` | AEOS reports which workflow steps a selected runtime cannot satisfy before execution begins. | P1 | 2 |

---

### 18.4 AI Runtime Orchestration — `PR-RUN`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-RUN-001` | AEOS performs no language-model inference of its own under any circumstance. | P0 | 1 |
| `PR-RUN-002` | AEOS remains independent from AI runtime implementation, and does not reproduce capabilities that runtimes already provide well. | P0 | 1 |
| `PR-RUN-003` | AEOS supports multiple runtimes and is not dependent on any single vendor. | P0 | 1 |
| `PR-RUN-004` | The user selects the runtime; AEOS never overrides or silently substitutes that selection. | P0 | 1 |
| `PR-RUN-005` | Switching runtime requires no change to workflows, rules, skills, prompts, or repository structure. | P0 | 1 |
| `PR-RUN-006` | AEOS is independent of any specific model, model family, or model version. | P0 | 1 |
| `PR-RUN-007` | AEOS automatically selects engineering capabilities appropriate to the chosen runtime, and reports anything the runtime cannot support before work begins. | P0 | 2 |
| `PR-RUN-008` | A workflow produces consistent, comparable outcomes regardless of which runtime performed the work. | P0 | 2 |
| `PR-RUN-009` | Invoking a runtime is an external effect requiring explicit approval, with scope and expected cost stated beforehand. | P0 | 1 |
| `PR-RUN-010` | Runtime unavailability degrades available options without corrupting project state or blocking non-inference capabilities. | P0 | 1 |
| `PR-RUN-011` | AEOS reports runtime errors clearly and never silently retries in a way that incurs unapproved cost. | P0 | 1 |
| `PR-RUN-012` | Support for a new AI runtime can be added without modifying AEOS itself and without changing existing projects. | P1 | 2 |
| `PR-RUN-013` | AEOS supports orchestrating more than one runtime within a single workflow when the user configures it. | P1 | 3 |
| `PR-RUN-014` | Runtime credentials are never written into prompts, logs, documentation, or Repository Assets. | P0 | 1 |
| `PR-RUN-015` | AEOS reports runtime usage per project so users can attribute cost and activity. | P2 | 3 |
| `PR-RUN-016` | AEOS can support new categories of AI runtime — commercial services, open-source models, interoperability standards, and user-supplied extensions — without changes to existing projects or Repository Assets. | P1 | 3 |

---

### 18.5 TDD Workflow — `PR-TDD`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-TDD-001` | TDD is the default development workflow for all AEOS-governed code work. | P0 | 1 |
| `PR-TDD-002` | AEOS requires a stated, confirmed behavior before a test is written. | P0 | 1 |
| `PR-TDD-003` | AEOS requires a failing test before implementation begins. | P0 | 1 |
| `PR-TDD-004` | AEOS verifies that a new test fails for the intended reason before proceeding. | P0 | 1 |
| `PR-TDD-005` | AEOS drives implementation toward the minimum change that satisfies the failing test. | P0 | 1 |
| `PR-TDD-006` | AEOS permits refactoring only while the test suite is passing. | P0 | 1 |
| `PR-TDD-007` | AEOS tracks and reports the current position in the red-green-refactor cycle. | P0 | 1 |
| `PR-TDD-008` | Skipping the TDD cycle is an explicit exception the user must acknowledge; it is never silent and never the default. | P0 | 1 |
| `PR-TDD-009` | AEOS orchestrates the project's existing test tooling rather than providing its own test framework. | P0 | 1 |
| `PR-TDD-010` | Test failures halt workflow progression and are reported with enough detail to act on. | P0 | 1 |
| `PR-TDD-011` | AEOS reports untested changes introduced outside the TDD cycle. | P1 | 2 |
| `PR-TDD-012` | AEOS applies the same TDD requirements to its own development. | P0 | 1 |

---

### 18.6 Documentation Generation — `PR-DOC`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-DOC-001` | AEOS generates documentation from the repository's actual state, not from assumption or from chat history. | P0 | 1 |
| `PR-DOC-002` | Generated documentation is complete: no placeholders, no TODOs, no unfinished sections. | P0 | 1 |
| `PR-DOC-003` | Generated documentation is internally consistent and consistent with the repository it describes. | P0 | 1 |
| `PR-DOC-004` | Generated documentation renders correctly on GitHub and remains valid Markdown. | P0 | 1 |
| `PR-DOC-005` | Generated documentation is suitable for human maintenance and for AI consumption from the same artifact. | P0 | 1 |
| `PR-DOC-006` | Documentation changes are proposed and approved before being written, like any other local change. | P0 | 1 |
| `PR-DOC-007` | AEOS maintains documentation as the project changes rather than generating it once. | P1 | 2 |
| `PR-DOC-008` | AEOS detects drift between documentation and repository state and reports it. | P1 | 3 |
| `PR-DOC-009` | AEOS supports project-defined documentation conventions without modification of AEOS itself. | P1 | 2 |
| `PR-DOC-010` | AEOS never publishes generated documentation containing unresolved uncertainty presented as fact. | P0 | 1 |

---

### 18.7 Rule Management — `PR-RUL`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-RUL-001` | Rules are versioned Repository Assets, inspectable and editable by humans. | P0 | 1 |
| `PR-RUL-002` | Rules are defined independently of any runtime and apply identically across runtimes. | P0 | 1 |
| `PR-RUL-003` | Each rule has a defined scope determining where it applies. | P0 | 1 |
| `PR-RUL-004` | Rule precedence is deterministic and explainable when rules overlap or conflict. | P0 | 2 |
| `PR-RUL-005` | AEOS applies applicable rules during generation, review, and refactoring. | P0 | 1 |
| `PR-RUL-006` | Rule violations are reported with severity and location. | P0 | 2 |
| `PR-RUL-007` | AEOS reports which rules were applied to a given action on request. | P0 | 2 |
| `PR-RUL-008` | Users add, modify, and remove rules without modifying AEOS itself. | P0 | 1 |
| `PR-RUL-009` | AEOS never applies a rule the user cannot inspect. | P0 | 1 |
| `PR-RUL-010` | AEOS reports rules that cannot be enforced under the selected runtime rather than silently ignoring them. | P1 | 3 |

---

### 18.8 Skill Management — `PR-SKL`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-SKL-001` | Skills are versioned Repository Assets packaging reusable engineering procedures. | P0 | 2 |
| `PR-SKL-002` | Skills are defined independently of any runtime and apply identically across runtimes. | P0 | 2 |
| `PR-SKL-003` | AEOS discovers available skills and reports them to the user. | P0 | 2 |
| `PR-SKL-004` | Skill selection and application are explained before execution. | P0 | 2 |
| `PR-SKL-005` | Users add, modify, and remove skills without modifying AEOS itself. | P0 | 2 |
| `PR-SKL-006` | Skills are composable within a workflow, with composition made visible to the user. | P1 | 3 |
| `PR-SKL-007` | Skills are versioned so a project can depend on a known skill revision. | P1 | 3 |
| `PR-SKL-008` | AEOS reports which skill was applied to a given action and why. | P1 | 2 |
| `PR-SKL-009` | Skills are portable between projects and between machines without modification. | P1 | 3 |

---

### 18.9 Prompt Management — `PR-PMT`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-PMT-001` | Prompts are versioned Repository Assets that outlive the session in which they were used. | P0 | 1 |
| `PR-PMT-002` | Prompts are composed from project context deliberately and are parameterizable. | P0 | 2 |
| `PR-PMT-003` | AEOS applies Context Minimization: the smallest context sufficient for the step. | P0 | 1 |
| `PR-PMT-004` | AEOS explains why each element of context was included, on request. | P0 | 2 |
| `PR-PMT-005` | The user can inspect the composed prompt before it is sent to a runtime. | P0 | 1 |
| `PR-PMT-006` | Prompts remain usable when a project changes runtime. | P0 | 2 |
| `PR-PMT-007` | Users add, modify, and remove prompts without modifying AEOS itself. | P0 | 2 |
| `PR-PMT-008` | AEOS never includes credentials or user-designated sensitive content in a prompt. | P0 | 1 |
| `PR-PMT-009` | AEOS reports the context size of a composed prompt before sending it. | P1 | 2 |
| `PR-PMT-010` | AEOS supports project-defined prompt conventions and overrides. | P1 | 3 |

---

### 18.10 Repository Management — `PR-REP`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-REP-001` | The repository is the authoritative source of truth for all AEOS project state. | P0 | 1 |
| `PR-REP-002` | Everything needed to understand and reproduce the project lives in the repository; nothing essential exists only outside it. | P0 | 1 |
| `PR-REP-003` | AEOS integrates with the project's version control system and reports its state before proposing changes. | P0 | 1 |
| `PR-REP-004` | AEOS supports Git operations required by its workflows, each under the appropriate approval gate. | P0 | 2 |
| `PR-REP-005` | AEOS never modifies version control history without explicit, specific confirmation of that exact operation. | P0 | 2 |
| `PR-REP-006` | AEOS never discards uncommitted work without explicit, specific confirmation. | P0 | 1 |
| `PR-REP-007` | AEOS integrates with the project's existing CI/CD systems and never replaces them. | P0 | 3 |
| `PR-REP-008` | AEOS orchestrates deployment through the project's delivery systems and requires explicit approval for every deployment. | P0 | 3 |
| `PR-REP-009` | AEOS Repository Assets are human-readable, diffable, and reviewable in ordinary code review. | P0 | 1 |
| `PR-REP-010` | AEOS Repository Assets are consumable by AI runtimes without transformation. | P0 | 1 |
| `PR-REP-011` | AEOS supports review workflows that evaluate changes against requirements, rules, and tests, reporting findings classified as Critical, Major, Minor, or Nitpick. | P0 | 2 |
| `PR-REP-012` | AEOS records architectural and product decisions as durable repository artifacts. | P1 | 2 |
| `PR-REP-013` | AEOS never writes credentials or secrets into Repository Assets. | P0 | 1 |
| `PR-REP-014` | AEOS reports when the repository has changed outside its knowledge and re-inspects before proceeding. | P1 | 2 |
| `PR-REP-015` | AEOS distinguishes Repository Assets from Runtime State, and never requires Runtime State in order to understand or reproduce a project. | P0 | 2 |
| `PR-REP-016` | Repository Assets remain readable and meaningful when AEOS is not running. | P1 | 2 |

---

### 18.11 Platform — `PR-PLT`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-PLT-001` | AEOS officially supports Windows, macOS, and Linux with equal capability. | P0 | 1 |
| `PR-PLT-002` | No product capability is exclusive to one platform. | P0 | 1 |
| `PR-PLT-003` | Workflows, rules, skills, prompts, and repository semantics behave identically on all supported platforms. | P0 | 1 |
| `PR-PLT-004` | A project prepared on one platform is usable on another without modification. | P0 | 2 |
| `PR-PLT-005` | Platform differences are absorbed by AEOS and not exposed to users or workflow authors. | P0 | 1 |
| `PR-PLT-006` | A capability is considered complete only when it functions on all three supported platforms. | P0 | 1 |

---

### 18.12 Distribution — `PR-DST`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-DST-001` | AEOS is distributed via GitHub Clone. | P0 | 1 |
| `PR-DST-002` | AEOS is distributed via native installers for Windows, macOS, and Linux. | P0 | 2 |
| `PR-DST-003` | AEOS is distributed via MCP distribution for MCP-capable runtimes and clients. | P0 | 2 |
| `PR-DST-004` | AEOS is distributed as a portable, self-contained distribution requiring no system-level installation. | P0 | 2 |
| `PR-DST-005` | The product architecture is identical across all distribution methods. | P0 | 1 |
| `PR-DST-006` | No product capability is exclusive to a distribution method. | P0 | 1 |
| `PR-DST-007` | A project is portable across distribution methods without modification. | P0 | 2 |
| `PR-DST-008` | Every distribution reports its version and origin. | P0 | 1 |
| `PR-DST-009` | Installation inspects the environment first and never overwrites an existing installation without approval. | P0 | 2 |
| `PR-DST-010` | Every distribution supports clean removal. | P1 | 2 |
| `PR-DST-011` | AEOS is distributed via platform package managers. | P2 | 4 |
| `PR-DST-012` | AEOS is distributed as Docker images. | P2 | 4 |
| `PR-DST-013` | AEOS is distributed via IDE marketplaces. | P2 | 4 |

---

### 18.13 Safety — `PR-SAF`

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| `PR-SAF-001` | The safe path is the default path in every workflow. | P0 | 1 |
| `PR-SAF-002` | AEOS fails closed: when uncertain, it stops and asks rather than proceeding. | P0 | 1 |
| `PR-SAF-003` | Destructive actions require explicit, specific confirmation and are never covered by a general approval. | P0 | 1 |
| `PR-SAF-004` | AEOS prefers reversible operations and states reversibility in every proposal. | P0 | 1 |
| `PR-SAF-005` | AEOS executes only the approved action; scope expansion requires a new proposal. | P0 | 1 |
| `PR-SAF-006` | Credentials and secrets are never placed in prompts, logs, reports, documentation, or Repository Assets. | P0 | 1 |
| `PR-SAF-007` | AEOS does not transmit project content to any runtime that the user has not selected and approved. | P0 | 1 |
| `PR-SAF-008` | AEOS reports what will leave the machine before it leaves. | P0 | 1 |
| `PR-SAF-009` | AEOS does not modify or remove components outside the project that it did not install. | P0 | 1 |
| `PR-SAF-010` | Interruption at any point leaves the project in a consistent, describable state. | P0 | 2 |
| `PR-SAF-011` | AEOS never presents an inference as an observed fact. | P0 | 1 |
| `PR-SAF-012` | Automation grants never authorize destructive actions. | P0 | 2 |

---

## 19. Quality Attributes

Product-level qualities. They apply to every capability and every requirement above.

| ID | Attribute | Product commitment |
| :--- | :--- | :--- |
| `PR-NFR-001` | **Transparency** | Every AEOS decision is explainable to the user on request: what was inspected, what was proposed, what was applied, and why. |
| `PR-NFR-002` | **Reproducibility** | The same project, workflow, and inputs produce the same AEOS behavior on any supported platform and distribution. |
| `PR-NFR-003` | **Responsiveness** | Inspection and reporting complete quickly enough to precede every action without discouraging their use. |
| `PR-NFR-004` | **Efficiency** | Context sent to runtimes is minimized by design; unnecessary context is a defect, not overhead. |
| `PR-NFR-005` | **Resilience** | Runtime, network, and tooling failures are handled as expected conditions, never as corrupting events. |
| `PR-NFR-006` | **Maintainability** | The repository favors clarity over cleverness and remains maintainable by contributors who did not write it. |
| `PR-NFR-007` | **Extensibility** | Runtimes and Repository Assets of every kind are added without modifying AEOS itself. |
| `PR-NFR-008` | **Portability** | Projects and assets move between platforms, machines, and distributions without modification. |
| `PR-NFR-009` | **Comprehensibility** | Every product artifact is intelligible to both humans and AI runtimes without a separate machine format. |
| `PR-NFR-010` | **Verifiability** | AEOS is developed under its own TDD requirements; product behavior is covered by tests written before implementation. |
| `PR-NFR-011` | **Privacy** | Project content leaves the machine only through a user-approved runtime, and only what was disclosed in the proposal. |
| `PR-NFR-012` | **Compatibility** | AEOS coexists with the user's existing tools, editors, pipelines, and conventions rather than requiring their replacement. |

---

## 20. Safety and Trust Model

AEOS operates on the user's machine, inside the user's repository, spending the user's money, on
behalf of the user's judgment. Trust is the product's primary asset and it is not recoverable once
spent.

### 20.1 Trust Boundaries

| Boundary | AEOS position |
| :--- | :--- |
| **The machine** | Belongs to the user. AEOS inspects freely, changes only with approval, and never touches what it does not own. |
| **The repository** | Belongs to the user. AEOS proposes changes; the user accepts them. History and uncommitted work are treated as irreplaceable. |
| **The runtime** | External. Everything crossing this boundary is disclosed before it crosses. Nothing crosses without approval. |
| **Credentials** | Never enter prompts, logs, reports, documentation, or Repository Assets. They are runtime state, never a product asset, and are never recorded. |
| **The user's judgment** | Final. AEOS may disagree and say so, but it does not act against a decision or re-litigate it by repetition. |

### 20.2 Failure Posture

- **Fail closed.** Uncertainty stops the workflow; it does not resolve toward action.
- **Fail visibly.** Failures are reported in the same detail as successes. Silent failure is a defect.
- **Fail consistently.** An interrupted or failed operation leaves a state AEOS can describe.
- **Fail without cost.** A halted workflow does not incur unapproved runtime usage.
- **Fail without blame.** Declining a proposal is a normal outcome, never treated as an error.

---

## 21. Success Metrics

Success is measured by whether AEOS changes how software gets built, not by activity counts.

| Category | Metric | Signal of success |
| :--- | :--- | :--- |
| **Adoption** | Projects under AEOS governance | AEOS is adopted for real work, not evaluated and abandoned. |
| **Adoption** | Projects still governed after 90 days | The discipline survives contact with deadlines. |
| **Discipline** | Share of code changes produced through the TDD cycle | Test-first is the actual path, not the documented one. |
| **Discipline** | Frequency of acknowledged TDD exceptions | Exceptions remain exceptional. |
| **Supervision** | Share of consequential actions preceded by an approved proposal | The gate holds in practice. |
| **Supervision** | Proposal decline rate | Non-zero: users are genuinely deciding, not rubber-stamping. |
| **Independence** | Projects that have switched runtime without changing workflows | Runtime independence is real, not aspirational. |
| **Independence** | Distinct runtimes in active use across projects | The product is not a single-vendor wrapper in practice. |
| **Portability** | Successful installations across all three platforms | Platform independence holds on real machines. |
| **Portability** | Projects used across more than one platform or distribution | Portability is exercised, not merely claimed. |
| **Efficiency** | Context size per workflow step over time | Context Minimization improves rather than erodes. |
| **Efficiency** | Runtime invocations required per completed workflow | Orchestration reduces waste rather than adding overhead. |
| **Trust** | Incidents of unintended destructive change | Target: zero. This is the metric that matters most. |
| **Trust** | Documentation drift detected and resolved | The repository stays true to itself. |

---

## 22. Release Phases

Phases describe capability maturity. Dates are set by the owner and are not part of this document.

| Phase | Name | Goal | Exit criteria |
| :--- | :--- | :--- | :--- |
| **1** | Foundation | A trustworthy core: inspect, explain, propose, confirm, execute — on all three platforms, with TDD enforced and at least one runtime integrated. | All Phase 1 `P0` requirements met on Windows, macOS, and Linux via GitHub Clone. AEOS builds itself under its own TDD workflow. |
| **2** | Orchestration | Full workflow orchestration, agentic sequencing, skills, capability matching, and the four minimum distribution methods. | All Phase 2 `P0` requirements met. A workflow runs unchanged on at least two runtimes. All four minimum distributions ship. |
| **3** | Lifecycle Completion | Remaining lifecycle stages — deployment, maintenance, drift detection, multi-runtime orchestration, CI/CD integration. | All Phase 3 `P0` requirements met. Every lifecycle stage in Section 9 is supported end to end. |
| **4** | Ecosystem | Broader distribution reach and ecosystem maturity. | Package manager, Docker, and IDE marketplace distributions available with identical product architecture. |

**Phase invariants.** No phase may relax a product principle. No phase may ship a capability on a
subset of supported platforms. No phase may introduce a distribution-exclusive capability. No phase
may weaken an approval gate to meet a date.

---

## 23. Assumptions, Dependencies, and Risks

### 23.1 Assumptions

1. Users have, or can obtain, access to at least one external AI runtime.
2. Users work in repositories under version control, or are willing to adopt it.
3. Users accept human supervision as a benefit rather than an obstacle.
4. Projects have, or can adopt, a test approach compatible with test-first development.
5. The AI runtime landscape will keep changing, and AEOS must absorb that change rather than track it.

### 23.2 Dependencies

| Dependency | Nature | Mitigation |
| :--- | :--- | :--- |
| External AI runtimes | Required for inference-dependent capabilities | Multiple runtimes supported; non-inference capabilities remain functional without any. |
| Version control systems | Required for repository capabilities | Integration, not replacement; state is inspected before use. |
| Project build and test tooling | Required for TDD and testing capabilities | Orchestrated, not provided; detected rather than assumed. |
| CI/CD and delivery platforms | Required for deployment capabilities | Integration only; AEOS never becomes the pipeline. |
| Host operating systems | Required for all capabilities | Three officially supported platforms with equal commitment. |

### 23.3 Risks

| Risk | Impact | Product response |
| :--- | :--- | :--- |
| **Approval fatigue** — users tire of confirming and stop reading proposals. | High | Action classes keep observation friction-free; proposals are concise and decision-oriented; scoped automation grants exist for repetitive low-risk work. |
| **Perceived slowness** — the disciplined path feels slower than raw AI chat. | High | Incremental execution and context minimization keep steps fast; the value proposition is verified work, and the product does not compete on unverified speed. |
| **Runtime churn** — vendors change APIs, models, and pricing. | Medium | The product is defined independently of runtime implementation; no capability depends on undocumented runtime behavior. |
| **Capability divergence** — runtimes differ enough to leak into workflows. | Medium | Capability fit is reported before work begins; gaps surface to the user rather than being papered over. |
| **Scope creep toward reimplementation** — rebuilding what runtimes already do well. | Medium | Runtime Strategy prohibits it; the architecture freeze routes such ideas to Appendix A. |
| **Platform inequality** — one platform quietly becomes primary. | Medium | A capability is incomplete until it works on all three; this is a release gate, not a preference. |
| **Asset sprawl** — rules, skills, and prompts accumulate beyond comprehension. | Low | Assets are versioned, scoped, inspectable, and reviewable in ordinary code review. |

---

## 24. Glossary

| Term | Definition |
| :--- | :--- |
| **AEOS** | AI Engineering Operating System. The product defined by this document. |
| **Action class** | Classification of an action by effect — observation, local change, external effect, or destructive — determining the approval required. |
| **Agentic orchestration** | Sequencing multi-step engineering work across runtimes, with each consequential step held to its approval gate. |
| **Approval gate** | The point at which a proposed action requires explicit human confirmation before execution. |
| **Automation grant** | An explicit, scoped, recorded, revocable delegation of approval authority for specific action classes. |
| **Context Minimization** | The principle that AEOS sends the smallest context sufficient for the task and can explain each inclusion. |
| **Distribution method** | An official way AEOS is delivered to users. Never changes the product architecture. |
| **Environment inspection** | Determining actual machine and project state before proposing or performing an action. |
| **Human-in-the-Loop** | The requirement that a human decides before AEOS acts consequentially. |
| **Product Boundary** | The line this document draws between product definition and implementation. What AEOS is belongs here; how AEOS is built belongs to architecture, specification, and runtime documents. |
| **Project profile** | The versioned Repository Asset describing a project's identity, technology, build and test approach, runtime selection, and applicable rules. |
| **Prompt** | A versioned, parameterized, portable Repository Asset composed of deliberately selected context and instruction. |
| **Proposal** | A statement of intended action including rationale, effects, reversibility, and the consequence of declining. |
| **Repository Asset** | Any durable, versioned artifact that forms part of the product and lives in the repository. Includes rules, skills, prompts, workflows, profiles, templates, playbooks, recipes, specifications, architecture documents, and manuals. |
| **Repository as Product** | The principle that the repository is the product and its authoritative source of truth. |
| **Rule** | A versioned, scoped engineering constraint applied during generation, review, and refactoring. |
| **Runtime** | An external AI system that performs inference. Always an integration, never a part of AEOS. |
| **Runtime State** | Transient, machine-local, or environment-specific information produced while AEOS runs. Not a Repository Asset and not part of the product. |
| **Skill** | A versioned, reusable, runtime-independent packaged engineering procedure. |
| **TDD cycle** | Define behavior → failing test → verify failure reason → minimal implementation → refactor green. |
| **Workflow** | A versioned, runtime-independent declaration of engineering steps, preconditions, approval gates, and success criteria. |

---

## 25. Document Governance

### 25.1 Status of This Document

This PRD is the **Product Source of Truth** for the AEOS repository. All downstream documents —
architecture, specifications, implementation guides, and tests — derive from it and trace to its
requirement identifiers.

### 25.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction, no semantic change | Contributor change, owner acceptance | Patch |
| New requirement, or clarification of an existing one | Owner approval | Minor |
| Change to a product principle, scope, or capability set | Explicit owner revision request | Major |
| Removal of a requirement | Explicit owner approval with recorded rationale | Major |

### 25.3 Requirement Identifier Policy

Identifiers are permanent. They are never reused, never renumbered, and never repurposed. A retired
requirement is marked retired in place, retaining its ID and its rationale.

### 25.4 Architecture Freeze

The product definition in this document is frozen unless the owner explicitly requests a revision.
Ideas that would alter the product's concepts, capability set, or principles are recorded in
[Appendix A](#appendix-a--recommendations-for-future-releases) rather than applied.

### 25.5 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical
or Major findings remain.

### 25.6 Traceability

| Layer | Obligation |
| :--- | :--- |
| Architecture documents | Every architectural decision traces to one or more `PR-` identifiers. |
| Specifications | Every specified behavior traces to a `PR-` identifier. |
| Tests | Every `P0` requirement is covered by at least one test written before its implementation. |
| Issues and pull requests | Reference the `PR-` identifiers they advance. |

### 25.7 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Superseded | Initial product definition. Establishes mission, thirteen product principles, interaction model, environment philosophy, ten capabilities, platform and distribution strategy, runtime strategy, scope, 151 numbered product requirements, and 12 quality attributes. |
| 1.1.0 | Superseded | Product/implementation separation pass. Adds Executive Summary, Product Boundary, and Repository Product Assets and Runtime State. Restates implementation-prescriptive requirements as observable outcomes. Clarifies the operating-system metaphor. Adds `PR-RUN-016`, `PR-REP-015`, `PR-REP-016`. All pre-existing identifiers preserved unchanged. Mission, vision, and frozen product principles unaltered. |
| 1.1.1 | Freeze candidate | Presentation conformance pass against AEOS-DOCSTD 3.0.0. Converted the document to GitHub-Flavored Markdown First form: all tables rendered as Markdown tables, collapsible blocks replaced by heading hierarchy, and all raw HTML removed. Rebuilt the title, metadata, and closing blocks to the standard template; added the companion-documents field. Renamed Section 3 to *Scope and Applicability*, retaining *The Product Boundary* as its opening subsection. Numbered all subsections. Corrected the version stated in the closing block. No requirement, identifier, principle, responsibility, ownership statement, or scope boundary was changed. |

---

## Appendix A — Recommendations for Future Releases

Recorded under the architecture freeze. **None of these are part of the current product definition.**
Each requires an explicit owner revision request before it may be adopted.

| # | Recommendation | Rationale | Why deferred |
| :--- | :--- | :--- | :--- |
| R1 | Shareable rule and skill collections distributable between organizations. | Teams converge on similar standards; sharing accelerates adoption and raises baseline quality. | Introduces a distribution concept beyond the current asset model. Requires owner approval. |
| R2 | Team-level policy governing which automation grants individual developers may issue. | Organizations may want to bound delegation centrally. | Adds an authority tier above the project owner. Significant scope change. |
| R3 | Measured context-effectiveness feedback to inform prompt composition over time. | Would make Context Minimization empirical rather than heuristic. | Introduces a measurement and feedback concept not present in the current definition. |
| R4 | AEOS-supplied workflow templates for common project archetypes. | Reduces time to first productive workflow for new users. | Distinct from user-authored Template assets, which are already in scope. Shipping opinionated templates risks becoming a framework layer, which conflicts with product positioning. |
| R5 | Cross-project analytics on discipline and supervision metrics. | Would let engineering leads observe practice health across a portfolio. | Introduces cross-project data aggregation, which raises privacy and scope questions. |
| R6 | Runtime capability benchmarking to advise runtime selection. | Users would gain evidence for choosing a runtime per workflow. | Risks appearing to rank vendors, in tension with Vendor Independence. |

---

## Appendix B — Requirement Index

| Prefix | Capability | Range | Count | Section |
| :--- | :--- | :--- | :--- | :--- |
| `PR-ENV` | Environment management | 001–013 | 13 | [18.1](#181-environment-management--pr-env) |
| `PR-PRJ` | Project management | 001–011 | 11 | [18.2](#182-project-management--pr-prj) |
| `PR-WFL` | Workflow orchestration | 001–016 | 16 | [18.3](#183-workflow-orchestration--pr-wfl) |
| `PR-RUN` | AI runtime orchestration | 001–016 | 16 | [18.4](#184-ai-runtime-orchestration--pr-run) |
| `PR-TDD` | TDD workflow | 001–012 | 12 | [18.5](#185-tdd-workflow--pr-tdd) |
| `PR-DOC` | Documentation generation | 001–010 | 10 | [18.6](#186-documentation-generation--pr-doc) |
| `PR-RUL` | Rule management | 001–010 | 10 | [18.7](#187-rule-management--pr-rul) |
| `PR-SKL` | Skill management | 001–009 | 9 | [18.8](#188-skill-management--pr-skl) |
| `PR-PMT` | Prompt management | 001–010 | 10 | [18.9](#189-prompt-management--pr-pmt) |
| `PR-REP` | Repository management | 001–016 | 16 | [18.10](#1810-repository-management--pr-rep) |
| `PR-PLT` | Platform | 001–006 | 6 | [18.11](#1811-platform--pr-plt) |
| `PR-DST` | Distribution | 001–013 | 13 | [18.12](#1812-distribution--pr-dst) |
| `PR-SAF` | Safety | 001–012 | 12 | [18.13](#1813-safety--pr-saf) |
| `PR-NFR` | Quality attributes | 001–012 | 12 | [19](#19-quality-attributes) |
| **Total** |  |  | **166** | — |

---

**End of Product Requirements Document**

AEOS-PRD · Version 1.1.1 · Product Source of Truth
