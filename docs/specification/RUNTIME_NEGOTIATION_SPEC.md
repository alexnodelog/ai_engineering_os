# AI Engineering Operating System

## AEOS — Runtime Negotiation Specification

*The permanent statement of how AEOS determines, before any step is dispatched, which Runtime,
Adapter, and Model combinations are compatible with a Workflow step's declared Engineering
Capability requirement.*

| Field | Value |
| :--- | :--- |
| **Document** | Runtime Negotiation Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SPEC-NEG |
| **Version** | 1.0.0 |
| **Status** | Frozen |
| **Owner** | Product Owner, AEOS |
| **Author** | Specification Governance Board, AEOS |
| **Audience** | Architects, implementers, reviewers, test authors, and AI runtimes |
| **Suggested path** | `docs/specification/RUNTIME_NEGOTIATION_SPEC.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `RUNTIME_ADAPTER_SPEC.md` (AEOS-SPEC-ADP) · `MODEL_REGISTRY.md` (AEOS-SPEC-MDL) |
| **Supersedes** | None |
| **Area code** | `NEG` |

> **Authority of this document.**
> This document specifies, precisely and testably, the Runtime Negotiation behavior of AEOS: the
> observable process by which AEOS determines whether a candidate Runtime, Adapter, and Model
> combination is compatible with a Workflow step's declared Engineering Capability requirement,
> drawing on the Runtime Blueprint's Capability Vocabulary, Capability Matching, Selection Custody,
> Degradation Handling, and Fault Surfacing subsystems (AEOS-BLUEPRINT Section 10).
>
> It defines no rationale, no structure, no technology, no interface surface, and no implementation.
> It defines no selection algorithm, no ranking algorithm, and no procedure by which a Runtime is
> chosen: the user's authority to select a Runtime is defined by AEOS-PRD and its custody by
> AEOS-BLUEPRINT, and this document states only what AEOS must determine and report before that
> authority is exercised. It is not a Product document, not an Architecture document, not a
> Blueprint, not a Runtime document, and not an implementation guide. Where this document and a
> document of higher authority both speak to a subject, the higher-authority document governs and
> any conflict here is a defect to be reported rather than acted upon.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Scope](#2-scope)
3. [Responsibilities](#3-responsibilities)
4. [Inputs](#4-inputs)
5. [Outputs](#5-outputs)
6. [Behavior](#6-behavior)
7. [Constraints](#7-constraints)
8. [Extension Points](#8-extension-points)
9. [Traceability](#9-traceability)
10. [Non-goals](#10-non-goals)
11. [References](#11-references)
12. [Document Governance](#12-document-governance)
13. [Appendix A — Identifier Index](#appendix-a--identifier-index)

---

## 1. Purpose

AEOS-PRD `PR-RUN-007` requires that AEOS "automatically selects engineering capabilities appropriate
to the chosen runtime, and reports anything the runtime cannot support before work begins," and
`PR-WFL-016` requires that AEOS report which workflow steps a selected runtime cannot satisfy before
execution begins. Neither requirement, by itself, states what AEOS must observably do to discharge
it. AEOS-BLUEPRINT Section 10 assigns the answer to a position — the Runtime Blueprint's Capability
Vocabulary, Capability Matching, Selection Custody, Degradation Handling, and Fault Surfacing
subsystems — and states, correctly for its layer, where that responsibility sits rather than what it
must do (AEOS-BLUEPRINT Section 17.1). RUNTIME_ADAPTER_SPEC.md and MODEL_REGISTRY.md each anticipated
this gap directly, each recording in its own Non-goals that comparing a step's requirement against a
candidate's declared offer is owned by "the Specification governing the `RUN` behavior domain," not
yet authored at the time either was written.

This Specification closes that gap for the Negotiation-specific portion of that domain. It states,
precisely enough to test, who participates in Negotiation; what Negotiation accepts and produces; how
capability, compatibility, and configured constraints are evaluated; how a stated preference is
treated; what governs the relationship between a compatible finding and an actual Selection; and what
happens when a candidate degrades, when no candidate is compatible, when a Negotiation is cancelled,
and when it times out. It states nothing about how any of this is implemented, and nothing about why
runtime independence or Human-in-the-Loop selection matter — AEOS-VISION and AEOS-PRD already state
that.

A reader of this document gains a fixed target that distinguishes two things easy to conflate:
determining that a combination is *compatible*, which this document specifies, and *selecting* one,
which remains, in every case, the act AEOS-PRD `PR-RUN-004` reserves to the user and AEOS-BLUEPRINT
`BP-RUN-005` holds unchanged. An implementer can determine exactly what Negotiation must compute and
report without inventing a selection mechanism AEOS does not have; a reviewer or test author can check
that no rule here crosses that line.

---

## 2. Scope

### 2.1 Behavior Domain and Area Registration

This Specification governs the **Runtime Negotiation behavior domain**: the observable process by
which a Workflow step's declared Engineering Capability requirement is compared against every
candidate Runtime, Adapter, and Model combination the Runtime Registry and Model Registry make
discoverable, producing a compatibility finding for each candidate, before any request for that step
is dispatched. This domain realizes the Negotiation-relevant content of the arrangement
AEOS-BLUEPRINT Section 10 (the Runtime Blueprint, `BP-RUN`) establishes — its Capability Vocabulary,
Capability Matching, Selection Custody, Degradation Handling, and Fault Surfacing subsystems, read for
their Negotiation content — and depends upon the Adapter Blueprint (AEOS-BLUEPRINT Section 11,
`BP-ADP`) and its Specification, RUNTIME_ADAPTER_SPEC.md (AEOS-SPEC-ADP), and upon the Model Registry
Specification, MODEL_REGISTRY.md (AEOS-SPEC-MDL).

This Specification registers the area code `NEG`, per AEOS-SPECSTD Section 11.4. `NEG` names its own,
distinct behavior domain; it is not registered by any Architecture or Blueprint document, and it does
not claim the whole of `BP-RUN`'s arrangement. AEOS-SPECSTD Section 6.2 permits a Specification to
cover a defined slice of a Blueprint layer's behavior rather than the whole of it, consistent with the
precedent MODEL_REGISTRY.md (`MDL`) already sets for a domain that does not correspond one-to-one with
a single Blueprint layer. This Specification's own boundary is stated completely in
[Section 2.2](#22-boundary) and does not narrow if a further Specification is later authored for the
remainder of `BP-RUN`'s arrangement — that would be a distinct behavior domain, registering its own
area code, not a split of this one under AEOS-SPECSTD Section 6.2's prohibition.

### 2.2 Boundary

The domain begins at the point where a Workflow step's Engineering Capability requirement is ready to
be compared against the candidates the Runtime Registry and Model Registry make discoverable (a
Repository read the Runtime Layer is permitted, per `AR-DEP-004`), and ends at the point where a
Negotiation Result exists for every evaluated candidate and is made available to Selection Custody and
to the Workflow Blueprint. This Specification does not cross into: the mediation of a request once
dispatched (owned by RUNTIME_ADAPTER_SPEC.md); the assembly or presentation of boundary disclosure
content (owned by the Runtime Blueprint's Boundary Disclosure Assembly responsibility); the placement
or satisfaction of an Approval Gate (owned by the Human Interaction Blueprint); or the act of Selection
itself (owned by AEOS-PRD and custodied, unchanged, by Selection Custody, `BP-RUN-005`).

### 2.3 Applicability

This Specification applies to every Workflow step that declares an Engineering Capability requirement,
regardless of the category of Runtime a candidate mediates — a commercial service, an open-source
model, an interoperability standard, or a user-supplied extension (`PR-RUN-016`) — and regardless of
whether a project is configured for a single Runtime or for the multi-Runtime orchestration
`PR-RUN-013` permits. It applies identically regardless of Platform or Distribution Method
(`PR-PLT-003`, `PR-DST-005`, `PR-DST-006`).

### 2.4 What This Specification Does Not Cover

| Not covered here | Owned by |
| :--- | :--- |
| Which Runtime a project or a step ultimately uses, and the custody of that choice | AEOS-PRD (`PR-RUN-004`), custodied by Selection Custody, `BP-RUN-005` |
| The mediation of a request once dispatched, and the declaration behavior an adapter itself must satisfy | RUNTIME_ADAPTER_SPEC.md (AEOS-SPEC-ADP) |
| A Model's identity, metadata, classification, and declared capability set as registered | MODEL_REGISTRY.md (AEOS-SPEC-MDL) |
| Assembling and presenting the disclosure of what will cross the boundary to a Runtime, and its expected cost | The Runtime Blueprint's Boundary Disclosure Assembly responsibility (`BP-RUN-006`, `BP-RUN-007`), a distinct behavior domain not covered here |
| The observable behavior and governance of the Runtime Registry itself — registration, metadata, classification, and lifecycle of a registered Runtime | `RUNTIME_REGISTRY.md`, a Runtime document |
| Approval Gate placement and Human Approval interaction | The Human Interaction Blueprint's behavior domain |
| Workflow step sequencing and Workflow State maintenance | The Workflow Blueprint's behavior domain |
| Transport, wire format, process model, concurrency, storage mechanism, and installation | Runtime documents and Implementation Guides, per AEOS-BLUEPRINT Section 18 |

A statement in this document that decides any of the above is a defect in this document and MUST be
reported rather than acted upon.

---

## 3. Responsibilities

### 3.1 What This Specification Is Answerable For

Applying `SS-P-02` to this document specifically: this Specification is answerable for precise,
testable rules covering Negotiation ownership, Negotiation participants, Capability Matching,
Compatibility Evaluation, Constraint Evaluation, Preference Handling, Selection Principles, Fallback
Behavior, Failure Behavior, Cancellation Behavior, Timeout Behavior, the Negotiation Lifecycle, and
this domain's interaction with the Runtime Registry, the Runtime Layer, the Adapter Blueprint, the
Model Registry, and the Capability Vocabulary — the Negotiation-relevant content of the position
AEOS-BLUEPRINT Section 10 assigns.

### 3.2 What This Specification Is Not Answerable For

| Question | Answered by |
| :--- | :--- |
| Which candidate is ultimately used | AEOS-PRD, custodied unchanged by Selection Custody, `BP-RUN-005` |
| What an adapter's own declaration must state | RUNTIME_ADAPTER_SPEC.md |
| What a Model's registration must itself state | MODEL_REGISTRY.md |
| Whether and how a human is asked to approve a crossing | The Human Interaction Blueprint's behavior domain |
| What is disclosed before a crossing, and its expected cost | The Runtime Blueprint's Boundary Disclosure Assembly responsibility |
| How this domain's behavior is realized in code | Implementation Guides |
| Why runtime independence, Human-in-the-Loop selection, and vendor neutrality are required | AEOS-VISION and AEOS-PRD |

### 3.3 The Responsibility Test

Every rule in [Section 6](#6-behavior) and [Section 7](#7-constraints) was tested against
AEOS-SPECSTD Section 7.2 before inclusion: is it a testable fact about required behavior, traceable to
a `PR-` identifier, and free of mechanism? A rule that would have stated how a compatible candidate is
chosen among several, or in what order candidates are internally evaluated, failed this test and was
rewritten as a rule about what must be reported, or removed.

### 3.4 Declared Dependencies

Per AEOS-SPECSTD `DP1`, this Specification declares its dependencies on other Specification domains
explicitly, by identifier, rather than presuming them. This domain depends on RUNTIME_ADAPTER_SPEC.md
for a candidate's Capability Advertisement and declared compatibility conditions (`SP-ADP-011`,
`SP-ADP-014`, `SP-ADP-018` through `SP-ADP-027`, `SP-ADP-061`, `SP-ADP-062`), and on MODEL_REGISTRY.md
for a candidate's declared Model capability set where one is declared (`SP-MDL-012`, `SP-MDL-023`,
`SP-MDL-039`, `SP-MDL-040`). Neither dependency is circular: RUNTIME_ADAPTER_SPEC.md and
MODEL_REGISTRY.md each state, in their own Non-goals, that Capability Matching against a step's
requirement is owned elsewhere, consistent with `DP3`.

---

## 4. Inputs

| Input | Required properties | Validity condition |
| :--- | :--- | :--- |
| **Capability Requirement** | An Engineering Capability reference, expressed in the neutral vocabulary; a reference to the Workflow step it belongs to. | MUST be expressed only in terms the Capability Vocabulary subsystem holds (`BP-RUN-003`). MUST NOT be accepted before the Workflow Blueprint has declared it for the step. |
| **Candidate Set** | Every Runtime Registry entry currently discoverable, together with its associated adapter and any Model reference the adapter declares. | MUST be drawn from Runtime Registry discovery output; MUST NOT be assembled independently of it. |
| **Capability Advertisement** | An adapter's declared offer, per RUNTIME_ADAPTER_SPEC.md `SP-ADP-014`. | MUST be answerable without dispatching an invocation, per `SP-ADP-011`'s discoverability requirement and the Capability Query input RUNTIME_ADAPTER_SPEC.md defines. |
| **Model Compatibility Declaration** | A Model's declared Engineering Capability set and any recorded revision, per MODEL_REGISTRY.md `SP-MDL-012`, `SP-MDL-030`. | MUST originate from the declaring adapter, per `SP-MDL-014`; MUST NOT be treated as independently known. |
| **Recorded Runtime Selection** | The Runtime, or the per-category Runtime configuration where `PR-RUN-013` orchestration is configured, recorded in the project's Profile. | MUST be read, never inferred; presented unchanged, per `BP-RUN-005`. |
| **Configured Constraint** | A project-declared restriction on which candidates apply to a step's category, where multi-Runtime orchestration is configured under `PR-RUN-013`. | MUST originate from the project's own declared configuration; MUST NOT be inferred. |
| **Negotiation Query** | A reference to the step or candidate whose compatibility is requested. | MUST be answerable without dispatching an invocation to any candidate's mediated Runtime. |
| **Cancellation Signal** | A reference to the Negotiation in progress it applies to. | MUST identify a Negotiation the receiving instance of this domain is currently evaluating. |

---

## 5. Outputs

| Output | Description | Observable state after |
| :--- | :--- | :--- |
| **Negotiation Result** | A confirmation that a candidate satisfies the step's Capability Requirement, or a stated gap, for exactly one candidate. | The candidate's compatibility is recorded before any request naming it is dispatched. |
| **Compatible Candidate Set** | The set of every candidate for which a Negotiation Result confirms satisfaction. | Available to Selection Custody and to the Workflow Blueprint as material; authorizes no Selection by itself. |
| **Negotiation Gap Report** | The stated difference, for a candidate that does not satisfy the requirement, expressed in the neutral vocabulary. | Available for inspection; reported before the step proceeds with that candidate. |
| **Degraded Option Set** | The reduced Compatible Candidate Set following the unavailability or incompatibility of a previously compatible candidate. | Available as material; no substitute candidate has been chosen on the requester's behalf. |
| **Negotiation Failure** | The report that no candidate in the Candidate Set satisfies the step's requirement. | The step does not proceed; the failure is available for inspection per [Section 6.9](#69-failure-behavior). |
| **Cancellation Outcome** | The reported result of a cancellation signal, per [Section 6.10](#610-cancellation-behavior). | The Negotiation is no longer in progress; no Negotiation Result is produced on its behalf. |
| **Timeout Outcome** | The reported result of an expired bounded wait for a candidate's compatibility determination, per [Section 6.11](#611-timeout-behavior). | The candidate is excluded from the Compatible Candidate Set for this Negotiation; evaluation of other candidates is unaffected. |
| **Negotiation State** | The current position of a Negotiation in its lifecycle, per [Section 6.12](#612-negotiation-lifecycle). | Available on request without requiring the Negotiation to be re-run. |

---

## 6. Behavior

The normative rules of this Specification. Identifiers are allocated sequentially within the `NEG`
area across this section, [Section 7](#7-constraints), and [Section 8](#8-extension-points), per
`ID1`.

### 6.1 Negotiation Ownership

Negotiation ownership is the boundary between determining that a combination is compatible and
choosing one. This Specification owns the former exclusively; it owns none of the latter, which
AEOS-PRD `PR-RUN-004` reserves to the user and `BP-RUN-005` holds unchanged.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-001` | This behavior domain MUST determine and report the compatibility of a candidate against a step's Capability Requirement, and MUST NOT itself select a candidate for the step. | `PR-RUN-004` · `PR-RUN-007` · `BP-RUN-005` |
| `SP-NEG-002` | A Negotiation Result MUST NOT be treated, by this domain or by any layer that consumes it, as satisfying an Approval Gate or as constituting a Selection. | `AR-BND-004` · `AR-BND-010` · `PR-SAF-005` |
| `SP-NEG-003` | This behavior domain MUST NOT hold knowledge of a specific Runtime, Vendor, or Model beyond what a candidate's declaring adapter or Model registration has itself made discoverable. | `AR-BND-005` · `BP-RUN-001` · `PR-RUN-002` |
| `SP-NEG-004` | This behavior domain MUST NOT perform inference in the course of determining compatibility. | `AR-BND-002` · `PR-RUN-001` · `BP-RUN-002` |

