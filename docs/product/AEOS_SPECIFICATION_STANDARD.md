# AI Engineering Operating System

## AEOS — Specification Standard

*The permanent statement of how every AEOS Specification document is written, scoped, identified,
traced, and frozen.*

| Field | Value |
| :--- | :--- |
| **Document** | Specification Standard |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SPECSTD |
| **Version** | 1.1.0 |
| **Status** | Frozen |
| **Owner** | Product Owner, AEOS |
| **Author** | Specification Governance Board, AEOS |
| **Audience** | AI systems, architects, contributors, maintainers, reviewers, and specification authors |
| **Suggested path** | `docs/product/SPECIFICATION_STANDARD.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document defines *how* every AEOS Specification document is written, structured, identified,
> traced, cross-referenced, extended, versioned, and frozen. It is normative for the **form and
> governance of the Specification layer only**.
>
> It is not a Product document, not an Architecture document, not a Blueprint, not a Runtime
> document, and not an Implementation guide. It defines no product requirement, no vision, no
> terminology, no structural decision, no technology choice, no runtime behavior, and no
> implementation. It does not alter the documentation hierarchy AEOS-DOCSTD defines; it supplies the
> Specification layer's own change control, which AEOS-DOCSTD Section 13.1 reserves to this layer.
> Where this document and a document of higher authority both speak to a subject, the
> higher-authority document governs and any conflict here is a defect to be reported.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Relationship to Other Documents](#3-relationship-to-other-documents)
4. [Specification Principles](#4-specification-principles)
5. [Purpose of Specifications](#5-purpose-of-specifications)
6. [Specification Ownership](#6-specification-ownership)
7. [Specification Responsibilities](#7-specification-responsibilities)
8. [What a Specification MUST Define](#8-what-a-specification-must-define)
9. [What a Specification MUST NOT Define](#9-what-a-specification-must-not-define)
10. [Standard Specification Structure](#10-standard-specification-structure)
11. [Identifier Conventions](#11-identifier-conventions)
12. [Normative Language](#12-normative-language)
13. [Traceability Rules](#13-traceability-rules)
14. [Cross-Reference Rules](#14-cross-reference-rules)
15. [Dependency Rules](#15-dependency-rules)
16. [Extension Rules](#16-extension-rules)
17. [Format Requirements](#17-format-requirements)
18. [Versioning and Change Management](#18-versioning-and-change-management)
19. [Freeze Policy](#19-freeze-policy)
20. [Document Governance](#20-document-governance)
21. [Appendix A — Specification Template](#appendix-a--specification-template)
22. [Appendix B — Recommended Specification Section Ordering](#appendix-b--recommended-specification-section-ordering)
23. [Appendix C — Rule Index](#appendix-c--rule-index)

---

## 1. Executive Summary

A Specification is the point in the AEOS document chain at which a decision stops being open to
interpretation. Above it, Architecture states how AEOS is structured and Blueprint states how that
structure is arranged to be built; both remain deliberately short of the level at which a reader
could implement anything from them alone. Below it, Implementation Guides and code exist only
because a Specification first said, precisely and testably, what they must do. A Specification that
is vague costs nothing at the moment it is written and costs everything at the moment two
implementations disagree about what it meant.

Every AEOS document is already required, by AEOS-DOCSTD, to be internally consistent, traceable, and
written for one artifact to serve two audiences. Specifications carry an additional obligation that
no other layer carries in the same degree: a Specification is read by a test before it is read by a
person. A paragraph that a human finds clear but a test cannot check is not yet a specification; it
is a description waiting to become one.

This Standard exists to make that obligation uniform. It states what a Specification is for, who
owns it, what question it answers, what it must contain, what it must never contain, how its rules
are identified and traced back to product requirements, how Specifications refer to one another and
to the layers above and below them, how they may be extended without weakening what is already
frozen, and how they move from draft to authoritative. It does none of this for the product itself —
it governs only the documents that state the product's behavior precisely.

| In one sentence |
| :--- | 
| A Specification states, precisely and testably, how one defined piece of AEOS behavior must work — nothing about why, nothing about how AEOS is structured, and nothing about how the statement is realized in code. |

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This Standard governs the Specification layer of AEOS documentation:

- the purpose and ownership of a Specification document;
- the responsibility boundary of the Specification layer, relative to every layer above and below it;
- what a Specification MUST and MUST NOT define;
- the standard structure every Specification document takes;
- the identifier convention for specified behavior and the rules governing its use;
- the traceability obligation linking every Specification to AEOS-PRD;
- the rules by which Specifications reference one another and the layers around them;
- the rules governing dependency between Specifications;
- the rules governing how a Specification is extended without weakening what is frozen;
- the format a Specification document is written in;
- the versioning, change management, and freeze policy specific to the Specification layer.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| The general form, language, and lifecycle of AEOS documentation | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| How AEOS is structured | Architecture documents |
| How the structure is arranged to be built | Blueprint documents |
| The content of any individual specified behavior | The Specification document that owns it |
| How specified behavior is realized in code | Implementation Guides and the codebase |
| How a person works within the result | Developer Guides |
| The verification of specified behavior — Test Specifications, Test Plans, Test Cases, and other testing artifacts | Downstream documents, not governed by this Standard |

A statement in this document that redefines a product requirement, the Vision, a glossary term, the
documentation hierarchy, or a structural decision is a **defect in this document**. It MUST be
reported rather than acted upon.

> **Note on testing artifacts (non-normative).** This Standard governs behavioral Specifications
> only — documents stating what AEOS must do. Test Specifications, Test Plans, Test Cases, and any
> other artifact that verifies a behavioral Specification are downstream of it, in the same sense
> Implementation Guides are downstream of it. This Standard states no requirement for such artifacts,
> assigns them no identifier prefix, and takes no position on their form; it notes the boundary only,
> so that a future testing-artifact standard is understood to be a separate document rather than an
> extension of this one.

### 2.3 Applicability

This Standard applies to every document that constitutes or amends the Specification layer of AEOS,
identified by the `SP` identifier prefix under [Section 11](#11-identifier-conventions), created or
revised after this Standard is frozen. It applies identically to human and AI authors, as
AEOS-DOCSTD Section 2.4 requires of every AEOS document.

This Standard does not retroactively invalidate a Specification document frozen before this Standard
was frozen. Where such a document diverges from this Standard, the divergence is recorded as a Minor
finding and is reconciled only through that document's own change control, consistent with
AEOS-DOCSTD Section 2.3.

A Specification document's validity and authority derive from its Document ID and its recorded
lifecycle state, never from its location in the repository. Repository layout, directory structure,
packaging, or a future reorganization of the repository does not alter, suspend, or invalidate a
Specification document; only this Standard's own change control, under
[Section 18](#18-versioning-and-change-management), does. This Standard takes no position on
repository layout itself, which remains a decision for Blueprint or Implementation governance.

### 2.4 Relationship to the Documentation Hierarchy

AEOS-DOCSTD Section 4.1 places Specification below Blueprint and above Implementation Guides in the
documentation hierarchy, and AEOS-DOCSTD Section 13.1 records that the Specification layer changes
through **its own change control**, traceable to `PR-` identifiers. This Standard is that change
control. It does not add, remove, reorder, or rename a layer in the hierarchy AEOS-DOCSTD defines,
and a reading of this document that would do so is a defect to be reported under
[Section 20.5](#205-review-policy).

---

## 3. Relationship to Other Documents

### 3.1 Governing and Companion Documents

Five documents govern this one. This Standard introduces nothing that contradicts them and restates
nothing they already define; where it appears to, the entry below states the correct reading.

| Document | Governs | This Standard's obligation |
| :--- | :--- | :--- |
| **AEOS-VISION** | Purpose, philosophy, invariants. | No rule in this Standard may make a Specification document, or the behavior it states, trade away an invariant. A specified behavior that would require AEOS to perform inference, privilege a vendor, or weaken Human-in-the-Loop is excluded regardless of how precisely it could be specified. |
| **AEOS-PRD** | Product definition, capabilities, and the numbered `PR-` requirements. | Every Specification governed by this Standard traces to one or more `PR-` identifiers, as AEOS-PRD Section 25.6 already requires. This Standard states *how* that trace is recorded; it does not add, remove, or reinterpret a requirement. |
| **AEOS-GLOSSARY** | Terminology, including the official definition of *Specification* itself and the `SP-` identifier prefix. | This Standard uses every term — *Specification*, *Architecture*, *Blueprint*, *Implementation*, *Freeze*, *Repository Asset*, and others — exactly as AEOS-GLOSSARY defines them, and defines none of them again. Where this Standard needs a term the Glossary does not yet define, it names the gap rather than defining the term locally. |
| **AEOS-DOCSTD** | The form, structure, language, and lifecycle of every AEOS document. | This Standard's own structure, format, normative vocabulary, template, and review classification follow AEOS-DOCSTD without exception. This Standard narrows AEOS-DOCSTD's general rules to the Specification layer; it does not loosen, replace, or duplicate them. |
| **AEOS-TECH** | Which technologies AEOS officially recognizes. | A Specification governed by this Standard names no technology. Where a specified behavior depends on a recognized technology existing, the Specification states the behavioral dependency and this Standard's [Section 15](#15-dependency-rules) governs how that dependency is declared — never AEOS-TECH's own content. |

### 3.2 Relationship to Architecture and Blueprint

Architecture and Blueprint are frozen documents, identified as AEOS-ARCH and AEOS-BLUEPRINT
respectively. This Standard cites each by Document ID and by the `AR-` and `BP-` identifiers
AEOS-GLOSSARY Section 6.4 registers for architectural decisions and Blueprint items, in the same form
it cites AEOS-PRD's `PR-` identifiers:

| Layer | What it supplies to a Specification | What a Specification owes it in return |
| :--- | :--- | :--- |
| **AEOS-ARCH** | The structural decisions and boundaries — each carrying an `AR-` identifier — within which a specified behavior must fit. | A Specification MUST NOT contradict a structural decision or boundary AEOS-ARCH has made, and MUST remain implementation-independent in the same sense AEOS-ARCH is: it states what must be true, never how the structure realizing it is built. |
| **AEOS-BLUEPRINT** | The buildable arrangement — each item carrying a `BP-` identifier — decomposed to the level of detail a Specification can be written against, per AEOS-DOCSTD Section 4.3. | A Specification MUST be written against AEOS-BLUEPRINT's arrangement, citing the `BP-` items it relies on, and MUST NOT restate the arrangement itself — only the precise, testable behavior expected within it. |

Where a later revision of AEOS-ARCH or AEOS-BLUEPRINT changes a decision or an item a Specification
depends on, the affected Specification MUST be revised under
[Section 18](#18-versioning-and-change-management); this Standard states the obligation only, not the
content of either document.

### 3.3 Precedence

| Situation | Resolution |
| :--- | :--- |
| A rule in this Standard conflicts with an AEOS-VISION invariant | The invariant governs. The rule is removed and the conflict is recorded as a defect in this document. |
| A rule in this Standard conflicts with an AEOS-PRD requirement | AEOS-PRD governs. The rule is corrected; the requirement is not reinterpreted. |
| A rule in this Standard conflicts with AEOS-GLOSSARY on the meaning of a term | AEOS-GLOSSARY governs. The conflict is a defect in this document and is reported. |
| This Standard conflicts with AEOS-DOCSTD on documentation form | AEOS-DOCSTD governs. This Standard narrows AEOS-DOCSTD; it does not override it. |
| A Specification document conflicts with this Standard on Specification form | This Standard governs. The Specification document is corrected. |
| A Specification document conflicts with an Architecture or Blueprint decision it is written against | The Architecture or Blueprint decision governs. The Specification is corrected, or the conflict is escalated if the Specification believes the decision is wrong. |
| Two Specification documents conflict with one another | Escalate to the owner, per [Section 15](#15-dependency-rules). A Contributor MUST NOT resolve it by preference. |

---

## 4. Specification Principles

These principles constrain every AEOS Specification document and every rule in this Standard. A
Specification that violates one is defective, not merely unconventional.

| ID | Principle |
| :--- | :--- |
| `SS-P-01` | A Specification states behavior, never mechanism. |
| `SS-P-02` | One Specification document owns one behavior domain. |
| `SS-P-03` | Every specified rule traces to one or more `PR-` requirement identifiers. |
| `SS-P-04` | A Specification is a testable statement, not a description. |
| `SS-P-05` | A Specification MUST NOT weaken, reinterpret, or quietly widen a product requirement. |
| `SS-P-06` | A Specification remains vendor-neutral and technology-neutral. |
| `SS-P-07` | Specification identifiers are permanent. |
| `SS-P-08` | A Specification is reviewed and frozen before anything depends on it. |
| `SS-P-09` | Extension of a Specification is additive; frozen behavior is never silently altered. |
| `SS-P-10` | A Specification is written so that a test can be derived from it without further interpretation. |

### SS-P-01 — A Specification states behavior, never mechanism

A Specification answers *what must happen* — given a defined input and a defined condition, what
output or effect is required, and what is required when the condition is not met. It never answers
*how* that outcome is produced. [Section 9](#9-what-a-specification-must-not-define) states the
prohibited categories this principle produces.

### SS-P-02 — One Specification document owns one behavior domain

This is AEOS-DOCSTD `DS-P-06` applied to the Specification layer. A Specification document that
answers two behavior domains is two Specification documents that have not yet been separated.
[Section 6](#6-specification-ownership) states how a domain is assigned.

### SS-P-03 — Every specified rule traces to one or more `PR-` requirement identifiers

A specified rule with no trace is not a specification of AEOS; it is an unauthorized addition to the
product, however precisely it is written. [Section 13](#13-traceability-rules) states the trace
mechanics.

### SS-P-04 — A Specification is a testable statement, not a description

A sentence that cannot be reduced to a pass or fail condition is not yet specified. Where a behavior
resists this reduction, the resistance is itself reported — as an open question for Architecture or
Blueprint to resolve — rather than written around with descriptive language that reads as though it
were testable.

### SS-P-05 — A Specification MUST NOT weaken, reinterpret, or quietly widen a product requirement

This is AEOS-DOCSTD `H2` applied to the Specification layer. A Specification MAY add precision a
requirement did not state; it MUST NOT add scope, remove an obligation, or narrow who or what is
bound by it.

### SS-P-06 — A Specification remains vendor-neutral and technology-neutral

This is AEOS-TECH's neutrality commitment applied to the Specification layer: a Specification is
satisfiable by more than one technology choice. Where a specified behavior would be satisfiable by
exactly one recognized technology, that fact belongs to AEOS-TECH or to Implementation Guides, not
to the Specification.

### SS-P-07 — Specification identifiers are permanent

This is AEOS-GLOSSARY `I1` applied to the `SP` prefix. An identifier is never reused, renumbered, or
reassigned to a different rule. [Section 11](#11-identifier-conventions) states the full identifier
policy.

### SS-P-08 — A Specification is reviewed and frozen before anything depends on it

This is AEOS-DOCSTD `DS-P-04` applied to the Specification layer, with the added consequence that a
Specification which is depended upon before it is frozen has let an unstable statement become load-
bearing. [Section 19](#19-freeze-policy) states the freeze checklist.

### SS-P-09 — Extension of a Specification is additive; frozen behavior is never silently altered

This is AEOS-DOCSTD `E1`–`E3` applied to the Specification layer. A new need is met by a new
identifier or a new Specification document, never by editing what a frozen identifier already means.
[Section 16](#16-extension-rules) states the mechanics.

### SS-P-10 — A Specification is written so that a test can be derived from it without further interpretation

This is AEOS-DOCSTD's AI-readability commitment, sharpened for the audience that reads a
Specification most literally: a test author, human or AI, deriving a test case from the text alone.
A Specification a reader must ask a question about before testing is incomplete.

---

## 5. Purpose of Specifications

The Specification layer exists because Architecture and Blueprint cannot, and must not, answer the
question a test needs answered. Architecture states how AEOS is structured; Blueprint states how
that structure is arranged to be built; neither is precise enough to determine whether a given
implementation is correct, and neither should be, because precision at that level would freeze
implementation choices before they need to be made.

A Specification closes that gap for exactly one behavior domain at a time. It states, in testable
terms traceable to the product requirements that justify it, what must be true of the system's
observable behavior. It is the layer at which "how must each defined behavior work, precisely and
testably" — the question AEOS-PRD Section 3.2 assigns to Specification documents — is answered
completely.

| A Specification exists to | A Specification does not exist to |
| :--- | :--- |
| Make one behavior domain testable. | Explain why the behavior is wanted — that is Vision and Product Requirements. |
| Give Implementation Guides and code a fixed target to build and be verified against. | Decide how the target is reached — that is Implementation. |
| Give reviewers and AI runtimes a way to check a claim of correctness without guessing intent. | Decide how AEOS is structured — that is Architecture and Blueprint. |
| Give downstream documents and tests a stable identifier to trace to. | Persuade a reader that the behavior is a good idea — no AEOS document does that above Developer Guides. |

---

## 6. Specification Ownership

Ownership is stated at three distinct levels, and this section governs each differently. A
Specification document, the behavior domain it owns, and the Specification Rules — `SP-` identifiers
— it contains are not interchangeable, and treating them as such is a common source of scope drift.

| Level | What it is | Governed by |
| :--- | :--- | :--- |
| Specification document | The Repository Asset itself: one file, one metadata block, one lifecycle state. | AEOS-DOCSTD's document lifecycle, applied to this layer. |
| Behavior domain | The set of behavior a single `AREA` code names, owned by exactly one Specification document. | [Section 6.2](#62-assigning-a-domain). |
| Specification Rule | One `SP-<AREA>-<NNN>` identifier: a single testable statement within a behavior domain. | [Section 11](#11-identifier-conventions). |

A document's version changes, under [Section 18](#18-versioning-and-change-management), whenever a
Rule within it is added, retired, or changed; a Rule's meaning does not change merely because the
document that carries it is reissued at a new version. Confusing document-level governance with
rule-level governance — for example, treating a document version bump as a change to what a specific
Rule requires, or citing a document where a specific Rule identifier is meant — MUST be avoided.

### 6.1 The Ownership Rule

`SS-P-02` requires that a Specification document own exactly one behavior domain. A behavior domain
is the set of behavior that a single `AREA` code, as defined in [Section 11](#11-identifier-conventions),
identifies. A Specification document MUST declare the behavior domain it owns in its authority
statement, in the form AEOS-DOCSTD Appendix A requires of every document's opening material.

### 6.2 Assigning a Domain

A behavior domain is assigned when a Specification document first registers the `AREA` code that
names it, following the registration rule in [Section 11.4](#114-area-registration). Two
Specification documents MUST NOT register the same `AREA` code for different behavior domains, and a
single behavior domain MUST NOT be split across two Specification documents without retiring one of
them under [Section 16](#16-extension-rules).

### 6.3 Who May Own a Specification

A Specification document is owned, in the AEOS-DOCSTD metadata sense, by the Product Owner, AEOS, as
every AEOS document is. Authorship of the behavior domain — the party that drafts, defends, and
revises the specified rules — MAY be delegated, but delegation does not relax any obligation this
Standard states: a delegated author is still bound by every rule a Product Owner author would be
bound by.

### 6.4 Ownership Does Not Confer Authority Over Other Layers

Owning a behavior domain gives a Specification document no authority to alter the Architecture or
Blueprint decisions the behavior is written against, no authority to alter the `PR-` requirements it
traces to, and no authority to alter another Specification document's domain, even where the two
domains interact. Interaction between domains is stated through the dependency mechanism in
[Section 15](#15-dependency-rules), never through one Specification editing another's content.

---

## 7. Specification Responsibilities

### 7.1 The Responsibility Row

This row extends the responsibility table AEOS-DOCSTD Section 5.1 already states for the
Specification layer; it does not replace that table.

| Document | Owns | Answers | MUST NOT contain |
| :--- | :--- | :--- | :--- |
| **Specification** | Precise, testable behavioral rules for one behavior domain | RULES | Rationale owned above it, structural decisions, technology choices, interface surfaces, procedures, installation steps, and code |

### 7.2 The Responsibility Test

Before a statement is added to a Specification document, its author applies the test AEOS-DOCSTD
Section 5.3 states, specialized for this layer:

> **Is this statement a testable fact about required behavior, traceable to a `PR-` identifier, and
> free of mechanism?**
> If the answer is no on any count, the statement does not belong in a Specification document —
> however true, useful, or well written it is. A statement that answers *why* belongs above; a
> statement that answers *how it is built* belongs below.

### 7.3 Common Violations

| Violation | What it looks like | Correct form |
| :--- | :--- | :--- |
| **Borrowed rationale** | The Specification restates why a requirement exists, "so the reader has context." | Reference the `PR-` identifier and, where useful, the AEOS-PRD section. State the behavior; do not re-argue it. |
| **Anticipated implementation** | The Specification names a data structure, a class, a file, or a step-by-step procedure because the author already knows how it will be built. | State the observable outcome only. Record the mechanism in Implementation Guides, once that document exists for this behavior. |
| **Silent architecture** | The Specification states a structural boundary — which component does what — that no Architecture or Blueprint document has established. | Raise the missing structural decision to Architecture or Blueprint rather than deciding it inside the Specification. |

---

## 8. What a Specification MUST Define

A Specification document MUST define the following for the behavior domain it owns. Each item is
required; an item with nothing to state for a given domain is recorded as explicitly out of scope
under [Non-goals](#10-standard-specification-structure) rather than omitted silently, consistent with
AEOS-DOCSTD `DS-P-10`.

| # | A Specification MUST define |
| :--- | :--- |
| `MD1` | The precise conditions under which the behavior applies, stated in terms a test can evaluate. |
| `MD2` | The Inputs the behavior accepts, including their required properties and the conditions under which an input is valid or invalid. |
| `MD3` | The Outputs or effects the behavior produces for every valid input and every defined condition, including the observable state after the behavior completes. |
| `MD4` | The behavior for every error, boundary, and failure condition the domain admits — not only the success path. |
| `MD5` | The Constraints the behavior operates under, including any invariant that MUST hold before, during, and after the behavior. |
| `MD6` | The Extension Points at which the behavior is designed to admit future variation without altering what is already specified. |
| `MD7` | The trace from every specified rule to one or more `PR-` requirement identifiers. |
| `MD8` | The Non-goals of the document — behavior a reader might reasonably expect this domain to cover, which it deliberately does not. |
| `MD9` | Acceptance criteria sufficient for a reviewer or an automated test to determine, without further interpretation, whether an implementation satisfies the specified behavior. |

---

## 9. What a Specification MUST NOT Define

`SS-P-01` and `SS-P-06` prohibit a Specification from crossing into mechanism, technology, or
procedure. The categories below are the prohibitions this Standard treats as absolute; each is
paired with the specification-level statement that remains permitted, to make the boundary
checkable rather than a matter of taste.

| # | A Specification MUST NOT define | The behavioral statement that remains permitted |
| :--- | :--- | :--- |
| `MN1` | **Implementation.** No code, no class or module design, no file layout, no data structure. | The observable outcome the implementation must produce. |
| `MN2` | **Runtime algorithms.** No step-by-step procedure, no pseudocode, no execution order that is not itself part of the observable behavior. | The result the procedure must reach, and any ordering that is observably required (for example, "the failure is reported before any partial effect is retained"). |
| `MN3` | **Technologies.** No vendor, language, library, framework, or product name, and no reference that would be satisfiable by only one entry in AEOS-TECH. | A behavioral dependency, stated in the terms [Section 15](#15-dependency-rules) requires, that any technology satisfying the dependency may fulfill. |
| `MN4` | **APIs.** No endpoint name, method signature, request or response schema, protocol binding, or wire format. | What a defined operation must accept and produce, stated behaviorally, leaving the surface that exposes it to Blueprint and Implementation. |
| `MN5` | **Installation.** No setup, configuration, deployment, or environment-preparation step. | A precondition the behavior assumes to hold, stated as a fact about system state rather than a procedure to reach it. |
| `MN6` | **Rationale.** No restatement of why the behavior is wanted beyond the `PR-` trace. | A citation to the `PR-` identifier and, where useful, the AEOS-PRD section that states the rationale. |
| `MN7` | **Structural decisions.** No statement of which component, service, or module performs the behavior. | A statement that the behavior is required wherever it applies, independent of which structural element realizes it. |

> **The single-satisfiability test.** Where an author is uncertain whether a statement has crossed
> into a prohibited category, the test is: *can this requirement be satisfied in more than one
> technically reasonable way?* A requirement satisfiable by exactly one mechanism, one technology, or
> one literal interface is not a specification of behavior; it is a decision that belongs to a lower
> layer, stated too early. This is a non-normative aid to applying `MN1`–`MN7`, not an additional rule.

---

## 10. Standard Specification Structure

### 10.1 Required Sections

Every Specification document carries the universal skeleton AEOS-DOCSTD Appendix A requires of every
AEOS document — title block, metadata block, authority statement, conformance notice, table of
contents where more than five sections are present, and a Document Governance section as its final
numbered section. This section defines the subject-section composition specific to the Specification
layer: the content that occupies position 9, "Subject sections," in AEOS-DOCSTD Appendix B.1's
universal ordering.

| # | Section | Required content |
| :--- | :--- | :--- |
| `RS1` | **Purpose** | Why this specific behavior domain must be specified, in one to three paragraphs. Realizes the Executive Summary position AEOS-DOCSTD requires, named for the Specification layer's own vocabulary, consistent with the naming latitude AEOS-DOCSTD Appendix B.3 already grants the Vision layer. |
| `RS2` | **Scope** | The behavior domain covered, its boundary, and the `AREA` code registered for it. Realizes the Scope and Applicability position AEOS-DOCSTD requires. |
| `RS3` | **Responsibilities** | What this Specification is answerable for, applying `SS-P-02` to this document specifically. |
| `RS4` | **Inputs** | Every input the behavior accepts, its required properties, and validity conditions, per `MD2`. |
| `RS5` | **Outputs** | Every output or effect the behavior produces, per `MD3`. |
| `RS6` | **Behavior** | The normative rules stating what must happen for every defined condition, including error and boundary conditions, per `MD1` and `MD4`. |
| `RS7` | **Constraints** | The invariants and limits the behavior operates under, per `MD5`. |
| `RS8` | **Extension Points** | Where and how the behavior admits future variation without altering frozen rules, per `MD6` and [Section 16](#16-extension-rules). |
| `RS9` | **Traceability** | The trace from every rule in Behavior and Constraints to one or more `PR-` identifiers, per `MD7` and [Section 13](#13-traceability-rules). |
| `RS10` | **Non-goals** | Behavior a reader might expect this domain to cover, which it deliberately does not, per `MD8`. |
| `RS11` | **References** | Every document, section, and identifier this Specification cites, per [Section 14](#14-cross-reference-rules). |

### 10.2 Ordering

| # | Rule |
| :--- | :--- |
| `RS12` | The eleven sections in [Section 10.1](#101-required-sections) MUST appear in the order listed, following AEOS-DOCSTD Appendix B.2's rule that general content precedes specific content within a document. |
| `RS13` | A section with nothing to state for a given behavior domain MUST still appear, and MUST state that it is empty and why, consistent with AEOS-DOCSTD `DS-P-10`. A section MUST NOT be omitted silently. |
| `RS14` | Where a document reconciles this ordering against AEOS-DOCSTD Appendix B.3's general guidance for the Specification layer — "scope, then definitions referenced, then normative rules, then error conditions, then acceptance criteria" — the mapping is: Scope realizes *scope*; References realizes *definitions referenced*, moved to the closing position consistent with AEOS-DOCSTD `O4`; Behavior and Constraints realize *normative rules* and *error conditions*; Traceability realizes *acceptance criteria*. This Standard elaborates that guidance; it does not conflict with it. |

---

## 11. Identifier Conventions

### 11.1 The Registered Prefix

AEOS-GLOSSARY Section 6.4 already registers the identifier shape and the `SP` prefix for specified
behavior:

```text
        SP-<AREA>-<NNN>

        SP      registered layer prefix for specified behavior (AEOS-GLOSSARY)
        AREA    three uppercase letters naming the behavior domain
        NNN     three digits, zero-padded, allocated sequentially from 001
