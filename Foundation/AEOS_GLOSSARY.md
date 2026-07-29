# AI Engineering Operating System

## AEOS — Glossary

*The permanent terminology source of truth for AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | Glossary |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-GLOSSARY |
| **Version** | 1.0.1 |
| **Status** | Freeze candidate — awaiting owner approval |
| **Owner** | Product Owner, AEOS |
| **Author** | Chief Information Architect and Terminology Authority, AEOS |
| **Audience** | Architects, contributors, maintainers, documentation authors, and AI runtimes consuming this repository |
| **Suggested path** | `docs/product/GLOSSARY.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) |
| **Supersedes** | AEOS-GLOSSARY 1.0.0 |

> **Authority of this document.**
> This document defines *how AEOS speaks*. It establishes the official name and canonical definition
> of every term the product uses, the conventions by which new names are formed, and the rules by
> which terminology changes.
>
> It defines no requirement, no capability, no architecture, no workflow, and no runtime behavior.
> Where this document and AEOS-PRD address the same term, AEOS-PRD governs the meaning and this
> document records it. Where this document and AEOS-VISION address the same term, AEOS-VISION governs
> the intent and this document records it. A definition here that appears to grant capability, impose
> a requirement, or decide a structure is a defect in this document and MUST be reported rather than
> acted upon.
>
> Every future AEOS document MUST use these definitions and MUST reference this glossary instead of
> restating them.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Terminology Principles](#3-terminology-principles)
4. [Core Terminology](#4-core-terminology)
5. [Terminology Relationships](#5-terminology-relationships)
6. [Naming Conventions](#6-naming-conventions)
7. [Reserved Terms](#7-reserved-terms)
8. [Deprecated Terminology](#8-deprecated-terminology)
9. [Document Governance](#9-document-governance)
10. [Appendix A — Quick Reference Table](#appendix-a--quick-reference-table)

---

## 1. Executive Summary

AEOS is a product whose primary artifact is a repository read by two kinds of maintainer: people who
will arrive years after the original participants have left, and AI runtimes that hold no knowledge
of the project beyond what the repository states. Both read the same words. Neither can ask a
question of someone who is no longer present.

A term used in two senses is therefore not a stylistic blemish. It is a defect with consequences:

| Where inconsistency lands | What it costs |
| :--- | :--- |
| **Traceability** | AEOS-PRD requires every architectural decision, specification, and test to trace to a requirement identifier. A trace is only verifiable if both ends use the same vocabulary. Two names for one concept break the chain silently. |
| **Review** | A review classifies findings against a stated standard. Where the standard's words are ambiguous, reviewers argue about meaning instead of about the change, and the argument is settled by whoever is most senior rather than by what was written. |
| **AI consumption** | An unstated convention is not a convention to a runtime; it is a guess with a plausible tone. Ambiguous terminology produces confidently wrong work at machine speed. |
| **Layer separation** | AEOS keeps Product, Architecture, Specification, Runtime, and Implementation strictly separated. That separation is enforced through words. When a product term drifts into architectural meaning, the boundary is crossed before anyone notices it was approached. |
| **Freeze** | A frozen document is only as stable as the meaning of its sentences. If definitions drift underneath it, the document changes without a version, an approval, or a record. |

This glossary exists so that those failures cannot occur quietly. It fixes one official name and one
canonical definition per concept, states which document holds authority over each, and defines the
conventions by which future names are formed. It is deliberately narrow: it standardizes language and
nothing else.

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the language of AEOS:

- the official name of each AEOS concept;
- the canonical definition of each official name;
- the relationships between official names;
- the conventions by which document names, asset names, and identifiers are formed;
- the words that carry a fixed meaning inside AEOS;
- the process by which terminology changes.

### 2.2 What This Document Does Not Govern

This document defines no requirement, no capability, no architecture, no workflow, and no runtime
behavior. It records which document holds authority over the meaning of each term; it does not
acquire ownership of a subject by recording it. The registered sources of truth are listed in the
Source of Truth entry in [Section 4](#4-core-terminology) and are not restated here.

A statement in this document that grants a capability, imposes a requirement, or decides a structure
is a defect in this document. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to every AEOS document at every layer, and binds human and AI authors
identically. An author who needs a term this document does not define proposes its addition here
under [Section 9](#9-document-governance) rather than defining it locally.

### 2.4 Recorded Deviation

AEOS-DOCSTD Section 7.3 states that the Glossary layer SHOULD NOT use normative keywords, on the
ground that a definition states meaning rather than obligation. This document uses normative keywords
in its principles, naming conventions, reserved terms, and change-management rules, which govern how
terminology is used rather than what a term means, and in entry notes that record a constraint on a
term's use. The deviation is deliberate: removing the keywords would leave those rules unenforceable
and would change what this document requires. It is recorded here as AEOS-DOCSTD Section 7.2 requires
of a deliberate deviation from a SHOULD.

---

## 3. Terminology Principles

These principles govern all AEOS terminology, in this document and in every document that follows it.

| # | Principle | Rule |
| :--- | :--- | :--- |
| T1 | **One concept, one official name.** | Each concept MUST have exactly one official name. Documents MUST use that name and MUST NOT introduce an alternative for the same concept. |
| T2 | **One official definition.** | Each official name MUST have exactly one canonical definition, recorded in [Core Terminology](#4-core-terminology). No document MAY state a competing definition. |
| T3 | **No unregistered synonyms.** | Synonymous engineering terms MUST NOT be used interchangeably. Where an alternative wording is genuinely required, it MUST be registered in this glossary as a synonym of the official name, or it MUST NOT be used. |
| T4 | **Definitions are stable across versions.** | A published definition MUST NOT be changed in place to mean something different. Meaning changes proceed by deprecation and replacement under [Section 7](#8-deprecated-terminology). |
| T5 | **Reference, do not redefine.** | Future documents MUST reference this glossary for terminology. A document that needs a term this glossary does not define MUST propose its addition here rather than defining it locally. |
| T6 | **Definitions are implementation-independent.** | A definition MUST describe what a concept *is*, never how it is built, stored, transported, or executed. A definition that can be satisfied in only one way is a defect in this document. |
| T7 | **Definitions are vendor-neutral.** | No definition MAY be expressed in terms of a specific vendor, runtime, model, platform, distribution method, language, or product. Vendor names MAY appear only as illustration, and confer nothing. |
| T8 | **Authority is recorded, not assumed.** | Every entry MUST state which document holds authority over its meaning. This glossary records definitions owned elsewhere; it does not acquire ownership of them by recording them. |
| T9 | **A name confers nothing.** | Defining a term neither creates a capability nor authorizes behavior. Product obligation arises only from AEOS-PRD requirement identifiers. |
| T10 | **Write for both audiences.** | Every definition MUST be intelligible to a human maintainer and unambiguous to an AI runtime, from the same text. A definition that requires tone, context, or prior acquaintance to disambiguate is incomplete. |

---

## 4. Core Terminology

### 4.1 How to Read an Entry

Each entry states a **Definition**, a **Purpose**, an **Authority**, its **Related terms**, and where
useful, **Notes** recording distinctions that are commonly gotten wrong.

The **Authority** field is the load-bearing one. It states which document governs the meaning:

| Authority value | Meaning |
| :--- | :--- |
| **AEOS-PRD** | The meaning is governed by the Product Requirements Document. This glossary records it and MUST NOT alter it. Product obligation attaches through `PR-` identifiers. |
| **AEOS-VISION** | The meaning is governed by the Vision Document. It expresses intent and confers no product obligation. |
| **AEOS-GLOSSARY** | This document is the defining authority. The term is terminology only: it names something the other documents use without naming, and confers no capability. |
| **AEOS-GLOSSARY (reserved for architecture)** | This document reserves the *name* and fixes the concept it refers to, so that architecture documents cannot introduce it under a competing label. The normative definition — whether the thing exists, what form it takes, and how it behaves — belongs to architecture documents and is not decided here. Such a term MUST NOT be cited to justify behavior, and MUST NOT appear in product-layer documents as though it carried obligation. |

Entries are ordered alphabetically. Related terms are given as official names and are all defined in
this section.

---

### 4.2 Terms

#### Action Class

**Definition.** The classification of an action by its effect — observation, local change, external
effect, or destructive — determining the approval required before it may be executed.

**Purpose.** Makes the strength of an Approval Gate a property of the action rather than a matter of
judgment at the moment of asking, so that identical actions are always gated identically.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Automation Grant · Human Approval · Proposal · Workflow.

**Notes.** The four classes are a closed set. A document that needs a fifth is proposing a change to
the product's concepts and MUST route the idea through owner approval rather than naming it.

#### AEOS

**Definition.** AI Engineering Operating System. The product defined by AEOS-PRD: an operating system
for AI-assisted, human-supervised software engineering that orchestrates external AI runtimes and
performs no inference of its own.

**Purpose.** Names the product as a whole, distinct from any repository, distribution, runtime, or
organization associated with it.

**Authority.** AEOS-PRD.

**Related terms.** Product · Repository · Runtime · Vision.

**Notes.** The name is always written in full uppercase. The expansion is *AI Engineering Operating
System*, without hyphenation. The operating-system element of the name is a metaphor for the
orchestration of engineering activity and nothing below that level; it MUST NOT be read as a claim
about processes, hardware, or the host operating system.

#### Approval Gate

**Definition.** The point at which a proposed action requires explicit human confirmation before
execution.

**Purpose.** Names the structural position of supervision within a workflow, independently of how any
particular confirmation is obtained.

**Authority.** AEOS-PRD.

**Related terms.** Action Class · Human Approval · Human-in-the-Loop · Proposal · Workflow.

**Notes.** An Approval Gate is a position; Human Approval is the act that satisfies it. The two MUST
NOT be used interchangeably.

#### Architecture

**Definition.** The layer that determines how AEOS is structured so that it can deliver the Product,
together with the documents that record those determinations.

**Purpose.** Names the layer between Product and Specification, and the body of decisions owned there.

**Authority.** AEOS-PRD.

**Related terms.** Blueprint · Implementation · Product · Specification · Freeze.

**Notes.** Capitalized *Architecture* always refers to the architecture of AEOS itself. The structure
of a user's software is *project architecture* and MUST be written that way; the Repository Asset kind
that records it is an *Architecture Document*. Architecture decides *how*; it never decides *whether*.

#### Automation Grant

**Definition.** An explicit, scoped, recorded, revocable delegation of approval authority for specific
action classes.

**Purpose.** Names the only mechanism by which supervision may be relaxed, so that relaxation is
always a decision someone made rather than a default that arrived.

**Authority.** AEOS-PRD.

**Related terms.** Action Class · Approval Gate · Human Approval · Human-in-the-Loop · Repository Asset.

**Notes.** An Automation Grant never authorizes destructive actions. *Automation* used without the
word *grant* MUST NOT be read as implying one.

#### Blueprint

**Definition.** The document layer that expresses how a defined portion of AEOS is to be realized,
positioned between Architecture and Specification: more concrete than a structural decision, less
precise than a testable statement of behavior.

**Purpose.** Names the intermediate layer in the AEOS document chain so that architecture documents
and specifications do not absorb each other's responsibilities.

**Authority.** AEOS-GLOSSARY (reserved for architecture).

**Related terms.** Architecture · Document · Implementation · Specification.

**Notes.** AEOS-PRD names no Blueprint layer. The name is reserved here so that the chain
Vision → Product → Architecture → Blueprint → Specification → Implementation has one vocabulary.
Whether Blueprints exist as documents, what they contain, and whether they constitute a Repository
Asset kind are architecture governance decisions and are not made here.

#### Bootstrap

**Definition.** The initial sequence by which AEOS becomes usable in an Environment or a Project for
the first time: inspection of what exists, proposal of what is missing, and execution of only what was
approved.

**Purpose.** Provides one name for the first-run experience, which spans environment preparation and
project initialization or adoption, so that neither term is stretched to cover both.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Environment · Project · Profile · Proposal · Human Approval.

**Notes.** Bootstrap describes a sequence, never a mechanism, an installer, or a script. It carries no
implication of unattended execution: every consequential step within a Bootstrap is subject to its
Action Class and its Approval Gate exactly as any other step is.

#### Capability

**Definition.** One of the ten product capabilities defined in AEOS-PRD that together constitute the
product, each identified as `C1` through `C10` and expressed as numbered product requirements.

**Purpose.** Names the unit in which the product's scope is stated and counted.

**Authority.** AEOS-PRD.

**Related terms.** Engineering Capability · Product · Specification · Workflow.

**Notes.** *Capability* is a reserved term with a fixed, closed referent. It MUST NOT be used to mean
"feature", "function", "module", or "what a runtime can do". For the ability a workflow step requires
and a runtime may or may not provide, use **Engineering Capability**.

#### Context

**Definition.** The information deliberately selected and supplied to a Runtime so that it can perform
a defined step of work, together with the reason each element was included.

**Purpose.** Names what crosses the boundary to an external runtime, so that its size, its content,
and its justification can be discussed as one thing.

**Authority.** AEOS-PRD.

**Related terms.** Context Minimization · Context Router · Prompt · Runtime · Workflow.

**Notes.** *Context* in AEOS always means selected project information. It MUST NOT be used to mean a
model's input window, which is a property of a Model and is written *context window*. Context that
cannot be justified element by element is not minimized, whatever its size.

#### Context Minimization

**Definition.** The principle that AEOS sends the smallest context sufficient for the task and can
explain each inclusion.

**Purpose.** Names the standard against which any context selection is judged.

**Authority.** AEOS-PRD.

**Related terms.** Context · Context Router · Prompt · Runtime.

**Notes.** Minimization is a property of deliberate selection, not of measured size. Bulk transfer is
a failure of design, and unnecessary context is a defect rather than overhead.

#### Context Router

**Definition.** The responsibility for selecting the minimum sufficient Context for a step of work and
for retaining the reason each element was included.

**Purpose.** Gives the selection responsibility one name, so that architecture documents do not each
invent a different one for it.

**Authority.** AEOS-GLOSSARY (reserved for architecture).

**Related terms.** Context · Context Minimization · Prompt · Workflow · Workflow Engine.

**Notes.** The term names a responsibility, not a component, module, process, service, or file. AEOS-PRD
states the product obligation — minimized context, explainable inclusion, inspectable prompt — without
naming anything that performs it. Whether that responsibility is realized as one thing, several, or
none is an architecture decision.

#### Contributor

**Definition.** A person or AI runtime proposing a change to AEOS itself — to its documents, its
assets, or its implementation.

**Purpose.** Distinguishes those who change the product from those who use it, since the two are
governed by different rules.

**Authority.** AEOS-VISION.

**Related terms.** Developer · Review · Freeze · Repository.

**Notes.** Contributors are bound by the guiding principles in AEOS-VISION and by the architecture
freeze. One individual may be both a Contributor and a Developer; the roles remain distinct in review
and governance, and a document MUST state which one it addresses.

#### Developer

**Definition.** A person who uses AEOS to build or maintain their own software project.

**Purpose.** Names the primary user of the product, as distinct from a person who changes the product.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Contributor · Project · Repository · Human Approval.

**Notes.** *Developer* is the general term for a user of AEOS and covers the solo developer,
the team member, the engineering lead, and the platform engineer described in AEOS-PRD. Where a
statement applies to only one of those, that role MUST be named explicitly instead.

#### Distribution Method

**Definition.** An official way AEOS is delivered to users. It never changes the product architecture.

**Purpose.** Separates packaging, discovery, and update mechanics from product meaning.

**Authority.** AEOS-PRD.

**Related terms.** Platform · Product · Repository · Bootstrap.

**Notes.** No capability is exclusive to a Distribution Method, and how AEOS was installed is never a
semantic difference. *Distribution* and *Distribution Method* refer to the same concept; the shorter
form MAY be used where no ambiguity arises.

#### Document

**Definition.** A durable, versioned artifact that states some part of what AEOS or a project knows
about itself, readable by humans and consumable by AI runtimes from the same text.

**Purpose.** Names the artifact kind through which AEOS records intent, requirement, structure,
behavior, and decision.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Repository Asset · Source of Truth · Specification · Freeze · Review.

**Notes.** Every AEOS document carries a Document ID, a version, a status, an owner, and a stated
authority. Documents are Repository Assets and are reviewed, versioned, and maintained like code. A
document containing placeholders or unfinished sections is an unfinished artifact and is not shipped.

#### Engineering Capability

**Definition.** A discrete unit of engineering work that a Workflow step requires in order to proceed,
and that a selected Runtime may or may not be able to perform.

**Purpose.** Makes capability fit expressible: a Workflow declares what it needs, a Runtime offers what
it provides, and the difference can be reported to the user before work begins rather than partway
through.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Capability · Runtime · Runtime Adapter · Workflow · Rule.

**Notes.** Engineering Capability and **Capability** are different terms and MUST NOT be substituted
for one another: a Capability is one of the ten product capabilities; an Engineering Capability is a
unit of work matched between a workflow and a runtime. This term names the concept only; the
obligation to report unsupported work before it begins arises from AEOS-PRD.

#### Environment

**Definition.** The machine on which AEOS operates, together with its platform, its project-relevant
tooling, and the AI runtimes available on it, as observed at a point in time.

**Purpose.** Names what is inspected before any environment-affecting action, and what is reported to
the user as fact.

**Authority.** AEOS-PRD.

**Related terms.** Bootstrap · Platform · Runtime · Tool · Runtime State.

**Notes.** An Environment is described by observation, never by assumption; where it cannot be
determined, the uncertainty is reported as such. The Environment belongs to the user: AEOS is a guest
on it. *Environment inspection* is the act of determining actual machine and project state before
proposing or performing an action.

#### Freeze

**Definition.** The governance state in which a document's definitions and decisions MUST NOT change
except through the change control that document defines, under the owner's approval.

**Purpose.** Makes stability a declared, auditable state rather than an informal expectation.

**Authority.** AEOS-PRD.

**Related terms.** Document · Review · Source of Truth · Architecture.

**Notes.** Freeze is a governance state, not a technical lock. A frozen document still accepts
editorial correction at patch level; what it does not accept is a change of meaning without an owner
decision. Under the architecture freeze, an improvement to the product's concepts is recorded as a
recommendation for a future release rather than applied.

#### Human Approval

**Definition.** The explicit act by which a person authorizes a specific proposed action before it is
executed.

**Purpose.** Names the act that satisfies an Approval Gate, so that the act and the gate can be spoken
about separately.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Action Class · Automation Grant · Human-in-the-Loop · Proposal.

**Notes.** Silence is not Human Approval. Ambiguity is not Human Approval. Approval of a different
action is not Human Approval of this one. Approval authorizes exactly what was proposed; scope
expansion requires a new Proposal.

#### Human-in-the-Loop

**Definition.** The requirement that a human decides before AEOS acts consequentially.

**Purpose.** Names the product's default posture toward supervision.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Automation Grant · Human Approval · Proposal.

**Notes.** Written with hyphens and initial capitals when naming the principle. The abbreviation *HITL*
MUST NOT be used in AEOS documents.

#### Implementation

**Definition.** The code and tests that realize the Architecture, Blueprint, and Specification layers,
traced back to the requirement identifiers they satisfy.

**Purpose.** Names the lowest layer of the AEOS document and delivery chain.

**Authority.** AEOS-PRD.

**Related terms.** Architecture · Blueprint · Specification · Repository · Review.

**Notes.** Capitalized *Implementation* refers to AEOS's own code. Software written by a Developer in
their own project is *project code* and MUST be written that way. *Implementation detail* is out of
scope for product-layer documents.

#### Model

**Definition.** A specific language model, model family, or model version that a Runtime uses to
perform inference.

**Purpose.** Separates the thing that performs inference from the system that exposes it, so that model
choice can be discussed as an ordinary engineering decision.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Runtime · Vendor · Context · Engineering Capability.

**Notes.** A Model is never a Runtime, and neither is ever part of AEOS. Model selection belongs to the
user. AEOS is independent of any model, model family, or model version, and no AEOS behavior may
depend on undocumented behavior of a specific one.

#### Platform

**Definition.** A supported host operating system on which AEOS operates: Windows, macOS, or Linux.

**Purpose.** Names the axis along which AEOS commits to equal capability.

**Authority.** AEOS-PRD.

**Related terms.** Environment · Distribution Method · Product · Tool.

**Notes.** *Platform* MUST NOT be used to mean a vendor's AI service, a distribution channel, a cloud
provider, or an application runtime. The three platforms are equal citizens; a capability that works on
only one of them is incomplete rather than shipped.

#### Product

**Definition.** The whole of what AEOS is and does for its users, as defined by AEOS-PRD; equivalently,
the layer that answers what AEOS is, who it is for, and what it must do for them.

**Purpose.** Names both the thing being built and the topmost layer of definition beneath the Vision.

**Authority.** AEOS-PRD.

**Related terms.** Vision · Architecture · Capability · Repository · Product Boundary.

**Notes.** *Product Boundary* is the line AEOS-PRD draws between product definition and implementation:
what AEOS is belongs to the product layer; how AEOS is built belongs to architecture, specification, and
runtime documents. The repository is the product's durable form, not a place where the product is kept.

#### Product Boundary

**Definition.** The line AEOS-PRD draws between product definition and implementation: what AEOS is
belongs to the product layer, and how AEOS is built belongs to architecture, specification, and runtime
documents.

**Purpose.** Gives reviewers one name for the boundary a document may have crossed, so that the crossing
can be reported as a specific defect rather than as a stylistic objection.

**Authority.** AEOS-PRD.

**Related terms.** Product · Architecture · Specification · Implementation · Document.

**Notes.** A product-layer statement that can be satisfied in only one way has crossed the Product
Boundary. It is reported as a defect in the document that contains it, and is not treated as an
architectural instruction.

#### Profile

**Definition.** The versioned Repository Asset describing a project's identity, technology, build and
test approach, runtime selection, and applicable rules.

**Purpose.** Gives a new session — human or AI — enough recorded knowledge to become useful without the
project being re-explained.

**Authority.** AEOS-PRD.

**Related terms.** Project · Project Type · Technology Stack · Repository Asset · Runtime · Rule.

**Notes.** *Profile* and *Project Profile* are the same term; the short form MAY be used where no
ambiguity arises. AEOS defines no other kind of profile, and the word MUST NOT be used for user
profiles, performance profiling, or runtime configuration.

#### Project

**Definition.** The unit of work AEOS operates on: a repository together with its Repository Assets,
governed by AEOS.

**Purpose.** Names the boundary within which profiles, rules, workflows, and approvals apply.

**Authority.** AEOS-PRD.

**Related terms.** Repository · Repository Asset · Profile · Project Type · Developer.

**Notes.** A Project always belongs to the user, never to AEOS. Adopting an existing project is the
ordinary case rather than the exception, and adoption never overwrites, relocates, or restructures what
was already there. Work on one project never affects another.

#### Project Type

**Definition.** A descriptive classification of a Project, recorded in its Profile, expressing the kind
of software the project builds.

**Purpose.** Lets projects of a similar kind be described consistently, so that documents and assets can
refer to the classification instead of enumerating characteristics.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Profile · Project · Technology Stack · Template.

**Notes.** Project Type is descriptive metadata and grants no behavior: no rule, workflow, or asset is
applied merely because a project carries a given type. AEOS-supplied workflow templates for common
project archetypes are a recorded recommendation for a future release, not a current product concept.

#### Prompt

**Definition.** A versioned, parameterized, portable Repository Asset composed of deliberately selected
context and instruction.

**Purpose.** Treats the text sent to a runtime as an engineering artifact subject to review, versioning,
and reuse, rather than as disposable session material.

**Authority.** AEOS-PRD.

**Related terms.** Context · Context Minimization · Repository Asset · Runtime · Skill.

**Notes.** A Prompt remains inspectable before it is sent, because a prompt the user cannot read is a
decision the user did not make. Credentials and user-designated sensitive content never appear in one.

#### Proposal

**Definition.** A statement of intended action including rationale, effects, reversibility, and the
consequence of declining.

**Purpose.** Names the artifact a human is asked to approve, so that approval always attaches to
something specific and recorded.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Human Approval · Action Class · Review · Workflow.

**Notes.** A declined Proposal is a normal outcome and is never treated as an error. Executing beyond
what a Proposal stated requires a new Proposal, not a broader reading of the old one.

#### Repository

**Definition.** The version-controlled store that contains a project's code and Repository Assets and is
the authoritative source of truth for that project.

**Purpose.** Names the durable form of the product and of every project governed by it.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Runtime State · Project · Source of Truth · Product.

**Notes.** Unqualified, *Repository* means the user's project repository; AEOS's own is *the AEOS
repository*. *Repository as Product* is the principle that the repository is the product and its
authoritative source of truth. What is not in the repository does not exist, and a repository remains
meaningful when AEOS is not running.

#### Repository Asset

**Definition.** Any durable, versioned artifact that forms part of the product and lives in the
repository. Includes rules, skills, prompts, workflows, profiles, templates, playbooks, recipes,
specifications, architecture documents, and manuals.

**Purpose.** Draws the line between what a project carries forward and what merely accompanies its
execution.

**Authority.** AEOS-PRD.

**Related terms.** Repository · Runtime State · Rule · Skill · Prompt · Template · Workflow · Profile · Document.

**Notes.** The list of asset kinds is open: new kinds MAY be introduced without changing what a
Repository Asset is. Every Repository Asset is durable, versioned, inspectable, consumable by AI
runtimes, portable, and extensible by users without modifying AEOS. The distinguishing test is stated
in AEOS-PRD: if losing it costs only repeated work it is Runtime State; if losing it costs product
meaning it is a Repository Asset.

#### Review

**Definition.** The examination of an artifact, change, or document against requirements, rules, and
tests, producing findings classified as Critical, Major, Minor, or Nitpick.

**Purpose.** Names the point at which a stated standard is actually applied, and at which generated work
becomes owned work.

**Authority.** AEOS-PRD.

**Related terms.** Proposal · Rule · Freeze · Document · Contributor.

**Notes.** The four severities are a closed set and MUST NOT be extended, renamed, or reordered. Review
examines a result; explanation before execution concerns an intent. Both are required, and neither
substitutes for the other. A review identifies inconsistencies; it does not redesign the artifact under
review.

#### Rule

**Definition.** A versioned, scoped engineering constraint applied during generation, review, and
refactoring.

**Purpose.** Carries an engineering lead's intent to every developer and every runtime without being
restated.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Review · Skill · Workflow · Profile.

**Notes.** Every Rule has a defined scope, and precedence between overlapping rules is deterministic and
explainable. AEOS never applies a rule the user cannot inspect. A rule that cannot be enforced under the
selected runtime is reported rather than silently ignored.

#### Runtime

**Definition.** An external AI system that performs inference. Always an integration, never a part of
AEOS.

**Purpose.** Names the class of external systems AEOS orchestrates and the boundary they sit behind.

**Authority.** AEOS-PRD.

**Related terms.** Model · Vendor · Runtime Adapter · Engineering Capability · Tool.

**Notes.** *Runtime* in AEOS always means an AI runtime. It MUST NOT be used to mean a language runtime,
an application runtime, or an execution environment; where those are meant, they MUST be written out.
A system that performs no inference is a **Tool**, not a Runtime. Runtime selection belongs to the user
and is never overridden or silently substituted.

#### Runtime Adapter

**Definition.** The responsibility for mediating between AEOS and one external Runtime, so that
workflows, rules, skills, and prompts remain unchanged when the runtime changes.

**Purpose.** Gives the integration responsibility one name, so that runtime independence can be
discussed without naming any particular integration mechanism.

**Authority.** AEOS-GLOSSARY (reserved for architecture).

**Related terms.** Runtime · Engineering Capability · Vendor · Model · Workflow.

**Notes.** The term names a responsibility, not a component, plugin, package, or interface. AEOS-PRD
states the product obligations — no inference, runtime independence, support for a new runtime without
modifying AEOS — without naming anything that discharges them. Whether adapters exist as discrete
artifacts, how they are declared, and how they are distributed are architecture decisions.

#### Runtime State

**Definition.** Transient, machine-local, or environment-specific information produced while AEOS runs.
Not a Repository Asset and not part of the product.

**Purpose.** Names everything that execution produces but the project does not carry forward, so that it
is never mistaken for product knowledge.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Environment · Workflow State · Repository.

**Notes.** Cache, temporary execution state, credentials, telemetry, generated temporary artifacts, and
machine-specific configuration are Runtime State. A project MUST remain fully understandable and
reproducible without it. Runtime State MUST NOT be confused with **Workflow State**, which is durable
and belongs to the repository.

#### Skill

**Definition.** A versioned, reusable, runtime-independent packaged engineering procedure.

**Purpose.** Lets a team's accumulated know-how be applied repeatedly and survive a change of vendor.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Rule · Prompt · Workflow · Template.

**Notes.** Skills are additive: users add, modify, and remove them without modifying AEOS. Which skill
was applied, and why, is reported rather than left implicit. *Skill* MUST NOT be used to describe a
runtime-specific feature offered under the same word by a vendor.

#### Source of Truth

**Definition.** The single artifact that governs a subject: where it conflicts with any other statement
about that subject, it governs and the other statement is a defect.

**Purpose.** Makes conflict resolution mechanical rather than a matter of seniority or recency.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Document · Freeze · Product · Repository · Vision.

**Notes.** The registered sources of truth are:

| Subject | Source of Truth |
| :--- | :--- |
| Why AEOS exists and what it must remain | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| Official terminology and naming | AEOS-GLOSSARY |
| A project's code, assets, and state | The Repository |

A document MUST NOT declare itself a source of truth for a subject already registered above.

#### Specification

**Definition.** A precise statement of required behavior, expressed testably and traceable to one or
more product requirement identifiers; also the document layer in which such statements live.

**Purpose.** Names the layer at which behavior becomes verifiable, between Blueprint and Implementation.

**Authority.** AEOS-PRD.

**Related terms.** Blueprint · Implementation · Document · Repository Asset · Review.

**Notes.** Specifications use RFC 2119 terminology. A specification MUST NOT weaken, reinterpret, or
quietly widen a product requirement; where the two conflict, AEOS-PRD governs.

#### TDD Cycle

**Definition.** Define behavior → failing test → verify failure reason → minimal implementation →
refactor green.

**Purpose.** Names the five-position cycle whose current position is tracked and reported.

**Authority.** AEOS-PRD.

**Related terms.** Workflow · Workflow State · Review · Rule · Tool.

**Notes.** Written *TDD Cycle*, capitalized, when naming the AEOS concept. Skipping it is an explicit
exception a human acknowledges; it is never silent and never the default. The cycle applies to AEOS's
own development without exemption.

#### Technology Stack

**Definition.** The set of languages, frameworks, libraries, build tools, and test tools a Project uses,
recorded descriptively in its Profile.

**Purpose.** Lets a project state what it is built with, so that inspection results and tooling
expectations can be recorded rather than inferred each session.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Profile · Project Type · Tool · Environment.

**Notes.** Descriptive, never prescriptive: AEOS neither supplies a stack nor requires one, and no
capability may depend on a particular technology choice. Recording a technology in a Profile confers no
preference on it.

#### Template

**Definition.** A Repository Asset providing a reusable starting point for work a project performs
repeatedly.

**Purpose.** Lets a project capture its own repeated starting points once instead of restating them.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Skill · Prompt · Project Type · Workflow.

**Notes.** Templates are authored and owned by the project. AEOS-supplied workflow templates for common
project archetypes are a recorded recommendation for a future release and are not part of the current
product definition; the two MUST NOT be conflated.

#### Tool

**Definition.** An external program or system a project depends on that performs no inference: build,
test, version control, packaging, delivery, and comparable systems.

**Purpose.** Separates the systems AEOS orchestrates from the AI runtimes it delegates inference to,
since the two are governed differently.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Runtime · Environment · Platform · Repository · Workflow.

**Notes.** A Tool performs no inference; a Runtime does. AEOS detects tools rather than assuming them,
orchestrates them rather than replacing them, and never removes or reconfigures a tool it did not
install without explicit, specific confirmation.

#### Vendor

**Definition.** An organization that supplies a Runtime, a Model, or a Tool used with AEOS.

**Purpose.** Names the commercial or organizational source of an external dependency, so that
independence from it can be stated precisely.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Runtime · Model · Tool · Distribution Method.

**Notes.** No Vendor is privileged and no Vendor is required; a vendor's absence reduces the runtimes
available to a user and never disables AEOS. Being named in AEOS documentation confers nothing; being
unnamed excludes nothing. Vendor names MUST NOT appear in definitions, requirements, or asset names.

#### Vision

**Definition.** The statement of why AEOS exists, what future it intends to serve, and which convictions
must survive every revision of the product; also the document that records it, AEOS-VISION.

**Purpose.** Names the layer above Product, against which a proposed change can be tested for whether it
serves the product's reason for existing.

**Authority.** AEOS-VISION.

**Related terms.** Product · Architecture · Source of Truth · Freeze.

**Notes.** The Vision governs reasoning; AEOS-PRD governs behavior. A statement in the Vision that
appears to grant capability or impose a requirement is a defect in that document. Its invariants are
identified `V1` through `V10`.

#### Workflow

**Definition.** A versioned, runtime-independent declaration of engineering steps, preconditions,
approval gates, and success criteria.

**Purpose.** Moves a team's engineering practice out of individual habit and into a reviewable,
portable asset.

**Authority.** AEOS-PRD.

**Related terms.** Workflow Engine · Workflow State · Repository Asset · Engineering Capability · Approval Gate.

**Notes.** Workflows execute unchanged across runtimes; if a workflow, rule, skill, prompt, or repository
must change when the runtime changes, runtime independence has been violated. *Agentic orchestration* is
the sequencing of multi-step work across runtimes with each consequential step held to its approval gate.

#### Workflow Engine

**Definition.** The responsibility for executing Workflow declarations incrementally, holding each
consequential step to its Approval Gate, and maintaining Workflow State across interruption.

**Purpose.** Gives the execution responsibility one name, so that architecture documents do not each
invent a different one for it.

**Authority.** AEOS-GLOSSARY (reserved for architecture).

**Related terms.** Workflow · Workflow State · Context Router · Approval Gate · Runtime Adapter.

**Notes.** The term names a responsibility, not a component, service, process, or executable. AEOS-PRD
states the product obligations — incremental execution, gated steps, resumable position, reported
outcomes — without naming anything that performs them. Whether the responsibility is realized as one
thing, several, or none is an architecture decision.

#### Workflow State

**Definition.** The durable record of where a Workflow currently stands: completed steps, the current
step, outstanding decisions, and the position within any active TDD Cycle.

**Purpose.** Names what makes a workflow inspectable mid-flight, safe to interrupt, and resumable
without re-establishing context.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Workflow · Workflow Engine · Runtime State · Repository · TDD Cycle.

**Notes.** Workflow State is project knowledge and belongs to the repository; **Runtime State** is a
consequence of execution and does not. The two MUST NOT be conflated, and Workflow State MUST NOT be
described as transient.

---

## 5. Terminology Relationships

The diagrams below record how the official terms relate. They are terminology maps, not architecture:
they state which concept is defined in terms of which, and nothing about structure, dependency, or
execution.

> **Reading the arrows.** Each chain states one relationship, given beneath it. An arrow never means
> "calls", "contains at runtime", "is implemented by", or "happens after".

### 5.1 The Definition Chain

Each layer is defined by, and must trace back to, the layer above it.

```text
        Vision            why AEOS exists and what it must remain
          |
          v
        Product           what AEOS is and what it must do
          |
          v
        Architecture      how AEOS is structured to deliver the Product
          |
          v
        Blueprint         how a defined portion is to be realized
          |
          v
        Specification     how each behavior must work, precisely and testably
          |
          v
        Implementation    the code and tests that realize it
