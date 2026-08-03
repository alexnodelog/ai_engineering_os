# AI Engineering Operating System

## AEOS — Runtime Adapter Specification

*The permanent statement of the observable contract between AEOS and every Runtime Adapter.*

| Field | Value |
| :--- | :--- |
| **Document** | Runtime Adapter Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SPEC-ADP |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Specification Governance Board, AEOS |
| **Audience** | Architects, implementers, reviewers, test authors, and AI runtimes |
| **Suggested path** | `docs/specification/RUNTIME_ADAPTER_SPEC.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) |
| **Supersedes** | None |
| **Area code** | `ADP` |

> **Authority of this document.**
> This document specifies, precisely and testably, the Runtime Adapter behavior of AEOS: the
> observable contract between the Runtime Blueprint and every adapter that mediates one external
> Runtime, as AEOS-BLUEPRINT Section 11 assigns that position.
> It defines no rationale, no structure, no technology, no interface surface, and no implementation.
> It is not a Product document, not an Architecture document, not a Blueprint, not an adapter SDK,
> and not an API reference. Where this document and a document of higher authority both speak to a
> subject, the higher-authority document governs and any conflict here is a defect to be reported
> rather than acted upon.

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

---

## 1. Purpose

AEOS orchestrates external AI Runtimes without performing inference of its own, and remains
independent of any single vendor, model, or runtime implementation (`PR-RUN-001`, `PR-RUN-002`,
`PR-RUN-003`). That independence is only as real as the contract at the one position AEOS is
permitted to hold Runtime-specific knowledge: the adapter. AEOS-BLUEPRINT Section 11 assigns that
position and names the subsystems it comprises; it deliberately states where the responsibility sits,
not what the responsibility must observably do, per AEOS-BLUEPRINT Section 17.1. Without a precise,
testable statement of that behavior, two adapters mediating two different Runtimes could each satisfy
the Blueprint's arrangement while producing outcomes a Workflow could not treat as comparable —
honoring the architecture on paper while defeating `PR-RUN-008` in practice.

This Specification closes that gap for the Runtime Adapter behavior domain. It states, precisely
enough to test, what an adapter's identity, lifecycle, registration, and discovery must be; what its
capability declaration and negotiation must state; what runtime, model, and version compatibility it
must report; what its responsibilities are for delivering Context and Prompt material, performing
invocation, normalizing responses and errors, handling cancellation and timeout, reporting health, and
observing its security boundary. It states nothing about how any of this is implemented, and nothing
about why runtime independence matters — AEOS-VISION and AEOS-PRD already state that.

A reader of this document gains a fixed target: an implementer building a new adapter can determine
exactly what observable behavior is required without inventing it, and a reviewer or a test author can
determine exactly what to check without consulting the author.

---

## 2. Scope

### 2.1 Behavior Domain and Area Registration

This Specification governs the **Runtime Adapter behavior domain**: the observable contract between
the Runtime Blueprint and every adapter admitted to mediate one external Runtime, as that position is
assigned by AEOS-BLUEPRINT Section 11 (the Adapter Blueprint, `BP-ADP`) and depended upon by
AEOS-BLUEPRINT Section 10 (the Runtime Blueprint, `BP-RUN`).

This Specification registers the area code `ADP`, per AEOS-SPECSTD Section 11.4. The code is chosen
to match the area AEOS-BLUEPRINT already registers for the same behavior domain at the Blueprint layer
(`BP-ADP`, AEOS-BLUEPRINT Section 6.2), so that one area name identifies the domain across both
layers without requiring a separate index. This is the first Specification document to use the `ADP`
code at the Specification layer; the registration is made here, by this document, consistent with
AEOS-GLOSSARY `I3`.

### 2.2 Boundary

The domain begins at the point where the Runtime Layer dispatches a runtime-independent request to
exactly one adapter, and ends at the boundary to the External AI Layer, which this Specification does
not cross: what occurs beyond that boundary is performed by a Runtime and is outside AEOS entirely
(`AR-BND-002`). Within that span, this Specification covers everything AEOS-BLUEPRINT Section 11.2
assigns to the Adapter Blueprint's subsystems — Adapter Declaration, Capability Advertisement, Request
Mediation, Result Mediation, Fault Mediation, and Adapter Admission — stated as testable behavior
rather than as arrangement.

### 2.3 Applicability

This Specification applies to every adapter admitted to an AEOS project, regardless of the category of
Runtime it mediates — a commercial service, an open-source model, an interoperability standard, or a
user-supplied extension (`PR-RUN-016`). It applies identically regardless of Platform or Distribution
Method (`PR-PLT-003`, `PR-DST-005`, `PR-DST-006`).

### 2.4 What This Specification Does Not Cover

| Not covered here | Owned by |
| :--- | :--- |
| Which Runtime a project selects, and the custody of that selection | The Runtime Blueprint's own behavior domain (not yet authored as a Specification; see [Section 11](#11-references)) |
| Capability matching, degradation handling, and boundary disclosure presentation | The Runtime Blueprint's own behavior domain |
| Approval Gate placement and Human Approval interaction | The Human Interaction Blueprint's behavior domain |
| Workflow step sequencing and Workflow State maintenance | The Workflow Blueprint's behavior domain |
| Context selection, justification, and composition | The Context Blueprint's behavior domain |
| Transport, wire format, process model, concurrency, storage mechanism, and installation | Runtime documents and Implementation Guides, per AEOS-BLUEPRINT Section 18 |

A statement in this document that decides any of the above is a defect in this document and MUST be
reported rather than acted upon.

---

## 3. Responsibilities

### 3.1 What This Specification Is Answerable For

Applying `SS-P-02` to this document specifically: this Specification is answerable for precise,
testable rules covering adapter identity, lifecycle, registration, discovery, capability declaration,
capability negotiation, runtime compatibility, model compatibility, context delivery, prompt delivery,
invocation, response normalization, error normalization, cancellation, timeout, health reporting,
version compatibility, and security responsibilities — the full set of behavior AEOS-BLUEPRINT Section
11 assigns to the position it calls the adapter.

### 3.2 What This Specification Is Not Answerable For

| Question | Answered by |
| :--- | :--- |
| Which Runtime is selected for a project | AEOS-PRD, custodied by the Runtime Blueprint |
| Whether a step's capability requirement is matched before this domain is reached | The Runtime Blueprint's own behavior domain |
| Whether and how a human is asked to approve a crossing | The Human Interaction Blueprint's behavior domain |
| How this domain's behavior is realized in code | Implementation Guides |
| Why runtime independence, vendor neutrality, and credential non-durability are required | AEOS-VISION and AEOS-PRD |

### 3.3 The Responsibility Test

Every rule in [Section 6](#6-behavior) and [Section 7](#7-constraints) was tested against
AEOS-SPECSTD Section 7.2 before inclusion: is it a testable fact about required behavior, traceable to
a `PR-` identifier, and free of mechanism? A statement that failed this test was rewritten as
observable behavior or removed rather than included for completeness.

---

## 4. Inputs

Every input the Runtime Adapter behavior domain accepts, per `MD2`. An input not listed here is out of
scope for this domain's Behavior rules.

| Input | Required properties | Validity condition |
| :--- | :--- | :--- |
| **Mediation Request** | An Engineering Capability reference; composed Context; composed Prompt; a reference to the approved crossing it corresponds to. | MUST correspond to a crossing already disclosed and approved (`PR-SAF-008`, `PR-RUN-009`). MUST NOT be accepted where Capability Negotiation for the step has not completed ([Section 6.6](#66-capability-negotiation)). |
| **Capability Query** | A reference to the adapter whose current declared offer is requested. | MUST be answerable without dispatching an invocation to the mediated Runtime. |
| **Adapter Declaration** | A declared identity; a declared capability offer; declared runtime, model, and version compatibility conditions. | MUST satisfy [Sections 6.1](#61-adapter-identity), [6.5](#65-capability-declaration), [6.7](#67-runtime-compatibility), [6.8](#68-model-compatibility), and [6.17](#617-version-compatibility) before Adapter Admission completes. |
| **Cancellation Signal** | A reference to the invocation it applies to. | MUST identify an invocation the receiving adapter is currently mediating. |
| **Health Query** | A reference to the adapter whose reachability is requested. | MUST be answerable without dispatching an invocation. |
| **Mediated Runtime Fault** | The raw failure condition observed at the boundary to the mediated Runtime. | Present only where the mediated Runtime has failed to return a valid result within its declared or observed behavior. |
| **Retirement Request** | A reference to the adapter to be retired. | MUST NOT be interpreted as a request to remove the adapter's declaration from the repository. |

---

## 5. Outputs

Every output or effect the Runtime Adapter behavior domain produces, per `MD3`, including the
observable state after each completes.

| Output | Description | Observable state after |
| :--- | :--- | :--- |
| **Capability Advertisement** | The adapter's declared offer, expressed in the neutral Engineering Capability vocabulary. | The offer is available for negotiation by the Runtime Layer. |
| **Negotiation Result** | A confirmation that a declared requirement is satisfied, or a stated gap, per [Section 6.6](#66-capability-negotiation). | The step's requirement is marked satisfied, or the gap is recorded, before dispatch. |
| **Mediation Result** | The normalized, runtime-independent form of what the mediated Runtime returned for an invocation. | The result is available to the Runtime Layer as material. It authorizes no action by itself (`AR-BND-004`). |
| **Normalized Fault** | A neutral condition representing a failure of the mediated Runtime, per [Section 6.13](#613-error-normalization-responsibilities). | The fault is available to the Workflow Blueprint as a condition to act on. No retry has been initiated by the adapter. |
| **Adapter Declaration Record** | The durable Repository Asset holding an adapter's declaration and its current lifecycle state. | The record is inspectable independent of the adapter's current lifecycle state. |
| **Health Report** | The adapter's current, observed reachability status, per [Section 6.16](#616-health-reporting). | The adapter's reachability is recorded as of the time observed, distinguished from an unobserved condition. |
| **Cancellation Outcome** | The reported result of a cancellation signal, per [Section 6.14](#614-cancellation-behavior). | The invocation is no longer in progress. No new invocation has been initiated on its behalf. |
| **Version Compatibility Statement** | The adapter's declared compatibility with this Specification's version, per [Section 6.17](#617-version-compatibility). | The adapter's compatibility is recorded before it is offered for selection. |

---

## 6. Behavior

The normative rules of this Specification. Identifiers are allocated sequentially within the `ADP`
area across this section, [Section 7](#7-constraints), and [Section 8](#8-extension-points), per
`ID1`.

### 6.1 Adapter Identity

An adapter's identity is the set of stable, self-declared properties by which it is registered,
discovered, and selected. Identity is distinct from the Runtime it mediates: the Runtime's internal
state may change without the adapter's identity changing.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-001` | An adapter MUST declare a stable identifier that remains constant for the life of its admission, independent of any change in the internal state of the Runtime it mediates. | `PR-RUN-003` · `PR-RUN-005` |
| `SP-ADP-002` | An adapter's declared identity MUST NOT be relied upon by any other layer as conveying knowledge of the mediated Runtime, Vendor, or Model. | `PR-RUN-002` · `PR-RUN-006` · `BP-ADP-001` |
| `SP-ADP-003` | An adapter MUST declare exactly one mediated Runtime as part of its identity, and MUST NOT declare an identity spanning more than one Runtime. | `PR-RUN-003` · `BP-ADP-002` |