### 6.2 Negotiation Participants

| Participant | Role in Negotiation |
| :--- | :--- |
| Workflow Blueprint | Declares the step's Capability Requirement and consumes the Negotiation Result. |
| Runtime Registry | Supplies the discoverable Candidate Set and each candidate's recorded availability. |
| Runtime Adapter (RUNTIME_ADAPTER_SPEC.md) | Supplies each candidate's Capability Advertisement, answered without invocation. |
| Model Registry | Supplies a candidate's declared Model compatibility where the adapter has declared one. |
| Selection Custody (`BP-RUN-005`) | Holds the user's recorded Runtime selection and receives the Negotiation Result for it unchanged. |
| The user, through the Profile | The sole authority for Selection; Negotiation reports to this authority and never substitutes for it. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-005` | This domain MUST address only the participants listed above, and only through the interaction stated for each. | `AR-DEP-004` · `PR-NFR-006` |
| `SP-NEG-006` | A participant's absence MUST reduce the Candidate Set only, and MUST NOT be treated as a failure of Negotiation itself. | `PR-RUN-010` · `AR-PRN-008` |
| `SP-NEG-007` | This domain MUST NOT address the Human Layer directly; where a Negotiation Result bears on a decision a human must make, it is reported to the Workflow Blueprint, which alone addresses the Human Layer. | `AR-DEP-004` · `PR-WFL-005` |
| `SP-NEG-008` | This domain MUST NOT address the External AI Layer directly, and MUST NOT require an invocation of any candidate's mediated Runtime to complete a Negotiation. | `AR-BND-002` · `AR-EXT-005` · `PR-RUN-001` |

### 6.3 Capability Matching

Capability Matching is the comparison, for one candidate at a time, of a step's Capability Requirement
against that candidate's Capability Advertisement, stated in the same neutral vocabulary both sides
already use.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-009` | Capability Matching MUST compare a step's Capability Requirement against a candidate's Capability Advertisement using only the neutral Engineering Capability vocabulary `BP-RUN-003` holds. | `BP-RUN-003` · `BP-RUN-004` · `PR-RUN-007` |
| `SP-NEG-010` | Capability Matching MUST produce a Negotiation Result for every candidate in the Candidate Set, not only for the recorded Runtime selection. | `BP-RUN-004` · `BP-RUN-013` · `PR-RUN-008` |
| `SP-NEG-011` | Capability Matching MUST complete for a step before any request for that step is dispatched, and MUST NOT be repeated during the step absent a revised Capability Requirement or a revised Capability Advertisement. | `BP-RUN-004` · `PR-WFL-016` |
| `SP-NEG-012` | Where a candidate's Capability Advertisement satisfies only part of a step's Capability Requirement, Capability Matching MUST report the unsatisfied part as a gap and MUST NOT report the candidate as fully compatible. | `SP-ADP-020` · `PR-RUN-007` · `PR-SAF-011` |
| `SP-NEG-013` | Capability Matching MUST NOT interpret, extend, or narrow a Capability Requirement or a Capability Advertisement beyond what each states in the neutral vocabulary. | `PR-RUN-008` · `BP-RUN-004` |

