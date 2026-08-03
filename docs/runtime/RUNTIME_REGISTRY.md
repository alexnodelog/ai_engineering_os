# AI Engineering Operating System

## AEOS — Runtime Registry

*The permanent statement of what the Runtime Registry is responsible for, and of the observable
behavior a project may depend on it for.*

| Field | Value |
| :--- | :--- |
| **Document** | Runtime Registry |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-RUNTIME-REG |
| **Version** | 1.0.0 |
| **Status** | Freeze candidate |
| **Owner** | Product Owner, AEOS |
| **Author** | Runtime Governance Board, AEOS |
| **Audience** | Architects, adapter authors, runtime integrators, reviewers, maintainers, and AI runtimes consuming this repository |
| **Suggested path** | `docs/runtime/RUNTIME_REGISTRY.md` |
| **Companion documents** | `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD, structural convention only) |
| **Supersedes** | None |

> **Authority of this document.**
> This document is a **Runtime document**, in the sense AEOS-PRD Section 3.2 names the layer: it
> states what a user and an AI runtime can observe the Runtime Registry doing, and it governs the
> observable behavior and governance of the Runtime Registry — registration, discovery, compatibility
> checking, capability lookup, availability, deprecation, and retirement.
>
> It is not a Product document, not an Architecture document, not a Blueprint, not a behavioral
> Specification under AEOS-SPECSTD, and not an Implementation Guide. It defines no product
> requirement, no vision, no terminology, no architectural layer or dependency, no arrangement of
> subsystems, no database schema, no storage mechanism, no registry backend, no API endpoint, no
> runtime implementation, no plugin loading mechanism, and no technology choice. It redefines no
> ownership that AEOS-PRD, Architecture, Blueprint, or Specification documents already hold. Where
> this document and a document of higher authority both speak to a subject, the higher-authority
> document governs and any conflict here is a defect to be reported rather than acted upon.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Responsibilities](#3-responsibilities)
4. [Registry Ownership](#4-registry-ownership)
5. [Runtime Identity](#5-runtime-identity)
6. [Runtime Metadata](#6-runtime-metadata)
7. [Runtime Classification](#7-runtime-classification)
8. [Runtime Capability Declaration](#8-runtime-capability-declaration)
9. [Supported Runtime Categories](#9-supported-runtime-categories)
10. [Runtime Lifecycle](#10-runtime-lifecycle)
11. [Registry Invariants](#11-registry-invariants)
12. [Inputs](#12-inputs)
13. [Outputs](#13-outputs)
14. [Extension Points](#14-extension-points)
15. [Constraints](#15-constraints)
16. [Non-goals](#16-non-goals)
17. [Traceability](#17-traceability)
18. [References](#18-references)
19. [Document Governance](#19-document-governance)
20. [Appendix A — Registry Identifier Index](#appendix-a--registry-identifier-index)

---

## 1. Purpose

AEOS orchestrates external Runtimes without ever performing inference itself, and without ever
depending on any one of them. That independence is a property AEOS-VISION `V6` states as an invariant
of purpose — no vendor, runtime, model, platform, or distribution is privileged or required — and
AEOS-PRD Section 18.4 (`PR-RUN`) states as a set of product requirements. Independence of this kind
is only real if there is one place a user, a workflow, and an AI runtime can all consult to learn
which Runtimes a project currently knows about, what each is declared to offer, and whether each is
currently reachable. That place is the Runtime Registry.

This document states what the Runtime Registry is responsible for and what a reader may depend on it
to do, precisely enough to be checked. It does not state how the registry is arranged internally —
that is AEOS-BLUEPRINT's Runtime Blueprint (`BP-RUN`) — and it does not state how the registry is
realized in a process, a file, a database, or a network call — that is left to Implementation Guides
and to a future Runtime document addressing execution mechanics, as recorded in
[Section 16](#16-non-goals). What remains, once arrangement and mechanism are set aside, is the
observable behavior this document specifies: what becomes true of a project when a Runtime is
registered, discovered, matched against a requirement, found available or unavailable, deprecated, or
retired.

| In one sentence |
| :--- |
| The Runtime Registry is the single, observable record of which Runtimes a project knows about, what each declares itself capable of, and whether each is currently reachable — nothing about how that record is arranged, and nothing about how it is stored. |

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the observable behavior and governance of the Runtime Registry:

- the ownership boundary of registry content, and what the registry MUST NOT hold;
- the identity by which a registered Runtime is distinguished from every other;
- the metadata recorded about a registered Runtime and its reportability;
- the classification of a registered Runtime into a declared category;
- the declaration of the Engineering Capabilities a registered Runtime offers;
- the categories of Runtime the registry MUST admit;
- the lifecycle a registry entry moves through, from registration to retirement;
- the invariants that MUST hold of the registry at every point in that lifecycle;
- the inputs the registry accepts and the outputs it produces;
- the extension points at which the registry admits addition without modification;
- the boundary between this document and mechanism, technology, and implementation.

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
| Precise, testable behavioral rules stated under AEOS-SPECSTD's own change control | Specification documents |
| In what process, over what transport, and by what persistence mechanism the registry executes | Runtime documents addressing execution mechanics, and Implementation Guides |
| Mediation of a request or a result to one specific Runtime | The Adapter Blueprint (`BP-ADP`) and its Runtime documents |
| Whether a workflow step proceeds, and the approval that step requires | The Workflow Blueprint (`BP-WFL`) |
| What code realizes any of the above | The codebase and its tests |

A statement in this document that redefines Product ownership, an Architecture Layer, the Runtime
Blueprint's arrangement, or a Specification's behavioral content is a **defect in this document**. It
MUST be reported rather than acted upon.

### 2.3 Relationship to Governing and Companion Documents

Six documents inform this one. This document contradicts none of them and restates nothing they
already define; where it appears to, the entry below states the correct reading.

| Document | Governs | This document's obligation |
| :--- | :--- | :--- |
| **AEOS-VISION** | Purpose, philosophy, invariants `V1`–`V10`. | No statement here may make invariant `V6` — that no vendor, runtime, model, platform, or distribution is privileged or required — unenforceable. |
| **AEOS-PRD** | Product definition, the ten capabilities, and the numbered `PR-` requirements, including `PR-RUN` (AI Runtime Orchestration). | Every behavioral statement here traces to one or more `PR-` identifiers and MUST NOT weaken, reinterpret, or widen any of them. |
| **AEOS-GLOSSARY** | Terminology, including *Runtime*, *Runtime Adapter*, *Runtime State*, *Engineering Capability*, *Model*, and *Vendor*. | Every term is used exactly as AEOS-GLOSSARY defines it. A term this document needs that the Glossary does not yet define is proposed in [Section 2.6](#26-terminology-introduced-by-this-document), never defined locally. |
| **AEOS-DOCSTD** | The form, structure, language, and lifecycle of every AEOS document. | This document's structure, normative language, and review classification follow it, applying [Section 4.5](#45-unassigned-layer)'s treatment of Runtime documents, recorded in [Section 2.4](#24-position-in-the-documentation-hierarchy). |
| **AEOS-ARCH** | The eight-layer architecture, including the Runtime Layer and the Adapter Layer, and extension points `EP-3` and `EP-4`. | This document MUST NOT contradict a structural decision or boundary AEOS-ARCH states, and names no layer, dependency, or interaction it does not already define. |
| **AEOS-BLUEPRINT** | The buildable arrangement of the Runtime Blueprint (`BP-RUN`) and the Adapter Blueprint (`BP-ADP`), and the boundary between Blueprint and Runtime documents stated in its Section 18. | This document is written against that arrangement, cites the `BP-` items it depends on, and MUST NOT restate or alter the arrangement itself. |

AEOS-SPECSTD governs the Specification layer only, identified by documents carrying an `SP-`
identifier. This document is not such a document: it carries no `SP-` identifier and is not subject to
AEOS-SPECSTD's change control. Its structure borrows AEOS-SPECSTD's section skeleton — Purpose, Scope,
Responsibilities, Inputs, Outputs, behavioral content, Constraints, Extension Points, Traceability,
Non-goals, References — as a stylistic convention appropriate to a document that states testable,
observable behavior, because no Runtime Document Standard yet exists to prescribe one. Where this
document's structure diverges from AEOS-SPECSTD's, the divergence is a stylistic choice made under
this document's own authority, not a defect against AEOS-SPECSTD.

### 2.4 Position in the Documentation Hierarchy

AEOS-DOCSTD Section 4.5 records that AEOS-PRD names a Runtime layer among the layers it defers to, and
that AEOS-DOCSTD does not itself assign that layer a position in the documentation hierarchy —
assigning one is a hierarchy decision reserved to the owner. Until the owner decides:

> This document is written to the responsibility boundary AEOS-PRD Section 3.2 states for Runtime
> documents — "states what users observe, never what executes" — and complies with every rule in
> AEOS-DOCSTD that does not depend on hierarchy position. It claims no position above Architecture, no
> position above Blueprint, and no position within the Specification layer.

AEOS-BLUEPRINT Section 20.5 already states how a conflict between this layer and the Blueprint resolves:
the Blueprint governs the arrangement of the Runtime Blueprint and the Adapter Blueprint; this document
governs the observable behavior and governance of the Runtime Registry within that arrangement. Where
the two cannot both hold, the conflict is escalated to the owner rather than resolved locally.

### 2.5 Identifier Registration (Proposed)

AEOS-GLOSSARY Section 6.4 registers the identifier shape `<LAYER>-<AREA>-<NNN>` and the layer prefixes
`PR`, `AR`, `BP`, `SP`, and `WF`. It reserves the introduction of a new layer prefix to Glossary
governance, under rule `I4`.

No layer prefix is yet registered for Runtime documents. This document therefore proposes, under its
own authority over the observable behavior it states — consistent with the precedent AEOS-ARCH Section
2.6 set for terms an existing document needed before the Glossary had registered them — the following
identifier shape for its own rules:

```text
        RT-REG-<NNN>

        RT      proposed layer prefix for Runtime documents
        REG     registered area code: Runtime Registry behavior
        NNN     three digits, zero-padded, allocated sequentially from 001