### 6.2 Adapter Lifecycle

An adapter occupies exactly one of five observable lifecycle states. Every transition is reported as
an observed fact; none is implicit.

| State | Meaning |
| :--- | :--- |
| **Declared** | A declaration has been submitted. Confers no availability. |
| **Admitted** | The declaration has been accepted and registered. Availability not yet confirmed. |
| **Available** | Observed as reachable and capable of mediating a request. |
| **Unavailable** | Admitted but not currently observed as reachable. |
| **Retired** | Withdrawn from use. No longer offered for selection. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-004` | An adapter MUST occupy exactly one of the states Declared, Admitted, Available, Unavailable, or Retired at any time. | `PR-NFR-001` · `PR-SAF-011` |
| `SP-ADP-005` | A transition between lifecycle states MUST be reported as an observed fact, and MUST NOT be inferred from the mere absence of contrary evidence. | `PR-SAF-011` · `BP-RUN-011` |
| `SP-ADP-006` | Retirement of an adapter MUST NOT alter, invalidate, or require modification of any Repository Asset that referenced the adapter's advertised capability while it was Available. | `PR-RUN-005` · `PR-RUN-012` |
| `SP-ADP-007` | An adapter MUST remain in the Declared state, conferring no availability, until Adapter Admission ([Section 6.3](#63-adapter-registration)) completes. | `PR-SAF-002` · `BP-ADP-005` |

### 6.3 Adapter Registration

Registration is the act by which an Admitted adapter's declaration becomes an asset the Runtime
Blueprint may present for selection.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-008` | Adapter Admission MUST accept an adapter's declaration as an additive act that requires no modification of any previously admitted adapter, Workflow, Rule, Skill, or Prompt. | `PR-RUN-012` · `PR-RUN-016` · `BP-ADP-005` · `BP-ADP-006` |
| `SP-ADP-009` | Registration MUST hold the adapter's declaration as a durable, versioned Repository Asset, inspectable independent of the adapter's current lifecycle state. | `PR-NFR-001` · `PR-RUN-012` |
| `SP-ADP-010` | Registration MUST record the point at which the adapter's declaration was accepted, as an observed fact available for inspection. | `PR-NFR-001` · `BP-ADP-005` |

