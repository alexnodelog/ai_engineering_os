# AI Engineering Operating System

## AEOS — Overview

*The permanent conceptual map of AEOS: how its Vision, Product, Architecture, Runtime, and
Implementation layers relate, and where each concept a reader hears about actually lives.*

| Field | Value |
| :--- | :--- |
| **Document** | Overview |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-OVERVIEW |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Documentation Governance Board, AEOS |
| **Audience** | Users and contributors of AEOS, and AI runtimes reading this repository — anyone seeking a single, connected account of how AEOS's parts relate |
| **Suggested path** | `docs/OVERVIEW.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `AEOS_RUNTIME_FLOW.md` (AEOS-RTF) · `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG) · `RUNTIME_ADAPTER_SPEC.md` (AEOS-SPEC-ADP) · `RUNTIME_NEGOTIATION_SPEC.md` (AEOS-SPEC-NEG) · `RUNTIME_CAPABILITY_SPEC.md` (AEOS-CAP) · `MODEL_REGISTRY.md` (AEOS-SPEC-MDL) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) · `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) |
| **Supersedes** | None |

> **Authority of this document.**
> This document provides a conceptual orientation to AEOS as a whole: how its Vision, Product,
> Architecture, Blueprint, Runtime, and Implementation layers relate, and how a reader moves between
> them. It states no product requirement, no architectural decision, no Blueprint arrangement, no
> specified behavior, no runtime lifecycle, and no terminology of its own. Every substantive
> statement in this document traces to, and is owned by, the frozen or in-progress document named
> alongside it. This document does not redefine, extend, narrow, or restate a definition,
> requirement, structural decision, or specified behavior recorded elsewhere; where a statement here
> appears to do so, that is a defect in this document, is reported rather than acted upon, and the
> referenced document governs.
>
> This document's position in the documentation hierarchy AEOS-DOCSTD Section 4.1 defines is reserved
> to the owner's decision under AEOS-DOCSTD rule `H5`, in the manner Section 4.5 reserves the Runtime
> layer's position. Until decided, this document complies with every rule in AEOS-DOCSTD that does
> not itself depend on hierarchy position, and is placed directly under `docs/`, outside every
> layer-specific subdirectory, following the placement `REPOSITORY_LAYOUT.md` Section 5.2 records for
> itself under the same provision. This document asserts no obligation of its own and accordingly
> uses no RFC 2119 normative language, consistent with AEOS-DOCSTD Section 7.3's treatment of a
> document that carries no rule of its own to state.
>
> This document differs from the repository's `README.md` in scope and depth. The README is the
> repository's root-level entry point and is explicitly not a document of the documentation
> hierarchy; it carries no Document ID and states, briefly, what a first-time reader needs before
> going further. This document is a Document under AEOS-DOCSTD, addresses every documentation-hierarchy
> layer and every Runtime document in one place, and is intended to remain the durable map between
> them as the repository grows. Where this document and the README differ in a statement of fact, the
> document each summarizes governs, and the difference is reported.
>
> Where this document and a document of higher authority, or any document it summarizes, both speak
> to a subject, the summarized or higher-authority document governs, and any conflict here is a
> defect to be reported rather than acted upon.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Why AEOS Exists — The Vision](#3-why-aeos-exists--the-vision)
4. [What AEOS Is — Product Scope](#4-what-aeos-is--product-scope)
5. [How AEOS Is Structured — The Architecture](#5-how-aeos-is-structured--the-architecture)
6. [The Runtime Layer — Orchestrating External AI](#6-the-runtime-layer--orchestrating-external-ai)
7. [The Workflow Engine — Sequencing and Supervision](#7-the-workflow-engine--sequencing-and-supervision)
8. [Agentic Orchestration — How AEOS Relates to Agents](#8-agentic-orchestration--how-aeos-relates-to-agents)
9. [Repository Assets — Rules, Skills, Prompts, Workflows, Templates, and Profiles](#9-repository-assets--rules-skills-prompts-workflows-templates-and-profiles)
10. [Extensibility — Extension Points, and What AEOS Does Not Yet Name](#10-extensibility--extension-points-and-what-aeos-does-not-yet-name)
11. [The Technology Catalog — Frameworks and Recognized Technologies](#11-the-technology-catalog--frameworks-and-recognized-technologies)
12. [The Documentation Hierarchy](#12-the-documentation-hierarchy)
13. [Repository Organization](#13-repository-organization)
14. [The Engineering Lifecycle and the Interaction Loop](#14-the-engineering-lifecycle-and-the-interaction-loop)
15. [Distribution](#15-distribution)
16. [Future Roadmap](#16-future-roadmap)
17. [How the Pieces Connect](#17-how-the-pieces-connect)
18. [Document Governance](#18-document-governance)
19. [Appendix A — Document Map (Non-Normative)](#appendix-a--document-map-non-normative)

---

## 1. Executive Summary

A repository this large gives a new reader two ways to get lost. One is depth: each frozen document
is precise about a narrow subject, and precision reads as density to someone who has not yet placed
that subject on a larger map. The other is distance: a reader who wants to know how "the Workflow
Engine" relates to "an adapter" or how a Skill differs from a Rule has to assemble that relationship
themselves from documents that, correctly, never restate one another.

This document is the map. It takes every major concept AEOS defines — why it exists, what it is,
how it is structured, how it reaches external AI, how it sequences and supervises work, what a
Repository Asset is, where extension is admitted, how its documentation and its repository are
organized, how it moves through an engineering lifecycle, how it reaches users, and where it is
going — and places each one in relation to the others, in one artifact a person or an AI runtime can
read start to finish. It states nothing new. Every claim below is a restatement, in connected form,
of something a named frozen or in-progress document already states on its own authority.

Where this document uses a term that no frozen AEOS document currently defines, it says so plainly
rather than defining one here — consistent with the discipline `PROJECT_BOOTSTRAP.md` and
`REPOSITORY_LAYOUT.md` already apply to themselves: a gap is recorded, not filled by invention.

---

## 2. Scope and Applicability

### 2.1 What This Document Provides

This document provides, and is the map for:

- the reasoning chain from why AEOS exists to what it is, how it is structured, and how it runs;
- a one-place account of every documentation-hierarchy layer and every Runtime document, and how
  they relate to one another;
- an accurate placement of concepts commonly asked about — an Agent, a Skill, a Hook, a Command, a
  Framework — against what AEOS actually defines, including an explicit statement where a term has
  no defined AEOS meaning;
- the shape of the AEOS repository itself: its documentation hierarchy and its physical organization;
- the engineering lifecycle AEOS orchestrates and the loop every consequential action follows;
- how AEOS reaches users, and what is planned but not yet part of the product.

This list is complete for what this document adds. It adds no content beyond connection and
reference.

### 2.2 What This Document Does Not Provide

| Not provided here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, reviewed, and frozen | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| How AEOS is structurally organized | AEOS-ARCH |
| How that structure is arranged to be built | AEOS-BLUEPRINT |
| The structural discipline a Specification document follows | AEOS-SPECSTD |
| The observable runtime lifecycle of AEOS | AEOS-RTF |
| What a project may depend on the Runtime Registry to do | AEOS-RUNTIME-REG |
| The observable contract between AEOS and a Runtime Adapter | AEOS-SPEC-ADP |
| How a Runtime, Adapter, and Model combination is found compatible with a step | AEOS-SPEC-NEG |
| The Engineering Capability vocabulary itself | AEOS-CAP |
| The identity, metadata, and lifecycle of a registered Model | AEOS-SPEC-MDL |
| How a new AEOS repository is initialized | AEOS-BOOT |
| What a host machine must provide before that initialization is attempted | AEOS-ENVSETUP |
| The permanent shape of the AEOS repository, path by path | AEOS-LAYOUT |

A statement in this document that grants a capability, imposes a requirement, defines a term, or
decides a structure is a defect in this document. It is reported rather than acted upon, and the
document listed above as owner governs.

### 2.3 Applicability

This document applies identically to a human reader — a developer, an engineering lead, or a
contributor — and to an AI runtime consuming this repository, consistent with AEOS-DOCSTD Section
2.4. It is written for a first encounter with AEOS and for a returning reader who wants to relocate a
concept quickly; [Appendix A](#appendix-a--document-map-non-normative) is built for the second case.

---

## 3. Why AEOS Exists — The Vision

AEOS-VISION states why AEOS exists and what it must always remain. Its vision statement:

> **AEOS exists so that software built with artificial intelligence can be engineered rather than
> merely produced.**

Its mission pursues that statement along four lines:

| Line | What AEOS is trying to change |
| :--- | :--- |
| Make the process explicit | Move engineering practice out of individual habit and into the repository, where it can be read, criticized, and improved. |
| Keep the human deciding | Structure work so the person retains genuine comprehension of what is about to occur, not just veto power. |
| Keep verification ahead of generation | Hold test-first development as the practice that scales verification at the rate AI can produce code. |
| Keep the practice free | Let a team's accumulated engineering knowledge survive every change of tool, model, platform, and employer. |

AEOS-VISION also states eight non-goals — boundaries considered and deliberately rejected, not gaps
awaiting a future release. AEOS will not become: a replacement operating system; a proprietary AI
platform; a single-vendor ecosystem; a fully autonomous software factory; a no-code or low-code
platform; an IDE, editor, or application framework; a replacement for version control, CI/CD, or
delivery systems; or (the eighth, recorded in AEOS-VISION Section 8 in full) a source of unbounded
scope. Two of these recur throughout this document and are worth holding in view now: AEOS does not
aim to remove the human deciding, and AEOS does not aim to become an application framework.

Ten invariants reduce the vision to its irreducible form — the properties whose loss would mean AEOS
no longer exists in any meaningful sense, whatever continued to carry the name:

| ID | Invariant |
| :--- | :--- |
| `V1` | AEOS performs no inference. It never contains a model and never becomes one. |
| `V2` | The human decides. Consequential action follows a human decision; delegation is chosen explicitly and can always be withdrawn. |
| `V3` | Nothing consequential happens without being understandable first. |
| `V4` | Verification precedes implementation. Test-first is the practice, including for AEOS itself. |
| `V5` | The repository is the product, and remains meaningful when AEOS is not running. |
| `V6` | No vendor, runtime, model, platform, or distribution is privileged or required. |
| `V7` | The safe path is the default, and uncertainty stops the work rather than resolving toward action. |
| `V8` | The user's machine, repository, credentials, and judgment belong to the user. |
| `V9` | What AEOS does can be inspected, by the human it works for and by the runtimes it works with. |
| `V10` | AEOS is extended, not modified, so that a user's practice never depends on forking the product. |

The full reasoning behind each — the long-term vision, the core philosophy, the design values, and
the guiding principles for contributors — is AEOS-VISION's subject and is not repeated here.

---

## 4. What AEOS Is — Product Scope

AEOS-PRD defines what AEOS is and what it must do, completely, as the Product Contract:

> AEOS lets developers build software through AI-assisted, human-supervised engineering workflows,
> orchestrating any AI runtime across the full lifecycle without ever becoming a model, an IDE, or a
> framework.

| AEOS is | AEOS is not |
| :--- | :--- |
| An operating system for AI-assisted engineering work | An AI model or an inference engine |
| An orchestrator of external AI runtimes | A replacement for those runtimes |
| A human-supervised system with approval gates | An autonomous agent that acts on its own judgment |
| A process enforcer: TDD-first, explain-before-execute | A prompt collection or a template pack |
| Vendor, runtime, model, platform, and distribution independent | A wrapper around one vendor's ecosystem |
| A manager of Repository Assets as versioned product artifacts | A hidden configuration store the user cannot read |
| An IDE-agnostic system usable from any editing surface | An IDE, an editor, or an application framework |

The product is ten capabilities, each expressed as numbered requirements in AEOS-PRD Section 18:

| # | Capability | Purpose |
| :--- | :--- | :--- |
| `C1` | Environment management | Know the machine; prepare it non-destructively. |
| `C2` | Project management | Establish, adopt, and describe projects without overwriting existing work. |
| `C3` | Workflow orchestration | Define and drive engineering workflows with approval gates. |
| `C4` | AI runtime orchestration | Select, adapt, and coordinate external runtimes. |
| `C5` | TDD workflow | Enforce test-first development as the primary path. |
| `C6` | Documentation generation | Produce and maintain documentation from the repository's actual state. |
| `C7` | Rule management | Express engineering constraints as versioned, inspectable assets. |
| `C8` | Skill management | Package reusable engineering procedures. |
| `C9` | Prompt management | Compose deliberate, minimized context and instruction for a runtime. |
| `C10` | Repository management | Treat the repository as the single source of truth for code and every Repository Asset. |

Version control, Git, CI/CD integration, code generation, testing, review, and deployment are
explicitly in scope, as capabilities AEOS orchestrates rather than replaces. Explicitly out of scope:
language-model inference; model training, fine-tuning, or hosting; replacing version control, CI/CD,
or hosting platforms; and being an IDE, editor, or application framework. Everything else is in
scope, with implementation detail deferred to downstream documents rather than treated as an
exclusion. The complete statement, including all 180 numbered requirements, is AEOS-PRD's subject.

---

## 5. How AEOS Is Structured — The Architecture

AEOS-ARCH defines the structure through which AEOS-PRD's obligations are met. The structure is eight
layers — six internal, realized by AEOS, and two external, named because the product's guarantees are
statements about what may cross their boundaries:

| # | Layer | Kind | Single responsibility |
| :--- | :--- | :--- | :--- |
| 1 | Human Layer | External | Deciding. |
| 2 | Workflow Layer | Internal | Sequencing work and holding every Approval Gate. |
| 3 | Context Layer | Internal | Selecting the minimum sufficient Context for one step, and retaining why. |
| 4 | Runtime Layer | Internal | Orchestrating external Runtimes in runtime-independent terms. |
| 5 | Adapter Layer | Internal | Translating between AEOS and one external Runtime. |
| 6 | Execution Layer | Internal | Observing the Environment and applying approved effects. |
| 7 | Repository Layer | Internal | Holding everything the project carries forward. |
| 8 | External AI Layer | External | Performing inference. |

Dependency moves in one direction only, toward lower abstraction: Human → Workflow → Context →
Runtime → Adapter → Execution → Repository, with the External AI Layer reached only through the
Adapter Layer. The Repository Layer depends on nothing and remains meaningful on its own. Five
structural decisions carry most of the product's obligations:

| Decision | What it makes structurally true |
| :--- | :--- |
| One store of durable meaning | Only the Repository Layer holds what a project carries forward. |
| One point of supervision | The Workflow Layer is the only layer that addresses the Human Layer and initiates a consequential action. |
| One place for runtime knowledge | Knowledge of a Runtime, Vendor, or Model exists only in the Adapter Layer. |
| One place for platform knowledge | Platform differences are absorbed in the Execution Layer. |
| Declaration separated from execution | Workflows, Rules, Skills, Prompts, and adapters are declared as Repository Assets and interpreted by layers that hold no policy of their own. |

AEOS-BLUEPRINT then decomposes six of the internal layers plus the Human Layer's arrangement into
seven Blueprint layers — the buildable arrangement a Specification can be written against:

| Blueprint | Arranges | Corresponding Architecture Layer |
| :--- | :--- | :--- |
| `BP-REP` Repository Blueprint | Custody of durable product meaning. | Repository Layer |
| `BP-WFL` Workflow Blueprint | Orchestration and the single position of supervision. | Workflow Layer |
| `BP-CTX` Context Blueprint | Selection of the minimum sufficient Context. | Context Layer |
| `BP-RUN` Runtime Blueprint | Coordination with external Runtimes in neutral terms. | Runtime Layer |
| `BP-ADP` Adapter Blueprint | Mediation with one external Runtime. | Adapter Layer |
| `BP-EXE` Execution Blueprint | Observation of the Environment and application of approved effects. | Execution Layer |
| `BP-HUM` Human Interaction Blueprint | Supervision as an experience: assembly, collection, custody, and reporting. | Human Layer |

The complete statement of every layer's responsibilities, prohibited responsibilities, dependencies,
invariants, and boundaries is AEOS-ARCH's subject; the complete statement of every subsystem within
each Blueprint layer is AEOS-BLUEPRINT's subject.

---

## 6. The Runtime Layer — Orchestrating External AI

> **A naming caution AEOS-ARCH states explicitly.** The *Runtime Layer* is an internal layer of AEOS
> that contains no Runtime. A Runtime, in AEOS-GLOSSARY's sense, is an external AI system that
> performs inference, and every Runtime belongs to the External AI Layer. The Runtime Layer holds the
> responsibility for orchestrating them — nothing about how AEOS itself executes, which is a Runtime
> *document's* subject, described below.

The Runtime Layer uses the Runtime selection recorded in the project's Profile, matches Engineering
Capability between a Workflow step and the selected Runtime, dispatches runtime-independent requests
to exactly one adapter, reduces available options when a Runtime is unavailable, and attributes
usage. It performs no inference, contains no Runtime and no Model, never overrides the user's
selection, and holds no credential. The Adapter Layer beneath it is the one position in AEOS
permitted to hold knowledge of a particular Runtime, Vendor, or Model — a boundary that is what lets a
Runtime be added, changed, or removed without any Workflow, Rule, Skill, or Prompt changing.

A family of Runtime documents states the observable behavior this layer must exhibit, each governing
one behavior domain and none restating another:

| Document | Behavior domain |
| :--- | :--- |
| `AEOS-RTF` — Runtime Flow Specification | The observable runtime lifecycle: the ordered phases a request passes through from acceptance to completion, failure, or cancellation. |
| `AEOS-RUNTIME-REG` — Runtime Registry | The single, observable record of which Runtimes a project knows about, what each declares itself capable of, and whether each is currently reachable. |
| `AEOS-SPEC-ADP` — Runtime Adapter Specification | The observable contract between the Runtime Blueprint and every adapter mediating one external Runtime. |
| `AEOS-SPEC-NEG` — Runtime Negotiation Specification | How a candidate Runtime, Adapter, and Model combination is determined compatible with a step's declared requirement, before dispatch. |
| `AEOS-CAP` — Runtime Capability Specification | The Engineering Capability vocabulary itself: how a term is identified, grouped, leveled, declared, matched, and retired. |
| `AEOS-SPEC-MDL` — Model Registry Specification | The identity, metadata, classification, declared capability, and lifecycle of a Model as AEOS makes it discoverable. |

Runtime independence is a property AEOS-VISION `V6` fixes as an invariant of purpose and AEOS-PRD
Section 18.4 (`PR-RUN`) states as product requirements. Runtimes AEOS orchestrates fall into
illustrative, non-exclusive categories: commercial AI services, AI-assisted development environments,
open-source models, interoperability standards such as MCP, and extensions supplied by users or third
parties. Being named confers no privilege; being absent implies no exclusion, and AEOS is expected to
outlive the current runtime landscape.

---

## 7. The Workflow Engine — Sequencing and Supervision

AEOS-GLOSSARY reserves the name **Workflow Engine** for a responsibility, not a component, service,
process, or executable:

> The responsibility for executing Workflow declarations incrementally, holding each consequential
> step to its Approval Gate, and maintaining Workflow State across interruption.

AEOS-ARCH's Workflow Layer discharges that responsibility. It is the only layer that addresses the
Human Layer, the only layer that initiates a consequential action, and the only position at which an
Approval Gate stands — so that the strength of a gate is a property of the action, not of whichever
subsystem happened to reach it. AEOS-BLUEPRINT's Workflow Blueprint (`BP-WFL`) then decomposes that
responsibility into named subsystems:

| Subsystem | Single responsibility |
| :--- | :--- |
| Workflow Declaration Reading | Interprets a declared Workflow into its steps, preconditions, gates, and success criteria. |
| Step Sequencing | Advances work one verifiable step at a time. |
| Precondition Evaluation | Determines whether a step may begin. |
| Action Classification | Assigns the Action Class of every effect a step intends. |
| Gate Placement | Determines, from the Action Class, where an Approval Gate stands. |
| Rule Application | Determines which Rules apply to a step and applies them at their declared point. |
| Skill Composition | Determines which Skills apply to a step and composes them at their declared point. |
| Cycle Position Keeping | Holds the position within an active TDD Cycle. |
| Capability Requirement Declaration | States, per step, the Engineering Capability the step requires. |
| Outcome Recording | Determines what a completed, partial, or failed step contributes to Workflow State. |
| Halt Handling | Stops the sequence on a declined proposal or a failed step. |

The Workflow Layer does not contain the Workflows it executes — those are Repository Assets
([Section 9](#9-repository-assets--rules-skills-prompts-workflows-templates-and-profiles)) — holds no
runtime-specific or platform-specific knowledge, does not reach the Adapter Layer or the External AI
Layer directly, and does not apply an effect itself; that belongs to the Execution Layer. The
engineering-lifecycle stage most associated with this layer, *agentic orchestration*, is addressed
next.

---

## 8. Agentic Orchestration — How AEOS Relates to Agents

AEOS-PRD names *agentic orchestration* as one stage of the engineering lifecycle
([Section 14](#14-the-engineering-lifecycle-and-the-interaction-loop)) and one product requirement:

> `PR-WFL-012` — AEOS supports agentic orchestration: multi-step work sequenced across runtimes, with
> each consequential step held to its approval gate.

No frozen AEOS document names a distinct "Agent System" as an architectural layer, a Blueprint layer,
a Runtime document, or a Repository Asset kind. What exists instead is this: AEOS orchestrates
*agentic runtimes* — AI-assisted development environments and comparable systems, one of the
illustrative runtime categories named in [Section 6](#6-the-runtime-layer--orchestrating-external-ai)
— exactly as it orchestrates any other Runtime, through the same Runtime Layer, the same Engineering
Capability matching, and the same Adapter Layer boundary. An agent, in whatever sense a given Runtime
uses that word, is external to AEOS. AEOS never reimplements one, competes with one, or becomes one.

This is a deliberate boundary, not an omission. AEOS-VISION records, as a non-goal considered and
rejected, that AEOS will not become a fully autonomous software factory: automation within AEOS exists
only as an **Automation Grant** — explicit, scoped, recorded, revocable, and never extending to a
destructive action — never as a destination toward which agentic orchestration is heading. Sequencing
multi-step work across runtimes still holds each consequential step to its Approval Gate; agentic
orchestration widens what one Workflow can sequence, not who decides.

---

## 9. Repository Assets — Rules, Skills, Prompts, Workflows, Templates, and Profiles

AEOS-PRD defines a **Repository Asset** as any durable, versioned artifact that forms part of the
product and lives in the repository — durable, versioned, inspectable, consumable by AI runtimes,
portable, and extensible by users without modifying AEOS. The distinguishing test, per AEOS-PRD: if
losing something costs only repeated work, it is Runtime State; if losing it costs product meaning, it
is a Repository Asset. The list of asset kinds is open — new kinds may be introduced without changing
what a Repository Asset is, a point [Section 10](#10-extensibility--extension-points-and-what-aeos-does-not-yet-name)
returns to.

### 9.1 Skills

A **Skill**, per AEOS-GLOSSARY, is a versioned, reusable, runtime-independent packaged engineering
procedure. AEOS-PRD's Skill management capability (`C8`) discovers, versions, composes, and applies
Skills, and reports which Skill was applied and why. Skills are additive — users add, modify, and
remove them without modifying AEOS — so that a team's accumulated know-how survives a change of
vendor. AEOS-GLOSSARY records one caution explicitly: *Skill* must not be used to describe a
runtime-specific feature offered under the same word by a vendor; an AEOS Skill is a project's own
asset, not a feature of whichever Runtime happens to be selected.

### 9.2 The Other Asset Kinds

| Kind | Definition | Governing capability |
| :--- | :--- | :--- |
| Rule | A versioned, scoped engineering constraint applied during generation, review, and refactoring. | `C7` Rule management |
| Prompt | A versioned, parameterized, portable asset composed of deliberately selected context and instruction. | `C9` Prompt management |
| Workflow | A versioned, runtime-independent declaration of engineering steps, preconditions, approval gates, and success criteria. | `C3` Workflow orchestration |
| Template | A reusable starting point for work a project performs repeatedly, authored and owned by the project. | Recorded within `C10` Repository management |
| Profile | The versioned asset describing a project's identity, technology, build and test approach, runtime selection, and applicable rules. | `C2` Project management |

Every Rule has a defined scope and deterministic precedence; a Rule AEOS cannot enforce under the
selected Runtime is reported, never silently ignored. A Prompt remains inspectable before it is sent,
and never carries credentials or user-designated sensitive content — the mechanism
[Section 5](#5-how-aeos-is-structured--the-architecture)'s Context Layer exists to guarantee. A
Workflow executes unchanged across Runtimes; if a Workflow, Rule, Skill, Prompt, or repository must
change when the Runtime changes, runtime independence has been violated. AEOS-supplied workflow
templates for common project archetypes are recorded in AEOS-PRD Appendix A as a recommendation for a
future release, not a current product concept — the Template kind described here is authored and owned
by the project, not supplied by AEOS.

---

## 10. Extensibility — Extension Points, and What AEOS Does Not Yet Name

AEOS-VISION invariant `V10` states that AEOS is extended, not modified. AEOS-ARCH makes that concrete
with six extension points — the complete set of places where the architecture admits addition without
structural change:

| ID | Extension point | Admits | Attaches at |
| :--- | :--- | :--- | :--- |
| `EP-1` | Repository Assets of an existing kind | Further Rules, Skills, Prompts, Workflows, Templates, Profiles, and documents | Repository Layer |
| `EP-2` | Repository Assets of a new kind | A kind of asset the product did not enumerate, carrying the properties every asset carries | Repository Layer |
| `EP-3` | Runtime adapters | Support for an additional Runtime, including categories that do not yet exist | Adapter Layer |
| `EP-4` | Engineering Capability declarations | Further units of work a step may require and an adapter may offer | Declared by Workflow assets and by adapters |
| `EP-5` | Tool integrations | Orchestration of an additional Tool the project already uses | Execution Layer |
| `EP-6` | Entry surfaces | Further means by which the Human Layer reaches the Workflow Layer | Workflow Layer, supplied by a Distribution Method |

Two terms sometimes asked about in this context — **Hooks** and **Commands** — appear in no frozen or
in-progress AEOS document as an architectural layer, a Blueprint subsystem, a Runtime document, or a
Repository Asset kind. This document does not define them, for the same reason AEOS-BOOT and
AEOS-LAYOUT record explicit non-goals rather than naming a directory or a mechanism no governing
document has yet authorized: a gap filled by invention here would read as settled and would not be.

What exists instead is the extension model above. A mechanism that triggers project behavior at a
declared point most likely takes the shape of a Rule or a Skill applied at a Workflow step's
declared application point (`EP-1`), a Tool integration at the Execution Layer (`EP-5`), or — if it
performs inference — a Runtime adapter (`EP-3`). A means of invoking AEOS from outside a running
Workflow is an entry surface (`EP-6`), supplied by a Distribution Method
([Section 15](#15-distribution)) and named there, not by a project-facing command vocabulary AEOS
itself defines. A concept that fits none of the six points is a proposal to change what AEOS owns, and
belongs in AEOS-PRD's Appendix A of recommendations for future releases, decided only by the owner —
not settled by naming it here.

---

## 11. The Technology Catalog — Frameworks and Recognized Technologies

AEOS-VISION and AEOS-PRD both state, as a considered non-goal rather than an oversight, that AEOS will
not become an application framework: it operates alongside whatever editing surface a developer
prefers and produces no framework the user's application must be built on. AEOS-PRD Appendix A records
that even AEOS-supplied workflow templates are deferred partly because shipping them risks "becoming a
framework layer, which conflicts with product positioning." AEOS is not, and does not intend to
become, a Framework.

*Framework*, in AEOS's own vocabulary, names one category within AEOS-TECH's Technology Catalog — the
frameworks a *user's project* uses, which AEOS-TECH recognizes, documents, and tests its own work
against:

| Category | Example scope |
| :--- | :--- |
| `TC-01` Operating Systems | Windows, macOS, Linux — the Platforms AEOS itself runs on. |
| `TC-03` Programming Languages | Languages a project is written in. |
| `TC-04` Frameworks | Application frameworks a project depends on. |
| `TC-09` AI Providers | Vendors supplying commercial Runtimes. |
| `TC-13` MCP and Interoperability Standards | Standards by which AEOS and a Runtime interoperate. |

AEOS-TECH draws a firm line between neutrality and official support: no Vendor, Runtime, Model,
Platform, or Distribution Method is privileged or required, and a technology's absence never disables
AEOS — it only reduces the options available to the user. Official support is a maintenance
commitment, not an endorsement: a supported technology is one AEOS-TECH has tested, documented, and
taken ownership of. Being named confers no privilege; being unnamed implies no exclusion. The complete
category set, the initial official technology list, and the evaluation and lifecycle process by which
a technology enters or leaves it are AEOS-TECH's subject.

---

## 12. The Documentation Hierarchy

AEOS-DOCSTD assigns documentation **authority**, not merely reading order. A document must not
contradict a document above it in the hierarchy, and every derivative document traces to the layer
above it and ultimately to a `PR-` requirement identifier:

```text
    AEOS-VISION            Why the product exists
          |
          v
    AEOS-PRD               What the product is and must do
          |
          v
    AEOS-GLOSSARY          What the terms mean
          |
          v
    AEOS-DOCSTD            How documentation is written
          |
          v
    AEOS-TECH              What technologies the project recognizes
          |
          v
    ARCHITECTURE           How the product is structured
          |
          v
    BLUEPRINT               How the structure is arranged to be built
          |
          v
    SPECIFICATION          How each behavior must work, precisely
          |
          v
    IMPLEMENTATION GUIDES  How the specified behavior is realized
          |
          v
    DEVELOPER GUIDES       How a person works within the result