```

Until the owner acts on this proposal under AEOS-GLOSSARY `I4`, this document is the sole authority
for the `RT` prefix and the `REG` area code, and every rule in [Section 11](#11-registry-invariants)
carries an `RT-REG-<NNN>` identifier on that basis. Identifiers allocated here follow AEOS-GLOSSARY
`I1`–`I3` regardless of registration state: they are immutable, a retired rule is marked retired in
place, and the `REG` area code is not reused for a different meaning.

### 2.6 Terminology Introduced by This Document

This document uses the term **Runtime Registry**, which AEOS-GLOSSARY does not yet define. Consistent
with AEOS-GLOSSARY rule `W4`, its addition is proposed here:

| Proposed term | Proposed short definition |
| :--- | :--- |
| Runtime Registry | The observable record, within a project, of which Runtimes are known to it, their declared category and Engineering Capabilities, and their observed availability. |

Until the owner acts on this proposal, the definition of record is [Section 4](#4-registry-ownership)
through [Section 10](#10-runtime-lifecycle) of this document. This document introduces no other term;
every other word it uses that names an AEOS concept — *Runtime*, *Runtime Adapter*, *Runtime State*,
*Engineering Capability*, *Model*, *Vendor*, *Repository Asset*, *Platform* — is used exactly as
AEOS-GLOSSARY defines it.

---

## 3. Responsibilities

### 3.1 What the Runtime Registry Is Answerable For

The Runtime Registry is answerable for: the set of Runtimes known to a project; the identity that
distinguishes one registered Runtime from another; the declared category and declared Engineering
Capabilities of each; the compatibility each declares with a version range, a Platform, and an
adapter; the observed availability of each; the lifecycle state — registered, available, unavailable,
deprecated, or retired — that each currently occupies; and the disclosure of all of the foregoing to a
user or an AI runtime on request.

### 3.2 What the Runtime Registry Is Not Answerable For

The Runtime Registry is not answerable for: selecting which registered Runtime a workflow step uses,
which belongs to the Workflow Blueprint and the user's own selection; translating a request into a
specific Runtime's terms, mediating its result, or holding its credentials, all of which belong to the
Adapter Blueprint; performing inference, which no part of AEOS ever performs; deciding whether a
workflow step proceeds, which belongs to the Workflow Blueprint's Approval Gates; or the internal
subsystem arrangement that realizes registry behavior, which belongs to the Runtime Blueprint
(`BP-RUN`).

### 3.3 The Responsibility Test

Before a statement is added to this document, its author applies the test AEOS-DOCSTD Section 5.3
states, adapted to a Runtime document's boundary:

> **Is this statement a testable fact about what a user or an AI runtime can observe the Runtime
> Registry doing, traceable to a `PR-`, `AR-`, or `BP-` identifier, and free of mechanism, technology,
> and interface detail?**
> If the answer is no on any count, the statement does not belong in this document — however true,
> useful, or well written it is. A statement that answers *why* belongs above; a statement that
> answers *how it is arranged* belongs to AEOS-BLUEPRINT; a statement that answers *how it is built*
> belongs below, to a future Implementation Guide.

---

## 4. Registry Ownership

The Runtime Registry is the single record, within a project, of which Runtimes are known to it. No
other record — a configuration file a workflow reads independently, a note held only in a chat
session, a value cached only in memory — competes with it for that role. Ownership of registry content
follows the same custody rule AEOS-ARCH assigns to durable product meaning generally: it exists as
Repository Assets, held in the Repository Layer, and nowhere else.

The registry holds no engineering policy of its own. It does not decide whether a workflow step
proceeds, does not hold an Approval Gate, and does not select Context. It holds no credential and no
other Runtime State: what a Runtime requires to be invoked is the Adapter Blueprint's custody, never
the registry's. Registering a Runtime confers no privilege on it — presence in the registry states
that a Runtime is known, never that it is preferred, recommended, or required.

The normative rules of registry ownership are stated in [Section 11.1](#111-ownership-invariants).

---

## 5. Runtime Identity

A registered Runtime is identified by an identity distinct from any Vendor, Model, or credential.
Identity is what lets a workflow, a report, or a user refer to "this Runtime" stably across a change
of the Model the Runtime uses to perform inference, consistent with `PR-RUN-006`'s requirement that
AEOS remain independent of any specific model, model family, or model version.

Identity is not itself a credential, is never derived from one, and is never sufficient on its own to
invoke the Runtime it names — invocation is mediated entirely by the Adapter Blueprint. Two distinct
registered Runtimes never share one identity, and one registered Runtime's identity does not change
when its declared capabilities, its compatibility range, or its lifecycle state changes.

The normative rules of Runtime identity are stated in [Section 11.2](#112-identity-invariants).

---

## 6. Runtime Metadata

Each registry entry carries metadata describing the Runtime it identifies: its declared category
([Section 7](#7-runtime-classification)), its declared Engineering Capabilities
([Section 8](#8-runtime-capability-declaration)), the identity of the adapter that mediates it, its
declared version and Platform compatibility, and its currently observed availability
([Section 10.7](#107-runtime-availability)).

Metadata is descriptive of what is known and observed; it is never a credential, a secret, or any
other form of Runtime State, consistent with `PR-RUN-014` and `PR-SAF-006`. Metadata is reportable to
a user or an AI runtime on request, and reporting it never alters registry state — inspection and
disclosure are read operations, never write operations, consistent with the inspectability AEOS-ARCH
`AR-PRN-007` requires of every layer.

The normative rules of Runtime metadata are stated in [Section 11.3](#113-metadata-invariants).

---

## 7. Runtime Classification

Every registered Runtime is classified into a declared category, so that a user, a report, and a
capability lookup can distinguish kinds of Runtime without inspecting adapter internals. Classification
is descriptive: it confers no privilege, no priority, and no default preference on the Runtime it
describes, consistent with `PR-RUN-003`'s requirement that AEOS depend on no single vendor.

A system that performs no inference is a Tool, in the sense AEOS-GLOSSARY fixes, never a Runtime, and
is never classified or registered here. The categories admitted are stated in
[Section 9](#9-supported-runtime-categories).

The normative rules of Runtime classification are stated in
[Section 11.4](#114-classification-invariants).

---

## 8. Runtime Capability Declaration

A registered Runtime declares the Engineering Capabilities it offers, expressed in the neutral
vocabulary the Runtime Blueprint's Capability Vocabulary subsystem holds — never in a Vendor's own
terms. This is what lets a workflow step state a requirement once and have it compared against every
registered Runtime's offer in the same terms, satisfying `PR-RUN-007`.

A declared capability is attributable to exactly the registered Runtime that declares it. Declaring a
new Engineering Capability term, or a new Runtime's declaration of an existing one, never requires
modifying an entry already recorded for a different Runtime.

The normative rules of capability declaration are stated in
[Section 11.5](#115-capability-declaration-invariants).

---

## 9. Supported Runtime Categories

The Runtime Registry admits every category of Runtime `PR-RUN-016` names — commercial services,
open-source models, interoperability standards, and user-supplied extensions — and admits a category
that does not yet exist without a change to the invariants this document states. Admission of a new
category never requires modifying an entry already registered under a different one.

The absence of a category from a given project's registry reduces the options available to that
project only. It never disables a capability that does not depend on the absent category, consistent
with `AR-PRN-008`'s requirement that a failing or absent layer reduce options rather than corrupt
state.

The normative rules of supported categories are stated in
[Section 11.6](#116-supported-category-invariants).

---

## 10. Runtime Lifecycle

A registry entry occupies exactly one of the following observable states at a time. The states below
describe what can be observed of an entry; they name no process, no thread, and no storage mechanism
by which a transition is carried out.

| State | What is observably true |
| :--- | :--- |
| **Registered** | The entry exists, its adapter and declared capabilities are recorded, and it has not yet been observed reachable. |
| **Available** | The entry is registered and has been observed reachable. |
| **Unavailable** | The entry is registered but is not currently observed reachable. |
| **Deprecated** | The entry remains usable by projects already depending on it, but is marked for eventual retirement and disclosed as such before selection. |
| **Retired** | The entry is no longer selectable for any workflow, and is marked retired in place rather than removed. |

### 10.1 Registration

An entry becomes registered only once the Engineering Capabilities of its mediating adapter have been
declared. Registration is admitted without any change to the registry itself, consistent with
`BP-RUN-012`, and without any change to an existing project. Registration is disclosed to the user
before the newly registered Runtime becomes selectable for a workflow step.

### 10.2 Discovery

The registry makes the complete set of registered entries enumerable on request. Discovery is a read
operation: it never alters registry state, and its result distinguishes an available entry from an
unavailable one.

### 10.3 Version Compatibility

An entry records the version compatibility its adapter declares. Before a workflow step that depends
on a registered Runtime begins, the registry reports whether that Runtime's declared version
compatibility is satisfied, consistent with `PR-RUN-008`'s requirement that a workflow produce
comparable outcomes regardless of which Runtime performed the work.

### 10.4 Platform Compatibility

An entry records every Platform on which its Runtime is declared available. The registry's own
behavior — enumeration, lookup, and reporting — is identical on every supported Platform, consistent
with `AR-PRN-010`. Where a registered Runtime itself is limited to fewer Platforms than AEOS supports,
that limitation is disclosed rather than silently absorbed.

### 10.5 Adapter Compatibility

An entry is associated with exactly one adapter, consistent with `BP-ADP-002`'s requirement that an
adapter mediate exactly one Runtime. The registry refuses registration of a Runtime that lacks a
declared, compatible adapter, and records the adapter's identity — never the adapter's internal
translation logic, which remains the Adapter Blueprint's exclusive knowledge under `AR-BND-005`.

### 10.6 Capability Lookup

Given an Engineering Capability a workflow step requires, the registry returns the set of registered
entries that declare it. Lookup occurs, and its result is available, before the dependent step begins,
consistent with `PR-WFL-016`. A lookup result distinguishes a declared capability from an entry's
currently observed availability — declaring a capability and being reachable to perform it are
different facts, and the registry never conflates them.

### 10.7 Runtime Availability

Availability is recorded as observed, never as inferred or assumed, consistent with `BP-RUN-011` and
`PR-SAF-011`. When a registered Runtime is unreachable, the registry reduces the options available to
a workflow rather than corrupting registry state, and the unavailability of one entry never affects the
recorded availability of another. The registry itself remains queryable — enumerable, searchable by
capability, and reportable — even when no registered Runtime is reachable.

### 10.8 Deprecation

The registry supports marking a registered entry deprecated without removing it. Deprecation is
disclosed to the user before a deprecated Runtime is selected for a new workflow, and it records the
reason. Deprecation never removes an existing project's ability to continue using the deprecated
Runtime until that entry is retired.

### 10.9 Retirement

A retired entry is marked retired in place, retaining its identity and the reason for retirement,
rather than being removed. A retired Runtime's identity is never reused for a different Runtime.
Retirement never alters the durable record of a workflow step that was previously completed using that
Runtime, and the registry reports, on request, why a retired entry is no longer selectable.

---

## 11. Registry Invariants

Each rule below carries an `RT-REG-<NNN>` identifier under the proposal in
[Section 2.5](#25-identifier-registration-proposed), and traces to one or more `PR-`, `AR-`, or `BP-`
identifiers. This section is the complete, consolidated statement of every normative rule in this
document; [Sections 4](#4-registry-ownership) through [10](#10-runtime-lifecycle) describe the same
behavior narratively and carry no separate obligation beyond what is stated here.

### 11.1 Ownership Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-001` | The Runtime Registry MUST be the single record, within a project, of which Runtimes are known to it. | `PR-RUN-003` · `AR-PRN-003` |
| `RT-REG-002` | Registry content MUST be held within the Repository Layer as Repository Assets. | `PR-REP-001` · `PR-REP-002` · `AR-BND-007` |
| `RT-REG-003` | The Runtime Registry MUST NOT hold a credential or any other Runtime State. | `PR-RUN-014` · `PR-SAF-006` · `PR-REP-013` · `PR-REP-015` |
| `RT-REG-004` | An entry in the Runtime Registry MUST confer no privilege on the Runtime it describes. | `PR-RUN-003` · `AR-BND-011` |
| `RT-REG-005` | The Runtime Registry MUST hold no engineering policy: no Rule, no Approval Gate, and no context-selection logic. | `AR-PRN-001` |