```

| Relationship | Statement |
| :--- | :--- |
| Vision → Product | The Product is constrained by the Vision. The Vision governs reasoning; the Product governs behavior. |
| Product → Architecture | Architecture decides how the Product is delivered. It decides *how*, never *whether*. |
| Architecture → Blueprint | A Blueprint expresses a defined portion of the Architecture in realizable form. |
| Blueprint → Specification | A Specification states the behavior precisely enough to be tested. |
| Specification → Implementation | Implementation realizes the Specification and traces back to product requirement identifiers. |

### 5.2 The Asset Chain

The Repository holds Repository Assets. Rule, Skill, Prompt, Template, and the other asset kinds are
*kinds of* Repository Asset, not stages of one another.

```text
        Repository
          |
          v
        Repository Asset
          |
          +-- Rule          engineering constraints the project agrees to
          +-- Skill         reusable engineering procedures
          +-- Prompt        deliberate, minimized instruction and context
          +-- Template      reusable starting points for repeated work
          +-- Workflow      engineering sequences and their approval gates
          +-- Profile       what the project is and how it is built
          +-- Document      specifications, architecture documents, manuals
          +-- ...           the list of kinds is open
```

| Relationship | Statement |
| :--- | :--- |
| Repository → Repository Asset | The Repository contains all Repository Assets and is their source of truth. |
| Repository Asset → kinds | Each kind is a Repository Asset and inherits every property of one: durable, versioned, inspectable, consumable, portable, extensible. |
| Repository Asset ↔ Runtime State | Mutually exclusive. Runtime State is never a Repository Asset, and no project requires Runtime State to be understood or reproduced. |

### 5.3 The Work Chain

How a unit of work relates to the context it needs and the runtime that performs it.

```text
        Context ---- selected under ----> Context Minimization
          |
          | supplied to a step of
          v
        Workflow ---- executed under ----> Approval Gate
          |
          | each step requires
          v
        Engineering Capability
          |
          | offered, or not, by
          v
        Runtime ---- performs inference using ----> Model