```

The Runtime layer AEOS-PRD names alongside Product, Architecture, and Specification has not yet been
assigned a position in this hierarchy; AEOS-DOCSTD Section 4.5 reserves that decision to the owner.
Until it is made, every Runtime document — the six named in
[Section 6](#6-the-runtime-layer--orchestrating-external-ai) — is written to the responsibility
boundary AEOS-PRD states for that layer and complies with every AEOS-DOCSTD rule that does not itself
depend on hierarchy position. This document, `REPOSITORY_LAYOUT.md`, and `PROJECT_BOOTSTRAP.md` each
record the same reservation for their own comparable position.

Twelve documentation principles bind every AEOS document; four recur throughout this one and are worth
naming directly: one document owns one responsibility (`DS-P-06`); a term is defined in exactly one
place, the Glossary, and never restated (`DS-P-07`); at any moment exactly one document is
authoritative for a given subject, and a conflict is reported rather than locally resolved
(`DS-P-08`); and an incomplete document — one with a placeholder, a `TODO`, or an unfinished section —
is not published (`DS-P-10`). The complete principle set, the responsibility boundary of every layer,
the normative-language rules, and the review-to-freeze lifecycle are AEOS-DOCSTD's subject.

---

## 13. Repository Organization

Two documents together state the shape of the AEOS repository. `PROJECT_BOOTSTRAP.md` (AEOS-BOOT)
states, once and normatively, the ordered procedure that produces a conformant repository from
nothing — including the `docs/` subtree's seven layer directories. `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT)
states the shape that procedure produces, independent of the procedure itself, deferring to AEOS-BOOT
wherever the two describe the same ground:

| Top-level entry | Purpose | Governing document |
| :--- | :--- | :--- |
| `docs/` | Houses every AEOS document, organized by documentation-hierarchy layer. | AEOS-DOCSTD (hierarchy) and AEOS-BOOT (subtree structure) |
| `README.md` | Repository-root orientation for a first-time reader. | AEOS-BOOT `BOOT-009` |
| Version-control ignore file | Excludes generated and transient content, where the Distribution Method depends on version control. | AEOS-BOOT `BOOT-010` |
| *(reserved)* source location | Will house AEOS's own source code, once written. | AEOS-LAYOUT, placement principles only — concrete name recorded as `NG-1` |
| *(reserved)* non-Document Repository Assets | Will house Rules, Skills, Prompts, Templates, Workflows, and Profiles for the AEOS repository itself. | AEOS-LAYOUT, placement principles only — concrete name recorded as `NG-2` |

Within `docs/`, seven subdirectories house the documentation hierarchy's layers:
`docs/foundation/` · `docs/architecture/` · `docs/product/` · `docs/specification/` ·
`docs/runtime/` · `docs/implementation/` · `docs/developer/`. A Document is placed at the path its own
metadata declares — discovery-first placement, never a fixed inventory maintained by another document
— per AEOS-BOOT rules `BOOT-004` and `BOOT-007`. `docs/developer/` is reserved for a Developer Guide,
none of which has yet been authored for AEOS. This document is placed directly under `docs/`, outside
every layer subdirectory, for the reason its authority statement gives.

