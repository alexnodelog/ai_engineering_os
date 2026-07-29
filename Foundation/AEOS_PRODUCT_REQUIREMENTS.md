<div align="center">

# AI Engineering Operating System

**AEOS — Product Requirements Document**

*The product source of truth for the AEOS repository.*

</div>

<table>
<thead>
<tr><th align="left">Field</th><th align="left">Value</th></tr>
</thead>
<tbody>
<tr><td><strong>Document</strong></td><td>Product Requirements Document (PRD)</td></tr>
<tr><td><strong>Product</strong></td><td>AI Engineering Operating System (AEOS)</td></tr>
<tr><td><strong>Document ID</strong></td><td>AEOS-PRD</td></tr>
<tr><td><strong>Version</strong></td><td>1.1.0</td></tr>
<tr><td><strong>Status</strong></td><td>Freeze candidate — awaiting owner approval</td></tr>
<tr><td><strong>Owner</strong></td><td>Product Owner, AEOS</td></tr>
<tr><td><strong>Author</strong></td><td>Chief Product Architect, AEOS</td></tr>
<tr><td><strong>Audience</strong></td><td>Product owner, engineering contributors, AI runtimes consuming this repository</td></tr>
<tr><td><strong>Suggested path</strong></td><td><code>docs/product/PRD.md</code></td></tr>
<tr><td><strong>Supersedes</strong></td><td>None</td></tr>
</tbody>
</table>

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
3. [Product Boundary](#3-product-boundary)
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

<table>
<thead>
<tr><th align="left">In one sentence</th></tr>
</thead>
<tbody>
<tr><td>AEOS lets developers build software through AI-assisted, human-supervised engineering workflows, orchestrating any AI runtime across the full lifecycle without ever becoming a model, an IDE, or a framework.</td></tr>
</tbody>
</table>

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

<table>
<thead>
<tr><th align="left">At a glance</th><th align="left"></th></tr>
</thead>
<tbody>
<tr><td><strong>Product category</strong></td><td>Operating system for AI-assisted, human-supervised software engineering</td></tr>
<tr><td><strong>Performs inference</strong></td><td>Never — external AI runtimes do, under user control</td></tr>
<tr><td><strong>Core loop</strong></td><td>Inspect → Explain → Propose → Confirm → Execute → Report</td></tr>
<tr><td><strong>Capabilities</strong></td><td>Ten, spanning environment, project, workflow, runtime, TDD, documentation, rules, skills, prompts, and repository</td></tr>
<tr><td><strong>Platforms</strong></td><td>Windows, macOS, Linux — equal citizens, no primary</td></tr>
<tr><td><strong>Distribution</strong></td><td>GitHub Clone, Native Installer, MCP, Portable — with package managers, containers, and IDE marketplaces planned</td></tr>
<tr><td><strong>Source of truth</strong></td><td>The repository, as versioned Repository Assets</td></tr>
<tr><td><strong>Default posture</strong></td><td>Human-in-the-Loop, TDD-first, safe by default, fails closed</td></tr>
</tbody>
</table>

---

## 3. Product Boundary

This document defines the **Product**. It does not define how the product is built.

That distinction is load-bearing. A product definition that leaks implementation constrains
decisions that have not been made yet, and ages the moment those decisions change. This PRD
therefore describes what AEOS is and what users can observe it doing — and stops there.

### Layers and Their Owners

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Question it answers</th><th align="left">Defined by</th><th align="left">Role of this PRD</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Product</strong></td>
<td>What is AEOS, who is it for, and what must it do for them?</td>
<td>This document</td>
<td><strong>Defines it completely.</strong> This is the Product Contract.</td>
</tr>
<tr>
<td><strong>Architecture</strong></td>
<td>How is AEOS structured so it can deliver the product?</td>
<td>Architecture documents</td>
<td>Defers entirely. Names no structure, no layering, no mechanism.</td>
</tr>
<tr>
<td><strong>Specification</strong></td>
<td>How must each defined behavior work, precisely and testably?</td>
<td>Specification documents</td>
<td>Defers entirely. States outcomes, never formats or interfaces.</td>
</tr>
<tr>
<td><strong>Runtime</strong></td>
<td>How does AEOS execute, in what environment, with what lifecycle?</td>
<td>Runtime documents</td>
<td>Defers entirely. States what users observe, never what executes.</td>
</tr>
<tr>
<td><strong>Repository</strong></td>
<td>What durable assets constitute the product?</td>
<td>This document names the asset kinds; architecture defines their form</td>
<td>Defines Repository Assets as product concepts only.</td>
</tr>
<tr>
<td><strong>Workflow</strong></td>
<td>What engineering sequence does AEOS drive, and where does the human decide?</td>
<td>This document defines the observable behavior; specification defines the mechanics</td>
<td>Defines the approval model and lifecycle coverage only.</td>
</tr>
<tr>
<td><strong>Implementation</strong></td>
<td>What code realizes all of the above?</td>
<td>The codebase and its tests</td>
<td>Defers entirely. Traces back to requirement identifiers defined here.</td>
</tr>
</tbody>
</table>

### What This Document Deliberately Avoids

<table>
<thead>
<tr><th align="left">Not defined here</th><th align="left">Because</th></tr>
</thead>
<tbody>
<tr><td>Internal structure, components, layering, or module boundaries</td><td>These are architecture decisions and must stay revisable.</td></tr>
<tr><td>Interfaces, data formats, schemas, or file layouts</td><td>These are specification decisions and would freeze prematurely here.</td></tr>
<tr><td>Storage, transport, process, or execution mechanisms</td><td>These are runtime decisions and vary by platform and distribution.</td></tr>
<tr><td>Languages, libraries, frameworks, or technology choices</td><td>These are implementation decisions and must follow the product, not precede it.</td></tr>
<tr><td>How integration with any external system is achieved</td><td>Naming a mechanism today would outlive its usefulness and constrain future standards.</td></tr>
</tbody>
</table>

### The Reading Rule

> **If a statement in this document can be satisfied in only one way, it is too specific.**
> Report it as a defect in this PRD rather than treating it as an architectural instruction.
> Every requirement here should admit more than one honest implementation.

Conversely, a downstream document may not weaken, reinterpret, or quietly widen a product
requirement. Architecture decides *how*. It does not decide *whether*.

## 4. Problem Statement

AI coding tools have become capable. AI engineering has not become disciplined.

<table>
<thead>
<tr><th align="left">#</th><th align="left">Problem</th><th align="left">Consequence</th></tr>
</thead>
<tbody>
<tr>
<td>P1</td>
<td><strong>Process lives in the developer's head.</strong> The sequence of analysis, design, testing, and review is improvised in each chat session.</td>
<td>Output quality varies per session, per developer, and per day. Nothing is reproducible.</td>
</tr>
<tr>
<td>P2</td>
<td><strong>Context is thrown at the model.</strong> Whole repositories are pasted in the hope that relevance emerges.</td>
<td>Cost rises, accuracy falls, and the model attends to the wrong things.</td>
</tr>
<tr>
<td>P3</td>
<td><strong>Tools act before they explain.</strong> Files are written, dependencies installed, and branches rewritten before the developer understands the intent.</td>
<td>Trust erodes. Developers either disable automation or stop reviewing it.</td>
</tr>
<tr>
<td>P4</td>
<td><strong>Tests come last, if at all.</strong> AI generates implementation first because implementation is what was asked for.</td>
<td>Untested generated code accumulates faster than humans can verify it.</td>
</tr>
<tr>
<td>P5</td>
<td><strong>Everything is vendor-locked.</strong> Prompts, rules, skills, and workflows are trapped inside one vendor's tool.</td>
<td>Changing runtime means rebuilding the practice from zero.</td>
</tr>
<tr>
<td>P6</td>
<td><strong>The environment is assumed.</strong> Tools install, overwrite, and configure without first inspecting what already exists.</td>
<td>Working machines break. Existing projects get clobbered.</td>
</tr>
<tr>
<td>P7</td>
<td><strong>Knowledge dies with the session.</strong> Decisions, constraints, and rationale live in a chat log, not in the repository.</td>
<td>The next session — human or AI — starts uninformed.</td>
</tr>
</tbody>
</table>

AEOS exists to solve these seven problems as a product, not as advice.

---

## 5. Mission and Vision

### Mission

> **AEOS is an operating system that enables developers to build software through AI-assisted,
> human-supervised engineering workflows.**

AEOS orchestrates the complete engineering lifecycle — environment preparation, project
initialization, requirement analysis, architecture, TDD, agentic orchestration, code generation,
review, refactoring, testing, documentation, deployment, and maintenance — while performing no
language-model inference of its own.

### Vision

A developer opens any project, on any operating system, with any AI runtime, and finds the same
engineering discipline waiting for them. The workflow is explicit. The rules are versioned. The
context is minimal and deliberate. Every consequential action is explained and approved before it
happens. When the runtime changes, the practice does not. When the developer leaves, the repository
still knows how the product is built.

### Positioning

<table>
<thead>
<tr><th align="left">Category</th><th align="left">What it provides</th><th align="left">Relationship to AEOS</th></tr>
</thead>
<tbody>
<tr><td>AI models and APIs</td><td>Inference</td><td>Orchestrated by AEOS; the user chooses which</td></tr>
<tr><td>AI coding assistants and agentic runtimes</td><td>Interactive generation and tool use</td><td>Orchestrated by AEOS; never reimplemented by it</td></tr>
<tr><td>IDEs and editors</td><td>Editing surface</td><td>Environments AEOS operates alongside</td></tr>
<tr><td>Application frameworks</td><td>Runtime structure for the user's code</td><td>Unrelated; AEOS produces no application framework</td></tr>
<tr><td>Version control and CI/CD platforms</td><td>History, collaboration, automation</td><td>Orchestrated by AEOS, never replaced</td></tr>
<tr><td><strong>AEOS</strong></td><td><strong>Engineering process, project context, orchestration, and human supervision</strong></td><td><strong>The product that makes the others work together as an engineering practice</strong></td></tr>
</tbody>
</table>

---

## 6. What AEOS Is and Is Not

<table>
<thead>
<tr><th align="left">AEOS is</th><th align="left">AEOS is not</th></tr>
</thead>
<tbody>
<tr><td>An operating system for AI-assisted engineering work</td><td>An AI model or an inference engine</td></tr>
<tr><td>An orchestrator of external AI runtimes</td><td>A replacement for those runtimes</td></tr>
<tr><td>A product with a versioned repository as its core artifact</td><td>A library or an application framework</td></tr>
<tr><td>A human-supervised system with approval gates</td><td>An autonomous agent that acts on its own judgment</td></tr>
<tr><td>A process enforcer: TDD-first, explain-before-execute</td><td>A prompt collection or a template pack</td></tr>
<tr><td>Vendor, runtime, model, platform, and distribution independent</td><td>A wrapper around one vendor's ecosystem</td></tr>
<tr><td>An environment-aware system that inspects before it acts</td><td>An installer that assumes a clean machine</td></tr>
<tr><td>A manager of Repository Assets — rules, skills, prompts, workflows, and more — as versioned product artifacts</td><td>A hidden configuration store the user cannot read</td></tr>
<tr><td>An IDE-agnostic system usable from any editing surface</td><td>An IDE, an editor, or a UI toolkit</td></tr>
</tbody>
</table>

---

## 7. Product Philosophy

These thirteen principles are mandatory. They constrain every capability, every requirement, and
every downstream design decision in this repository. A feature that violates a principle is not a
feature — it is a defect.

<details>
<summary><strong>1. Human-in-the-Loop by Default</strong></summary>

<br>

The human is the decision-maker. AEOS is the system that prepares decisions and executes them
faithfully once made.

Full automation is never the default and never inferred from context. It is granted explicitly, in
scope, by the owner of the project, and it can be withdrawn at any time. Where automation has been
granted, AEOS still records what it did so the human can audit the decision after the fact.

**Implication:** every workflow terminates in an approval gate before any consequential action.

</details>

<details>
<summary><strong>2. Explain Before Execute</strong></summary>

<br>

AEOS never performs a consequential action that the human has not first been able to understand.

Before execution, AEOS states what it found, what it intends to do, why it intends to do it, what
will change, and what happens if the action is declined. The explanation is written for a human
reader, not as a log dump.

**Implication:** an action with no explanation is not executable, regardless of who requested it.

</details>

<details>
<summary><strong>3. Incremental Execution</strong></summary>

<br>

Work advances in small, verifiable steps. Each step has a defined start state, a defined end state,
and a way to tell whether it succeeded.

Large operations are decomposed into a sequence of approvable steps rather than presented as a
single irreversible leap. A failed step stops the sequence; it does not cascade.

**Implication:** progress is always inspectable mid-flight, and interruption is always safe.

</details>

<details>
<summary><strong>4. TDD-first Development</strong></summary>

<br>

Tests are written before the implementation they verify. This applies to code AEOS helps generate,
and it applies to AEOS itself.

The cycle is explicit: define the behavior, write the failing test, confirm it fails for the right
reason, implement the minimum that passes, refactor under a green suite. AEOS treats a request to
skip this cycle as an exception requiring explicit human acknowledgment, not as a shortcut.

**Implication:** generated implementation without a preceding test is a process violation AEOS must surface.

</details>

<details>
<summary><strong>5. Repository as Product</strong></summary>

<br>

The repository is not where the product is stored. The repository *is* the product.

Requirements, rules, skills, prompts, workflows, profiles, decisions, and documentation are
Repository Assets: versioned artifacts living beside the code. What is not in the repository does
not exist. A session ends; the repository persists and remains authoritative for the next human and
the next AI.

**Implication:** no product-relevant state may live only in a chat session, a vendor account, or a machine-local cache.

</details>

<details>
<summary><strong>6. Context Minimization</strong></summary>

<br>

AEOS sends the smallest context that is sufficient for the task, and it knows why each piece was sent.

Context is selected deliberately and scoped to the active step, not accumulated by default. Less
context means lower cost, higher accuracy, better privacy, and a smaller blast radius when something
goes wrong. Bulk context transfer is a failure of design, not a feature.

**Implication:** every context selection must be explainable to the user on request.

</details>

<details>
<summary><strong>7. Vendor Independence</strong></summary>

<br>

No vendor is privileged. No vendor is required.

Product capabilities are defined in vendor-neutral terms. A vendor's absence reduces the runtimes
available to the user; it never disables AEOS itself.

**Implication:** no product capability may be defined, documented, or understood in terms of one vendor's offering.

</details>

<details>
<summary><strong>8. Runtime Independence</strong></summary>

<br>

Workflows are defined independently of the runtime that executes them. The same workflow runs on a
different runtime without being rewritten.

Runtimes differ in capability. AEOS reconciles this by declaring what a workflow needs and matching
it against what a runtime offers — not by encoding one runtime's assumptions into the workflow.

**Implication:** switching runtime is a configuration change, not a migration project.

</details>

<details>
<summary><strong>9. Model Independence</strong></summary>

<br>

AEOS is not tuned to one model family, one model size, or one generation of models.

Model selection belongs to the user. Models improve, deprecate, and change price; AEOS absorbs that
churn so the project does not have to. AEOS never assumes a specific model's quirks are universal.

**Implication:** no capability may depend on undocumented behavior of a specific model version.

</details>

<details>
<summary><strong>10. Platform Independence</strong></summary>

<br>

Windows, macOS, and Linux are equal citizens. Not one primary and two ports.

The same product behavior, the same workflows, and the same repository semantics apply on every
supported platform. Platform-specific handling exists to preserve identical behavior, not to justify
different behavior.

**Implication:** a capability that works on only one platform is incomplete, not shipped.

</details>

<details>
<summary><strong>11. Distribution Independence</strong></summary>

<br>

How AEOS was installed does not change what AEOS is.

Clone, native installer, MCP distribution, and portable distribution all deliver the same product
architecture, the same capabilities, and the same behavior. Distribution affects packaging,
discovery, and update mechanics — nothing else.

**Implication:** no capability may be exclusive to a distribution channel.

</details>

<details>
<summary><strong>12. Safety by Default</strong></summary>

<br>

The safe path is the default path, and the unsafe path requires a deliberate act.

AEOS inspects before it changes, prefers reversible operations, refuses to destroy without explicit
confirmation, keeps secrets out of context and out of logs, and fails closed when it is uncertain.
Uncertainty resolves toward asking, never toward proceeding.

**Implication:** "it probably would have been fine" is never a justification for an unconfirmed destructive action.

</details>

<details>
<summary><strong>13. Extensibility by Design</strong></summary>

<br>

AEOS is extended without being modified.

New runtimes, and new Repository Assets of every kind, are added as declared, versioned assets.
Extending AEOS must not require forking it, patching it, or understanding how it works inside.

**Implication:** if a common extension requires modifying AEOS itself, AEOS has failed to be extensible.

</details>

### Principle Conflict Resolution

When two principles pull in opposite directions, resolve in this order:

1. **Safety by Default** — never trade safety for convenience.
2. **Human-in-the-Loop by Default** — when unsure, ask the human.
3. **Explain Before Execute** — never act faster than the human can understand.
4. **TDD-first Development** — never trade verification for speed.
5. **Repository as Product** — never let state escape the repository.
6. All remaining principles, weighed on the merits of the specific case.

---

## 8. Users

<table>
<thead>
<tr><th align="left">User</th><th align="left">Context</th><th align="left">What AEOS must give them</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Solo developer</strong></td>
<td>Builds and maintains projects alone; uses AI heavily; has no process safety net.</td>
<td>A dependable process without a team to enforce it, fast setup on their machine, and confidence that nothing is changed behind their back.</td>
</tr>
<tr>
<td><strong>Engineering lead / architect</strong></td>
<td>Responsible for how a team builds, not only what it ships.</td>
<td>Rules, workflows, and standards expressed as versioned Repository Assets that apply uniformly to every developer and every runtime.</td>
</tr>
<tr>
<td><strong>Platform / DevOps engineer</strong></td>
<td>Owns environments, installation, and delivery across a heterogeneous fleet.</td>
<td>Predictable installation on all three platforms, non-destructive environment handling, and integration with existing repository and CI/CD systems.</td>
</tr>
<tr>
<td><strong>Open-source maintainer</strong></td>
<td>Accepts contributions from unknown contributors using unknown tools.</td>
<td>A repository that encodes the project's engineering practice so contributions and AI-assisted changes arrive already aligned.</td>
</tr>
<tr>
<td><strong>AI runtime</strong> <em>(non-human consumer)</em></td>
<td>Reads the repository to determine how to act within the project.</td>
<td>Unambiguous, minimal, machine-consumable definitions of rules, skills, prompts, and workflow state.</td>
</tr>
</tbody>
</table>

---

## 9. The Engineering Lifecycle

AEOS orchestrates the complete engineering lifecycle. Every stage below is a first-class product
concern. Stages are ordered for readability; real projects re-enter them continuously.

<table>
<thead>
<tr><th align="left">#</th><th align="left">Stage</th><th align="left">What AEOS does</th><th align="left">Human decision point</th></tr>
</thead>
<tbody>
<tr><td>1</td><td><strong>Environment preparation</strong></td><td>Inspects the machine, reports what exists, proposes only the missing or misaligned pieces.</td><td>Approve the environment plan.</td></tr>
<tr><td>2</td><td><strong>Project initialization</strong></td><td>Establishes or adopts a project, its profile, and its Repository Assets without overwriting existing work.</td><td>Approve initialization or adoption.</td></tr>
<tr><td>3</td><td><strong>Requirement analysis</strong></td><td>Turns intent into stated, traceable requirements; surfaces ambiguity as questions rather than assumptions.</td><td>Confirm requirements and resolve ambiguity.</td></tr>
<tr><td>4</td><td><strong>Architecture</strong></td><td>Captures structure, boundaries, and decisions as durable repository artifacts.</td><td>Approve architecture and decisions.</td></tr>
<tr><td>5</td><td><strong>TDD</strong></td><td>Drives the test-first cycle and refuses to advance to implementation without a failing test.</td><td>Approve test intent and accept the failing test.</td></tr>
<tr><td>6</td><td><strong>Agentic orchestration</strong></td><td>Sequences multi-step work across runtimes, holding each step to its approval gate.</td><td>Approve the plan; approve each consequential step.</td></tr>
<tr><td>7</td><td><strong>Code generation</strong></td><td>Delegates generation to the selected runtime with minimized, deliberate context.</td><td>Approve generated changes before they are applied.</td></tr>
<tr><td>8</td><td><strong>Review</strong></td><td>Evaluates changes against requirements, rules, and tests; classifies findings by severity.</td><td>Accept, reject, or request revision.</td></tr>
<tr><td>9</td><td><strong>Refactoring</strong></td><td>Improves structure under a green test suite, with behavior preservation as the stated goal.</td><td>Approve the refactoring scope.</td></tr>
<tr><td>10</td><td><strong>Testing</strong></td><td>Runs the project's own test tooling, reports results, and blocks progress on failure.</td><td>Decide how to respond to failures.</td></tr>
<tr><td>11</td><td><strong>Documentation</strong></td><td>Generates and maintains documentation from the repository's actual state.</td><td>Approve documentation changes.</td></tr>
<tr><td>12</td><td><strong>Deployment</strong></td><td>Orchestrates the project's existing delivery pipelines; never replaces them.</td><td>Explicit approval — always, without exception.</td></tr>
<tr><td>13</td><td><strong>Maintenance</strong></td><td>Supports ongoing change: drift detection, dependency and documentation currency, incremental improvement.</td><td>Approve maintenance actions.</td></tr>
</tbody>
</table>

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

<table>
<thead>
<tr><th align="left">Phase</th><th align="left">Obligation</th></tr>
</thead>
<tbody>
<tr><td><strong>Inspect</strong></td><td>Determine the actual current state before forming any intent. Never act on assumed state.</td></tr>
<tr><td><strong>Explain</strong></td><td>State what exists, in language a human can act on. Distinguish observed fact from inference.</td></tr>
<tr><td><strong>Propose</strong></td><td>State the intended action, its rationale, its effects, its reversibility, and the consequence of declining. Offer alternatives where they exist.</td></tr>
<tr><td><strong>Confirm</strong></td><td>Wait for explicit human approval. Silence is not approval. Ambiguity is not approval. A prior approval for a different action is not approval.</td></tr>
<tr><td><strong>Execute</strong></td><td>Perform exactly what was approved — no more. Scope expansion requires a new proposal.</td></tr>
<tr><td><strong>Report</strong></td><td>State what actually happened, including partial completion and failure, and record it in the repository.</td></tr>
</tbody>
</table>

### Action Classes

Not every action needs the same gate. Actions are classified by their effect on the user's system,
and the classification determines the approval required.

<table>
<thead>
<tr><th align="left">Class</th><th align="left">Definition</th><th align="left">Examples</th><th align="left">Approval</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Observation</strong></td>
<td>Reads state; changes nothing.</td>
<td>Inspecting the environment, reading Repository Assets, reporting status.</td>
<td>None required.</td>
</tr>
<tr>
<td><strong>Local change</strong></td>
<td>Changes state that is reversible within the repository.</td>
<td>Writing files, updating configuration, adding assets.</td>
<td>Explicit approval of the proposal.</td>
</tr>
<tr>
<td><strong>External effect</strong></td>
<td>Reaches outside the repository or the machine.</td>
<td>Installing software, invoking a runtime, pushing to a remote, triggering CI.</td>
<td>Explicit approval, with cost and scope stated.</td>
</tr>
<tr>
<td><strong>Destructive</strong></td>
<td>Loses information or is not reversible by AEOS.</td>
<td>Deleting files, overwriting uncommitted work, rewriting history, deploying.</td>
<td>Explicit, specific confirmation of that exact action. Never covered by a general approval.</td>
</tr>
</tbody>
</table>

### Automation Grants

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

<table>
<thead>
<tr><th align="left">Finding</th><th align="left">AEOS behavior</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Nothing exists</strong></td>
<td>Report the empty state. Propose creation. Execute after approval.</td>
</tr>
<tr>
<td><strong>Exists and is correct</strong></td>
<td>Report it. Propose no change. Do nothing. Reuse it.</td>
</tr>
<tr>
<td><strong>Exists and differs from expectation</strong></td>
<td>Report both states, state the difference and its consequence, propose reconciliation options including "leave as is". Never reconcile silently.</td>
</tr>
<tr>
<td><strong>Exists and is unrecognized</strong></td>
<td>Report the finding. Do not modify. Do not delete. Ask the human what it is.</td>
</tr>
<tr>
<td><strong>Exists and conflicts with a required action</strong></td>
<td>Stop. Explain the conflict. Propose resolution options with tradeoffs. Wait. Never force through.</td>
</tr>
<tr>
<td><strong>Cannot be determined</strong></td>
<td>Report the uncertainty explicitly. Never present an assumption as a finding. Fail closed.</td>
</tr>
</tbody>
</table>

The user's machine belongs to the user. AEOS is a guest on it and behaves accordingly: it does not
uninstall what it did not install, does not reconfigure what it does not own, and does not treat a
pre-existing tool as a problem to be solved.

---

## 12. Product Capabilities

AEOS provides ten capabilities. Together they constitute the product. Each is defined here in
product terms; each is expressed as numbered requirements in [Section 18](#18-product-requirements).

<table>
<thead>
<tr><th align="left">#</th><th align="left">Capability</th><th align="left">Purpose</th><th align="left">ID prefix</th></tr>
</thead>
<tbody>
<tr><td>C1</td><td><strong>Environment management</strong></td><td>Know the machine; prepare it non-destructively.</td><td><code>PR-ENV</code></td></tr>
<tr><td>C2</td><td><strong>Project management</strong></td><td>Establish, adopt, and describe projects.</td><td><code>PR-PRJ</code></td></tr>
<tr><td>C3</td><td><strong>Workflow orchestration</strong></td><td>Define and drive engineering workflows with approval gates.</td><td><code>PR-WFL</code></td></tr>
<tr><td>C4</td><td><strong>AI runtime orchestration</strong></td><td>Select, adapt, and coordinate external runtimes.</td><td><code>PR-RUN</code></td></tr>
<tr><td>C5</td><td><strong>TDD workflow</strong></td><td>Enforce test-first development as the primary path.</td><td><code>PR-TDD</code></td></tr>
<tr><td>C6</td><td><strong>Documentation generation</strong></td><td>Produce and maintain documentation from repository truth.</td><td><code>PR-DOC</code></td></tr>
<tr><td>C7</td><td><strong>Rule management</strong></td><td>Express engineering constraints as versioned assets.</td><td><code>PR-RUL</code></td></tr>
<tr><td>C8</td><td><strong>Skill management</strong></td><td>Package reusable engineering procedures.</td><td><code>PR-SKL</code></td></tr>
<tr><td>C9</td><td><strong>Prompt management</strong></td><td>Manage prompts as versioned, portable, minimized assets.</td><td><code>PR-PMT</code></td></tr>
<tr><td>C10</td><td><strong>Repository management</strong></td><td>Treat the repository as the product's source of truth, including version control and delivery integration.</td><td><code>PR-REP</code></td></tr>
</tbody>
</table>

<details>
<summary><strong>C1 — Environment management</strong></summary>

<br>

AEOS understands the machine it runs on before it changes anything about it. It detects the
platform, the tooling relevant to the project, and the AI runtimes available for use. It reports
findings in human terms, proposes only what is missing or misaligned, and executes only after
approval. It never assumes a clean machine, never silently upgrades, and never removes what it did
not install. Environment state is a product concern because a workflow that cannot describe its
environment cannot be reproduced.

</details>

<details>
<summary><strong>C2 — Project management</strong></summary>

<br>

A project is the unit AEOS operates on. AEOS creates new projects and adopts existing ones — adoption
being the harder and more common case, and therefore the one that must be safest. Each project has a
profile describing what it is, how it is built and tested, which runtime it uses, and which rules
apply. The profile is a Repository Asset, readable by humans and by AI runtimes, and it is the reason
a new session can become useful immediately without re-explaining the project.

</details>

<details>
<summary><strong>C3 — Workflow orchestration</strong></summary>

<br>

Workflows encode how engineering work proceeds: the steps, their order, their preconditions, their
approval gates, and their success criteria. AEOS drives them incrementally, keeps observable state,
survives interruption, and can resume without losing position. Workflows are declared as versioned
assets rather than embedded in tool behavior, so a team's practice is inspectable, reviewable, and
portable across runtimes and machines.

</details>

<details>
<summary><strong>C4 — AI runtime orchestration</strong></summary>

<br>

AEOS coordinates external AI runtimes and performs no inference itself. It reports which runtimes
are available, lets the user select one and switch later without penalty, and automatically applies
the engineering capabilities appropriate to that choice. Where a selected runtime cannot support part
of the requested work, the user is told before the work begins rather than partway through. Where it
can, the workflow behaves as it would anywhere else. Runtime failure is handled as an ordinary
condition: reported clearly, never silently retried into cost, and never resolved by substituting a
different runtime without asking.

</details>

<details>
<summary><strong>C5 — TDD workflow</strong></summary>

<br>

TDD is a product capability, not a recommendation. AEOS drives the red-green-refactor cycle
explicitly: define behavior, write the failing test, verify the failure is for the right reason,
implement minimally, refactor green. It tracks cycle position, blocks implementation that has no
preceding test, and surfaces skipping as an explicit exception the human must acknowledge. This is
the primary means by which AI-generated code stays verifiable at the speed it is produced.

</details>

<details>
<summary><strong>C6 — Documentation generation</strong></summary>

<br>

Documentation is generated from what the repository actually contains, kept consistent with it, and
maintained as the project changes. AEOS produces documentation suitable for GitHub, for human
maintenance, and for AI consumption — the same artifact serving all three. It detects drift between
documentation and reality, proposes updates, and never publishes a document containing placeholders,
TODOs, or unfinished sections.

</details>

<details>
<summary><strong>C7 — Rule management</strong></summary>

<br>

Rules are the engineering constraints a project agrees to: standards, boundaries, required practices,
prohibitions. AEOS manages them as versioned Repository Assets with defined scope and precedence,
applies them during generation and review, reports violations with severity, and never applies a rule
the user cannot inspect. Rules are how an engineering lead's intent reaches every developer and every
runtime without being restated.

</details>

<details>
<summary><strong>C8 — Skill management</strong></summary>

<br>

Skills are packaged, reusable engineering procedures — a repeatable way to perform a specific kind of
work. AEOS discovers, versions, composes, and applies them, and makes clear which skill was applied
and why. Skills are runtime-independent so a team's accumulated know-how survives a change of vendor,
and they are additive so extending AEOS does not mean modifying it.

</details>

<details>
<summary><strong>C9 — Prompt management</strong></summary>

<br>

Prompts are engineering assets, not disposable text. AEOS manages them as versioned, parameterized,
portable artifacts, composes them from project context deliberately, and holds them to Context
Minimization: the smallest sufficient context, with the reason for each inclusion available on
request. Prompts remain inspectable before they are sent, because a prompt the user cannot read is a
decision the user did not make.

</details>

<details>
<summary><strong>C10 — Repository management</strong></summary>

<br>

The repository is the product. AEOS treats it as the single source of truth for code and for every
Repository Asset: requirements, rules, skills, prompts, workflows, profiles, decisions, and
documentation. Version control, branching,
history, and CI/CD integration are product capabilities: AEOS orchestrates the project's existing
Git and delivery systems, explains what it will do to them before doing it, and treats history
rewriting and deployment as destructive actions requiring specific confirmation. AEOS never replaces
the version control or CI/CD system it integrates with.

</details>

---

## 13. Repository Product Assets and Runtime State

The repository is the product. That principle only holds if it is clear what belongs in the
repository and what does not. This section draws that line as a product distinction — not as a
storage design.

### Repository Product Assets

**Repository Assets** are the durable, versioned artifacts that together constitute the product.
They are what a project carries forward: the accumulated statement of what is being built, how it is
built, and why it is built that way.

Repository Assets include, but are not limited to:

<table>
<thead>
<tr><th align="left">Asset</th><th align="left">What it carries</th></tr>
</thead>
<tbody>
<tr><td><strong>Rules</strong></td><td>The engineering constraints a project agrees to.</td></tr>
<tr><td><strong>Skills</strong></td><td>Reusable engineering procedures the project knows how to perform.</td></tr>
<tr><td><strong>Prompts</strong></td><td>Deliberate, minimized instruction and context used when work is delegated to a runtime.</td></tr>
<tr><td><strong>Workflows</strong></td><td>The engineering sequences a project follows, including where the human decides.</td></tr>
<tr><td><strong>Profiles</strong></td><td>What a project is, how it is built and tested, and which runtime and rules apply.</td></tr>
<tr><td><strong>Templates</strong></td><td>Reusable starting points for work the project performs repeatedly.</td></tr>
<tr><td><strong>Playbooks</strong></td><td>Established responses to recurring engineering situations.</td></tr>
<tr><td><strong>Recipes</strong></td><td>Known-good sequences for producing a specific, repeatable result.</td></tr>
<tr><td><strong>Specifications</strong></td><td>Precise statements of required behavior, traceable to product requirements.</td></tr>
<tr><td><strong>Architecture Documents</strong></td><td>The structural decisions that realize the product, and the reasoning behind them.</td></tr>
<tr><td><strong>Manuals</strong></td><td>Documentation for the humans and AI runtimes that will maintain the project.</td></tr>
</tbody>
</table>

The list is open on purpose. New kinds of Repository Asset may be introduced without changing what a
Repository Asset *is*.

### What Every Repository Asset Has in Common

These are observable product properties, not a definition of form.

<table>
<thead>
<tr><th align="left">Property</th><th align="left">What the user can rely on</th></tr>
</thead>
<tbody>
<tr><td><strong>Durable</strong></td><td>It survives the session, the machine, and the runtime that produced it.</td></tr>
<tr><td><strong>Versioned</strong></td><td>It changes through the project's ordinary review and history, like code.</td></tr>
<tr><td><strong>Inspectable</strong></td><td>A human can read it and understand what it does before it takes effect.</td></tr>
<tr><td><strong>Consumable</strong></td><td>An AI runtime can act on it without a separate machine-only version existing.</td></tr>
<tr><td><strong>Portable</strong></td><td>It moves between machines, platforms, and distribution methods unchanged.</td></tr>
<tr><td><strong>Extensible</strong></td><td>Users add, modify, and remove their own without modifying AEOS.</td></tr>
</tbody>
</table>

This document defines Repository Assets as product concepts only. It does not define their form,
their expression, their relationships to one another, or how they are organized. Those are
architecture and specification concerns.

### Runtime State

**Runtime State** is everything AEOS produces or depends on while running that is *not* part of the
product. It is a consequence of execution, not a statement of what the product is.

Runtime State includes:

<table>
<thead>
<tr><th align="left">Runtime State</th><th align="left">Why it is not a product asset</th></tr>
</thead>
<tbody>
<tr><td><strong>Cache</strong></td><td>An optimization. Losing it costs time, never meaning.</td></tr>
<tr><td><strong>Temporary execution state</strong></td><td>Belongs to a run in progress, not to the project.</td></tr>
<tr><td><strong>Credentials</strong></td><td>Belong to a person or an organization, never to a repository.</td></tr>
<tr><td><strong>Telemetry</strong></td><td>Describes usage, not the product being built.</td></tr>
<tr><td><strong>Generated temporary artifacts</strong></td><td>Reproducible from Repository Assets on demand.</td></tr>
<tr><td><strong>Machine-specific configuration</strong></td><td>True of one machine, and therefore not true of the project.</td></tr>
</tbody>
</table>

### The Distinction, Stated as a Test

> **If losing it costs only repeated work, it is Runtime State.**
> **If losing it costs product meaning, it is a Repository Asset.**

Runtime State is intentionally excluded from Repository Product Assets. A project must remain fully
understandable and reproducible without it. Nothing in this document specifies where Runtime State
lives, how long it persists, or how it is managed — those are runtime and architecture concerns.

---

## 14. Platform Support

Platform support is a core product capability, not a compatibility exercise.

<table>
<thead>
<tr><th align="left">Platform</th><th align="left">Status</th><th align="left">Commitment</th></tr>
</thead>
<tbody>
<tr><td><strong>Windows</strong></td><td>Officially supported</td><td>Full product capability. First-class citizen.</td></tr>
<tr><td><strong>macOS</strong></td><td>Officially supported</td><td>Full product capability. First-class citizen.</td></tr>
<tr><td><strong>Linux</strong></td><td>Officially supported</td><td>Full product capability. First-class citizen.</td></tr>
</tbody>
</table>

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

### Official at minimum

<table>
<thead>
<tr><th align="left">Method</th><th align="left">Description</th><th align="left">Primary user</th></tr>
</thead>
<tbody>
<tr><td><strong>GitHub Clone</strong></td><td>Clone the repository and use AEOS directly from source.</td><td>Contributors, teams standardizing on a pinned revision, users who want full transparency.</td></tr>
<tr><td><strong>Native Installer</strong></td><td>Platform-native installation for Windows, macOS, and Linux.</td><td>Developers who want a supported, updatable install with the least setup.</td></tr>
<tr><td><strong>MCP Distribution</strong></td><td>AEOS made available to MCP-capable AI runtimes and clients.</td><td>Users who work primarily inside an AI runtime or IDE and want AEOS available there.</td></tr>
<tr><td><strong>Portable Distribution</strong></td><td>Self-contained, relocatable, no system-level installation.</td><td>Locked-down machines, air-gapped environments, ephemeral and shared systems.</td></tr>
</tbody>
</table>

### Planned

<table>
<thead>
<tr><th align="left">Method</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Package Managers</strong></td><td>Native ecosystem installation and update paths per platform.</td></tr>
<tr><td><strong>Docker Images</strong></td><td>Reproducible, isolated environments for CI and containerized workflows.</td></tr>
<tr><td><strong>IDE Marketplace Distribution</strong></td><td>Discovery and installation from within the developer's editing surface.</td></tr>
</tbody>
</table>

### Distribution Invariants

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

### Runtime Examples

<table>
<thead>
<tr><th align="left">Category</th><th align="left">Examples</th></tr>
</thead>
<tbody>
<tr><td><strong>Commercial AI services</strong></td><td>Claude · OpenAI · Gemini</td></tr>
<tr><td><strong>AI-assisted development environments</strong></td><td>Cursor · GitHub Copilot</td></tr>
<tr><td><strong>Open-source models</strong></td><td>Locally or privately hosted models of any size</td></tr>
<tr><td><strong>Interoperability standards</strong></td><td>MCP and comparable standards, present and future</td></tr>
<tr><td><strong>Extensions</strong></td><td>Plugins and integrations supplied by users or third parties</td></tr>
<tr><td><strong>Not yet released</strong></td><td>Runtime categories that do not exist at the time of writing</td></tr>
</tbody>
</table>

This list is illustrative. Being named here confers no privilege; being absent implies no exclusion.
The final row is deliberate: AEOS is expected to outlive the current runtime landscape.

### Integration Model

<table>
<thead>
<tr><th align="left">Rule</th><th align="left">Meaning</th></tr>
</thead>
<tbody>
<tr><td><strong>Runtimes are integrations, not components</strong></td><td>An external runtime is never part of AEOS. AEOS remains whole and functional as runtimes come and go.</td></tr>
<tr><td><strong>Independent of runtime implementation</strong></td><td>AEOS is defined by what the user gets, never by how a runtime works inside. The means of integration belongs to architecture.</td></tr>
<tr><td><strong>Do not rebuild what exists</strong></td><td>Where a mature runtime already provides a capability well, AEOS orchestrates it rather than recreating it.</td></tr>
<tr><td><strong>Fit is reported, not assumed</strong></td><td>AEOS automatically applies the engineering capabilities a selected runtime supports, and states before work begins what it cannot support.</td></tr>
<tr><td><strong>The user chooses</strong></td><td>Runtime selection is the user's decision. AEOS may report suitability; it never overrides the choice or silently substitutes.</td></tr>
<tr><td><strong>Graceful degradation</strong></td><td>An unavailable runtime reduces available options. It never corrupts project state and never blocks non-inference capabilities.</td></tr>
<tr><td><strong>Cost and effect transparency</strong></td><td>Invoking a runtime is an external effect. The user knows before it happens, not after.</td></tr>
</tbody>
</table>

### Runtime Independence in Practice

A workflow authored against AEOS runs on a different runtime by changing the project's runtime
selection. The workflow file does not change. The rules do not change. The skills do not change. The
repository does not change. If any of those must change, runtime independence has been violated and
the violation is a defect in AEOS, not a limitation of the user's setup.

---

## 17. Product Scope

### In Scope

All ten capabilities in [Section 12](#12-product-capabilities), plus explicitly:

<table>
<thead>
<tr><th align="left">Capability</th><th align="left">AEOS role</th></tr>
</thead>
<tbody>
<tr><td><strong>Version control</strong></td><td>Product capability. AEOS understands and orchestrates the project's version control state.</td></tr>
<tr><td><strong>Git</strong></td><td>Product capability. AEOS integrates with Git operations under the standard approval gates.</td></tr>
<tr><td><strong>CI/CD integration</strong></td><td>Product capability. AEOS integrates with existing pipelines and delivery systems.</td></tr>
<tr><td><strong>Code generation</strong></td><td>Product capability, delegated to runtimes and governed by AEOS workflow, rules, and gates.</td></tr>
<tr><td><strong>Testing</strong></td><td>Product capability. AEOS orchestrates the project's test tooling and gates progress on results.</td></tr>
<tr><td><strong>Review</strong></td><td>Product capability. AEOS evaluates changes against requirements, rules, and tests with severity-classified findings.</td></tr>
<tr><td><strong>Deployment</strong></td><td>Product capability. AEOS orchestrates the project's delivery systems with explicit approval, always.</td></tr>
</tbody>
</table>

### Out of Scope

Only the following are outside the product. Everything else is in scope; implementation detail
belongs to downstream documents, not to exclusion.

<table>
<thead>
<tr><th align="left">Excluded</th><th align="left">Reason</th></tr>
</thead>
<tbody>
<tr><td><strong>Language-model inference</strong></td><td>AEOS performs no inference. It orchestrates runtimes that do.</td></tr>
<tr><td><strong>Model training, fine-tuning, or hosting</strong></td><td>Belongs to model vendors and open-source model providers.</td></tr>
<tr><td><strong>Replacing version control, CI/CD, or hosting platforms</strong></td><td>AEOS integrates with these systems; it does not become one.</td></tr>
<tr><td><strong>Being an IDE, editor, or application framework</strong></td><td>AEOS operates alongside editing surfaces and produces no application framework.</td></tr>
<tr><td><strong>Implementation detail</strong></td><td>Architecture, interfaces, data formats, schemas, algorithms, file layout, and technology choices belong to downstream architecture and specification documents.</td></tr>
</tbody>
</table>

---

## 18. Product Requirements

### How to Read This Section

<table>
<thead>
<tr><th align="left">Field</th><th align="left">Meaning</th></tr>
</thead>
<tbody>
<tr><td><strong>ID</strong></td><td>Stable identifier. Downstream documents, tests, and issues reference it. IDs are never reused or renumbered.</td></tr>
<tr><td><strong>Priority</strong></td><td><code>P0</code> product cannot ship without it · <code>P1</code> required for a complete product · <code>P2</code> planned enhancement</td></tr>
<tr><td><strong>Phase</strong></td><td>Target release phase — see <a href="#22-release-phases">Section 22</a>.</td></tr>
</tbody>
</table>

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

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-ENV-001</code></td><td>AEOS inspects the environment before proposing or performing any environment-affecting action.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-002</code></td><td>AEOS detects the host platform and adapts its behavior to preserve identical product semantics across Windows, macOS, and Linux.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-003</code></td><td>AEOS reports the discovered environment state to the user in human-readable form, distinguishing observed facts from inferences.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-004</code></td><td>AEOS detects AI runtimes available on the machine and reports which are usable for the current project.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-005</code></td><td>AEOS detects project-relevant tooling (build, test, version control, delivery) rather than assuming its presence.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-006</code></td><td>When a required component is missing, AEOS proposes an action and executes only after explicit approval.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-007</code></td><td>When a component already exists and is correct, AEOS reuses it and proposes no change.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-008</code></td><td>When a component exists but differs from expectation, AEOS reports both states and the difference, and proposes reconciliation options including taking no action.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-009</code></td><td>AEOS never modifies, replaces, or removes a component it did not install without explicit, specific confirmation.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-010</code></td><td>When environment state cannot be determined, AEOS reports the uncertainty and stops rather than assuming.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-ENV-011</code></td><td>AEOS provides an on-demand environment report the user can inspect at any time without changing state.</td><td>P1</td><td>1</td></tr>
<tr><td><code>PR-ENV-012</code></td><td>AEOS records environment findings in the project so a workflow's environment assumptions are reproducible.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-ENV-013</code></td><td>AEOS detects when the environment has drifted from previously recorded findings and reports the drift.</td><td>P2</td><td>3</td></tr>
</tbody>
</table>

---

### 18.2 Project Management — `PR-PRJ`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-PRJ-001</code></td><td>AEOS initializes a new project with the assets required for AEOS-governed work.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-002</code></td><td>AEOS adopts an existing project without overwriting, relocating, or restructuring existing content.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-003</code></td><td>Before initialization or adoption, AEOS inspects the target location and reports what it found.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-004</code></td><td>AEOS maintains a project profile describing the project's identity, technology, build and test approach, selected runtime, and applicable rules.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-005</code></td><td>The project profile is a versioned Repository Asset, readable and editable by humans and consumable by AI runtimes.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-006</code></td><td>AEOS proposes profile values derived from inspection and requires confirmation before recording them.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-007</code></td><td>AEOS reports current project status — profile, workflow position, runtime selection, and outstanding decisions — on demand.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PRJ-008</code></td><td>A project is portable: it functions identically on any supported platform and under any distribution method.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-PRJ-009</code></td><td>Users can work on multiple independent projects on one machine; work in one never affects another.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-PRJ-010</code></td><td>AEOS detects that a project's recorded profile no longer matches the repository's actual state and reports the divergence.</td><td>P1</td><td>3</td></tr>
<tr><td><code>PR-PRJ-011</code></td><td>AEOS supports removing its own project assets cleanly, leaving the user's project intact.</td><td>P1</td><td>2</td></tr>
</tbody>
</table>

---

### 18.3 Workflow Orchestration — `PR-WFL`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-WFL-001</code></td><td>AEOS provides workflows covering every stage of the engineering lifecycle defined in Section 9.</td><td>P0</td><td>1–3</td></tr>
<tr><td><code>PR-WFL-002</code></td><td>Workflows are declared as versioned Repository Assets, inspectable and reviewable by humans.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-003</code></td><td>Workflows are defined independently of any AI runtime and execute unchanged across runtimes.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-004</code></td><td>AEOS executes workflows incrementally, one verifiable step at a time.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-005</code></td><td>Every consequential step follows Inspect → Explain → Propose → Confirm → Execute → Report.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-006</code></td><td>AEOS classifies each action as observation, local change, external effect, or destructive, and applies the corresponding approval requirement.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-007</code></td><td>Users can see where a workflow currently stands, what has been completed, and what decisions remain outstanding.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-008</code></td><td>Users can safely pause an engineering workflow and resume it later without losing position or re-establishing context.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-WFL-009</code></td><td>A declined proposal halts the workflow without side effects and without penalty.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-010</code></td><td>A failed step halts the workflow, reports the failure, and does not proceed to dependent steps.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-011</code></td><td>AEOS reports what actually occurred after execution, including partial completion.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-WFL-012</code></td><td>AEOS supports agentic orchestration: multi-step work sequenced across runtimes, with each consequential step held to its approval gate.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-WFL-013</code></td><td>Users can define project-specific workflows without modifying AEOS itself.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-WFL-014</code></td><td>Automation grants are explicit, scoped, recorded, revocable, and never extend to destructive actions.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-WFL-015</code></td><td>AEOS records an auditable history of proposals, decisions, and executions within the project.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-WFL-016</code></td><td>AEOS reports which workflow steps a selected runtime cannot satisfy before execution begins.</td><td>P1</td><td>2</td></tr>
</tbody>
</table>

---

### 18.4 AI Runtime Orchestration — `PR-RUN`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-RUN-001</code></td><td>AEOS performs no language-model inference of its own under any circumstance.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-002</code></td><td>AEOS remains independent from AI runtime implementation, and does not reproduce capabilities that runtimes already provide well.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-003</code></td><td>AEOS supports multiple runtimes and is not dependent on any single vendor.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-004</code></td><td>The user selects the runtime; AEOS never overrides or silently substitutes that selection.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-005</code></td><td>Switching runtime requires no change to workflows, rules, skills, prompts, or repository structure.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-006</code></td><td>AEOS is independent of any specific model, model family, or model version.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-007</code></td><td>AEOS automatically selects engineering capabilities appropriate to the chosen runtime, and reports anything the runtime cannot support before work begins.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-RUN-008</code></td><td>A workflow produces consistent, comparable outcomes regardless of which runtime performed the work.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-RUN-009</code></td><td>Invoking a runtime is an external effect requiring explicit approval, with scope and expected cost stated beforehand.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-010</code></td><td>Runtime unavailability degrades available options without corrupting project state or blocking non-inference capabilities.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-011</code></td><td>AEOS reports runtime errors clearly and never silently retries in a way that incurs unapproved cost.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-012</code></td><td>Support for a new AI runtime can be added without modifying AEOS itself and without changing existing projects.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-RUN-013</code></td><td>AEOS supports orchestrating more than one runtime within a single workflow when the user configures it.</td><td>P1</td><td>3</td></tr>
<tr><td><code>PR-RUN-014</code></td><td>Runtime credentials are never written into prompts, logs, documentation, or Repository Assets.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUN-015</code></td><td>AEOS reports runtime usage per project so users can attribute cost and activity.</td><td>P2</td><td>3</td></tr>
<tr><td><code>PR-RUN-016</code></td><td>AEOS can support new categories of AI runtime — commercial services, open-source models, interoperability standards, and user-supplied extensions — without changes to existing projects or Repository Assets.</td><td>P1</td><td>3</td></tr>
</tbody>
</table>

---

### 18.5 TDD Workflow — `PR-TDD`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-TDD-001</code></td><td>TDD is the default development workflow for all AEOS-governed code work.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-002</code></td><td>AEOS requires a stated, confirmed behavior before a test is written.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-003</code></td><td>AEOS requires a failing test before implementation begins.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-004</code></td><td>AEOS verifies that a new test fails for the intended reason before proceeding.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-005</code></td><td>AEOS drives implementation toward the minimum change that satisfies the failing test.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-006</code></td><td>AEOS permits refactoring only while the test suite is passing.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-007</code></td><td>AEOS tracks and reports the current position in the red-green-refactor cycle.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-008</code></td><td>Skipping the TDD cycle is an explicit exception the user must acknowledge; it is never silent and never the default.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-009</code></td><td>AEOS orchestrates the project's existing test tooling rather than providing its own test framework.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-010</code></td><td>Test failures halt workflow progression and are reported with enough detail to act on.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-TDD-011</code></td><td>AEOS reports untested changes introduced outside the TDD cycle.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-TDD-012</code></td><td>AEOS applies the same TDD requirements to its own development.</td><td>P0</td><td>1</td></tr>
</tbody>
</table>

---

### 18.6 Documentation Generation — `PR-DOC`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-DOC-001</code></td><td>AEOS generates documentation from the repository's actual state, not from assumption or from chat history.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DOC-002</code></td><td>Generated documentation is complete: no placeholders, no TODOs, no unfinished sections.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DOC-003</code></td><td>Generated documentation is internally consistent and consistent with the repository it describes.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DOC-004</code></td><td>Generated documentation renders correctly on GitHub and remains valid Markdown.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DOC-005</code></td><td>Generated documentation is suitable for human maintenance and for AI consumption from the same artifact.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DOC-006</code></td><td>Documentation changes are proposed and approved before being written, like any other local change.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DOC-007</code></td><td>AEOS maintains documentation as the project changes rather than generating it once.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-DOC-008</code></td><td>AEOS detects drift between documentation and repository state and reports it.</td><td>P1</td><td>3</td></tr>
<tr><td><code>PR-DOC-009</code></td><td>AEOS supports project-defined documentation conventions without modification of AEOS itself.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-DOC-010</code></td><td>AEOS never publishes generated documentation containing unresolved uncertainty presented as fact.</td><td>P0</td><td>1</td></tr>
</tbody>
</table>

---

### 18.7 Rule Management — `PR-RUL`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-RUL-001</code></td><td>Rules are versioned Repository Assets, inspectable and editable by humans.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUL-002</code></td><td>Rules are defined independently of any runtime and apply identically across runtimes.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUL-003</code></td><td>Each rule has a defined scope determining where it applies.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUL-004</code></td><td>Rule precedence is deterministic and explainable when rules overlap or conflict.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-RUL-005</code></td><td>AEOS applies applicable rules during generation, review, and refactoring.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUL-006</code></td><td>Rule violations are reported with severity and location.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-RUL-007</code></td><td>AEOS reports which rules were applied to a given action on request.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-RUL-008</code></td><td>Users add, modify, and remove rules without modifying AEOS itself.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUL-009</code></td><td>AEOS never applies a rule the user cannot inspect.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-RUL-010</code></td><td>AEOS reports rules that cannot be enforced under the selected runtime rather than silently ignoring them.</td><td>P1</td><td>3</td></tr>
</tbody>
</table>

---

### 18.8 Skill Management — `PR-SKL`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-SKL-001</code></td><td>Skills are versioned Repository Assets packaging reusable engineering procedures.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-SKL-002</code></td><td>Skills are defined independently of any runtime and apply identically across runtimes.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-SKL-003</code></td><td>AEOS discovers available skills and reports them to the user.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-SKL-004</code></td><td>Skill selection and application are explained before execution.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-SKL-005</code></td><td>Users add, modify, and remove skills without modifying AEOS itself.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-SKL-006</code></td><td>Skills are composable within a workflow, with composition made visible to the user.</td><td>P1</td><td>3</td></tr>
<tr><td><code>PR-SKL-007</code></td><td>Skills are versioned so a project can depend on a known skill revision.</td><td>P1</td><td>3</td></tr>
<tr><td><code>PR-SKL-008</code></td><td>AEOS reports which skill was applied to a given action and why.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-SKL-009</code></td><td>Skills are portable between projects and between machines without modification.</td><td>P1</td><td>3</td></tr>
</tbody>
</table>

---

### 18.9 Prompt Management — `PR-PMT`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-PMT-001</code></td><td>Prompts are versioned Repository Assets that outlive the session in which they were used.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PMT-002</code></td><td>Prompts are composed from project context deliberately and are parameterizable.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-PMT-003</code></td><td>AEOS applies Context Minimization: the smallest context sufficient for the step.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PMT-004</code></td><td>AEOS explains why each element of context was included, on request.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-PMT-005</code></td><td>The user can inspect the composed prompt before it is sent to a runtime.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PMT-006</code></td><td>Prompts remain usable when a project changes runtime.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-PMT-007</code></td><td>Users add, modify, and remove prompts without modifying AEOS itself.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-PMT-008</code></td><td>AEOS never includes credentials or user-designated sensitive content in a prompt.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PMT-009</code></td><td>AEOS reports the context size of a composed prompt before sending it.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-PMT-010</code></td><td>AEOS supports project-defined prompt conventions and overrides.</td><td>P1</td><td>3</td></tr>
</tbody>
</table>

---

### 18.10 Repository Management — `PR-REP`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-REP-001</code></td><td>The repository is the authoritative source of truth for all AEOS project state.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-002</code></td><td>Everything needed to understand and reproduce the project lives in the repository; nothing essential exists only outside it.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-003</code></td><td>AEOS integrates with the project's version control system and reports its state before proposing changes.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-004</code></td><td>AEOS supports Git operations required by its workflows, each under the appropriate approval gate.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-REP-005</code></td><td>AEOS never modifies version control history without explicit, specific confirmation of that exact operation.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-REP-006</code></td><td>AEOS never discards uncommitted work without explicit, specific confirmation.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-007</code></td><td>AEOS integrates with the project's existing CI/CD systems and never replaces them.</td><td>P0</td><td>3</td></tr>
<tr><td><code>PR-REP-008</code></td><td>AEOS orchestrates deployment through the project's delivery systems and requires explicit approval for every deployment.</td><td>P0</td><td>3</td></tr>
<tr><td><code>PR-REP-009</code></td><td>AEOS Repository Assets are human-readable, diffable, and reviewable in ordinary code review.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-010</code></td><td>AEOS Repository Assets are consumable by AI runtimes without transformation.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-011</code></td><td>AEOS supports review workflows that evaluate changes against requirements, rules, and tests, reporting findings classified as Critical, Major, Minor, or Nitpick.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-REP-012</code></td><td>AEOS records architectural and product decisions as durable repository artifacts.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-REP-013</code></td><td>AEOS never writes credentials or secrets into Repository Assets.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-REP-014</code></td><td>AEOS reports when the repository has changed outside its knowledge and re-inspects before proceeding.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-REP-015</code></td><td>AEOS distinguishes Repository Assets from Runtime State, and never requires Runtime State in order to understand or reproduce a project.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-REP-016</code></td><td>Repository Assets remain readable and meaningful when AEOS is not running.</td><td>P1</td><td>2</td></tr>
</tbody>
</table>

---

### 18.11 Platform — `PR-PLT`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-PLT-001</code></td><td>AEOS officially supports Windows, macOS, and Linux with equal capability.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PLT-002</code></td><td>No product capability is exclusive to one platform.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PLT-003</code></td><td>Workflows, rules, skills, prompts, and repository semantics behave identically on all supported platforms.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PLT-004</code></td><td>A project prepared on one platform is usable on another without modification.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-PLT-005</code></td><td>Platform differences are absorbed by AEOS and not exposed to users or workflow authors.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-PLT-006</code></td><td>A capability is considered complete only when it functions on all three supported platforms.</td><td>P0</td><td>1</td></tr>
</tbody>
</table>

---

### 18.12 Distribution — `PR-DST`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-DST-001</code></td><td>AEOS is distributed via GitHub Clone.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DST-002</code></td><td>AEOS is distributed via native installers for Windows, macOS, and Linux.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-DST-003</code></td><td>AEOS is distributed via MCP distribution for MCP-capable runtimes and clients.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-DST-004</code></td><td>AEOS is distributed as a portable, self-contained distribution requiring no system-level installation.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-DST-005</code></td><td>The product architecture is identical across all distribution methods.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DST-006</code></td><td>No product capability is exclusive to a distribution method.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DST-007</code></td><td>A project is portable across distribution methods without modification.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-DST-008</code></td><td>Every distribution reports its version and origin.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-DST-009</code></td><td>Installation inspects the environment first and never overwrites an existing installation without approval.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-DST-010</code></td><td>Every distribution supports clean removal.</td><td>P1</td><td>2</td></tr>
<tr><td><code>PR-DST-011</code></td><td>AEOS is distributed via platform package managers.</td><td>P2</td><td>4</td></tr>
<tr><td><code>PR-DST-012</code></td><td>AEOS is distributed as Docker images.</td><td>P2</td><td>4</td></tr>
<tr><td><code>PR-DST-013</code></td><td>AEOS is distributed via IDE marketplaces.</td><td>P2</td><td>4</td></tr>
</tbody>
</table>

---

### 18.13 Safety — `PR-SAF`

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Requirement</th><th align="left">Priority</th><th align="left">Phase</th></tr>
</thead>
<tbody>
<tr><td><code>PR-SAF-001</code></td><td>The safe path is the default path in every workflow.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-002</code></td><td>AEOS fails closed: when uncertain, it stops and asks rather than proceeding.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-003</code></td><td>Destructive actions require explicit, specific confirmation and are never covered by a general approval.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-004</code></td><td>AEOS prefers reversible operations and states reversibility in every proposal.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-005</code></td><td>AEOS executes only the approved action; scope expansion requires a new proposal.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-006</code></td><td>Credentials and secrets are never placed in prompts, logs, reports, documentation, or Repository Assets.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-007</code></td><td>AEOS does not transmit project content to any runtime that the user has not selected and approved.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-008</code></td><td>AEOS reports what will leave the machine before it leaves.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-009</code></td><td>AEOS does not modify or remove components outside the project that it did not install.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-010</code></td><td>Interruption at any point leaves the project in a consistent, describable state.</td><td>P0</td><td>2</td></tr>
<tr><td><code>PR-SAF-011</code></td><td>AEOS never presents an inference as an observed fact.</td><td>P0</td><td>1</td></tr>
<tr><td><code>PR-SAF-012</code></td><td>Automation grants never authorize destructive actions.</td><td>P0</td><td>2</td></tr>
</tbody>
</table>

---

## 19. Quality Attributes

Product-level qualities. They apply to every capability and every requirement above.

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Attribute</th><th align="left">Product commitment</th></tr>
</thead>
<tbody>
<tr><td><code>PR-NFR-001</code></td><td><strong>Transparency</strong></td><td>Every AEOS decision is explainable to the user on request: what was inspected, what was proposed, what was applied, and why.</td></tr>
<tr><td><code>PR-NFR-002</code></td><td><strong>Reproducibility</strong></td><td>The same project, workflow, and inputs produce the same AEOS behavior on any supported platform and distribution.</td></tr>
<tr><td><code>PR-NFR-003</code></td><td><strong>Responsiveness</strong></td><td>Inspection and reporting complete quickly enough to precede every action without discouraging their use.</td></tr>
<tr><td><code>PR-NFR-004</code></td><td><strong>Efficiency</strong></td><td>Context sent to runtimes is minimized by design; unnecessary context is a defect, not overhead.</td></tr>
<tr><td><code>PR-NFR-005</code></td><td><strong>Resilience</strong></td><td>Runtime, network, and tooling failures are handled as expected conditions, never as corrupting events.</td></tr>
<tr><td><code>PR-NFR-006</code></td><td><strong>Maintainability</strong></td><td>The repository favors clarity over cleverness and remains maintainable by contributors who did not write it.</td></tr>
<tr><td><code>PR-NFR-007</code></td><td><strong>Extensibility</strong></td><td>Runtimes and Repository Assets of every kind are added without modifying AEOS itself.</td></tr>
<tr><td><code>PR-NFR-008</code></td><td><strong>Portability</strong></td><td>Projects and assets move between platforms, machines, and distributions without modification.</td></tr>
<tr><td><code>PR-NFR-009</code></td><td><strong>Comprehensibility</strong></td><td>Every product artifact is intelligible to both humans and AI runtimes without a separate machine format.</td></tr>
<tr><td><code>PR-NFR-010</code></td><td><strong>Verifiability</strong></td><td>AEOS is developed under its own TDD requirements; product behavior is covered by tests written before implementation.</td></tr>
<tr><td><code>PR-NFR-011</code></td><td><strong>Privacy</strong></td><td>Project content leaves the machine only through a user-approved runtime, and only what was disclosed in the proposal.</td></tr>
<tr><td><code>PR-NFR-012</code></td><td><strong>Compatibility</strong></td><td>AEOS coexists with the user's existing tools, editors, pipelines, and conventions rather than requiring their replacement.</td></tr>
</tbody>
</table>

---

## 20. Safety and Trust Model

AEOS operates on the user's machine, inside the user's repository, spending the user's money, on
behalf of the user's judgment. Trust is the product's primary asset and it is not recoverable once
spent.

### Trust Boundaries

<table>
<thead>
<tr><th align="left">Boundary</th><th align="left">AEOS position</th></tr>
</thead>
<tbody>
<tr><td><strong>The machine</strong></td><td>Belongs to the user. AEOS inspects freely, changes only with approval, and never touches what it does not own.</td></tr>
<tr><td><strong>The repository</strong></td><td>Belongs to the user. AEOS proposes changes; the user accepts them. History and uncommitted work are treated as irreplaceable.</td></tr>
<tr><td><strong>The runtime</strong></td><td>External. Everything crossing this boundary is disclosed before it crosses. Nothing crosses without approval.</td></tr>
<tr><td><strong>Credentials</strong></td><td>Never enter prompts, logs, reports, documentation, or Repository Assets. They are runtime state, never a product asset, and are never recorded.</td></tr>
<tr><td><strong>The user's judgment</strong></td><td>Final. AEOS may disagree and say so, but it does not act against a decision or re-litigate it by repetition.</td></tr>
</tbody>
</table>

### Failure Posture

- **Fail closed.** Uncertainty stops the workflow; it does not resolve toward action.
- **Fail visibly.** Failures are reported in the same detail as successes. Silent failure is a defect.
- **Fail consistently.** An interrupted or failed operation leaves a state AEOS can describe.
- **Fail without cost.** A halted workflow does not incur unapproved runtime usage.
- **Fail without blame.** Declining a proposal is a normal outcome, never treated as an error.

---

## 21. Success Metrics

Success is measured by whether AEOS changes how software gets built, not by activity counts.

<table>
<thead>
<tr><th align="left">Category</th><th align="left">Metric</th><th align="left">Signal of success</th></tr>
</thead>
<tbody>
<tr><td rowspan="2"><strong>Adoption</strong></td><td>Projects under AEOS governance</td><td>AEOS is adopted for real work, not evaluated and abandoned.</td></tr>
<tr><td>Projects still governed after 90 days</td><td>The discipline survives contact with deadlines.</td></tr>
<tr><td rowspan="2"><strong>Discipline</strong></td><td>Share of code changes produced through the TDD cycle</td><td>Test-first is the actual path, not the documented one.</td></tr>
<tr><td>Frequency of acknowledged TDD exceptions</td><td>Exceptions remain exceptional.</td></tr>
<tr><td rowspan="2"><strong>Supervision</strong></td><td>Share of consequential actions preceded by an approved proposal</td><td>The gate holds in practice.</td></tr>
<tr><td>Proposal decline rate</td><td>Non-zero: users are genuinely deciding, not rubber-stamping.</td></tr>
<tr><td rowspan="2"><strong>Independence</strong></td><td>Projects that have switched runtime without changing workflows</td><td>Runtime independence is real, not aspirational.</td></tr>
<tr><td>Distinct runtimes in active use across projects</td><td>The product is not a single-vendor wrapper in practice.</td></tr>
<tr><td rowspan="2"><strong>Portability</strong></td><td>Successful installations across all three platforms</td><td>Platform independence holds on real machines.</td></tr>
<tr><td>Projects used across more than one platform or distribution</td><td>Portability is exercised, not merely claimed.</td></tr>
<tr><td rowspan="2"><strong>Efficiency</strong></td><td>Context size per workflow step over time</td><td>Context Minimization improves rather than erodes.</td></tr>
<tr><td>Runtime invocations required per completed workflow</td><td>Orchestration reduces waste rather than adding overhead.</td></tr>
<tr><td rowspan="2"><strong>Trust</strong></td><td>Incidents of unintended destructive change</td><td>Target: zero. This is the metric that matters most.</td></tr>
<tr><td>Documentation drift detected and resolved</td><td>The repository stays true to itself.</td></tr>
</tbody>
</table>

---

## 22. Release Phases

Phases describe capability maturity. Dates are set by the owner and are not part of this document.

<table>
<thead>
<tr><th align="left">Phase</th><th align="left">Name</th><th align="left">Goal</th><th align="left">Exit criteria</th></tr>
</thead>
<tbody>
<tr>
<td><strong>1</strong></td>
<td>Foundation</td>
<td>A trustworthy core: inspect, explain, propose, confirm, execute — on all three platforms, with TDD enforced and at least one runtime integrated.</td>
<td>All Phase 1 <code>P0</code> requirements met on Windows, macOS, and Linux via GitHub Clone. AEOS builds itself under its own TDD workflow.</td>
</tr>
<tr>
<td><strong>2</strong></td>
<td>Orchestration</td>
<td>Full workflow orchestration, agentic sequencing, skills, capability matching, and the four minimum distribution methods.</td>
<td>All Phase 2 <code>P0</code> requirements met. A workflow runs unchanged on at least two runtimes. All four minimum distributions ship.</td>
</tr>
<tr>
<td><strong>3</strong></td>
<td>Lifecycle Completion</td>
<td>Remaining lifecycle stages — deployment, maintenance, drift detection, multi-runtime orchestration, CI/CD integration.</td>
<td>All Phase 3 <code>P0</code> requirements met. Every lifecycle stage in Section 9 is supported end to end.</td>
</tr>
<tr>
<td><strong>4</strong></td>
<td>Ecosystem</td>
<td>Broader distribution reach and ecosystem maturity.</td>
<td>Package manager, Docker, and IDE marketplace distributions available with identical product architecture.</td>
</tr>
</tbody>
</table>

**Phase invariants.** No phase may relax a product principle. No phase may ship a capability on a
subset of supported platforms. No phase may introduce a distribution-exclusive capability. No phase
may weaken an approval gate to meet a date.

---

## 23. Assumptions, Dependencies, and Risks

### Assumptions

1. Users have, or can obtain, access to at least one external AI runtime.
2. Users work in repositories under version control, or are willing to adopt it.
3. Users accept human supervision as a benefit rather than an obstacle.
4. Projects have, or can adopt, a test approach compatible with test-first development.
5. The AI runtime landscape will keep changing, and AEOS must absorb that change rather than track it.

### Dependencies

<table>
<thead>
<tr><th align="left">Dependency</th><th align="left">Nature</th><th align="left">Mitigation</th></tr>
</thead>
<tbody>
<tr><td>External AI runtimes</td><td>Required for inference-dependent capabilities</td><td>Multiple runtimes supported; non-inference capabilities remain functional without any.</td></tr>
<tr><td>Version control systems</td><td>Required for repository capabilities</td><td>Integration, not replacement; state is inspected before use.</td></tr>
<tr><td>Project build and test tooling</td><td>Required for TDD and testing capabilities</td><td>Orchestrated, not provided; detected rather than assumed.</td></tr>
<tr><td>CI/CD and delivery platforms</td><td>Required for deployment capabilities</td><td>Integration only; AEOS never becomes the pipeline.</td></tr>
<tr><td>Host operating systems</td><td>Required for all capabilities</td><td>Three officially supported platforms with equal commitment.</td></tr>
</tbody>
</table>

### Risks

<table>
<thead>
<tr><th align="left">Risk</th><th align="left">Impact</th><th align="left">Product response</th></tr>
</thead>
<tbody>
<tr><td><strong>Approval fatigue</strong> — users tire of confirming and stop reading proposals.</td><td>High</td><td>Action classes keep observation friction-free; proposals are concise and decision-oriented; scoped automation grants exist for repetitive low-risk work.</td></tr>
<tr><td><strong>Perceived slowness</strong> — the disciplined path feels slower than raw AI chat.</td><td>High</td><td>Incremental execution and context minimization keep steps fast; the value proposition is verified work, and the product does not compete on unverified speed.</td></tr>
<tr><td><strong>Runtime churn</strong> — vendors change APIs, models, and pricing.</td><td>Medium</td><td>The product is defined independently of runtime implementation; no capability depends on undocumented runtime behavior.</td></tr>
<tr><td><strong>Capability divergence</strong> — runtimes differ enough to leak into workflows.</td><td>Medium</td><td>Capability fit is reported before work begins; gaps surface to the user rather than being papered over.</td></tr>
<tr><td><strong>Scope creep toward reimplementation</strong> — rebuilding what runtimes already do well.</td><td>Medium</td><td>Runtime Strategy prohibits it; the architecture freeze routes such ideas to Appendix A.</td></tr>
<tr><td><strong>Platform inequality</strong> — one platform quietly becomes primary.</td><td>Medium</td><td>A capability is incomplete until it works on all three; this is a release gate, not a preference.</td></tr>
<tr><td><strong>Asset sprawl</strong> — rules, skills, and prompts accumulate beyond comprehension.</td><td>Low</td><td>Assets are versioned, scoped, inspectable, and reviewable in ordinary code review.</td></tr>
</tbody>
</table>

---

## 24. Glossary

<table>
<thead>
<tr><th align="left">Term</th><th align="left">Definition</th></tr>
</thead>
<tbody>
<tr><td><strong>AEOS</strong></td><td>AI Engineering Operating System. The product defined by this document.</td></tr>
<tr><td><strong>Action class</strong></td><td>Classification of an action by effect — observation, local change, external effect, or destructive — determining the approval required.</td></tr>
<tr><td><strong>Agentic orchestration</strong></td><td>Sequencing multi-step engineering work across runtimes, with each consequential step held to its approval gate.</td></tr>
<tr><td><strong>Approval gate</strong></td><td>The point at which a proposed action requires explicit human confirmation before execution.</td></tr>
<tr><td><strong>Automation grant</strong></td><td>An explicit, scoped, recorded, revocable delegation of approval authority for specific action classes.</td></tr>
<tr><td><strong>Context Minimization</strong></td><td>The principle that AEOS sends the smallest context sufficient for the task and can explain each inclusion.</td></tr>
<tr><td><strong>Distribution method</strong></td><td>An official way AEOS is delivered to users. Never changes the product architecture.</td></tr>
<tr><td><strong>Environment inspection</strong></td><td>Determining actual machine and project state before proposing or performing an action.</td></tr>
<tr><td><strong>Human-in-the-Loop</strong></td><td>The requirement that a human decides before AEOS acts consequentially.</td></tr>
<tr><td><strong>Product Boundary</strong></td><td>The line this document draws between product definition and implementation. What AEOS is belongs here; how AEOS is built belongs to architecture, specification, and runtime documents.</td></tr>
<tr><td><strong>Project profile</strong></td><td>The versioned Repository Asset describing a project's identity, technology, build and test approach, runtime selection, and applicable rules.</td></tr>
<tr><td><strong>Prompt</strong></td><td>A versioned, parameterized, portable Repository Asset composed of deliberately selected context and instruction.</td></tr>
<tr><td><strong>Proposal</strong></td><td>A statement of intended action including rationale, effects, reversibility, and the consequence of declining.</td></tr>
<tr><td><strong>Repository Asset</strong></td><td>Any durable, versioned artifact that forms part of the product and lives in the repository. Includes rules, skills, prompts, workflows, profiles, templates, playbooks, recipes, specifications, architecture documents, and manuals.</td></tr>
<tr><td><strong>Repository as Product</strong></td><td>The principle that the repository is the product and its authoritative source of truth.</td></tr>
<tr><td><strong>Rule</strong></td><td>A versioned, scoped engineering constraint applied during generation, review, and refactoring.</td></tr>
<tr><td><strong>Runtime</strong></td><td>An external AI system that performs inference. Always an integration, never a part of AEOS.</td></tr>
<tr><td><strong>Runtime State</strong></td><td>Transient, machine-local, or environment-specific information produced while AEOS runs. Not a Repository Asset and not part of the product.</td></tr>
<tr><td><strong>Skill</strong></td><td>A versioned, reusable, runtime-independent packaged engineering procedure.</td></tr>
<tr><td><strong>TDD cycle</strong></td><td>Define behavior → failing test → verify failure reason → minimal implementation → refactor green.</td></tr>
<tr><td><strong>Workflow</strong></td><td>A versioned, runtime-independent declaration of engineering steps, preconditions, approval gates, and success criteria.</td></tr>
</tbody>
</table>

---

## 25. Document Governance

### Status of This Document

This PRD is the **Product Source of Truth** for the AEOS repository. All downstream documents —
architecture, specifications, implementation guides, and tests — derive from it and trace to its
requirement identifiers.

### Change Control

<table>
<thead>
<tr><th align="left">Change type</th><th align="left">Requires</th><th align="left">Version impact</th></tr>
</thead>
<tbody>
<tr><td>Editorial correction, no semantic change</td><td>Contributor change, owner acceptance</td><td>Patch</td></tr>
<tr><td>New requirement, or clarification of an existing one</td><td>Owner approval</td><td>Minor</td></tr>
<tr><td>Change to a product principle, scope, or capability set</td><td>Explicit owner revision request</td><td>Major</td></tr>
<tr><td>Removal of a requirement</td><td>Explicit owner approval with recorded rationale</td><td>Major</td></tr>
</tbody>
</table>

### Requirement Identifier Policy

Identifiers are permanent. They are never reused, never renumbered, and never repurposed. A retired
requirement is marked retired in place, retaining its ID and its rationale.

### Architecture Freeze

The product definition in this document is frozen unless the owner explicitly requests a revision.
Ideas that would alter the product's concepts, capability set, or principles are recorded in
[Appendix A](#appendix-a--recommendations-for-future-releases) rather than applied.

### Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical
or Major findings remain.

### Traceability

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Obligation</th></tr>
</thead>
<tbody>
<tr><td>Architecture documents</td><td>Every architectural decision traces to one or more <code>PR-</code> identifiers.</td></tr>
<tr><td>Specifications</td><td>Every specified behavior traces to a <code>PR-</code> identifier.</td></tr>
<tr><td>Tests</td><td>Every <code>P0</code> requirement is covered by at least one test written before its implementation.</td></tr>
<tr><td>Issues and pull requests</td><td>Reference the <code>PR-</code> identifiers they advance.</td></tr>
</tbody>
</table>

### Revision History

<table>
<thead>
<tr><th align="left">Version</th><th align="left">Status</th><th align="left">Summary</th></tr>
</thead>
<tbody>
<tr><td>1.0.0</td><td>Superseded</td><td>Initial product definition. Establishes mission, thirteen product principles, interaction model, environment philosophy, ten capabilities, platform and distribution strategy, runtime strategy, scope, 151 numbered product requirements, and 12 quality attributes.</td></tr>
<tr><td>1.1.0</td><td>Freeze candidate</td><td>Product/implementation separation pass. Adds Executive Summary, Product Boundary, and Repository Product Assets and Runtime State. Restates implementation-prescriptive requirements as observable outcomes. Clarifies the operating-system metaphor. Adds <code>PR-RUN-016</code>, <code>PR-REP-015</code>, <code>PR-REP-016</code>. All pre-existing identifiers preserved unchanged. Mission, vision, and frozen product principles unaltered.</td></tr>
</tbody>
</table>

---

## Appendix A — Recommendations for Future Releases

Recorded under the architecture freeze. **None of these are part of the current product definition.**
Each requires an explicit owner revision request before it may be adopted.

<table>
<thead>
<tr><th align="left">#</th><th align="left">Recommendation</th><th align="left">Rationale</th><th align="left">Why deferred</th></tr>
</thead>
<tbody>
<tr>
<td>R1</td>
<td>Shareable rule and skill collections distributable between organizations.</td>
<td>Teams converge on similar standards; sharing accelerates adoption and raises baseline quality.</td>
<td>Introduces a distribution concept beyond the current asset model. Requires owner approval.</td>
</tr>
<tr>
<td>R2</td>
<td>Team-level policy governing which automation grants individual developers may issue.</td>
<td>Organizations may want to bound delegation centrally.</td>
<td>Adds an authority tier above the project owner. Significant scope change.</td>
</tr>
<tr>
<td>R3</td>
<td>Measured context-effectiveness feedback to inform prompt composition over time.</td>
<td>Would make Context Minimization empirical rather than heuristic.</td>
<td>Introduces a measurement and feedback concept not present in the current definition.</td>
</tr>
<tr>
<td>R4</td>
<td>AEOS-supplied workflow templates for common project archetypes.</td>
<td>Reduces time to first productive workflow for new users.</td>
<td>Distinct from user-authored Template assets, which are already in scope. Shipping opinionated templates risks becoming a framework layer, which conflicts with product positioning.</td>
</tr>
<tr>
<td>R5</td>
<td>Cross-project analytics on discipline and supervision metrics.</td>
<td>Would let engineering leads observe practice health across a portfolio.</td>
<td>Introduces cross-project data aggregation, which raises privacy and scope questions.</td>
</tr>
<tr>
<td>R6</td>
<td>Runtime capability benchmarking to advise runtime selection.</td>
<td>Users would gain evidence for choosing a runtime per workflow.</td>
<td>Risks appearing to rank vendors, in tension with Vendor Independence.</td>
</tr>
</tbody>
</table>

---

## Appendix B — Requirement Index

<table>
<thead>
<tr><th align="left">Prefix</th><th align="left">Capability</th><th align="left">Range</th><th align="left">Count</th><th align="left">Section</th></tr>
</thead>
<tbody>
<tr><td><code>PR-ENV</code></td><td>Environment management</td><td>001–013</td><td>13</td><td><a href="#181-environment-management--pr-env">15.1</a></td></tr>
<tr><td><code>PR-PRJ</code></td><td>Project management</td><td>001–011</td><td>11</td><td><a href="#182-project-management--pr-prj">15.2</a></td></tr>
<tr><td><code>PR-WFL</code></td><td>Workflow orchestration</td><td>001–016</td><td>16</td><td><a href="#183-workflow-orchestration--pr-wfl">15.3</a></td></tr>
<tr><td><code>PR-RUN</code></td><td>AI runtime orchestration</td><td>001–016</td><td>16</td><td><a href="#184-ai-runtime-orchestration--pr-run">15.4</a></td></tr>
<tr><td><code>PR-TDD</code></td><td>TDD workflow</td><td>001–012</td><td>12</td><td><a href="#185-tdd-workflow--pr-tdd">15.5</a></td></tr>
<tr><td><code>PR-DOC</code></td><td>Documentation generation</td><td>001–010</td><td>10</td><td><a href="#186-documentation-generation--pr-doc">15.6</a></td></tr>
<tr><td><code>PR-RUL</code></td><td>Rule management</td><td>001–010</td><td>10</td><td><a href="#187-rule-management--pr-rul">15.7</a></td></tr>
<tr><td><code>PR-SKL</code></td><td>Skill management</td><td>001–009</td><td>9</td><td><a href="#188-skill-management--pr-skl">15.8</a></td></tr>
<tr><td><code>PR-PMT</code></td><td>Prompt management</td><td>001–010</td><td>10</td><td><a href="#189-prompt-management--pr-pmt">15.9</a></td></tr>
<tr><td><code>PR-REP</code></td><td>Repository management</td><td>001–016</td><td>16</td><td><a href="#1810-repository-management--pr-rep">15.10</a></td></tr>
<tr><td><code>PR-PLT</code></td><td>Platform</td><td>001–006</td><td>6</td><td><a href="#1811-platform--pr-plt">15.11</a></td></tr>
<tr><td><code>PR-DST</code></td><td>Distribution</td><td>001–013</td><td>13</td><td><a href="#1812-distribution--pr-dst">15.12</a></td></tr>
<tr><td><code>PR-SAF</code></td><td>Safety</td><td>001–012</td><td>12</td><td><a href="#1813-safety--pr-saf">15.13</a></td></tr>
<tr><td><code>PR-NFR</code></td><td>Quality attributes</td><td>001–012</td><td>12</td><td><a href="#19-quality-attributes">16</a></td></tr>
<tr><td colspan="3"><strong>Total</strong></td><td><strong>166</strong></td><td>—</td></tr>
</tbody>
</table>

---

<div align="center">

**End of Product Requirements Document**

AEOS-PRD · Version 1.0.0 · Product Source of Truth

</div>