```

| Relationship | Statement |
| :--- | :--- |
| Context → Workflow | Context is selected per step of a Workflow, minimized deliberately, and explainable element by element. |
| Workflow → Engineering Capability | A Workflow step declares the engineering work it requires. |
| Engineering Capability → Runtime | A Runtime either provides the required work or does not; the difference is reported before work begins. |
| Runtime → Model | A Runtime performs inference using a Model. Neither is part of AEOS. |

### 5.4 The Supervision Chain

How an intended action becomes an executed one.

```text
        Action Class ---- determines the strength of ----> Approval Gate
                                                              ^
        Proposal ---- presented at -------------------------- +
          |
          | satisfied by
          v
        Human Approval ---- may be delegated by ----> Automation Grant
          |                                             (never for destructive actions)
          | authorizes exactly what was proposed
          v
        Execution ---- recorded as ----> Workflow State
```

| Relationship | Statement |
| :--- | :--- |
| Action Class → Approval Gate | The class of an action determines the approval it requires. |
| Proposal → Human Approval | Approval attaches to a specific Proposal and to nothing beyond it. |
| Human Approval → Automation Grant | A grant delegates approval authority explicitly, scoped and revocably, and never for destructive actions. |
| Execution → Workflow State | What actually occurred, including partial completion and failure, is recorded in the project. |

### 5.5 Terms That Are Commonly Confused

| These are different | The distinction |
| :--- | :--- |
| **Capability** vs **Engineering Capability** | One of the ten product capabilities, versus a unit of work matched between a workflow step and a runtime. |
| **Repository Asset** vs **Runtime State** | Losing it costs product meaning, versus losing it costs only repeated work. |
| **Workflow State** vs **Runtime State** | Durable project knowledge in the repository, versus a transient consequence of execution. |
| **Runtime** vs **Tool** | Performs inference, versus performs no inference. |
| **Runtime** vs **Model** | The external system that exposes inference, versus the model it uses to perform it. |
| **Approval Gate** vs **Human Approval** | The position at which confirmation is required, versus the act that satisfies it. |
| **Review** vs **explanation before execution** | Examination of a result, versus the obligation to be understood before acting. Both are required. |
| **Developer** vs **Contributor** | Uses AEOS on their own project, versus changes AEOS itself. |
| **Architecture** vs **project architecture** | The structure of AEOS, versus the structure of a user's software. |
| **Implementation** vs **project code** | AEOS's own code, versus code written by a Developer in their project. |

---

## 6. Naming Conventions

These conventions govern every name introduced in an AEOS repository. They apply to documents, assets,
identifiers, and prose.

### 6.1 General Rules

| # | Rule |
| :--- | :--- |
| N1 | A name MUST describe what a thing is or does, never how it is built. |
| N2 | A name MUST NOT contain a vendor, runtime, model, product, or platform name. |
| N3 | A name MUST NOT encode a version, a date, a status, or an author. |
| N4 | Abbreviations MUST NOT be invented. Only abbreviations registered in this glossary MAY be used; at present the registered set is `AEOS`, `PRD`, `TDD`, `CI/CD`, and `MCP`. |
| N5 | Names SHOULD be stable. Renaming a published thing follows the deprecation process in [Section 7](#8-deprecated-terminology). |
| N6 | English MUST be the language of all names and identifiers. |

### 6.2 Document Names

| Aspect | Convention |
| :--- | :--- |
| **File name** | Product-level AEOS documents MUST be named `AEOS_<NAME>.md`, uppercase, words separated by underscores (for example `AEOS_PRODUCT_REQUIREMENTS.md`). Other documents SHOULD use the repository's prevailing convention for their location. |
| **Document ID** | MUST be `AEOS-<NAME>`, uppercase, words separated by hyphens (for example `AEOS-PRD`, `AEOS-VISION`, `AEOS-GLOSSARY`). A Document ID MUST be unique and MUST NOT be reused after retirement. |
| **Version** | MUST be semantic versioning, `MAJOR.MINOR.PATCH`, with the version impact of a change determined by that document's own change control. |
| **Status** | MUST be one of: `Draft`, `Review`, `Freeze candidate`, `Frozen`, `Superseded`, `Retired`. |
| **Header block** | Every AEOS document MUST open with a metadata block stating at least Document, Product, Document ID, Version, Status, Owner, Audience, and Supersedes, followed by a statement of the document's authority. |

### 6.3 Repository Asset Names

| Aspect | Convention |
| :--- | :--- |
| **Form** | Repository Asset names MUST be lowercase kebab-case (for example `test-first-cycle`, `commit-message-standard`). |
| **Content** | A name MUST state the asset's purpose. It MUST NOT state its kind redundantly: an asset of kind Rule is not named `rule-…` where its kind is already evident from its declaration. |
| **Uniqueness** | Asset names MUST be unique within their kind and scope. Two assets of the same kind MUST NOT share a name. |
| **Versioning** | Versions MUST NOT appear in asset names. Assets are versioned through the repository and their own declared version. |

### 6.4 Identifiers

All AEOS identifiers share one shape:

```text
        <LAYER>-<AREA>-<NNN>

        LAYER   registered layer prefix, uppercase
        AREA    three uppercase letters naming the area, allocated by the owning document
        NNN     three digits, zero-padded, allocated sequentially from 001