```

This Standard uses that prefix exactly as registered and introduces no other top-level prefix for
Specification content. AEOS-GLOSSARY `I4` reserves the introduction of a new layer prefix to
Glossary governance; a Specification Standard that invented `SPR-`, `SPI-`, or `SPC-` on its own
authority would both violate that rule and redefine terminology this Standard has no authority to
redefine.

### 11.2 Why No Sub-Prefixes Are Introduced

A finer-grained identifier — distinguishing, for example, a rule about Inputs from a rule about
Constraints — is unnecessary for the purpose a sub-prefix would serve. The section in which an
identifier appears already carries that distinction, per [Section 10](#10-standard-specification-structure),
and a rule does not change kind when it is extended. Splitting the prefix would fragment
traceability for no compensating gain: two prefixes tracing to the same `PR-` identifier are harder
to audit than one.

Where a future need for finer sub-classification is identified, it is recorded as a proposal against
AEOS-GLOSSARY under that document's own change control, per `I4`, and is not enacted by this
Standard.

### 11.3 Identifier Rules

| # | Rule |
| :--- | :--- |
| `ID1` | Every specified rule MUST carry an `SP-<AREA>-<NNN>` identifier, allocated sequentially within its `AREA` from `001`. |
| `ID2` | Identifiers MUST be immutable: never reused, never renumbered, and never reassigned to different intent, per AEOS-GLOSSARY `I1`. |
| `ID3` | A retired identifier MUST be marked retired in place, retaining its identifier and the reason for retirement, per AEOS-GLOSSARY `I2`. |
| `ID4` | Every `SP-` identifier MUST trace to one or more `PR-` identifiers, per AEOS-GLOSSARY `I5` and [Section 13](#13-traceability-rules). |
| `ID5` | An `AREA` code MUST be registered before it is used, per [Section 11.4](#114-area-registration), and MUST NOT be reused across behavior domains with a different meaning, per AEOS-GLOSSARY `I3`. |
| `ID6` | An identifier MUST NOT be assigned to a statement that fails the responsibility test in [Section 7.2](#72-the-responsibility-test). |
| `ID7` | Where a revision changes the context an existing `SP-` identifier depends on — for example, the set of `PR-` identifiers it traces to — the revision SHOULD preserve backward traceability wherever practical, such as by retaining a superseded trace alongside the new one rather than removing it outright. This is a recommendation only: it does not relax `ID2` or `ID3`, and it MUST NOT be read as permission to edit what a frozen identifier requires. |

### 11.4 Area Registration

An `AREA` code names one behavior domain, owned by exactly one Specification document per
[Section 6](#6-specification-ownership). A code is registered at the moment a Specification document
first uses it, by the document that introduces it, consistent with AEOS-GLOSSARY `I3`.

This Standard requires that a running, append-only registry of `AREA` codes exist and be consulted
before a new code is registered, so that two Specification documents cannot collide. This Standard
does not itself carry that registry: doing so would require this frozen document to change every
time a Specification document is added, which AEOS-DOCSTD `DS-P-12` counsels against. The registry's
location and storage form are a decision for Blueprint or Implementation governance; this Standard
requires only that it exist, that it be append-only, and that every Specification document record,
in its own metadata, the `AREA` code it registers or reuses.

Where a Specification domain corresponds directly to an AEOS-PRD capability, its `AREA` code SHOULD
match the corresponding `PR-` prefix's area segment (for example, a Specification for the
environment-management capability SHOULD register `SP-ENV`), so that the trace required by
[Section 13](#13-traceability-rules) is legible without consulting a separate index.

---

## 12. Normative Language

### 12.1 Adoption

AEOS-DOCSTD Section 7.1 adopts the key words of RFC 2119 as interpreted by RFC 8174 for every AEOS
document: the keywords carry their normative meaning only when written in all capitals. This Standard
adopts the same rule without modification.

### 12.2 Requirement for the Specification Layer

AEOS-DOCSTD Section 7.3 already states that normative language MUST be used in Specification
documents, because "a specification exists to state testable obligations" and "a specification
sentence without a keyword states nothing enforceable." This Standard restates that requirement here
only to make it locally discoverable; AEOS-DOCSTD governs it.

| # | Rule |
| :--- | :--- |
| `NL1` | Every normative sentence in a Specification document's Behavior, Constraints, and Extension Points sections MUST carry a keyword from AEOS-DOCSTD Section 7.2. |
| `NL2` | A Specification document MUST include the conformance notice AEOS-DOCSTD Section 7.5 requires, verbatim. |
| `NL3` | Every rule stated in [Section 8](#8-what-a-specification-must-define) MUST be independently checkable by a reviewer without consulting the author, per AEOS-DOCSTD Section 7.4, rule 2. |

---

## 13. Traceability Rules

### 13.1 The Trace Requirement

`SS-P-03`, AEOS-GLOSSARY `I5`, and AEOS-PRD Section 25.6 together establish one obligation, stated
here for the Specification layer's own use:

> Every `SP-` identifier MUST trace to one or more `PR-` identifiers. A specified rule with no trace
> is a defect and MUST be corrected before the document is frozen.

### 13.2 Trace Rules

| # | Rule |
| :--- | :--- |
| `TR1` | Each `SP-` rule MUST state the `PR-` identifier(s) it traces to, in its Traceability section, using the reference form [Section 14.2](#142-reference-forms) requires. |
| `TR2` | A trace MUST NOT be stated at the document level only; each individual rule carries its own trace, so that a partial change to the requirement set can be checked against a partial set of rules. |
| `TR3` | Where a `PR-` requirement is retired, every `SP-` rule that traced only to it MUST be retired in the same change, per [Section 16](#16-extension-rules), and MUST NOT be left tracing to a retired requirement without comment. |
| `TR4` | A Specification MUST NOT introduce a rule that has no corresponding `PR-` requirement, even where the rule appears self-evidently necessary. The gap is reported to AEOS-PRD governance instead. |
| `TR5` | Downstream documents — Implementation Guides, tests, issues, and pull requests — reference the `SP-` identifiers they realize or affect, consistent with the traceability chain AEOS-PRD Section 25.6 establishes for Architecture, Specification, and tests. |

---

## 14. Cross-Reference Rules

### 14.1 General Rule

Cross-referencing in Specification documents follows AEOS-DOCSTD Section 11.2 without modification.
This section states the reference forms specific to Specification content.

### 14.2 Reference Forms

| Reference to | Form |
| :--- | :--- |
| A product requirement | `PR-<AREA>-<NNN>` — for example, `PR-ENV-003`. |
| A specified rule in this document | `SP-<AREA>-<NNN>` — for example, `SP-ENV-014`. |
| A specified rule in another Specification document | The document's file name or Document ID, followed by the identifier — for example, "per the Environment Specification, `SP-ENV-014`." |
| A term | The term itself, spelled as AEOS-GLOSSARY spells it, linked where useful. |
| An Architecture decision | `AR-<AREA>-<NNN>`, per AEOS-ARCH — for example, `AR-PRN-001`. |
| A Blueprint item | `BP-<AREA>-<NNN>`, per AEOS-BLUEPRINT — for example, `BP-PRN-001`. |
| A section within the same document | An internal link to the section heading. |

### 14.3 Rules

| # | Rule |
| :--- | :--- |
| `XR1` | A reference MUST identify the owning document and the referenced item precisely enough to be found without searching, per AEOS-DOCSTD `T` rules in Section 11.2. |
| `XR2` | A reference MUST NOT be made by relative phrasing such as "the document above" or "as previously described." |
| `XR3` | A Specification MUST NOT quote a passage from another document in place of citing it; a brief, clearly marked summary is permitted where it orients the reader, per AEOS-DOCSTD Section 11.2. |
| `XR4` | A Specification MUST NOT restate a Glossary definition; it cites the term and, where useful, links to the entry. |
| `XR5` | A Specification MUST NOT restate another Specification's rule to avoid a cross-document reference; the reference is used instead. |

---

## 15. Dependency Rules

### 15.1 What a Dependency Is

A Specification document depends on another where a rule in its Behavior, Inputs, or Constraints
sections presumes that a rule owned by a different behavior domain already holds. A dependency is a
statement about behavior, never about implementation or technology, consistent with `MN3`.

### 15.2 Rules

| # | Rule |
| :--- | :--- |
| `DP1` | A dependency on another Specification's rule MUST be declared explicitly, by identifier, in the depending document's Inputs or Constraints section. |
| `DP2` | A dependency MUST NOT be implicit. A Behavior section that presumes an unstated precondition from another domain is incomplete, per `MD1`, and MUST be corrected. |
| `DP3` | Circular dependency between two Specification documents — each presuming a rule the other has not yet stated — MUST NOT exist. Where two domains appear mutually dependent, the shared behavior is factored into a third domain that both depend on. |
| `DP4` | A dependency MUST be stated behaviorally: what must already be true, not which document, technology, or component makes it true, consistent with `MN3` and `MN7`. |
| `DP5` | Where a Specification depends on a rule that is later retired under [Section 16](#16-extension-rules), the depending document MUST be revised in the same change or MUST record the resulting gap as an open finding; it MUST NOT be left silently depending on a retired rule. |
| `DP6` | A conflict between two Specification documents' stated dependencies is resolved as [Section 3.3](#33-precedence) states: escalated to the owner, never resolved unilaterally by either document's author. |

---

## 16. Extension Rules

### 16.1 Additive Extension

`SS-P-09` requires that extension of a Specification be additive. This section states the mechanics.

| # | Rule |
| :--- | :--- |
| `EX1` | A new behavior within an existing domain is added as a new `SP-<AREA>-<NNN>` identifier under that domain's registered `AREA` code, per [Section 11.4](#114-area-registration). It is never inserted by renumbering or reinterpreting an existing identifier. |
| `EX2` | A behavior extension that would change what an existing, frozen `SP-` identifier requires is a new identifier, not an edit to the old one. The old identifier is retired in place if it is superseded, per `ID3`, and the revision history records the reason. |
| `EX3` | An Extension Point declared under `RS8` MUST state, in behavioral terms, the boundary within which future variation is permitted without a new identifier, and the boundary beyond which a new identifier is required. An Extension Point with no stated boundary is incomplete. |
| `EX4` | A Specification MUST NOT declare an Extension Point that would allow a future addition to contradict `SS-P-05` — that is, no Extension Point may be used to smuggle a weakening of a product requirement in under the appearance of an addition. |
| `EX5` | Extending a Specification document to cover a new behavior domain, rather than adding to an existing one, requires a new Specification document and a new `AREA` code, per [Section 6.2](#62-assigning-a-domain); it is not achieved by widening an existing document's Scope section after the fact. |

### 16.2 Retirement

| # | Rule |
| :--- | :--- |
| `EX6` | Retiring a specified rule follows AEOS-DOCSTD `E3`: the identifier is marked retired in place, with its reason, and is never reused. |
| `EX7` | A rule MUST NOT be retired solely because the `PR-` requirement it traces to was refined, unless the refinement removed the obligation the rule stated. A refinement that only clarifies leaves the rule in force. |

---

## 17. Format Requirements

A Specification document is written in the format AEOS-DOCSTD Section 8 requires of every AEOS
document, without deviation: GitHub-Flavored Markdown, no raw HTML, standard constructs preferred
over markup, and rendering that requires no external asset. This Standard adds two rules specific to
the content a Specification document carries.

| # | Rule |
| :--- | :--- |
| `FM1` | A fenced code block in a Specification document MUST be marked as a non-normative illustration — for example, of a value's shape — and MUST NOT contain executable implementation code, consistent with `MN1`. Where a block could be read as an API surface, `MN4` governs and the block MUST be removed or rewritten in behavioral terms. |
| `FM2` | Inputs, Outputs, and Constraints SHOULD be expressed in Markdown tables wherever the items compared share a shape, per AEOS-DOCSTD `R4`, so that a reader is not required to hold several paragraphs in memory to compare them. |

---

## 18. Versioning and Change Management

### 18.1 Versioning of Specification Documents

AEOS-DOCSTD Section 13.3 states the default versioning rule for AEOS documents and defers to a
document's own change control where one exists. This is that change control for the Specification
layer.

| Increment | When |
| :--- | :--- |
| **Patch** | Editorial correction with no change to a specified rule's meaning or trace. |
| **Minor** | Addition of a new `SP-` identifier under an existing `AREA` code that does not alter what any existing identifier requires. |
| **Major** | Any change to what an existing `SP-` identifier requires; retirement of an identifier; a change to the document's `AREA` code, ownership, or declared behavior domain; or any change that would invalidate a downstream Implementation Guide or test written against the prior version. |

### 18.2 Change Control for This Standard

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Clarification of an existing rule that does not invalidate a Specification document already conformant to it | Owner approval | Minor |
| Addition of a rule, a required section, or a rule table | Explicit owner revision request | Major |
| Change to what a Specification MUST or MUST NOT define | Explicit owner revision request with recorded rationale | Major |
| Change to the identifier convention, the `SP` prefix, or the shape `SP-<AREA>-<NNN>` | Explicit owner revision request, coordinated with AEOS-GLOSSARY governance under its `I4` | Major |
| Change affecting the responsibility boundary between Specification and Architecture, Blueprint, or Implementation | Explicit owner revision request with recorded rationale, coordinated with the affected layer's own governance | Major |

### 18.3 Cascading Change

| # | Rule |
| :--- | :--- |
| `CM1` | A Major change to this Standard does not automatically invalidate a Specification document frozen under a prior version. AEOS-DOCSTD Section 2.3's transition rule applies: the divergence is recorded as a Minor finding against the older document, reconciled at that document's own pace. |
| `CM2` | A Major change to a `PR-` requirement that a `SP-` rule traces to requires the owning Specification document to be revised in the same change window, per `TR3`; it MUST NOT be left tracing silently to a requirement that no longer says what it once said. |

---

## 19. Freeze Policy

### 19.1 Lifecycle

A Specification document follows the lifecycle states and stages AEOS-DOCSTD Section 12 defines for
every AEOS document, without modification. This section states the checklist specific to
Specification content that a reviewer applies before recommending Stage 5, Freeze.

### 19.2 Freeze Checklist

- [ ] Every rule in Behavior, Constraints, and Extension Points carries an `SP-<AREA>-<NNN>` identifier.
- [ ] Every identifier traces to one or more `PR-` identifiers, per [Section 13](#13-traceability-rules).
- [ ] No rule falls into a category `MN1`–`MN7` prohibits.
- [ ] All eleven sections in [Section 10.1](#101-required-sections) are present, in order, and none is silently empty.
- [ ] Every cross-reference resolves and follows the forms in [Section 14.2](#142-reference-forms).
- [ ] Every declared dependency is explicit and non-circular, per [Section 15](#15-dependency-rules).
- [ ] Every Extension Point states its boundary, per `EX3`.
- [ ] The document's `AREA` code is registered and unique, per [Section 11.4](#114-area-registration).
- [ ] No Critical or Major finding remains open, per AEOS-DOCSTD Section 12.3.

### 19.3 What a Freeze Means

A frozen Specification document is authoritative for the behavior domain it owns. AEOS-DOCSTD
Section 12.5 governs what that means; this Standard adds only that a frozen Specification is the
document Implementation Guides, tests, and code are written and verified against, and that a defect
discovered in a frozen Specification does not authorize a silent local fix in the Implementation —
it is reported and resolved at the Specification, under [Section 18](#18-versioning-and-change-management).

---

## 20. Document Governance

### 20.1 Status

This document is the **Specification Source of Truth** for the AEOS repository. It is intended to be
frozen as part of AEOS 1.0 and to remain stable across the life of the product. Every Specification
document references it instead of defining its own form, structure, or identifier convention.

### 20.2 Change Control

Change control for this document itself is stated in [Section 18.2](#182-change-control-for-this-standard).

### 20.3 Identifier Policy

The `SS-P-` principle identifiers and the section-scoped rule identifiers (`MD`, `MN`, `RS`, `ID`,
`NL`, `TR`, `XR`, `DP`, `EX`, `FM`, `CM`) defined in this document are permanent. They are never
reused, renumbered, or repurposed. A retired rule is marked retired in place, retaining its
identifier and its rationale, per AEOS-DOCSTD `E3`.

### 20.4 Relationship to the Architecture Freeze

This document introduces no architecture, no requirement, and no capability. Ideas arising from it
that would alter the documentation hierarchy, a product requirement, or a glossary term are recorded
as recommendations for the owning document's governance and are applied only after explicit owner
approval there — never enacted here.

### 20.5 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4.

### 20.6 Traceability

| Layer | Obligation |
| :--- | :--- |
| Specification documents | Every specified rule traces to one or more `PR-` identifiers, per [Section 13](#13-traceability-rules). |
| Implementation Guides | Every guide traces to the `SP-` identifiers it realizes. |
| Tests | Every `SP-` rule for a `P0` requirement is covered by at least one test written before its implementation, consistent with AEOS-PRD Section 25.6. |
| Issues and pull requests | Reference the `SP-` identifiers they advance. |

### 20.7 Precedence

Precedence between this document and every document above it is stated in
[Section 3.3](#33-precedence). Precedence between this document and a Specification document it
governs is: this document governs on matters of form, identifier convention, traceability, and
lifecycle; the Specification document governs on the content of the behavior domain it owns.

### 20.8 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Freeze candidate | Initial Specification Writing Standard. Establishes ten specification principles, the relationship of the Specification layer to Vision, Product Requirements, Glossary, Document Standard, Supported Technologies, Architecture, and Blueprint, specification ownership rules, the responsibility boundary of the Specification layer, nine required-content rules and seven prohibited-content rules with a single-satisfiability test, an eleven-section standard specification structure with ordering rules, the identifier convention built on the existing `SP-<AREA>-<NNN>` shape with an explicit decision not to introduce new top-level prefixes, normative language rules, traceability rules, cross-reference rules, dependency rules, extension and retirement rules, format requirements, a Specification-layer versioning and change-control table, a freeze checklist, and document governance. Introduces no product requirement, no vision, no terminology, no architecture, no technology, and no implementation. Redefines no hierarchy AEOS-DOCSTD establishes. |
| 1.1.0 | Freeze candidate | Governance refinement pass preceding the AEOS 1.0 Architecture Freeze. Added a non-normative distinction between a Specification document, the behavior domain it owns, and a Specification Rule, as an introduction to [Section 6](#6-specification-ownership). Added a row and a non-normative note to [Section 2.2](#22-what-this-document-does-not-govern) clarifying that this Standard governs behavioral Specifications only and that Test Specifications, Test Plans, Test Cases, and other testing artifacts are downstream documents outside its scope; no testing governance is introduced. Added a statement to [Section 2.3](#23-applicability) that a Specification document's validity derives from its Document ID and lifecycle state, not from repository layout, directory structure, packaging, or reorganization. Added rule `ID7` to [Section 11.3](#113-identifier-rules), a SHOULD-level recommendation that revisions preserve backward traceability wherever practical; `ID2` and `ID3` are unchanged and identifier immutability is not weakened. Updated the Appendix C rule index accordingly. No principle, responsibility, ownership boundary, required or prohibited content rule, identifier prefix, structural section, traceability obligation, or precedence rule was changed. |

---

## Appendix A — Specification Template

**A.1 is non-normative. A.2 is normative.**

### A.1 Template

```markdown
# AI Engineering Operating System

