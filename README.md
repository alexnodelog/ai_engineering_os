# AI Engineering Operating System (AEOS)

AEOS is an operating system for AI-assisted software engineering. It orchestrates the engineering
lifecycle — environment preparation, project initialization, requirement analysis, architecture,
test-driven development, agentic orchestration, code generation, review, refactoring, testing,
documentation, deployment, and maintenance — while performing no language-model inference of its
own. Inference is delegated to external AI runtimes the user chooses and controls. AEOS supplies
what those runtimes do not: durable project context, an enforced engineering discipline, deliberate
context minimization, a human approval gate in front of every consequential action, and a repository
that remains authoritative after the session ends.

This repository is the product. Every rule, workflow, and decision AEOS applies lives here, in a
form both a human and an AI runtime can read.

> **Status.** The documents this repository defines span Frozen, Freeze candidate, and Draft status,
> recorded individually in [Documentation Hierarchy](#documentation-hierarchy). This README describes
> the product those documents define; it does not assert that every described capability has shipped.

---

## Table of Contents

1. [What AEOS Is](#what-aeos-is)
2. [Why AEOS Exists](#why-aeos-exists)
3. [Core Philosophy](#core-philosophy)
4. [Major Features](#major-features)
5. [Repository Overview](#repository-overview)
6. [Documentation Hierarchy](#documentation-hierarchy)
7. [Installation Overview](#installation-overview)
8. [Runtime Support](#runtime-support)
9. [Development Workflow Overview](#development-workflow-overview)
10. [Repository Structure](#repository-structure)
11. [Contribution Overview](#contribution-overview)
12. [License](#license)
13. [Documentation Links](#documentation-links)

---

## What AEOS Is

*Operating system* describes what AEOS does for engineering activity, not how AEOS is built: it
makes many independent engineering capabilities usable together, consistently, so that every project
need not solve the same problems again on its own.

| AEOS is | AEOS is not |
| :--- | :--- |
| An orchestrator of external AI runtimes | An AI model or an inference engine |
| A human-supervised system with approval gates in front of every consequential action | An autonomous agent that acts on its own judgment |
| A process enforcer — test-first, explain-before-execute | A prompt collection or a template pack |
| Independent of vendor, runtime, model, platform, and distribution method | A wrapper around one vendor's ecosystem |
| A manager of versioned Repository Assets — rules, skills, prompts, workflows, and more | A hidden configuration store the user cannot read |
| A system that inspects the machine and the project before it acts | An installer that assumes a clean machine |

AEOS is defined completely, as a product, in the Product Requirements Document; the reasoning behind
that definition is recorded in the Vision Document. Neither is restated here — see
[Documentation Links](#documentation-links).

## Why AEOS Exists

AI coding tools have become capable. AI engineering has not become disciplined. Process improvised
per chat session is not reproducible; context pasted in bulk raises cost and lowers accuracy; tools
that act before they explain destroy trust; tests written last leave generated code unverified;
practice locked to one vendor must be rebuilt when the vendor changes; environments assumed rather
than inspected break working machines; and decisions made in a chat log are lost when the session
closes.

AEOS exists so that software built with artificial intelligence can be engineered rather than merely
produced — so that a developer opening any project, on any machine, with any AI runtime, in any year,
finds the same engineering discipline already present in the repository, in a form both they and
their AI can read. The full statement of this reasoning, including the invariants that must survive
every future revision of the product, is the Vision Document's subject and is not repeated here.

## Core Philosophy

Thirteen product principles are mandatory. They constrain every capability AEOS defines, and a
feature that violates one is treated as a defect rather than a feature.

| # | Principle | # | Principle |
| :--- | :--- | :--- | :--- |
| 1 | Human-in-the-Loop by Default | 8 | Runtime Independence |
| 2 | Explain Before Execute | 9 | Model Independence |
| 3 | Incremental Execution | 10 | Platform Independence |
| 4 | TDD-first Development | 11 | Distribution Independence |
| 5 | Repository as Product | 12 | Safety by Default |
| 6 | Context Minimization | 13 | Extensibility by Design |
| 7 | Vendor Independence | | |

Where two principles pull in opposite directions, Safety by Default resolves first, then
Human-in-the-Loop by Default, then Explain Before Execute, then TDD-first Development, then
Repository as Product, with every remaining principle weighed on the merits of the specific case.

The convictions behind these principles, and the values, non-goals, and invariants that govern the
product's long-term direction, are the Vision Document's subject.

## Major Features

AEOS provides ten product capabilities. Each is defined completely, as numbered requirements, in the
Product Requirements Document.

| Capability | Purpose |
| :--- | :--- |
| Environment management | Know the machine; prepare it non-destructively. |
| Project management | Establish, adopt, and describe projects without overwriting existing work. |
| Workflow orchestration | Define and drive engineering workflows with approval gates. |
| AI runtime orchestration | Select, adapt, and coordinate external runtimes. |
| TDD workflow | Enforce test-first development as the primary path. |
| Documentation generation | Produce and maintain documentation from the repository's actual state. |
| Rule management | Express engineering constraints as versioned, inspectable assets. |
| Skill management | Package reusable engineering procedures. |
| Prompt management | Compose deliberate, minimized context and instruction for a runtime. |
| Repository management | Treat the repository as the single source of truth for code and every Repository Asset. |

Every consequential action AEOS performs follows one loop, applied without exception: **Inspect →
Explain → Propose → Confirm → Execute → Report**. Its phases, and the classification of actions that
determines how strict its approval gate is, are stated in full in the Product Requirements Document.

## Repository Overview

AEOS-VISION invariant V5 states that the repository is the product: what is not in the repository
does not exist, and a session ends while the repository persists as the source of truth for the next
human and the next AI runtime.

Durable, versioned content that constitutes the product — rules, skills, prompts, workflows,
profiles, templates, playbooks, recipes, specifications, architecture documents, and manuals — is a
**Repository Asset**: durable, versioned, inspectable, consumable by an AI runtime, portable, and
extensible without modifying AEOS itself. Content that is a consequence of running AEOS rather than a
statement of what it is — caches, temporary execution state, credentials, telemetry, machine-specific
configuration — is **Runtime State**, and is deliberately excluded from the product: if losing
something costs only repeated work, it is Runtime State; if losing it costs product meaning, it is a
Repository Asset. The full definition of both, as product concepts, is the Product Requirements
Document's subject; their physical placement is the Repository Layout Guide's subject.

## Documentation Hierarchy

AEOS-DOCSTD assigns documentation **authority**, not merely reading order: a document must not
contradict a document above it in the hierarchy, and every derivative document traces to the layer
above it and ultimately to a Product Requirements identifier.

| Layer | Document | ID | Version | Status |
| :--- | :--- | :--- | :--- | :--- |
| Vision | Vision Document | `AEOS-VISION` | 1.0.1 | Frozen |
| Product Requirements | Product Requirements Document | `AEOS-PRD` | 1.2.0 | Freeze candidate |
| Glossary *(foundational)* | Glossary | `AEOS-GLOSSARY` | 1.0.1 | Frozen |
| Document Standard *(foundational)* | Document Standard | `AEOS-DOCSTD` | 3.0.0 | Frozen |
| Technology Catalog | Supported Technologies | `AEOS-TECH` | 1.0.1 | Frozen |
| Architecture | Architecture | `AEOS-ARCH` | 1.1.0 | Frozen |
| Blueprint | Blueprint | `AEOS-BLUEPRINT` | 1.1.0 | Frozen |
| Specification (form) | Specification Standard | `AEOS-SPECSTD` | 1.1.0 | Frozen |
| Specification | Runtime Adapter Specification | `AEOS-SPEC-ADP` | 1.0.0 | Draft |
| Specification | Runtime Negotiation Specification | `AEOS-SPEC-NEG` | 1.0.0 | Draft |
| Specification | Model Registry Specification | `AEOS-SPEC-MDL` | 1.0.0 | Freeze candidate |
| Implementation Guide | Project Bootstrap Guide | `AEOS-BOOT` | 3.0.0 | Draft |
| Implementation Guide | Environment Setup Guide | `AEOS-ENVSETUP` | 1.0.0 | Draft |
| Developer Guide | *(none authored yet)* | — | — | — |

Vision and Glossary and Document Standard are **foundational**: they serve every layer at once rather
than deriving from, or being derived from by, a single neighbor. Every remaining listed layer derives
from the layer above it.

Two further document groups exist in this repository whose position in the hierarchy above is
reserved to the product owner's decision, and which comply with every Document Standard rule that
does not itself depend on that position:

| Group | Document | ID | Version | Status |
| :--- | :--- | :--- | :--- | :--- |
| Runtime | Runtime Flow Specification | `AEOS-RTF` | 1.0.0 | Freeze candidate |
| Runtime | Runtime Registry | `AEOS-RUNTIME-REG` | 1.0.0 | Freeze candidate |
| Runtime | Runtime Capability Specification | `AEOS-CAP` | 1.0.0 | Freeze candidate |
| Repository | Repository Layout Guide | `AEOS-LAYOUT` | 1.0.0 | Draft |

## Installation Overview

AEOS supports multiple official distribution methods. The product architecture, capability set, and
behavior are identical regardless of which one is used; distribution affects packaging, discovery,
and update mechanics only.

| Method | Primary user | Status |
| :--- | :--- | :--- |
| GitHub Clone | Contributors, teams standardizing on a pinned revision | Official |
| Native Installer | Developers who want a supported, updatable install | Official |
| MCP Distribution | Users who work primarily inside an MCP-capable AI runtime or IDE | Official |
| Portable Distribution | Locked-down, air-gapped, or ephemeral machines | Official |
| Package Managers | Native per-platform ecosystem installation | Planned |
| Docker Images | Reproducible, isolated environments for CI | Planned |
| IDE Marketplace Distribution | Discovery and installation from the editing surface | Planned |

Windows, macOS, and Linux are officially supported as equal citizens: no platform is primary, and a
capability that works on only one platform is treated as incomplete. Before a new AEOS repository is
usable, a host machine satisfies the prerequisites the Environment Setup Guide states, and the
repository itself is produced by the reproducible, deterministic, platform-neutral procedure the
Project Bootstrap Guide states. Neither procedure is restated here.

## Runtime Support

AEOS integrates external AI runtimes; it never reimplements, competes with, or hides them, and it
performs no inference of its own. Runtime selection belongs to the user, and switching runtime is a
configuration change rather than a migration project.

| Category | Examples |
| :--- | :--- |
| Commercial AI services | Claude, OpenAI, Gemini |
| AI-assisted development environments | Cursor, GitHub Copilot |
| Open-source models | Locally or privately hosted models of any size |
| Interoperability standards | MCP and comparable standards, present and future |
| Extensions | Plugins and integrations supplied by users or third parties |

This list is illustrative: being named confers no privilege, and being absent implies no exclusion.
The observable runtime lifecycle, the registry a runtime is discovered through, the capability model
shared by every runtime and model, the adapter contract mediating each runtime, the negotiation that
matches a workflow step to a compatible runtime, and the model registry itself are each governed by a
dedicated document — see [Documentation Hierarchy](#documentation-hierarchy).

## Development Workflow Overview

AEOS orchestrates the complete engineering lifecycle: environment preparation, project
initialization, requirement analysis, architecture, TDD, agentic orchestration, code generation,
review, refactoring, testing, documentation, deployment, and maintenance. Real projects re-enter
these stages continuously rather than proceeding in a straight line.

Every consequential step within that lifecycle passes through the same interaction loop:

```text
INSPECT → EXPLAIN → PROPOSE → CONFIRM → EXECUTE → REPORT
```

Inspection determines actual state before any intent is formed; explanation states what was found in
language a human can act on; a proposal states the intended action, its rationale, its effects, its
reversibility, and the consequence of declining; execution performs exactly what was approved and
nothing more; and reporting states what actually happened, including partial completion or failure.
Actions are classified by effect — observation, local change, external effect, destructive — and the
classification determines what approval is required. The complete statement of this model, including
how automation may be delegated and revoked, is the Product Requirements Document's subject.

## Repository Structure

The repository root holds exactly three things: `README.md`, a version-control ignore file where the
chosen distribution method depends on version control, and `docs/`. A project owner may add a license
file at the root at their own discretion; nothing in this repository's governing documents requires
or forbids one.

```text
<repository root>
├── README.md
├── .gitignore                    (where the Distribution Method depends on version control)
└── docs/
    ├── REPOSITORY_LAYOUT.md
    ├── foundation/
    ├── architecture/
    ├── product/
    ├── specification/
    ├── runtime/
    ├── implementation/
    └── developer/
```

Each `docs/` subdirectory houses one documentation-hierarchy layer, and a document is placed at the
path its own metadata declares rather than at a path this README assigns it. `docs/developer/` is
reserved for Developer Guides in advance of one being authored. The authoritative statement of this
structure — what belongs at each path, why, and who is answerable for it — is the Repository Layout
Guide's subject; the procedure that produces it from nothing is the Project Bootstrap Guide's
subject.

## Contribution Overview

No Developer Guide exists yet for this repository — `docs/developer/` is reserved for one, not yet
authored. Until it exists, anyone proposing a change to AEOS, human or AI, is bound by the guiding
principles the Vision Document states for contributors: prefer clarity over novelty; minimize
unnecessary complexity; keep architectural responsibilities separate; preserve backward compatibility
where reasonable; justify every major decision against the project's lifetime rather than a deadline;
treat human approval as the default for engineering decisions; do not rebuild what mature systems
already do well; record better ideas rather than applying them silently; write for both a human
maintainer and an AI runtime from one artifact; test first, including here; leave the repository more
understandable than it was found; and ask, rather than assume, when in doubt.

Each frozen document also states its own change-control process — the review classification it uses,
the approval a given kind of change requires, and its own revision history — in that document's
Document Governance section. A proposed change to a document is evaluated against that document's own
governance rules, not against a rule stated here.

## License

No frozen document in this repository declares a license. Establishing one is explicitly reserved to
the project owner, outside the repository initialization procedure the Project Bootstrap Guide
states. A license file, where the project owner wants one, may be added at the repository root at the
owner's discretion.

## Documentation Links

| Document | Path |
| :--- | :--- |
| Vision Document | `docs/foundation/VISION.md` |
| Product Requirements Document | `docs/foundation/PRD.md` |
| Glossary | `docs/foundation/GLOSSARY.md` |
| Document Standard | `docs/foundation/DOCUMENT_STANDARD.md` |
| Supported Technologies | `docs/foundation/SUPPORTED_TECHNOLOGIES.md` |
| Architecture | `docs/architecture/ARCHITECTURE.md` |
| Blueprint | `docs/architecture/BLUEPRINT.md` |
| Specification Standard | `docs/product/SPECIFICATION_STANDARD.md` |
| Runtime Adapter Specification | `docs/specification/RUNTIME_ADAPTER_SPEC.md` |
| Runtime Negotiation Specification | `docs/specification/RUNTIME_NEGOTIATION_SPEC.md` |
| Model Registry Specification | `docs/specification/MODEL_REGISTRY.md` |
| Runtime Flow Specification | `docs/runtime/RUNTIME_FLOW.md` |
| Runtime Registry | `docs/runtime/RUNTIME_REGISTRY.md` |
| Runtime Capability Specification | `docs/runtime/RUNTIME_CAPABILITY_SPEC.md` |
| Project Bootstrap Guide | `docs/implementation/PROJECT_BOOTSTRAP.md` |
| Environment Setup Guide | `docs/implementation/ENVIRONMENT_SETUP.md` |
| Repository Layout Guide | `docs/REPOSITORY_LAYOUT.md` |

---

*This document is the repository's root-level entry point, per the Repository Layout Guide's
root-level entries and the Project Bootstrap Guide's placement rules. It is not itself a document of
the documentation hierarchy above, carries no Document ID, and imposes no requirement; where it
appears to conflict with any document it links to, the linked document governs and the conflict is a
defect in this README to be reported.*
