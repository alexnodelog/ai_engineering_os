<div align="center">

# AI Engineering Operating System

**AEOS — Glossary**

*The permanent terminology source of truth for AEOS.*

</div>

<table>
<thead>
<tr><th align="left">Field</th><th align="left">Value</th></tr>
</thead>
<tbody>
<tr><td><strong>Document</strong></td><td>Glossary</td></tr>
<tr><td><strong>Product</strong></td><td>AI Engineering Operating System (AEOS)</td></tr>
<tr><td><strong>Document ID</strong></td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Version</strong></td><td>1.0.0</td></tr>
<tr><td><strong>Status</strong></td><td>Freeze candidate — awaiting owner approval</td></tr>
<tr><td><strong>Owner</strong></td><td>Product Owner, AEOS</td></tr>
<tr><td><strong>Author</strong></td><td>Chief Information Architect and Terminology Authority, AEOS</td></tr>
<tr><td><strong>Audience</strong></td><td>Architects, contributors, maintainers, documentation authors, and AI runtimes consuming this repository</td></tr>
<tr><td><strong>Suggested path</strong></td><td><code>docs/product/GLOSSARY.md</code></td></tr>
<tr><td><strong>Companion documents</strong></td><td><code>AEOS_VISION.md</code> (AEOS-VISION) · <code>AEOS_PRODUCT_REQUIREMENTS.md</code> (AEOS-PRD)</td></tr>
<tr><td><strong>Supersedes</strong></td><td>None</td></tr>
</tbody>
</table>

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

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**,
**SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as
described in RFC 2119.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Terminology Principles](#2-terminology-principles)
3. [Core Terminology](#3-core-terminology)
4. [Terminology Relationships](#4-terminology-relationships)
5. [Naming Conventions](#5-naming-conventions)
6. [Reserved Terms](#6-reserved-terms)
7. [Deprecated Terminology](#7-deprecated-terminology)
8. [Appendix A — Quick Reference Table](#appendix-a--quick-reference-table)
9. [Revision History](#revision-history)

---

<section>

## 1. Executive Summary

AEOS is a product whose primary artifact is a repository read by two kinds of maintainer: people who
will arrive years after the original participants have left, and AI runtimes that hold no knowledge
of the project beyond what the repository states. Both read the same words. Neither can ask a
question of someone who is no longer present.

A term used in two senses is therefore not a stylistic blemish. It is a defect with consequences:

<table>
<thead>
<tr><th align="left">Where inconsistency lands</th><th align="left">What it costs</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Traceability</strong></td>
<td>AEOS-PRD requires every architectural decision, specification, and test to trace to a requirement identifier. A trace is only verifiable if both ends use the same vocabulary. Two names for one concept break the chain silently.</td>
</tr>
<tr>
<td><strong>Review</strong></td>
<td>A review classifies findings against a stated standard. Where the standard's words are ambiguous, reviewers argue about meaning instead of about the change, and the argument is settled by whoever is most senior rather than by what was written.</td>
</tr>
<tr>
<td><strong>AI consumption</strong></td>
<td>An unstated convention is not a convention to a runtime; it is a guess with a plausible tone. Ambiguous terminology produces confidently wrong work at machine speed.</td>
</tr>
<tr>
<td><strong>Layer separation</strong></td>
<td>AEOS keeps Product, Architecture, Specification, Runtime, and Implementation strictly separated. That separation is enforced through words. When a product term drifts into architectural meaning, the boundary is crossed before anyone notices it was approached.</td>
</tr>
<tr>
<td><strong>Freeze</strong></td>
<td>A frozen document is only as stable as the meaning of its sentences. If definitions drift underneath it, the document changes without a version, an approval, or a record.</td>
</tr>
</tbody>
</table>

This glossary exists so that those failures cannot occur quietly. It fixes one official name and one
canonical definition per concept, states which document holds authority over each, and defines the
conventions by which future names are formed. It is deliberately narrow: it standardizes language and
nothing else.

</section>

---

<section>

## 2. Terminology Principles

These principles govern all AEOS terminology, in this document and in every document that follows it.

<table>
<thead>
<tr><th align="left">#</th><th align="left">Principle</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr>
<td>T1</td>
<td><strong>One concept, one official name.</strong></td>
<td>Each concept SHALL have exactly one official name. Documents MUST use that name and MUST NOT introduce an alternative for the same concept.</td>
</tr>
<tr>
<td>T2</td>
<td><strong>One official definition.</strong></td>
<td>Each official name SHALL have exactly one canonical definition, recorded in <a href="#3-core-terminology">Core Terminology</a>. No document MAY state a competing definition.</td>
</tr>
<tr>
<td>T3</td>
<td><strong>No unregistered synonyms.</strong></td>
<td>Synonymous engineering terms MUST NOT be used interchangeably. Where an alternative wording is genuinely required, it MUST be registered in this glossary as a synonym of the official name, or it MUST NOT be used.</td>
</tr>
<tr>
<td>T4</td>
<td><strong>Definitions are stable across versions.</strong></td>
<td>A published definition MUST NOT be changed in place to mean something different. Meaning changes proceed by deprecation and replacement under <a href="#7-deprecated-terminology">Section 7</a>.</td>
</tr>
<tr>
<td>T5</td>
<td><strong>Reference, do not redefine.</strong></td>
<td>Future documents MUST reference this glossary for terminology. A document that needs a term this glossary does not define MUST propose its addition here rather than defining it locally.</td>
</tr>
<tr>
<td>T6</td>
<td><strong>Definitions are implementation-independent.</strong></td>
<td>A definition MUST describe what a concept <em>is</em>, never how it is built, stored, transported, or executed. A definition that can be satisfied in only one way is a defect in this document.</td>
</tr>
<tr>
<td>T7</td>
<td><strong>Definitions are vendor-neutral.</strong></td>
<td>No definition MAY be expressed in terms of a specific vendor, runtime, model, platform, distribution method, language, or product. Vendor names MAY appear only as illustration, and confer nothing.</td>
</tr>
<tr>
<td>T8</td>
<td><strong>Authority is recorded, not assumed.</strong></td>
<td>Every entry SHALL state which document holds authority over its meaning. This glossary records definitions owned elsewhere; it does not acquire ownership of them by recording them.</td>
</tr>
<tr>
<td>T9</td>
<td><strong>A name confers nothing.</strong></td>
<td>Defining a term neither creates a capability nor authorizes behavior. Product obligation arises only from AEOS-PRD requirement identifiers.</td>
</tr>
<tr>
<td>T10</td>
<td><strong>Write for both audiences.</strong></td>
<td>Every definition SHALL be intelligible to a human maintainer and unambiguous to an AI runtime, from the same text. A definition that requires tone, context, or prior acquaintance to disambiguate is incomplete.</td>
</tr>
</tbody>
</table>

</section>

---

<section>

## 3. Core Terminology

### How to Read an Entry

Each entry states a **Definition**, a **Purpose**, an **Authority**, its **Related terms**, and where
useful, **Notes** recording distinctions that are commonly gotten wrong.

The **Authority** field is the load-bearing one. It states which document governs the meaning:

<table>
<thead>
<tr><th align="left">Authority value</th><th align="left">Meaning</th></tr>
</thead>
<tbody>
<tr>
<td><strong>AEOS-PRD</strong></td>
<td>The meaning is governed by the Product Requirements Document. This glossary records it and MUST NOT alter it. Product obligation attaches through <code>PR-</code> identifiers.</td>
</tr>
<tr>
<td><strong>AEOS-VISION</strong></td>
<td>The meaning is governed by the Vision Document. It expresses intent and confers no product obligation.</td>
</tr>
<tr>
<td><strong>AEOS-GLOSSARY</strong></td>
<td>This document is the defining authority. The term is terminology only: it names something the other documents use without naming, and confers no capability.</td>
</tr>
<tr>
<td><strong>AEOS-GLOSSARY (reserved for architecture)</strong></td>
<td>This document reserves the <em>name</em> and fixes the concept it refers to, so that architecture documents cannot introduce it under a competing label. The normative definition — whether the thing exists, what form it takes, and how it behaves — belongs to architecture documents and is not decided here. Such a term MUST NOT be cited to justify behavior, and MUST NOT appear in product-layer documents as though it carried obligation.</td>
</tr>
</tbody>
</table>

Entries are ordered alphabetically. Related terms are given as official names and are all defined in
this section.

---

<details>
<summary><strong>Action Class</strong> — the classification of an action by its effect, determining the approval it requires</summary>

<br>

**Definition.** The classification of an action by its effect — observation, local change, external
effect, or destructive — determining the approval required before it may be executed.

**Purpose.** Makes the strength of an Approval Gate a property of the action rather than a matter of
judgment at the moment of asking, so that identical actions are always gated identically.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Automation Grant · Human Approval · Proposal · Workflow.

**Notes.** The four classes are a closed set. A document that needs a fifth is proposing a change to
the product's concepts and MUST route the idea through owner approval rather than naming it.

</details>

<details>
<summary><strong>AEOS</strong> — the product defined by AEOS-PRD; an operating system for AI-assisted software engineering</summary>

<br>

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

</details>

<details>
<summary><strong>Approval Gate</strong> — the point at which a proposed action requires explicit human confirmation before execution</summary>

<br>

**Definition.** The point at which a proposed action requires explicit human confirmation before
execution.

**Purpose.** Names the structural position of supervision within a workflow, independently of how any
particular confirmation is obtained.

**Authority.** AEOS-PRD.

**Related terms.** Action Class · Human Approval · Human-in-the-Loop · Proposal · Workflow.

**Notes.** An Approval Gate is a position; Human Approval is the act that satisfies it. The two MUST
NOT be used interchangeably.

</details>

<details>
<summary><strong>Architecture</strong> — the layer that determines how AEOS is structured so that it can deliver the Product</summary>

<br>

**Definition.** The layer that determines how AEOS is structured so that it can deliver the Product,
together with the documents that record those determinations.

**Purpose.** Names the layer between Product and Specification, and the body of decisions owned there.

**Authority.** AEOS-PRD.

**Related terms.** Blueprint · Implementation · Product · Specification · Freeze.

**Notes.** Capitalized *Architecture* always refers to the architecture of AEOS itself. The structure
of a user's software is *project architecture* and MUST be written that way; the Repository Asset kind
that records it is an *Architecture Document*. Architecture decides *how*; it never decides *whether*.

</details>

<details>
<summary><strong>Automation Grant</strong> — an explicit, scoped, recorded, revocable delegation of approval authority</summary>

<br>

**Definition.** An explicit, scoped, recorded, revocable delegation of approval authority for specific
action classes.

**Purpose.** Names the only mechanism by which supervision may be relaxed, so that relaxation is
always a decision someone made rather than a default that arrived.

**Authority.** AEOS-PRD.

**Related terms.** Action Class · Approval Gate · Human Approval · Human-in-the-Loop · Repository Asset.

**Notes.** An Automation Grant never authorizes destructive actions. *Automation* used without the
word *grant* MUST NOT be read as implying one.

</details>

<details>
<summary><strong>Blueprint</strong> — the document layer between Architecture and Specification</summary>

<br>

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

</details>

<details>
<summary><strong>Bootstrap</strong> — the first, human-approved sequence that makes AEOS usable in an Environment or Project</summary>

<br>

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

</details>

<details>
<summary><strong>Capability</strong> — one of the ten product capabilities that together constitute AEOS</summary>

<br>

**Definition.** One of the ten product capabilities defined in AEOS-PRD that together constitute the
product, each identified as `C1` through `C10` and expressed as numbered product requirements.

**Purpose.** Names the unit in which the product's scope is stated and counted.

**Authority.** AEOS-PRD.

**Related terms.** Engineering Capability · Product · Specification · Workflow.

**Notes.** *Capability* is a reserved term with a fixed, closed referent. It MUST NOT be used to mean
"feature", "function", "module", or "what a runtime can do". For the ability a workflow step requires
and a runtime may or may not provide, use **Engineering Capability**.

</details>

<details>
<summary><strong>Context</strong> — the information deliberately selected and supplied to a Runtime for a step of work</summary>

<br>

**Definition.** The information deliberately selected and supplied to a Runtime so that it can perform
a defined step of work, together with the reason each element was included.

**Purpose.** Names what crosses the boundary to an external runtime, so that its size, its content,
and its justification can be discussed as one thing.

**Authority.** AEOS-PRD.

**Related terms.** Context Minimization · Context Router · Prompt · Runtime · Workflow.

**Notes.** *Context* in AEOS always means selected project information. It MUST NOT be used to mean a
model's input window, which is a property of a Model and is written *context window*. Context that
cannot be justified element by element is not minimized, whatever its size.

</details>

<details>
<summary><strong>Context Minimization</strong> — the principle of sending the smallest context sufficient for the task</summary>

<br>

**Definition.** The principle that AEOS sends the smallest context sufficient for the task and can
explain each inclusion.

**Purpose.** Names the standard against which any context selection is judged.

**Authority.** AEOS-PRD.

**Related terms.** Context · Context Router · Prompt · Runtime.

**Notes.** Minimization is a property of deliberate selection, not of measured size. Bulk transfer is
a failure of design, and unnecessary context is a defect rather than overhead.

</details>

<details>
<summary><strong>Context Router</strong> — the named responsibility for selecting and justifying the Context of a step</summary>

<br>

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

</details>

<details>
<summary><strong>Contributor</strong> — a person or AI runtime proposing a change to AEOS itself</summary>

<br>

**Definition.** A person or AI runtime proposing a change to AEOS itself — to its documents, its
assets, or its implementation.

**Purpose.** Distinguishes those who change the product from those who use it, since the two are
governed by different rules.

**Authority.** AEOS-VISION.

**Related terms.** Developer · Review · Freeze · Repository.

**Notes.** Contributors are bound by the guiding principles in AEOS-VISION and by the architecture
freeze. One individual may be both a Contributor and a Developer; the roles remain distinct in review
and governance, and a document MUST state which one it addresses.

</details>

<details>
<summary><strong>Developer</strong> — a person who uses AEOS to build or maintain their own project</summary>

<br>

**Definition.** A person who uses AEOS to build or maintain their own software project.

**Purpose.** Names the primary user of the product, as distinct from a person who changes the product.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Contributor · Project · Repository · Human Approval.

**Notes.** *Developer* is the general term for a user of AEOS and covers the solo developer,
the team member, the engineering lead, and the platform engineer described in AEOS-PRD. Where a
statement applies to only one of those, that role MUST be named explicitly instead.

</details>

<details>
<summary><strong>Distribution Method</strong> — an official way AEOS is delivered to users</summary>

<br>

**Definition.** An official way AEOS is delivered to users. It never changes the product architecture.

**Purpose.** Separates packaging, discovery, and update mechanics from product meaning.

**Authority.** AEOS-PRD.

**Related terms.** Platform · Product · Repository · Bootstrap.

**Notes.** No capability is exclusive to a Distribution Method, and how AEOS was installed is never a
semantic difference. *Distribution* and *Distribution Method* refer to the same concept; the shorter
form MAY be used where no ambiguity arises.

</details>

<details>
<summary><strong>Document</strong> — a durable, versioned, human- and AI-readable artifact stating part of what a project or product knows about itself</summary>

<br>

**Definition.** A durable, versioned artifact that states some part of what AEOS or a project knows
about itself, readable by humans and consumable by AI runtimes from the same text.

**Purpose.** Names the artifact kind through which AEOS records intent, requirement, structure,
behavior, and decision.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Repository Asset · Source of Truth · Specification · Freeze · Review.

**Notes.** Every AEOS document carries a Document ID, a version, a status, an owner, and a stated
authority. Documents are Repository Assets and are reviewed, versioned, and maintained like code. A
document containing placeholders or unfinished sections is an unfinished artifact and is not shipped.

</details>

<details>
<summary><strong>Engineering Capability</strong> — a discrete unit of engineering work a Workflow step requires and a Runtime may or may not provide</summary>

<br>

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

</details>

<details>
<summary><strong>Environment</strong> — the machine, its tooling, and its available runtimes, as observed at a point in time</summary>

<br>

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

</details>

<details>
<summary><strong>Freeze</strong> — the governance state in which a document's content changes only through its stated change control</summary>

<br>

**Definition.** The governance state in which a document's definitions and decisions MUST NOT change
except through the change control that document defines, under the owner's approval.

**Purpose.** Makes stability a declared, auditable state rather than an informal expectation.

**Authority.** AEOS-PRD.

**Related terms.** Document · Review · Source of Truth · Architecture.

**Notes.** Freeze is a governance state, not a technical lock. A frozen document still accepts
editorial correction at patch level; what it does not accept is a change of meaning without an owner
decision. Under the architecture freeze, an improvement to the product's concepts is recorded as a
recommendation for a future release rather than applied.

</details>

<details>
<summary><strong>Human Approval</strong> — the explicit act by which a person authorizes a specific proposed action</summary>

<br>

**Definition.** The explicit act by which a person authorizes a specific proposed action before it is
executed.

**Purpose.** Names the act that satisfies an Approval Gate, so that the act and the gate can be spoken
about separately.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Action Class · Automation Grant · Human-in-the-Loop · Proposal.

**Notes.** Silence is not Human Approval. Ambiguity is not Human Approval. Approval of a different
action is not Human Approval of this one. Approval authorizes exactly what was proposed; scope
expansion requires a new Proposal.

</details>

<details>
<summary><strong>Human-in-the-Loop</strong> — the requirement that a human decides before AEOS acts consequentially</summary>

<br>

**Definition.** The requirement that a human decides before AEOS acts consequentially.

**Purpose.** Names the product's default posture toward supervision.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Automation Grant · Human Approval · Proposal.

**Notes.** Written with hyphens and initial capitals when naming the principle. The abbreviation *HITL*
MUST NOT be used in AEOS documents.

</details>

<details>
<summary><strong>Implementation</strong> — the code and tests that realize the layers above them</summary>

<br>

**Definition.** The code and tests that realize the Architecture, Blueprint, and Specification layers,
traced back to the requirement identifiers they satisfy.

**Purpose.** Names the lowest layer of the AEOS document and delivery chain.

**Authority.** AEOS-PRD.

**Related terms.** Architecture · Blueprint · Specification · Repository · Review.

**Notes.** Capitalized *Implementation* refers to AEOS's own code. Software written by a Developer in
their own project is *project code* and MUST be written that way. *Implementation detail* is out of
scope for product-layer documents.

</details>

<details>
<summary><strong>Model</strong> — a language model, model family, or model version used by a Runtime to perform inference</summary>

<br>

**Definition.** A specific language model, model family, or model version that a Runtime uses to
perform inference.

**Purpose.** Separates the thing that performs inference from the system that exposes it, so that model
choice can be discussed as an ordinary engineering decision.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Runtime · Vendor · Context · Engineering Capability.

**Notes.** A Model is never a Runtime, and neither is ever part of AEOS. Model selection belongs to the
user. AEOS is independent of any model, model family, or model version, and no AEOS behavior may
depend on undocumented behavior of a specific one.

</details>

<details>
<summary><strong>Platform</strong> — a supported host operating system</summary>

<br>

**Definition.** A supported host operating system on which AEOS operates: Windows, macOS, or Linux.

**Purpose.** Names the axis along which AEOS commits to equal capability.

**Authority.** AEOS-PRD.

**Related terms.** Environment · Distribution Method · Product · Tool.

**Notes.** *Platform* MUST NOT be used to mean a vendor's AI service, a distribution channel, a cloud
provider, or an application runtime. The three platforms are equal citizens; a capability that works on
only one of them is incomplete rather than shipped.

</details>

<details>
<summary><strong>Product</strong> — the whole of what AEOS is and does for its users, as defined by AEOS-PRD</summary>

<br>

**Definition.** The whole of what AEOS is and does for its users, as defined by AEOS-PRD; equivalently,
the layer that answers what AEOS is, who it is for, and what it must do for them.

**Purpose.** Names both the thing being built and the topmost layer of definition beneath the Vision.

**Authority.** AEOS-PRD.

**Related terms.** Vision · Architecture · Capability · Repository · Product Boundary.

**Notes.** *Product Boundary* is the line AEOS-PRD draws between product definition and implementation:
what AEOS is belongs to the product layer; how AEOS is built belongs to architecture, specification, and
runtime documents. The repository is the product's durable form, not a place where the product is kept.

</details>

<details>
<summary><strong>Product Boundary</strong> — the line between what AEOS is and how AEOS is built</summary>

<br>

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

</details>

<details>
<summary><strong>Profile</strong> — the versioned Repository Asset describing what a Project is and how it is built</summary>

<br>

**Definition.** The versioned Repository Asset describing a project's identity, technology, build and
test approach, runtime selection, and applicable rules.

**Purpose.** Gives a new session — human or AI — enough recorded knowledge to become useful without the
project being re-explained.

**Authority.** AEOS-PRD.

**Related terms.** Project · Project Type · Technology Stack · Repository Asset · Runtime · Rule.

**Notes.** *Profile* and *Project Profile* are the same term; the short form MAY be used where no
ambiguity arises. AEOS defines no other kind of profile, and the word MUST NOT be used for user
profiles, performance profiling, or runtime configuration.

</details>

<details>
<summary><strong>Project</strong> — the unit of work AEOS operates on</summary>

<br>

**Definition.** The unit of work AEOS operates on: a repository together with its Repository Assets,
governed by AEOS.

**Purpose.** Names the boundary within which profiles, rules, workflows, and approvals apply.

**Authority.** AEOS-PRD.

**Related terms.** Repository · Repository Asset · Profile · Project Type · Developer.

**Notes.** A Project always belongs to the user, never to AEOS. Adopting an existing project is the
ordinary case rather than the exception, and adoption never overwrites, relocates, or restructures what
was already there. Work on one project never affects another.

</details>

<details>
<summary><strong>Project Type</strong> — a descriptive classification of a Project recorded in its Profile</summary>

<br>

**Definition.** A descriptive classification of a Project, recorded in its Profile, expressing the kind
of software the project builds.

**Purpose.** Lets projects of a similar kind be described consistently, so that documents and assets can
refer to the classification instead of enumerating characteristics.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Profile · Project · Technology Stack · Template.

**Notes.** Project Type is descriptive metadata and grants no behavior: no rule, workflow, or asset is
applied merely because a project carries a given type. AEOS-supplied workflow templates for common
project archetypes are a recorded recommendation for a future release, not a current product concept.

</details>

<details>
<summary><strong>Prompt</strong> — a versioned, parameterized, portable Repository Asset of deliberate instruction and context</summary>

<br>

**Definition.** A versioned, parameterized, portable Repository Asset composed of deliberately selected
context and instruction.

**Purpose.** Treats the text sent to a runtime as an engineering artifact subject to review, versioning,
and reuse, rather than as disposable session material.

**Authority.** AEOS-PRD.

**Related terms.** Context · Context Minimization · Repository Asset · Runtime · Skill.

**Notes.** A Prompt remains inspectable before it is sent, because a prompt the user cannot read is a
decision the user did not make. Credentials and user-designated sensitive content never appear in one.

</details>

<details>
<summary><strong>Proposal</strong> — a statement of intended action, its rationale, effects, reversibility, and the consequence of declining</summary>

<br>

**Definition.** A statement of intended action including rationale, effects, reversibility, and the
consequence of declining.

**Purpose.** Names the artifact a human is asked to approve, so that approval always attaches to
something specific and recorded.

**Authority.** AEOS-PRD.

**Related terms.** Approval Gate · Human Approval · Action Class · Review · Workflow.

**Notes.** A declined Proposal is a normal outcome and is never treated as an error. Executing beyond
what a Proposal stated requires a new Proposal, not a broader reading of the old one.

</details>

<details>
<summary><strong>Repository</strong> — the version-controlled store that holds a Project's code and Repository Assets and is its Source of Truth</summary>

<br>

**Definition.** The version-controlled store that contains a project's code and Repository Assets and is
the authoritative source of truth for that project.

**Purpose.** Names the durable form of the product and of every project governed by it.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Runtime State · Project · Source of Truth · Product.

**Notes.** Unqualified, *Repository* means the user's project repository; AEOS's own is *the AEOS
repository*. *Repository as Product* is the principle that the repository is the product and its
authoritative source of truth. What is not in the repository does not exist, and a repository remains
meaningful when AEOS is not running.

</details>

<details>
<summary><strong>Repository Asset</strong> — a durable, versioned artifact that forms part of the product and lives in the repository</summary>

<br>

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

</details>

<details>
<summary><strong>Review</strong> — examination of an artifact against requirements, rules, and tests before it enters the Repository</summary>

<br>

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

</details>

<details>
<summary><strong>Rule</strong> — a versioned, scoped engineering constraint applied during generation, review, and refactoring</summary>

<br>

**Definition.** A versioned, scoped engineering constraint applied during generation, review, and
refactoring.

**Purpose.** Carries an engineering lead's intent to every developer and every runtime without being
restated.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Review · Skill · Workflow · Profile.

**Notes.** Every Rule has a defined scope, and precedence between overlapping rules is deterministic and
explainable. AEOS never applies a rule the user cannot inspect. A rule that cannot be enforced under the
selected runtime is reported rather than silently ignored.

</details>

<details>
<summary><strong>Runtime</strong> — an external AI system that performs inference</summary>

<br>

**Definition.** An external AI system that performs inference. Always an integration, never a part of
AEOS.

**Purpose.** Names the class of external systems AEOS orchestrates and the boundary they sit behind.

**Authority.** AEOS-PRD.

**Related terms.** Model · Vendor · Runtime Adapter · Engineering Capability · Tool.

**Notes.** *Runtime* in AEOS always means an AI runtime. It MUST NOT be used to mean a language runtime,
an application runtime, or an execution environment; where those are meant, they MUST be written out.
A system that performs no inference is a **Tool**, not a Runtime. Runtime selection belongs to the user
and is never overridden or silently substituted.

</details>

<details>
<summary><strong>Runtime Adapter</strong> — the named responsibility that mediates between AEOS and one external Runtime</summary>

<br>

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

</details>

<details>
<summary><strong>Runtime State</strong> — transient, machine-local, or environment-specific information produced while AEOS runs</summary>

<br>

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

</details>

<details>
<summary><strong>Skill</strong> — a versioned, reusable, runtime-independent packaged engineering procedure</summary>

<br>

**Definition.** A versioned, reusable, runtime-independent packaged engineering procedure.

**Purpose.** Lets a team's accumulated know-how be applied repeatedly and survive a change of vendor.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Rule · Prompt · Workflow · Template.

**Notes.** Skills are additive: users add, modify, and remove them without modifying AEOS. Which skill
was applied, and why, is reported rather than left implicit. *Skill* MUST NOT be used to describe a
runtime-specific feature offered under the same word by a vendor.

</details>

<details>
<summary><strong>Source of Truth</strong> — the single artifact that governs a subject and wins any conflict about it</summary>

<br>

**Definition.** The single artifact that governs a subject: where it conflicts with any other statement
about that subject, it governs and the other statement is a defect.

**Purpose.** Makes conflict resolution mechanical rather than a matter of seniority or recency.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Document · Freeze · Product · Repository · Vision.

**Notes.** The registered sources of truth are:

<table>
<thead>
<tr><th align="left">Subject</th><th align="left">Source of Truth</th></tr>
</thead>
<tbody>
<tr><td>Why AEOS exists and what it must remain</td><td>AEOS-VISION</td></tr>
<tr><td>What AEOS is and what it must do</td><td>AEOS-PRD</td></tr>
<tr><td>Official terminology and naming</td><td>AEOS-GLOSSARY</td></tr>
<tr><td>A project's code, assets, and state</td><td>The Repository</td></tr>
</tbody>
</table>

A document MUST NOT declare itself a source of truth for a subject already registered above.

</details>

<details>
<summary><strong>Specification</strong> — a precise, testable statement of required behavior, traceable to a requirement identifier</summary>

<br>

**Definition.** A precise statement of required behavior, expressed testably and traceable to one or
more product requirement identifiers; also the document layer in which such statements live.

**Purpose.** Names the layer at which behavior becomes verifiable, between Blueprint and Implementation.

**Authority.** AEOS-PRD.

**Related terms.** Blueprint · Implementation · Document · Repository Asset · Review.

**Notes.** Specifications use RFC 2119 terminology. A specification MUST NOT weaken, reinterpret, or
quietly widen a product requirement; where the two conflict, AEOS-PRD governs.

</details>

<details>
<summary><strong>TDD Cycle</strong> — define behavior, failing test, verified failure reason, minimal implementation, refactor green</summary>

<br>

**Definition.** Define behavior → failing test → verify failure reason → minimal implementation →
refactor green.

**Purpose.** Names the five-position cycle whose current position is tracked and reported.

**Authority.** AEOS-PRD.

**Related terms.** Workflow · Workflow State · Review · Rule · Tool.

**Notes.** Written *TDD Cycle*, capitalized, when naming the AEOS concept. Skipping it is an explicit
exception a human acknowledges; it is never silent and never the default. The cycle applies to AEOS's
own development without exemption.

</details>

<details>
<summary><strong>Technology Stack</strong> — the languages, frameworks, and tools a Project uses, recorded in its Profile</summary>

<br>

**Definition.** The set of languages, frameworks, libraries, build tools, and test tools a Project uses,
recorded descriptively in its Profile.

**Purpose.** Lets a project state what it is built with, so that inspection results and tooling
expectations can be recorded rather than inferred each session.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Profile · Project Type · Tool · Environment.

**Notes.** Descriptive, never prescriptive: AEOS neither supplies a stack nor requires one, and no
capability may depend on a particular technology choice. Recording a technology in a Profile confers no
preference on it.

</details>

<details>
<summary><strong>Template</strong> — a Repository Asset providing a reusable starting point for repeated work</summary>

<br>

**Definition.** A Repository Asset providing a reusable starting point for work a project performs
repeatedly.

**Purpose.** Lets a project capture its own repeated starting points once instead of restating them.

**Authority.** AEOS-PRD.

**Related terms.** Repository Asset · Skill · Prompt · Project Type · Workflow.

**Notes.** Templates are authored and owned by the project. AEOS-supplied workflow templates for common
project archetypes are a recorded recommendation for a future release and are not part of the current
product definition; the two MUST NOT be conflated.

</details>

<details>
<summary><strong>Tool</strong> — an external program or system that performs non-inference work for a Project</summary>

<br>

**Definition.** An external program or system a project depends on that performs no inference: build,
test, version control, packaging, delivery, and comparable systems.

**Purpose.** Separates the systems AEOS orchestrates from the AI runtimes it delegates inference to,
since the two are governed differently.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Runtime · Environment · Platform · Repository · Workflow.

**Notes.** A Tool performs no inference; a Runtime does. AEOS detects tools rather than assuming them,
orchestrates them rather than replacing them, and never removes or reconfigures a tool it did not
install without explicit, specific confirmation.

</details>

<details>
<summary><strong>Vendor</strong> — an organization that supplies a Runtime, Model, or Tool</summary>

<br>

**Definition.** An organization that supplies a Runtime, a Model, or a Tool used with AEOS.

**Purpose.** Names the commercial or organizational source of an external dependency, so that
independence from it can be stated precisely.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Runtime · Model · Tool · Distribution Method.

**Notes.** No Vendor is privileged and no Vendor is required; a vendor's absence reduces the runtimes
available to a user and never disables AEOS. Being named in AEOS documentation confers nothing; being
unnamed excludes nothing. Vendor names MUST NOT appear in definitions, requirements, or asset names.

</details>

<details>
<summary><strong>Vision</strong> — the statement of why AEOS exists and what it must remain</summary>

<br>

**Definition.** The statement of why AEOS exists, what future it intends to serve, and which convictions
must survive every revision of the product; also the document that records it, AEOS-VISION.

**Purpose.** Names the layer above Product, against which a proposed change can be tested for whether it
serves the product's reason for existing.

**Authority.** AEOS-VISION.

**Related terms.** Product · Architecture · Source of Truth · Freeze.

**Notes.** The Vision governs reasoning; AEOS-PRD governs behavior. A statement in the Vision that
appears to grant capability or impose a requirement is a defect in that document. Its invariants are
identified `V1` through `V10`.

</details>

<details>
<summary><strong>Workflow</strong> — a versioned, runtime-independent declaration of steps, preconditions, approval gates, and success criteria</summary>

<br>

**Definition.** A versioned, runtime-independent declaration of engineering steps, preconditions,
approval gates, and success criteria.

**Purpose.** Moves a team's engineering practice out of individual habit and into a reviewable,
portable asset.

**Authority.** AEOS-PRD.

**Related terms.** Workflow Engine · Workflow State · Repository Asset · Engineering Capability · Approval Gate.

**Notes.** Workflows execute unchanged across runtimes; if a workflow, rule, skill, prompt, or repository
must change when the runtime changes, runtime independence has been violated. *Agentic orchestration* is
the sequencing of multi-step work across runtimes with each consequential step held to its approval gate.

</details>

<details>
<summary><strong>Workflow Engine</strong> — the named responsibility for executing Workflow declarations under their Approval Gates</summary>

<br>

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

</details>

<details>
<summary><strong>Workflow State</strong> — the durable record of where a Workflow stands</summary>

<br>

**Definition.** The durable record of where a Workflow currently stands: completed steps, the current
step, outstanding decisions, and the position within any active TDD Cycle.

**Purpose.** Names what makes a workflow inspectable mid-flight, safe to interrupt, and resumable
without re-establishing context.

**Authority.** AEOS-GLOSSARY.

**Related terms.** Workflow · Workflow Engine · Runtime State · Repository · TDD Cycle.

**Notes.** Workflow State is project knowledge and belongs to the repository; **Runtime State** is a
consequence of execution and does not. The two MUST NOT be conflated, and Workflow State MUST NOT be
described as transient.

</details>

</section>

---

<section>

## 4. Terminology Relationships

The diagrams below record how the official terms relate. They are terminology maps, not architecture:
they state which concept is defined in terms of which, and nothing about structure, dependency, or
execution.

> **Reading the arrows.** Each chain states one relationship, given beneath it. An arrow never means
> "calls", "contains at runtime", "is implemented by", or "happens after".

### 4.1 The Definition Chain

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

<table>
<thead>
<tr><th align="left">Relationship</th><th align="left">Statement</th></tr>
</thead>
<tbody>
<tr><td>Vision → Product</td><td>The Product is constrained by the Vision. The Vision governs reasoning; the Product governs behavior.</td></tr>
<tr><td>Product → Architecture</td><td>Architecture decides how the Product is delivered. It decides <em>how</em>, never <em>whether</em>.</td></tr>
<tr><td>Architecture → Blueprint</td><td>A Blueprint expresses a defined portion of the Architecture in realizable form.</td></tr>
<tr><td>Blueprint → Specification</td><td>A Specification states the behavior precisely enough to be tested.</td></tr>
<tr><td>Specification → Implementation</td><td>Implementation realizes the Specification and traces back to product requirement identifiers.</td></tr>
</tbody>
</table>

### 4.2 The Asset Chain

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

<table>
<thead>
<tr><th align="left">Relationship</th><th align="left">Statement</th></tr>
</thead>
<tbody>
<tr><td>Repository → Repository Asset</td><td>The Repository contains all Repository Assets and is their source of truth.</td></tr>
<tr><td>Repository Asset → kinds</td><td>Each kind is a Repository Asset and inherits every property of one: durable, versioned, inspectable, consumable, portable, extensible.</td></tr>
<tr><td>Repository Asset ↔ Runtime State</td><td>Mutually exclusive. Runtime State is never a Repository Asset, and no project requires Runtime State to be understood or reproduced.</td></tr>
</tbody>
</table>

### 4.3 The Work Chain

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

<table>
<thead>
<tr><th align="left">Relationship</th><th align="left">Statement</th></tr>
</thead>
<tbody>
<tr><td>Context → Workflow</td><td>Context is selected per step of a Workflow, minimized deliberately, and explainable element by element.</td></tr>
<tr><td>Workflow → Engineering Capability</td><td>A Workflow step declares the engineering work it requires.</td></tr>
<tr><td>Engineering Capability → Runtime</td><td>A Runtime either provides the required work or does not; the difference is reported before work begins.</td></tr>
<tr><td>Runtime → Model</td><td>A Runtime performs inference using a Model. Neither is part of AEOS.</td></tr>
</tbody>
</table>

### 4.4 The Supervision Chain

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

<table>
<thead>
<tr><th align="left">Relationship</th><th align="left">Statement</th></tr>
</thead>
<tbody>
<tr><td>Action Class → Approval Gate</td><td>The class of an action determines the approval it requires.</td></tr>
<tr><td>Proposal → Human Approval</td><td>Approval attaches to a specific Proposal and to nothing beyond it.</td></tr>
<tr><td>Human Approval → Automation Grant</td><td>A grant delegates approval authority explicitly, scoped and revocably, and never for destructive actions.</td></tr>
<tr><td>Execution → Workflow State</td><td>What actually occurred, including partial completion and failure, is recorded in the project.</td></tr>
</tbody>
</table>

### 4.5 Terms That Are Commonly Confused

<table>
<thead>
<tr><th align="left">These are different</th><th align="left">The distinction</th></tr>
</thead>
<tbody>
<tr><td><strong>Capability</strong> vs <strong>Engineering Capability</strong></td><td>One of the ten product capabilities, versus a unit of work matched between a workflow step and a runtime.</td></tr>
<tr><td><strong>Repository Asset</strong> vs <strong>Runtime State</strong></td><td>Losing it costs product meaning, versus losing it costs only repeated work.</td></tr>
<tr><td><strong>Workflow State</strong> vs <strong>Runtime State</strong></td><td>Durable project knowledge in the repository, versus a transient consequence of execution.</td></tr>
<tr><td><strong>Runtime</strong> vs <strong>Tool</strong></td><td>Performs inference, versus performs no inference.</td></tr>
<tr><td><strong>Runtime</strong> vs <strong>Model</strong></td><td>The external system that exposes inference, versus the model it uses to perform it.</td></tr>
<tr><td><strong>Approval Gate</strong> vs <strong>Human Approval</strong></td><td>The position at which confirmation is required, versus the act that satisfies it.</td></tr>
<tr><td><strong>Review</strong> vs <strong>explanation before execution</strong></td><td>Examination of a result, versus the obligation to be understood before acting. Both are required.</td></tr>
<tr><td><strong>Developer</strong> vs <strong>Contributor</strong></td><td>Uses AEOS on their own project, versus changes AEOS itself.</td></tr>
<tr><td><strong>Architecture</strong> vs <strong>project architecture</strong></td><td>The structure of AEOS, versus the structure of a user's software.</td></tr>
<tr><td><strong>Implementation</strong> vs <strong>project code</strong></td><td>AEOS's own code, versus code written by a Developer in their project.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 5. Naming Conventions

These conventions govern every name introduced in an AEOS repository. They apply to documents, assets,
identifiers, and prose.

### 5.1 General Rules

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>N1</td><td>A name MUST describe what a thing is or does, never how it is built.</td></tr>
<tr><td>N2</td><td>A name MUST NOT contain a vendor, runtime, model, product, or platform name.</td></tr>
<tr><td>N3</td><td>A name MUST NOT encode a version, a date, a status, or an author.</td></tr>
<tr><td>N4</td><td>Abbreviations MUST NOT be invented. Only abbreviations registered in this glossary MAY be used; at present the registered set is <code>AEOS</code>, <code>PRD</code>, <code>TDD</code>, <code>CI/CD</code>, and <code>MCP</code>.</td></tr>
<tr><td>N5</td><td>Names SHOULD be stable. Renaming a published thing follows the deprecation process in <a href="#7-deprecated-terminology">Section 7</a>.</td></tr>
<tr><td>N6</td><td>English SHALL be the language of all names and identifiers.</td></tr>
</tbody>
</table>

### 5.2 Document Names

<table>
<thead>
<tr><th align="left">Aspect</th><th align="left">Convention</th></tr>
</thead>
<tbody>
<tr><td><strong>File name</strong></td><td>Product-level AEOS documents MUST be named <code>AEOS_&lt;NAME&gt;.md</code>, uppercase, words separated by underscores (for example <code>AEOS_PRODUCT_REQUIREMENTS.md</code>). Other documents SHOULD use the repository's prevailing convention for their location.</td></tr>
<tr><td><strong>Document ID</strong></td><td>MUST be <code>AEOS-&lt;NAME&gt;</code>, uppercase, words separated by hyphens (for example <code>AEOS-PRD</code>, <code>AEOS-VISION</code>, <code>AEOS-GLOSSARY</code>). A Document ID MUST be unique and MUST NOT be reused after retirement.</td></tr>
<tr><td><strong>Version</strong></td><td>MUST be semantic versioning, <code>MAJOR.MINOR.PATCH</code>, with the version impact of a change determined by that document's own change control.</td></tr>
<tr><td><strong>Status</strong></td><td>MUST be one of: <code>Draft</code>, <code>Review</code>, <code>Freeze candidate</code>, <code>Frozen</code>, <code>Superseded</code>, <code>Retired</code>.</td></tr>
<tr><td><strong>Header block</strong></td><td>Every AEOS document MUST open with a metadata block stating at least Document, Product, Document ID, Version, Status, Owner, Audience, and Supersedes, followed by a statement of the document's authority.</td></tr>
</tbody>
</table>

### 5.3 Repository Asset Names

<table>
<thead>
<tr><th align="left">Aspect</th><th align="left">Convention</th></tr>
</thead>
<tbody>
<tr><td><strong>Form</strong></td><td>Repository Asset names MUST be lowercase kebab-case (for example <code>test-first-cycle</code>, <code>commit-message-standard</code>).</td></tr>
<tr><td><strong>Content</strong></td><td>A name MUST state the asset's purpose. It MUST NOT state its kind redundantly: an asset of kind Rule is not named <code>rule-…</code> where its kind is already evident from its declaration.</td></tr>
<tr><td><strong>Uniqueness</strong></td><td>Asset names MUST be unique within their kind and scope. Two assets of the same kind MUST NOT share a name.</td></tr>
<tr><td><strong>Versioning</strong></td><td>Versions MUST NOT appear in asset names. Assets are versioned through the repository and their own declared version.</td></tr>
</tbody>
</table>

### 5.4 Identifiers

All AEOS identifiers share one shape:

```text
        <LAYER>-<AREA>-<NNN>

        LAYER   registered layer prefix, uppercase
        AREA    three uppercase letters naming the area, allocated by the owning document
        NNN     three digits, zero-padded, allocated sequentially from 001
```

<table>
<thead>
<tr><th align="left">Prefix</th><th align="left">Identifies</th><th align="left">Owning document</th><th align="left">Example</th></tr>
</thead>
<tbody>
<tr><td><code>PR</code></td><td>Product requirement</td><td>AEOS-PRD</td><td><code>PR-ENV-001</code></td></tr>
<tr><td><code>AR</code></td><td>Architecture decision</td><td>Architecture documents</td><td><code>AR-&lt;AREA&gt;-001</code></td></tr>
<tr><td><code>BP</code></td><td>Blueprint item</td><td>Blueprint documents</td><td><code>BP-&lt;AREA&gt;-001</code></td></tr>
<tr><td><code>SP</code></td><td>Specified behavior</td><td>Specification documents</td><td><code>SP-&lt;AREA&gt;-001</code></td></tr>
<tr><td><code>WF</code></td><td>Workflow</td><td>Workflow assets</td><td><code>WF-&lt;AREA&gt;-001</code></td></tr>
</tbody>
</table>

Identifier rules:

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>I1</td><td>Identifiers MUST be immutable. They are never reused, never renumbered, and never reassigned to different intent.</td></tr>
<tr><td>I2</td><td>A retired item MUST be marked retired in place, retaining its identifier and its rationale.</td></tr>
<tr><td>I3</td><td>An <code>AREA</code> code MUST be registered by the document that introduces it, and MUST NOT be reused across layers with a different meaning.</td></tr>
<tr><td>I4</td><td>New layer prefixes MUST NOT be invented. Adding one is a change to this glossary and requires owner approval.</td></tr>
<tr><td>I5</td><td>Every architecture, blueprint, and specification identifier MUST trace to one or more <code>PR-</code> identifiers.</td></tr>
<tr><td>I6</td><td>Identifiers already in use in frozen documents — the product requirement prefixes <code>PR-ENV</code>, <code>PR-PRJ</code>, <code>PR-WFL</code>, <code>PR-RUN</code>, <code>PR-TDD</code>, <code>PR-DOC</code>, <code>PR-RUL</code>, <code>PR-SKL</code>, <code>PR-PMT</code>, <code>PR-REP</code>, <code>PR-PLT</code>, <code>PR-DST</code>, <code>PR-SAF</code>, <code>PR-NFR</code>, and the short-form series <code>C</code> (capabilities), <code>P</code> (problems), <code>V</code> (invariants), <code>G</code> (guiding principles), <code>R</code> (recommendations) — MUST be preserved exactly as published and MUST NOT be redefined by this or any later document.</td></tr>
</tbody>
</table>

### 5.5 Technology Identifiers

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>X1</td><td>A technology MUST be referred to by its supplier's canonical name, spelled and capitalized as the supplier publishes it.</td></tr>
<tr><td>X2</td><td>Marketing modifiers, edition names, and tiers MUST NOT be used unless they are load-bearing for the statement being made.</td></tr>
<tr><td>X3</td><td>Where a machine-consumable asset must identify a technology, the identifier SHOULD be lowercase kebab-case derived from the canonical name.</td></tr>
<tr><td>X4</td><td>Version references SHOULD use the supplier's published version string, and MUST NOT be paraphrased ("latest", "current", "recent").</td></tr>
<tr><td>X5</td><td>Naming a technology confers no privilege, no requirement, and no support commitment.</td></tr>
</tbody>
</table>

### 5.6 Terms in Prose

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>W1</td><td>A Reserved Term MUST be capitalized when used in its AEOS sense, and MUST be lowercase when used in its ordinary English sense.</td></tr>
<tr><td>W2</td><td>Where a Reserved Term's ordinary sense would be ambiguous, the sentence MUST be qualified instead (for example <em>project architecture</em>, <em>context window</em>, <em>language runtime</em>).</td></tr>
<tr><td>W3</td><td>A document MUST NOT restate a glossary definition. It MAY quote one, attributed to AEOS-GLOSSARY.</td></tr>
<tr><td>W4</td><td>Where a document uses a term this glossary does not define, it MUST either propose the addition or rephrase using defined terms.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 6. Reserved Terms

These words carry a specific, fixed meaning inside AEOS. Their meanings MUST remain stable across every
version of every AEOS document. Where an ordinary English sense is intended, the word MUST be lowercase
and the sentence MUST make the ordinary sense unmistakable.

<table>
<thead>
<tr><th align="left">Reserved Term</th><th align="left">Fixed AEOS meaning</th><th align="left">MUST NOT be used to mean</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Product</strong></td>
<td>What AEOS is and does, as defined by AEOS-PRD; the topmost definition layer beneath the Vision.</td>
<td>A commercial offering, a release, a package, or a feature set.</td>
</tr>
<tr>
<td><strong>Architecture</strong></td>
<td>How AEOS itself is structured to deliver the Product.</td>
<td>The structure of a user's software (write <em>project architecture</em>), or a diagram.</td>
</tr>
<tr>
<td><strong>Capability</strong></td>
<td>One of the ten product capabilities <code>C1</code>–<code>C10</code>.</td>
<td>A feature, a function, a module, or what a runtime can do (write <em>Engineering Capability</em>).</td>
</tr>
<tr>
<td><strong>Workflow</strong></td>
<td>A versioned, runtime-independent declaration of steps, preconditions, gates, and success criteria.</td>
<td>A CI pipeline, a job definition, a habit, or an informal sequence of actions.</td>
</tr>
<tr>
<td><strong>Repository</strong></td>
<td>The version-controlled store that is a project's source of truth.</td>
<td>A package registry, an artifact store, a data store, or a directory.</td>
</tr>
<tr>
<td><strong>Asset</strong></td>
<td>Short form of Repository Asset: a durable, versioned artifact forming part of the product.</td>
<td>A binary, a media file, a build output, or anything transient.</td>
</tr>
<tr>
<td><strong>Runtime</strong></td>
<td>An external AI system that performs inference.</td>
<td>A language runtime, an application runtime, an execution environment, or a Tool.</td>
</tr>
<tr>
<td><strong>Review</strong></td>
<td>Examination of an artifact against requirements, rules, and tests, producing severity-classified findings.</td>
<td>A casual read, an approval, or a retrospective.</td>
</tr>
<tr>
<td><strong>Freeze</strong></td>
<td>The governance state in which content changes only through stated change control.</td>
<td>A technical lock, a feature freeze in the scheduling sense, or an indefinite prohibition on change.</td>
</tr>
<tr>
<td><strong>Context</strong></td>
<td>Information deliberately selected and supplied to a Runtime for a step of work.</td>
<td>A model's input window (write <em>context window</em>), or background information generally.</td>
</tr>
<tr>
<td><strong>Rule</strong></td>
<td>A versioned, scoped engineering constraint applied during generation, review, and refactoring.</td>
<td>A linter configuration, a policy about people, or a convention that is merely preferred.</td>
</tr>
<tr>
<td><strong>Skill</strong></td>
<td>A versioned, reusable, runtime-independent packaged engineering procedure.</td>
<td>A vendor feature of the same name, or a person's ability.</td>
</tr>
<tr>
<td><strong>Prompt</strong></td>
<td>A versioned, parameterized Repository Asset of deliberate instruction and context.</td>
<td>Disposable chat text, or a user interface prompt.</td>
</tr>
<tr>
<td><strong>Profile</strong></td>
<td>The Repository Asset describing what a project is and how it is built.</td>
<td>A user account, a preference set, or performance profiling.</td>
</tr>
<tr>
<td><strong>Platform</strong></td>
<td>A supported host operating system: Windows, macOS, or Linux.</td>
<td>A vendor's AI service, a cloud provider, or a distribution channel.</td>
</tr>
<tr>
<td><strong>Model</strong></td>
<td>A language model, model family, or model version used by a Runtime.</td>
<td>A data model, a domain model, or a mental model.</td>
</tr>
<tr>
<td><strong>Specification</strong></td>
<td>A precise, testable statement of required behavior traceable to a requirement identifier.</td>
<td>A requirement, a design note, or a description of existing behavior.</td>
</tr>
</tbody>
</table>

> **On the strength of "reserved".**
> A reserved meaning is not a preference about style. A document that uses one of these words in a
> different sense states something other than what its author intended, and will be read incorrectly by
> the AI runtimes that maintain this repository. Such usage is a defect and is reported in review.

</section>

---

<section>

## 7. Deprecated Terminology

Definitions are load-bearing for traceability, review, and freeze. A definition that changes quietly
invalidates every document that relied on it, without a version, an approval, or a record. AEOS
therefore never changes a definition in place to mean something different.

### 7.1 Rules

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>D1</td><td>A published definition MUST NOT be edited to mean something different. Editorial correction that preserves meaning is permitted; a change of meaning is not.</td></tr>
<tr><td>D2</td><td>A change of meaning MUST proceed by deprecating the existing term and introducing a new official name with its own definition.</td></tr>
<tr><td>D3</td><td>A deprecated term MUST remain documented, in place, with its original definition intact, its deprecated status, the version in which it was deprecated, the term that supersedes it, and the reason.</td></tr>
<tr><td>D4</td><td>A superseding term's entry MUST reference the term it replaces, so that a reader of either can reach the other.</td></tr>
<tr><td>D5</td><td>A deprecated term MUST NOT be used in new documents. Existing documents retain it until they are revised under their own change control.</td></tr>
<tr><td>D6</td><td>A deprecated name MUST NOT be reused later for a different concept.</td></tr>
<tr><td>D7</td><td>Deprecation is never silent: it MUST appear in the deprecation record and in this document's revision history.</td></tr>
</tbody>
</table>

### 7.2 Deprecation Record

The record below is the complete list of deprecated AEOS terminology. It is empty because no term has
been deprecated: version 1.0.0 is the first publication of AEOS terminology, and all terms in
[Core Terminology](#3-core-terminology) are current.

<table>
<thead>
<tr><th align="left">Deprecated term</th><th align="left">Original definition</th><th align="left">Superseded by</th><th align="left">Deprecated in</th><th align="left">Reason</th></tr>
</thead>
<tbody>
<tr><td colspan="5" align="center"><em>No terms are deprecated as of AEOS-GLOSSARY 1.0.0.</em></td></tr>
</tbody>
</table>

### 7.3 Change Control

<table>
<thead>
<tr><th align="left">Change type</th><th align="left">Requires</th><th align="left">Version impact</th></tr>
</thead>
<tbody>
<tr><td>Editorial correction with no change of meaning</td><td>Contributor change, owner acceptance</td><td>Patch</td></tr>
<tr><td>Clarification of an existing definition that preserves its meaning</td><td>Owner approval</td><td>Minor</td></tr>
<tr><td>Addition of a term, naming convention, or reserved term</td><td>Owner approval</td><td>Minor</td></tr>
<tr><td>Deprecation of a term and introduction of its replacement</td><td>Explicit owner revision request with recorded rationale</td><td>Major</td></tr>
<tr><td>Change to a terminology principle or to an identifier convention</td><td>Explicit owner revision request with recorded rationale</td><td>Major</td></tr>
<tr><td>Removal of a term without replacement</td><td>Explicit owner decision, recorded, with the definition preserved in the deprecation record</td><td>Major</td></tr>
</tbody>
</table>

### 7.4 Precedence

<table>
<thead>
<tr><th align="left">Situation</th><th align="left">Resolution</th></tr>
</thead>
<tbody>
<tr><td>This document conflicts with AEOS-PRD on the meaning of a term</td><td>AEOS-PRD governs. The conflict is a defect in this document and is reported.</td></tr>
<tr><td>This document conflicts with AEOS-VISION on intent</td><td>AEOS-VISION governs for intent, AEOS-PRD for behavior. The conflict is a defect in this document and is reported.</td></tr>
<tr><td>A downstream document defines a term this glossary already defines</td><td>This glossary governs. The downstream definition is removed and replaced by a reference.</td></tr>
<tr><td>A downstream document needs a term this glossary does not define</td><td>The term is proposed for addition here under change control. It is not defined locally.</td></tr>
<tr><td>A term reserved for architecture is cited as authority for product behavior</td><td>The citation is invalid. Product obligation arises only from <code>PR-</code> identifiers.</td></tr>
</tbody>
</table>

### 7.5 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major findings remain.

</section>

---

<section>

## Appendix A — Quick Reference Table

Short definitions for scanning. The canonical definition of every term is its entry in
[Core Terminology](#3-core-terminology); where the two appear to differ, the entry governs.

<table>
<thead>
<tr><th align="left">Term</th><th align="left">Short definition</th><th align="left">Primary document</th></tr>
</thead>
<tbody>
<tr><td><strong>Action Class</strong></td><td>Classification of an action by effect, determining the approval it requires.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>AEOS</strong></td><td>The product: an operating system for AI-assisted, human-supervised software engineering.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Approval Gate</strong></td><td>The point at which a proposed action requires explicit human confirmation.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Architecture</strong></td><td>How AEOS is structured to deliver the Product.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Automation Grant</strong></td><td>Explicit, scoped, recorded, revocable delegation of approval authority.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Blueprint</strong></td><td>Document layer between Architecture and Specification.</td><td>AEOS-GLOSSARY (reserved for architecture)</td></tr>
<tr><td><strong>Bootstrap</strong></td><td>The first, human-approved sequence making AEOS usable in an environment or project.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Capability</strong></td><td>One of the ten product capabilities <code>C1</code>–<code>C10</code>.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Context</strong></td><td>Information deliberately selected and supplied to a Runtime for a step of work.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Context Minimization</strong></td><td>Sending the smallest context sufficient for the task, with each inclusion explainable.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Context Router</strong></td><td>The named responsibility for selecting and justifying a step's Context.</td><td>AEOS-GLOSSARY (reserved for architecture)</td></tr>
<tr><td><strong>Contributor</strong></td><td>A person or AI runtime proposing a change to AEOS itself.</td><td>AEOS-VISION</td></tr>
<tr><td><strong>Developer</strong></td><td>A person who uses AEOS to build or maintain their own project.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Distribution Method</strong></td><td>An official way AEOS is delivered; never changes the product architecture.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Document</strong></td><td>A durable, versioned artifact readable by humans and consumable by AI runtimes.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Engineering Capability</strong></td><td>A unit of engineering work a workflow step requires and a runtime may or may not provide.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Environment</strong></td><td>The machine, its platform, its tooling, and its available runtimes, as observed.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Freeze</strong></td><td>The governance state in which content changes only through stated change control.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Human Approval</strong></td><td>The explicit act authorizing a specific proposed action.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Human-in-the-Loop</strong></td><td>The requirement that a human decides before AEOS acts consequentially.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Implementation</strong></td><td>The code and tests that realize the layers above them.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Model</strong></td><td>A language model, model family, or version used by a Runtime to perform inference.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Platform</strong></td><td>A supported host operating system: Windows, macOS, or Linux.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Product</strong></td><td>What AEOS is and does for its users; the layer beneath the Vision.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Product Boundary</strong></td><td>The line between what AEOS is and how AEOS is built.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Profile</strong></td><td>The asset describing a project's identity, technology, build and test approach, runtime, and rules.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Project</strong></td><td>The unit of work AEOS operates on: a repository and its Repository Assets.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Project Type</strong></td><td>A descriptive classification of a project recorded in its Profile.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Prompt</strong></td><td>A versioned, parameterized asset of deliberate instruction and context.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Proposal</strong></td><td>A statement of intended action, rationale, effects, reversibility, and consequence of declining.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Repository</strong></td><td>The version-controlled store that is a project's source of truth.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Repository Asset</strong></td><td>A durable, versioned artifact forming part of the product.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Review</strong></td><td>Examination against requirements, rules, and tests, producing severity-classified findings.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Rule</strong></td><td>A versioned, scoped engineering constraint applied during generation, review, and refactoring.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Runtime</strong></td><td>An external AI system that performs inference; always an integration.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Runtime Adapter</strong></td><td>The named responsibility mediating between AEOS and one external Runtime.</td><td>AEOS-GLOSSARY (reserved for architecture)</td></tr>
<tr><td><strong>Runtime State</strong></td><td>Transient, machine-local information produced while AEOS runs; not a product asset.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Skill</strong></td><td>A versioned, reusable, runtime-independent packaged engineering procedure.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Source of Truth</strong></td><td>The artifact that governs a subject and wins any conflict about it.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Specification</strong></td><td>A precise, testable statement of required behavior, traceable to a requirement identifier.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>TDD Cycle</strong></td><td>Define behavior → failing test → verify failure reason → minimal implementation → refactor green.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Technology Stack</strong></td><td>The languages, frameworks, and tools a project uses, recorded in its Profile.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Template</strong></td><td>An asset providing a reusable starting point for repeated work.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Tool</strong></td><td>An external program or system that performs non-inference work for a project.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Vendor</strong></td><td>An organization that supplies a Runtime, Model, or Tool.</td><td>AEOS-GLOSSARY</td></tr>
<tr><td><strong>Vision</strong></td><td>Why AEOS exists and what it must remain.</td><td>AEOS-VISION</td></tr>
<tr><td><strong>Workflow</strong></td><td>A versioned, runtime-independent declaration of steps, gates, and success criteria.</td><td>AEOS-PRD</td></tr>
<tr><td><strong>Workflow Engine</strong></td><td>The named responsibility for executing workflows under their approval gates.</td><td>AEOS-GLOSSARY (reserved for architecture)</td></tr>
<tr><td><strong>Workflow State</strong></td><td>The durable record of where a workflow stands.</td><td>AEOS-GLOSSARY</td></tr>
</tbody>
</table>

</section>

---

<section>

## Revision History

<table>
<thead>
<tr><th align="left">Version</th><th align="left">Status</th><th align="left">Summary</th></tr>
</thead>
<tbody>
<tr>
<td>1.0.0</td>
<td>Freeze candidate</td>
<td>Initial terminology definition. Establishes ten terminology principles, forty-nine canonical term entries with recorded authority, four terminology relationship chains and a confusion table, naming conventions for documents, repository assets, identifiers, technology references, and prose, seventeen reserved terms, and the deprecation policy with an empty deprecation record. Introduces no requirement, capability, or architecture. Four terms — Blueprint, Context Router, Runtime Adapter, and Workflow Engine — are recorded as reserved for architecture: the names are fixed, the normative definitions are not made here.</td>
</tr>
</tbody>
</table>

</section>

---

<div align="center">

**End of Glossary**

AEOS-GLOSSARY · Version 1.0.0 · Terminology Source of Truth

</div>