### 11.2 Identity Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-006` | Each registered Runtime MUST carry an identity distinct from any Vendor, Model, or credential. | `PR-RUN-003` · `PR-RUN-006` · `PR-RUN-014` |
| `RT-REG-007` | A Runtime's identity MUST remain stable across a change of the Model it uses. | `PR-RUN-006` |
| `RT-REG-008` | Two distinct registered Runtimes MUST NOT share one identity. | `PR-NFR-002` · `PR-NFR-009` |
| `RT-REG-009` | An identity MUST NOT itself be, or be derived from, a credential. | `PR-RUN-014` · `PR-SAF-006` |

### 11.3 Metadata Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-010` | An entry MUST record the declared category, the declared Engineering Capabilities, the identity of the mediating adapter, and the observed availability of the Runtime it describes. | `PR-NFR-001` · `PR-PRJ-007` |
| `RT-REG-011` | Registry metadata MUST NOT include a credential, a secret, or any other Runtime State. | `PR-RUN-014` · `PR-SAF-006` · `PR-REP-013` |
| `RT-REG-012` | Registry metadata MUST be reportable to a user or an AI runtime on request. | `PR-NFR-001` · `PR-PRJ-007` |
| `RT-REG-013` | Reporting registry metadata MUST NOT alter registry state. | `AR-PRN-007` |
| `RT-REG-014` | Metadata recorded for one entry MUST NOT be required to interpret metadata recorded for another. | `PR-NFR-006` · `AR-PRN-001` |