## AEOS — <Behavior Domain> Specification

*<One-line statement of the behavior this document specifies.>*

| Field | Value |                          <!-- required: metadata block, per AEOS-DOCSTD -->
| :--- | :--- |
| **Document** | <Behavior Domain> Specification |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | <SPEC-ID> |
| **Version** | <x.y.z> |
| **Status** | <lifecycle state> |
| **Owner** | Product Owner, AEOS |
| **Author** | <author role> |
| **Audience** | Architects, implementers, reviewers, test authors, and AI runtimes |
| **Suggested path** | `docs/specification/<NAME>.md` |
| **Companion documents** | AEOS-PRD · AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-SPECSTD |
| **Supersedes** | <document ID and version, or None> |
| **Area code** | `<AREA>` |

> **Authority of this document.**                <!-- required -->
> This document specifies, precisely and testably, the <behavior domain> behavior of AEOS.
> It defines no rationale, no structure, no technology, no interface surface, and no implementation.

> The key words MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY ...   <!-- required, AEOS-DOCSTD 7.5 -->

---

## Table of Contents

---

## 1. Purpose                                    <!-- RS1 -->
## 2. Scope                                       <!-- RS2 -->
## 3. Responsibilities                             <!-- RS3 -->
## 4. Inputs                                        <!-- RS4 -->
## 5. Outputs                                        <!-- RS5 -->
## 6. Behavior                                        <!-- RS6 -->
## 7. Constraints                                       <!-- RS7 -->
## 8. Extension Points                                    <!-- RS8 -->
## 9. Traceability                                          <!-- RS9 -->
## 10. Non-goals                                              <!-- RS10 -->
## 11. References                                              <!-- RS11 -->

