# AI Engineering Operating System

## AEOS — Runtime Capability Specification

*The permanent statement of the observable Engineering Capability model shared by every Runtime,
Adapter, and Model within AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | Runtime Capability Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-CAP |
| **Version** | 1.0.0 |
| **Status** | Freeze candidate |
| **Owner** | Product Owner, AEOS |
| **Author** | Runtime Governance Board, AEOS |
| **Audience** | Architects, adapter authors, runtime integrators, reviewers, maintainers, and AI runtimes consuming this repository |
| **Suggested path** | `docs/runtime/RUNTIME_CAPABILITY_SPEC.md` |
| **Companion documents** | `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD, structural convention only) · `AEOS_RUNTIME_FLOW.md` (AEOS-RTF) · `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG) · `RUNTIME_ADAPTER_SPEC.md` (AEOS-SPEC-ADP) · `MODEL_REGISTRY.md` (AEOS-SPEC-MDL) |
| **Supersedes** | None |

> **Authority of this document.**
> This document is a **Runtime document**, in the sense AEOS-PRD Section 3.2 names the layer: it
> states what a user and an AI runtime can observe of the Engineering Capability model, and it
> governs the observable behavior and governance of that model — capability identity, ownership,
> taxonomy, declaration, discovery, compatibility, negotiation inputs, validation, evolution,
> deprecation, and retirement — as the single shared vocabulary a Workflow step, a Runtime, an
> Adapter, and a Model each depend upon without any of them owning it.
>
> AEOS-BLUEPRINT Section 10 assigns the neutral vocabulary of Engineering Capability to the Runtime
> Blueprint's Capability Vocabulary subsystem (`BP-RUN-003`), and Section 11 assigns the statement of
> one Runtime's offer in that vocabulary to the Adapter Blueprint's Capability Advertisement
> responsibility (`BP-ADP-003`). Neither assignment states what the vocabulary itself consists of, how
> a term is identified, classified, leveled, validated, extended, deprecated, or retired, or what
> makes one requirement and one offer comparable at all — a Blueprint arrangement is deliberately
> short of that level of detail, per AEOS-BLUEPRINT Section 17.1. This document closes that gap,
> precisely enough to test, without relocating either assignment.
>
> This document is not a Product document, not an Architecture document, not a Blueprint, not a
> behavioral Specification under AEOS-SPECSTD, and not an Implementation Guide. It defines no product
> requirement, no vision, no terminology already owned by AEOS-GLOSSARY, no architectural layer or
> dependency, no arrangement of subsystems, no provider-specific capability name, no implementation
> API, no protocol, no database schema, no runtime algorithm, no provider capability catalog, and no
> technology choice. It redefines no ownership that AEOS-PRD, Architecture, Blueprint, or Specification
> documents already hold, and it does not restate the domain-specific declaration and negotiation
> procedures `RUNTIME_ADAPTER_SPEC.md` (`SP-ADP`) and `MODEL_REGISTRY.md` (`SP-MDL`) already state for
> an Adapter's and a Model's own declarations. Where this document and a document of higher authority
> both speak to a subject, the higher-authority document governs and any conflict here is a defect to
> be reported rather than acted upon.
>
> AEOS-DOCSTD Section 4.5 records that the Runtime layer AEOS-PRD names has not yet been assigned a
> position in the documentation hierarchy, and that until it is, a Runtime document is written to the
> responsibility boundary AEOS-PRD states for it and complies with every rule of AEOS-DOCSTD that does
> not depend on hierarchy position. This document is written under that provision, as
> `AEOS_RUNTIME_FLOW.md` and `RUNTIME_REGISTRY.md` already were. It voluntarily adopts AEOS-SPECSTD's
> structural discipline — precise, testable, `MUST`-level statements; a fixed subject-section shape;
> explicit traceability — for the same reason those two documents give: no Runtime Document Standard
> yet exists to prescribe one, and AEOS-DOCSTD's own principles ask no less rigor of any AEOS document,
> whatever its layer. It is not, on that account, subject to AEOS-SPECSTD's change control, and it
> registers no `SP-` behavior domain.
>
> This document's own rules carry the document-local identifier `CR-<NNN>`. Consistent with the
> precedent `AEOS_RUNTIME_FLOW.md` set for `RTF-<NNN>`, `CR` is not a registered AEOS-GLOSSARY layer
> prefix and makes no such claim; it is a traceability convention internal to this document alone.
> [Section 2.5](#25-identifier-registration) states the reasoning and the relationship to the `CR-`
> trace family `RUNTIME_REGISTRY.md` Section 17.3 already anticipated.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Responsibilities](#3-responsibilities)
4. [Capability Identity](#4-capability-identity)
5. [Capability Ownership](#5-capability-ownership)
6. [Capability Taxonomy](#6-capability-taxonomy)
7. [Capability Declaration](#7-capability-declaration)
8. [Capability Discovery](#8-capability-discovery)
9. [Capability Compatibility](#9-capability-compatibility)
10. [Capability Validation](#10-capability-validation)
11. [Capability Lifecycle](#11-capability-lifecycle)
12. [Relationship to Sibling Runtime Positions](#12-relationship-to-sibling-runtime-positions)
13. [Observable Capability Behavior](#13-observable-capability-behavior)
14. [Inputs](#14-inputs)
15. [Outputs](#15-outputs)
16. [Extension Points](#16-extension-points)
17. [Constraints](#17-constraints)
18. [Non-goals](#18-non-goals)
19. [Traceability](#19-traceability)
20. [References](#20-references)
21. [Document Governance](#21-document-governance)
22. [Appendix A — Capability Rule Index](#appendix-a--capability-rule-index)

---

## 1. Purpose

AEOS orchestrates external Runtimes while remaining independent of every one of them (`PR-RUN-002`,
`PR-RUN-003`, `PR-RUN-006`). Independence of this kind is only real where a Workflow step can state
what engineering work it needs, and a Runtime can state what it offers, in terms that name neither —
and where the two statements can be compared and the difference reported before work begins
(`PR-RUN-007`). AEOS-GLOSSARY names that unit of work **Engineering Capability** and AEOS-BLUEPRINT
assigns the vocabulary that expresses it to the Runtime Blueprint's Capability Vocabulary subsystem
(`BP-RUN-003`), and the statement of an offer in that vocabulary to the Adapter Blueprint's Capability
Advertisement responsibility (`BP-ADP-003`). Both assignments state *where* the vocabulary is held.
Neither states what the vocabulary *is*: how one term is distinguished from another, how terms are
grouped and leveled, what makes a declaration well-formed, what makes a requirement and an offer
comparable, or how a term is added, deprecated, and retired without breaking a project that already
depends on it.

Two documents already depend on that gap being closed without closing it themselves.
`RUNTIME_ADAPTER_SPEC.md` states, precisely and testably, what an Adapter's own capability declaration
and negotiation behavior must do (`SP-ADP-014`–`SP-ADP-027`), and `MODEL_REGISTRY.md` states what a
Model's own classification and declaration behavior must do (`SP-MDL-009`–`SP-MDL-019`) — each written
against a neutral vocabulary neither of them defines. `RUNTIME_REGISTRY.md` records what a registered
Runtime declares in that same vocabulary (`RT-REG-019`–`RT-REG-022`) without defining it either.
`AEOS_RUNTIME_FLOW.md` states that an unsatisfiable capability requirement is reported before a
Workflow step begins (`RTF-019`) without stating what "unsatisfiable" means. Every one of these
documents is correct to defer the question — none of them owns the vocabulary — but the question does
not answer itself.

This document answers it. It states, precisely enough to test, what an Engineering Capability term's
identity is; who may declare against the vocabulary and who may not; how terms are grouped into
categories and leveled; what a well-formed requirement, offer, and constraint look like; how discovery
of the vocabulary behaves; what comparison determines a match, a partial match, or a gap; what a
negotiation needs as input before the procedure `SP-ADP-018`–`SP-ADP-021` already states is carried
out; and how a term evolves, is deprecated, and is retired without invalidating a project that already
depends on it. It states nothing about how any of this is implemented, and nothing about why runtime
independence matters — AEOS-VISION and AEOS-PRD already state that.

A reader of this document gains one fixed model: whatever declares a requirement, whatever declares an
offer, and whatever compares the two, does so against the same rules, regardless of which Runtime,
Vendor, Model, or Adapter is involved.

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the observable Engineering Capability model shared by every Runtime, Adapter,
and Model:

- the identity by which one Engineering Capability term is distinguished from every other;
- the ownership boundary of the vocabulary, and of a declaration made against it;
- the taxonomy — categories and levels — by which terms are organized;
- the declaration model for a capability requirement and a capability offer;
- the discovery behavior of the vocabulary itself;
- the compatibility test by which a requirement and an offer are compared;
- the inputs a negotiation between a requirement and an offer needs;
- the validation that determines whether a declaration is well-formed;
- the lifecycle a term moves through — evolution, deprecation, and retirement;
- the relationship between this model and the Runtime Registry, the Runtime position, the Model
  position, and the Adapter position;
- the inputs this model accepts and the outputs it produces;
- the extension points at which the vocabulary admits addition without modification;
- the boundary between this document and mechanism, technology, protocol, and implementation.

This list is complete.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, and frozen | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| The architectural layers of AEOS, their responsibilities, and their boundaries | AEOS-ARCH |
| The subsystem arrangement of the Runtime Blueprint (`BP-RUN`) and the Adapter Blueprint (`BP-ADP`) | AEOS-BLUEPRINT |
| An Adapter's own capability declaration and negotiation procedure | `RUNTIME_ADAPTER_SPEC.md` (`SP-ADP`) |
| A Model's own classification and declaration procedure | `MODEL_REGISTRY.md` (`SP-MDL`) |
| A registered Runtime's own identity, metadata, and lifecycle | `RUNTIME_REGISTRY.md` |
| The order in which capability negotiation is engaged within a request's lifecycle | `AEOS_RUNTIME_FLOW.md` |
| Whether, and when, a Workflow step's requirement is matched against a Runtime's offer during execution | The Runtime Blueprint's own behavior domain (`BP-RUN-004`), not yet specified |
| Runtime selection, its custody, and boundary disclosure content | The Runtime Blueprint's own behavior domain, not yet specified |
| Whether a workflow step proceeds, and the approval that step requires | The Workflow Blueprint (`BP-WFL`) and the Human Interaction Blueprint (`BP-HUM`) |
| In what process, over what transport, and by what persistence mechanism this model executes | Runtime documents addressing execution mechanics, and Implementation Guides |
| What code realizes any of the above | The codebase and its tests |

A statement in this document that redefines Product ownership, an Architecture Layer, a Blueprint
arrangement, or a sibling document's specified behavior is a **defect in this document**. It MUST be
reported rather than acted upon.

### 2.3 Relationship to Governing and Companion Documents

| Document | Governs | This document's obligation |
| :--- | :--- | :--- |
| **AEOS-VISION** | Purpose, philosophy, invariants `V1`–`V10`. | No statement here may make invariant `V6` — that no vendor, runtime, model, platform, or distribution is privileged or required — unenforceable. |
| **AEOS-PRD** | Product definition, the ten capabilities, and the numbered `PR-` requirements, including `PR-RUN` (AI Runtime Orchestration). | Every behavioral statement here traces to one or more `PR-` identifiers and MUST NOT weaken, reinterpret, or widen any of them. |
| **AEOS-GLOSSARY** | Terminology, including *Engineering Capability*, *Capability*, *Runtime*, *Runtime Adapter*, *Model*, and *Vendor*. | Every term is used exactly as AEOS-GLOSSARY defines it. [Section 2.6](#26-terminology-note--capability-and-engineering-capability) states the one distinction this document depends on most. |
| **AEOS-DOCSTD** | The form, structure, language, and lifecycle of every AEOS document. | This document's structure, normative language, and review classification follow it, applying AEOS-DOCSTD Section 4.5's treatment of Runtime documents. |
| **AEOS-ARCH** | The eight-layer architecture, including the Runtime Layer and the Adapter Layer, and extension point `EP-4`. | This document MUST NOT contradict a structural decision or boundary AEOS-ARCH states, and names no layer, dependency, or interaction it does not already define. |
| **AEOS-BLUEPRINT** | The buildable arrangement of the Runtime Blueprint (`BP-RUN`) and the Adapter Blueprint (`BP-ADP`), and the boundaries stated in its Sections 17 and 18. | This document is written against that arrangement, cites the `BP-` items it depends on, and MUST NOT restate or relocate the arrangement itself. |
| **`RUNTIME_ADAPTER_SPEC.md`** | An Adapter's own capability declaration, negotiation, runtime compatibility, and model compatibility behavior (`SP-ADP-014`–`SP-ADP-027`). | This document supplies the vocabulary those rules already presume, and does not restate them. [Section 2.7](#27-relationship-to-sibling-specification-documents) states the boundary precisely. |
| **`MODEL_REGISTRY.md`** | A Model's own classification and capability declaration behavior (`SP-MDL-009`–`SP-MDL-019`). | As above. |
| **`RUNTIME_REGISTRY.md`** | A registered Runtime's identity, metadata, classification, and lifecycle, including its capability declaration (`RT-REG-019`–`RT-REG-022`). | This document supplies the vocabulary that declaration is expressed in. |
| **`AEOS_RUNTIME_FLOW.md`** | The order in which capability negotiation is engaged during a request's lifecycle (`RTF-018`, `RTF-019`). | This document states what "satisfiable" and "unsatisfiable" mean; it does not state when that determination is made. |

AEOS-SPECSTD governs the Specification layer only, identified by documents carrying an `SP-`
identifier. This document is not such a document: it carries no `SP-` identifier and is not subject to
AEOS-SPECSTD's change control. Its structure borrows AEOS-SPECSTD's section skeleton — Purpose, Scope,
Responsibilities, Inputs, Outputs, behavioral content, Constraints, Extension Points, Traceability,
Non-goals, References — as a stylistic convention appropriate to a document that states testable,
observable behavior, for the same reason `AEOS_RUNTIME_FLOW.md` and `RUNTIME_REGISTRY.md` already give.
Where this document's structure diverges from AEOS-SPECSTD's, the divergence is a stylistic choice made
under this document's own authority, not a defect against AEOS-SPECSTD.

### 2.4 Position in the Documentation Hierarchy

> This document is written to the responsibility boundary AEOS-PRD Section 3.2 states for Runtime
> documents — "states what users observe, never what executes" — and complies with every rule in
> AEOS-DOCSTD that does not depend on hierarchy position, per AEOS-DOCSTD Section 4.5. It claims no
> position above Architecture, no position above Blueprint, and no position within the Specification
> layer.

AEOS-BLUEPRINT Section 18.1 already states how a conflict between this layer and the Blueprint
resolves: the Blueprint governs the arrangement of the Runtime Blueprint and the Adapter Blueprint;
this document governs the observable behavior of the capability model within that arrangement. Where
the two cannot both hold, the conflict is escalated to the owner rather than resolved locally.

### 2.5 Identifier Registration

AEOS-GLOSSARY Section 6.4 registers the identifier shape `<LAYER>-<AREA>-<NNN>` and the layer prefixes
`PR`, `AR`, `BP`, `SP`, and `WF`. It reserves the introduction of a new layer prefix to Glossary
governance, under rule `I4`.

`RUNTIME_REGISTRY.md` Section 17.3 already recorded that no document in this repository traced to
`SP-`, `RF-`, `RA-`, `MR-`, or `CR-` identifiers at the time it was written, and recorded that a
"Capability Registry" document, had one existed, would have carried the `CR-` trace family. Three of
those anticipated documents were since authored under different, more descriptive names: `RF-` became
`RTF-` (`AEOS_RUNTIME_FLOW.md`), `RA-` was absorbed into `SP-ADP` (`RUNTIME_ADAPTER_SPEC.md`), and
`MR-` was absorbed into `SP-MDL` (`MODEL_REGISTRY.md`). This document is the fourth: it fulfills the
`CR-` anticipation, under the descriptive title *Runtime Capability Specification* rather than
*Capability Registry* — a naming difference only, not a claim to a different identity than the one
already anticipated.

Consistent with the reasoning `AEOS_RUNTIME_FLOW.md` gives for its own `RTF-<NNN>` identifier, this
document does not propose `CR` as a new AEOS-GLOSSARY layer prefix under `I4`. It uses `CR-<NNN>` as a
document-local traceability convention only, allocated sequentially from `001` across
[Section 13](#13-observable-capability-behavior) and [Section 17](#17-constraints), and never reused,
renumbered, or reassigned, consistent with AEOS-GLOSSARY `I1` and `I2`. Every `CR-` identifier traces
to one or more `PR-`, `AR-`, or `BP-` identifiers, per [Section 19](#19-traceability).

### 2.6 Terminology Note — Capability and Engineering Capability

AEOS-GLOSSARY defines **Capability** as one of the ten product capabilities `C1`–`C10`, and
**Engineering Capability** as a discrete unit of engineering work a Workflow step requires and a
selected Runtime may or may not perform. The two terms are not interchangeable, and AEOS-GLOSSARY
states explicitly that a document MUST NOT substitute one for the other.

Every use of the word "capability" in this document's title and normative statements — capability
identity, capability declaration, capability offer, capability requirement, capability category,
capability level, capability constraint — is a descriptive compound built from **Engineering
Capability**, exactly as `MODEL_REGISTRY.md` and `RUNTIME_ADAPTER_SPEC.md` already use "capability
declaration," "capability negotiation," and "capability query" without redefining the term or
proposing a new one. This document introduces no new AEOS-GLOSSARY Reserved Term.

### 2.7 Relationship to Sibling Specification Documents

`RUNTIME_ADAPTER_SPEC.md` states, as testable rules within its own `ADP` behavior domain, that an
Adapter's declaration MUST use only the neutral vocabulary (`SP-ADP-014`), MUST NOT assert or omit a
capability inaccurately (`SP-ADP-015`), and MUST complete negotiation before dispatch (`SP-ADP-019`).
`MODEL_REGISTRY.md` states, within its own `MDL` behavior domain, that a Model's declared set MUST use
only the neutral vocabulary (`SP-MDL-012`), MUST be attributable to one declaring Adapter
(`SP-MDL-013`), and MUST be classified using only registered category terms (`SP-MDL-009`). Neither
document states what the neutral vocabulary itself consists of; both presuppose it.

This document supplies what they presuppose. Where a rule in this document and a rule in `SP-ADP` or
`SP-MDL` appear to overlap, the two operate at different levels and neither restates the other:

| Level | Question answered | Owned by |
| :--- | :--- | :--- |
| The vocabulary itself | What is a well-formed Engineering Capability term, category, level, and constraint, and how are two declarations compared? | This document |
| A domain's own declaration behavior | What must an Adapter's declaration, or a Model's declaration, do, procedurally, within its own domain? | `SP-ADP`, `SP-MDL` |

This document MUST NOT be read as amending, weakening, or superseding `SP-ADP-014`–`SP-ADP-027` or
`SP-MDL-009`–`SP-MDL-019`, and neither of those documents' rules amends, weakens, or supersedes this
one. A genuine conflict between this document and either — a case where the same declaration could
conform to one and not the other — is a defect, reported and resolved under
[Section 21.5](#215-precedence), never resolved locally by a contributor's preference.

---

## 3. Responsibilities

### 3.1 What This Document Is Answerable For

This document is answerable for: the identity of an Engineering Capability term; the ownership
boundary between the vocabulary and a declaration made against it; the taxonomy of categories and
levels; the well-formedness of a capability requirement, offer, and constraint; the discovery behavior
of the vocabulary; the compatibility test and the inputs a negotiation needs before it is carried out;
the validation that admits or rejects a declaration; and the evolution, deprecation, and retirement
lifecycle of a term.

### 3.2 What This Document Is Not Answerable For

| Question | Answered by |
| :--- | :--- |
| Whether, and when, a Workflow step's requirement is compared against a Runtime's offer during execution | The Runtime Blueprint's own behavior domain (`BP-RUN-004`), not yet specified |
| What an Adapter's own declaration and negotiation procedure must do | `RUNTIME_ADAPTER_SPEC.md` |
| What a Model's own classification and declaration procedure must do | `MODEL_REGISTRY.md` |
| What a registered Runtime's identity and lifecycle must be | `RUNTIME_REGISTRY.md` |
| The order in which capability negotiation is engaged in a request's lifecycle | `AEOS_RUNTIME_FLOW.md` |
| Runtime selection, its custody, and boundary disclosure content | The Runtime Blueprint's own behavior domain, not yet specified |
| How this model is realized in code | Implementation Guides, once written against this document |
| Why runtime independence and capability neutrality are required | AEOS-VISION and AEOS-PRD |

### 3.3 The Responsibility Test

Every statement in [Section 13](#13-observable-capability-behavior) and
[Section 17](#17-constraints) was tested before inclusion:

> **Is this statement a testable fact about the shared capability model, traceable to a `PR-`, `AR-`,
> or `BP-` identifier, and free of mechanism, technology, protocol, and provider-specific naming?**
> A statement that failed this test was rewritten as observable behavior or removed rather than
> included for completeness.

---

## 4. Capability Identity

An Engineering Capability term's identity is the stable, self-contained property by which it is
recognized, declared against, and compared — independent of who requires it and independent of who
offers it. Identity is what lets a Workflow step declared against one Adapter's mediated Runtime today
be re-evaluated against a different Adapter's mediated Runtime tomorrow, without either declaration
changing, satisfying `PR-RUN-005` and `PR-RUN-008`.

Identity is not a name chosen for convenience. A term's identity MUST be expressible without naming a
Vendor, a Runtime, or a Model, consistent with the neutrality AEOS-BLUEPRINT already requires of the
vocabulary that holds it (`BP-RUN-001`, `AR-BND-013`). Two terms that describe the same underlying
engineering work but are spelled differently are two identities, not one, until the vocabulary's own
evolution rules ([Section 11.1](#111-evolution)) reconcile them.

Identity is also where this document draws its sharpest line against a term AEOS-GLOSSARY reserves for
a different purpose: an Engineering Capability's identity MUST NOT be confused with, aliased to, or
expressed in terms of one of the ten product capabilities `C1`–`C10`, per the distinction
[Section 2.6](#26-terminology-note--capability-and-engineering-capability) states.

The normative rules of capability identity are stated in [Section 13.1](#131-capability-identity-rules).

---

## 5. Capability Ownership

Two different things can be owned here, and this document is careful never to let the second be
mistaken for the first. The **vocabulary** — the complete set of admitted terms, categories, and
levels — is held by exactly one position: the Runtime Blueprint's Capability Vocabulary subsystem
(`BP-RUN-003`). This document states the vocabulary's observable behavior; it does not relocate its
custody. A **declaration** made against the vocabulary — a Workflow step's requirement, or an Adapter's
offer — is a different thing, owned by the party that made it: a requirement is attributable to exactly
one declaring Workflow step, and an offer is attributable to exactly one declaring Adapter
(`AR-BND-005`, `BP-ADP-001`, `BP-ADP-002`).

No third party — not this document, not a reviewer, not another Adapter — may alter a declaration it
did not make. Ownership of the vocabulary confers no authority to edit a declaration; ownership of a
declaration confers no authority to redefine a term in the vocabulary. The two ownership boundaries are
independent and this document enforces both without merging them.

The normative rules of capability ownership are stated in [Section 13.2](#132-capability-ownership-rules).

---

## 6. Capability Taxonomy

### 6.1 Categories

A category is a named grouping of related Engineering Capability terms within the vocabulary. Every
admitted term belongs to at least one category, so that a discovery request, a report, or a human
reviewer can navigate the vocabulary by kind of work rather than by an unordered list of terms.
Categories are themselves part of the neutral vocabulary: a category MUST be defined without naming a
Vendor, Runtime, or Model, exactly as `MODEL_REGISTRY.md` `SP-MDL-019` already requires for a Model's
own classification against one.

### 6.2 Levels

A level expresses degree or scope of support for a term without changing the term's identity — for
example, that an offer satisfies a term fully or only in part. A level is optional: a term MAY be
declared without one, in which case full support is assumed rather than treated as an unstated gap.
Where a term does declare levels, the ordering between them MUST itself be stated in the vocabulary,
never inferred by whichever party is comparing a requirement to an offer.

The normative rules of capability taxonomy are stated in [Section 13.3](#133-capability-taxonomy-rules).

---

## 7. Capability Declaration

A declaration is a statement made against the vocabulary, and this document recognizes exactly two
kinds, corresponding to the two sides AEOS-GLOSSARY's definition of Engineering Capability already
names: a **requirement**, declared by a Workflow step, states what engineering work it needs; an
**offer**, declared by an Adapter, states what its mediated Runtime provides. Both are expressed in
identical terms drawn from the same vocabulary, which is what makes the two comparable at all
(`PR-RUN-007`).

This document states what makes either kind of declaration well-formed: composed only of admitted
terms, carrying at most one level per term, and attributable to exactly one declaring party. It does
not state the procedure by which an Adapter arrives at its offer or a Workflow step arrives at its
requirement — that procedure, for an Adapter, is `SP-ADP-014`–`SP-ADP-021`; for a Workflow step, it is
a Workflow Blueprint concern outside this document's scope, per
[Section 18](#18-non-goals).

The normative rules of capability declaration are stated in [Section 13.4](#134-capability-declaration-rules).

---

## 8. Capability Discovery

Discovery is how the vocabulary itself — its admitted terms, their categories, and their declared
levels — becomes enumerable on request, independent of any particular requirement or offer. Discovery
is distinct from the discovery `RUNTIME_ADAPTER_SPEC.md` `SP-ADP-011`–`SP-ADP-013` already states for
an Adapter becoming visible as a candidate, and distinct from the discovery `RUNTIME_REGISTRY.md`
Section 10.2 already states for a registered Runtime becoming enumerable: both of those discover
*declarations*; this document's discovery surfaces the *vocabulary* those declarations are expressed
against.

Discovery is a read operation. It never alters the vocabulary, and its result distinguishes a term that
is admitted but not currently offered by any reachable Runtime from a term that has never been admitted
at all — the same distinction `MODEL_REGISTRY.md` `SP-MDL-041` already draws for a capability query
answered against registered Models.

The normative rules of capability discovery are stated in [Section 13.5](#135-capability-discovery-rules).

---

## 9. Capability Compatibility

### 9.1 The Compatibility Test

The compatibility test is the comparison by which a requirement and an offer are determined to match,
partially match, or fail to match. The test operates on identity and, where declared, on level: a
requirement and an offer are compatible only where their terms share one identity, and where any
required level is met or exceeded by the offered level. The test produces exactly one of three
outcomes — satisfied, partially satisfied, or not satisfied — and produces the same outcome every time
it is applied to the same requirement and the same offer, regardless of which Adapter, Runtime, or
Model declared the offer (`AR-BND-011`).

### 9.2 Capability Constraint

A capability constraint is a condition an offer declares under which it holds — for example, a version
range or a Platform range. A constraint is evaluated as part of the compatibility test: an offer that
fails its own declared constraint is treated as not satisfying the requirement it would otherwise match.
A constraint MUST be stated as an observable condition, never as an assumption, mirroring the discipline
`SP-ADP-022` already requires of an Adapter's own Runtime compatibility declaration.

### 9.3 Capability Negotiation Inputs

Negotiation is the exchange `SP-ADP-018`–`SP-ADP-021` already specifies, by which a step's requirement
and an Adapter's offer are compared and a match or a gap is stated before work begins. This document
does not restate that procedure; it states what the procedure needs before it can run: the required
term and its level, the offered term and its level, and any capability constraint declared on the
offer. Nothing beyond those three is required as a negotiation input, and negotiation MUST NOT be made
to depend on information the declaring Adapter or Workflow step has not already declared, consistent
with the minimum-sufficient-crossing principle AEOS-ARCH Section 6.3 already states.

The normative rules of capability compatibility are stated in [Section 13.6](#136-capability-compatibility-rules).

---

## 10. Capability Validation

Validation determines whether a term, category, level, requirement, offer, or constraint is
well-formed under the rules this document states. Validation is not negotiation: it asks whether a
single declaration conforms to the vocabulary's own shape, not whether two declarations match each
other. A declaration that fails validation is rejected and the rejection is reported to the declaring
party; it is never silently discarded or silently corrected on the declaring party's behalf.

Validation is answerable without invoking any Runtime, consistent with `AR-BND-002`'s prohibition on
AEOS performing inference, and it never alters a declaration other than the one being validated.

The normative rules of capability validation are stated in [Section 13.7](#137-capability-validation-rules).

---

## 11. Capability Lifecycle

A term in the vocabulary moves through three observable stages once admitted. The stages describe what
can be observed of a term; they name no process, no thread, and no storage mechanism by which a
transition is carried out.

| Stage | What is observably true |
| :--- | :--- |
| **Evolving** | The term is admitted and available for new requirements and offers; addition of related terms, categories, or levels does not require it to change. |
| **Deprecated** | The term remains usable by a requirement or an offer already declared against it, but is disclosed as marked for eventual retirement before any new declaration is made against it. |
| **Retired** | The term is no longer available to satisfy any new requirement, and is marked retired in place rather than removed from the vocabulary's record. |

### 11.1 Evolution

Evolution is the addition of a new term, category, or level to the vocabulary, or a new level scale for
an existing term. Every addition is admitted without altering the meaning of anything already admitted,
consistent with AEOS-ARCH `AR-PRN-009` ("extension over modification") and without requiring a change
to an Adapter's or a Model's existing declaration (`PR-RUN-012`). Evolution attaches only at the
extension point [Section 16](#16-extension-points) names.

### 11.2 Deprecation

Deprecation marks a term for eventual retirement without removing its current usefulness. A deprecated
term continues to participate in the compatibility test exactly as before, and continues to satisfy any
requirement or offer already declared against it. What changes is disclosure: a new declaration against
a deprecated term MUST be told so before it is made, and the reason for the deprecation is recorded
alongside the term.

### 11.3 Retirement

Retirement removes a term's availability to any *new* requirement or offer while leaving the historical
record — including any Workflow outcome that depended on the term before retirement — untouched
(`PR-REP-015`, `PR-REP-016`). A retired term is marked retired in place, retaining its identifier and
the reason for retirement, consistent with AEOS-GLOSSARY `I2`. The retirement of one term reduces the
vocabulary's reach only, and MUST NOT disable a capability that does not depend on the retired term,
consistent with AEOS-ARCH `AR-PRN-008`.

The normative rules of the capability lifecycle are stated in
[Sections 13.8](#138-capability-evolution-rules) through
[13.10](#1310-capability-retirement-rules).

---

## 12. Relationship to Sibling Runtime Positions

This document does not exist beside the rest of the Runtime layer; it exists underneath it. Four
positions already depend on the model this document states, and none of them is altered by it.

### 12.1 Registry Relationship

`RUNTIME_REGISTRY.md` records, for each registered Runtime, the Engineering Capabilities it declares
(`RT-REG-019`–`RT-REG-022`), expressed in the vocabulary this document states. This document does not
restate the Registry's own recording, discovery, or lookup behavior; it supplies the terms that
behavior is expressed in. Where the Registry's own rules and this document's model appear to overlap,
this document governs the vocabulary and the Registry governs what it does with a Runtime's declaration
against that vocabulary.

### 12.2 Runtime Relationship

The Runtime Blueprint's Capability Matching subsystem is answerable, at execution time, for comparing a
step's requirement against a selected Runtime's currently declared offer (`BP-RUN-004`). This document
states the compatibility test that comparison MUST use as its own; it does not state when, during a
request's lifecycle, that comparison is performed — `AEOS_RUNTIME_FLOW.md` `RTF-014` through `RTF-017`
already state that ordering, and this document does not restate it.

### 12.3 Model Relationship

`MODEL_REGISTRY.md` states that a Model's declared Engineering Capability set MUST be expressed only in
the neutral vocabulary (`SP-MDL-012`) and classified using only registered category terms (`SP-MDL-009`).
This document supplies the vocabulary and the category structure those rules already presume, and does
not restate `MODEL_REGISTRY.md`'s own registration, revision, or query behavior.

### 12.4 Adapter Relationship

`RUNTIME_ADAPTER_SPEC.md` states that an Adapter's capability offer, negotiation, runtime compatibility,
and model compatibility declarations MUST be expressed in the neutral vocabulary
(`SP-ADP-014`–`SP-ADP-027`). This document supplies that vocabulary and the compatibility test those
declarations are compared under, and does not restate the Adapter's own declaration or negotiation
procedure.

The normative rules of these relationships are stated in
[Section 13.11](#1311-relationship-rules).

---

## 13. Observable Capability Behavior

The normative rules of this document. Identifiers are allocated sequentially from `001` across this
section and [Section 17](#17-constraints), per [Section 2.5](#25-identifier-registration).

### 13.1 Capability Identity Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-001` | An Engineering Capability term's identity MUST be stable and independent of the Runtime, Vendor, Model, or Adapter that offers or requires it. | `PR-RUN-002` · `PR-RUN-005` · `PR-RUN-006` · `AR-BND-005` · `AR-BND-013` · `BP-RUN-003` |
| `CR-002` | An Engineering Capability term, once admitted to the vocabulary, MUST NOT be reused for a different meaning. | `PR-NFR-001` · `AR-GOV-001` |
| `CR-003` | An Engineering Capability term's identity MUST be expressible without naming a technology, protocol, or interface. | `PR-RUN-002` · `AR-BND-013` |
| `CR-004` | The identity of an Engineering Capability term MUST be distinguishable from the identity of a product Capability `C1`–`C10`, per AEOS-GLOSSARY's reservation of the two terms. | `PR-NFR-009` |
| `CR-005` | Two Engineering Capability terms with different identities MUST NOT be treated as interchangeable by any declaration or comparison this document governs. | `PR-RUN-008` · `BP-RUN-013` |

### 13.2 Capability Ownership Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-006` | The vocabulary of Engineering Capability terms MUST be held only by the position AEOS-BLUEPRINT assigns to it, and this document MUST NOT be read as relocating that custody. | `BP-RUN-003` · `BP-GOV-009` |
| `CR-007` | An Engineering Capability offer MUST be attributable to exactly one declaring Adapter. | `AR-BND-005` · `BP-ADP-001` · `BP-ADP-002` |
| `CR-008` | An Engineering Capability requirement MUST be attributable to exactly one declaring Workflow step, and MUST NOT be attributed to the vocabulary itself. | `PR-WFL-016` · `AR-PRN-006` |
| `CR-009` | No party other than the declaring Adapter or the declaring Workflow step MAY alter a declaration it did not make. | `AR-BND-005` · `BP-ADP-007` |
| `CR-010` | This document MUST NOT be read as granting itself, or any other position, authority to relocate a responsibility AEOS-BLUEPRINT already assigns. | `BP-GOV-009` · `BP-GOV-010` |

### 13.3 Capability Taxonomy Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-011` | An Engineering Capability term MUST belong to at least one category within the vocabulary. | `PR-RUN-007` · `BP-RUN-003` |
| `CR-012` | A category MUST be defined without naming a Vendor, Runtime, or Model. | `AR-BND-013` · `PR-RUN-005` |
| `CR-013` | The admission of a new category MUST NOT alter the meaning of a term already admitted under a different category. | `AR-PRN-009` · `PR-NFR-007` |
| `CR-014` | Where this document's taxonomy and a domain-specific document's category rule (for example, `MODEL_REGISTRY.md` `SP-MDL-017`–`SP-MDL-019`) both speak to categories, the domain-specific rule governs declaration behavior within its own domain and this document governs the structure of the vocabulary itself; neither restates the other. | `PR-NFR-001` · `BP-GOV-009` |
| `CR-015` | An Engineering Capability term MAY declare a level, expressing degree or scope of support, without altering the term's identity. | `PR-RUN-007` · `PR-RUN-008` |
| `CR-016` | A level MUST be expressed within the vocabulary and MUST NOT be expressed in a Vendor's own terms. | `AR-BND-013` · `BP-RUN-003` |
| `CR-017` | The absence of a declared level for a term MUST be treated as full support, not as an unstated gap. | `PR-SAF-011` · `AR-PRN-007` |

### 13.4 Capability Declaration Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-018` | A capability requirement MUST be expressed using only terms already admitted to the vocabulary, or terms admitted at the same time under [Section 11.1](#111-evolution). | `PR-RUN-007` · `BP-RUN-003` |
| `CR-019` | A capability requirement MUST be stated before the Workflow step that depends on it begins. | `PR-WFL-016` · `RTF-018` |
| `CR-020` | A capability offer MUST be expressed using only terms already admitted to the vocabulary. | `PR-RUN-007` · `BP-ADP-003` |
| `CR-021` | A capability offer MUST be well-formed: composed only of admitted terms, carrying at most one level per term, and attributable to exactly one declaring Adapter. | `BP-ADP-001` · `BP-ADP-003` |
| `CR-022` | A revision to a capability requirement or a capability offer MUST be distinguishable from its initial declaration. | `PR-NFR-001` · `PR-NFR-002` |
| `CR-023` | A capability requirement MUST NOT be satisfied by inference; satisfaction MUST be determined only by the compatibility test in [Section 13.6](#136-capability-compatibility-rules). | `AR-BND-002` · `PR-RUN-001` |
| `CR-024` | A capability offer's persistence in the vocabulary's record MUST be independent of the current reachability of the Runtime it describes. | `BP-RUN-011` · `PR-RUN-010` |
| `CR-025` | This document's declaration model MUST NOT be read as restating or superseding the declaration procedures a domain-specific document already states for its own domain, including `SP-ADP-014`–`SP-ADP-021` and `SP-MDL-012`–`SP-MDL-019`; this document states the shared model those procedures presuppose. | `BP-GOV-009` · `BP-GOV-010` |

### 13.5 Capability Discovery Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-026` | The vocabulary MUST be enumerable on request, independent of any declared offer or requirement. | `AR-PRN-007` · `PR-ENV-004` |
| `CR-027` | A term's category and, where declared, its level MUST be reportable together with the term. | `PR-SAF-011` · `AR-PRN-007` |
| `CR-028` | Discovery of the vocabulary MUST NOT alter the vocabulary. | `PR-ENV-011` |
| `CR-029` | Discovery MUST distinguish a term that is admitted but not currently offered by any reachable Runtime from a term that has never been admitted. | `PR-SAF-011` · `BP-RUN-004` |
| `CR-030` | The absence of a discoverable offer for a required term MUST reduce the set of selectable Runtimes and MUST NOT be reported as a failure of a capability that does not depend on it. | `PR-RUN-010` · `AR-PRN-008` |

### 13.6 Capability Compatibility Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-031` | A capability requirement and a capability offer MUST be compared using identity only: a match exists only where the required term's identity and the offered term's identity are the same. | `PR-RUN-007` · `PR-RUN-008` |
| `CR-032` | Where a requirement declares a level, an offer MUST satisfy it only where the offer's declared level meets or exceeds the required level, using an ordering the vocabulary itself states. | `PR-RUN-007` · `PR-RUN-008` |
| `CR-033` | The compatibility test MUST produce exactly one of three outcomes for a given requirement and offer: satisfied, partially satisfied, or not satisfied. | `PR-SAF-011` · `PR-NFR-001` |
| `CR-034` | A partially satisfied outcome MUST be reported as such and MUST NOT be reported as a satisfied outcome. | `PR-RUN-008` · `PR-SAF-011` |
| `CR-035` | A capability constraint declared on an offer MUST be evaluated as part of the compatibility test, and an offer failing its own declared constraint MUST be treated as not satisfying the requirement it would otherwise match. | `PR-ENV-004` · `PR-PLT-003` |
| `CR-036` | A capability constraint MUST be expressed as an observable condition, never as an assumption. | `PR-SAF-011` · `AR-PRN-007` |
| `CR-037` | The inputs a negotiation between a requirement and an offer needs MUST be limited to the required term and its level, the offered term and its level, and any capability constraint declared on the offer. | `PR-RUN-007` · `PR-SAF-008` |
| `CR-038` | A negotiation input MUST be available before the negotiation procedure a domain-specific document states (for example, `SP-ADP-018`–`SP-ADP-021`) is carried out; this document states the inputs, not the procedure. | `BP-RUN-004` · `BP-GOV-010` |
| `CR-039` | The compatibility test's result for one requirement and one offer MUST be reproducible: the same requirement and the same offer produce the same result whenever compared. | `PR-RUN-008` · `PR-NFR-001` |
| `CR-040` | The compatibility test MUST NOT depend on which Adapter, Runtime, or Model declares the offer being compared. | `AR-BND-011` · `PR-RUN-003` |

### 13.7 Capability Validation Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-041` | A term, category, level, requirement, offer, or constraint MUST be rejected as invalid where it fails the well-formedness rules this document states. | `PR-SAF-011` · `PR-NFR-001` |
| `CR-042` | A rejected declaration MUST be reported to the declaring party rather than silently discarded. | `PR-SAF-011` · `AR-PRN-007` |
| `CR-043` | Validation MUST occur without invoking any Runtime. | `AR-BND-002` · `AR-EXT-005` |
| `CR-044` | Validation MUST NOT alter a declaration that was not itself the subject of the validation performed. | `PR-ENV-011` · `AR-LAY-006` |
| `CR-045` | A term, category, or level that fails validation MUST NOT be admitted to the vocabulary. | `PR-NFR-001` · `PR-SAF-011` |

### 13.8 Capability Evolution Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-046` | A new Engineering Capability term, category, or level MUST be admitted to the vocabulary as an addition, without requiring a change to a term, category, or level already admitted. | `AR-PRN-009` · `AR-EXT-001` · `PR-RUN-012` |
| `CR-047` | A revision to what an admitted term, category, or level means MUST NOT narrow or widen its meaning silently; a meaning change MUST be admitted as a new term rather than a redefinition of an existing one. | `PR-NFR-001` · `AR-GOV-006` |
| `CR-048` | Evolution of the vocabulary MUST NOT require modifying an Adapter's or a Model's existing declaration. | `PR-RUN-012` · `BP-ADP-005` |
| `CR-049` | Evolution of the vocabulary MUST attach only at the extension point [Section 16](#16-extension-points) names, and MUST NOT introduce a new position, dependency, or layer. | `AR-EXT-001` · `AR-EXT-004` |

### 13.9 Capability Deprecation Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-050` | A term MAY be marked deprecated without being retired; a deprecated term MUST remain usable by a requirement or an offer already declared against it. | `PR-SAF-010` · `AR-PRN-008` |
| `CR-051` | A deprecation MUST be disclosed before a new requirement or offer is declared against the deprecated term. | `PR-SAF-011` · `AR-PRN-007` |
| `CR-052` | A deprecation MUST record the reason it was made, retained alongside the term. | `PR-NFR-001` · `AR-GOV-006` |
| `CR-053` | A deprecated term MUST continue to participate in the compatibility test exactly as an undeprecated term does, until it is retired. | `PR-RUN-010` · `PR-SAF-010` |

### 13.10 Capability Retirement Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-054` | A retired term MUST NOT be admitted as satisfying any new requirement declared after retirement. | `PR-NFR-001` · `AR-GOV-006` |
| `CR-055` | A retired term MUST be marked retired in place, retaining its identifier and the reason for retirement; it MUST NOT be removed from the vocabulary's record. | `AR-GOV-006` · `PR-NFR-001` |
| `CR-056` | Retirement of a term MUST NOT retroactively invalidate a Workflow outcome that depended on it before retirement. | `PR-REP-015` · `PR-REP-016` · `PR-SAF-010` |
| `CR-057` | The retirement of one term MUST reduce the vocabulary's reach only, and MUST NOT disable a capability that does not depend on the retired term. | `AR-PRN-008` · `PR-RUN-010` |

### 13.11 Relationship Rules

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CR-058` | The Runtime Registry's declaration and lookup of an Engineering Capability MUST be expressed in the vocabulary this document states, and MUST NOT introduce a term outside it. | `BP-RUN-011` · `PR-RUN-007` |
| `CR-059` | This document MUST NOT restate a rule `RUNTIME_REGISTRY.md` already states about a registered Runtime's own declaration (for example, `RT-REG-019`–`RT-REG-022`); it supplies the vocabulary that declaration is expressed in. | `BP-GOV-009` |
| `CR-060` | The Runtime Blueprint's Capability Matching responsibility (`BP-RUN-004`) MUST use the compatibility test this document states as its own; this document does not state when, in a Workflow step's execution, that test is applied. | `BP-RUN-004` · `PR-RUN-007` |
| `CR-061` | A Model's declared Engineering Capability set MUST be expressed in the vocabulary this document states; this document does not state the registration behavior `MODEL_REGISTRY.md` already states for a Model's own declaration (`SP-MDL-012`). | `BP-GOV-009` |
| `CR-062` | A Model's classification into a category MUST use only a category this document admits. | `BP-RUN-003` |
| `CR-063` | An Adapter's capability offer MUST be expressed in the vocabulary this document states; this document does not state the declaration and negotiation procedure `RUNTIME_ADAPTER_SPEC.md` already states for an Adapter's own behavior (`SP-ADP-014`). | `BP-GOV-009` |
| `CR-064` | An Adapter's runtime and model compatibility declarations MUST be expressed as capability constraints in the sense this document states, and MUST NOT introduce a comparison mechanism this document does not admit. | `BP-ADP-001` |
| `CR-065` | Where a sibling Runtime or Specification document's capability-related rule and this document's model appear to diverge, the divergence MUST be reported as a defect and resolved by revision, never by a contributor choosing which to follow. | `PR-NFR-001` · `AR-GOV-007` |

---

## 14. Inputs

Every input the capability model accepts. An input not listed here is out of scope for this document's
behavior.

| Input | Required properties | Validity condition |
| :--- | :--- | :--- |
| **Capability Requirement** | A term reference; an optional level. | MUST reference a term already admitted to the vocabulary, or one admitted concurrently under evolution (`CR-018`). |
| **Capability Offer** | A term reference; an optional level; a reference to exactly one declaring Adapter. | MUST be well-formed per `CR-021` and attributable to exactly one Adapter. |
| **Capability Constraint** | A condition attached to an offer, expressed as an observable fact. | MUST NOT be expressed as an assumption (`CR-036`). |
| **Vocabulary Query** | Optionally, a category or a term reference to narrow the result. | MUST be answerable without invoking any Runtime (`CR-043`). |
| **Validation Request** | A term, category, level, requirement, offer, or constraint to be checked. | MUST identify exactly one declaration under test. |
| **Evolution Proposal** | A new term, category, level, or level scale, with its category membership if a term. | MUST NOT alter the meaning of anything already admitted (`CR-013`, `CR-047`). |
| **Deprecation Request** | A reference to the term to be deprecated; a reason. | MUST identify a term currently admitted and not already retired. |
| **Retirement Request** | A reference to the term to be retired; a reason. | MUST NOT be interpreted as a request to remove the term's record. |

---

## 15. Outputs

Every output the capability model produces, including the observable state after each completes.

| Output | Description | Observable state after |
| :--- | :--- | :--- |
| **Vocabulary Enumeration** | The complete, current set of admitted terms, their categories, and their declared levels. | Available for inspection without altering the vocabulary. |
| **Compatibility Result** | One of satisfied, partially satisfied, or not satisfied, for a given requirement and offer. | The result is reproducible for the same requirement and offer (`CR-039`). |
| **Negotiation Inputs** | The required term and level, the offered term and level, and any capability constraint, assembled for the negotiation procedure a domain-specific document carries out. | Available before that procedure begins (`CR-038`). |
| **Validation Result** | Accepted or rejected, with a stated reason where rejected. | The declaring party has been informed (`CR-042`). |
| **Category and Level Listing** | The categories and, where declared, the level scale for a queried term. | Reportable together, never separately (`CR-027`). |
| **Deprecation Disclosure** | The fact of deprecation and its reason, for a queried term. | Disclosed before any new declaration is made against the term (`CR-051`). |
| **Retirement Record** | The fact of retirement, its reason, and the term's prior admitted state. | The term remains inspectable, marked retired in place (`CR-055`). |

---

## 16. Extension Points

| Extension point | What is added | What MUST NOT change | Architectural attachment |
| :--- | :--- | :--- | :--- |
| **New term admission** | A capability term expressible by both a Workflow step and an Adapter. | That the vocabulary names no Runtime, Vendor, or Model (`CR-012`, `CR-016`). | `EP-4` |
| **New category admission** | A category grouping related terms. | The registered meaning of an existing category (`CR-013`). | `EP-4` |
| **New level scale** | A level scale for an existing or new term, and the ordering between its levels. | The term's identity (`CR-015`). | `EP-4` |
| **New constraint kind** | A new kind of observable condition an offer may declare. | The requirement that a constraint be observable, never assumed (`CR-036`). | `EP-4` |

Extension at any of these points follows AEOS-ARCH `AR-EXT-001` through `AR-EXT-008`: it occurs only
through a named extension point, is declared, versioned, and inspectable, and never weakens a gate,
introduces a second path to inference, or requires modifying AEOS itself.

---

## 17. Constraints

This document states observable behavior only. The categories below are the boundary it does not
cross, each paired with the statement that remains within scope, so the boundary is checkable rather
than a matter of taste.

| ID | This document MUST NOT define | The statement that remains in scope |
| :--- | :--- | :--- |
| `CR-CON-1` | **Provider-specific capability names.** No Vendor's or Runtime's own name for a unit of engineering work. | The neutral term the vocabulary admits, stated independent of any Vendor's naming (`CR-001`, `CR-012`). |
| `CR-CON-2` | **Implementation APIs.** No endpoint, method signature, request or response schema, or wire format for declaring, querying, or comparing a capability. | The behavior an implementation of declaration, discovery, or comparison must produce. |
| `CR-CON-3` | **Protocols.** No transport, serialization, or interoperability-standard binding. | A behavioral dependency any protocol satisfying it may fulfill. |
| `CR-CON-4` | **Database schemas.** No table, field type, index, or query language for vocabulary storage. | That the vocabulary and its declarations be recorded as Repository Assets, independent of storage mechanism. |
| `CR-CON-5` | **Runtime algorithms.** No step-by-step comparison procedure, scoring function, or ranking algorithm. | The result the comparison MUST reach, per [Section 13.6](#136-capability-compatibility-rules). |
| `CR-CON-6` | **A provider capability catalog.** No Vendor-specific list of what a named commercial or open-source Runtime currently offers, and no ranking or benchmarking of one Runtime, Vendor, or Model against another. | The neutral vocabulary any provider's declaration is expressed in, without naming or ranking the provider. |
| `CR-CON-7` | **Technology choices.** No vendor, language, library, framework, or product name, and no reference satisfiable by only one entry in AEOS-TECH. | A behavioral dependency any technology satisfying it may fulfill. |

> **The single-satisfiability test.** Where an author is uncertain whether a statement in this document
> has crossed into a prohibited category, the test is: *can this requirement be satisfied in more than
> one technically reasonable way?* A requirement satisfiable by exactly one mechanism, one technology,
> or one literal interface belongs to a lower layer, stated too early. This is a non-normative aid, not
> an additional rule.

---

## 18. Non-goals

The following are behavior a reader might reasonably expect this document to cover. It deliberately
does not, for the reason stated.

| Non-goal | Reason |
| :--- | :--- |
| Whether, and at what point in a request's lifecycle, a Workflow step's requirement is compared against a selected Runtime's offer | Owned by the Runtime Blueprint's own behavior domain (`BP-RUN-004`), not yet specified, and by `AEOS_RUNTIME_FLOW.md`'s phase ordering. |
| An Adapter's own capability declaration and negotiation procedure | Owned entirely by `RUNTIME_ADAPTER_SPEC.md` (`SP-ADP-014`–`SP-ADP-027`); this document supplies the vocabulary only. |
| A Model's own classification and declaration procedure | Owned entirely by `MODEL_REGISTRY.md` (`SP-MDL-009`–`SP-MDL-019`); this document supplies the vocabulary only. |
| A registered Runtime's identity, metadata, and lifecycle | Owned entirely by `RUNTIME_REGISTRY.md`. |
| Runtime selection, its custody, and the presentation of boundary disclosure content | Owned by the Runtime Blueprint's Selection Custody and Boundary Disclosure Assembly subsystems, not yet specified as behavior. |
| Ranking or benchmarking Runtimes, Vendors, or Models against one another | Would privilege one over another, contrary to `AR-BND-011`; recorded as a deferred recommendation, not a requirement, in AEOS-PRD Appendix A. |
| A vendor-specific catalog of capability names, prices, or release schedules | Would require holding Vendor-specific knowledge outside the Adapter Layer, breaching `AR-BND-005`. |
| The storage, transport, process, or concurrency mechanism by which the vocabulary persists | A Runtime-execution and Implementation concern, excluded under [Section 17](#17-constraints) and left to a future Runtime document addressing execution mechanics. |
| An interface, API, or wire format for declaring, querying, or comparing a capability | Interface detail belongs to a Specification document or an Implementation Guide, should one later be written for this behavior domain. |
| Approval Gate placement and Human Approval interaction | Owned by the Human Interaction Blueprint's behavior domain. |
| Performing inference to answer a vocabulary query or a compatibility test | Forbidden absolutely by `AR-BND-002`; see `CR-023` and `CR-043`. |

---

## 19. Traceability

### 19.1 The Trace Requirement

Every `CR-<NNN>` rule in [Section 13](#13-observable-capability-behavior) and
[Section 17](#17-constraints) traces to one or more `PR-`, `AR-`, or `BP-` identifiers, stated beside
the rule. A rule with no trace would describe behavior the product did not ask for, and is a defect to
be corrected before this document is treated as complete.

### 19.2 Trace Summary by Topic

| Topic | `PR-` families traced | `AR-` identifiers traced | `BP-` identifiers traced |
| :--- | :--- | :--- | :--- |
| Identity | `PR-RUN` · `PR-NFR` | `AR-BND-005` · `AR-BND-013` · `AR-GOV-001` | `BP-RUN-003` · `BP-RUN-013` |
| Ownership | `PR-WFL` | `AR-BND-005` · `AR-PRN-006` | `BP-RUN-003` · `BP-ADP-001` · `BP-ADP-002` · `BP-ADP-007` · `BP-GOV-009` · `BP-GOV-010` |
| Taxonomy | `PR-RUN` · `PR-NFR` · `PR-SAF` | `AR-BND-013` · `AR-PRN-007` · `AR-PRN-009` | `BP-RUN-003` · `BP-GOV-009` |
| Declaration | `PR-RUN` · `PR-WFL` · `PR-NFR` | `AR-BND-002` | `BP-RUN-003` · `BP-RUN-011` · `BP-ADP-001` · `BP-ADP-003` · `BP-GOV-009` · `BP-GOV-010` |
| Discovery | `PR-ENV` · `PR-SAF` · `PR-RUN` | `AR-PRN-007` · `AR-PRN-008` | `BP-RUN-004` |
| Compatibility | `PR-RUN` · `PR-SAF` · `PR-ENV` · `PR-PLT` · `PR-NFR` | `AR-PRN-007` · `AR-BND-011` | `BP-RUN-004` · `BP-GOV-010` |
| Validation | `PR-SAF` · `PR-NFR` · `PR-ENV` | `AR-BND-002` · `AR-EXT-005` · `AR-LAY-006` | — |
| Evolution | `PR-RUN` · `PR-NFR` | `AR-PRN-009` · `AR-EXT-001` · `AR-EXT-004` · `AR-GOV-006` | `BP-ADP-005` |
| Deprecation | `PR-SAF` · `PR-NFR` · `PR-RUN` | `AR-PRN-007` · `AR-PRN-008` · `AR-GOV-006` | — |
| Retirement | `PR-NFR` · `PR-REP` · `PR-SAF` | `AR-PRN-008` · `AR-GOV-006` | — |
| Relationships | `PR-RUN` · `PR-NFR` | `AR-GOV-007` | `BP-RUN-003` · `BP-RUN-004` · `BP-RUN-011` · `BP-ADP-001` · `BP-GOV-009` |

### 19.3 Traceability to Sibling Runtime and Specification Documents

Unlike `RUNTIME_REGISTRY.md`, which recorded at the time of its authorship that no sibling Runtime or
Specification document existed to trace to, this document is written after four such documents already
exist, and traces to each by its actual, registered identifier rather than by a name anticipated in
advance:

| Anticipated prefix (`RUNTIME_REGISTRY.md` §17.3) | Realized as | Cited in this document |
| :--- | :--- | :--- |
| `RF-` (Runtime Flow) | `RTF-` (`AEOS_RUNTIME_FLOW.md`) | `RTF-018`, `RTF-019` |
| `RA-` (Runtime Adapter) | `SP-ADP-` (`RUNTIME_ADAPTER_SPEC.md`) | `SP-ADP-009` through `SP-ADP-027` |
| `MR-` (Model Registry) | `SP-MDL-` (`MODEL_REGISTRY.md`) | `SP-MDL-009` through `SP-MDL-019`, `SP-MDL-041` |
| `RR-` (Runtime Registry) | `RT-REG-` (`RUNTIME_REGISTRY.md`) | `RT-REG-019` through `RT-REG-022` |
| `CR-` (Capability Registry) | `CR-` (this document, under the title *Runtime Capability Specification*) | Throughout [Section 13](#13-observable-capability-behavior) and [Section 17](#17-constraints) |

Where a later revision of any of these four documents changes an identifier this document cites, this
document's citation MUST be reconciled in the same change or the next revision, consistent with
AEOS-SPECSTD `DP5`'s reasoning applied voluntarily here.

---

## 20. References

| Document | Document ID | Cited for |
| :--- | :--- | :--- |
| `AEOS_VISION.md` | AEOS-VISION | Invariant `V6` (no vendor, runtime, model, platform, or distribution is privileged or required), which this document's neutrality rules are written to preserve. |
| `AEOS_PRODUCT_REQUIREMENTS.md` | AEOS-PRD | `PR-RUN-001` through `PR-RUN-016`, and the `PR-NFR`, `PR-SAF`, `PR-ENV`, `PR-WFL`, `PR-PLT`, and `PR-REP` families cited throughout this document. |
| `AEOS_GLOSSARY.md` | AEOS-GLOSSARY | The definitions of *Engineering Capability*, *Capability*, *Runtime*, *Runtime Adapter*, *Model*, *Vendor*, and *Repository Asset*, used throughout this document without restatement. |
| `AEOS_DOCUMENT_STANDARD.md` | AEOS-DOCSTD | The form, structure, and lifecycle this document follows, including Section 4.5's treatment of Runtime documents. |
| `AEOS_SUPPORTED_TECHNOLOGIES.md` | AEOS-TECH | The technology-neutrality this document observes throughout, per [Section 17](#17-constraints). |
| `AEOS_ARCHITECTURE.md` | AEOS-ARCH | `AR-BND-002`, `AR-BND-005`, `AR-BND-011`, `AR-BND-013`, `AR-PRN-006` through `AR-PRN-009`, `AR-LAY-006`, `AR-GOV-001`, `AR-GOV-006`, `AR-GOV-007`, and extension point `EP-4` (Engineering Capability declarations). |
| `AEOS_BLUEPRINT.md` | AEOS-BLUEPRINT | `BP-RUN-001`, `BP-RUN-003`, `BP-RUN-004`, `BP-RUN-011`–`BP-RUN-013`, `BP-ADP-001`–`BP-ADP-003`, `BP-ADP-005`, `BP-ADP-007`, `BP-GOV-004`, `BP-GOV-008`–`BP-GOV-010`, `BP-GOV-013`, and Sections 17 and 18, which fix this document's boundary against the Blueprint layer and against the Specification layer. |
| `AEOS_SPECIFICATION_STANDARD.md` | AEOS-SPECSTD | The structural convention this document's section ordering borrows, without being governed by it. |
| `AEOS_RUNTIME_FLOW.md` | AEOS-RTF | The `RTF-<NNN>` identifier precedent this document's own `CR-<NNN>` convention follows, and `RTF-018`, `RTF-019` cited in [Section 7](#7-capability-declaration). |
| `RUNTIME_REGISTRY.md` | AEOS-RUNTIME-REG | The `CR-` trace anticipation this document fulfills (§17.3), and `RT-REG-019`–`RT-REG-022` cited in [Section 12.1](#121-registry-relationship). |
| `RUNTIME_ADAPTER_SPEC.md` | AEOS-SPEC-ADP | `SP-ADP-009` through `SP-ADP-027`, cited throughout [Section 7](#7-capability-declaration), [Section 9](#9-capability-compatibility), and [Section 12.4](#124-adapter-relationship). |
| `MODEL_REGISTRY.md` | AEOS-SPEC-MDL | `SP-MDL-009` through `SP-MDL-019` and `SP-MDL-041`, cited throughout [Section 6](#6-capability-taxonomy), [Section 8](#8-capability-discovery), and [Section 12.3](#123-model-relationship). |

---

## 21. Document Governance

### 21.1 Status

This document is a **freeze candidate**. It is complete and self-reviewed against AEOS-DOCSTD, and is
intended to become the Runtime Capability Source of Truth once the owner's review under
[Section 21.4](#214-review-policy) records no Critical or Major finding. It declares itself the source
of truth for no subject already registered by AEOS-GLOSSARY or claimed by AEOS-PRD, AEOS-ARCH,
AEOS-BLUEPRINT, `RUNTIME_ADAPTER_SPEC.md`, or `MODEL_REGISTRY.md`: the shared vocabulary those
documents already presuppose is a subject none of them claims.

### 21.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Clarification of an existing rule or boundary | Owner approval | Minor |
| Addition of a rule, an extension point, or a lifecycle stage | Explicit owner revision request | Major |
| Adoption of the `CR` document-local prefix as a registered AEOS-GLOSSARY layer prefix | AEOS-GLOSSARY's own change control, under `I4` | Not a change to this document's version |
| Placement of the Runtime layer in the documentation hierarchy | AEOS-DOCSTD's own change control, under Section 14.2 and Section 4.5 | Not a change to this document's version |
| Removal of a rule or an extension point | Explicit owner decision, recorded, with the reasoning preserved in place | Major |

### 21.3 Relationship to the Architecture and Blueprint Freeze

This document introduces no Architecture Layer, no Blueprint subsystem, and no product capability. It
grants no capability that AEOS-PRD `PR-RUN` does not already require, and relocates no responsibility
AEOS-BLUEPRINT already assigns. An idea arising from it that would alter the Runtime Blueprint's
arrangement is routed to AEOS-BLUEPRINT under that document's change control; an idea that would alter
the product's concepts, capability set, or principles is recorded as a recommendation for a future
release under AEOS-PRD governance. Neither is resolved here.

### 21.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains.

A finding is **Critical** where this document contradicts a higher-authority document, assumes a
responsibility it does not own — including any statement of provider-specific capability names,
implementation APIs, protocols, database schemas, runtime algorithms, a provider capability catalog, or
a technology choice prohibited under [Section 17](#17-constraints) — restates or contradicts a rule
`RUNTIME_ADAPTER_SPEC.md` or `MODEL_REGISTRY.md` already states within its own domain, or states
behavior that would cause incorrect work if acted upon.

### 21.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION on an invariant of purpose | AEOS-VISION governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-PRD on product behavior or scope | AEOS-PRD governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-GLOSSARY on the meaning of a term | AEOS-GLOSSARY governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-DOCSTD on documentation form | AEOS-DOCSTD governs. The deviation is a finding against this document. |
| This document conflicts with AEOS-ARCH on a structural decision | AEOS-ARCH governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-BLUEPRINT on the arrangement of `BP-RUN` or `BP-ADP` | AEOS-BLUEPRINT governs the arrangement; this document governs the vocabulary's observable behavior, per AEOS-BLUEPRINT Section 18.1. Where the two cannot both hold, escalate to the owner. |
| This document conflicts with `RUNTIME_ADAPTER_SPEC.md` or `MODEL_REGISTRY.md` on a declaration a rule in either already states for its own domain | The domain-specific document governs its own domain; this document governs the vocabulary. A genuine conflict — where the same declaration could conform to one and not the other — is escalated to the owner, per [Section 2.7](#27-relationship-to-sibling-specification-documents). |
| This document's structure diverges from AEOS-SPECSTD's | Not a conflict. AEOS-SPECSTD governs the Specification layer only; the divergence is a stylistic choice under this document's own authority. |
| A future Specification document deviates from the observable behavior stated here | Escalate to the owner. Neither document has default authority over the other, since Runtime and Specification are siblings under AEOS-PRD Section 3.2, not one subordinate to the other. |
| Two Runtime documents conflict with one another | Escalate to the owner. A contributor MUST NOT resolve it by preference. |

### 21.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Freeze candidate | Initial Runtime Capability Specification. Establishes the document's authority as a Runtime-layer document under AEOS-PRD Section 3.2 and AEOS-DOCSTD Section 4.5's unassigned-layer treatment, fulfilling the `CR-` trace anticipation `RUNTIME_REGISTRY.md` Section 17.3 recorded for a prospective "Capability Registry" document. States the observable Engineering Capability model — identity, ownership, taxonomy (categories and levels), declaration (requirement and offer), discovery, compatibility (the compatibility test, capability constraints, and negotiation inputs), validation, and lifecycle (evolution, deprecation, retirement) — shared by every Runtime, Adapter, and Model, and its relationship to the Runtime Registry, the Runtime position, the Model position, and the Adapter position. States sixty-five `CR-001`–`CR-065` behavior rules and seven `CR-CON-1`–`CR-CON-7` constraints, an Inputs and Outputs table, four Extension Points attaching to `EP-4`, a consolidated traceability table, and eleven Non-goals. Explicitly delineates its boundary against `RUNTIME_ADAPTER_SPEC.md` (`SP-ADP`) and `MODEL_REGISTRY.md` (`SP-MDL`), neither restating nor superseding either. Introduces no product requirement, no Architecture Layer, no Blueprint subsystem, no AEOS-GLOSSARY term, and no implementation. Relocates no responsibility AEOS-BLUEPRINT assigns. |

---

## Appendix A — Capability Rule Index

**Non-normative.** This appendix summarizes identifiers stated normatively in
[Section 13](#13-observable-capability-behavior) and [Section 17](#17-constraints). Where it and the
body differ, the body governs.

| Group | Subject | Range | Count | Section |
| :--- | :--- | :--- | :--- | :--- |
| Identity | Term identity, stability, and neutrality | `CR-001`–`CR-005` | 5 | [13.1](#131-capability-identity-rules) |
| Ownership | Vocabulary custody and declaration attribution | `CR-006`–`CR-010` | 5 | [13.2](#132-capability-ownership-rules) |
| Taxonomy | Categories and levels | `CR-011`–`CR-017` | 7 | [13.3](#133-capability-taxonomy-rules) |
| Declaration | Requirement and offer well-formedness | `CR-018`–`CR-025` | 8 | [13.4](#134-capability-declaration-rules) |
| Discovery | Vocabulary enumeration and query | `CR-026`–`CR-030` | 5 | [13.5](#135-capability-discovery-rules) |
| Compatibility | The compatibility test, constraints, and negotiation inputs | `CR-031`–`CR-040` | 10 | [13.6](#136-capability-compatibility-rules) |
| Validation | Well-formedness checking | `CR-041`–`CR-045` | 5 | [13.7](#137-capability-validation-rules) |
| Evolution | Additive vocabulary change | `CR-046`–`CR-049` | 4 | [13.8](#138-capability-evolution-rules) |
| Deprecation | Marking a term for eventual retirement | `CR-050`–`CR-053` | 4 | [13.9](#139-capability-deprecation-rules) |
| Retirement | Permanent removal from availability | `CR-054`–`CR-057` | 4 | [13.10](#1310-capability-retirement-rules) |
| Relationships | Registry, Runtime, Model, and Adapter boundaries | `CR-058`–`CR-065` | 8 | [13.11](#1311-relationship-rules) |
| Constraints | Mechanism, technology, and catalog exclusions | `CR-CON-1`–`CR-CON-7` | 7 | [17](#17-constraints) |
| **Total** | | | **72** | — |

---

**End of Runtime Capability Specification**

AEOS-CAP · Version 1.0.0 · Runtime Capability Source of Truth (freeze candidate, pending owner review) ·
Traces to `PR-RUN` · `PR-NFR` · `PR-SAF` · `PR-ENV` · `PR-WFL` · `PR-PLT` · `PR-REP`, composing
`AR-` · `BP-` in full, and citing `RTF-` · `SP-ADP-` · `SP-MDL-` · `RT-REG-` by their sibling documents'
own identifiers