```

| Prefix | Identifies | Owning document | Example |
| :--- | :--- | :--- | :--- |
| `PR` | Product requirement | AEOS-PRD | `PR-ENV-001` |
| `AR` | Architecture decision | Architecture documents | `AR-<AREA>-001` |
| `BP` | Blueprint item | Blueprint documents | `BP-<AREA>-001` |
| `SP` | Specified behavior | Specification documents | `SP-<AREA>-001` |
| `WF` | Workflow | Workflow assets | `WF-<AREA>-001` |

Identifier rules:

| # | Rule |
| :--- | :--- |
| I1 | Identifiers MUST be immutable. They are never reused, never renumbered, and never reassigned to different intent. |
| I2 | A retired item MUST be marked retired in place, retaining its identifier and its rationale. |
| I3 | An `AREA` code MUST be registered by the document that introduces it, and MUST NOT be reused across layers with a different meaning. |
| I4 | New layer prefixes MUST NOT be invented. Adding one is a change to this glossary and requires owner approval. |
| I5 | Every architecture, blueprint, and specification identifier MUST trace to one or more `PR-` identifiers. |
| I6 | Identifiers already in use in frozen documents — the product requirement prefixes `PR-ENV`, `PR-PRJ`, `PR-WFL`, `PR-RUN`, `PR-TDD`, `PR-DOC`, `PR-RUL`, `PR-SKL`, `PR-PMT`, `PR-REP`, `PR-PLT`, `PR-DST`, `PR-SAF`, `PR-NFR`, and the short-form series `C` (capabilities), `P` (problems), `V` (invariants), `G` (guiding principles), `R` (recommendations) — MUST be preserved exactly as published and MUST NOT be redefined by this or any later document. |

### 6.5 Technology Identifiers

| # | Rule |
| :--- | :--- |
| X1 | A technology MUST be referred to by its supplier's canonical name, spelled and capitalized as the supplier publishes it. |
| X2 | Marketing modifiers, edition names, and tiers MUST NOT be used unless they are load-bearing for the statement being made. |
| X3 | Where a machine-consumable asset must identify a technology, the identifier SHOULD be lowercase kebab-case derived from the canonical name. |
| X4 | Version references SHOULD use the supplier's published version string, and MUST NOT be paraphrased ("latest", "current", "recent"). |
| X5 | Naming a technology confers no privilege, no requirement, and no support commitment. |

### 6.6 Terms in Prose

| # | Rule |
| :--- | :--- |
| W1 | A Reserved Term MUST be capitalized when used in its AEOS sense, and MUST be lowercase when used in its ordinary English sense. |
| W2 | Where a Reserved Term's ordinary sense would be ambiguous, the sentence MUST be qualified instead (for example *project architecture*, *context window*, *language runtime*). |
| W3 | A document MUST NOT restate a glossary definition. It MAY quote one, attributed to AEOS-GLOSSARY. |
| W4 | Where a document uses a term this glossary does not define, it MUST either propose the addition or rephrase using defined terms. |

---

## 7. Reserved Terms

These words carry a specific, fixed meaning inside AEOS. Their meanings MUST remain stable across every
version of every AEOS document. Where an ordinary English sense is intended, the word MUST be lowercase
and the sentence MUST make the ordinary sense unmistakable.

| Reserved Term | Fixed AEOS meaning | MUST NOT be used to mean |
| :--- | :--- | :--- |
| **Product** | What AEOS is and does, as defined by AEOS-PRD; the topmost definition layer beneath the Vision. | A commercial offering, a release, a package, or a feature set. |
| **Architecture** | How AEOS itself is structured to deliver the Product. | The structure of a user's software (write *project architecture*), or a diagram. |
| **Capability** | One of the ten product capabilities `C1`–`C10`. | A feature, a function, a module, or what a runtime can do (write *Engineering Capability*). |
| **Workflow** | A versioned, runtime-independent declaration of steps, preconditions, gates, and success criteria. | A CI pipeline, a job definition, a habit, or an informal sequence of actions. |
| **Repository** | The version-controlled store that is a project's source of truth. | A package registry, an artifact store, a data store, or a directory. |
| **Asset** | Short form of Repository Asset: a durable, versioned artifact forming part of the product. | A binary, a media file, a build output, or anything transient. |
| **Runtime** | An external AI system that performs inference. | A language runtime, an application runtime, an execution environment, or a Tool. |
| **Review** | Examination of an artifact against requirements, rules, and tests, producing severity-classified findings. | A casual read, an approval, or a retrospective. |
| **Freeze** | The governance state in which content changes only through stated change control. | A technical lock, a feature freeze in the scheduling sense, or an indefinite prohibition on change. |
| **Context** | Information deliberately selected and supplied to a Runtime for a step of work. | A model's input window (write *context window*), or background information generally. |
| **Rule** | A versioned, scoped engineering constraint applied during generation, review, and refactoring. | A linter configuration, a policy about people, or a convention that is merely preferred. |
| **Skill** | A versioned, reusable, runtime-independent packaged engineering procedure. | A vendor feature of the same name, or a person's ability. |
| **Prompt** | A versioned, parameterized Repository Asset of deliberate instruction and context. | Disposable chat text, or a user interface prompt. |
| **Profile** | The Repository Asset describing what a project is and how it is built. | A user account, a preference set, or performance profiling. |
| **Platform** | A supported host operating system: Windows, macOS, or Linux. | A vendor's AI service, a cloud provider, or a distribution channel. |
| **Model** | A language model, model family, or model version used by a Runtime. | A data model, a domain model, or a mental model. |
| **Specification** | A precise, testable statement of required behavior traceable to a requirement identifier. | A requirement, a design note, or a description of existing behavior. |

> **On the strength of "reserved".**
> A reserved meaning is not a preference about style. A document that uses one of these words in a
> different sense states something other than what its author intended, and will be read incorrectly by
> the AI runtimes that maintain this repository. Such usage is a defect and is reported in review.

---

## 8. Deprecated Terminology

Definitions are load-bearing for traceability, review, and freeze. A definition that changes quietly
invalidates every document that relied on it, without a version, an approval, or a record. AEOS
therefore never changes a definition in place to mean something different.

### 8.1 Rules

| # | Rule |
| :--- | :--- |
| D1 | A published definition MUST NOT be edited to mean something different. Editorial correction that preserves meaning is permitted; a change of meaning is not. |
| D2 | A change of meaning MUST proceed by deprecating the existing term and introducing a new official name with its own definition. |
| D3 | A deprecated term MUST remain documented, in place, with its original definition intact, its deprecated status, the version in which it was deprecated, the term that supersedes it, and the reason. |
| D4 | A superseding term's entry MUST reference the term it replaces, so that a reader of either can reach the other. |
| D5 | A deprecated term MUST NOT be used in new documents. Existing documents retain it until they are revised under their own change control. |
| D6 | A deprecated name MUST NOT be reused later for a different concept. |
| D7 | Deprecation is never silent: it MUST appear in the deprecation record and in this document's revision history. |

### 8.2 Deprecation Record

The record below is the complete list of deprecated AEOS terminology. It is empty because no term has
been deprecated: version 1.0.0 is the first publication of AEOS terminology, and all terms in
[Core Terminology](#4-core-terminology) are current.

| Deprecated term | Original definition | Superseded by | Deprecated in | Reason |
| :--- | :--- | :--- | :--- | :--- |
| *No terms are deprecated as of AEOS-GLOSSARY 1.0.1.* | — | — | — | — |

---

## 9. Document Governance

### 9.1 Status

This document is the Terminology Source of Truth for the AEOS repository. It is intended to be frozen
as part of AEOS 1.0 and to remain stable across the life of the product.

### 9.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Clarification of an existing definition that preserves its meaning | Owner approval | Minor |
| Addition of a term, naming convention, or reserved term | Owner approval | Minor |
| Deprecation of a term and introduction of its replacement | Explicit owner revision request with recorded rationale | Major |
| Change to a terminology principle or to an identifier convention | Explicit owner revision request with recorded rationale | Major |
| Removal of a term without replacement | Explicit owner decision, recorded, with the definition preserved in the deprecation record | Major |

### 9.3 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major findings remain.

### 9.4 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-PRD on the meaning of a term | AEOS-PRD governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-VISION on intent | AEOS-VISION governs for intent, AEOS-PRD for behavior. The conflict is a defect in this document and is reported. |
| A downstream document defines a term this glossary already defines | This glossary governs. The downstream definition is removed and replaced by a reference. |
| A downstream document needs a term this glossary does not define | The term is proposed for addition here under change control. It is not defined locally. |
| A term reserved for architecture is cited as authority for product behavior | The citation is invalid. Product obligation arises only from `PR-` identifiers. |

### 9.5 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Freeze candidate | Initial terminology definition. Establishes ten terminology principles, forty-nine canonical term entries with recorded authority, four terminology relationship chains and a confusion table, naming conventions for documents, repository assets, identifiers, technology references, and prose, seventeen reserved terms, and the deprecation policy with an empty deprecation record. Introduces no requirement, capability, or architecture. Four terms — Blueprint, Context Router, Runtime Adapter, and Workflow Engine — are recorded as reserved for architecture: the names are fixed, the normative definitions are not made here. |
| 1.0.1 | Freeze candidate | Format conformance only, under AEOS-DOCSTD. Converted the document from HTML-in-Markdown to GitHub-Flavored Markdown: every table rendered as a Markdown table, every collapsible term entry rendered as a heading, and all raw HTML, alignment attributes, and structural wrappers removed. Adopted the required conformance notice verbatim and normalized the keyword `SHALL` to `MUST`, which AEOS-DOCSTD Section 7.2 recognizes as synonyms, so that every keyword used is covered by that notice. Added the required scope section, recorded the deliberate deviation from the Glossary-layer guidance on normative keywords, consolidated status, change control, review policy, precedence, and revision history into a Document Governance section as the final numbered section, renumbered sections accordingly, and updated internal links. Marked Appendix A non-normative. The one-line summary that headed each collapsible entry was removed with the collapsible construct; the same short definition is retained for every term in Appendix A. No definition, purpose, authority assignment, principle, relationship, naming convention, reserved term, deprecation rule, or ownership statement was changed. |

---

## Appendix A — Quick Reference Table

**This appendix is non-normative.**

Short definitions for scanning. The canonical definition of every term is its entry in
[Core Terminology](#4-core-terminology); where the two appear to differ, the entry governs.

| Term | Short definition | Primary document |
| :--- | :--- | :--- |
| **Action Class** | Classification of an action by effect, determining the approval it requires. | AEOS-PRD |
| **AEOS** | The product: an operating system for AI-assisted, human-supervised software engineering. | AEOS-PRD |
| **Approval Gate** | The point at which a proposed action requires explicit human confirmation. | AEOS-PRD |
| **Architecture** | How AEOS is structured to deliver the Product. | AEOS-PRD |
| **Automation Grant** | Explicit, scoped, recorded, revocable delegation of approval authority. | AEOS-PRD |
| **Blueprint** | Document layer between Architecture and Specification. | AEOS-GLOSSARY (reserved for architecture) |
| **Bootstrap** | The first, human-approved sequence making AEOS usable in an environment or project. | AEOS-GLOSSARY |
| **Capability** | One of the ten product capabilities `C1`–`C10`. | AEOS-PRD |
| **Context** | Information deliberately selected and supplied to a Runtime for a step of work. | AEOS-PRD |
| **Context Minimization** | Sending the smallest context sufficient for the task, with each inclusion explainable. | AEOS-PRD |
| **Context Router** | The named responsibility for selecting and justifying a step's Context. | AEOS-GLOSSARY (reserved for architecture) |
| **Contributor** | A person or AI runtime proposing a change to AEOS itself. | AEOS-VISION |
| **Developer** | A person who uses AEOS to build or maintain their own project. | AEOS-GLOSSARY |
| **Distribution Method** | An official way AEOS is delivered; never changes the product architecture. | AEOS-PRD |
| **Document** | A durable, versioned artifact readable by humans and consumable by AI runtimes. | AEOS-GLOSSARY |
| **Engineering Capability** | A unit of engineering work a workflow step requires and a runtime may or may not provide. | AEOS-GLOSSARY |
| **Environment** | The machine, its platform, its tooling, and its available runtimes, as observed. | AEOS-PRD |
| **Freeze** | The governance state in which content changes only through stated change control. | AEOS-PRD |
| **Human Approval** | The explicit act authorizing a specific proposed action. | AEOS-PRD |
| **Human-in-the-Loop** | The requirement that a human decides before AEOS acts consequentially. | AEOS-PRD |
| **Implementation** | The code and tests that realize the layers above them. | AEOS-PRD |
| **Model** | A language model, model family, or version used by a Runtime to perform inference. | AEOS-GLOSSARY |
| **Platform** | A supported host operating system: Windows, macOS, or Linux. | AEOS-PRD |
| **Product** | What AEOS is and does for its users; the layer beneath the Vision. | AEOS-PRD |
| **Product Boundary** | The line between what AEOS is and how AEOS is built. | AEOS-PRD |
| **Profile** | The asset describing a project's identity, technology, build and test approach, runtime, and rules. | AEOS-PRD |
| **Project** | The unit of work AEOS operates on: a repository and its Repository Assets. | AEOS-PRD |
| **Project Type** | A descriptive classification of a project recorded in its Profile. | AEOS-GLOSSARY |
| **Prompt** | A versioned, parameterized asset of deliberate instruction and context. | AEOS-PRD |
| **Proposal** | A statement of intended action, rationale, effects, reversibility, and consequence of declining. | AEOS-PRD |
| **Repository** | The version-controlled store that is a project's source of truth. | AEOS-PRD |
| **Repository Asset** | A durable, versioned artifact forming part of the product. | AEOS-PRD |
| **Review** | Examination against requirements, rules, and tests, producing severity-classified findings. | AEOS-PRD |
| **Rule** | A versioned, scoped engineering constraint applied during generation, review, and refactoring. | AEOS-PRD |
| **Runtime** | An external AI system that performs inference; always an integration. | AEOS-PRD |
| **Runtime Adapter** | The named responsibility mediating between AEOS and one external Runtime. | AEOS-GLOSSARY (reserved for architecture) |
| **Runtime State** | Transient, machine-local information produced while AEOS runs; not a product asset. | AEOS-PRD |
| **Skill** | A versioned, reusable, runtime-independent packaged engineering procedure. | AEOS-PRD |
| **Source of Truth** | The artifact that governs a subject and wins any conflict about it. | AEOS-GLOSSARY |
| **Specification** | A precise, testable statement of required behavior, traceable to a requirement identifier. | AEOS-PRD |
| **TDD Cycle** | Define behavior → failing test → verify failure reason → minimal implementation → refactor green. | AEOS-PRD |
| **Technology Stack** | The languages, frameworks, and tools a project uses, recorded in its Profile. | AEOS-GLOSSARY |
| **Template** | An asset providing a reusable starting point for repeated work. | AEOS-PRD |
| **Tool** | An external program or system that performs non-inference work for a project. | AEOS-GLOSSARY |
| **Vendor** | An organization that supplies a Runtime, Model, or Tool. | AEOS-GLOSSARY |
| **Vision** | Why AEOS exists and what it must remain. | AEOS-VISION |
| **Workflow** | A versioned, runtime-independent declaration of steps, gates, and success criteria. | AEOS-PRD |
| **Workflow Engine** | The named responsibility for executing workflows under their approval gates. | AEOS-GLOSSARY (reserved for architecture) |
| **Workflow State** | The durable record of where a workflow stands. | AEOS-GLOSSARY |

---

**End of Glossary**

AEOS-GLOSSARY · Version 1.0.1 · Terminology Source of Truth