Where AEOS-LAYOUT records a placement decision it does not yet make — the concrete location of
AEOS's own source code, test code, and non-Document Repository Assets — it records the gap as a
non-goal rather than filling it with a guess, and this document does not fill it either.
[Appendix A](#appendix-a--document-map-non-normative) lists every document's own suggested path as
recorded in its own metadata.

---

## 14. The Engineering Lifecycle and the Interaction Loop

AEOS-PRD Section 9 states that AEOS orchestrates the complete engineering lifecycle. Stages are ordered
for readability; real projects re-enter them continuously.

| # | Stage | What AEOS does |
| :--- | :--- | :--- |
| 1 | Environment preparation | Inspects the machine, reports what exists, proposes only the missing or misaligned pieces. |
| 2 | Project initialization | Establishes or adopts a project without overwriting existing work. |
| 3 | Requirement analysis | Turns intent into stated, traceable requirements. |
| 4 | Architecture | Captures structure, boundaries, and decisions as durable repository artifacts. |
| 5 | TDD | Drives the test-first cycle; refuses to advance without a failing test. |
| 6 | Agentic orchestration | Sequences multi-step work across runtimes, holding each step to its approval gate. |
| 7 | Code generation | Delegates generation to the selected runtime with minimized, deliberate context. |
| 8 | Review | Evaluates changes against requirements, rules, and tests, classified by severity. |
| 9 | Refactoring | Improves structure under a green test suite. |
| 10 | Testing | Runs the project's own test tooling and blocks progress on failure. |
| 11 | Documentation | Generates and maintains documentation from the repository's actual state. |
| 12 | Deployment | Orchestrates the project's existing delivery pipelines; never replaces them. |
| 13 | Maintenance | Supports drift detection, dependency and documentation currency, and incremental improvement. |

Every consequential action within that lifecycle follows one loop, without exception:

```text
INSPECT → EXPLAIN → PROPOSE → CONFIRM → EXECUTE → REPORT
```

Inspection determines actual state before intent is formed. Explanation states what was found, in
language a human can act on. A Proposal states the intended action, its rationale, its effects, its
reversibility, and the consequence of declining. Confirmation waits for explicit human approval —
silence is not approval, and a prior approval for a different action is not approval either. Execution
performs exactly what was approved. Reporting states what actually happened, including partial
completion or failure, and records it in the repository.

Actions are classified by effect, and the classification determines the approval required:

| Class | Definition | Approval |
| :--- | :--- | :--- |
| Observation | Reads state; changes nothing. | None required. |
| Local change | Changes state reversible within the repository. | Explicit approval of the proposal. |
| External effect | Reaches outside the repository or the machine. | Explicit approval, with cost and scope stated. |
| Destructive | Loses information or is not reversible by AEOS. | Explicit, specific confirmation of that exact action. |

An **Automation Grant** delegates the human's authority deliberately — explicit, scoped, recorded,
revocable, and never covering a destructive action. The complete statement of the interaction model,
including how a grant is issued and withdrawn, is AEOS-PRD Section 10's subject.

---

## 15. Distribution

AEOS-PRD states that the product architecture is identical regardless of installation method. Four
distribution methods are official at minimum, with three more planned:

| Method | Primary user | Status |
| :--- | :--- | :--- |
| GitHub Clone | Contributors, teams standardizing on a pinned revision | Official |
| Native Installer | Developers who want a supported, updatable install | Official |
| MCP Distribution | Users who work primarily inside an MCP-capable AI runtime or IDE | Official |
| Portable Distribution | Locked-down, air-gapped, or ephemeral machines | Official |
| Package Managers | Native per-platform ecosystem installation | Planned |
| Docker Images | Reproducible, isolated environments for CI | Planned |
| IDE Marketplace Distribution | Discovery and installation from the editing surface | Planned |

Five invariants hold regardless of method: every method delivers the same product architecture; no
capability is exclusive to a method; a project is portable across methods without modification;
installation method is a deployment detail, never a semantic difference; and every method reports its
version and origin. Windows, macOS, and Linux are equal citizens — a capability that works on only one
Platform is treated as incomplete, never as shipped. A host machine satisfies AEOS-ENVSETUP's
prerequisites before AEOS-BOOT's reproducible, deterministic, platform-neutral initialization
procedure is attempted; neither procedure is restated here.

---

## 16. Future Roadmap

AEOS-PRD organizes capability maturity into four phases. Dates are set by the owner and are not part
of the product definition.

| Phase | Name | Goal |
| :--- | :--- | :--- |
| 1 | Foundation | A trustworthy core: inspect, explain, propose, confirm, execute — on all three Platforms, with TDD enforced and at least one Runtime integrated. |
| 2 | Orchestration | Full workflow orchestration, agentic sequencing, Skills, capability matching, and the four minimum distribution methods. |
| 3 | Lifecycle Completion | Remaining lifecycle stages: deployment, maintenance, drift detection, multi-runtime orchestration, CI/CD integration. |
| 4 | Ecosystem | Broader distribution reach: package manager, Docker, and IDE marketplace distributions. |

No phase may relax a product principle, ship a capability on a subset of supported Platforms,
introduce a distribution-exclusive capability, or weaken an Approval Gate to meet a date.

AEOS-PRD Appendix A separately records six recommendations considered for future releases, under the
architecture freeze — **none are part of the current product definition**, and each requires an
explicit owner revision request before adoption:

| # | Recommendation | Why deferred |
| :--- | :--- | :--- |
| `R1` | Shareable rule and skill collections distributable between organizations. | Introduces a distribution concept beyond the current asset model. |
| `R2` | Team-level policy governing which automation grants individual developers may issue. | Adds an authority tier above the project owner. |
| `R3` | Measured context-effectiveness feedback to inform prompt composition over time. | Introduces a measurement and feedback concept not present today. |
| `R4` | AEOS-supplied workflow templates for common project archetypes. | Risks becoming a framework layer, in tension with product positioning. |
| `R5` | Cross-project analytics on discipline and supervision metrics. | Raises privacy and cross-project scope questions. |
| `R6` | Runtime capability benchmarking to advise runtime selection. | Risks appearing to rank vendors, in tension with Vendor Independence. |

---

## 17. How the Pieces Connect

The sections above can be read independently, but they describe one chain. Tracing a single step of
engineering work through it makes the connection concrete.

| Layer or document | What happens to this one step |
| :--- | :--- |
| AEOS-VISION `V2` | The human is the party who must ultimately decide whether this step proceeds. |
| Workflow Layer / Workflow Engine | Interprets the step from a Workflow declaration, assigns it an Action Class, and places its Approval Gate. |
| Rule and Skill application | Applicable Rules constrain the step; applicable Skills, if any, are composed into it — both reported, not silent. |
| Context Layer / Context Router | Selects the minimum sufficient Context for the step and retains why each element was included. |
| Runtime Layer | Matches the step's declared Engineering Capability against the Runtime selected in the project's Profile. |
| Adapter Layer | Translates the request into that Runtime's terms and the result back into AEOS terms. |
| External AI Layer | Performs the inference. AEOS authorizes nothing on the basis of its output alone. |
| Human Layer | Receives the Proposal — rationale, effect, reversibility, and the cost of declining — and approves, declines, or grants automation. |
| Execution Layer | Applies exactly the approved effect, and only that effect, to the Environment and the Repository. |
| Repository Layer | Holds the result, the Workflow State, and the decision record as durable Repository Assets. |

Every row is owned by a document named earlier in this Overview, and no row restates what that
document already states more precisely. This is the whole of what "connecting" AEOS's concepts means
here: not a new account of how the product works, but a single path through the accounts that already
exist, so that a reader who wants to go deeper on any row already knows exactly which document to
open next — [Appendix A](#appendix-a--document-map-non-normative) names it directly.

---

## 18. Document Governance

### 18.1 Status

This document is a non-normative orientation artifact. It is not a Source of Truth for any subject —
AEOS-GLOSSARY's registered Sources of Truth remain exactly as that document states them — and it must
not be referenced as authoritative for a claim any summarized document could instead be cited for.

### 18.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Correction of a broken reference, an inaccurate summary, or an out-of-date version or status in [Appendix A](#appendix-a--document-map-non-normative) | Owner approval | Patch |
| Addition of a section summarizing a documentation-hierarchy layer or Runtime document not yet covered | Owner approval | Minor |
| Reordering of sections, or a change to which document a subject is attributed | Explicit owner revision request | Major |
| Assignment of a documentation-hierarchy position to this document | Explicit owner revision request, per AEOS-DOCSTD `H5` | Major |

### 18.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. An idea surfaced while reading it that would change what AEOS owns belongs in AEOS-PRD
Appendix A, decided only after explicit owner approval — never enacted here.

### 18.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms, before recommending freeze:

- Every factual claim traces to, and correctly restates the direction of authority of, a named
  frozen or in-progress AEOS document.
- No statement introduces a product requirement, an architectural decision, a Blueprint arrangement,
  a specified behavior, a runtime lifecycle, or a terminology definition not already stated elsewhere.
- Every term this document states has no current AEOS definition — at the time of writing, an Agent
  System as a distinct component, a Hook, a Command, and AEOS itself as a Framework — remains
  correctly described as undefined, and is not quietly given a definition here.
- [Appendix A](#appendix-a--document-map-non-normative)'s Document IDs, versions, statuses, and paths
  match each summarized document's own metadata block exactly.
- No Critical or Major finding remains open.

### 18.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with any document it summarizes | The summarized document governs. The conflict is a defect in this document and is reported. |
| This document's Appendix A falls out of date with a summarized document's own metadata | The summarized document's metadata governs. |
| A future Developer Guide restates a concept this document also summarizes | Both stand; the Developer Guide governs day-to-day practice and this document governs orientation, consistent with AEOS-DOCSTD's derivation chain. |
| This document names a concept — an Agent System, a Hook, a Command, or a Framework AEOS itself provides — as though AEOS defined it | That statement is a defect in this document, reported and corrected to match [Sections 8](#8-agentic-orchestration--how-aeos-relates-to-agents), [10](#10-extensibility--extension-points-and-what-aeos-does-not-yet-name), and [11](#11-the-technology-catalog--frameworks-and-recognized-technologies). |

### 18.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Overview. Connects AEOS-VISION, AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-DOCSTD, AEOS-TECH, the six Runtime documents (AEOS-RTF, AEOS-RUNTIME-REG, AEOS-SPEC-ADP, AEOS-SPEC-NEG, AEOS-CAP, AEOS-SPEC-MDL), AEOS-BOOT, and AEOS-LAYOUT into one conceptual map spanning vision, product scope, architecture, the Runtime Layer, the Workflow Engine, agentic orchestration, Repository Assets, extensibility, the Technology Catalog, the documentation hierarchy, repository organization, the engineering lifecycle, distribution, and the roadmap. States plainly that no frozen AEOS document defines a distinct Agent System component, a Hook, a Command, or AEOS itself as a Framework, and maps each to the nearest concept AEOS does define rather than inventing one. Introduces no requirement, terminology, architecture, or specified behavior of its own. |

---

## Appendix A — Document Map (Non-Normative)

**This appendix is non-normative.** It restates each summarized document's own metadata for quick
relocation; the document's own metadata block governs if the two ever diverge.

| Layer | Document | ID | Version | Status | Suggested path |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Foundation | Vision Document | `AEOS-VISION` | 1.0.1 | Frozen | `docs/foundation/VISION.md` |
| Foundation | Product Requirements Document | `AEOS-PRD` | 1.2.0 | Freeze candidate | `docs/foundation/PRD.md` |
| Foundation | Glossary | `AEOS-GLOSSARY` | 1.0.1 | Frozen | `docs/foundation/GLOSSARY.md` |
| Foundation | Document Standard | `AEOS-DOCSTD` | 3.0.0 | Frozen | `docs/foundation/DOCUMENT_STANDARD.md` |
| Foundation | Supported Technologies | `AEOS-TECH` | 1.0.1 | Frozen | `docs/foundation/SUPPORTED_TECHNOLOGIES.md` |
| Architecture | Architecture | `AEOS-ARCH` | 1.1.0 | Frozen | `docs/architecture/ARCHITECTURE.md` |
| Architecture | Blueprint | `AEOS-BLUEPRINT` | 1.1.0 | Frozen | `docs/architecture/BLUEPRINT.md` |
| Product | Specification Standard | `AEOS-SPECSTD` | 1.1.0 | Frozen | `docs/product/SPECIFICATION_STANDARD.md` |
| Runtime | Runtime Flow Specification | `AEOS-RTF` | 1.0.0 | Freeze candidate | `docs/runtime/RUNTIME_FLOW.md` |
| Runtime | Runtime Registry | `AEOS-RUNTIME-REG` | 1.0.0 | Freeze candidate | `docs/runtime/RUNTIME_REGISTRY.md` |
| Runtime | Runtime Capability Specification | `AEOS-CAP` | 1.0.0 | Freeze candidate | `docs/runtime/RUNTIME_CAPABILITY_SPEC.md` |
| Specification | Runtime Adapter Specification | `AEOS-SPEC-ADP` | 1.0.0 | Draft | `docs/specification/RUNTIME_ADAPTER_SPEC.md` |
| Specification | Runtime Negotiation Specification | `AEOS-SPEC-NEG` | 1.0.0 | Draft | `docs/specification/RUNTIME_NEGOTIATION_SPEC.md` |
| Specification | Model Registry Specification | `AEOS-SPEC-MDL` | 1.0.0 | Freeze candidate | `docs/specification/MODEL_REGISTRY.md` |
| Implementation | Project Bootstrap Guide | `AEOS-BOOT` | 3.0.0 | Draft | `docs/implementation/PROJECT_BOOTSTRAP.md` |
| Implementation | Environment Setup Guide | `AEOS-ENVSETUP` | 1.0.0 | Draft | `docs/implementation/ENVIRONMENT_SETUP.md` |
| *(reserved position)* | Repository Layout Guide | `AEOS-LAYOUT` | 1.0.0 | Draft | `docs/REPOSITORY_LAYOUT.md` |
| *(reserved position)* | Overview *(this document)* | `AEOS-OVERVIEW` | 1.0.0 | Draft | `docs/OVERVIEW.md` |

No Developer Guide has yet been authored for AEOS; `docs/developer/` remains reserved for one.

---

**End of Overview**

AEOS-OVERVIEW · Version 1.0.0 · Non-normative orientation — every statement traces to the document
that governs it