### 11.4 Classification Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-015` | Each registered Runtime MUST be classified into a declared category. | `PR-RUN-016` |
| `RT-REG-016` | Classification MUST be descriptive and MUST confer no privilege, priority, or default preference. | `PR-RUN-003` · `AR-BND-011` |
| `RT-REG-017` | The registry MUST admit a category not previously classified without modifying an existing entry. | `PR-RUN-016` · `AR-PRN-009` |
| `RT-REG-018` | A system that performs no inference MUST NOT be classified or registered as a Runtime. | `PR-RUN-001` · `PR-RUN-002` |

### 11.5 Capability Declaration Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-019` | A declared capability MUST be expressed in the neutral Engineering Capability vocabulary. | `PR-RUN-007` · `BP-RUN-003` |
| `RT-REG-020` | A declared capability MUST NOT be expressed in a Vendor's terms. | `PR-RUN-002` · `BP-RUN-001` |
| `RT-REG-021` | A declared capability MUST be attributable to the specific registered Runtime that declares it. | `PR-RUN-007` · `PR-NFR-001` |
| `RT-REG-022` | The registry MUST admit declaration of a new Engineering Capability term without modifying an existing entry. | `PR-NFR-007` · `AR-EXT-002` |

### 11.6 Supported Category Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-023` | The registry MUST admit commercial-service, open-source, interoperability-standard, and user-supplied Runtime categories, and any future category, without a change to the invariants of this document. | `PR-RUN-016` · `AR-EXT-004` |
| `RT-REG-024` | Admission of a new category MUST NOT require a change to an already-registered entry. | `PR-RUN-012` · `PR-RUN-016` |
| `RT-REG-025` | The absence of a category from a project's registry MUST reduce available options only, and MUST NOT disable a capability that does not depend on that category. | `PR-RUN-010` · `AR-PRN-008` |

