# AI Engineering Operating System

## AEOS — Context Router Specification

*The permanent statement of the specified, observable behavior of the Context Router.*

| Field | Value |
| :--- | :--- |
| **Document** | Context Router Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SPEC-CTX |
| **Version** | 1.0.1 |
| **Status** | Frozen |
| **Owner** | Product Owner, AEOS |
| **Author** | Chief Specification Architect, AEOS |
| **Audience** | Architects, implementers, reviewers, test authors, and AI runtimes consuming this repository |
| **Suggested path** | `docs/specification/CONTEXT_ROUTER_SPECIFICATION.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `AEOS_SPEC.md` (AEOS-SPEC) |
| **Supersedes** | None |
| **Area code** | `CTX` |

> **Authority of this document.**
> This document specifies, precisely and testably, the **observable behavior of the Context
> Router** — the responsibility AEOS-GLOSSARY reserves under that name and AEOS-ARCH Section 4.5
> assigns to the Context Layer, arranged by AEOS-BLUEPRINT Section 9 as the Context Blueprint
> (`BP-CTX`). It registers the `CTX` behavior domain under AEOS-SPECSTD Section 11.4 and attaches at
> the `EPS-3` extension point AEOS-SPEC Section 8.1 declares for exactly this domain.
>
> It defines no vision, no product requirement, no terminology, no architecture, no Blueprint
> arrangement, no interface, no algorithm, no technology, and no implementation. It redefines
> nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPEC;
> where a statement here appears to, that is a defect in this document and MUST be reported rather
> than acted upon. It sits below AEOS-PRD, AEOS-ARCH, and AEOS-BLUEPRINT, and beside AEOS-SPEC,
> whose `SP-SYS-023`, `SP-SYS-039`, `SP-SYS-040`, and `SP-SYS-052` it depends upon and does not
> restate. It is written entirely under AEOS-SPECSTD, which governs its form, structure, identifier
> convention, traceability, and lifecycle; where this document and AEOS-SPECSTD both speak to a
> subject, AEOS-SPECSTD governs the form and this document governs the content of the `CTX`
> behavior domain. Where this document and a document of higher authority both speak to the same
> subject, the higher-authority document governs and the conflict is a defect to be reported.

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
13. [Appendix A — SP-CTX Rule Index](#appendix-a--sp-ctx-rule-index)

---

## 1. Purpose

AEOS-ARCH states that the Context Layer exists to determine the smallest set of information
sufficient for one step of work and to retain the reason each element was included, and that in
doing so it discharges the responsibility AEOS-GLOSSARY reserves under the name **Context Router**.
AEOS-BLUEPRINT arranges that layer into five subsystems — Context Selection, Inclusion
Justification, Prompt Composition, Sensitivity Exclusion, and Composition Disclosure — and states
the boundaries between them. Neither document is precise enough to determine whether a given
selection, composition, or exclusion is correct, and neither should be: that precision belongs to
the Specification layer, which AEOS-SPECSTD governs and AEOS-SPEC's `SYS` domain reserves as `EPS-3`
for exactly this purpose.

This document is that domain-specific Specification. It states, rule by rule, what the Context
Layer must do whenever a step of work requires Context: what it may select, why an inclusion must
be justified, how a selection is composed with a step's declared Prompt assets, what must never
enter that composition, and what must be true of a composed result before it leaves this domain. It
states nothing about how selection, composition, or exclusion is mechanically carried out; that
question belongs to Implementation Guides once they exist for this domain.

Precision matters here specifically because this domain is where project information last passes
through a boundary AEOS controls before a composed result moves toward a Runtime. AEOS-PRD's `PR-PMT`
requirements and AEOS-ARCH's Minimum Sufficient Crossing principle already establish that this
crossing must be minimized and justified; this document is where that obligation becomes a set of
rules a reviewer or a test can check without asking the author what was meant.

---

## 2. Scope

### 2.1 Behavior Domain and Area Registration

This document registers the area code `CTX` under AEOS-SPECSTD Section 11.4. The `CTX` behavior
domain is the observable behavior of the Context Layer AEOS-ARCH Section 4.5 defines and the
Context Blueprint AEOS-BLUEPRINT Section 9 arranges: the selection of Context for one step of work,
the justification of every selected element, the composition of that selection with a step's
declared Prompt assets, the exclusion of credentials and project-designated sensitive content, and
the disclosure of the composed result before it is passed outward.

The five subsystems AEOS-BLUEPRINT Section 9.2 names — Context Selection, Inclusion Justification,
Prompt Composition, Sensitivity Exclusion, and Composition Disclosure — are this document's complete
organizing structure. This document introduces no subsystem beyond those five, consistent with
AEOS-BLUEPRINT `BP-GOV-010`, and relocates none of them, consistent with `BP-GOV-009`.

### 2.2 Attachment to the System Specification

This document attaches at the `EPS-3` extension point AEOS-SPEC Section 8.1 declares for the Context
behavior domain. Consistent with `SP-SYS-EXT-1`, it registers `CTX` and does not reuse `SYS`.
Consistent with `SP-SYS-EXT-2`, it cites rather than restates the `SP-SYS` rules it depends on; that
declaration is stated in full in [Section 7.4](#74-declared-dependencies). Consistent with
`SP-SYS-EXT-3`, no rule in this document contradicts a rule AEOS-SPEC Section 6 or Section 7 states,
and in particular none narrows, widens, or restates `SP-SYS-039`, `SP-SYS-040`, or `SP-SYS-052`,
which `EPS-3` fixes as the boundary this document must not disturb.

### 2.3 Boundary of This Document

This document specifies the behavior of the Context Layer alone. It does not specify how a Workflow
step comes to declare a context need, which the Workflow behavior domain owns; how a composed result
is presented to a person, which the Human Interaction behavior domain owns; how a composed result is
transmitted to a Runtime, which the Adapter and Runtime coordination behavior domains own; or how any
Repository Asset this domain reads is stored, versioned, or written, which the Repository behavior
domain owns. The resulting exclusions are stated in full in [Section 10](#10-non-goals).

---

## 3. Responsibilities

This Specification is answerable for:

- The resolution of a step's declared context need against the Repository Layer, and the selection
  of the minimum sufficient set of elements that need requires.
- The retention of a justification for every selected element.
- The composition of a selection with the Prompt assets a step declares.
- The exclusion of credentials and project-designated sensitive content from a composition, and the
  completion of that exclusion before composition completes.
- The disclosure of a composed result, and its measured size, for inspection before the result is
  passed outward.
- The isolation of a step's selection, justification, and composed result from every other step.
- The independence of this domain's behavior from any Runtime, Vendor, or Model.

This Specification is **not** answerable for:

- Whether, or why, a step requires Context at all — owned by the Workflow behavior domain.
- The presentation of a composed result to a person, or the collection of a human decision about
  it — owned by the Human Interaction behavior domain.
- The transmission of a composed result to a Runtime, or any disclosure obligation attached to
  crossing the External AI boundary — owned by the Adapter and Runtime coordination behavior
  domains and stated at the system level by `SP-SYS-023`.
- Which Runtime, Vendor, or Model a project has selected — owned by the Runtime coordination
  behavior domain.
- The storage, versioning, or durable custody of any Repository Asset this domain reads, or of any
  Workflow State recording that a selection occurred — owned by the Repository behavior domain.
- The content, wording, or authorship of a Prompt asset itself — owned by the project that declares
  it, as a Repository Asset under AEOS-PRD `C9`.
- Any structural decision, subsystem boundary, or dependency direction — owned by AEOS-ARCH and
  AEOS-BLUEPRINT.
- Any data structure, interface, storage mechanism, or algorithm realizing the behavior below —
  owned by Implementation Guides, which do not yet exist for this domain.

---

## 4. Inputs

The inputs below are the material every rule in [Section 6](#6-behavior) operates on. An input's
validity condition is part of the behavior this document specifies; an invalid input is a defined
condition, not an unhandled one.

| Input | Required properties | Valid when | Invalid when |
| :--- | :--- | :--- | :--- |
| Declared context need | States what a single, identified Workflow step requires | The step is identified and the requirement is stated for that step alone | No step is identified, or the requirement is stated for more than one step at once |
| Repository Asset candidate | Versioned, inspectable, resolvable to one unambiguous value where more than one candidate applies | Resolution is deterministic and the candidate is current relative to the repository | Two applicable candidates resolve ambiguously, or a candidate is stale relative to the repository |
| Environment finding recorded in the Repository Layer | Distinguished as observed fact or as inference | The distinction is stated | The distinction is absent, or an undeterminable state is presented as a finding |
| Prompt asset declared by a step | Versioned, parameterized, portable, identified singly | The declaration identifies exactly one Prompt asset per declared use | The declaration is ambiguous among more than one asset |
| Project-declared selection convention (optional) | A stated preference among candidates able to satisfy the same declared need | Declared as a Repository Asset under the extension point in [Section 8.1](#81-context-extension-points) | Inferred rather than declared |
| Project-declared sensitivity designation (optional) | A stated designation of content as sensitive | Declared as a Repository Asset under the extension point in [Section 8.1](#81-context-extension-points) | Inferred rather than declared |

---

## 5. Outputs

The outputs below describe externally observable, contractual behavior — what a counterparty
receives and can rely on — not the internal artifact or mechanism that produces it. An
implementation MAY realize an output through any internal form; this document constrains only what
the output must observably be.

| Output | Content | Produced when |
| :--- | :--- | :--- |
| Composed result | The step's selected elements, the retained justification for each, and the step's declared Prompt assets, combined into one composition; minimized to the step's declared need; carrying no credential and no project-designated sensitive content | Whenever a step's declared context need is resolved and composition completes |
| Composition size | The measured size of a composed result | Alongside every composed result |
| Unresolved-selection report | A statement that a declared need could not be resolved to exactly one candidate | Whenever `SP-CTX-006` applies |

---

## 6. Behavior

Each rule below is independently testable: a reviewer or an automated test can determine compliance
from the rule's text alone, without consulting this document's author, per AEOS-SPECSTD `NL3`. A
rule's own pass/fail condition is its acceptance criterion, satisfying `MD9`.

### 6.1 The Context Lifecycle

A step's Context passes through the five subsystems AEOS-BLUEPRINT Section 9.2 names, in the order
below. Sensitivity Exclusion is required to complete before Prompt Composition completes, which is
why the diagram shows exclusion nested inside composition rather than following it; the two are not
independent phases performed one after the other in full.

```mermaid
flowchart LR
    S["Context Selection"] --> J["Inclusion Justification"]
    J --> C["Prompt Composition begins"]
    C --> X["Sensitivity Exclusion completes"]
    X --> F["Prompt Composition completes"]
    F --> D["Composition Disclosure"]
    D --> W["Passed to the requesting Workflow step"]