### 6.4 Adapter Discovery

Discovery is how an Admitted adapter becomes visible as a candidate for Runtime selection, distinct
from Availability.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-011` | An Admitted adapter MUST be discoverable as a candidate for Runtime selection without requiring the selecting party to name the adapter's implementation mechanism. | `PR-ENV-004` · `PR-RUN-004` |
| `SP-ADP-012` | Discovery MUST report an adapter's declared identity together with its current lifecycle state, so that a candidate reported as Available is distinguishable from one reported as Declared only. | `PR-ENV-004` · `PR-SAF-011` |
| `SP-ADP-013` | The absence of a discoverable adapter for a given Runtime MUST reduce the set of selectable options and MUST NOT be reported as a failure of any other capability. | `PR-RUN-010` · `BP-ADP-004` · `BP-RUN-008` |

### 6.5 Capability Declaration

Capability Declaration is the adapter's statement, in the neutral Engineering Capability vocabulary
the Runtime Blueprint holds, of what its mediated Runtime offers.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-014` | An adapter MUST declare its offered Engineering Capabilities using only the neutral vocabulary; a declaration MUST NOT name the mediated Runtime, Vendor, or Model in place of a capability term. | `PR-RUN-002` · `PR-RUN-007` · `BP-ADP-003` · `BP-RUN-003` |
| `SP-ADP-015` | A capability declaration MUST NOT assert a capability the adapter's mediated Runtime does not provide, and MUST NOT omit a capability the Runtime provides that the vocabulary already expresses. | `PR-RUN-007` · `PR-RUN-008` · `BP-ADP-003` |
| `SP-ADP-016` | An adapter MUST NOT declare a capability that reproduces work AEOS itself performs outside inference. | `PR-RUN-002` · `BP-ADP-012` |
| `SP-ADP-017` | A change to an adapter's capability declaration MUST be reported as a revision, distinguishable from its initial declaration, and MUST NOT silently alter a capability match already reported for a Workflow step in progress. | `PR-RUN-007` · `PR-WFL-016` · `BP-RUN-004` |

### 6.6 Capability Negotiation

Negotiation is the exchange by which a step's declared requirement is compared against an adapter's
declared offer, and a match or a gap is stated before work begins.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-018` | An adapter MUST respond to a capability requirement with either a confirmation that it can be satisfied or a statement of the gap, expressed in the same neutral vocabulary as the requirement. | `PR-RUN-007` · `PR-WFL-016` · `BP-RUN-004` |
| `SP-ADP-019` | An adapter MUST complete negotiation for a step before any request for that step is dispatched to its mediated Runtime. | `PR-RUN-007` · `BP-RUN-004` |
| `SP-ADP-020` | An adapter MUST NOT partially satisfy a declared requirement and report it as fully satisfied. | `PR-RUN-008` · `PR-SAF-011` |
| `SP-ADP-021` | Where an adapter's declared offer changes between negotiation and dispatch, the adapter MUST report the change rather than dispatch against a superseded negotiation. | `PR-RUN-011` · `PR-SAF-011` |

### 6.7 Runtime Compatibility

Runtime Compatibility is the adapter's statement of the conditions under which it is able to mediate
its Runtime.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-022` | An adapter MUST declare the compatibility conditions under which it is able to mediate its Runtime, stated as observable conditions rather than as an assumption. | `PR-ENV-004` · `PR-SAF-011` |
| `SP-ADP-023` | Where the mediated Runtime does not meet the adapter's declared compatibility conditions, the adapter MUST report itself as Unavailable rather than attempt mediation. | `PR-RUN-010` · `PR-SAF-002` · `BP-ADP-004` |
| `SP-ADP-024` | An adapter's compatibility declaration MUST NOT be used by any other layer to infer compatibility with a different adapter or a different Runtime. | `PR-RUN-003` · `BP-ADP-001` |