### 11.7 Registration Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-026` | An entry MUST become registered only once the Engineering Capabilities of its mediating adapter have been declared. | `BP-RUN-012` · `PR-RUN-012` |
| `RT-REG-027` | Registration MUST be admitted without any change to the Runtime Registry itself. | `BP-RUN-012` · `AR-EXT-007` |
| `RT-REG-028` | Registration MUST be disclosed to the user before the registered Runtime becomes selectable for a workflow. | `PR-SAF-008` · `PR-NFR-001` |

### 11.8 Discovery Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-029` | The registry MUST make the complete set of registered Runtimes enumerable on request. | `PR-PRJ-007` · `PR-NFR-001` |
| `RT-REG-030` | Discovery MUST NOT alter registry state. | `AR-PRN-007` |
| `RT-REG-031` | A discovery result MUST distinguish an available entry from an unavailable one. | `PR-RUN-010` · `BP-RUN-011` |

### 11.9 Version Compatibility Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-032` | An entry MUST record the version compatibility its adapter declares. | `PR-NFR-002` · `PR-RUN-008` |
| `RT-REG-033` | The registry MUST report a version incompatibility before a dependent workflow step begins. | `PR-RUN-007` · `PR-WFL-016` |
| `RT-REG-034` | The registry MUST NOT treat a registered Runtime as compatible outside the version range its adapter declares. | `PR-NFR-002` · `PR-RUN-008` |

### 11.10 Platform Compatibility Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-035` | An entry MUST record every Platform on which its Runtime is declared available. | `PR-PLT-001` · `PR-PLT-006` |
| `RT-REG-036` | The registry's own behavior MUST be identical across every supported Platform. | `PR-PLT-003` · `AR-PRN-010` |
| `RT-REG-037` | A Platform limitation on a registered Runtime MUST be disclosed rather than silently absorbed as a capability gap. | `PR-NFR-001` · `PR-PLT-002` |

### 11.11 Adapter Compatibility Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-038` | An entry MUST be associated with exactly one adapter. | `BP-ADP-002` · `PR-RUN-003` |
| `RT-REG-039` | The registry MUST refuse registration of a Runtime that lacks a declared, compatible adapter. | `BP-RUN-012` · `BP-ADP-002` |
| `RT-REG-040` | The registry MUST record the mediating adapter's identity and MUST NOT record that adapter's internal translation logic. | `BP-ADP-001` · `AR-BND-005` |

### 11.12 Capability Lookup Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-041` | Given a required Engineering Capability, the registry MUST return the set of registered Runtimes that declare it. | `PR-RUN-007` · `BP-RUN-004` |
| `RT-REG-042` | Capability lookup MUST occur, and its result MUST be available, before the dependent workflow step begins. | `PR-WFL-016` · `BP-RUN-004` |
| `RT-REG-043` | A lookup result MUST distinguish a declared capability from an entry's observed availability. | `PR-RUN-010` · `BP-RUN-011` |

### 11.13 Availability Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-044` | Availability MUST be recorded as observed, never as inferred or assumed. | `BP-RUN-011` · `PR-SAF-011` |
| `RT-REG-045` | When a registered Runtime is unreachable, the registry MUST reduce available options rather than corrupt registry state. | `PR-RUN-010` · `BP-RUN-008` · `BP-RUN-009` |
| `RT-REG-046` | The unavailability of one registered Runtime MUST NOT affect the recorded availability of another. | `AR-PRN-008` · `PR-RUN-010` |
| `RT-REG-047` | The registry MUST remain queryable when no registered Runtime is reachable. | `PR-RUN-010` · `BP-RUN-008` |

### 11.14 Deprecation Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-048` | The registry MUST support marking a registered entry deprecated without removing it. | `PR-NFR-008` · `AR-PRN-008` |
| `RT-REG-049` | Deprecation MUST be disclosed before a deprecated Runtime is selected for a new workflow. | `PR-SAF-008` · `PR-NFR-001` |
| `RT-REG-050` | Deprecation MUST NOT remove an existing project's ability to continue using the deprecated Runtime until it is retired. | `PR-NFR-008` · `PR-RUN-010` |
| `RT-REG-051` | A deprecation MUST record its reason. | `PR-NFR-001` |

### 11.15 Retirement Invariants

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `RT-REG-052` | A retired entry MUST be marked retired in place, retaining its identity and the reason for retirement. | `PR-NFR-006` · `AR-PRN-007` |
| `RT-REG-053` | A retired Runtime's identity MUST NOT be reused for a different Runtime. | `PR-NFR-002` · `PR-NFR-009` |
| `RT-REG-054` | Retirement MUST NOT alter the durable record of a workflow step previously completed using that Runtime. | `PR-WFL-011` · `AR-PRN-003` |
| `RT-REG-055` | The registry MUST report, on request, why a retired Runtime is no longer selectable. | `PR-NFR-001` |

---

## 12. Inputs

| Input | Description | Source |
| :--- | :--- | :--- |
| Adapter capability declaration | The Engineering Capabilities an adapter declares its mediated Runtime offers, and the version and Platform range it declares compatible. | Adapter Blueprint (`BP-ADP`) |
| Workflow step capability requirement | The Engineering Capability a workflow step declares it requires. | Workflow Blueprint (`BP-WFL`), via the Runtime Blueprint |
| Observed reachability signal | Whether a registered Runtime is currently reachable, as observed rather than assumed. | Runtime Blueprint's Availability Registration subsystem (`BP-RUN`) |
| Registration, deprecation, or retirement instruction | An explicit instruction to register, deprecate, or retire an entry. | The Human Layer, via the Workflow Layer's Approval Gate applicable to the action |