```

Nothing in this lifecycle is retained once the composed result has been passed outward, and this is
the point at which the lifecycle terminates for the Context Layer: `SP-CTX-022` and `SP-CTX-023` fix
that no earlier point in this diagram may be treated as an ending, and that no state from it persists
afterward. [Section 7.1](#71-isolation-and-non-inheritance-constraints) states this boundary
normatively; [Section 7.5](#75-context-custody) explains it further.

### 6.2 Context Selection

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-001` | Before selection for a step completes, the Context Layer MUST resolve that step's declared context need against the Repository Layer. | `PR-PMT-002` · `PR-PMT-003` |
| `SP-CTX-002` | The Context Layer MUST select, for a step, only elements that step's declared context need requires, and MUST NOT select an element beyond that requirement. | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-003` | A selection performed for one step MUST be scoped to that step alone; the Context Layer MUST NOT carry an element selected for a prior step into a subsequent step's selection. | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-004` | Where a project has declared a selection convention under the extension point in [Section 8.1](#81-context-extension-points), the Context Layer MUST apply that convention when choosing among candidates able to satisfy the same declared need. | `PR-PMT-010` |
| `SP-CTX-005` | Where no project-declared selection convention applies to a choice among candidates, the Context Layer MUST select the candidate that satisfies the declared need with the least additional content. | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-006` | Where two or more candidate elements resolve to differing content for the same declared need, and neither `SP-CTX-004` nor `SP-CTX-005` yields exactly one candidate, the Context Layer MUST report the condition as unresolved and MUST NOT select any of the conflicting candidates. | `PR-SAF-002` |