### 6.8 Model Compatibility

Where a mediated Runtime exposes more than one Model, the adapter states which it is compatible with,
without that knowledge propagating beyond the adapter's own declaration.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-025` | An adapter MAY declare the Models its mediated Runtime exposes as part of its capability declaration, expressed as an opaque reference rather than as a term that alters the neutral vocabulary. | `PR-RUN-006` · `BP-ADP-001` · `AR-BND-005` |
| `SP-ADP-026` | A Model reference declared by an adapter MUST NOT be required by any Workflow, Rule, Skill, or Prompt in order to function. | `PR-RUN-005` · `PR-RUN-006` |
| `SP-ADP-027` | An adapter MUST NOT declare Model compatibility in terms that privilege one Model over another for a capability both are declared to satisfy. | `PR-RUN-003` · `PR-RUN-004` · `BP-ADP-011` |

### 6.9 Context Delivery Responsibilities

Context arrives at the adapter already selected, justified, and composed by the Context Blueprint. The
adapter's responsibility is to carry it across the boundary without altering its meaning and to
transmit nothing beyond what the approved crossing accounted for.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-028` | An adapter MUST transmit to its mediated Runtime only the Context composed and disclosed for the approved crossing, and MUST NOT add, omit, or reorder material in a way that changes its meaning. | `PR-SAF-007` · `PR-SAF-008` |
| `SP-ADP-029` | An adapter MUST NOT retain a copy of transmitted Context beyond what is required to complete the mediation in progress. | `PR-REP-015` |
| `SP-ADP-030` | An adapter MUST report, on request, the measured size of Context actually transmitted, as material for cost disclosure. | `PR-RUN-009` · `PR-SAF-008` · `BP-RUN-007` |
| `SP-ADP-031` | An adapter MUST NOT transmit Context to its mediated Runtime for a step that has not completed Capability Negotiation for that step. | `PR-RUN-007` · `PR-SAF-007` |

### 6.10 Prompt Delivery Responsibilities

A Prompt is composed above the adapter. The adapter's responsibility is to deliver it intact and to
report what was actually sent.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-032` | An adapter MUST deliver a composed Prompt to its mediated Runtime without altering its instructional content. | `PR-RUN-008` · `PR-SAF-008` |
| `SP-ADP-033` | Where an adapter must transform a Prompt's form to make it acceptable to its mediated Runtime, the transformation MUST preserve the Prompt's meaning and MUST be reportable on request. | `PR-RUN-008` · `PR-NFR-001` |
| `SP-ADP-034` | An adapter MUST NOT substitute a different Prompt than the one composed and disclosed for the approved crossing. | `PR-SAF-007` · `PR-SAF-008` |

### 6.11 Invocation Responsibilities

Invocation is the act of dispatching a mediated request to the Runtime once negotiation, disclosure,
and approval are complete.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-035` | An adapter MUST NOT initiate an invocation of its mediated Runtime that was not dispatched to it by the Runtime Layer following an approved crossing. | `PR-SAF-005` · `PR-RUN-009` · `AR-BND-003` |
| `SP-ADP-036` | An adapter MUST express an invocation in the terms its mediated Runtime accepts, translated from the runtime-independent request it received. | `PR-RUN-002` · `PR-RUN-005` · `BP-ADP-001` |
| `SP-ADP-037` | An adapter MUST NOT retry a failed invocation on its own initiative; a retry MUST be dispatched again by the Runtime Layer as a new invocation. | `PR-RUN-011` · `BP-RUN-010` · `BP-ADP-009` |
| `SP-ADP-038` | An adapter MUST NOT combine more than one approved crossing into a single invocation, and MUST NOT split one approved crossing into invocations that exceed the scope approved. | `PR-SAF-005` · `AR-BND-014` |
| `SP-ADP-039` | An adapter MUST make available, on request, the actual scope of what was transmitted in an invocation it performed. | `PR-NFR-001` · `PR-SAF-008` |

### 6.12 Response Normalization Responsibilities

Result Mediation converts what the mediated Runtime returns into neutral, runtime-independent terms.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-040` | An adapter MUST convert a result returned by its mediated Runtime into the runtime-independent form the Runtime Layer expects before returning it. | `PR-RUN-005` · `PR-RUN-008` |
| `SP-ADP-041` | Normalization MUST NOT add content the mediated Runtime did not return, and MUST NOT omit content the Runtime returned that the runtime-independent form is capable of expressing. | `PR-SAF-011` · `PR-NFR-001` |
| `SP-ADP-042` | A normalized result MUST remain distinguishable from an observed fact where the underlying content is itself an inference performed by the mediated Runtime. | `PR-SAF-011` |
| `SP-ADP-043` | A normalized result MUST NOT be treated by the adapter as authorizing any action, satisfying any Approval Gate, or expanding any approved scope. | `PR-SAF-005` · `PR-WFL-005` · `AR-BND-004` |

### 6.13 Error Normalization Responsibilities