## 12. Document Governance                          <!-- required, AEOS-DOCSTD -->
Status · Change control · Review policy · Precedence · Revision history

---

**End of <Behavior Domain> Specification**

<SPEC-ID> · Version <x.y.z> · Traces to <PR- identifiers>
```

### A.2 Template Rules

| # | Rule |
| :--- | :--- |
| `TP-S1` | Every Specification document MUST include the metadata block, the authority statement, and the conformance notice, per AEOS-DOCSTD `TP1` and `TP2`, plus the `Area code` field this Standard adds. |
| `TP-S2` | The eleven sections in [Section 10.1](#101-required-sections) MUST appear, in order, before Document Governance. |
| `TP-S3` | A section that would be empty MUST instead state that it is empty and why, per `RS13`. |
| `TP-S4` | The closing block MUST state the `PR-` identifiers the document traces to, in addition to the document ID and version AEOS-DOCSTD requires of every closing block. |

---

## Appendix B — Recommended Specification Section Ordering

**Non-normative.** This appendix restates [Section 10](#10-standard-specification-structure) in the
form of AEOS-DOCSTD Appendix B, for a reader comparing this Standard directly against that one.

| # | Section | AEOS-DOCSTD universal position realized |
| :--- | :--- | :--- |
| 1 | Title block | Position 1 |
| 2 | Metadata block | Position 2 |
| 3 | Authority statement | Position 3 |
| 4 | Conformance notice | Position 4 |
| 5 | Table of contents | Position 5 |
| 6 | Purpose | Position 6, Executive Summary |
| 7 | Scope | Position 7, Scope and Applicability |
| 8 | Responsibilities, Inputs, Outputs, Behavior, Constraints, Extension Points | Position 9, Subject sections |
| 9 | Traceability, Non-goals | Position 10, Constraints and boundaries |
| 10 | References | Position 9, carried last within Subject sections per `RS14` |
| 11 | Document Governance | Position 12 |
| 12 | Closing block | Position 14 |

---

## Appendix C — Rule Index

**Non-normative.** Indexes the rule identifiers stated in the document body.

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `SS-P-01` to `SS-P-10` | Specification principles | 10 | [4](#4-specification-principles) |
| `MD1` to `MD9` | What a Specification MUST define | 9 | [8](#8-what-a-specification-must-define) |
| `MN1` to `MN7` | What a Specification MUST NOT define | 7 | [9](#9-what-a-specification-must-not-define) |
| `RS1` to `RS14` | Standard Specification structure and ordering | 14 | [10](#10-standard-specification-structure) |
| `ID1` to `ID7` | Identifier conventions | 7 | [11.3](#113-identifier-rules) |
| `NL1` to `NL3` | Normative language | 3 | [12.2](#122-requirement-for-the-specification-layer) |
| `TR1` to `TR5` | Traceability rules | 5 | [13.2](#132-trace-rules) |
| `XR1` to `XR5` | Cross-reference rules | 5 | [14.3](#143-rules) |
| `DP1` to `DP6` | Dependency rules | 6 | [15.2](#152-rules) |
| `EX1` to `EX7` | Extension and retirement rules | 7 | [16](#16-extension-rules) |
| `FM1` to `FM2` | Format requirements | 2 | [17](#17-format-requirements) |
| `CM1` to `CM2` | Cascading change rules | 2 | [18.3](#183-cascading-change) |
| `TP-S1` to `TP-S4` | Specification template rules | 4 | [Appendix A.2](#a2-template-rules) |

---

**End of Specification Standard**

AEOS-SPECSTD · Version 1.1.0 · Specification Source of Truth