### 6.3 Inclusion Justification

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-007` | For every element a selection includes under [Section 6.2](#62-context-selection), the Context Layer MUST retain a reason stating why that element was included. | `PR-PMT-004` |
| `SP-CTX-008` | An element MUST NOT be carried into a selection without a reason retained for it; the reason MUST be retained no later than the moment the element is selected. | `PR-PMT-004` |
| `SP-CTX-009` | A retained reason MUST be available on request without requiring the selection that produced it to be repeated. | `PR-PMT-004` · `PR-NFR-001` |

### 6.4 Prompt Composition

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-010` | The Context Layer MUST compose a step's selected elements with the Prompt assets that step declares, producing exactly one composed result for that step. | `PR-PMT-002` |
| `SP-CTX-011` | A composed result MUST carry, for each element it includes, the reason retained for that element under [Section 6.3](#63-inclusion-justification). | `PR-PMT-004` · `PR-PMT-005` |
| `SP-CTX-012` | Composition MUST admit a project-defined Prompt asset under the same composition behavior as any other Prompt asset. | `PR-PMT-007` |
| `SP-CTX-013` | Composition behavior for a step MUST NOT differ according to which Runtime, Vendor, or Model is selected for that step. | `PR-PMT-006` · `PR-RUN-005` |

### 6.5 Sensitivity Exclusion

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-014` | A credential MUST NOT enter a composed result. | `PR-PMT-008` · `PR-SAF-006` · `PR-RUN-014` |
| `SP-CTX-015` | Content a project has designated sensitive MUST NOT enter a composed result. | `PR-PMT-008` |
| `SP-CTX-016` | Exclusion required by `SP-CTX-014` and `SP-CTX-015` MUST complete before composition for the step completes. | `PR-PMT-008` · `PR-SAF-006` |
| `SP-CTX-017` | An element excluded under this section MUST be removed from the composed result; the Context Layer MUST NOT retain it in an annotated, marked, or otherwise recoverable form within that result. | `PR-SAF-006` · `PR-RUN-014` |

### 6.6 Composition Disclosure

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-018` | A composed result MUST be made available for inspection before the Context Layer passes it outward. | `PR-PMT-005` |
| `SP-CTX-019` | The Context Layer MUST make the measured size of a composed result available alongside that result. | `PR-PMT-009` |
| `SP-CTX-020` | The Context Layer MUST pass a composed result only to the step of the Workflow Layer that requested it. | `PR-SAF-007` |
| `SP-CTX-021` | The inspectability required by `SP-CTX-018` MUST hold before any subsequent crossing of the External AI boundary that carries the composed result, consistent with the disclosure obligation that crossing requires. | `PR-SAF-007` · `PR-SAF-008` · `PR-NFR-011` |

> **On human approval, non-normative.** This domain does not itself collect a human decision — under
> AEOS-ARCH Section 5.3, the Context Layer may address only the Repository Layer, and cannot reach
> the Human Layer directly. `SP-CTX-018` through `SP-CTX-021` state what this domain owes toward that
> decision: an inspectable, sized, correctly scoped composed result. The decision itself, and the
> disclosure obligation attached to crossing the External AI boundary, are owned by `SP-SYS-023` and
> by the future Human Interaction and Runtime coordination Specifications, per
> [Section 7.4](#74-declared-dependencies) and [Section 10](#10-non-goals).

---

## 7. Constraints

The invariants below MUST hold before, during, and after every behavior stated in
[Section 6](#6-behavior).

### 7.1 Isolation and Non-Inheritance Constraints

> **On the inheritance model, non-normative.** `SP-CTX-003` (Section 6.2) and `SP-CTX-024`, below,
> together fix that Context is never inherited across steps: no element, justification, or composed
> result produced for one step is available, in whole or in part, to any other step. This note
> states no obligation beyond what those two rules already require; it names the pattern explicitly
> so a reader need not infer it from separate rules.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-022` | The Context Layer MUST NOT hold a composed result once it has been passed outward under `SP-CTX-020`. | `PR-REP-015` |
| `SP-CTX-023` | The Context Layer MUST hold no durable state of any kind. | `PR-REP-015` |
| `SP-CTX-024` | A selection, a justification, or a composed result produced for one step MUST NOT be treated as available for reuse by a different step, regardless of similarity between the steps. | `PR-PMT-003` · `PR-NFR-004` |

### 7.2 Independence Constraints

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-025` | The Context Layer MUST hold no knowledge of any Runtime, Vendor, or Model. | `PR-RUN-005` · `PR-RUN-006` |
| `SP-CTX-026` | The Context Layer MUST address no counterparty other than the Repository Layer, and only for reading. | `PR-NFR-006` · `PR-NFR-007` |

### 7.3 Non-Functional Constraints

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-027` | Every rule in [Section 6](#6-behavior) MUST be verifiable in isolation, with the Repository Layer replaceable at the point the Context Layer reads it, for the purpose of that verification. | `PR-NFR-010` |
| `SP-CTX-028` | Behavior specified in [Section 6](#6-behavior) MUST be identical on every supported Platform. | `PR-PLT-003` · `PR-PLT-006` |
| `SP-CTX-029` | The Context Layer MUST make a selection, a justification, or a composed result explainable to the user on request. | `PR-NFR-001` |

### 7.4 Declared Dependencies

Per AEOS-SPECSTD `DP1`, this document declares its dependency on the following rules of AEOS-SPEC's
`SYS` behavior domain explicitly, by identifier, rather than restating them:

| Dependency | States | Why this domain depends on it |
| :--- | :--- | :--- |
| `SP-SYS-023` | AEOS MUST disclose what will cross the External AI boundary, and its expected cost, before that crossing occurs. | The composed result this domain produces is the material that disclosure describes; `SP-CTX-021` is written to make that disclosure possible, not to duplicate it. |
| `SP-SYS-039` | AEOS MUST make a composed Prompt available for human inspection before it is sent to a Runtime. | `SP-CTX-018` supplies the inspectability this rule requires, scoped to this domain's own boundary. |
| `SP-SYS-040` | AEOS MUST exclude a credential or user-designated sensitive content from a Prompt composition before that composition completes. | `SP-CTX-014` through `SP-CTX-017` are this domain's realization of this rule. |
| `SP-SYS-052` | The Context AEOS sends toward a Runtime for one step MUST be the smallest set that step requires, with the reason for each inclusion available on request. | `SP-CTX-002`, `SP-CTX-005`, `SP-CTX-007`, and `SP-CTX-009` are this domain's realization of this rule. |

Consistent with AEOS-SPECSTD `DP4`, each dependency above is stated behaviorally: what must already
hold, not which document, technology, or component makes it hold. Consistent with `DP2`, none of the
rules in [Section 6](#6-behavior) or elsewhere in this document presumes an unstated precondition
from `SP-SYS`; every precondition this domain relies on is named in this table.

### 7.5 Context Custody

**This section is non-normative.** It draws together, in one place, what
[Section 6](#6-behavior) and [Section 7.1](#71-isolation-and-non-inheritance-constraints) through
[Section 7.3](#73-non-functional-constraints) already require about who holds a step's selection,
its justification, and its composed result, and when that changes. It states no obligation beyond
those already-normative rules. The word *custody* is used rather than *ownership*, which elsewhere
in the AEOS document family — AEOS-DOCSTD, AEOS-ARCH, and AEOS-SPECSTD — names definitional or
documentation authority over a concept, a different relationship from the one described here;
*custody* instead follows the sense AEOS-ARCH and AEOS-BLUEPRINT already use for Repository Asset
custody, Workflow State custody, and Selection Custody.

While a step's selection, justification, or composed result exists within this domain, the Context
Layer is its sole holder, consistent with the single-step scope `SP-CTX-003` fixes and the
exactly-one-result rule `SP-CTX-010` fixes. Holding transfers to the requesting Workflow step at the
moment `SP-CTX-020` passes a composed result outward, and at no other moment; from that point the
Context Layer holds nothing, per `SP-CTX-022` and `SP-CTX-023`. At no point does more than one party
hold the same selection, justification, or composed result at once.

---

## 8. Extension Points

`SS-P-09` requires that extension of a Specification be additive; frozen behavior is never silently
altered. This document's ordinary extension mechanism is the addition of new `SP-CTX-<NNN>`
identifiers under AEOS-SPECSTD Section 18.1. Beyond that ordinary mechanism, this document declares
three extension points at which a project is intended to extend this domain's behavior without
altering what [Section 6](#6-behavior) and [Section 7](#7-constraints) already fix. These three
points are exactly the three AEOS-BLUEPRINT Section 9.7 names for the Context Blueprint; this
document introduces none beyond them, consistent with `BP-GOV-010`.

### 8.1 Context Extension Points

| ID | Extension point | What is added | Boundary — MUST NOT change |
| :--- | :--- | :--- | :--- |
| `EPC-1` | Prompt admission | A project-defined Prompt asset, parameterized by the project. | The inspectability `SP-CTX-018` requires. |
| `EPC-2` | Selection convention admission | A project-defined convention governing what a step's selection prefers among candidates. | The retained-reason obligation `SP-CTX-007` requires, and the non-overridability of the exclusion `SP-CTX-014` and `SP-CTX-015` require. |
| `EPC-3` | Sensitivity designation | A project's designation of content as sensitive. | The removal, rather than annotation, `SP-CTX-017` requires. |

### 8.2 Rules Governing These Extension Points

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-CTX-EXT-1` | An extension admitted at `EPC-1`, `EPC-2`, or `EPC-3` MUST be admitted as an additive declaration and MUST NOT require a change to [Section 6](#6-behavior) or [Section 7](#7-constraints) of this document. | `PR-NFR-007` |
| `SP-CTX-EXT-2` | An admission at `EPC-1` MUST NOT alter the inspectability `SP-CTX-018` requires. | `PR-PMT-005` |
| `SP-CTX-EXT-3` | An admission at `EPC-2` MUST NOT relax the retained-reason obligation `SP-CTX-007` requires. | `PR-PMT-004` |
| `SP-CTX-EXT-4` | An admission at `EPC-2` MUST NOT override or bypass the exclusion required by [Section 6.5](#65-sensitivity-exclusion). | `PR-PMT-008` · `PR-SAF-006` |
| `SP-CTX-EXT-5` | An admission at `EPC-3` MUST designate content for removal under [Section 6.5](#65-sensitivity-exclusion) and MUST NOT designate content for annotation in place of removal. | `PR-PMT-008` · `PR-SAF-006` |

---

## 9. Traceability

Every rule in [Section 6](#6-behavior), [Section 7](#7-constraints), and
[Section 8.2](#82-rules-governing-these-extension-points) traces to one or more `PR-` identifiers,
per AEOS-SPECSTD `TR1`. The complete rule-by-rule trace is
[Appendix A](#appendix-a--sp-ctx-rule-index).

### 9.1 Trace Density by `PR-` Prefix

| `PR-` prefix | Rules in this document that trace to it |
| :--- | :--- |
| `PR-PMT` | `SP-CTX-001` through `SP-CTX-005` · `SP-CTX-007` through `SP-CTX-016` · `SP-CTX-018` · `SP-CTX-019` · `SP-CTX-024` · `SP-CTX-EXT-2` through `SP-CTX-EXT-5` |
| `PR-RUN` | `SP-CTX-013` · `SP-CTX-014` · `SP-CTX-017` · `SP-CTX-025` |
| `PR-SAF` | `SP-CTX-006` · `SP-CTX-014` · `SP-CTX-016` · `SP-CTX-017` · `SP-CTX-020` · `SP-CTX-021` · `SP-CTX-EXT-4` · `SP-CTX-EXT-5` |
| `PR-REP` | `SP-CTX-022` · `SP-CTX-023` |
| `PR-PLT` | `SP-CTX-028` |
| `PR-NFR` | `SP-CTX-002` · `SP-CTX-003` · `SP-CTX-005` · `SP-CTX-009` · `SP-CTX-021` · `SP-CTX-024` · `SP-CTX-026` · `SP-CTX-027` · `SP-CTX-029` · `SP-CTX-EXT-1` |

No rule in this document lacks a trace, and no rule traces to a requirement this document does not
cite by identifier, consistent with AEOS-SPECSTD `TR1` and `TR4`.

### 9.2 Grounding in Architecture and Blueprint Identifiers

Per AEOS-BLUEPRINT `BP-GOV-008`, this document traces to at least one Blueprint identifier and to at
least one `PR-` identifier. This document is written against the complete `BP-CTX-001` through
`BP-CTX-012` arrangement AEOS-BLUEPRINT Section 9.8 states; every rule in
[Section 6](#6-behavior) and [Section 7](#7-constraints) realizes one or more of those Blueprint
rules as testable behavior, without restating them.

This document's Constraints are additionally grounded in the following AEOS-ARCH invariants, cited
for orientation and not restated: `AR-LAY-006` and `AR-LAY-008` ground
[Section 7.1](#71-isolation-and-non-inheritance-constraints) and
[Section 7.3](#73-non-functional-constraints); `AR-BND-005`, `AR-BND-009`, and `AR-PRN-005` ground
[Section 7.2](#72-independence-constraints); `AR-DEP-004` grounds `SP-CTX-026`; and `AR-BND-016`
grounds `SP-CTX-021`.

### 9.3 Downstream Traceability

Downstream documents — Implementation Guides, tests, issues, and pull requests — reference the
`SP-CTX-<NNN>` identifiers they realize or affect, consistent with AEOS-SPECSTD `TR5`.

---

## 10. Non-goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it.

| Non-goal | Why it is out of scope | Where it belongs |
| :--- | :--- | :--- |
| Whether, or why, a step requires Context at all | Owned by the Workflow behavior domain, reserved as `EPS-2` in AEOS-SPEC | Future Workflow behavior Specification |
| Presentation of a composed result to a person, and collection of the resulting decision | Owned by the Human Interaction behavior domain, reserved as `EPS-7` in AEOS-SPEC | Future Human Interaction behavior Specification |
| Transmission of a composed result to a Runtime, and the disclosure obligation attached to that crossing | Owned by the Adapter and Runtime coordination behavior domains, reserved as `EPS-5` and `EPS-4` in AEOS-SPEC, and stated at the system level by `SP-SYS-023` | Future Adapter and Runtime coordination behavior Specifications |
| Selection, availability, or degradation of a Runtime, Vendor, or Model | Owned by the Runtime coordination behavior domain | Future Runtime coordination behavior Specification |
| Storage, versioning, or durable custody of a Repository Asset this domain reads | Owned by the Repository behavior domain, reserved as `EPS-1` in AEOS-SPEC | Future Repository behavior Specification |
| The content, wording, or authorship of a Prompt asset | Owned by the project that declares it, as a Repository Asset under AEOS-PRD `C9` | The project's own Repository Assets |
| A selection mechanism, ranking method, storage technology, or indexing technique | Prohibited to the Specification layer by AEOS-SPECSTD `MN1`–`MN3` | Implementation Guides, AEOS-TECH |
| Interfaces, endpoints, request or response schemas, wire formats | Prohibited to the Specification layer by AEOS-SPECSTD `MN4` | Implementation Guides |
| Data structures, classes, modules, file layouts | Prohibited to the Specification layer by AEOS-SPECSTD `MN1` | Implementation Guides |
| Installation, deployment, or environment-preparation procedure | Prohibited to the Specification layer by AEOS-SPECSTD `MN5` | Runtime documents, Developer Guides |
| Rationale for why any rule above exists | Prohibited to the Specification layer by AEOS-SPECSTD `MN6` | AEOS-VISION, AEOS-PRD |
| Structural decisions, subsystem boundaries, or dependency direction | Owned by AEOS-ARCH and AEOS-BLUEPRINT | AEOS-ARCH, AEOS-BLUEPRINT |
| Test procedures, test plans, or test cases | Outside AEOS-SPECSTD's scope, per its Section 2.2 note on testing artifacts | A future Test Specification layer |

---

## 11. References

| Reference | Cited for |
| :--- | :--- |
| AEOS-VISION | Invariants V1, V2, V6, V7, and V9, underlying the independence, safety, and inspectability posture this document specifies behaviorally |
| AEOS-PRD Section 12.9 | The `C9` Prompt management capability this document's behavior realizes |
| AEOS-PRD Section 18 | Every `PR-` identifier this document traces to |
| AEOS-PRD Section 19 | The `PR-NFR` quality attributes cited in [Section 7.3](#73-non-functional-constraints) |
| AEOS-GLOSSARY | The definitions of every capitalized term used in this document, including *Context*, *Context Minimization*, *Context Router*, *Prompt*, and *Engineering Capability* |
| AEOS-DOCSTD | The form, structure, and lifecycle this document, like every AEOS document, follows |
| AEOS-ARCH Section 4.5 | The Context Layer's purpose, responsibilities, owned concepts, dependencies, and prohibited responsibilities this document specifies precisely |
| AEOS-ARCH Section 5.3 | The permitted-interaction rule confining the Context Layer to reading the Repository Layer, specified in [Section 7.2](#72-independence-constraints) |
| AEOS-ARCH Section 6.3 | The Minimum Sufficient Crossing principle this document's selection and minimization rules realize |
| AEOS-ARCH Section 7.1 | The External AI boundary's disclosure obligation this document's `SP-CTX-021` is written to serve |
| AEOS-ARCH Section 8 | The `AR-` invariants cited in [Section 9.2](#92-grounding-in-architecture-and-blueprint-identifiers) |
| AEOS-BLUEPRINT Section 9 | The Context Blueprint (`BP-CTX`), its five subsystems, and its twelve rules this document is written against |
| AEOS-BLUEPRINT Section 17 | The Blueprint/Specification boundary this document is written against |
| AEOS-SPECSTD | The Specification Standard this document is written entirely under |
| AEOS-SPEC Section 8.1 | The `EPS-3` extension point this document attaches at |
| AEOS-SPEC Sections 6.7 and 7.3 | `SP-SYS-039`, `SP-SYS-040`, and `SP-SYS-052`, declared as dependencies in [Section 7.4](#74-declared-dependencies) |
| AEOS-SPEC Section 6.4 | `SP-SYS-023`, declared as a dependency in [Section 7.4](#74-declared-dependencies) |

---

## 12. Document Governance

### 12.1 Status

This document is a Specification-layer document of the AEOS repository, attaching at `EPS-3` of
AEOS-SPEC, and is intended to be frozen as part of the AEOS 1.0 release alongside AEOS-VISION,
AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, and AEOS-SPEC.

### 12.2 Change Control

This document's change control follows AEOS-SPECSTD Section 18.1 without modification, applied to
the `CTX` behavior domain.

| Increment | When |
| :--- | :--- |
| **Patch** | Editorial correction with no change to a specified rule's meaning or trace. |
| **Minor** | Addition of a new `SP-CTX-<NNN>` identifier that does not alter what an existing identifier requires. |
| **Major** | Any change to what an existing `SP-CTX-<NNN>` identifier requires; retirement of an identifier; a change to the `CTX` area code, ownership, or declared behavior domain; addition or removal of a `EPC-` extension point; or any change that would invalidate a downstream Implementation Guide, Runtime document, or test written against the prior version. |

### 12.3 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and apply the AEOS-SPECSTD Section 19.2 freeze
checklist before recommending freeze, per AEOS-DOCSTD Section 12.3 and 12.4.

### 12.4 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-SPEC on a rule `EPS-3` fixes as a boundary | AEOS-SPEC governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-SPECSTD on form, identifier convention, traceability, or lifecycle | AEOS-SPECSTD governs. This document is corrected. |
| This document conflicts with AEOS-SPECSTD on the content of the `CTX` behavior domain | This document governs its own content, per AEOS-SPECSTD Section 20.7. |
| A future Specification attaching at a different `EPS-` extension point states a dependency on this document that this document does not confirm | The apparent need is reported against this document. It is not resolved by a contradictory rule in the attaching document. |

### 12.5 Traceability

Traceability for this document is stated in full in [Section 9](#9-traceability) and
[Appendix A](#appendix-a--sp-ctx-rule-index). Downstream documents reference this document's
`SP-CTX-<NNN>` identifiers under AEOS-SPECSTD `TR5`.

### 12.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Superseded | Initial Context Router Specification. Registers the `CTX` behavior domain and attaches at `EPS-3` of AEOS-SPEC. Establishes twenty-nine `SP-CTX` rules organized under the five AEOS-BLUEPRINT `BP-CTX` subsystems — Context Selection, Inclusion Justification, Prompt Composition, Sensitivity Exclusion, and Composition Disclosure — together with isolation, independence, and non-functional constraints and four declared dependencies on `SP-SYS` rules. States three Context extension points, `EPC-1` through `EPC-3`, matching AEOS-BLUEPRINT Section 9.7 exactly, with five governing rules. Traces every rule to one or more `PR-` identifiers and grounds the document as a whole in the complete `BP-CTX-001` through `BP-CTX-012` arrangement and in the relevant `AR-` invariants. Introduces no product requirement, no vision, no terminology, no architectural decision, and no Blueprint arrangement. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPEC. |
| 1.0.1 | Freeze candidate | Review-response clarification pass, addressing Architecture Review Board findings on version 1.0.0 regarding Context ownership, the Context inheritance model, and lifecycle termination. Added a non-normative note to [Section 7.1](#71-isolation-and-non-inheritance-constraints) naming the inheritance model `SP-CTX-003` and `SP-CTX-024` already establish, so it need not be inferred from separate rules. Added a new non-normative [Section 7.5](#75-context-custody), Context Custody, drawing together what `SP-CTX-003`, `SP-CTX-010`, `SP-CTX-020`, `SP-CTX-022`, and `SP-CTX-023` already establish about who holds a step's selection, justification, and composed result and when that changes — using the term *custody* rather than *ownership*, since the latter already names definitional or documentation authority elsewhere in the AEOS document family (AEOS-DOCSTD, AEOS-ARCH, AEOS-SPECSTD) and *custody* is the term AEOS-ARCH and AEOS-BLUEPRINT already use for this relationship (Repository Asset custody, Workflow State custody, Selection Custody). Extended [Section 6.1](#61-the-context-lifecycle) to state explicitly that the Context Layer's lifecycle terminates at the point `SP-CTX-022` and `SP-CTX-023` already fix, without adding a lifecycle phase. No `SP-CTX` identifier was added, removed, retired, or changed in meaning; no trace was added or altered; no extension point, constraint, or behavior rule was changed; no new obligation was introduced. |

---

## Appendix A — SP-CTX Rule Index

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Section 6](#6-behavior), [Section 7](#7-constraints), or
[Section 8.2](#82-rules-governing-these-extension-points).

| ID | Section | Short label | Traces to |
| :--- | :--- | :--- | :--- |
| `SP-CTX-001` | 6.2 | Resolve declared need before selection completes | `PR-PMT-002` · `PR-PMT-003` |
| `SP-CTX-002` | 6.2 | Select only what the need requires | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-003` | 6.2 | Selection scoped to one step, no carry-forward | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-004` | 6.2 | Apply a declared selection convention where one exists | `PR-PMT-010` |
| `SP-CTX-005` | 6.2 | Default preference is least additional content | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-006` | 6.2 | Unresolved conflict is reported, not guessed | `PR-SAF-002` |
| `SP-CTX-007` | 6.3 | Every selected element carries a retained reason | `PR-PMT-004` |
| `SP-CTX-008` | 6.3 | Reason retained no later than selection | `PR-PMT-004` |
| `SP-CTX-009` | 6.3 | Reason available on request without re-selection | `PR-PMT-004` · `PR-NFR-001` |
| `SP-CTX-010` | 6.4 | Compose selection with declared Prompt assets | `PR-PMT-002` |
| `SP-CTX-011` | 6.4 | Composed result carries each element's reason | `PR-PMT-004` · `PR-PMT-005` |
| `SP-CTX-012` | 6.4 | Project-defined Prompt admitted like any other | `PR-PMT-007` |
| `SP-CTX-013` | 6.4 | Composition independent of Runtime, Vendor, Model | `PR-PMT-006` · `PR-RUN-005` |
| `SP-CTX-014` | 6.5 | No credential enters a composed result | `PR-PMT-008` · `PR-SAF-006` · `PR-RUN-014` |
| `SP-CTX-015` | 6.5 | No designated-sensitive content enters a composed result | `PR-PMT-008` |
| `SP-CTX-016` | 6.5 | Exclusion completes before composition completes | `PR-PMT-008` · `PR-SAF-006` |
| `SP-CTX-017` | 6.5 | Excluded content is removed, not annotated | `PR-SAF-006` · `PR-RUN-014` |
| `SP-CTX-018` | 6.6 | Composed result inspectable before passed outward | `PR-PMT-005` |
| `SP-CTX-019` | 6.6 | Measured size available alongside the result | `PR-PMT-009` |
| `SP-CTX-020` | 6.6 | Passed only to the requesting Workflow step | `PR-SAF-007` |
| `SP-CTX-021` | 6.6 | Inspectability holds before External AI crossing | `PR-SAF-007` · `PR-SAF-008` · `PR-NFR-011` |
| `SP-CTX-022` | 7.1 | No retention once passed outward | `PR-REP-015` |
| `SP-CTX-023` | 7.1 | No durable state of any kind | `PR-REP-015` |
| `SP-CTX-024` | 7.1 | No reuse of one step's material by another | `PR-PMT-003` · `PR-NFR-004` |
| `SP-CTX-025` | 7.2 | No knowledge of Runtime, Vendor, or Model | `PR-RUN-005` · `PR-RUN-006` |
| `SP-CTX-026` | 7.2 | Addresses only the Repository Layer, for reading | `PR-NFR-006` · `PR-NFR-007` |
| `SP-CTX-027` | 7.3 | Verifiable in isolation | `PR-NFR-010` |
| `SP-CTX-028` | 7.3 | Identical on every supported Platform | `PR-PLT-003` · `PR-PLT-006` |
| `SP-CTX-029` | 7.3 | Explainable to the user on request | `PR-NFR-001` |
| `SP-CTX-EXT-1` | 8.2 | Extensions are additive, never require a rule change | `PR-NFR-007` |
| `SP-CTX-EXT-2` | 8.2 | Prompt admission preserves inspectability | `PR-PMT-005` |
| `SP-CTX-EXT-3` | 8.2 | Selection convention preserves reason retention | `PR-PMT-004` |
| `SP-CTX-EXT-4` | 8.2 | Selection convention cannot bypass exclusion | `PR-PMT-008` · `PR-SAF-006` |
| `SP-CTX-EXT-5` | 8.2 | Sensitivity designation removes, never annotates | `PR-PMT-008` · `PR-SAF-006` |

---

**End of Context Router Specification**

AEOS-SPEC-CTX · Version 1.0.1 · Traces to `PR-PMT` · `PR-RUN` · `PR-SAF` · `PR-REP` · `PR-PLT` ·
`PR-NFR`