### 6.4 Compatibility Evaluation

Beyond capability, a candidate's suitability depends on runtime compatibility, Model compatibility,
version compatibility, and Platform compatibility conditions the candidate itself declares.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-014` | Compatibility Evaluation MUST treat a candidate reported Unavailable under RUNTIME_ADAPTER_SPEC.md's Adapter Lifecycle as not currently evaluable, distinct from a candidate found incompatible. | `SP-ADP-023` · `PR-SAF-011` · `PR-ENV-004` |
| `SP-NEG-015` | Compatibility Evaluation MUST honor a candidate's declared runtime compatibility conditions and MUST NOT report a candidate compatible where its mediated Runtime does not meet them. | `SP-ADP-022` · `SP-ADP-023` · `PR-ENV-004` |
| `SP-NEG-016` | Where a candidate declares a Model reference, Compatibility Evaluation MAY use it to determine capability compatibility but MUST NOT require it of any Workflow step. | `SP-ADP-025` · `SP-ADP-026` · `PR-RUN-006` |
| `SP-NEG-017` | Compatibility Evaluation MUST honor a candidate's declared version compatibility with RUNTIME_ADAPTER_SPEC.md and MUST report an incompatible declared version before the candidate is included in the Compatible Candidate Set. | `SP-ADP-062` · `PR-NFR-002` · `PR-RUN-012` |
| `SP-NEG-018` | Compatibility Evaluation MUST treat a Platform limitation recorded for a candidate as a compatibility condition, disclosed rather than silently absorbed. | `AR-LAY-007` · `PR-PLT-002` · `PR-PLT-003` |
| `SP-NEG-019` | A candidate's compatibility MUST be evaluated independently of every other candidate's compatibility; one candidate's incompatibility MUST NOT be inferred from, or reported as, another's. | `PR-SAF-011` · `AR-PRN-008` |

### 6.5 Constraint Evaluation

Beyond capability and compatibility, a project MAY declare configuration constraints — such as the
per-category Runtime configuration `PR-RUN-013` permits for multi-Runtime orchestration — that further
restrict which otherwise-compatible candidates apply to a given step.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-020` | Where a project is configured for multi-Runtime orchestration under `PR-RUN-013`, Constraint Evaluation MUST restrict the Candidate Set for a step to the candidates the configuration designates for that step's category before Capability Matching is reported for the step. | `PR-RUN-013` · `BP-RUN-004` |
| `SP-NEG-021` | Constraint Evaluation MUST NOT restrict the Candidate Set on any basis the project has not itself declared. | `AR-BND-011` · `PR-RUN-003` |
| `SP-NEG-022` | A configured constraint MUST be reported alongside the Negotiation Result it affected, distinguishing a candidate excluded by configuration from a candidate excluded by incompatibility. | `PR-SAF-011` · `PR-NFR-001` |
| `SP-NEG-023` | The absence of a declared constraint MUST leave the full Candidate Set available for evaluation; this domain MUST NOT infer a constraint the project has not declared. | `AR-PRN-008` · `PR-RUN-010` |
| `SP-NEG-024` | Constraint Evaluation MUST NOT be used to exclude a candidate on the basis of Vendor, Runtime, or Model identity alone, independent of a declared project configuration or a stated incompatibility. | `AR-BND-011` · `PR-RUN-003` |