### 12.1 Input Validity

An input is invalid, and MUST be refused with the reason disclosed, where it would register a system
that performs no inference, where it declares a capability outside the neutral vocabulary, where it
lacks a compatible declared adapter, or where it would reuse a retired identity for a different
Runtime.

---

## 13. Outputs

| Output | Description | Consumer |
| :--- | :--- | :--- |
| Registry entry | A durable Repository Asset recording an identity, a declared category, declared capabilities, an adapter reference, declared compatibility, and a lifecycle state. | Repository Layer |
| Enumerable Runtime list | The complete, current set of registered entries, produced by discovery. | Workflow Layer, Human Layer, on request |
| Capability lookup result | The set of registered Runtimes declaring a required Engineering Capability, with declared and observed status distinguished. | Workflow Layer, before a dependent step begins |
| Availability status | The currently observed reachability of a registered entry. | Workflow Layer, Human Layer |
| Lifecycle disclosure | The content disclosed on registration, deprecation, or retirement, including the reason recorded for deprecation or retirement. | Human Layer, via the Workflow Layer's disclosure obligation |

---

## 14. Extension Points

| Extension point | What is added | What MUST NOT change | Architectural attachment |
| :--- | :--- | :--- | :--- |
| New Runtime category admission | A category of Runtime `PR-RUN-016` anticipates but the registry has not yet classified. | That classification remains descriptive and confers no privilege (`RT-REG-016`). | `EP-3` |
| New adapter registration | A newly admitted adapter, and the Runtime entry it mediates. | That registration confers no privilege and that its absence disables no other layer (`RT-REG-004`, `RT-REG-027`). | `EP-3` |
| New Engineering Capability declaration | A capability term expressible by both a workflow step and an adapter. | That the vocabulary names no Runtime, Vendor, or Model (`RT-REG-019`, `RT-REG-020`). | `EP-4` |

Extension at any of these points follows AEOS-ARCH `AR-EXT-001` through `AR-EXT-008`: it occurs only
through a named extension point, is declared, versioned, and inspectable, and never weakens a gate,
introduces a second path to inference, or requires modifying AEOS itself.

---

## 15. Constraints

This document states observable behavior only. The categories below are the boundary this document
does not cross, each paired with the statement that remains within scope, so the boundary is checkable
rather than a matter of taste.

