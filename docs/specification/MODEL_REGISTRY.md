# AI Engineering Operating System

## AEOS — Model Registry Specification

*The permanent statement of how a Model's identity, metadata, classification, and declared
capability are registered, discovered, and retired within AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | Model Registry Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SPEC-MDL |
| **Version** | 1.0.0 |
| **Status** | Freeze candidate |
| **Owner** | Product Owner, AEOS |
| **Author** | Specification Governance Board, AEOS |
| **Audience** | Architects, implementers, reviewers, test authors, and AI runtimes |
| **Suggested path** | `docs/specification/MODEL_REGISTRY.md` |
| **Companion documents** | `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_VISION.md` (AEOS-VISION) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) |
| **Supersedes** | None |
| **Area code** | `MDL` |

> **Authority of this document.**
> This document specifies, precisely and testably, the registration, discovery, and lifecycle
> behavior of the AEOS Model Registry behavior domain: how a Model's identity, declared metadata,
> classification, and declared Engineering Capability are made discoverable, and how that
> discoverability changes over time.
>
> It defines no rationale beyond its `PR-` trace, no architectural layer or component, no Blueprint
> arrangement, no database schema, no storage implementation, no API surface, and no implementation.
> It is not a Product document, not an Architecture document, not a Blueprint, and not an
> Implementation Guide. Where this document and a document of higher authority both speak to a
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
13. [Appendix A — Identifier Index](#appendix-a--identifier-index)

---

## 1. Purpose

AEOS-PRD `PR-RUN-006` commits AEOS to independence from any specific Model, and AEOS-ARCH
`AR-BND-005` confines knowledge of a Model to the Adapter Layer alone. Neither statement, by itself,
says what a Workflow step, a reviewer, or the Human Layer may learn about a Model before depending on
it, or how that knowledge changes as an adapter's declaration changes over time. AEOS-BLUEPRINT
assigns the neutral vocabulary of Engineering Capability to the Runtime Blueprint's Capability
Vocabulary subsystem (`BP-RUN-003`) and the act of stating an offer in that vocabulary to the Adapter
Blueprint's Capability Advertisement responsibility (`BP-ADP-003`), but a Blueprint arrangement is
deliberately short of the level at which a test can check whether a given declaration was accepted,
revised, or withdrawn correctly.

This Specification closes that gap for one behavior domain: the identity, metadata, classification,
declared capability, lifecycle, and query behavior of a Model as AEOS makes it discoverable. It
states, precisely and testably, what must be true of a Model's registration at every point from the
moment a declaring adapter first states it to the moment that declaration is withdrawn — without
holding, duplicating, or becoming a second authority over knowledge that `AR-BND-005` and `BP-ADP-001`
already confine to the Adapter Layer.

The **Model Registry**, as this document uses the term, names a behavior domain — a set of testable
rules — and not a new architectural layer, subsystem, or Repository Asset kind. Every rule in this
document is realized within the layers, subsystems, and dependencies AEOS-ARCH and AEOS-BLUEPRINT
already assign; this Specification introduces none of its own, consistent with AEOS-BLUEPRINT
`BP-GOV-009`, under which a Specification MUST NOT relocate a responsibility the Blueprint assigns.

---

## 2. Scope

### 2.1 Behavior Domain Registered

This document registers the `AREA` code `MDL` under AEOS-SPECSTD §11.4 and AEOS-GLOSSARY `I3`. `MDL`
names the Model Registry behavior domain and is not previously registered by any Architecture or
Blueprint document at the time of this document's authorship.

### 2.2 What This Domain Covers

| Subject | Covered as |
| :--- | :--- |
| Model identity | How a Model is identified for the purposes of registration, discovery, and query. |
| Model metadata | How descriptive properties of a Model are made available and how their origin is treated. |
| Model classification | How a Model is categorized using the neutral Engineering Capability vocabulary. |
| Model capability declaration | How a Model's declared Engineering Capability set is expressed and attributed. |
| Capability categories | How categories within the neutral vocabulary are defined and extended. |
| Model lifecycle | Registration, discovery, versioning, compatibility, deprecation, and retirement of a Model's registration. |
| Capability queries | How a query for declared Model capability is answered. |

### 2.3 Boundary

This domain covers the observable behavior of registration, discovery, and query only. It does not
cover how an adapter mediates its Runtime, how a Workflow step's requirement is matched against a
Model's declared offer, how credentials reach a Runtime, or how the cost of crossing to a Runtime is
disclosed — each of which is owned by a distinct Blueprint-assigned responsibility and, where not yet
specified, by a distinct future Specification document. [Section 10](#10-non-goals) states these
exclusions completely.

### 2.4 Applicability

This domain applies to every Model that a Runtime Adapter declares to AEOS, regardless of the
category of Runtime the adapter mediates, per `PR-RUN-016`. It applies identically regardless of
Platform, per `AR-LAY-007`, and regardless of Distribution Method, per `PR-DST-005`.

---

## 3. Responsibilities

### 3.1 Answerable For

| # | This Specification is answerable for |
| :--- | :--- |
| 1 | The identity behavior of a Model within the Model Registry behavior domain. |
| 2 | The treatment of a Model's declared metadata. |
| 3 | The classification of a Model within the neutral Engineering Capability vocabulary. |
| 4 | The declaration behavior of a Model's Engineering Capability set. |
| 5 | The definition and extension of capability categories within that vocabulary. |
| 6 | The registration, discovery, versioning, compatibility, deprecation, and retirement behavior of a Model's registration. |
| 7 | The behavior of a capability query answered against registered Models. |

### 3.2 Not Answerable For

| Excluded responsibility | Owned by |
| :--- | :--- |
| Mediating a Runtime, or holding a Vendor's or Runtime's implementation detail | The Adapter Blueprint (`BP-ADP`) and the Specification governing the `ADP` behavior domain, not yet frozen at the time of this document's authorship. |
| Comparing a Workflow step's required Engineering Capability against a Model's currently declared capability set at the moment a step begins | The Runtime Blueprint's Capability Matching responsibility (`BP-RUN-004`) and the Specification governing the `RUN` behavior domain, not yet frozen at the time of this document's authorship. |
| Holding or transmitting credentials | The Adapter Blueprint (`BP-ADP-008`). |
| Assembling the disclosure of what will cross the boundary to a Runtime, and its expected cost | The Runtime Blueprint's Boundary Disclosure Assembly responsibility (`BP-RUN-006`, `BP-RUN-007`). |
| Performing inference | No layer of AEOS; forbidden absolutely by `AR-BND-002`. |
| Deciding whether a Workflow step proceeds | The Workflow Blueprint (`BP-WFL`). |

### 3.3 Registry Ownership

The Model Registry behavior domain does not own Model knowledge. `AR-BND-005` confines Runtime,
Vendor, and Model knowledge to the Adapter Layer, and AEOS-ARCH §11.3 states explicitly that a second
surface for Model knowledge would breach that invariant. Every rule in this document is written to
that constraint: the domain's registration, discovery, and query behavior operates only on what a
declaring adapter has stated, presents nothing as independently known, and holds no authority to add,
alter, or interpret a Model's identity, metadata, classification, or declared capability beyond what
the declaring adapter provided. [Section 7.1](#71-registry-ownership-invariants) states this
constraint in testable form.

---

## 4. Inputs

| Input | Required properties | Validity condition |
| :--- | :--- | :--- |
| Capability declaration | Declaring adapter identity; a Model identifier scoped to that adapter; a declared Engineering Capability set expressed in the neutral vocabulary | Valid only when it originates from the adapter mediating the declared Model's Runtime, per `SP-MDL-014`. |
| Descriptive metadata (optional, accompanying a declaration) | One or more descriptive properties, in whatever form the declaring adapter provides | Valid whenever supplied by the declaring adapter; no property is mandatory. |
| Classification terms (optional, accompanying a declaration) | One or more terms drawn from the neutral vocabulary's registered capability categories | Valid only when every term is a category the Capability Vocabulary subsystem holds, per `SP-MDL-009`. |
| Declaration revision | The previously declared Model identifier; the changed metadata, classification, or capability set | Valid only when it originates from the same declaring adapter as the prior declaration. |
| Deprecation notice | The declared Model identifier; an indication that the declaration is deprecated | Valid only when it originates from the declaring adapter. |
| Retirement notice | The declared Model identifier | Valid when it originates from the declaring adapter, or arises from the withdrawal of the declaring adapter itself. |
| Discovery request | None beyond the request | Always valid; discovery is available on demand, per `SP-MDL-024`. |
| Capability query | Engineering Capability terms or classification terms, expressed in the neutral vocabulary | Valid only when every term is one the Capability Vocabulary subsystem holds. |

---

## 5. Outputs

| Output | Content | Produced when |
| :--- | :--- | :--- |
| Registration record | Model identifier, declaring adapter identity, declared metadata, declared classification, declared capability set, and current lifecycle status | A capability declaration is accepted (`SP-MDL-020`). |
| Discovery result | The current registration record or records matching a discovery request, each carrying its lifecycle status | A discovery request is made (`SP-MDL-024`). |
| Query result | For each queried term: declared-and-reachable, declared-and-unreachable, or not declared | A capability query is answered ([§6.7](#67-capability-queries)). |
| Revision record | The prior and the revised declaration, distinguished as a distinct, ordered revision | A declaration changes (`SP-MDL-027`). |
| Deprecation marker | The Model identifier and its deprecated status | A deprecation notice is accepted (`SP-MDL-032`). |
| Retirement removal | The Model's removal from default discovery output | A retirement notice is accepted, or the declaring adapter is withdrawn (`SP-MDL-035`). |

---

## 6. Behavior

### 6.1 Model Identity

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-001` | A Model MUST be identified, within this behavior domain, by an identifier scoped to the single adapter that declares it. | `PR-RUN-006` · `AR-BND-005` · `BP-ADP-001` |
| `SP-MDL-002` | A Model identifier MUST remain associated with the same Model for as long as the declaring adapter continues to declare it, and MUST NOT be reassigned to a different Model. | `PR-NFR-001` · `PR-RUN-008` · `BP-RUN-013` |
| `SP-MDL-003` | The Model Registry behavior MUST NOT assign, alter, or interpret a Model identifier independently of the declaring adapter. | `AR-BND-005` · `BP-ADP-001` · `BP-GOV-009` |
| `SP-MDL-004` | Two Models declared by different adapters MUST be treated as distinct, even where their declared identifiers are textually identical. | `PR-RUN-003` · `AR-BND-011` · `BP-ADP-002` |

### 6.2 Model Metadata

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-005` | The Model Registry behavior MUST make available, for a registered Model, whatever descriptive metadata the declaring adapter provides. | `PR-RUN-003` · `BP-ADP-003` |
| `SP-MDL-006` | The Model Registry behavior MUST NOT require a descriptive metadata property the declaring adapter does not supply. | `PR-RUN-003` · `BP-ADP-003` |
| `SP-MDL-007` | Descriptive metadata MUST be presented as declared, and MUST NOT be presented as independently verified or observed by AEOS. | `PR-SAF-011` · `AR-PRN-007` |
| `SP-MDL-008` | The Model Registry behavior MUST NOT infer, estimate, or supply a metadata value the declaring adapter did not provide. | `PR-SAF-011` · `AR-BND-002` · `AR-BND-005` |

### 6.3 Model Classification

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-009` | A Model MUST be classified only using terms drawn from the neutral vocabulary the Capability Vocabulary subsystem holds. | `BP-RUN-003` · `PR-RUN-007` |
| `SP-MDL-010` | The Model Registry behavior MUST NOT classify a Model by Vendor identity. | `AR-BND-011` · `PR-RUN-003` |
| `SP-MDL-011` | The Model Registry behavior MUST NOT rank, score, or order Models against one another. | `AR-BND-011` · `PR-RUN-003` |

### 6.4 Model Capability Declaration

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-012` | A Model's declared Engineering Capability set MUST be expressed only in the neutral vocabulary the Capability Vocabulary subsystem holds. | `BP-RUN-003` · `BP-ADP-003` · `PR-RUN-007` |
| `SP-MDL-013` | A Model's declared Engineering Capability set MUST be attributable to exactly one declaring adapter. | `BP-ADP-002` · `AR-BND-005` |
| `SP-MDL-014` | The Model Registry behavior MUST NOT accept a capability declaration for a Model from any party other than the adapter mediating that Model's Runtime. | `AR-BND-005` · `BP-ADP-001` · `BP-GOV-009` |
| `SP-MDL-015` | A Model's declared Engineering Capability set MUST persist independently of the Model's current reachability. | `BP-RUN-011` · `PR-RUN-010` |
| `SP-MDL-016` | The Model Registry behavior MUST NOT add a declared Engineering Capability that the mediating adapter has not itself declared. | `AR-BND-005` · `BP-ADP-001` · `BP-GOV-009` |

### 6.5 Capability Categories

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-017` | A capability category MUST be defined only within the neutral vocabulary the Capability Vocabulary subsystem holds. | `BP-RUN-003` · `PR-RUN-007` |
| `SP-MDL-018` | A new capability category MUST be admitted as an addition to the vocabulary and MUST NOT alter the meaning of an existing category. | `AR-PRN-009` · `PR-NFR-007` |
| `SP-MDL-019` | A capability category MUST NOT be defined in terms of a specific Vendor, Runtime, or Model. | `AR-BND-013` · `PR-RUN-005` |

### 6.6 Model Lifecycle

#### 6.6.1 Registration

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-020` | A Model becomes discoverable through this behavior domain only when its declaring adapter's capability declaration for it is accepted, per `SP-MDL-014`. | `BP-ADP-005` · `PR-RUN-012` |
| `SP-MDL-021` | Registration of a Model MUST occur without modification to the Runtime Blueprint's arrangement or to any existing project. | `BP-RUN-012` · `BP-ADP-006` · `PR-RUN-012` · `PR-RUN-016` |
| `SP-MDL-022` | A Model's registration MUST record the identity of its declaring adapter alongside the Model's identifier. | `AR-BND-005` · `AR-PRN-007` · `PR-NFR-001` |

#### 6.6.2 Discovery

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-023` | The Model Registry behavior MUST make a registered Model's identity, declared metadata, classification, and declared Engineering Capability set available for inspection before a Workflow step that depends on that Model begins. | `PR-RUN-007` · `BP-RUN-004` |
| `SP-MDL-024` | Discovery of a registered Model MUST be available on demand without altering registry state. | `PR-ENV-011` · `PR-NFR-003` |
| `SP-MDL-025` | Discovery output MUST distinguish a Model that is registered but currently unreachable from a Model that is not registered. | `BP-RUN-011` · `PR-ENV-004` · `PR-SAF-011` |
| `SP-MDL-026` | Discovery output MUST distinguish an observed fact about a Model's registration from any inference. | `PR-SAF-011` · `AR-PRN-007` |

#### 6.6.3 Versioning

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-027` | A revision to a Model's declared metadata, classification, or capability set MUST be recorded as a distinct, ordered revision rather than merged silently with the prior declaration. | `PR-NFR-001` · `PR-NFR-002` · `AR-PRN-007` |
| `SP-MDL-028` | The Model Registry behavior MUST retain the ability to state which declared revision of a Model a prior discovery or query result reflected. | `PR-NFR-002` · `BP-RUN-013` |
| `SP-MDL-029` | A previously recorded revision MUST NOT be reordered or renumbered. | `PR-NFR-001` · `PR-NFR-002` |

#### 6.6.4 Compatibility

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-030` | Where a Model's declared Engineering Capability set changes, the Model Registry behavior MUST make it possible to determine whether the prior declared set is a subset of the revised declared set. | `PR-RUN-008` · `BP-RUN-013` |
| `SP-MDL-031` | A revision that removes a previously declared Engineering Capability MUST be reported as a narrowing, distinguished from a revision that only adds. | `PR-NFR-001` · `PR-RUN-008` |

> **Boundary note (non-normative).** Comparing a Workflow step's required Engineering Capability
> against a Model's currently declared capability set at the moment a step begins is Capability
> Matching, owned by the Runtime Blueprint (`BP-RUN-004`) and by the Specification governing the
> `RUN` behavior domain. This Specification supplies the declared capability set such a comparison
> reads; it does not perform the comparison. See [Section 10](#10-non-goals).

#### 6.6.5 Deprecation

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-032` | A deprecation MUST originate from the declaring adapter's declaration. | `AR-BND-005` · `BP-ADP-001` |
| `SP-MDL-033` | A deprecated Model MUST remain discoverable, marked distinctly from a Model that is not deprecated. | `PR-NFR-001` · `AR-PRN-007` |
| `SP-MDL-034` | The Model Registry behavior MUST NOT remove a deprecated Model from discovery solely because it is deprecated; removal occurs only under Retirement, [Section 6.6.6](#666-retirement). | `PR-NFR-001` · `AR-PRN-007` |

#### 6.6.6 Retirement

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-035` | Retirement MUST originate from the declaring adapter ceasing to declare the Model, or from the withdrawal of the declaring adapter itself. | `AR-BND-005` · `BP-ADP-001` |
| `SP-MDL-036` | Retirement MUST remove the Model from Discovery's default output. | `BP-ADP-001` · `PR-NFR-001` |
| `SP-MDL-037` | Discovery output MUST distinguish a retired Model from a Model that is merely unreachable. | `BP-RUN-011` · `PR-SAF-011` · `PR-ENV-004` |
| `SP-MDL-038` | A retired Model's identifier MUST NOT be reassigned by the same adapter to a different Model. | `PR-NFR-001` · `PR-NFR-002` |

### 6.7 Capability Queries

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-039` | A capability query MUST be answerable without invoking any Runtime. | `AR-BND-002` · `AR-EXT-005` · `PR-RUN-001` |
| `SP-MDL-040` | A capability query MUST NOT alter Model Registry state. | `PR-ENV-011` |
| `SP-MDL-041` | A query result MUST distinguish a capability that is declared but currently unreachable from a capability that has never been declared. | `BP-RUN-004` · `PR-RUN-007` |
| `SP-MDL-042` | A query result relied upon by a Workflow step MUST be reported before that step begins. | `PR-RUN-007` · `BP-RUN-004` |
| `SP-MDL-043` | A capability query MUST NOT require reaching a Runtime that has not been selected and approved. | `PR-SAF-007` · `PR-RUN-004` |

---

## 7. Constraints

### 7.1 Registry Ownership Invariants

| ID | Invariant | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-044` | The Model Registry behavior MUST NOT hold Model knowledge that was not declared by the mediating adapter, before, during, or after any rule in [Section 6](#6-behavior) is applied. | `AR-BND-005` · `BP-ADP-001` · `BP-GOV-009` |
| `SP-MDL-045` | The Model Registry behavior MUST NOT constitute a second surface of Model knowledge alongside the Adapter Layer. | `AR-BND-005` · `BP-ADP-001` |
| `SP-MDL-046` | Where discoverable output would conflict with the declaring adapter's current declaration, the declaring adapter's current declaration governs, and the discoverable output MUST be corrected. | `PR-NFR-001` · `AR-BND-005` |

### 7.2 Neutrality Invariants

| ID | Invariant | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-047` | The Model Registry behavior MUST NOT privilege one Model, Runtime, or Vendor over another in what it makes discoverable. | `AR-BND-011` · `PR-RUN-003` · `PR-RUN-004` |
| `SP-MDL-048` | The Model Registry behavior MUST NOT rank or benchmark Models. | `AR-BND-011` · `PR-RUN-003` |
| `SP-MDL-049` | The Model Registry behavior MUST express every Model's classification and declared capability in the neutral vocabulary, and MUST NOT expose a Vendor's proprietary terms as the basis for a registry-level decision. | `BP-RUN-003` · `AR-BND-013` |

### 7.3 Additional Registry Invariants

| ID | Invariant | Traces to |
| :--- | :--- | :--- |
| `SP-MDL-050` | The absence of a Model from the registry MUST reduce available options only, and MUST NOT block a capability that does not depend on that Model. | `AR-PRN-008` · `PR-RUN-010` |
| `SP-MDL-051` | The Model Registry behavior MUST NOT hold or expose a credential. | `PR-SAF-006` · `AR-BND-009` |
| `SP-MDL-052` | A Model's identifier and registration record MUST be identical in meaning across every supported Platform. | `AR-LAY-007` · `PR-PLT-003` |

---

## 8. Extension Points

| Extension point | What is added | Boundary — what MUST NOT change |
| :--- | :--- | :--- |
| **Capability category admission** | A new capability category within the neutral vocabulary, per the Runtime Blueprint's Capability vocabulary extension point (AEOS-BLUEPRINT §10.7). | The registered meaning of an existing category, per `SP-MDL-018`. |
| **Metadata property admission** | A new descriptive metadata property a declaring adapter may supply. | The optionality of every existing property, per `SP-MDL-006`; no previously valid declaration becomes invalid. |
| **Lifecycle state admission** | A new discoverable lifecycle state beyond registered, deprecated, and retired. | The meaning of an existing lifecycle state, and Retirement's discovery-removal rule, `SP-MDL-036`. |

Each Extension Point above is bounded per AEOS-SPECSTD `EX3`: an addition within the stated boundary
requires no new identifier in this document; an addition that would cross the boundary is a revision
to an existing `SP-MDL` identifier or a new one, under AEOS-SPECSTD's own Extension Rules section.

---

## 9. Traceability

Every rule in [Section 6](#6-behavior) and [Section 7](#7-constraints) states its own trace in its
own row, per AEOS-SPECSTD `TR1` and `TR2`. This section consolidates that trace by subsection, per
AEOS-BLUEPRINT's Appendix B convention, so that a reader holding a `PR-`, `AR-`, or `BP-` identifier
can find the rules that depend on it without searching the full rule set.

| Subsection | Rule range | Product requirements traced | Architecture invariants traced | Blueprint rules traced |
| :--- | :--- | :--- | :--- | :--- |
| Model Identity | `SP-MDL-001`–`004` | `PR-RUN-003` · `PR-RUN-006` · `PR-RUN-008` · `PR-NFR-001` | `AR-BND-005` · `AR-BND-011` | `BP-ADP-001` · `BP-ADP-002` · `BP-RUN-013` · `BP-GOV-009` |
| Model Metadata | `SP-MDL-005`–`008` | `PR-RUN-003` · `PR-SAF-011` | `AR-PRN-007` · `AR-BND-002` · `AR-BND-005` | `BP-ADP-003` |
| Model Classification | `SP-MDL-009`–`011` | `PR-RUN-003` · `PR-RUN-007` | `AR-BND-011` | `BP-RUN-003` |
| Model Capability Declaration | `SP-MDL-012`–`016` | `PR-RUN-007` · `PR-RUN-010` | `AR-BND-005` | `BP-RUN-003` · `BP-RUN-011` · `BP-ADP-001` · `BP-ADP-002` · `BP-ADP-003` · `BP-GOV-009` |
| Capability Categories | `SP-MDL-017`–`019` | `PR-RUN-005` · `PR-RUN-007` · `PR-NFR-007` | `AR-PRN-009` · `AR-BND-013` | `BP-RUN-003` |
| Lifecycle — Registration | `SP-MDL-020`–`022` | `PR-RUN-012` · `PR-RUN-016` · `PR-NFR-001` | `AR-BND-005` · `AR-PRN-007` | `BP-RUN-012` · `BP-ADP-005` · `BP-ADP-006` |
| Lifecycle — Discovery | `SP-MDL-023`–`026` | `PR-RUN-007` · `PR-ENV-004` · `PR-ENV-011` · `PR-NFR-003` · `PR-SAF-011` | `AR-PRN-007` | `BP-RUN-004` · `BP-RUN-011` |
| Lifecycle — Versioning | `SP-MDL-027`–`029` | `PR-NFR-001` · `PR-NFR-002` | `AR-PRN-007` | `BP-RUN-013` |
| Lifecycle — Compatibility | `SP-MDL-030`–`031` | `PR-NFR-001` · `PR-RUN-008` | — | `BP-RUN-013` |
| Lifecycle — Deprecation | `SP-MDL-032`–`034` | `PR-NFR-001` | `AR-BND-005` · `AR-PRN-007` | `BP-ADP-001` |
| Lifecycle — Retirement | `SP-MDL-035`–`038` | `PR-NFR-001` · `PR-NFR-002` · `PR-ENV-004` · `PR-SAF-011` | `AR-BND-005` | `BP-ADP-001` · `BP-RUN-011` |
| Capability Queries | `SP-MDL-039`–`043` | `PR-RUN-001` · `PR-RUN-004` · `PR-RUN-007` · `PR-SAF-007` · `PR-ENV-011` | `AR-BND-002` · `AR-EXT-005` | `BP-RUN-004` |
| Registry Ownership Invariants | `SP-MDL-044`–`046` | `PR-NFR-001` | `AR-BND-005` | `BP-ADP-001` · `BP-GOV-009` |
| Neutrality Invariants | `SP-MDL-047`–`049` | `PR-RUN-003` · `PR-RUN-004` | `AR-BND-011` · `AR-BND-013` | `BP-RUN-003` |
| Additional Registry Invariants | `SP-MDL-050`–`052` | `PR-RUN-010` · `PR-SAF-006` · `PR-PLT-003` | `AR-PRN-008` · `AR-BND-009` · `AR-LAY-007` | — |

Per AEOS-BLUEPRINT `BP-GOV-008`, this document traces to at least one Blueprint identifier and at
least one `PR-` identifier for every rule; the table above and each rule's own row together satisfy
that obligation.

---

## 10. Non-goals

| Behavior a reader might expect this domain to cover | Why it is out of scope | Owned by |
| :--- | :--- | :--- |
| A vendor-specific catalog of Model names, prices, or release schedules | Would require holding Vendor-specific knowledge outside the Adapter Layer, breaching `AR-BND-005`. | The Adapter Layer, and the Vendor's own published material, neither of which AEOS reproduces. |
| Ranking or benchmarking Models against one another | Would privilege one Vendor or Model over another, contrary to `AR-BND-011`; recorded as a deferred recommendation, not a requirement, in AEOS-PRD Appendix A. | Not owned by AEOS at any layer. |
| The algorithm or data structure an adapter uses to compute or store its own declaration | Mechanism, prohibited to a Specification by AEOS-SPECSTD `MN1` and `MN2`. | Implementation Guides, once written against the `ADP` behavior domain's Specification. |
| Comparing a Workflow step's required Engineering Capability against a Model's currently declared capability set at the moment a step begins | Owned by the Runtime Blueprint's Capability Matching responsibility (`BP-RUN-004`), a distinct behavior domain. | The Specification governing the `RUN` behavior domain, not yet frozen at the time of this document's authorship. |
| Holding, transmitting, or disclosing credentials needed to reach a Model | Owned by the Adapter Blueprint (`BP-ADP-008`), a distinct behavior domain. | The Specification governing the `ADP` behavior domain, not yet frozen at the time of this document's authorship. |
| Assembling what will cross the boundary to a Runtime and its expected cost | Owned by the Runtime Blueprint's Boundary Disclosure Assembly responsibility (`BP-RUN-006`, `BP-RUN-007`), a distinct behavior domain. | The Specification governing the `RUN` behavior domain, not yet frozen at the time of this document's authorship. |
| A database schema, storage engine, or persistence mechanism for a registration record | Mechanism, prohibited by AEOS-SPECSTD `MN1`. | Implementation Guides. |
| An API endpoint, method signature, or wire format for a discovery request or capability query | Interface surface, prohibited by AEOS-SPECSTD `MN4`. | Implementation Guides, and Blueprint or Architecture governance if a new entry surface is required. |
| Performing inference to answer a capability query | Forbidden absolutely by `AR-BND-002`; see `SP-MDL-039`. | No layer of AEOS. |

---

## 11. References

| Document | Document ID | Cited for |
| :--- | :--- | :--- |
| `AEOS_PRODUCT_REQUIREMENTS.md` | AEOS-PRD | `PR-RUN`, `PR-ENV`, `PR-SAF`, `PR-PLT`, `PR-NFR` requirement families cited throughout this document. |
| `AEOS_VISION.md` | AEOS-VISION | The invariants of Runtime Independence, Model Independence, and Vendor Independence this document is written to preserve. |
| `AEOS_GLOSSARY.md` | AEOS-GLOSSARY | The definitions of *Model*, *Runtime*, *Runtime Adapter*, *Vendor*, *Engineering Capability*, and *Specification*, used throughout this document without restatement, per `XR4`. |
| `AEOS_DOCUMENT_STANDARD.md` | AEOS-DOCSTD | The form, structure, and lifecycle this document follows. |
| `AEOS_SUPPORTED_TECHNOLOGIES.md` | AEOS-TECH | The technology-neutrality this document observes in every rule, per `SS-P-06`. |
| `AEOS_ARCHITECTURE.md` | AEOS-ARCH | `AR-BND-002`, `AR-BND-005`, `AR-BND-009`, `AR-BND-011`, `AR-BND-013`, `AR-PRN-007`, `AR-PRN-008`, `AR-PRN-009`, `AR-LAY-007`, `AR-EXT-005`, and AEOS-ARCH §11.3's treatment of Model providers and capability registries as extension points. |
| `AEOS_BLUEPRINT.md` | AEOS-BLUEPRINT | `BP-RUN-003`, `BP-RUN-004`, `BP-RUN-011`, `BP-RUN-012`, `BP-RUN-013`, `BP-ADP-001` through `BP-ADP-006`, `BP-ADP-008`, `BP-GOV-008`, `BP-GOV-009`. |
| `AEOS_SPECIFICATION_STANDARD.md` | AEOS-SPECSTD | The Specification-layer form, identifier convention, traceability rules, and freeze checklist this document conforms to. |

### 11.1 Forthcoming Companion Specifications

At the time of this document's authorship, the Specification documents governing the `RUN` behavior
domain (Runtime coordination behavior, including Capability Matching and Boundary Disclosure
Assembly) and the `ADP` behavior domain (Adapter mediation behavior, including credential handling)
have not been frozen. This document depends on rules those domains will state, per AEOS-SPECSTD
`DP1`; the dependencies are declared behaviorally in [Section 3.2](#32-not-answerable-for) and
[Section 10](#10-non-goals) rather than by citing an identifier that does not yet exist. When those
Specification documents are frozen, this document's dependencies MUST be reconciled against their
stated identifiers, per AEOS-SPECSTD `DP5`.

---

## 12. Document Governance

### 12.1 Status

This document is a **Freeze candidate**. It is the first Specification document to register the
`MDL` `AREA` code, per [Section 2.1](#21-behavior-domain-registered). It is not yet reviewed and
frozen, and per AEOS-SPECSTD `SS-P-08`, nothing MUST depend on it until it is.

### 12.2 Change Control

This document follows the versioning rule AEOS-SPECSTD §18.1 states for every Specification
document.

| Increment | When |
| :--- | :--- |
| **Patch** | Editorial correction with no change to a rule's meaning or trace. |
| **Minor** | Addition of a new `SP-MDL` identifier that does not alter what an existing identifier requires. |
| **Major** | Any change to what an existing `SP-MDL` identifier requires; retirement of an identifier; a change to this document's `AREA` code, ownership, or declared behavior domain; or any change that would invalidate a downstream Implementation Guide or test written against a prior version. |

### 12.3 Review Policy

Review of this document classifies findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identifies inconsistencies without redesigning the document, and recommends Freeze only when no
Critical or Major finding remains, per AEOS-DOCSTD §12.3–12.4 and the freeze checklist AEOS-SPECSTD
§19.2 states.

### 12.4 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, or AEOS-TECH | The higher-authority document governs; the conflict is a defect here and is reported. |
| This document conflicts with an AEOS-ARCH invariant or an AEOS-BLUEPRINT rule it is written against | AEOS-ARCH or AEOS-BLUEPRINT governs; this document is corrected. |
| This document conflicts with AEOS-SPECSTD on Specification form, identifier convention, or lifecycle | AEOS-SPECSTD governs; this document is corrected. |
| A downstream Implementation Guide or test conflicts with a rule in this document | This document governs; the downstream artifact is corrected. |
| This document states an architectural decision, a Blueprint arrangement, a database schema, an API surface, or an implementation | The statement is a defect here, per AEOS-SPECSTD `MN1`–`MN7`, and is reported rather than acted upon. |

### 12.5 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Freeze candidate | Initial Model Registry Specification. Registers the `MDL` `AREA` code. Establishes the Model Registry behavior domain as a non-structural set of testable rules governing Model identity, metadata, classification, capability declaration, capability categories, and the registration, discovery, versioning, compatibility, deprecation, and retirement stages of a Model's lifecycle, together with capability query behavior. States fifty-two `SP-MDL` rules, an Inputs and Outputs table, three Extension Points, a consolidated traceability table, and nine Non-goals. Introduces no architecture, no Blueprint arrangement, no requirement, no terminology, and no implementation. Relocates no responsibility AEOS-BLUEPRINT assigns. |

---

## Appendix A — Identifier Index

**Non-normative.** Indexes the identifier ranges stated in the document body.

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `SP-MDL-001`–`004` | Model Identity | 4 | [6.1](#61-model-identity) |
| `SP-MDL-005`–`008` | Model Metadata | 4 | [6.2](#62-model-metadata) |
| `SP-MDL-009`–`011` | Model Classification | 3 | [6.3](#63-model-classification) |
| `SP-MDL-012`–`016` | Model Capability Declaration | 5 | [6.4](#64-model-capability-declaration) |
| `SP-MDL-017`–`019` | Capability Categories | 3 | [6.5](#65-capability-categories) |
| `SP-MDL-020`–`022` | Lifecycle — Registration | 3 | [6.6.1](#661-registration) |
| `SP-MDL-023`–`026` | Lifecycle — Discovery | 4 | [6.6.2](#662-discovery) |
| `SP-MDL-027`–`029` | Lifecycle — Versioning | 3 | [6.6.3](#663-versioning) |
| `SP-MDL-030`–`031` | Lifecycle — Compatibility | 2 | [6.6.4](#664-compatibility) |
| `SP-MDL-032`–`034` | Lifecycle — Deprecation | 3 | [6.6.5](#665-deprecation) |
| `SP-MDL-035`–`038` | Lifecycle — Retirement | 4 | [6.6.6](#666-retirement) |
| `SP-MDL-039`–`043` | Capability Queries | 5 | [6.7](#67-capability-queries) |
| `SP-MDL-044`–`046` | Registry Ownership Invariants | 3 | [7.1](#71-registry-ownership-invariants) |
| `SP-MDL-047`–`049` | Neutrality Invariants | 3 | [7.2](#72-neutrality-invariants) |
| `SP-MDL-050`–`052` | Additional Registry Invariants | 3 | [7.3](#73-additional-registry-invariants) |
| **Total** | | **52** | — |

---

**End of Model Registry Specification**

AEOS-SPEC-MDL · Version 1.0.0 · Traces to `PR-RUN` · `PR-ENV` · `PR-SAF` · `PR-PLT` · `PR-NFR`