### 6.6 Preference Handling

The only preference this domain recognizes is the user's own recorded Runtime selection, held in the
Profile, and any per-category configuration `PR-RUN-013` permits. This domain computes no preference
of its own.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-025` | This behavior domain MUST NOT infer, compute, or default a preference among compatible candidates that the project has not itself recorded. | `AR-BND-011` · `PR-RUN-003` · `PR-RUN-004` |
| `SP-NEG-026` | Where the Profile records a Runtime selection, Negotiation MUST treat that recorded selection as the candidate to evaluate for the step in hand, without altering the Negotiation Result any other candidate would receive. | `BP-RUN-005` · `PR-RUN-004` |
| `SP-NEG-027` | Presentation of the Compatible Candidate Set MUST NOT be ordered, scored, or annotated in a way that implies one compatible candidate is recommended over another. | `AR-BND-011` · `PR-RUN-003` |
| `SP-NEG-028` | A stated project configuration under [Section 6.5](#65-constraint-evaluation) MUST be treated as a constraint on the Candidate Set, not as a ranking of candidates within it. | `PR-RUN-013` · `AR-BND-011` |

### 6.7 Selection Principles

These principles govern the relationship between what Negotiation determines and what Selection
Custody holds, so that the existence of a Compatible Candidate Set is never mistaken for a choice.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-029` | A Negotiation Result, and the Compatible Candidate Set it contributes to, MUST NOT itself constitute a Selection of any candidate. | `PR-RUN-004` · `BP-RUN-005` |
| `SP-NEG-030` | Where exactly one candidate is compatible, this domain MUST report it as the sole compatible candidate and MUST NOT apply it as the step's Selection without a separately recorded act of Selection. | `PR-RUN-004` · `AR-BND-010` |
| `SP-NEG-031` | Where the Profile's recorded Runtime selection is itself found compatible, that recorded selection MUST be used for the step without requiring a new act of Selection; this is a continuation of an existing Selection, not a new one made by this domain. | `BP-RUN-005` · `PR-RUN-004` |
| `SP-NEG-032` | Where the Profile's recorded Runtime selection is found incompatible, this domain MUST report the gap and MUST NOT substitute a different candidate as the step's Selection. | `PR-RUN-004` · `PR-WFL-016` · `BP-ADP-009` |
| `SP-NEG-033` | This domain MUST NOT resolve a tie among multiple compatible candidates by any means; where more than one candidate is compatible and no Selection is recorded, every compatible candidate MUST be reported, and the choice among them remains outstanding. | `AR-BND-010` · `PR-RUN-004` · `AR-BND-011` |

### 6.8 Fallback Behavior

Fallback in this domain means only that the Candidate Set is reduced; it never means that a substitute
candidate is chosen automatically. This follows AEOS-ARCH Section 6.5, "Absence Reduces Options,"
applied to Negotiation.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-034` | Where a previously compatible candidate becomes unavailable or is found incompatible, this domain MUST reduce the Compatible Candidate Set to exclude it and MUST NOT select or propose a substitute candidate on the requester's behalf. | `PR-RUN-010` · `PR-RUN-004` · `AR-PRN-008` · `BP-ADP-009` |
| `SP-NEG-035` | A reduced Compatible Candidate Set MUST NOT alter durable project state, including the Profile's recorded Runtime selection. | `BP-RUN-009` · `PR-SAF-010` |
| `SP-NEG-036` | The unavailability or incompatibility of one candidate MUST NOT affect the Negotiation Result of another candidate. | `PR-RUN-010` · `AR-PRN-008` |
| `SP-NEG-037` | Where the reduction under `SP-NEG-034` leaves no compatible candidate, this domain MUST proceed to Failure Behavior, [Section 6.9](#69-failure-behavior), rather than report a degraded result as success. | `PR-SAF-011` · `PR-RUN-011` |
| `SP-NEG-038` | A Degraded Option Set MUST be reported as distinct from the Compatible Candidate Set that preceded it, so that the reduction is itself an observed, inspectable fact. | `PR-NFR-001` · `AR-PRN-007` |

### 6.9 Failure Behavior

Negotiation fails, distinctly from a candidate's unavailability, when no candidate in the Candidate
Set satisfies the step's Capability Requirement.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-039` | This domain MUST report a Negotiation Failure where every evaluated candidate's Negotiation Result states a gap, and MUST NOT report the step as satisfiable. | `PR-RUN-007` · `PR-WFL-016` |
| `SP-NEG-040` | A Negotiation Failure MUST be distinguished from the unavailability of every candidate; the former states that no candidate's declared offer satisfies the requirement, the latter states that no candidate could be evaluated. | `PR-SAF-011` · `AR-PRN-007` |
| `SP-NEG-041` | A Negotiation Failure MUST NOT halt any other Workflow step or capability that does not depend on the failed step's requirement. | `PR-RUN-010` · `AR-PRN-008` |
| `SP-NEG-042` | A Negotiation Failure MUST be reported with the gap stated for every evaluated candidate, so that a reviewer can determine what would need to change for the step to become satisfiable. | `PR-NFR-001` · `PR-SAF-011` |
| `SP-NEG-043` | A Negotiation Failure MUST NOT be resolved by this domain through relaxing, reinterpreting, or narrowing the step's Capability Requirement. | `PR-RUN-008` · `BP-RUN-004` |

### 6.10 Cancellation Behavior

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-044` | This domain MUST accept a cancellation signal for a Negotiation it is currently evaluating. | `PR-SAF-005` · `PR-WFL-009` |
| `SP-NEG-045` | On accepting a cancellation, this domain MUST NOT produce a Negotiation Result for the cancelled Negotiation, and MUST NOT report any candidate as compatible or incompatible on its behalf. | `PR-RUN-011` · `PR-WFL-009` |
| `SP-NEG-046` | A cancelled Negotiation MUST NOT be treated as a Negotiation Failure; the two are distinct outcomes and MUST be reported distinctly. | `PR-WFL-009` · `PR-WFL-010` · `PR-SAF-011` |
| `SP-NEG-047` | Cancellation of a Negotiation MUST NOT alter durable project state, including the Profile's recorded Runtime selection. | `PR-SAF-010` · `BP-RUN-009` |

### 6.11 Timeout Behavior

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-048` | This domain MUST observe a bounded wait for a candidate's Capability Advertisement or compatibility determination, and MUST NOT wait indefinitely. | `PR-SAF-002` · `PR-RUN-011` |
| `SP-NEG-049` | An expired wait for one candidate MUST exclude that candidate from the Compatible Candidate Set for the current Negotiation and MUST NOT block evaluation of any other candidate. | `PR-RUN-010` · `AR-PRN-008` |
| `SP-NEG-050` | An expired wait MUST NOT be reported as a Negotiation Result stating incompatibility; it is a distinct condition — evaluation did not complete — and MUST be reported as such. | `PR-SAF-011` · `PR-NFR-001` |
| `SP-NEG-051` | Where a candidate's compatibility determination arrives after its wait has already expired and been reported, this domain MUST NOT retroactively include that candidate in a Compatible Candidate Set already reported for the Negotiation. | `PR-SAF-011` · `PR-RUN-011` |

### 6.12 Negotiation Lifecycle

A Negotiation occupies exactly one of six observable states. Every transition is reported as an
observed fact; none is implicit.

