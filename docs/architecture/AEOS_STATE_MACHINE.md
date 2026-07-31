# AI Engineering Operating System

## AEOS — State Machine Specification

*The permanent statement of the specified, observable behavior of state within AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | State Machine Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SPEC-STM |
| **Version** | 1.0.0 |
| **Status** | Freeze candidate |
| **Owner** | Product Owner, AEOS |
| **Author** | Chief Specification Architect, AEOS |
| **Audience** | Architects, implementers, reviewers, test authors, and AI runtimes consuming this repository |
| **Suggested path** | `docs/specification/STATE_MACHINE_SPECIFICATION.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `AEOS_SPEC.md` (AEOS-SPEC) · `AEOS_WORKFLOW_ENGINE.md` (AEOS-SPEC-WFL) |
| **Supersedes** | None |
| **Area code** | `STM` |

> **Authority of this document.**
> This document specifies, precisely and testably, the **observable behavior of state** within
> AEOS: the general model, ownership pattern, lifecycle, closed set of categories, and transition
> rules that govern any entity whose position AEOS-GLOSSARY records as durable, resumable state,
> beginning with the concept that Glossary defines as **Workflow State**. It registers the `STM`
> behavior domain under AEOS-SPECSTD Section 11.4.
>
> This document does not attach at an `EPS-1` through `EPS-7` extension point of AEOS-SPEC. The
> behavior it specifies does not correspond to one of the seven architectural layers AEOS-ARCH
> Section 4 names; it is, in the sense AEOS-ARCH Sections 4.4 and 4.9 already establish, jointly
> owned by the Workflow Layer's state-maintenance responsibility and the Repository Layer's
> state-custody responsibility. This document does not, on its own authority, introduce an eighth
> extension point or a new architectural layer, consistent with `SP-SYS-EXT-4`. It exists so that
> the Workflow behavior domain (`WFL`, attached at `EPS-2`) and any future domain-specific
> Specification whose entities carry durable, resumable state are written against one fixed state
> model rather than each re-deriving it — in the sense AEOS-SPEC's own Purpose states for why the
> `SYS` domain exists, applied here to the narrower, cross-cutting subject of state rather than to
> the complete interaction contract `SYS` already owns.
>
> It defines no vision, no product requirement, no terminology, no architecture, no Blueprint
> arrangement, no interface, no algorithm, no technology, and no implementation. It redefines
> nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPEC;
> where a statement here appears to, that is a defect in this document and MUST be reported rather
> than acted upon. It does not restate, narrow, widen, or contradict a rule AEOS-SPEC-WFL already
> states for a Workflow's steps — in particular `SP-WFL-018` and `SP-WFL-023` through `SP-WFL-034`
> — and depends on those rules, and on `SP-SYS-050`, under AEOS-SPECSTD Section 15, without
> restating them; [Section 7.3](#73-declared-dependencies) states the complete dependency set. It
> is written entirely under AEOS-SPECSTD, which governs its form, structure, identifier convention,
> traceability, and lifecycle; where this document and AEOS-SPECSTD both speak to a subject,
> AEOS-SPECSTD governs the form and this document governs the content of the `STM` behavior domain.
> Where this document and a document of higher authority both speak to the same subject, the
> higher-authority document governs and the conflict is a defect to be reported.

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
13. [Appendix A — SP-STM Rule Index](#appendix-a--sp-stm-rule-index)

---

## 1. Purpose

AEOS-ARCH states that durable state exists only within the Repository Layer (`AR-LAY-006`) and that
an owned concept — including Workflow State maintenance — belongs to exactly one layer alone
(`AR-LAY-009`). AEOS-ARCH Section 4.4 assigns Workflow State maintenance to the Workflow Layer;
Section 4.9 assigns Workflow State custody to the Repository Layer. AEOS-BLUEPRINT arranges both
sides of that division — Cycle Position Keeping, Outcome Recording, and Halt Handling within the
Workflow Blueprint (`BP-WFL-007`, `BP-WFL-009`, `BP-WFL-012`), and Workflow State Custody and
Decision Record Custody within the Repository Blueprint (`BP-REP-007`, `BP-REP-008`) — without
stating what a state is, how many categories exist, which transitions between them are valid, or
what an invalid transition requires. AEOS-SPEC-WFL, in turn, states forty-four rules realizing that
arrangement precisely for a Workflow's own steps: cycle position (`SP-WFL-018`), outcome recording
(`SP-WFL-023` through `SP-WFL-025`), halt handling (`SP-WFL-026` through `SP-WFL-028`), pause and
resume (`SP-WFL-029` through `SP-WFL-031`), visibility (`SP-WFL-032`), and custody (`SP-WFL-033`,
`SP-WFL-034`) — but it states these as behavior of the Workflow Engine specifically, for a Workflow's
steps specifically, and it neither needs nor claims to state the general shape every one of those
rules already presupposes: what a state category is, how many exist, what makes a transition between
them valid, and what happens when a transition that is not valid is attempted.

This document is that general shape. It states, once, the closed set of state categories an entity
governed by this model may occupy; the lifecycle an entity's state follows from its first appearance
to a terminal outcome; the complete table of valid transitions between categories and the condition
each requires; the handling required when an attempted transition is not in that table; the specific
behavior of state while an entity is held for a Human Approval decision; the persistence, resume, and
recovery responsibilities a durable, resumable state carries; the distinct behavior a state carries
when it results from failure, from cancellation, or from completion; the consistency a recorded state
MUST maintain; and the visibility a recorded state MUST offer on request. It states nothing about how
any one behavior domain's entities are sequenced, evaluated, or classified — that is, and remains,
each domain's own Specification, beginning with AEOS-SPEC-WFL for a Workflow's steps.

Precision matters here specifically because AEOS-PRD `PR-SAF-010` requires that an interruption, at
any point, leave the project in a state AEOS can describe, and `PR-WFL-008` requires that a paused
Workflow resume without losing position or re-establishing context. Neither requirement is met by an
implementation that merely avoids crashing; both are met only by a state model precise enough that
"describable" and "resumable" are testable properties of a specific, recorded value — which is what
this document exists to make them.

---

## 2. Scope

### 2.1 Behavior Domain and Area Registration

This document registers the area code `STM` under AEOS-SPECSTD Section 11.4. No existing
Specification document has registered `STM`; the code is available for registration by this
document, consistent with `ID5`. The `STM` behavior domain is the general, cross-cutting observable
behavior of state: the closed set of state categories, the ownership pattern separating the decision
to transition from the custody of the durable record, the lifecycle from an entity's first
appearance to a terminal outcome, the complete valid-transition table and the handling of any
attempted transition outside it, the behavior of state at a Human Approval decision, the
persistence, resume, and recovery responsibilities durable state carries, the distinct behavior of a
failure-caused, a cancellation-caused, and a completion-caused terminal or halted condition, the
consistency a recorded state MUST maintain, and the visibility a recorded state MUST offer.

This document's area code does not match a single `PR-` prefix's area segment. AEOS-SPECSTD
Section 11.4 recommends, as a SHOULD, that an area code match the corresponding `PR-` prefix where a
Specification domain corresponds directly to one AEOS-PRD capability; the `STM` behavior domain does
not correspond to one capability. It generalizes behavior that `PR-WFL`, `PR-REP`, `PR-SAF`, and
`PR-TDD` each partially establish for the concept AEOS-GLOSSARY names Workflow State, in the same
sense that AEOS-SPEC's own `SYS` code — cited as precedent, not restated — does not match a single
`PR-` prefix either.

### 2.2 Relationship to the System Specification and to the Workflow Engine Specification

This document does not attach at any of the seven extension points AEOS-SPEC Section 8.1 declares.
Consistent with `SP-SYS-EXT-4`, an eighth extension point — naming a functional area beyond the
seven AEOS-ARCH and AEOS-BLUEPRINT recognize — requires prior Architecture or Blueprint recognition
this document does not have and does not seek; this document creates no such extension point on its
own authority, and states no rule that requires one. Instead, this document depends on AEOS-SPEC's
`SP-SYS-050` and on AEOS-SPEC-WFL's state-related rules, cited by identifier and not restated,
consistent with AEOS-SPECSTD `DP1` and `DP4`. The complete, declared dependency set is stated in
[Section 7.3](#73-declared-dependencies).

AEOS-SPEC-WFL is not required to change for this document to exist, and this document does not
require AEOS-SPEC-WFL to record a formal binding under [Section 8](#8-extension-points) for its
existing rules to remain valid: `SP-WFL-018` and `SP-WFL-023` through `SP-WFL-034` already realize,
for a Workflow's steps, exactly the category and transition model [Section 6](#6-behavior) states in
general form, and this document treats that realization as a fact to depend upon rather than a
change to request. A future revision of AEOS-SPEC-WFL MAY record that binding explicitly under
[Section 8.1](#81-state-machine-extension-points); this document does not require it to.

The Context behavior domain is deliberately absent from this document's dependency set. AEOS-ARCH
Section 4.5 and AEOS-GLOSSARY's Context Router entry state that the Context Layer holds no durable
state; this document's state model accordingly has no entity to govern there, and this document
cites no `SP-CTX` identifier for that reason, stated here so the absence is not mistaken for an
oversight.

### 2.3 Boundary of This Document

This document specifies the general state model alone. It does not specify which concrete
conditions, causes, or vocabulary a Workflow's steps use to realize the categories this document
fixes — that is AEOS-SPEC-WFL's `WFL` behavior domain; how a step's precondition is evaluated, an
effect classified, or a gate placed — also `WFL`; how a decision is collected from a person, or a
Proposal, an Explanation, a Status, or a Report is assembled — the Human Interaction behavior domain;
how an approved effect is performed or an Environment observed — the Execution behavior domain; or
how a state's durable record is stored, versioned, or physically written — the Repository behavior
domain. The resulting exclusions are stated in full in [Section 10](#10-non-goals).

---

## 3. Responsibilities

This Specification is answerable for:

- The closed set of state categories an entity governed by this model may occupy, and the meaning
  of each.
- The separation between the behavior domain that decides a transition and the behavior domain that
  holds the durable record of the resulting state.
- The lifecycle an entity's state follows, from its first appearance to a terminal category.
- The complete table of valid transitions between categories, and the condition each requires.
- The handling required when an attempted transition is not in that table.
- The behavior of state while an entity is held for an outstanding Human Approval decision,
  including where an Automation Grant resolves that hold.
- The persistence responsibility a transition carries before it may be presented as complete.
- The resume responsibility a held entity carries, and the recovery responsibility a durably
  recorded state carries against the Repository's actual condition.
- The distinct behavior a state carries when it results from a failed step, from a decision not to
  continue, or from reaching declared success criteria.
- The consistency a recorded state MUST maintain across time and across counterparties, and its
  visibility on request.
- The mechanism by which a domain-specific Specification binds its own concrete state values to the
  categories and transitions this document fixes.

This Specification is **not** answerable for:

- The concrete state values, conditions, or causes specific to any one behavior domain's entities —
  for a Workflow's steps, owned by AEOS-SPEC-WFL and bound to this model under
  [Section 8](#8-extension-points).
- The sequencing of a Workflow's steps, the evaluation of preconditions, or the classification of
  intended effects — owned by the Workflow behavior domain.
- The collection of a human decision, or the assembly of a Proposal, an Explanation, a Status, or a
  Report — owned by the Human Interaction behavior domain and stated at the system level by
  `SP-SYS-004` through `SP-SYS-007`.
- The performance of an approved effect, or the observation of the Environment before or after it —
  owned by the Execution behavior domain.
- The storage, versioning, or physical writing of a durable state record — owned by the Repository
  behavior domain.
- The selection or composition of Context — the Context behavior domain holds no durable state and
  this document accordingly governs no entity there.
- Any structural decision, layer boundary, or dependency direction — owned by AEOS-ARCH and
  AEOS-BLUEPRINT.
- Any data structure, interface, storage mechanism, process model, or algorithm realizing the
  behavior below — owned by Implementation Guides, which do not yet exist for this domain.

---

## 4. Inputs

The inputs below are the material every rule in [Section 6](#6-behavior) operates on. An input's
validity condition is part of the behavior this document specifies; an invalid input is a defined
condition, not an unhandled one.

| Input | Required properties | Valid when | Invalid when |
| :--- | :--- | :--- | :--- |
| Transition-triggering event | An event drawn from the set [Section 6.6](#66-valid-transition-rules) names, identifying exactly one entity this state model governs | It matches one of Section 6.6's table rows for the entity's current category | It names a condition not in Section 6.6's table, identifies more than one entity, or identifies no entity |
| Durably recorded prior state (from the Repository behavior domain) | The entity's last durably recorded category and condition, or an indication that the entity has not yet transitioned | It resolves to exactly one category in [Section 6.4](#64-state-categories)'s closed set, or correctly indicates no prior transition | It resolves to no category in Section 6.4's closed set, or to more than one |
| Collected decision (from the Human Interaction behavior domain) | An explicit act answering exactly one outstanding gate held for one entity | Explicit, unambiguous, and answering the specific entity's outstanding gate | Silent, ambiguous, inferred, or answering a different entity's gate |
| Automation Grant (optional) | An explicit, scoped, recorded, revocable delegation of approval authority | Its scope covers the Action Class of the intended effect and that class is not Destructive | Its scope does not cover the intended effect's Action Class, it covers a Destructive effect, or it has been revoked |
| Repository observation of actual condition (for recovery) | A report, from the Repository behavior domain, of an entity's actual durable condition as currently observed | Obtained at or after the moment [Section 6.11](#611-recovery-responsibilities) requires it | Obtained before a durable write this document requires has completed, or presumed rather than obtained |

---

## 5. Outputs

Every output below is externally observable, contractual behavior; none names an implementation
artifact.

| Output | Produced when |
| :--- | :--- |
| Current state report — an entity's current category and its recorded condition | On request, per `SP-STM-030`, and whenever a transition in [Section 6](#6-behavior) occurs |
| Transition record — the category reached, together with the input that caused it | Whenever a valid transition under [Section 6.6](#66-valid-transition-rules) occurs, per `SP-STM-010` |
| Invalid-transition report | Whenever an attempted transition is rejected under [Section 6.7](#67-invalid-transition-handling) |
| Divergence report | Whenever a recovery check under [Section 6.11](#611-recovery-responsibilities) finds the durably recorded state does not match the Repository's actual condition |
| Resume-readiness confirmation | Whenever an entity transitions from Held to Advancing, per [Section 6.10](#610-resume-responsibilities) |

---

## 6. Behavior

Every rule in this section MUST hold for every entity this state model governs, regardless of which
behavior domain owns that entity's state maintenance, consistent with [Section 7.2](#72-domain-neutrality-constraints).

### 6.1 State Model

> **On this section, non-normative.** This section is this document's testable realization of
> AEOS-GLOSSARY's Workflow State definition, elaborating the value space and determinacy that
> definition presupposes but does not itself enumerate. [Section 6.4](#64-state-categories) states
> the closed category set this section refers to.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-001` | At any moment, an entity governed by this state model MUST occupy exactly one state category, drawn from the closed set [Section 6.4](#64-state-categories) states, and MUST NOT occupy more than one category at once. | `PR-SAF-010` · `PR-WFL-007` |
| `SP-STM-002` | An entity's recorded state, together with the position Workflow State already records, MUST be sufficient by itself to determine which transitions [Section 6.6](#66-valid-transition-rules) makes available next, without requiring information other than what [Section 4](#4-inputs) defines. | `PR-WFL-008` · `PR-SAF-010` |

### 6.2 State Ownership

> **On this section, non-normative.** This section generalizes, for any entity this state model
> governs, the custody pattern `SP-WFL-033` and `SP-WFL-034` already realize for a Workflow's steps.
> It does not restate those rules; a domain-specific Specification realizes this section's rules for
> its own entities in the same way AEOS-SPEC-WFL already does for a Workflow's.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-003` | A transition of an entity's state category MUST be decided only by the behavior domain that owns state maintenance for that entity, and MUST NOT be decided by any other behavior domain. | `PR-NFR-006` · `PR-NFR-007` |
| `SP-STM-004` | The durable record of an entity's state MUST exist only within the Repository behavior domain; the behavior domain that decides an entity's transitions MUST NOT hold that record as the entity's state of record itself. | `PR-REP-001` · `PR-REP-002` · `PR-WFL-008` |

### 6.3 State Lifecycle

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-005` | An entity's state MUST begin in the Advancing category at the moment the owning behavior domain begins evaluating that entity's first step, and MUST NOT exist in any category before that moment. | `PR-WFL-004` |
| `SP-STM-006` | Once an entity's state reaches the Completed or the Cancelled category, no further transition of that entity's state MUST occur. | `PR-WFL-011` · `PR-SAF-010` |

### 6.4 State Categories

This document's state model recognizes a closed set of five state categories. An entity's state
MUST always resolve to exactly one of the five; this document introduces no sixth. A domain that
needs an additional category reports the need under [Section 8](#8-extension-points) or
[Section 12.2](#122-change-control) rather than introducing one locally, consistent with the closed
treatment AEOS-GLOSSARY already gives Action Class.

| Category | Meaning |
| :--- | :--- |
| **Advancing** | The entity is currently the subject of active sequencing, evaluation, or execution by its owning behavior domain. |
| **Held** | Progression is suspended pending a specific, named, resolvable condition — an outstanding Human Approval decision, or a voluntary pause between steps — and the entity's recorded position is preserved. |
| **Halted** | Progression has stopped as the direct result of a declined Proposal or a failed step; the entity does not advance further without a new decision. |
| **Completed** | The entity has reached its declared success criteria. |
| **Cancelled** | A human has decided the entity will not continue, communicated through the same decision path as any other decision. |

Completed and Cancelled are terminal: [Section 6.6](#66-valid-transition-rules) defines no
transition out of either, consistent with `SP-STM-006`.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-007` | An entity's state MUST resolve to exactly one of Advancing, Held, Halted, Completed, or Cancelled, and this document's five-category set MUST be treated as closed for AEOS 1.0. | `PR-WFL-007` · `PR-WFL-011` |
| `SP-STM-008` | A durably recorded value that does not resolve to one of the five categories MUST be reported as an inconsistency and MUST NOT be interpreted as any one of them by assumption. | `PR-SAF-002` · `PR-SAF-010` |

### 6.5 State Transitions

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-009` | A transition MUST occur only as a result of one of the Inputs [Section 4](#4-inputs) defines, and MUST NOT occur spontaneously or as a consequence of the passage of time alone. | `PR-SAF-002` |
| `SP-STM-010` | Every transition MUST be recorded together with the input that caused it, before the entity is presented, to any counterparty, as having completed that transition. | `PR-WFL-015` |

### 6.6 Valid Transition Rules

The table below is the complete set of valid transitions between the categories
[Section 6.4](#64-state-categories) states. A row's "From" value of "— (creation)" names an entity's
first appearance in this state model, not a transition from an existing category.

| From | To | Condition |
| :--- | :--- | :--- |
| — (creation) | Advancing | The owning behavior domain begins evaluating the entity's first step, per `SP-STM-005`. |
| Advancing | Held | The entity reaches a step requiring an outstanding Human Approval decision, or a human chooses to pause the entity between steps. |
| Advancing | Halted | A step's Proposal is declined, or a step fails. |
| Advancing | Completed | Every step the entity's declaration requires has reached a recorded outcome, and no step remains unresolved. |
| Held | Advancing | An explicit Human Approval authorizes continuation, an Automation Grant resolves the outstanding gate per [Section 6.8](#68-human-approval-state-behavior), or a paused entity is explicitly resumed. |
| Held | Cancelled | A human, presented with the decision to continue, explicitly declines to continue. |
| Halted | Held | A new decision is proposed for the entity following the halt. |

No transition beyond the seven listed exists.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-011` | A transition MUST occur only between a From category and a To category listed in the table above, under the condition stated for that pair; a transition to a category not reachable from the entity's current category MUST NOT occur. | `PR-SAF-002` · `PR-WFL-005` |
| `SP-STM-012` | Advancing MUST NOT transition to Halted for any reason other than a declined Proposal or a failed step. | `PR-WFL-009` · `PR-WFL-010` |
| `SP-STM-013` | Halted MUST NOT transition directly to Advancing; a return to Advancing following a Halted state MUST occur only through the Held category, per the table above. | `PR-SAF-005` |

### 6.7 Invalid Transition Handling

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-014` | An attempted transition to a category not reachable from an entity's current category under [Section 6.6](#66-valid-transition-rules) MUST be rejected, MUST leave the entity's current state unchanged, and MUST be reported as an invalid transition rather than silently ignored or silently corrected. | `PR-SAF-002` · `PR-WFL-007` |

### 6.8 Human Approval State Behavior

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-015` | An entity MUST enter the Held category no later than the moment an Approval Gate is placed for one of its steps. | `PR-WFL-005` · `PR-WFL-006` |
| `SP-STM-016` | Held state entered for an outstanding Human Approval decision MUST NOT resolve to Advancing on silence or ambiguity; only an explicit Human Approval or an explicit decline resolves it. | `PR-SAF-002` · `PR-WFL-005` |
| `SP-STM-017` | Where an Automation Grant covers the Action Class of a step's intended effect and that class is not Destructive, Held MAY resolve to Advancing without a separately collected Human Approval for that step, consistent with the Automation Grant's scope. | `PR-WFL-014` · `PR-SAF-012` |

### 6.9 Persistence Responsibilities

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-018` | A transition MUST NOT be reported as complete to any counterparty until the durable write establishing the resulting state has completed. | `PR-WFL-008` · `PR-SAF-010` |
| `SP-STM-019` | The durable record of an entity's state MUST include, at minimum, its current category and the condition recorded for it under whichever of [Sections 6.8](#68-human-approval-state-behavior) through [6.14](#614-completion-behavior) applies. | `PR-REP-001` · `PR-WFL-007` |

### 6.10 Resume Responsibilities

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-020` | Resuming an entity from Held to Advancing MUST make available, without re-derivation, every element of that entity's recorded position needed to continue. | `PR-WFL-008` · `PR-WFL-007` |
| `SP-STM-021` | Resuming an entity MUST NOT require Runtime State; the entity's durably recorded state MUST be sufficient by itself to resume. | `PR-REP-015` |

### 6.11 Recovery Responsibilities

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-022` | Before resuming an entity from Held, or reporting its current state, the durably recorded state MUST be checked against the Repository's actual condition; where the two diverge, the divergence MUST be reported and MUST NOT be silently reconciled by assuming the durably recorded state is correct. | `PR-REP-014` |
| `SP-STM-023` | Where an interruption occurs before a transition's durable write completes, the entity's state of record MUST remain its last successfully durably recorded state; the transition MUST NOT be treated as having occurred. | `PR-SAF-010` |

### 6.12 Failure State Behavior

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-024` | An entity transitioning to Halted as the result of a failed step MUST retain, as part of its recorded condition, that the halt resulted from failure, distinct from a halt resulting from decline. | `PR-WFL-010` · `PR-WFL-011` |
| `SP-STM-025` | Where a domain-specific Specification declares a dependency between two entities this state model governs, a dependent entity MUST NOT transition to Advancing while the entity it depends on remains Halted. | `PR-WFL-009` · `PR-WFL-010` |

### 6.13 Cancellation Behavior

> **On this section, non-normative.** No `PR-` requirement establishes a cancellation capability
> distinct from the decline mechanism `PR-WFL-009` already establishes. This section treats
> Cancellation as a specific realization of that mechanism — a decline made while an entity is Held
> rather than while a specific step's Proposal is outstanding — and does not introduce a new one;
> [Section 10](#10-non-goals) states this boundary explicitly.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-026` | Held MUST transition to Cancelled when a human, presented with the decision to continue the entity, explicitly declines to continue it; that decline MUST be explicit and MUST NOT be inferred. | `PR-WFL-009` · `PR-SAF-002` |
| `SP-STM-027` | A transition to Cancelled MUST leave no effect behind beyond what was already durably recorded as complete at the moment of cancellation, and MUST NOT itself reverse a prior transition. | `PR-WFL-009` · `PR-SAF-004` |

### 6.14 Completion Behavior

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-028` | Advancing MUST transition to Completed only when every step the owning behavior domain's declaration requires has itself reached a recorded outcome. | `PR-WFL-011` |
| `SP-STM-029` | A Completed state's recorded outcome MUST distinguish full completion of every step from completion that includes one or more partially completed steps. | `PR-WFL-011` |

### 6.15 State Visibility

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-030` | An entity's current state category, together with the condition recorded for it, MUST be available on request without requiring the entity to change state in order to produce that report. | `PR-WFL-007` · `PR-NFR-001` |
| `SP-STM-031` | Where an entity is Held, its outstanding condition — the specific decision or pause awaited — MUST be stated as part of that visibility, not merely that the entity is Held. | `PR-WFL-007` |

---

## 7. Constraints

The invariants below MUST hold before, during, and after every behavior stated in
[Section 6](#6-behavior).

### 7.1 State Consistency Requirements

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-032` | At every point in time, an entity's durably recorded state MUST correspond to exactly one category in [Section 6.4](#64-state-categories)'s closed set and to a condition consistent with that category's meaning. | `PR-SAF-010` |
| `SP-STM-033` | An entity's state MUST NOT be observably different depending on which counterparty inspects it, provided both inspect the same durable revision. | `PR-SAF-010` · `PR-REP-001` |

### 7.2 Domain Neutrality Constraints

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-034` | The state model in [Section 6](#6-behavior) MUST apply without variation according to which behavior domain owns an entity's state maintenance. | `PR-NFR-006` |
| `SP-STM-035` | A domain-specific Specification MUST NOT restate the category set or the transition rules with different names or different conditions. | `PR-NFR-006` · `PR-NFR-007` |

### 7.3 Declared Dependencies

Per AEOS-SPECSTD `DP1`, this document declares its dependency on the following rules explicitly, by
identifier, rather than restating them:

| Dependency | States | Why this domain depends on it |
| :--- | :--- | :--- |
| `SP-SYS-050` | An interruption at any point in the system interaction loop leaves the project in a describable state. | [Section 6.7](#67-invalid-transition-handling), [Section 6.11](#611-recovery-responsibilities), and [Section 7.1](#71-state-consistency-requirements) rely on this rule for the describability an interrupted or divergent entity must retain. |
| `SP-WFL-018` | Where a step falls within an active TDD Cycle, its position within the cycle is held as part of Workflow State. | [Section 6.4](#64-state-categories) treats cycle position as part of the condition recorded for the Advancing and Held categories, without restating how that position is kept. |
| `SP-WFL-023` through `SP-WFL-025` | Outcome Recording determines and durably records what a completed, partially completed, or failed step contributes to Workflow State. | [Section 6.14](#614-completion-behavior) and [Section 6.9](#69-persistence-responsibilities) generalize the recording and durability pattern these rules already realize for a Workflow's steps. |
| `SP-WFL-026` through `SP-WFL-028` | Halt Handling stops progression on decline or failure and leaves a describable state. | [Section 6.6](#66-valid-transition-rules) and [Section 6.12](#612-failure-state-behavior) generalize the Halted category from this realization. |
| `SP-WFL-029` through `SP-WFL-032` | Pause and Resume preserve a Workflow's recorded position across a chosen interruption, and that position is visible on request. | [Section 6.10](#610-resume-responsibilities) and [Section 6.15](#615-state-visibility) generalize this realization to the Held category and to state visibility generally. |
| `SP-WFL-033` and `SP-WFL-034` | The Workflow Engine holds Workflow State through the Repository behavior domain and holds no durable state of its own beyond an unresolved step. | [Section 6.2](#62-state-ownership) generalizes this custody pattern to any entity this state model governs. |
| `SP-WFL-039` | The Workflow Engine is the only point at which an Approval Gate is placed. | [Section 6.8](#68-human-approval-state-behavior) relies on this rule for there being exactly one point at which an entity enters Held for an outstanding decision. |

Consistent with AEOS-SPECSTD `DP4`, each dependency above is stated behaviorally: what must already
hold, not which document, technology, or component makes it hold. Consistent with `DP2`, no rule in
[Section 6](#6-behavior) or elsewhere in this document presumes an unstated precondition from `SP-SYS`
or `SP-WFL`; every precondition this domain relies on is named in this table.

### 7.4 Non-Functional Constraints

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-036` | Every rule in [Section 6](#6-behavior) MUST be verifiable in isolation, with the owning behavior domain and the Repository behavior domain replaceable at the point this domain reaches them, for the purpose of that verification. | `PR-NFR-010` |
| `SP-STM-037` | Behavior specified in [Section 6](#6-behavior) MUST be identical on every supported Platform. | `PR-PLT-003` · `PR-PLT-006` |
| `SP-STM-038` | A state category, a recorded condition, or a transition MUST be explainable to the user on request: what was inspected, what triggered it, and why. | `PR-NFR-001` |

---

## 8. Extension Points

`SS-P-09` requires that extension of a Specification be additive; frozen behavior is never silently
altered. This document's ordinary extension mechanism is the addition of new `SP-STM-<NNN>`
identifiers under AEOS-SPECSTD Section 18.1. Beyond that ordinary mechanism, this document declares
two extension points at which a domain-specific Specification binds its own concrete state values to
the model [Section 6](#6-behavior) and [Section 7](#7-constraints) already fix, without altering either.
Unlike the Context and Workflow Extension Points, these are not drawn from one AEOS-BLUEPRINT layer's
own extension table: this document is not itself bound to one Blueprint layer, consistent with
[Section 2.2](#22-relationship-to-the-system-specification-and-to-the-workflow-engine-specification),
and declares these points on its own authority, in the same sense AEOS-SPEC declares `EPS-1` through
`EPS-7` on its own authority.

### 8.1 State Machine Extension Points

| ID | Extension point | What is added | Boundary — MUST NOT change |
| :--- | :--- | :--- | :--- |
| `EPM-1` | Category binding | A domain-specific Specification's declaration of which of its own concrete state values or conditions correspond to each of the five categories in [Section 6.4](#64-state-categories), for the entities that domain owns. | The five-category closed set `SP-STM-007` fixes, and the transition table `SP-STM-011` fixes. |
| `EPM-2` | Additional entity kind | A domain-specific Specification's application of this state model to an entity kind other than a Workflow's steps. | The category set, the transition table, and any binding another Specification has already recorded at `EPM-1` for a different entity kind. |

### 8.2 Rules Governing These Extension Points

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-STM-EXT-1` | An extension admitted at `EPM-1` or `EPM-2` MUST be admitted as an additive declaration and MUST NOT require a change to [Section 6](#6-behavior) or [Section 7](#7-constraints) of this document. | `PR-NFR-007` |
| `SP-STM-EXT-2` | An admission at `EPM-1` MUST NOT introduce a category beyond the five [Section 6.4](#64-state-categories) states. | `PR-NFR-006` |
| `SP-STM-EXT-3` | An admission at `EPM-1` or `EPM-2` MUST NOT alter a transition condition [Section 6.6](#66-valid-transition-rules) states. | `PR-SAF-002` |
| `SP-STM-EXT-4` | An admission at `EPM-2` MUST NOT bind an entity kind another Specification document has already bound, without retiring that prior binding first. | `PR-NFR-006` |

---

## 9. Traceability

Every rule in [Section 6](#6-behavior), [Section 7](#7-constraints), and
[Section 8.2](#82-rules-governing-these-extension-points) traces to one or more `PR-` identifiers,
per AEOS-SPECSTD `TR1`. The complete rule-by-rule trace is
[Appendix A](#appendix-a--sp-stm-rule-index).

### 9.1 Trace Density by `PR-` Prefix

| `PR-` prefix | Rules in this document that trace to it |
| :--- | :--- |
| `PR-WFL` | `SP-STM-001` · `SP-STM-002` · `SP-STM-004` through `SP-STM-007` · `SP-STM-010` through `SP-STM-012` · `SP-STM-014` through `SP-STM-020` · `SP-STM-024` through `SP-STM-031` |
| `PR-SAF` | `SP-STM-001` · `SP-STM-002` · `SP-STM-006` · `SP-STM-008` · `SP-STM-009` · `SP-STM-011` · `SP-STM-013` · `SP-STM-014` · `SP-STM-016` · `SP-STM-017` · `SP-STM-018` · `SP-STM-023` · `SP-STM-026` · `SP-STM-027` · `SP-STM-032` · `SP-STM-033` · `SP-STM-EXT-3` |
| `PR-REP` | `SP-STM-004` · `SP-STM-019` · `SP-STM-021` · `SP-STM-022` · `SP-STM-033` |
| `PR-NFR` | `SP-STM-003` · `SP-STM-030` · `SP-STM-034` · `SP-STM-035` · `SP-STM-036` · `SP-STM-038` · `SP-STM-EXT-1` · `SP-STM-EXT-2` · `SP-STM-EXT-4` |
| `PR-PLT` | `SP-STM-037` |

No rule in this document lacks a trace, and no rule traces to a requirement this document does not
cite by identifier, consistent with AEOS-SPECSTD `TR1` and `TR4`.

### 9.2 Grounding in Architecture and Blueprint Identifiers

Per AEOS-BLUEPRINT `BP-GOV-008`, this document traces to at least one Blueprint identifier and to at
least one `PR-` identifier. This document's ownership rules in
[Section 6.2](#62-state-ownership) are grounded in `AR-LAY-006` and `AR-LAY-009`, and in `BP-WFL-009`
and `BP-REP-007`, cited for orientation and not restated. Its persistence rules in
[Section 6.9](#69-persistence-responsibilities) are grounded in `BP-REP-007` and `BP-REP-008`. Its
recovery rules in [Section 6.11](#611-recovery-responsibilities) are grounded in `AR-BND-008`. Its
non-functional constraints in [Section 7.4](#74-non-functional-constraints) are grounded in
`AR-LAY-008` and `AR-LAY-007`.

### 9.3 Downstream Traceability

Downstream documents — Implementation Guides, tests, issues, and pull requests — reference the
`SP-STM-<NNN>` identifiers they realize or affect, consistent with AEOS-SPECSTD `TR5`. A
domain-specific Specification that records a binding under [Section 8.1](#81-state-machine-extension-points)
references the `SP-STM-<NNN>` identifiers that binding realizes, in addition to its own identifiers.

---

## 10. Non-goals

This document deliberately does not cover the following. Each is a reasonable expectation a reader
might expect this domain to cover, which it deliberately does not, stated here so a reader does not
search for it.

| Non-goal | Why it is out of scope | Where it belongs |
| :--- | :--- | :--- |
| The concrete state values, conditions, or vocabulary specific to any one behavior domain's entities — for example, a Workflow's own step-level outcomes | Owned by that domain's own Specification, bound to this model under [Section 8](#8-extension-points) | AEOS-SPEC-WFL for a Workflow's steps; a future domain-specific Specification for any other entity kind |
| The sequencing of a Workflow's steps, the evaluation of preconditions, or the classification of intended effects | Owned by the Workflow behavior domain | AEOS-SPEC-WFL |
| The collection of a human decision, or the assembly of a Proposal, an Explanation, a Status, or a Report | Owned by the Human Interaction behavior domain, reserved as `EPS-7` in AEOS-SPEC | Future Human Interaction behavior Specification |
| The performance of an approved effect, or the observation of the Environment before or after it | Owned by the Execution behavior domain, reserved as `EPS-6` in AEOS-SPEC | Future Execution behavior Specification |
| The selection or composition of Context for a step | Owned by the Context behavior domain, reserved as `EPS-3` in AEOS-SPEC; the Context Layer holds no durable state and this document's state model does not apply to it | The Context Router Specification (AEOS-SPEC-CTX) |
| A sixth state category, or a transition condition beyond those [Section 6.6](#66-valid-transition-rules) states | Not established by any `PR-` requirement; introducing one is a Major revision of this document under [Section 12.2](#122-change-control), not a local extension | This document's own change control |
| A cancellation capability distinct from the decline mechanism `PR-WFL-009` already establishes | Not established by any `PR-` requirement; [Section 6.13](#613-cancellation-behavior) treats Cancellation as a realization of that mechanism, not a new one | A future AEOS-PRD capability, should one be defined |
| Automatic reversal of a Completed or Cancelled entity's recorded effects | Not established by any `PR-` requirement; consistent with `SP-WFL-043`, cited and not restated | A future AEOS-PRD capability, should one be defined |
| Automatic retry of a failed step or a declined Proposal | Not established by any `PR-` requirement; consistent with `SP-WFL-044` and `PR-RUN-011`, cited and not restated | A future AEOS-PRD capability, should one be defined |
| A durable storage mechanism, schema, data structure, or technology realizing persistence | Prohibited to the Specification layer by AEOS-SPECSTD `MN1`–`MN3` | Implementation Guides, AEOS-TECH |
| Interfaces, endpoints, request or response schemas, wire formats | Prohibited to the Specification layer by AEOS-SPECSTD `MN4` | Implementation Guides |
| Installation, deployment, or environment-preparation procedure | Prohibited to the Specification layer by AEOS-SPECSTD `MN5` | Runtime documents, Developer Guides |
| Rationale for why any rule above exists | Prohibited to the Specification layer by AEOS-SPECSTD `MN6` | AEOS-VISION, AEOS-PRD |
| Structural decisions, layer boundaries, or dependency direction | Owned by AEOS-ARCH and AEOS-BLUEPRINT | AEOS-ARCH, AEOS-BLUEPRINT |
| Test procedures, test plans, or test cases | Outside AEOS-SPECSTD's scope, per its Section 2.2 note on testing artifacts | A future Test Specification layer |

---

## 11. References

| Reference | Cited for |
| :--- | :--- |
| AEOS-VISION | Invariants `V2`, `V3`, `V5`, `V7`, and `V9`, underlying the supervision, inspectability, and repository-as-product posture this document specifies behaviorally |
| AEOS-PRD Section 9 | The engineering lifecycle stages the entities this document governs serve |
| AEOS-PRD Section 10 | The interaction loop, Action Classes, and Automation Grants realized behaviorally in [Section 6.8](#68-human-approval-state-behavior) |
| AEOS-PRD Section 18.3 | Every `PR-WFL` identifier this document traces to |
| AEOS-PRD Section 18.4 | `PR-RUN-011`, cited in [Section 10](#10-non-goals) |
| AEOS-PRD Section 18.5 | `PR-TDD` identifiers underlying cycle-position grounding in [Section 7.3](#73-declared-dependencies) |
| AEOS-PRD Section 18.10 | Every `PR-REP` identifier this document traces to |
| AEOS-PRD Section 18.11 | Every `PR-PLT` identifier this document traces to |
| AEOS-PRD Section 18.13 | Every `PR-SAF` identifier this document traces to |
| AEOS-PRD Section 19 | Every `PR-NFR` identifier this document traces to |
| AEOS-GLOSSARY | The definitions of every capitalized term used in this document, including *Workflow State*, *Runtime State*, *Approval Gate*, *Human Approval*, *Action Class*, *Automation Grant*, *Proposal*, *Repository*, and *Repository Asset* |
| AEOS-DOCSTD | The form, structure, and lifecycle this document, like every AEOS document, follows |
| AEOS-ARCH Section 4.4 | The Workflow Layer's state-maintenance responsibility, cited in [Section 6.2](#62-state-ownership) |
| AEOS-ARCH Section 4.9 | The Repository Layer's state-custody responsibility, cited in [Section 6.2](#62-state-ownership) |
| AEOS-ARCH Section 8 | `AR-LAY-006`, `AR-LAY-009`, and `AR-BND-008`, cited in [Section 9.2](#92-grounding-in-architecture-and-blueprint-identifiers) |
| AEOS-BLUEPRINT Section 7 | `BP-REP-007` and `BP-REP-008`, cited in [Section 9.2](#92-grounding-in-architecture-and-blueprint-identifiers) |
| AEOS-BLUEPRINT Section 8 | `BP-WFL-007`, `BP-WFL-009`, and `BP-WFL-012`, cited in [Section 9.2](#92-grounding-in-architecture-and-blueprint-identifiers) |
| AEOS-SPECSTD | The Specification Standard this document is written entirely under |
| AEOS-SPEC Section 7.2 | `SP-SYS-050`, declared as a dependency in [Section 7.3](#73-declared-dependencies) |
| AEOS-SPEC Section 8.1 | The seven `EPS-` extension points this document does not attach at, per [Section 2.2](#22-relationship-to-the-system-specification-and-to-the-workflow-engine-specification) |
| AEOS-SPEC-WFL Section 6 | `SP-WFL-018` and `SP-WFL-023` through `SP-WFL-034`, declared as dependencies in [Section 7.3](#73-declared-dependencies) |
| AEOS-SPEC-WFL Section 7.3 | `SP-WFL-039`, declared as a dependency in [Section 7.3](#73-declared-dependencies) |

---

## 12. Document Governance

### 12.1 Status

This document is a Specification-layer document of the AEOS repository. It does not attach at an
AEOS-SPEC extension point, and it is intended to be frozen as part of the AEOS 1.0 release alongside
AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-SPEC,
AEOS-SPEC-WFL, and the Context Router Specification (AEOS-SPEC-CTX).

### 12.2 Change Control

This document's change control follows AEOS-SPECSTD Section 18.1 without modification, applied to
the `STM` behavior domain.

| Increment | When |
| :--- | :--- |
| **Patch** | Editorial correction with no change to a specified rule's meaning or trace. |
| **Minor** | Addition of a new `SP-STM-<NNN>` identifier that does not alter what an existing identifier requires. |
| **Major** | Any change to what an existing `SP-STM-<NNN>` identifier requires; retirement of an identifier; a change to the `STM` area code, ownership, or declared behavior domain; addition or removal of an `EPM-` extension point; addition of a sixth state category or a transition beyond those `SP-STM-011` fixes; or any change that would invalidate a downstream Implementation Guide, Runtime document, or test written against the prior version. |

### 12.3 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and apply the AEOS-SPECSTD Section 19.2 freeze
checklist before recommending freeze, per AEOS-DOCSTD Section 12.3 and 12.4.

### 12.4 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-SPEC on a rule reserved as system-level, cross-cutting behavior | AEOS-SPEC governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-SPEC-WFL on a matter neither document's declared boundary resolves | Escalated to the owner, per AEOS-SPECSTD Section 15.2 `DP6`. Neither document resolves it by local reinterpretation. |
| This document conflicts with AEOS-SPECSTD on form, identifier convention, traceability, or lifecycle | AEOS-SPECSTD governs. This document is corrected. |
| This document conflicts with AEOS-SPECSTD on the content of the `STM` behavior domain | This document governs its own content, per AEOS-SPECSTD Section 20.7. |
| A future Specification records a binding at `EPM-1` or `EPM-2` that states a dependency on this document that this document does not confirm | The apparent need is reported against this document. It is not resolved by a contradictory rule in the attaching document. |
| A future Specification attaching at an AEOS-SPEC `EPS-` point states a rule about state that would contradict this document | This document governs the general state model. The conflict is reported against the attaching document, consistent with `SP-SYS-EXT-3`'s treatment of contradiction between a domain-specific Specification and the foundation it depends on. |

### 12.5 Traceability

Traceability for this document is stated in full in [Section 9](#9-traceability) and
[Appendix A](#appendix-a--sp-stm-rule-index). Downstream documents reference this document's
`SP-STM-<NNN>` identifiers under AEOS-SPECSTD `TR5`.

### 12.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Freeze candidate | Initial State Machine Specification. Registers the `STM` behavior domain under AEOS-SPECSTD Section 11.4, without attaching at an AEOS-SPEC `EPS-` extension point, consistent with `SP-SYS-EXT-4`. Establishes thirty-eight `SP-STM` rules organized under fifteen subsections of Behavior — State Model, State Ownership, State Lifecycle, State Categories, State Transitions, Valid Transition Rules, Invalid Transition Handling, Human Approval State Behavior, Persistence Responsibilities, Resume Responsibilities, Recovery Responsibilities, Failure State Behavior, Cancellation Behavior, Completion Behavior, and State Visibility — together with State Consistency, Domain Neutrality, and Non-Functional Constraints and seven declared dependencies on `SP-SYS` and `SP-WFL` rules. Fixes a closed set of five state categories — Advancing, Held, Halted, Completed, Cancelled — and a seven-row valid transition table. States two extension points, `EPM-1` and `EPM-2`, declared on this document's own authority rather than derived from one AEOS-BLUEPRINT layer, with four governing rules. Traces every rule to one or more `PR-` identifiers and grounds the document as a whole in `AR-LAY-006`, `AR-LAY-009`, `AR-BND-008`, `BP-WFL-007`, `BP-WFL-009`, `BP-WFL-012`, `BP-REP-007`, and `BP-REP-008`. Explicitly excludes, as Non-goals, a sixth state category, a cancellation capability distinct from the existing decline mechanism, automatic reversal, and automatic retry, none of which any `PR-` requirement establishes. Introduces no product requirement, no vision, no terminology, no architectural decision, and no Blueprint arrangement. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPEC, and restates no rule already stated by AEOS-SPEC or AEOS-SPEC-WFL. |

---

## Appendix A — SP-STM Rule Index

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Section 6](#6-behavior), [Section 7](#7-constraints), or
[Section 8.2](#82-rules-governing-these-extension-points).

| ID | Section | Short label | Traces to |
| :--- | :--- | :--- | :--- |
| `SP-STM-001` | 6.1 | Exactly one state category at a time | `PR-SAF-010` · `PR-WFL-007` |
| `SP-STM-002` | 6.1 | Recorded state is self-sufficient for next transitions | `PR-WFL-008` · `PR-SAF-010` |
| `SP-STM-003` | 6.2 | Transitions decided only by the owning domain | `PR-NFR-006` · `PR-NFR-007` |
| `SP-STM-004` | 6.2 | Durable record held only by the Repository domain | `PR-REP-001` · `PR-REP-002` · `PR-WFL-008` |
| `SP-STM-005` | 6.3 | State begins in Advancing at first-step evaluation | `PR-WFL-004` |
| `SP-STM-006` | 6.3 | No further transition after Completed or Cancelled | `PR-WFL-011` · `PR-SAF-010` |
| `SP-STM-007` | 6.4 | Closed set of five state categories | `PR-WFL-007` · `PR-WFL-011` |
| `SP-STM-008` | 6.4 | Out-of-set value reported, not interpreted | `PR-SAF-002` · `PR-SAF-010` |
| `SP-STM-009` | 6.5 | Transition only from a defined Input | `PR-SAF-002` |
| `SP-STM-010` | 6.5 | Transition recorded with its cause before completion | `PR-WFL-015` |
| `SP-STM-011` | 6.6 | Transition only per the valid-transition table | `PR-SAF-002` · `PR-WFL-005` |
| `SP-STM-012` | 6.6 | Advancing→Halted only on decline or failure | `PR-WFL-009` · `PR-WFL-010` |
| `SP-STM-013` | 6.6 | Halted→Advancing only through Held | `PR-SAF-005` |
| `SP-STM-014` | 6.7 | Invalid transition rejected, unchanged, reported | `PR-SAF-002` · `PR-WFL-007` |
| `SP-STM-015` | 6.8 | Enter Held no later than gate placement | `PR-WFL-005` · `PR-WFL-006` |
| `SP-STM-016` | 6.8 | No resolution to Advancing on silence or ambiguity | `PR-SAF-002` · `PR-WFL-005` |
| `SP-STM-017` | 6.8 | Automation Grant MAY resolve a non-Destructive gate | `PR-WFL-014` · `PR-SAF-012` |
| `SP-STM-018` | 6.9 | Not reported complete until durable write completes | `PR-WFL-008` · `PR-SAF-010` |
| `SP-STM-019` | 6.9 | Durable record includes category and condition | `PR-REP-001` · `PR-WFL-007` |
| `SP-STM-020` | 6.10 | Resume makes recorded position available without re-derivation | `PR-WFL-008` · `PR-WFL-007` |
| `SP-STM-021` | 6.10 | Resume MUST NOT require Runtime State | `PR-REP-015` |
| `SP-STM-022` | 6.11 | Recorded state checked against actual condition before resume or report | `PR-REP-014` |
| `SP-STM-023` | 6.11 | Interrupted write leaves last successfully recorded state | `PR-SAF-010` |
| `SP-STM-024` | 6.12 | Failure-caused Halted distinct from decline-caused | `PR-WFL-010` · `PR-WFL-011` |
| `SP-STM-025` | 6.12 | Dependent entity withheld while depended-upon is Halted | `PR-WFL-009` · `PR-WFL-010` |
| `SP-STM-026` | 6.13 | Held→Cancelled on explicit decline to continue | `PR-WFL-009` · `PR-SAF-002` |
| `SP-STM-027` | 6.13 | Cancellation leaves no effect beyond what was recorded complete | `PR-WFL-009` · `PR-SAF-004` |
| `SP-STM-028` | 6.14 | Completed only when every required step has an outcome | `PR-WFL-011` |
| `SP-STM-029` | 6.14 | Completed outcome distinguishes full from partial completion | `PR-WFL-011` |
| `SP-STM-030` | 6.15 | Current category and condition available on request | `PR-WFL-007` · `PR-NFR-001` |
| `SP-STM-031` | 6.15 | Held's outstanding condition stated, not merely "Held" | `PR-WFL-007` |
| `SP-STM-032` | 7.1 | Recorded state always resolves to one category and a consistent condition | `PR-SAF-010` |
| `SP-STM-033` | 7.1 | Same durable revision observed identically by every counterparty | `PR-SAF-010` · `PR-REP-001` |
| `SP-STM-034` | 7.2 | State model applies without variation by owning domain | `PR-NFR-006` |
| `SP-STM-035` | 7.2 | No restatement of categories or transitions under different names | `PR-NFR-006` · `PR-NFR-007` |
| `SP-STM-036` | 7.4 | Verifiable in isolation | `PR-NFR-010` |
| `SP-STM-037` | 7.4 | Identical on every supported Platform | `PR-PLT-003` · `PR-PLT-006` |
| `SP-STM-038` | 7.4 | Explainable to the user on request | `PR-NFR-001` |
| `SP-STM-EXT-1` | 8.2 | Extensions are additive, no change to Section 6 or 7 | `PR-NFR-007` |
| `SP-STM-EXT-2` | 8.2 | `EPM-1` admission introduces no sixth category | `PR-NFR-006` |
| `SP-STM-EXT-3` | 8.2 | `EPM-1`/`EPM-2` admission alters no transition condition | `PR-SAF-002` |
| `SP-STM-EXT-4` | 8.2 | `EPM-2` admission binds no already-bound entity kind | `PR-NFR-006` |

---

**End of State Machine Specification**

AEOS-SPEC-STM · Version 1.0.0 · Traces to `PR-WFL` · `PR-REP` · `PR-SAF` · `PR-NFR` · `PR-PLT`