Fault Mediation converts a failure of the mediated Runtime into an ordinary, neutral condition the
Workflow Blueprint can act on, without resolving it by substitution.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-044` | An adapter MUST convert a fault arising from its mediated Runtime into a neutral condition expressed in terms that name no other Runtime. | `PR-RUN-011` · `BP-ADP-009` |
| `SP-ADP-045` | An adapter MUST NOT resolve a fault by silently substituting a different Runtime, Vendor, or Model. | `PR-RUN-004` · `PR-RUN-011` · `BP-ADP-009` |
| `SP-ADP-046` | A normalized fault MUST distinguish, where the mediated Runtime's own report distinguishes, between a condition that may succeed on a subsequent, separately approved attempt and one that will not. | `PR-RUN-011` · `PR-SAF-002` |
| `SP-ADP-047` | An adapter MUST NOT include a credential or secret in a normalized fault. | `PR-SAF-006` · `PR-RUN-014` · `BP-ADP-008` |
| `SP-ADP-048` | An adapter MUST report a fault rather than absorb it silently; the absence of a reported result MUST NOT be the only signal that a fault occurred. | `PR-RUN-011` · `PR-NFR-001` |

### 6.14 Cancellation Behavior

Cancellation halts an invocation in progress at the request of the layer that dispatched it.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-049` | An adapter MUST accept a cancellation signal for an invocation it is currently mediating. | `PR-SAF-005` · `PR-RUN-009` |
| `SP-ADP-050` | On accepting a cancellation, an adapter MUST NOT initiate a new invocation on behalf of the cancelled request. | `PR-RUN-011` · `PR-SAF-005` |
| `SP-ADP-051` | An adapter MUST report the outcome of a cancellation as one of: halted before invocation, halted during invocation with no result retained, or not cancellable because the mediated Runtime had already returned a result. An adapter MUST NOT report a cancellation as successful where the underlying invocation had already completed. | `PR-SAF-011` · `PR-NFR-001` |
| `SP-ADP-052` | A cancelled invocation MUST NOT be counted or reported as a completed unit of Runtime usage. | `PR-RUN-015` |

### 6.15 Timeout Behavior

A timeout is a bounded wait for a response from the mediated Runtime, expired without an
adapter-initiated retry.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-053` | An adapter MUST observe a bounded wait for a response to a dispatched invocation, and MUST report the expiry of that wait as a fault, normalized per [Section 6.13](#613-error-normalization-responsibilities), rather than continue waiting indefinitely. | `PR-SAF-002` · `PR-RUN-011` |
| `SP-ADP-054` | An adapter MUST NOT treat an expired wait as a successful invocation with an empty result. | `PR-SAF-011` |
| `SP-ADP-055` | An adapter MUST NOT initiate a new invocation following an expired wait without a new dispatch from the Runtime Layer. | `PR-RUN-011` · `PR-SAF-005` |
| `SP-ADP-056` | Where an adapter's mediated Runtime later returns a result after the wait has already expired and been reported, the adapter MUST NOT deliver that late result as though it were a response to a subsequent, separately dispatched invocation. | `PR-SAF-011` · `PR-RUN-011` |

### 6.16 Health Reporting

Health Reporting is the adapter's account of whether its mediated Runtime is currently reachable,
stated as an observation rather than an assumption.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-057` | An adapter MUST report its mediated Runtime's reachability as observed at the time of the report, and MUST NOT report reachability inferred from the mere absence of a prior fault. | `PR-ENV-004` · `PR-SAF-011` · `BP-RUN-011` |
| `SP-ADP-058` | A health report MUST distinguish observed-reachable, observed-unreachable, and not-yet-observed; an adapter MUST NOT default an unobserved condition to reachable. | `PR-SAF-002` · `PR-SAF-011` |
| `SP-ADP-059` | An adapter MUST make its current health report available to the Runtime Layer without requiring an invocation to be dispatched first. | `PR-ENV-004` · `PR-RUN-010` |
| `SP-ADP-060` | A report that a mediated Runtime is unreachable MUST reduce the options available for selection and MUST NOT alter durable project state. | `PR-RUN-010` · `BP-RUN-008` · `BP-RUN-009` |

### 6.17 Version Compatibility

An adapter declares the version of this behavior domain, and of its own revision, against which it was
written, so that incompatibility is reported rather than discovered as a defect.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-061` | An adapter MUST declare the version of this Specification it was written to satisfy. | `PR-NFR-001` · `PR-RUN-012` |
| `SP-ADP-062` | Where an adapter's declared version is incompatible with the version of this Specification in force, the condition MUST be reported before the adapter is offered for selection, and the adapter MUST NOT be admitted silently. | `PR-SAF-002` · `PR-RUN-011` |
| `SP-ADP-063` | A Minor or Patch change to this Specification, per AEOS-SPECSTD Section 18.1, MUST NOT invalidate an adapter declared compatible with a prior Minor or Patch version. | `PR-NFR-002` · `PR-RUN-012` |
| `SP-ADP-064` | An adapter's declared compatibility with this Specification MUST be stated independently of the version of the Runtime it mediates; the two are reported separately. | `PR-RUN-003` · `PR-RUN-006` |

### 6.18 Security Responsibilities

Security responsibilities confine what an adapter may hold, transmit, and disclose, consistent with
the boundary the Adapter Blueprint is assigned.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-065` | An adapter MUST NOT record a credential or secret in any durable artifact, including its own declaration. | `PR-SAF-006` · `PR-RUN-014` · `BP-ADP-008` · `AR-BND-009` |
| `SP-ADP-066` | An adapter MUST hold a credential its mediated Runtime requires only as Runtime State, for no longer than the duration of the invocation it authorizes. | `PR-RUN-014` · `PR-SAF-006` |
| `SP-ADP-067` | An adapter MUST NOT transmit project content to a Runtime other than the one it mediates and that the user has selected and approved. | `PR-SAF-007` · `BP-ADP-002` · `BP-ADP-011` |
| `SP-ADP-068` | An adapter MUST supply the Runtime Layer with the material needed to disclose the scope and expected cost of a crossing before the crossing is proposed, and MUST NOT initiate a crossing that has not been so disclosed. | `PR-SAF-008` · `PR-RUN-009` · `BP-RUN-006` · `BP-RUN-007` |
| `SP-ADP-069` | An adapter's presence or absence MUST confer no privilege on the Runtime it mediates over any other admitted adapter. | `PR-RUN-003` · `PR-RUN-004` · `BP-ADP-011` |

---

## 7. Constraints