| State | Meaning |
| :--- | :--- |
| **Not Started** | No Capability Requirement has yet been submitted for evaluation. |
| **Evaluating** | One or more candidates' Negotiation Results are outstanding. |
| **Resolved** | Every candidate in the Candidate Set has a Negotiation Result, yielding a Compatible Candidate Set, a Negotiation Failure, or both a Compatible Candidate Set and a stated Negotiation Gap Report. |
| **Degraded** | A previously Resolved Negotiation's Compatible Candidate Set has been reduced under [Section 6.8](#68-fallback-behavior) without a new evaluation being required. |
| **Cancelled** | A cancellation signal was accepted before the Negotiation reached Resolved. |
| **Timed Out** | A bounded wait expired for one or more candidates before the Negotiation reached Resolved for the full Candidate Set. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-052` | A Negotiation MUST occupy exactly one of Not Started, Evaluating, Resolved, Degraded, Cancelled, or Timed Out at any time. | `PR-NFR-001` · `PR-SAF-011` |
| `SP-NEG-053` | A transition between Negotiation states MUST be reported as an observed fact, and MUST NOT be inferred from the mere absence of contrary evidence. | `PR-SAF-011` · `AR-PRN-007` |
| `SP-NEG-054` | A Negotiation MUST reach Resolved, Cancelled, or Timed Out for every Capability Requirement it accepts; it MUST NOT remain indefinitely in Evaluating. | `PR-SAF-002` · `PR-RUN-011` |
| `SP-NEG-055` | The current state of a Negotiation MUST be available on request without re-running the evaluation. | `PR-WFL-007` · `PR-NFR-001` |
| `SP-NEG-056` | A Resolved Negotiation for a step MUST NOT be silently superseded by a later Negotiation for the same step and Capability Requirement; a revised Capability Advertisement or a revised Capability Requirement produces a new, separately reported Negotiation, consistent with `SP-ADP-021`. | `SP-ADP-021` · `PR-SAF-011` · `PR-RUN-011` |

### 6.13 Registry Interaction

This domain reads from the Runtime Registry and the Model Registry; it writes to neither.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-057` | This domain MUST read the Candidate Set from the Runtime Registry's discovery output and MUST NOT assemble a Candidate Set independently of it. | `BP-RUN-011` · `PR-ENV-004` |
| `SP-NEG-058` | This domain MUST NOT alter Runtime Registry or Model Registry state in the course of a Negotiation. | `AR-DEP-004` · `PR-ENV-011` |
| `SP-NEG-059` | This domain MUST treat a Runtime Registry entry's recorded availability, and a Model Registry entry's recorded reachability, as observed facts and MUST NOT infer either from the absence of a contrary report. | `BP-RUN-011` · `PR-SAF-011` |
| `SP-NEG-060` | This domain MUST NOT require an entry not currently discoverable through the Runtime Registry or the Model Registry to be evaluated as a candidate. | `PR-RUN-010` · `BP-ADP-004` |

### 6.14 Runtime Interaction

This domain is engaged by the Runtime Layer's capability-matching responsibility (AEOS-ARCH
Section 4.6) and completes before the Runtime Layer dispatches a request to an adapter.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-061` | This domain MUST complete for a step's Capability Requirement before the Runtime Layer dispatches any request for that step. | `BP-RUN-004` · `PR-WFL-016` |
| `SP-NEG-062` | This domain MUST NOT dispatch a request to any candidate; dispatch remains the Runtime Layer's own act, performed only after a Negotiation Result confirms compatibility and a Selection is recorded. | `AR-DEP-004` · `BP-RUN-004` · `AR-BND-003` |
| `SP-NEG-063` | This domain MUST NOT be required for the operation of any Workflow step that does not declare an Engineering Capability requirement. | `PR-RUN-010` · `BP-ADP-004` |
| `SP-NEG-064` | This domain's observable behavior MUST be identical across every supported Platform and Distribution Method. | `PR-PLT-003` · `PR-DST-005` |

### 6.15 Adapter Interaction

This domain is the Runtime-Blueprint-side counterpart to the Capability Negotiation responsibility
RUNTIME_ADAPTER_SPEC.md assigns to each adapter (`SP-ADP-018` through `SP-ADP-021`); it queries what an
adapter has declared and never requires an adapter to do more than answer.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-065` | This domain MUST query a candidate's Capability Advertisement through the Capability Query input RUNTIME_ADAPTER_SPEC.md defines, and MUST NOT require an invocation of the candidate's mediated Runtime to do so. | `SP-ADP-011` · `SP-ADP-018` · `AR-BND-002` |
| `SP-NEG-066` | This domain MUST treat a candidate's declared runtime compatibility conditions and declared version compatibility as authoritative for that candidate; it MUST NOT hold or infer a separate compatibility determination that conflicts with them. | `SP-ADP-022` · `SP-ADP-061` · `AR-BND-005` |
| `SP-NEG-067` | This domain MUST NOT require a candidate's declaring adapter to alter, extend, or narrow its Capability Advertisement to accommodate a specific step's Capability Requirement. | `SP-ADP-015` · `BP-ADP-003` · `PR-RUN-007` |
| `SP-NEG-068` | Where a candidate's Capability Advertisement changes between its query and the point a Negotiation Result is reported, this domain MUST report the change and MUST NOT report a Negotiation Result against a superseded Advertisement, consistent with `SP-ADP-021`. | `SP-ADP-021` · `PR-RUN-011` · `PR-SAF-011` |

### 6.16 Model Interaction

Where a candidate declares a Model, this domain MAY read that declaration through the Model Registry,
but it never requires one and never holds Model knowledge independently of the declaring adapter.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-069` | This domain MUST NOT require a candidate to declare a Model reference in order to be evaluated for compatibility. | `SP-ADP-026` · `PR-RUN-006` |
| `SP-NEG-070` | Where a candidate declares a Model, this domain MAY read that Model's declared Engineering Capability set from the Model Registry to inform Capability Matching, and MUST NOT hold that capability set independently of the Model Registry's current record. | `SP-MDL-012` · `SP-MDL-023` · `AR-BND-005` |
| `SP-NEG-071` | This domain MUST NOT rank, score, or compare Models against one another in the course of a Negotiation. | `SP-MDL-011` · `SP-MDL-048` · `AR-BND-011` |
| `SP-NEG-072` | A Model Registry query performed in the course of a Negotiation MUST be answerable without invoking any Runtime, and MUST NOT alter Model Registry state. | `SP-MDL-039` · `SP-MDL-040` · `AR-BND-002` |

### 6.17 Capability Interaction

This domain operates exclusively within the neutral Engineering Capability vocabulary the Capability
Vocabulary subsystem holds; it never introduces, on its own authority, a term the vocabulary does not
already express.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-073` | This domain MUST express every Capability Requirement, Capability Advertisement, Negotiation Result, and Negotiation Gap Report exclusively in terms the Capability Vocabulary subsystem holds. | `BP-RUN-003` · `PR-RUN-007` |
| `SP-NEG-074` | This domain MUST NOT introduce a new Engineering Capability term; extension of the vocabulary occurs only through the Runtime Blueprint's own capability vocabulary extension point (AEOS-BLUEPRINT Section 10.7). | `BP-RUN-003` · `AR-PRN-009` |
| `SP-NEG-075` | This domain MUST NOT treat a term expressed outside the neutral vocabulary — a Vendor's proprietary capability name, for example — as satisfying or contradicting a Capability Requirement. | `AR-BND-013` · `PR-RUN-002` · `BP-RUN-001` |
| `SP-NEG-076` | A revision to the neutral vocabulary MUST NOT alter the meaning of a Negotiation Result already reported for a step in progress. | `SP-ADP-017` · `PR-WFL-016` |

---

## 7. Constraints

