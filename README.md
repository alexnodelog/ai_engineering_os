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

```text
        Developer
            |
            v  (decides)
   +-------------------+          +------------------------+
   |  AEOS Repository  | <------> |  External AI Runtime    |
   |  rules, workflows, |  uses   |  (performs inference)   |
   |  history, context  |         +------------------------+
   +-------------------+
            |
            v
     Engineering Workflow
```

---

## Table of Contents

| | |
| :--- | :--- |
| 1. [Who Is AEOS For?](#who-is-aeos-for) | 9. [Documentation Hierarchy](#documentation-hierarchy) |
| 2. [Start Here](#start-here) | 10. [Installation Overview](#installation-overview) |
| 3. [Current Repository Status](#current-repository-status) | 11. [Runtime Support](#runtime-support) |
| 4. [What AEOS Is](#what-aeos-is) | 12. [Development Workflow Overview](#development-workflow-overview) |
| 5. [Why AEOS Exists](#why-aeos-exists) | 13. [Contribution Overview](#contribution-overview) |
| 6. [Core Philosophy](#core-philosophy) | 14. [License](#license) |
| 7. [Major Features](#major-features) | 15. [Further Reading](#further-reading) |
| 8. [The Repository](#the-repository) | |

---

## Who Is AEOS For?

| Reader | What AEOS offers them |
| :--- | :--- |
| Solo developer | A dependable process without a team to enforce it, and confidence that nothing changes behind their back. |
| Engineering lead or architect | Rules, workflows, and standards expressed as versioned assets that apply uniformly to every developer and every runtime. |
| Platform or DevOps engineer | Predictable installation on Windows, macOS, and Linux, non-destructive environment handling, and integration with existing repository and CI/CD systems. |
| Open-source maintainer | A repository that encodes its own engineering practice, so contributions from unknown people using unknown tools arrive already aligned. |
| AI runtime *(non-human)* | Unambiguous, minimal, machine-consumable definitions of rules, skills, prompts, and workflow state. |

## Start Here

This repository is documentation-first: the product is defined completely before any of it is
built. What to read first depends on why the repository was opened.

| Reading for... | Start with |
| :--- | :--- |
| A first impression of what AEOS is and why it exists | [Vision Document](docs/foundation/VISION.md), then [Product Requirements Document](docs/foundation/PRD.md) |
| How AEOS is structured, to build or extend it | [Architecture](docs/architecture/ARCHITECTURE.md), then [Blueprint](docs/architecture/BLUEPRINT.md), then the Specification layer |
| Preparing a machine to work in this repository | [Environment Setup Guide](docs/implementation/ENVIRONMENT_SETUP.md), then [Project Bootstrap Guide](docs/implementation/PROJECT_BOOTSTRAP.md) |
| What a specific AEOS term means | [Glossary](docs/foundation/GLOSSARY.md) |

The documents above are also, in that order, the authority order the documentation hierarchy assigns
them:

```text
Vision --> Product Requirements --> Architecture --> Blueprint --> Specification --> Implementation Guides
```

The complete map of every document, including the two groups whose position in that order is not
yet assigned, is [Documentation Hierarchy](#documentation-hierarchy).

## Current Repository Status

| Layer | Status |
| :--- | :--- |
| Vision, Glossary, Document Standard, Technology Catalog | Frozen |
| Product Requirements | Freeze candidate |
| Architecture, Blueprint | Frozen |
| Specification Standard | Frozen |
| Specification (behavior-domain documents) | Draft to Freeze candidate |
| Runtime *(position not yet assigned)* | Freeze candidate |
| Implementation Guides | Draft |
| Repository Layout *(position not yet assigned)* | Draft |
| Developer Guides | Not yet authored |

`Frozen` changes only through an explicit owner revision request. `Freeze candidate` and `Draft`
remain open to ordinary review. Per-document versions and identifiers are in
[Documentation Hierarchy](#documentation-hierarchy). This summary describes the documentation set;
it does not assert that every capability those documents define has been implemented.

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

AEOS is defined completely, as a product, in the [Product Requirements Document](docs/foundation/PRD.md).

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
their AI can read. The reasoning behind this, and the invariants that must survive every future
revision of the product, are recorded in the [Vision Document](docs/foundation/VISION.md).

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
The convictions behind these principles are recorded in the Vision Document.

## Major Features

AEOS provides ten product capabilities, each defined as numbered requirements in the Product
Requirements Document.

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
Explain → Propose → Confirm → Execute → Report**. Its phases, and the action classes that determine
how strict its approval gate is, are stated in full in the Product Requirements Document.

## The Repository

AEOS-VISION invariant V5 states that the repository is the product: what is not in the repository
does not exist, and the repository remains the source of truth for the next human and the next AI
runtime after a session ends.

Durable, versioned content that constitutes the product — rules, skills, prompts, workflows,
profiles, templates, playbooks, recipes, specifications, architecture documents, and manuals — is a
**Repository Asset**. Content that is a consequence of running AEOS rather than a statement of what
it is — caches, temporary execution state, credentials, telemetry, machine-specific configuration —
is **Runtime State** and is excluded from the product: if losing something costs only repeated work,
it is Runtime State; if losing it costs product meaning, it is a Repository Asset.

The repository root holds exactly three things: `README.md`, a version-control ignore file where the
chosen distribution method depends on version control, and `docs/`.

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
path its own metadata declares. `docs/developer/` is reserved for Developer Guides in advance of one
being authored. The [Repository Layout Guide](docs/REPOSITORY_LAYOUT.md) is the authoritative
statement of this structure; the [Project Bootstrap Guide](docs/implementation/PROJECT_BOOTSTRAP.md)
is the procedure that produces it from nothing.

## Documentation Hierarchy

[Start Here](#start-here) gives a short reading path. The table below is the complete map: every
document this repository defines, the authority layer AEOS-DOCSTD assigns it, and its current
version and status. AEOS-DOCSTD assigns documentation **authority**, not merely reading order: a
document must not contradict a document above it, and every derivative document traces to the layer
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

Vision, Glossary, and Document Standard are **foundational**: they serve every layer at once rather
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

To prepare a new AEOS repository for use:

1. Prepare the host machine — see the [Environment Setup Guide](docs/implementation/ENVIRONMENT_SETUP.md).
2. Initialize the repository — see the [Project Bootstrap Guide](docs/implementation/PROJECT_BOOTSTRAP.md).

Both procedures are reproducible, deterministic, and platform-neutral; neither is restated here.

Once a repository exists, AEOS supports multiple official distribution methods. The product
architecture, capability set, and behavior are identical regardless of which one is used;
distribution affects packaging, discovery, and update mechanics only.

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
capability that works on only one platform is treated as incomplete.

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
The observable runtime lifecycle, the registry a runtime is discovered through, the shared capability
model, the adapter contract, the negotiation that matches a workflow step to a compatible runtime,
and the model registry are each covered by one of the Runtime and Specification documents in
[Documentation Hierarchy](#documentation-hierarchy).

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
classification determines what approval is required. This model is stated in full in the Product
Requirements Document.

## Contribution Overview

**How to contribute.** No Developer Guide exists yet for this repository — `docs/developer/` is
reserved for one, not yet authored. Until it exists, a proposed change is evaluated against the
Vision Document's guiding principles for contributors, listed below, and against the Document
Governance section of whichever document the change targets — not against a single repository-wide
process.

**Which documents are open to change.** See [Current Repository Status](#current-repository-status).
A `Frozen` document changes only through an explicit owner revision request; a `Freeze candidate` or
`Draft` document remains open to review under its own Document Governance section.

**Where discussion begins.** No frozen document designates a specific venue for proposals or review.
That is not decided here, and none is invented; it remains open for a future Developer Guide.

**Guiding principles for any contributor, human or AI:**

- Prefer clarity over novelty.
- Minimize unnecessary complexity.
- Keep architectural responsibilities separate.
- Preserve backward compatibility where reasonable.
- Justify every major decision against the project's lifetime, not a deadline.
- Treat human approval as the default for engineering decisions.
- Do not rebuild what mature systems already do well.
- Record better ideas rather than applying them silently.
- Write for both a human maintainer and an AI runtime from one artifact.
- Test first, including here.
- Leave the repository more understandable than it was found.
- Ask, rather than assume, when in doubt.

## License

No frozen document in this repository declares a license. Establishing one is explicitly reserved to
the project owner, outside the repository initialization procedure the Project Bootstrap Guide
states. A license file, where the project owner wants one, may be added at the repository root at the
owner's discretion.

## Further Reading

Every document this README names is listed here, with its path.

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

*This document is the repository's root-level entry point. It is not itself a document of the
documentation hierarchy above, carries no Document ID, and imposes no requirement; where it appears
to conflict with any document it links to, the linked document governs and the conflict is a defect
in this README to be reported.*