The invariants this behavior domain operates under, per `MD5`. Unlike [Section 6](#6-behavior), which
states conditional rules for defined situations, these hold continuously across every adapter's
admission.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-ADP-070` | An adapter MUST mediate exactly one Runtime for the whole of its admission; mediating a second Runtime requires a second, separately admitted adapter. | `PR-RUN-003` · `BP-ADP-002` |
| `SP-ADP-071` | Knowledge of a specific Runtime, Vendor, or Model MUST NOT be observable outside the adapter that declares it. | `PR-RUN-002` · `PR-RUN-005` · `PR-RUN-006` · `BP-ADP-001` · `AR-BND-005` |
| `SP-ADP-072` | An adapter MUST NOT alter, extend, or narrow a Workflow, Rule, Skill, or Prompt as a condition of mediating a request. | `PR-RUN-005` · `BP-ADP-007` |
| `SP-ADP-073` | An adapter MUST NOT be required for the operation of any behavior domain that does not require inference. | `PR-RUN-010` · `BP-ADP-004` |
| `SP-ADP-074` | An adapter MUST NOT declare or perform a capability that duplicates work its mediated Runtime already performs. | `PR-RUN-002` · `BP-ADP-012` |
| `SP-ADP-075` | An adapter's declaration MUST remain a Repository Asset independent of the adapter's current lifecycle state; retirement marks the declaration retired in place and does not remove it. | `PR-NFR-001` · `PR-RUN-012` |
| `SP-ADP-076` | An adapter MUST NOT hold engineering policy — no Rule, no Approval Gate, no context-selection decision, no Workflow knowledge. | `PR-WFL-005` · `PR-SAF-001` · `AR-BND-003` |
| `SP-ADP-077` | An adapter MUST NOT address any layer other than by returning a result or a normalized fault to the Runtime Layer. | `PR-NFR-006` · `AR-DEP-004` |
| `SP-ADP-078` | An adapter's observable behavior MUST be identical across every supported Platform and Distribution Method. | `PR-PLT-003` · `PR-DST-005` |
| `SP-ADP-079` | An adapter MUST NOT be privileged or excluded on the basis of the Distribution Method through which AEOS was delivered. | `PR-DST-006` |

---

## 8. Extension Points

Where and how this behavior domain admits future variation without altering a rule already stated in
[Section 6](#6-behavior) or [Section 7](#7-constraints), per `MD6`. Each states, per `EX3`, the
boundary within which variation is permitted and the point beyond which a new identifier or a revision
under AEOS-SPECSTD Section 18 is required.

| ID | Extension point | Boundary | Traces to |
| :--- | :--- | :--- | :--- |
| `SP-ADP-080` | **Adapter Admission.** A Runtime not previously mediated MAY be admitted by declaring a new adapter that satisfies [Sections 6.1 through 6.18](#61-adapter-identity). | Admission MUST NOT alter the behavior required of any previously admitted adapter, nor require a change to any existing project. | `PR-RUN-012` · `PR-RUN-016` · `BP-ADP-005` · `BP-ADP-006` |
| `SP-ADP-081` | **Capability Offer Extension.** An adapter's declared capability offer MAY be revised to add a capability the neutral vocabulary already expresses, or, where the vocabulary itself is extended under the Runtime Blueprint's own extension point, to declare an offer against the extended term. | A capability offer extension MUST NOT declare an offer in terms outside the neutral vocabulary. | `PR-RUN-007` · `BP-ADP-003` |
| `SP-ADP-082` | **Runtime Category Admission.** An adapter MAY mediate a Runtime of a category not previously mediated by any admitted adapter — a commercial service, an open-source model, an interoperability standard, or a user-supplied extension — without a change to this Specification. | The admitted adapter MUST satisfy every rule in [Sections 6](#6-behavior) and [7](#7-constraints) without exception for its category. | `PR-RUN-016` · `BP-ADP-005` |
| `SP-ADP-083` | **Model Exposure Extension.** An adapter MAY revise its declared Model references as its mediated Runtime's exposed Models change, without re-admission. | The revision MUST be reported per [Section 6.5](#65-capability-declaration) and MUST NOT be required by any Workflow, Rule, Skill, or Prompt. | `PR-RUN-006` |
| `SP-ADP-084` | **Version Compatibility Extension.** An adapter MAY declare compatibility with a later Minor or Patch version of this Specification without a new admission. | A Major version of this Specification requires the adapter's declared compatibility to be revised before it is offered for selection. | `PR-RUN-012` |

An extension admitted under this section MUST NOT be used to weaken a rule stated in
[Section 6](#6-behavior) or [Section 7](#7-constraints), per `EX4`. A weakening disguised as an
addition is a defect in the extending adapter's declaration, not a use of these extension points.

---

## 9. Traceability

Every rule in [Section 6](#6-behavior), [Section 7](#7-constraints), and
[Section 8](#8-extension-points) states its own trace inline, per `TR1` and `TR2`. This section
consolidates that trace as the acceptance-criteria index `MD9` requires: a reviewer or an automated
test can confirm conformance for a given `PR-` or `BP-` identifier by locating every `SP-ADP-` rule
that cites it and checking the described observable behavior against an implementation.

| `PR-` / `BP-` / `AR-` identifier | `SP-ADP-` rules that trace to it |
| :--- | :--- |
| `PR-RUN-001` | Governs this domain's boundary as a whole; no adapter behavior performs inference. |
| `PR-RUN-002` | `014` `016` `036` `071` `074` |
| `PR-RUN-003` | `001` `003` `024` `027` `064` `069` `070` |
| `PR-RUN-004` | `013`* `024`* `027` `045` `069` (*via `BP-ADP` citation) |
| `PR-RUN-005` | `001` `006` `026` `036` `071` `072` |
| `PR-RUN-006` | `002` `025` `026` `027` `064` `083` |
| `PR-RUN-007` | `014` `015` `017` `018` `019` `020` `031` `081` |
| `PR-RUN-008` | `015` `020` `032` `033` `040` |
| `PR-RUN-009` | `030` `035` `049` `068` |
| `PR-RUN-010` | `013` `023` `059` `060` `073` |
| `PR-RUN-011` | `017` `021` `037` `044` `045` `046` `048` `050` `053` `055` `056` `062` |
| `PR-RUN-012` | `006` `008` `009` `061` `063` `075` `080` `084` |
| `PR-RUN-014` | `047` `065` `066` |
| `PR-RUN-015` | `052` |
| `PR-RUN-016` | `008` `082` `080` |
| `PR-ENV-004` | `011` `012` `022` `057` `059` |
| `PR-WFL-005` | `043` `076` |
| `PR-WFL-016` | `017` `018` |
| `PR-SAF-001` | `076` |
| `PR-SAF-002` | `007` `023` `046` `053` `058` `062` |
| `PR-SAF-005` | `035` `038` `043` `049` `050` `055` |
| `PR-SAF-006` | `047` `065` `066` |
| `PR-SAF-007` | `028` `031` `034` `067` |
| `PR-SAF-008` | `028` `030` `032` `034` `039` `068` |
| `PR-SAF-011` | `004` `005` `012` `020` `021` `022` `041` `042` `051` `054` `056` `057` `058` |
| `PR-NFR-001` | `004` `009` `010` `033` `039` `041` `048` `051` `061` `075` |
| `PR-NFR-002` | `063` |
| `PR-NFR-006` | `077` |
| `PR-REP-015` | `029` |
| `PR-PLT-003` | `078` |
| `PR-DST-005` | `078` |
| `PR-DST-006` | `079` |
| `BP-ADP-001` | `002` `024` `025` `036` `071` |
| `BP-ADP-002` | `003` `070` `067` |
| `BP-ADP-003` | `014` `015` `081` |
| `BP-ADP-004` | `013` `023` `073` |
| `BP-ADP-005` | `007` `008` `010` `080` `082` |
| `BP-ADP-006` | `008` `080` |
| `BP-ADP-007` | `072` |
| `BP-ADP-008` | `047` `065` |
| `BP-ADP-009` | `044` `045` `037` |
| `BP-ADP-011` | `027` `067` `069` |
| `BP-ADP-012` | `016` `074` |
| `BP-RUN-003` | `014` |
| `BP-RUN-004` | `017` `018` `019` |
| `BP-RUN-006` | `068` |
| `BP-RUN-007` | `030` `068` |
| `BP-RUN-008` | `013` `060` |
| `BP-RUN-009` | `060` |
| `BP-RUN-010` | `037` |
| `BP-RUN-011` | `005` `057` |
| `AR-BND-003` | `035` `076` |
| `AR-BND-004` | `043` |
| `AR-BND-005` | `025` `071` |
| `AR-BND-009` | `065` |
| `AR-BND-014` | `038` |
| `AR-DEP-004` | `077` |

---

## 10. Non-goals

Behavior a reader might reasonably expect this domain to cover, which it deliberately does not, per
`MD8`.

| Non-goal | Reason |
| :--- | :--- |
| An SDK, method signature, or interface definition for building an adapter | Prohibited by `MN4`; owned by Implementation Guides once this Specification is realized in code. |
| An HTTP endpoint, JSON schema, or wire format for a mediated request or result | Prohibited by `MN4`; a technology and protocol decision outside the Specification layer. |
| A statement specific to any named protocol or vendor interface, including the Model Context Protocol or any commercial runtime's API | Prohibited by `MN3`; this Specification is satisfiable by any technology meeting the stated behavior, per the single-satisfiability test in AEOS-SPECSTD Section 9. |
| A transport mechanism, process model, or concurrency model for how mediation actually executes | Owned by Runtime documents, per AEOS-BLUEPRINT Section 18, which this document is not. |
| An installation, configuration, or deployment procedure for an adapter | Prohibited by `MN5`. |
| The algorithm by which the Runtime Blueprint matches a step's requirement against multiple adapters' offers, selects among them, or degrades when none is available | Owned by the Runtime Blueprint's own behavior domain, a companion Specification not yet authored; see [Section 11](#11-references). |
| The mechanics of Approval Gate placement or how Human Approval is obtained | Owned by the Human Interaction Blueprint's behavior domain. |
| A credential storage mechanism | This Specification states only that a credential MUST NOT be durable ([Section 6.18](#618-security-responsibilities)); the mechanism belongs to Implementation Guides. |
| A statement of which component, service, or process realizes an adapter | Prohibited by `MN7`; this Specification binds the position AEOS-BLUEPRINT assigns, not any structural element. |
| Runtime capability benchmarking or ranking of one Runtime against another | Excluded from the product itself, per AEOS-PRD Appendix A, R6; restating it here would exceed this document's authority. |

---

## 11. References

### 11.1 Governing Documents

| Document | Cited for |
| :--- | :--- |
| AEOS-VISION | The invariants this Specification MUST NOT trade away, per AEOS-SPECSTD Section 3.1. |
| AEOS-PRD | The `PR-RUN-001` through `PR-RUN-016`, `PR-SAF-`, `PR-ENV-004`, `PR-WFL-016`, `PR-NFR-`, `PR-PLT-003`, `PR-REP-015`, and `PR-DST-` identifiers every rule in this document traces to. |
| AEOS-GLOSSARY | The definitions of Runtime, Runtime Adapter, Engineering Capability, Vendor, Model, Repository Asset, Runtime State, and every other term this document uses without redefining. |
| AEOS-DOCSTD | The form, structure, language, and lifecycle every AEOS document takes, including this one. |
| AEOS-SPECSTD | The form, structure, identifier convention, and traceability rules specific to the Specification layer, applied throughout this document. |
| AEOS-ARCH | The layer model and the boundary invariants this Specification is written against, in particular `AR-BND-002`, `AR-BND-003`, `AR-BND-004`, `AR-BND-005`, `AR-BND-009`, `AR-BND-014`, and `AR-DEP-004`. |
| AEOS-BLUEPRINT | The Runtime Blueprint (Section 10, `BP-RUN`) and the Adapter Blueprint (Section 11, `BP-ADP`), whose arrangement this Specification states behavior for, and Sections 17 and 18, which fix this Specification's boundary against the Blueprint layer and against Runtime documents respectively. |

### 11.2 Forward References (Non-normative)

The following documents govern behavior domains adjacent to this one. Each corresponds to a
responsibility AEOS-GLOSSARY reserves for architecture, or to an arrangement AEOS-BLUEPRINT already
assigns, and each would ordinarily be cited by identifier under `DP1` where this Specification depends
on a rule it states. None had been authored as a Specification document at the time this document was
written. Consistent with `DS-P-10`, that gap is recorded here as a finished statement rather than
worked around by inventing content this document has no authority to state:

| Document | Adjacent behavior domain | Relationship to this document |
| :--- | :--- | :--- |
| `AEOS_CONTEXT_ROUTER.md` | The Context Router responsibility (AEOS-GLOSSARY) — Context selection, justification, and composition, arranged by the Context Blueprint. | Supplies the Context this document's [Section 6.9](#69-context-delivery-responsibilities) receives already composed. This document states no rule about how that composition occurs. |
| `AEOS_WORKFLOW_ENGINE.md` | The Workflow Engine responsibility (AEOS-GLOSSARY) — incremental execution, Approval Gates, and Workflow State, arranged by the Workflow Blueprint. | Dispatches the approved crossings this document's [Section 6.11](#611-invocation-responsibilities) mediates. This document states no rule about gate placement or step sequencing. |
| `AEOS_STATE_MACHINE.md` | Workflow State — the durable record of a Workflow's position, held by the Repository. | Records the outcome this document's Outputs ([Section 5](#5-outputs)) return upward. This document holds no durable state of its own, per [Section 7](#7-constraints), `SP-ADP-077`. |
| `AEOS_RUNTIME_FLOW.md` | The Runtime Blueprint's own behavior domain (`BP-RUN`) — capability matching, selection custody, boundary disclosure assembly, and degradation handling. | Supplies this document's Inputs ([Section 4](#4-inputs)) and consumes this document's Outputs. This document states no rule about matching, custody, disclosure presentation, or degradation policy; see [Section 2.4](#24-what-this-specification-does-not-cover). |
| `AEOS_SPEC.md` | A prospective index of the Specification layer as a whole. | Not consulted in the authorship of this document beyond AEOS-SPECSTD, which governs the Specification layer directly. Where such an index is later authored, this document is expected to be listed under the `ADP` area it registers. |

No rule in this document depends, by identifier, on any of the five documents above, per `DP1` and
`DP2`. Once each is authored, it MUST be consulted to confirm that no implicit dependency has been
introduced into this document in the interval, per `DP5`.

---

## 12. Document Governance

### 12.1 Status

This document is authored as the Freeze candidate for the Runtime Adapter behavior domain of AEOS
1.0. Its current lifecycle state is **Draft**, per AEOS-DOCSTD Section 12.1: it is not yet
authoritative and MUST NOT be referenced as a source of truth until it has passed Review, Revision,
and Approval under AEOS-DOCSTD Section 12.2 and the Freeze checklist AEOS-SPECSTD Section 19.2 states.
Downstream Implementation Guides, tests, issues, and pull requests MAY reference the `SP-ADP-`
identifiers stated here in anticipation of Freeze, consistent with `TR5`, but MUST treat them as
subject to revision until this document's status changes.

### 12.2 Change Control

Versioning and change control for this document follow AEOS-SPECSTD Section 18.1 without
modification: a Patch increment for an editorial correction with no change of meaning; a Minor
increment for the addition of a new `SP-ADP-` identifier that does not alter what an existing
identifier requires; a Major increment for any change to what an existing identifier requires, any
retirement, or any change to the `ADP` area registration itself.

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
| An adapter's declared behavior conflicts with a rule this document states | This document governs. The adapter is nonconformant until corrected. |
| This document conflicts with a companion Specification governing an adjacent behavior domain, once authored | Escalated to the owner, per AEOS-SPECSTD Section 3.3. Neither document is resolved unilaterally. |

### 12.5 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial authoring of the Runtime Adapter Specification. Registers the `ADP` area code for the Specification layer, matching AEOS-BLUEPRINT's `BP-ADP` registration for the same behavior domain. States 84 `SP-ADP-` rules across eighteen behavior areas — adapter identity, lifecycle, registration, discovery, capability declaration, capability negotiation, runtime compatibility, model compatibility, context delivery, prompt delivery, invocation, response normalization, error normalization, cancellation, timeout, health reporting, version compatibility, and security responsibilities — ten cross-cutting constraints, and five extension points, each traced to one or more `PR-`, `BP-`, or `AR-` identifiers. Defines Inputs and Outputs for the domain and states ten explicit Non-goals. Introduces no product requirement, no architectural decision, no Blueprint arrangement, and no implementation; redefines no term AEOS-GLOSSARY owns. |

---

**End of Runtime Adapter Specification**

AEOS-SPEC-ADP · Version 1.0.0 · Traces to `PR-RUN-001`–`PR-RUN-016` · `PR-SAF-001` · `PR-SAF-002` ·
`PR-SAF-005` · `PR-SAF-006` · `PR-SAF-007` · `PR-SAF-008` · `PR-SAF-011` · `PR-ENV-004` ·
`PR-WFL-005` · `PR-WFL-016` · `PR-NFR-001` · `PR-NFR-002` · `PR-NFR-006` · `PR-REP-015` ·
`PR-PLT-003` · `PR-DST-005` · `PR-DST-006`