The invariants this behavior domain operates under, per `MD5`. Unlike [Section 6](#6-behavior), which
states conditional rules for defined situations, these hold continuously across every Negotiation.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-NEG-077` | This domain MUST NOT select, choose, rank, or recommend a candidate under any circumstance; every rule in this document that appears to narrow toward one candidate states a compatibility fact, never a choice. | `PR-RUN-004` · `AR-BND-010` · `AR-BND-011` |
| `SP-NEG-078` | This domain MUST NOT hold durable state of its own; a Negotiation's current state ([Section 6.12](#612-negotiation-lifecycle)) is available for the duration of its evaluation and is not a Repository Asset in its own right. | `AR-BND-007` · `PR-REP-015` |
| `SP-NEG-079` | This domain MUST NOT hold, request, or transmit a credential. | `AR-BND-009` · `PR-SAF-006` · `PR-RUN-014` |
| `SP-NEG-080` | This domain MUST NOT address the External AI Layer, directly or indirectly, under any rule in this document. | `AR-BND-002` · `AR-EXT-005` · `AR-DEP-004` |
| `SP-NEG-081` | This domain's determination of compatibility MUST be reproducible: the same Capability Requirement, Candidate Set, and set of declarations MUST produce the same Negotiation Results. | `PR-NFR-002` · `PR-RUN-008` |
| `SP-NEG-082` | This domain MUST NOT privilege one Vendor, Runtime, or Model over another in any rule it applies. | `AR-BND-011` · `PR-RUN-003` |
| `SP-NEG-083` | This domain MUST NOT be required for the operation of any capability that does not depend on an Engineering Capability requirement. | `BP-ADP-004` · `PR-RUN-010` |
| `SP-NEG-084` | This domain's observable behavior MUST NOT vary by the Distribution Method through which AEOS was delivered. | `PR-DST-006` · `AR-LAY-007` |

---

## 8. Extension Points

Where and how this behavior domain admits future variation without altering a rule already stated in
[Section 6](#6-behavior) or [Section 7](#7-constraints), per `MD6`. Each states, per `EX3`, the
boundary within which variation is permitted and the point beyond which a new identifier or a revision
under AEOS-SPECSTD Section 18 is required.

| ID | Extension point | Boundary | Traces to |
| :--- | :--- | :--- | :--- |
| `SP-NEG-085` | **Candidate Category Admission.** A candidate mediating a category of Runtime not previously evaluated by this domain MAY be evaluated without a change to this Specification. | The candidate MUST satisfy every rule in [Sections 6](#6-behavior) and [7](#7-constraints) without exception for its category. | `PR-RUN-016` · `BP-ADP-005` |
| `SP-NEG-086` | **Capability Vocabulary Extension.** Where the Capability Vocabulary subsystem admits a new term, this domain MAY evaluate a Capability Requirement or Capability Advertisement expressed against it without a change to this Specification. | The extension MUST NOT alter the meaning of a term already in use, per `SP-NEG-074`. | `BP-RUN-003` · `AR-PRN-009` |
| `SP-NEG-087` | **Configured Constraint Extension.** A project MAY declare an additional configuration constraint under [Section 6.5](#65-constraint-evaluation) as multi-Runtime orchestration is configured for further step categories. | The extension MUST NOT exclude a candidate on a basis this domain has not been told to apply, per `SP-NEG-021`. | `PR-RUN-013` |
| `SP-NEG-088` | **Version Compatibility Extension.** This Specification MAY be revised to a later Minor or Patch version without invalidating a Negotiation Result already reported under a prior Minor or Patch version. | A Major version of this Specification requires every dependent rule to be reconciled before new Negotiations rely on it. | `PR-NFR-002` · `PR-RUN-012` |
| `SP-NEG-089` | **Companion Domain Admission.** A future Specification governing Boundary Disclosure Assembly, or the remainder of the `BP-RUN` behavior domain this document does not cover, MAY be authored and depended upon without altering a rule already stated here. | The companion domain MUST NOT relocate a responsibility this document assigns, per `BP-GOV-009`. | `BP-GOV-009` · `PR-NFR-006` |

An extension admitted under this section MUST NOT be used to weaken a rule stated in
[Section 6](#6-behavior) or [Section 7](#7-constraints), per `EX4`. A weakening disguised as an
addition is a defect in the extending declaration, not a use of these extension points.

---

## 9. Traceability

Every rule in [Section 6](#6-behavior), [Section 7](#7-constraints), and
[Section 8](#8-extension-points) states its own trace inline, per `TR1` and `TR2`. This section
consolidates that trace as the acceptance-criteria index `MD9` requires: a reviewer or an automated
test can confirm conformance for a given `PR-`, `BP-`, or `AR-` identifier by locating every `SP-NEG-`
rule that cites it and checking the described observable behavior against an implementation.

### 9.1 Product Requirements

| `PR-` identifier | `SP-NEG-` rules that trace to it |
| :--- | :--- |
| `PR-RUN-001` | `004` `008` |
| `PR-RUN-002` | `003` `075` |
| `PR-RUN-003` | `021` `024` `025` `027` `082` |
| `PR-RUN-004` | `001` `025` `026` `029` `030` `031` `032` `033` `034` `077` |
| `PR-RUN-006` | `016` `069` |
| `PR-RUN-007` | `001` `009` `012` `039` `067` `073` |
| `PR-RUN-008` | `010` `013` `043` `081` |
| `PR-RUN-010` | `006` `023` `034` `036` `041` `049` `060` `063` `083` |
| `PR-RUN-011` | `037` `045` `048` `051` `054` `056` `068` |
| `PR-RUN-012` | `017` `088` |
| `PR-RUN-013` | `020` `028` `087` |
| `PR-RUN-014` | `079` |
| `PR-RUN-016` | `085` |
| `PR-SAF-002` | `048` `054` |
| `PR-SAF-005` | `002` `044` |
| `PR-SAF-006` | `079` |
| `PR-SAF-010` | `035` `047` |
| `PR-SAF-011` | `012` `014` `019` `022` `037` `040` `042` `046` `050` `051` `052` `053` `056` `059` `068` |
| `PR-WFL-005` | `007` |
| `PR-WFL-007` | `055` |
| `PR-WFL-009` | `044` `045` `046` |
| `PR-WFL-010` | `046` |
| `PR-WFL-016` | `011` `032` `039` `061` `076` |
| `PR-NFR-001` | `022` `038` `042` `050` `052` `055` |
| `PR-NFR-002` | `017` `081` `088` |
| `PR-NFR-006` | `005` `089` |
| `PR-ENV-004` | `014` `015` `057` |
| `PR-ENV-011` | `058` |
| `PR-REP-015` | `078` |
| `PR-PLT-002` | `018` |
| `PR-PLT-003` | `018` `064` |
| `PR-DST-005` | `064` |
| `PR-DST-006` | `084` |

### 9.2 Architecture and Blueprint

| `AR-` / `BP-` identifier | `SP-NEG-` rules that trace to it |
| :--- | :--- |
| `AR-BND-002` | `004` `008` `065` `072` `080` |
| `AR-BND-003` | `062` |
| `AR-BND-004` | `002` |
| `AR-BND-005` | `003` `066` `070` |
| `AR-BND-007` | `078` |
| `AR-BND-009` | `079` |
| `AR-BND-010` | `002` `030` `033` `077` |
| `AR-BND-011` | `021` `024` `025` `027` `028` `033` `071` `077` `082` |
| `AR-BND-013` | `075` |
| `AR-DEP-004` | `005` `007` `058` `062` `080` |
| `AR-PRN-007` | `038` `040` `053` |
| `AR-PRN-008` | `006` `019` `023` `034` `036` `041` `049` |
| `AR-PRN-009` | `074` `086` |
| `AR-EXT-005` | `008` `080` |
| `AR-LAY-007` | `018` `084` |
| `BP-RUN-001` | `003` `075` |
| `BP-RUN-002` | `004` |
| `BP-RUN-003` | `009` `073` `074` `086` |
| `BP-RUN-004` | `009` `010` `011` `013` `020` `043` `061` `062` |
| `BP-RUN-005` | `001` `026` `029` `031` |
| `BP-RUN-009` | `035` `047` |
| `BP-RUN-011` | `057` `059` |
| `BP-RUN-013` | `010` |
| `BP-ADP-003` | `067` |
| `BP-ADP-004` | `060` `063` `083` |
| `BP-ADP-005` | `085` |
| `BP-ADP-009` | `032` `034` |
| `BP-GOV-009` | `089` |

### 9.3 Depended-Upon Specification Rules

Per `DP1`, this table records the `SP-ADP-` and `SP-MDL-` identifiers this document depends on and the
`SP-NEG-` rules that cite them; it is a declared dependency, not a restatement of either document's
content, consistent with `XR5`.

| Depended-upon identifier | `SP-NEG-` rules that cite it |
| :--- | :--- |
| `SP-ADP-011` | `065` |
| `SP-ADP-015` | `067` |
| `SP-ADP-017` | `076` |
| `SP-ADP-018` | `065` |
| `SP-ADP-020` | `012` |
| `SP-ADP-021` | `056` `068` |
| `SP-ADP-022` | `015` `066` |
| `SP-ADP-023` | `014` `015` |
| `SP-ADP-025` | `016` |
| `SP-ADP-026` | `016` `069` |
| `SP-ADP-061` | `066` |
| `SP-ADP-062` | `017` |
| `SP-MDL-011` | `071` |
| `SP-MDL-012` | `070` |
| `SP-MDL-023` | `070` |
| `SP-MDL-039` | `072` |
| `SP-MDL-040` | `072` |
| `SP-MDL-048` | `071` |

Per AEOS-BLUEPRINT `BP-GOV-008`, this document traces to at least one Blueprint identifier and at
least one `PR-` identifier for every rule; the tables above and each rule's own row together satisfy
that obligation.

---

## 10. Non-goals

Behavior a reader might reasonably expect this domain to cover, which it deliberately does not, per
`MD8`.

| Non-goal | Reason |
| :--- | :--- |
| A selection algorithm, ranking algorithm, or scoring function for choosing among compatible candidates | Prohibited by this document's own scope (see [Section 1](#1-purpose)) and by `AR-BND-011`; Selection is the user's act, custodied by `BP-RUN-005`, never this domain's. |
| The mechanics of Approval Gate placement or how Human Approval is obtained | Owned by the Human Interaction Blueprint's behavior domain. |
| Assembling or presenting the disclosure of what will cross the boundary to a Runtime, and its expected cost | Owned by the Runtime Blueprint's Boundary Disclosure Assembly responsibility (`BP-RUN-006`, `BP-RUN-007`), a distinct behavior domain not covered here. |
| A Model's identity, metadata, classification, or declared capability set as registered | Owned by MODEL_REGISTRY.md (AEOS-SPEC-MDL). |
| An adapter's identity, lifecycle, registration, discovery, or its own Capability Negotiation responsibility | Owned by RUNTIME_ADAPTER_SPEC.md (AEOS-SPEC-ADP). |
| The observable behavior and governance of the Runtime Registry itself | Owned by `RUNTIME_REGISTRY.md`, a Runtime document. |
| An HTTP endpoint, JSON schema, or wire format for a Negotiation Query or a Negotiation Result | Prohibited by `MN4`; a technology and protocol decision outside the Specification layer. |
| A transport mechanism, process model, or concurrency model for how Negotiation actually executes, or the order in which candidates are internally evaluated | Owned by Runtime documents, per AEOS-BLUEPRINT Section 18, which this document is not. |
| A weighting, cost-comparison, or benchmarking scheme for comparing compatible candidates | Excluded from the product itself, per AEOS-PRD Appendix A, R6, and by `AR-BND-011`; restating it here would exceed this document's authority. |
| A new Engineering Capability vocabulary term, or a change to the neutral vocabulary's meaning | Owned by the Runtime Blueprint's Capability Vocabulary subsystem (`BP-RUN-003`) and its own extension point (AEOS-BLUEPRINT Section 10.7). |
| A statement of which component, service, or process realizes Negotiation | Prohibited by `MN7`; this Specification binds the behavior domain, not any structural element. |

---

## 11. References

### 11.1 Governing Documents

| Document | Cited for |
| :--- | :--- |
| AEOS-VISION | The invariants this Specification MUST NOT trade away, per AEOS-SPECSTD Section 3.1 — in particular that a Runtime selection belongs to the user and is never overridden or silently substituted. |
| AEOS-PRD | The `PR-RUN-001` through `PR-RUN-016`, `PR-SAF-`, `PR-WFL-`, `PR-NFR-`, `PR-ENV-004`, `PR-ENV-011`, `PR-REP-015`, `PR-PLT-`, and `PR-DST-` identifiers every rule in this document traces to. |
| AEOS-GLOSSARY | The definitions of Runtime, Runtime Adapter, Model, Vendor, Engineering Capability, Repository Asset, Profile, and every other term this document uses without redefining. |
| AEOS-DOCSTD | The form, structure, language, and lifecycle every AEOS document takes, including this one. |
| AEOS-SPECSTD | The form, structure, identifier convention, and traceability rules specific to the Specification layer, applied throughout this document. |
| AEOS-ARCH | The layer model and the boundary invariants this Specification is written against, in particular `AR-BND-002`, `AR-BND-003`, `AR-BND-004`, `AR-BND-005`, `AR-BND-007`, `AR-BND-009`, `AR-BND-010`, `AR-BND-011`, `AR-BND-013`, `AR-DEP-004`, `AR-PRN-007`, `AR-PRN-008`, `AR-PRN-009`, `AR-EXT-005`, and `AR-LAY-007`, and Section 6.5's "Absence Reduces Options" principle, which [Section 6.8](#68-fallback-behavior) applies. |
| AEOS-BLUEPRINT | The Runtime Blueprint (Section 10, `BP-RUN`) and the Adapter Blueprint (Section 11, `BP-ADP`), whose arrangement this Specification states Negotiation behavior for, and Sections 17 and 19, which fix this Specification's boundary against the Blueprint layer. |
| RUNTIME_ADAPTER_SPEC.md (AEOS-SPEC-ADP) | The `SP-ADP-` rules this Specification depends on, per [Section 3.4](#34-declared-dependencies) and [Section 9.3](#93-depended-upon-specification-rules). |
| MODEL_REGISTRY.md (AEOS-SPEC-MDL) | The `SP-MDL-` rules this Specification depends on, per [Section 3.4](#34-declared-dependencies) and [Section 9.3](#93-depended-upon-specification-rules). |

### 11.2 Forward References and a Note on Identifier Families (Non-normative)

The following documents govern behavior domains adjacent to this one. Consistent with `DS-P-10`, their
relationship to this document is recorded here as a finished statement rather than worked around by
inventing content this document has no authority to state.

| Document | Adjacent behavior domain | Relationship to this document |
| :--- | :--- | :--- |
| `AEOS_RUNTIME_FLOW.md` (AEOS-RTF) | The observable runtime lifecycle of AEOS, including the Runtime Execution phase within which this domain's behavior is engaged. | A sibling Runtime document, not a Specification. Its `RTF-` identifiers are document-local, per AEOS-RTF's own authority statement, and are cited here only by document name, never as a governing trace. |
| `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG) | The observable behavior and governance of the Runtime Registry, including discovery, capability lookup, and availability, which [Section 6.13](#613-registry-interaction) reads from. | A sibling Runtime document. Its `RT` identifier prefix is proposed, not adopted by AEOS-GLOSSARY at the time of this document's authorship, per AEOS-GLOSSARY `I4`. No rule in this document cites an `RT-REG-` identifier as a governing trace, consistent with the discipline RUNTIME_ADAPTER_SPEC.md and MODEL_REGISTRY.md already apply to it. |
| A prospective Boundary Disclosure Specification | The Runtime Blueprint's Boundary Disclosure Assembly responsibility (`BP-RUN-006`, `BP-RUN-007`) and the remainder of the `BP-RUN` behavior domain this document does not cover. | Not yet authored at the time of this document's authorship. `SP-NEG-089` records the extension point by which it may later be admitted. |
| A prospective Runtime Capability Specification | A capability-registry-facing behavior domain distinct from Model Registry, Runtime Registry, and Negotiation. | No such document exists in the repository at the time of this document's authorship. No rule in this document depends on one, and this document introduces no `RC` or `CR` identifier prefix, consistent with AEOS-GLOSSARY `I4`. |
| `AEOS_SPEC.md` | A prospective index of the Specification layer as a whole. | Not consulted in the authorship of this document beyond AEOS-SPECSTD, which governs the Specification layer directly. Where such an index is later authored, this document is expected to be listed under the `NEG` area it registers. |

No rule in this document depends, by identifier, on any of the five documents above, per `DP1` and
`DP2`. Once each is authored or adopted, it MUST be consulted to confirm that no implicit dependency
has been introduced into this document in the interval, per `DP5`.

---

## 12. Document Governance

### 12.1 Status

This document is authored as the Freeze candidate for the Runtime Negotiation behavior domain of AEOS
1.0. Its current lifecycle state is **Draft**, per AEOS-DOCSTD Section 12.1: it is not yet
authoritative and MUST NOT be referenced as a source of truth until it has passed Review, Revision,
and Approval under AEOS-DOCSTD Section 12.2 and the Freeze checklist AEOS-SPECSTD Section 19.2 states.
Downstream Implementation Guides, tests, issues, and pull requests MAY reference the `SP-NEG-`
identifiers stated here in anticipation of Freeze, consistent with `TR5`, but MUST treat them as
subject to revision until this document's status changes.

### 12.2 Change Control

Versioning and change control for this document follow AEOS-SPECSTD Section 18.1 without
modification: a Patch increment for an editorial correction with no change of meaning; a Minor
increment for the addition of a new `SP-NEG-` identifier that does not alter what an existing
identifier requires; a Major increment for any change to what an existing identifier requires, any
retirement, or any change to the `NEG` area registration itself.

### 12.3 Review Policy

Review of this document follows AEOS-DOCSTD Sections 12.3 and 12.4: findings are classified Critical,
Major, Minor, or Nitpick; a reviewer identifies inconsistencies without redesigning the document under
review; and freeze is recommended only once no Critical or Major finding remains open. Before freeze
is recommended, a reviewer additionally applies the checklist in AEOS-SPECSTD Section 19.2 to this
document's content.

### 12.4 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, or AEOS-GLOSSARY | The higher-authority document governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-DOCSTD or AEOS-SPECSTD on form, structure, or identifier convention | The governing Standard governs. This document is corrected. |
| This document conflicts with AEOS-ARCH or AEOS-BLUEPRINT on a structural decision or an assigned responsibility | AEOS-ARCH or AEOS-BLUEPRINT governs. This document is corrected, or the conflict is escalated if this document's author believes the structural decision is wrong. |
| This document conflicts with RUNTIME_ADAPTER_SPEC.md or MODEL_REGISTRY.md on a rule either already states | Escalated to the owner, per AEOS-SPECSTD Section 3.3. Neither document is resolved unilaterally; this document's dependencies are declared, not authoritative over either. |
| A Negotiation's observed behavior conflicts with a rule this document states | This document governs. The observed behavior is nonconformant until corrected. |
| This document conflicts with a future companion Specification governing the remainder of `BP-RUN`'s arrangement, once authored | Escalated to the owner, per AEOS-SPECSTD Section 3.3. Neither document is resolved unilaterally. |

### 12.5 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial authoring of the Runtime Negotiation Specification. Registers the `NEG` area code for the Specification layer, naming a distinct behavior domain within the arrangement AEOS-BLUEPRINT Section 10 (`BP-RUN`) establishes. States 89 `SP-NEG-` rules across seventeen behavior areas — Negotiation ownership, Negotiation participants, Capability Matching, Compatibility Evaluation, Constraint Evaluation, Preference Handling, Selection Principles, Fallback Behavior, Failure Behavior, Cancellation Behavior, Timeout Behavior, the Negotiation Lifecycle, Registry Interaction, Runtime Interaction, Adapter Interaction, Model Interaction, and Capability Interaction — eight cross-cutting constraints, and five extension points, each traced to one or more `PR-`, `BP-`, or `AR-` identifiers, with explicit dependencies declared against `SP-ADP-` and `SP-MDL-` rules per `DP1`. Defines Inputs and Outputs for the domain and states eleven explicit Non-goals, foremost among them any selection, ranking, or scoring algorithm. Introduces no product requirement, no architectural decision, no Blueprint arrangement, and no implementation; redefines no term AEOS-GLOSSARY owns and relocates no responsibility AEOS-BLUEPRINT assigns. |

---

## Appendix A — Identifier Index

**Non-normative.** Indexes the identifier ranges stated in the document body. Where it and the body
differ, the body governs.

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `SP-NEG-001`–`004` | Negotiation Ownership | 4 | [6.1](#61-negotiation-ownership) |
| `SP-NEG-005`–`008` | Negotiation Participants | 4 | [6.2](#62-negotiation-participants) |
| `SP-NEG-009`–`013` | Capability Matching | 5 | [6.3](#63-capability-matching) |
| `SP-NEG-014`–`019` | Compatibility Evaluation | 6 | [6.4](#64-compatibility-evaluation) |
| `SP-NEG-020`–`024` | Constraint Evaluation | 5 | [6.5](#65-constraint-evaluation) |
| `SP-NEG-025`–`028` | Preference Handling | 4 | [6.6](#66-preference-handling) |
| `SP-NEG-029`–`033` | Selection Principles | 5 | [6.7](#67-selection-principles) |
| `SP-NEG-034`–`038` | Fallback Behavior | 5 | [6.8](#68-fallback-behavior) |
| `SP-NEG-039`–`043` | Failure Behavior | 5 | [6.9](#69-failure-behavior) |
| `SP-NEG-044`–`047` | Cancellation Behavior | 4 | [6.10](#610-cancellation-behavior) |
| `SP-NEG-048`–`051` | Timeout Behavior | 4 | [6.11](#611-timeout-behavior) |
| `SP-NEG-052`–`056` | Negotiation Lifecycle | 5 | [6.12](#612-negotiation-lifecycle) |
| `SP-NEG-057`–`060` | Registry Interaction | 4 | [6.13](#613-registry-interaction) |
| `SP-NEG-061`–`064` | Runtime Interaction | 4 | [6.14](#614-runtime-interaction) |
| `SP-NEG-065`–`068` | Adapter Interaction | 4 | [6.15](#615-adapter-interaction) |
| `SP-NEG-069`–`072` | Model Interaction | 4 | [6.16](#616-model-interaction) |
| `SP-NEG-073`–`076` | Capability Interaction | 4 | [6.17](#617-capability-interaction) |
| `SP-NEG-077`–`084` | Constraints | 8 | [7](#7-constraints) |
| `SP-NEG-085`–`089` | Extension Points | 5 | [8](#8-extension-points) |
| **Total** | | **89** | — |

---

**End of Runtime Negotiation Specification**

AEOS-SPEC-NEG · Version 1.0.0 · Traces to `PR-RUN-001`–`PR-RUN-004` · `PR-RUN-006`–`PR-RUN-008` ·
`PR-RUN-010`–`PR-RUN-014` · `PR-RUN-016` · `PR-SAF-002` · `PR-SAF-005` · `PR-SAF-006` · `PR-SAF-010` ·
`PR-SAF-011` · `PR-WFL-005` · `PR-WFL-007` · `PR-WFL-009` · `PR-WFL-010` · `PR-WFL-016` · `PR-NFR-001` ·
`PR-NFR-002` · `PR-NFR-006` · `PR-ENV-004` · `PR-ENV-011` · `PR-REP-015` · `PR-PLT-002` · `PR-PLT-003` ·
`PR-DST-005` · `PR-DST-006`