| # | This document MUST NOT define | The statement that remains in scope |
| :--- | :--- | :--- |
| `RT-CON-1` | **Database schemas.** No table, no field type, no index, no query language. | That an entry records certain information, stated in [Section 6](#6-runtime-metadata), independent of how that information is stored. |
| `RT-CON-2` | **Storage implementation.** No file format, no database engine, no cache layer. | The requirement that registry content be a Repository Asset (`RT-REG-002`), leaving the mechanism to a future Runtime document and to Implementation Guides. |
| `RT-CON-3` | **Registry backend.** No process, no service, no client-server arrangement. | The behavior a backend of any shape must produce, stated throughout [Sections 4](#4-registry-ownership)–[10](#10-runtime-lifecycle). |
| `RT-CON-4` | **API endpoints.** No route, no method signature, no request or response schema, no wire format. | What a defined operation — registration, discovery, lookup — must accept and produce, stated behaviorally. |
| `RT-CON-5` | **Runtime implementation.** No code, no class or module design, no algorithm. | The observable outcome an implementation of the registry must produce. |
| `RT-CON-6` | **Plugin loading mechanisms.** No discovery protocol, no manifest format, no dynamic loading procedure. | The requirement that a new adapter be admitted without modifying the registry (`RT-REG-027`), leaving the loading mechanism to Implementation Guides. |
| `RT-CON-7` | **Technology choices.** No vendor, language, library, framework, or product name, and no reference satisfiable by only one entry in AEOS-TECH. | A behavioral dependency any technology satisfying it may fulfill. |

> **The single-satisfiability test.** Where an author is uncertain whether a statement in this document
> has crossed into a prohibited category, the test is: *can this requirement be satisfied in more than
> one technically reasonable way?* A requirement satisfiable by exactly one mechanism, one technology,
> or one literal interface belongs to a lower layer, stated too early. This is a non-normative aid, not
> an additional rule.

---

## 16. Non-goals

The following are behavior a reader might reasonably expect this document to cover. It deliberately
does not, for the reason stated.

| Non-goal | Reason |
| :--- | :--- |
| Selecting which registered Runtime a workflow step uses | Selection belongs to the user and to the Workflow Blueprint's Selection Custody; the registry only reports what is registered and available. |
| Translating a request into a specific Runtime's terms | Translation is the Adapter Blueprint's exclusive responsibility, never the registry's. |
| Registering a specific Model, independent of the Runtime that exposes it | A Model is reached only through the Runtime that exposes it, consistent with AEOS-ARCH Section 11.3; Model-level registration, if ever needed, belongs to a prospective Model Registry document and is out of scope here. |
| Sequencing coordinated work across more than one Runtime within a workflow | Sequencing belongs to the Workflow Layer's agentic orchestration and to a prospective Runtime Flow document; this document states only what the registry reports about each Runtime individually. |
| The storage, transport, process, or concurrency mechanism by which registry entries persist | These are Runtime-execution and Implementation concerns, excluded under [Section 15](#15-constraints) and left to a future Runtime document addressing execution mechanics. |
| Any interface, wire format, or API surface for querying the registry | Interface detail belongs to a Specification document or an Implementation Guide, should one later be written for this behavior domain. |
| Credential issuance, storage, or rotation | Credentials are never part of the registry under `RT-REG-003`; their custody belongs entirely to the Adapter Blueprint. |

---

## 17. Traceability

### 17.1 The Trace Requirement

Every `RT-REG-<NNN>` rule in [Section 11](#11-registry-invariants) traces to one or more `PR-`, `AR-`,
or `BP-` identifiers, stated beside the rule. A rule with no trace would describe behavior the product
did not ask for, and is a defect to be corrected before this document is treated as complete.

### 17.2 Trace Summary by Topic

| Topic | `PR-` families traced | `AR-` identifiers traced | `BP-` identifiers traced |
| :--- | :--- | :--- | :--- |
| Ownership | `PR-RUN` · `PR-REP` · `PR-SAF` | `AR-PRN-001` · `AR-PRN-003` · `AR-BND-007` · `AR-BND-011` | — |
| Identity | `PR-RUN` · `PR-NFR` · `PR-SAF` | — | — |
| Metadata | `PR-NFR` · `PR-PRJ` · `PR-RUN` · `PR-SAF` · `PR-REP` | `AR-PRN-001` · `AR-PRN-007` | — |
| Classification | `PR-RUN` | `AR-BND-011` · `AR-PRN-009` | — |
| Capability declaration | `PR-RUN` · `PR-NFR` | `AR-EXT-002` | `BP-RUN-001` · `BP-RUN-003` |
| Supported categories | `PR-RUN` | `AR-EXT-004` · `AR-PRN-008` | — |
| Registration | `PR-RUN` · `PR-SAF` · `PR-NFR` | `AR-EXT-007` | `BP-RUN-012` |
| Discovery | `PR-PRJ` · `PR-NFR` · `PR-RUN` | `AR-PRN-007` | `BP-RUN-011` |
| Version compatibility | `PR-NFR` · `PR-RUN` · `PR-WFL` | — | — |
| Platform compatibility | `PR-PLT` · `PR-NFR` | `AR-PRN-010` | — |
| Adapter compatibility | `PR-RUN` | `AR-BND-005` | `BP-RUN-012` · `BP-ADP-001` · `BP-ADP-002` |
| Capability lookup | `PR-RUN` · `PR-WFL` | — | `BP-RUN-004` · `BP-RUN-011` |
| Availability | `PR-RUN` · `PR-SAF` | `AR-PRN-008` | `BP-RUN-008` · `BP-RUN-009` · `BP-RUN-011` |
| Deprecation | `PR-NFR` · `PR-SAF` | `AR-PRN-008` | — |
| Retirement | `PR-NFR` · `PR-WFL` | `AR-PRN-003` · `AR-PRN-007` | — |

### 17.3 Traceability to Sibling Runtime Documents

This document does not trace to `SP-`, `RF-`, `RA-`, `MR-`, or `CR-` identifiers, and that absence is
recorded here as a stated fact rather than left silent, consistent with AEOS-DOCSTD `DS-P-10`:

- No behavioral Specification document, under AEOS-SPECSTD, currently specifies Runtime Registry
  behavior in this repository, so no `SP-` trace exists. Should one later be written for interface or
  wire-format detail this document excludes under [Section 15](#15-constraints), it would trace back
  to the `RT-REG-` rules stated here, not the reverse.
- No Runtime Flow, Runtime Adapter, Model Registry, or Capability Registry document exists in this
  repository as a separate, frozen artifact at the time of writing, so no `RF-`, `RA-`, `MR-`, or `CR-`
  trace exists. `RF-`, `RA-`, `MR-`, and `CR-` are, in any case, not identifier prefixes AEOS-GLOSSARY
  Section 6.4 currently registers; minting them here as though registered would violate AEOS-GLOSSARY
  rule `I4`, which reserves the introduction of a new layer prefix to Glossary governance.
- Where such companion documents are later authored, this document's traceability is extended, under
  its own change control, to reference them by whatever identifier scheme their own governance
  registers — never by a prefix assumed in advance of that registration.

---

## 18. References

| Reference | What is cited |
| :--- | :--- |
| AEOS-VISION | Invariant `V6` (no vendor, runtime, model, platform, or distribution is privileged or required). |
| AEOS-PRD, Section 3.2 | The Runtime layer's position among the layers AEOS-PRD defers to. |
| AEOS-PRD, Section 18.4 (`PR-RUN`) | The AI Runtime Orchestration requirements this document traces to. |
| AEOS-PRD, Sections 18.1, 18.3, 20, 21 (`PR-REP`, `PR-WFL`, `PR-PLT`, `PR-SAF`) and Section 22 (`PR-NFR`) | Supporting requirements this document traces to. |
| AEOS-GLOSSARY, Core Terminology | The definitions of *Runtime*, *Runtime Adapter*, *Runtime State*, *Engineering Capability*, *Model*, *Vendor*, *Platform*, and *Repository Asset* used throughout. |
| AEOS-GLOSSARY, Section 6.4 | The identifier shape and registered prefixes this document's proposed `RT` prefix follows. |
| AEOS-DOCSTD, Section 4.5 | The unassigned position of the Runtime layer in the documentation hierarchy. |
| AEOS-ARCH, Section 4.6–4.7 | The Runtime Layer and Adapter Layer this document's behavior is orchestrated within. |
| AEOS-ARCH, Section 11 | Extension points `EP-3` and `EP-4`, which this document's extension points attach to. |
| AEOS-BLUEPRINT, Section 10 (`BP-RUN`) | The Runtime Blueprint's subsystem arrangement this document is written against. |
| AEOS-BLUEPRINT, Section 11 (`BP-ADP`) | The Adapter Blueprint this document's Adapter Compatibility behavior depends on. |
| AEOS-BLUEPRINT, Section 18 | The Blueprint and Runtime boundary this document observes. |
| AEOS-SPECSTD | The structural convention this document's section ordering borrows, without being governed by it. |

This document anticipates, but does not cite as existing, a Runtime Flow document, a Runtime Adapter
document, and a Model Registry document. Their absence is recorded in
[Section 17.3](#173-traceability-to-sibling-runtime-documents).

---

## 19. Document Governance

### 19.1 Status

This document is a **freeze candidate**. It is complete and self-reviewed against AEOS-DOCSTD, and is
intended to become the Runtime Registry Source of Truth once the owner's review under
[Section 19.4](#194-review-policy) records no Critical or Major finding. It declares itself the source
of truth for no subject already registered by AEOS-GLOSSARY or claimed by AEOS-PRD, AEOS-ARCH, or
AEOS-BLUEPRINT: the observable behavior and governance of the Runtime Registry is a subject none of
them claims.

### 19.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Clarification of an existing rule or boundary | Owner approval | Minor |
| Addition of a rule, an extension point, or a lifecycle state | Explicit owner revision request | Major |
| Adoption of the proposed `RT` prefix and `Runtime Registry` term by AEOS-GLOSSARY | AEOS-GLOSSARY's own change control, under `I4` and `W4` | Not a change to this document's version |
| Placement of the Runtime layer in the documentation hierarchy | AEOS-DOCSTD's own change control, under Section 14.2 and Section 4.5 | Not a change to this document's version |
| Removal of a rule or an extension class | Explicit owner decision, recorded, with the reasoning preserved in place | Major |

### 19.3 Relationship to the Architecture and Blueprint Freeze

This document introduces no Architecture Layer, no Blueprint subsystem, and no product capability. It
grants no capability that AEOS-PRD `PR-RUN` does not already require. An idea arising from it that
would alter the Runtime Blueprint's arrangement is routed to AEOS-BLUEPRINT under that document's
change control; an idea that would alter the product's concepts, capability set, or principles is
recorded as a recommendation for a future release under AEOS-PRD governance. Neither is resolved here.

### 19.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains.

A finding is **Critical** where this document contradicts a higher-authority document, assumes a
responsibility it does not own — including any statement of database schema, storage implementation,
registry backend, API endpoint, runtime implementation, plugin loading mechanism, or technology choice
prohibited under [Section 15](#15-constraints) — or states behavior that would cause incorrect work if
acted upon.

### 19.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION on an invariant of purpose | AEOS-VISION governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-PRD on product behavior or scope | AEOS-PRD governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-GLOSSARY on the meaning of a term | AEOS-GLOSSARY governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-DOCSTD on documentation form | AEOS-DOCSTD governs. The deviation is a finding against this document. |
| This document conflicts with AEOS-ARCH on a structural decision | AEOS-ARCH governs. The conflict is a defect here and is reported. |
| This document conflicts with AEOS-BLUEPRINT on the arrangement of `BP-RUN` or `BP-ADP` | AEOS-BLUEPRINT governs the arrangement; this document governs execution, per AEOS-BLUEPRINT Section 20.5. Where the two cannot both hold, escalate to the owner. |
| This document's structure diverges from AEOS-SPECSTD's | Not a conflict. AEOS-SPECSTD governs the Specification layer only; the divergence is a stylistic choice under this document's own authority. |
| A future Specification document deviates from the observable behavior stated here | Escalate to the owner. Neither document has default authority over the other, since Runtime and Specification are siblings under AEOS-PRD Section 3.2, not one subordinate to the other. |
| Two Runtime documents conflict with one another | Escalate to the owner. A contributor MUST NOT resolve it by preference. |

### 19.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Freeze candidate | Initial Runtime Registry document. Establishes the document's authority as a Runtime-layer document under AEOS-PRD Section 3.2 and AEOS-DOCSTD Section 4.5's unassigned-layer treatment; proposes a Runtime-layer identifier prefix `RT` and area code `REG` under AEOS-GLOSSARY `I4`, and proposes the term *Runtime Registry* under `W4`. States registry ownership, Runtime identity, Runtime metadata, Runtime classification, Runtime capability declaration, supported Runtime categories, and a nine-part Runtime lifecycle (registration, discovery, version compatibility, Platform compatibility, adapter compatibility, capability lookup, availability, deprecation, retirement) narratively, and consolidates fifty-five `RT-REG-001`–`RT-REG-055` invariants traced to `PR-`, `AR-`, and `BP-` identifiers. States Inputs, Outputs, three Extension Points attaching to `EP-3` and `EP-4`, seven Constraints excluding database schema, storage implementation, registry backend, API endpoint, runtime implementation, plugin loading, and technology choice, and seven Non-goals. Records the absence of `SP-`, `RF-`, `RA-`, `MR-`, and `CR-` traces as a stated fact. Introduces no product requirement, no Architecture Layer, no Blueprint subsystem, and no implementation. |

---

## Appendix A — Registry Identifier Index

**Non-normative.** This appendix summarizes identifiers stated normatively in
[Section 11](#11-registry-invariants). Where it and the body differ, the body governs.

| Group | Subject | Range | Count | Section |
| :--- | :--- | :--- | :--- | :--- |
| Ownership | Custody, credential exclusion, non-privilege | 001–005 | 5 | [11.1](#111-ownership-invariants) |
| Identity | Stable, unique, credential-free identity | 006–009 | 4 | [11.2](#112-identity-invariants) |
| Metadata | Recorded fields and their reportability | 010–014 | 5 | [11.3](#113-metadata-invariants) |
| Classification | Declared category and its neutrality | 015–018 | 4 | [11.4](#114-classification-invariants) |
| Capability declaration | Neutral-vocabulary capability statements | 019–022 | 4 | [11.5](#115-capability-declaration-invariants) |
| Supported categories | Category admission and its extensibility | 023–025 | 3 | [11.6](#116-supported-category-invariants) |
| Registration | Entry into the registry | 026–028 | 3 | [11.7](#117-registration-invariants) |
| Discovery | Enumeration of registered entries | 029–031 | 3 | [11.8](#118-discovery-invariants) |
| Version compatibility | Declared version-range checking | 032–034 | 3 | [11.9](#119-version-compatibility-invariants) |
| Platform compatibility | Declared Platform-range checking | 035–037 | 3 | [11.10](#1110-platform-compatibility-invariants) |
| Adapter compatibility | One-adapter-per-entry checking | 038–040 | 3 | [11.11](#1111-adapter-compatibility-invariants) |
| Capability lookup | Requirement-to-offer resolution | 041–043 | 3 | [11.12](#1112-capability-lookup-invariants) |
| Availability | Observed reachability | 044–047 | 4 | [11.13](#1113-availability-invariants) |
| Deprecation | Marking an entry for eventual retirement | 048–051 | 4 | [11.14](#1114-deprecation-invariants) |
| Retirement | Permanent removal from selectability | 052–055 | 4 | [11.15](#1115-retirement-invariants) |
| **Total** | | | **55** | — |

---

**End of Runtime Registry**

AEOS-RUNTIME-REG · Version 1.0.0 · Runtime Registry Source of Truth (freeze candidate, pending owner review)
