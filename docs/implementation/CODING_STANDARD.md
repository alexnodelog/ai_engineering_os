# AI Engineering Operating System

## AEOS — Coding Standard

*The permanent statement of the repository-wide coding principles that govern AEOS's own
Implementation.*

| Field | Value |
| :--- | :--- |
| **Document** | Coding Standard |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-CODESTD |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Engineering Governance Board, AEOS |
| **Audience** | Contributors, engineering reviewers, maintainers, and AI runtimes authoring or reviewing AEOS's own Implementation |
| **Suggested path** | `docs/implementation/CODING_STANDARD.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states the **repository-wide coding principles that govern AEOS's own
> Implementation**: the engineering rules, expressed at the level of principle rather than
> mechanism, that every unit of code and test realizing the Architecture, Blueprint, and
> Specification layers MUST follow, regardless of the programming language, framework, or tool used
> to write it.
>
> This document is an **Implementation Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that
> layer: concrete guidance for building to specification, expressed at the point specification meets
> construction. It is not a Product document, not a Vision document, not an Architecture document,
> not a Blueprint, not a Runtime document, and not a behavioral Specification under AEOS-SPECSTD. It
> states no product requirement, no architectural decision, no Blueprint arrangement, no specified
> behavior, no runtime lifecycle, and no terminology; where a statement here appears to do any of
> these, that is a defect in this document and MUST be reported rather than acted upon.
>
> This document is also **not** a Language Style Guide, **not** a Formatter configuration, **not** a
> Linter configuration, and **not** an Implementation Specification. [Section 2.3](#23-distinguishing-this-standard-from-adjacent-artifacts)
> distinguishes each of those from this document precisely, because each answers a different
> question and none of them is authored here. This document names no programming language, no
> framework, no library, and no technology choice beyond what AEOS-TECH already recognizes; where a
> principle below can be satisfied in only one technical way, that is a defect in this document.
>
> AEOS-DOCSTD Section 4.1 positions Implementation Guides beneath Specification and above Developer
> Guides in the documentation hierarchy. This document is written under AEOS-DOCSTD's general
> document template and the Section 4.3 purpose statement for this layer, in the absence of a
> dedicated Implementation Guide Standard — in the same spirit `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) and
> `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) record for their own comparable position. It does not, on
> that account, establish such a Standard; a future Implementation Guide Standard remains reserved to
> the owner under AEOS-DOCSTD `H5`.
>
> Where this document and a document of higher authority both speak to a subject, the
> higher-authority document governs and any conflict here is a defect to be reported rather than
> acted upon.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Coding Philosophy](#3-coding-philosophy)
4. [Readability Principles](#4-readability-principles)
5. [Maintainability Principles](#5-maintainability-principles)
6. [Modularity Principles](#6-modularity-principles)
7. [Naming Principles](#7-naming-principles)
8. [File Organization Principles](#8-file-organization-principles)
9. [Error Handling Principles](#9-error-handling-principles)
10. [Logging Principles](#10-logging-principles)
11. [Documentation Expectations](#11-documentation-expectations)
12. [Dependency Management Principles](#12-dependency-management-principles)
13. [Security-Related Coding Principles](#13-security-related-coding-principles)
14. [AI-Assisted Coding Expectations](#14-ai-assisted-coding-expectations)
15. [Human Review Expectations](#15-human-review-expectations)
16. [Non-Goals](#16-non-goals)
17. [Traceability](#17-traceability)
18. [References](#18-references)
19. [Document Governance](#19-document-governance)
20. [Appendix A — CS Rule Index](#appendix-a--cs-rule-index)
21. [Appendix B — Coding Standard Review Checklist (Non-Normative)](#appendix-b--coding-standard-review-checklist-non-normative)

---

## 1. Executive Summary

A repository that states what AEOS is, how it is structured, and how each behavior must work still
leaves one question unanswered: once a Contributor sits down to write the code that realizes all of
that, what engineering discipline binds the writing itself? Without an answer, that discipline
exists only where each Contributor happens to carry it — in a habit, a preference, a convention
picked up elsewhere — and AEOS-VISION `V5`'s promise, that the repository is the product and remains
meaningful to whoever arrives next, quietly stops covering the code that realizes it.

This document closes that gap, once, at the level a durable engineering standard can hold: as
principle rather than mechanism. It states what AEOS's own Implementation MUST favor and MUST avoid
— in readability, maintainability, modularity, naming, file organization, error handling, logging,
code-level documentation, dependency management, security, AI-assisted authorship, and human review
— without naming a language, a framework, a formatter, or a tool to enforce any of it. A future
Contributor, human or AI, reads this document once and applies it to whatever AEOS's own
Implementation is written in, on the day it is written in it.

Four properties bind the principles this document states:

| Property | What it requires of this document |
| :--- | :--- |
| **Runtime-neutral** | No principle depends on which AI Runtime a Contributor used to produce or review the code, consistent with AEOS-PRD `PR-RUN-002` and `PR-RUN-006`. |
| **Language-neutral** | No principle can be satisfied in only one programming language; where a principle would require naming one, it is stated as an outcome instead, or omitted. |
| **Vendor-neutral** | No principle names a supplier, product, or brand; where a category of technology is relevant, this document refers to the category AEOS-TECH already recognizes, never to one entry within it. |
| **Platform-neutral** | No principle depends on Windows, macOS, or Linux behaving differently, consistent with AEOS-PRD `PR-PLT-003` and `PR-PLT-005`. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the engineering principles that bind AEOS's own Implementation — the code and
tests that realize the Architecture, Blueprint, and Specification layers, in the sense AEOS-GLOSSARY's
*Implementation* entry fixes that term:

- the coding philosophy AEOS's own Implementation is written under;
- the readability, maintainability, and modularity a unit of code MUST exhibit;
- the naming and file-organization principles that apply to it;
- the error-handling and logging discipline it follows;
- the expectations placed on documentation that lives inside the code itself;
- the principles governing the dependencies it takes on;
- the security-related coding principles it follows;
- the expectations placed on AI-assisted authorship of it;
- the expectations placed on its human review before it is merged.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, and frozen | AEOS-DOCSTD |
| Which technologies AEOS recognizes and at what tier | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH |
| How that structure is arranged to be built | AEOS-BLUEPRINT |
| What observable behavior a defined unit of Implementation MUST produce | Specification documents, under AEOS-SPECSTD |
| Where AEOS's own source and test code is placed on disk | A future Implementation Guide or Blueprint-derived placement decision, per `REPOSITORY_LAYOUT.md` non-goal `NG-1` |
| The engineering constraints a Developer's own Project applies during generation, review, and refactoring | That Project's own Rules, under AEOS-PRD `PR-RUL` |

A statement in this document that grants a capability, imposes a product requirement, defines a
term, decides a structure, or specifies observable behavior is a **defect in this document**. It
MUST be reported rather than acted upon.

### 2.3 Distinguishing This Standard From Adjacent Artifacts

Five kinds of artifact are easily confused with one another because each constrains how code looks
or behaves. Each answers a different question, and this document is exactly one of them.

| Artifact | Question it answers | Form | This document's relationship to it |
| :--- | :--- | :--- | :--- |
| **Coding Standard** (this document) | What engineering principle, independent of language or tool, MUST code in AEOS's own Implementation follow? | A frozen, technology-neutral document | — |
| **Language Style Guide** | What syntactic form does code in one specific language take — indentation, brace placement, import ordering, line length? | A language-specific document, adopted once AEOS's own Implementation language is fixed | Not authored here. This document states no syntax rule and can be satisfied in any language. |
| **Formatter** | What tool, run automatically, rewrites code into one canonical syntactic form? | Technology-specific tool configuration | Not named or configured here. A formatter MAY be adopted to enforce a future Language Style Guide; it enforces no principle this document states directly. |
| **Linter** | What tool, run automatically, flags code against a configurable rule set? | Technology-specific tool configuration | Not named or configured here. A linter MAY be configured so that some of this document's principles are checked automatically; the configuration itself is never this document. |
| **Implementation Specification** | What observable behavior MUST a defined unit of AEOS's own Implementation produce, precisely and testably? | A Specification document under AEOS-SPECSTD | Not restated here. This document governs how code is written; it never states what a unit of code must do. The single-satisfiability test AEOS-SPECSTD Section 9 states for behavioral statements applies equally here: a principle satisfiable in only one technical way has crossed into a different artifact and is a defect in this document. |

### 2.4 Applicability

This document binds AEOS's own Implementation, as AEOS-GLOSSARY's *Implementation* entry defines
that term, and every Contributor — human or AI runtime — who authors, modifies, or reviews it,
consistent with AEOS-DOCSTD Section 2.4's rule that a Standard applies identically to human and AI
authors.

This document does not bind *project code*, in the sense AEOS-GLOSSARY's *Implementation* entry
distinguishes that term: code a Developer writes in their own Project remains governed by that
Project's own Rules, under AEOS-PRD `PR-RUL`, which a Developer adds, modifies, and removes without
modifying AEOS itself. A Developer MAY adopt some or all of the principles this document states as
Rules within their own Project; doing so is the Developer's own choice, made through their own
Project's Rule management, and confers on this document no authority over that Project.

This document is not itself a Rule in the sense AEOS-GLOSSARY's *Rule* entry defines that term — a
versioned, scoped engineering constraint applied during generation, review, and refactoring within a
Project. It is a frozen Implementation Guide. Where AEOS's own Implementation is later governed
through Rules of its own, those Rules are expected to draw on the principles stated here; this
document does not, by that expectation, become a Rule itself.

---

## 3. Coding Philosophy

The principles in this section state the conviction each later section applies to one concern. They
are foundational to the rest of this document and are not, on their own, exhaustive rules; the
sections that follow state the rules each conviction implies.

| ID | Principle | Traces to |
| :--- | :--- | :--- |
| `CS-001` | **Clarity over cleverness.** Where two implementations are otherwise equivalent, AEOS's own Implementation MUST favor the plainer one. A solution understood only by its author is a liability, not an achievement. | AEOS-VISION §6.14 · `PR-NFR-006` |
| `CS-002` | **Explicit over implicit.** Behavior that matters MUST be stated in code, not left to an unstated convention, an inferred default, or a pattern the reader is expected to already know. | AEOS-VISION §6.15 |
| `CS-003` | **The lifetime of the product is the correct time horizon.** An engineering decision in AEOS's own Implementation MUST be evaluated against whether it will still make sense to a maintainer who did not make it, not only against the task that prompted it. | AEOS-VISION §6.16 |
| `CS-004` | **Verification precedes implementation.** AEOS's own Implementation is developed under the TDD Cycle without exemption: a stated, confirmed behavior and a failing test precede the code that satisfies it. | AEOS-VISION `V4` · `PR-TDD-001` through `PR-TDD-012` |
| `CS-005` | **One artifact serves two audiences.** Code is written to be intelligible to a human maintainer and consumable by an AI runtime from the same source, without a separate machine-oriented form. | `PR-NFR-009` |

---

## 4. Readability Principles

A reader of AEOS's own Implementation is assumed to be competent, short of time, and encountering
the code without the context its author held while writing it — the same reader AEOS-DOCSTD Section
6.3 assumes for documentation, applied here to code.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-006` | The name of a function, method, or variable MUST state what it does or represents; a reader MUST NOT need to read its body to learn what its name already claims to say. | `CS-002` |
| `CS-007` | A unit of code (a function, method, or the language's nearest equivalent) MUST do one thing. Where it does more than one, it MUST be decomposed into units that each do one. | `CS-001` |
| `CS-008` | Nesting depth and branching complexity SHOULD be minimized; where a reader following one execution path would need to hold more than a small number of open conditions in mind at once, the code SHOULD be restructured. | `CS-001` |
| `CS-009` | A comment MUST explain something the code itself cannot state — why a choice was made, what a non-obvious constraint requires — and MUST NOT restate what the code already says in its own terms. | `CS-002` |
| `CS-010` | A literal value whose meaning is not self-evident MUST be named once and referenced, rather than repeated inline at each point of use. | `CS-001` |

## 5. Maintainability Principles

Maintainability is measured against the reader who inherits the code, not the author who wrote it,
consistent with `CS-003`.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-011` | Logic already expressed elsewhere in AEOS's own Implementation MUST NOT be duplicated; duplicated logic is factored into one place and referenced from each point that needs it. | `PR-NFR-006` |
| `CS-012` | A change to behavior MUST be accompanied, in the same change, by the test that demonstrates it, consistent with the TDD Cycle `CS-004` states. | `PR-TDD-001` · `PR-TDD-003` |
| `CS-013` | AEOS's own Implementation MUST NOT depend on undocumented behavior of a dependency, an AI Runtime, or a Model. | `PR-RUN-006` · AEOS-GLOSSARY *Model* |
| `CS-014` | A workaround for a defect outside AEOS's own Implementation MUST be isolated from surrounding code and MUST record the reason it exists, so that it can be found and removed once the external defect is resolved. | `CS-003` |

## 6. Modularity Principles

Architectural structure is Architecture's exclusive subject; this section states no structural
decision of its own and instead states how code realizing an already-decided structure stays easy to
extend.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-015` | Where AEOS's own Implementation realizes a layer AEOS-ARCH defines, its module boundaries MUST follow that layer's boundaries. This document states no additional structural decision, and a code module boundary that contradicts AEOS-ARCH is a defect in the code, not a license granted by this document. | AEOS-ARCH |
| `CS-016` | A module MUST depend only on what its responsibility requires; a dependency introduced for convenience rather than necessity MUST be removed or justified in the change that introduces it. | `PR-NFR-006` |
| `CS-017` | AEOS's own Implementation is extended by addition, not by modification of what already works, consistent with AEOS-VISION invariant `V10` and the extensibility every Repository Asset kind already commits to. | AEOS-VISION `V10` · `PR-NFR-007` |
| `CS-018` | A module's public surface MUST be limited to what its dependents actually require; internal detail MUST NOT be exposed for the convenience of a caller that does not need it. | `CS-016` |

## 7. Naming Principles

AEOS-GLOSSARY Section 6 governs how AEOS names its own documents, Repository Assets, and
identifiers; it does not govern the identifiers a Contributor chooses inside AEOS's own
Implementation's source code, which is a distinct subject this section addresses without
duplicating that Section.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-019` | A code identifier MUST state what the thing it names is or does, never how it is built or how it happens to be used at one call site, mirroring the principle AEOS-GLOSSARY `N1` states for AEOS's own naming. | AEOS-GLOSSARY `N1` |
| `CS-020` | Where code, or a comment within it, refers to a concept AEOS-GLOSSARY defines, it MUST use the Glossary's official name for that concept and MUST NOT introduce an alternative wording for the same concept. An identifier MAY depart from the Glossary's capitalization only where the language's own conventions require it, and the departure MUST NOT introduce a second name for the concept. | AEOS-GLOSSARY `T1` · `T3` |
| `CS-021` | An abbreviation MUST NOT be invented where the full term remains clear within the line length the code otherwise permits; where an abbreviation is used, it MUST be applied consistently throughout AEOS's own Implementation. | `CS-002` |
| `CS-022` | English MUST be the language of every identifier in AEOS's own Implementation, consistent with the rule AEOS-GLOSSARY `N6` states for AEOS's own naming. | AEOS-GLOSSARY `N6` |

## 8. File Organization Principles

This document fixes no top-level source or test directory name for AEOS's own Implementation; that
placement decision is reserved elsewhere, as [Section 2.2](#22-what-this-document-does-not-govern)
states. The principles below apply once a location is chosen.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-023` | A file's location and name MUST reflect the responsibility of the code it contains, not the order in which the code was written or the convenience of the moment it was added. | `CS-019` |
| `CS-024` | A file MUST NOT mix responsibilities that belong to different layers AEOS-ARCH defines; where a file would need to, it is split along the layer boundary instead. | `CS-015` |
| `CS-025` | Test code MUST be organized so that a reader can locate the test for a given unit of Implementation without consulting a separate index. | `PR-TDD-007` |
| `CS-026` | This document does not fix a top-level source or test directory name for AEOS's own Implementation. That decision belongs to a future Implementation Guide or Blueprint-derived placement decision, consistent with `REPOSITORY_LAYOUT.md` non-goal `NG-1`. | `REPOSITORY_LAYOUT.md` `NG-1` |

## 9. Error Handling Principles

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-027` | AEOS's own Implementation fails closed: where it encounters a condition it has not verified it can handle correctly, it MUST stop and report rather than proceed on an assumption. | AEOS-VISION `V7` · `PR-SAF-002` |
| `CS-028` | An error MUST be reported with enough detail for the party that receives it to act on it, consistent with the reporting `PR-WFL-011` and `PR-TDD-010` already require of AEOS's workflow and test behavior. | `PR-WFL-010` · `PR-WFL-011` · `PR-TDD-010` |
| `CS-029` | An interruption at any point in AEOS's own Implementation MUST leave the state it was acting on consistent and describable, consistent with `PR-SAF-010`. | `PR-SAF-010` |
| `CS-030` | An error condition MUST NOT be silently discarded. Where code observes an error it does not handle, it MUST propagate the error rather than suppress it. | `CS-027` |
| `CS-031` | A destructive or irreversible effect MUST NOT be taken as a side effect of error recovery; recovery from an error remains subject to the same Approval Gate as the action that produced the error, consistent with `PR-SAF-003` and `PR-SAF-004`. | `PR-SAF-003` · `PR-SAF-004` |

## 10. Logging Principles

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-032` | A log entry MUST distinguish an observed fact from an inference, consistent with `PR-SAF-011` and AEOS-VISION `V9`. | `PR-SAF-011` · AEOS-VISION `V9` |
| `CS-033` | A log entry MUST NOT contain a credential or a secret, consistent with `PR-SAF-006` and `PR-REP-013`. | `PR-SAF-006` · `PR-REP-013` |
| `CS-034` | Logging in AEOS's own Implementation supports Transparency and the disclosure `PR-SAF-008` requires before content leaves the machine; it is not a substitute for the Explain, Propose, and Report steps `PR-WFL-005` requires. | `PR-NFR-001` · `PR-SAF-008` · `PR-WFL-005` |
| `CS-035` | Log output MUST remain meaningful as plain, structured text without a proprietary viewer, consistent with the plain-text durability `PR-REP-016` requires of Repository Assets generally. | `PR-REP-016` |

## 11. Documentation Expectations

This section governs documentation that lives inside AEOS's own source and test code — comments,
docstrings, and equivalent inline material. It does not govern AEOS's own Markdown documentation,
which remains the exclusive subject of AEOS-DOCSTD, and it does not govern documentation AEOS
generates for a Developer's Project, which remains the exclusive subject of AEOS-PRD `PR-DOC`.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-036` | Every unit of AEOS's own Implementation that other code depends on MUST state, at the point its interface is declared, what it does and why it exists. | `CS-005` |
| `CS-037` | Code-level documentation MUST be kept current with the code it documents; documentation describing behavior the code no longer has is a defect of the same severity as incorrect code. | AEOS-DOCSTD `DS-P-03`, applied here to code |
| `CS-038` | Code-level documentation MUST NOT restate what a Specification document already states normatively for the behavior in question; it references the Specification instead. | AEOS-DOCSTD `DS-P-07`, applied here to code |

## 12. Dependency Management Principles

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-039` | A dependency AEOS's own Implementation relies on MUST be drawn from AEOS-TECH's recognized set, at or above the Conditionally Supported tier, consistent with the pattern `ENVIRONMENT_SETUP.md` `SETUP-005` already applies at the setup layer. | AEOS-TECH · `ENVIRONMENT_SETUP.md` `SETUP-005` |
| `CS-040` | A dependency MUST resolve deterministically, so that the same declaration produces the same resolved set on every supported Platform, consistent with AEOS-TECH `TC-07`'s reproducibility criterion and `PR-NFR-002`. | AEOS-TECH `TC-07` · `PR-NFR-002` · `PR-PLT-003` |
| `CS-041` | A dependency MUST be added only when its responsibility could not reasonably be met by AEOS's own Implementation or by a dependency already present; each addition is a deliberate decision, not an incidental one. | `CS-016` |
| `CS-042` | AEOS's own Implementation MUST NOT depend on the presence of a Vendor, Runtime, Model, or Platform beyond what AEOS-PRD `PR-RUN` and `PR-PLT` already commit to, consistent with AEOS-VISION `V6`. | AEOS-VISION `V6` · `PR-RUN` · `PR-PLT` |

## 13. Security-Related Coding Principles

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-043` | A credential or secret MUST NOT be written into AEOS's own Implementation, its tests, its logs, or any Repository Asset, consistent with `PR-SAF-006` and `PR-REP-013`. | `PR-SAF-006` · `PR-REP-013` |
| `CS-044` | Input that originates outside AEOS's own Implementation's trust boundary — from the Environment, a Runtime, a Model, or a Tool — MUST be validated before it is acted upon, consistent with the fail-closed posture `CS-027` states. | `PR-SAF-002` · `CS-027` |
| `CS-045` | AEOS's own Implementation MUST NOT modify, replace, or remove a component it did not itself install, consistent with `PR-SAF-009` and `PR-ENV-009`. | `PR-SAF-009` · `PR-ENV-009` |
| `CS-046` | A dependency's security-support status is drawn from AEOS-TECH's own evaluation, under its Security category and currency criteria; this document performs no independent security evaluation of its own. | AEOS-TECH `TC-20` |

## 14. AI-Assisted Coding Expectations

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-047` | An AI runtime authoring or modifying AEOS's own Implementation is a Contributor, in the sense AEOS-GLOSSARY's *Contributor* entry defines that term, and is held to every rule in this document identically to a human Contributor. | AEOS-GLOSSARY *Contributor* · AEOS-DOCSTD Section 2.4 |
| `CS-048` | Code an AI runtime contributes to AEOS's own Implementation follows the same TDD Cycle as code a human Contributor writes; a failing test precedes it, without exemption. | `PR-TDD-001` through `PR-TDD-012` · AEOS-GLOSSARY *TDD Cycle* |
| `CS-049` | An AI runtime MUST NOT present a generated inference as an observed fact within AEOS's own Implementation or the code-level documentation that accompanies it, consistent with `PR-SAF-011`. | `PR-SAF-011` |
| `CS-050` | Code an AI runtime produces is proposed and reviewed under the same Approval Gate as code a human Contributor produces before it is merged into AEOS's own Implementation; it is never merged solely on the basis of having been generated. | AEOS-VISION §6.1 · §6.9 |

## 15. Human Review Expectations

AEOS-GLOSSARY's *Review* entry already defines review as the examination of an artifact or change
against requirements, rules, and tests, producing findings classified as Critical, Major, Minor, or
Nitpick — a closed set of four severities that MUST NOT be extended, renamed, or reordered. This
section applies that existing definition to AEOS's own Implementation; it does not restate or extend
it.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CS-051` | A change to AEOS's own Implementation MUST be reviewed before it is merged, consistent with `PR-REP-011` and AEOS-GLOSSARY's *Review* entry. | `PR-REP-011` · AEOS-GLOSSARY *Review* |
| `CS-052` | A change MUST NOT be merged while a Critical or Major finding against it remains open, mirroring the freeze discipline AEOS-DOCSTD `DS-P-04` states for documentation, applied here to code by this document's own decision. | AEOS-DOCSTD `DS-P-04`, applied here to code |
| `CS-053` | A reviewer identifies inconsistencies with this document and MUST NOT redesign the change under review, mirroring the review conduct AEOS-GLOSSARY's *Review* entry and AEOS-DOCSTD Section 12.4 already state. | AEOS-GLOSSARY *Review* |
| `CS-054` | A reviewer citing this document MUST cite the specific `CS-<NNN>` rule a finding violates, so that the finding is actionable rather than a matter of preference. | `CS-051` |

---

## 16. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document, in
the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Fixing AEOS's own Implementation's programming language or technology stack beyond what AEOS-TECH already recognizes. | AEOS-TECH, and a future Blueprint-derived decision. |
| `NG-2` | Fixing a top-level source or test directory name for AEOS's own Implementation. | A future Implementation Guide or Blueprint-derived placement decision, per `REPOSITORY_LAYOUT.md` `NG-1`. |
| `NG-3` | Configuring a formatter, a linter, an IDE, or a compiler. | Technology-specific configuration, adopted once AEOS's own Implementation language is fixed; see [Section 2.3](#23-distinguishing-this-standard-from-adjacent-artifacts). |
| `NG-4` | Defining the observable behavior any unit of AEOS's own Implementation MUST produce. | Specification documents, under AEOS-SPECSTD. |
| `NG-5` | Binding the Rules that govern a Developer's own Project code. | That Project's own Rules, under AEOS-PRD `PR-RUL`. |
| `NG-6` | Defining a commit message standard, a branching strategy, or code review tooling. | A future Implementation Guide or Developer Guide, once authored. |
| `NG-7` | Establishing the Implementation Guide layer's own Standard, identifier conventions, or structure for documents other than this one. | Reserved to the owner under AEOS-DOCSTD `H5`. |

## 17. Traceability

Every `CS-<NNN>` rule in this document traces to one or more `PR-` identifiers, an AEOS-VISION
conviction or invariant, an AEOS-GLOSSARY entry, or another `CS-<NNN>` rule it refines, stated inline
in [Sections 3](#3-coding-philosophy) through [15](#15-human-review-expectations) and indexed in full
in [Appendix A](#appendix-a--cs-rule-index). The table below summarizes trace density by `PR-`
prefix, consistent with the practice AEOS-SPECSTD `CM2` establishes for Specification documents and
that `PROJECT_BOOTSTRAP.md` Section 11 already adopts for an Implementation Guide.

| `PR-` prefix | Rules in this document that trace to it |
| :--- | :--- |
| `PR-SAF` | `CS-027` · `CS-028` · `CS-029` · `CS-031` · `CS-032` · `CS-033` · `CS-034` · `CS-043` · `CS-044` · `CS-045` · `CS-049` |
| `PR-NFR` | `CS-001` · `CS-005` · `CS-011` · `CS-016` · `CS-017` · `CS-034` · `CS-040` |
| `PR-TDD` | `CS-004` · `CS-012` · `CS-048` |
| `PR-REP` | `CS-033` · `CS-035` · `CS-043` · `CS-051` |
| `PR-RUN` | `CS-013` · `CS-042` |
| `PR-PLT` | `CS-040` · `CS-042` |
| `PR-WFL` | `CS-028` · `CS-034` |
| `PR-ENV` | `CS-045` |
| `PR-RUL` | referenced in [Section 2.4](#24-applicability), not traced by any `CS-<NNN>` rule |
| `PR-DOC` | referenced in [Section 11](#11-documentation-expectations), not traced by any `CS-<NNN>` rule |

Three rules trace to AEOS-DOCSTD or AEOS-GLOSSARY directly rather than to AEOS-PRD, because they
apply a discipline that document already states for its own subject to code by this document's own
decision, rather than a product requirement: `CS-037`, `CS-038`, and `CS-052`. This document, as a
whole, traces to AEOS-DOCSTD `H4`'s requirement that every derivative document trace to the layer
above it, satisfied here by tracing each rule to the `PR-` requirement, AEOS-VISION conviction, or
AEOS-GLOSSARY entry it ultimately serves, and to AEOS-DOCSTD `H6`'s requirement that a document
belong to a layer, satisfied by this document's position as an Implementation Guide, stated in the
authority statement at the head of this document.

## 18. References

| Document | Document ID | Cited for |
| :--- | :--- | :--- |
| `AEOS_VISION.md` | AEOS-VISION | Invariants `V4`, `V6`, `V7`, `V9`, `V10`, and the convictions in §6.1, §6.9, §6.14, §6.15, and §6.16, cited throughout [Sections 3](#3-coding-philosophy) through [15](#15-human-review-expectations). |
| `AEOS_PRODUCT_REQUIREMENTS.md` | AEOS-PRD | The `PR-SAF`, `PR-NFR`, `PR-TDD`, `PR-REP`, `PR-RUN`, `PR-PLT`, `PR-WFL`, `PR-ENV`, `PR-RUL`, and `PR-DOC` identifiers this document's rules trace to, indexed in [Section 17](#17-traceability). |
| `AEOS_GLOSSARY.md` | AEOS-GLOSSARY | Terminology used without redefinition throughout this document, including *Implementation*, *Contributor*, *Review*, *Rule*, *Repository Asset*, *TDD Cycle*, and the naming conventions in Section 6, cited in [Sections 2.4](#24-applicability), [7](#7-naming-principles), [14](#14-ai-assisted-coding-expectations), and [15](#15-human-review-expectations). |
| `AEOS_DOCUMENT_STANDARD.md` | AEOS-DOCSTD | The documentation hierarchy, the Implementation Guide layer's purpose (Section 4.3), the general document template, and the principles and rules this document adopts by analogy for code (`DS-P-03`, `DS-P-04`, `DS-P-07`, `H4`, `H5`, `H6`). |
| `AEOS_SUPPORTED_TECHNOLOGIES.md` | AEOS-TECH | The recognized technology set and support tiers `CS-039`, `CS-040`, and `CS-046` trace to, including categories `TC-07` and `TC-20`; this document names no technology choice of its own. |
| `AEOS_SPECIFICATION_STANDARD.md` | AEOS-SPECSTD | The single-satisfiability test cited in [Section 2.3](#23-distinguishing-this-standard-from-adjacent-artifacts), and the traceability practice `CM2` [Section 17](#17-traceability) adopts; this document states no specified behavior of its own. |
| `AEOS_ARCHITECTURE.md` | AEOS-ARCH | The layer boundaries `CS-015` and `CS-024` follow; this document states no structural decision of its own. |
| `AEOS_BLUEPRINT.md` | AEOS-BLUEPRINT | Referenced for completeness as the layer immediately above Specification; this document states no arrangement of its own. |
| `PROJECT_BOOTSTRAP.md` | AEOS-BOOT | The Implementation Guide precedent this document's authority statement and [Section 17](#17-traceability) follow. |
| `ENVIRONMENT_SETUP.md` | AEOS-ENVSETUP | The dependency-tier pattern `CS-039` follows (`SETUP-005`), and a second Implementation Guide precedent for this document's authority statement. |
| `REPOSITORY_LAYOUT.md` | AEOS-LAYOUT | Non-goal `NG-1`, which `CS-026` and this document's own `NG-2` defer to for source and test directory placement. |

## 19. Document Governance

### 19.1 Status

This document is a **Draft**. It is intended to become the AEOS 1.0 Coding Standard Source of Truth
once the owner's review under [Section 19.4](#194-review-policy) records no Critical or Major
finding, at which point it is intended to be placed and frozen alongside the AEOS 1.0 document set.

### 19.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `CS-<NNN>` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Any change to what an existing `CS-<NNN>` rule requires, or its retirement | Explicit owner revision request | Major |
| Change to a section's scope, or to [Section 2.3](#23-distinguishing-this-standard-from-adjacent-artifacts)'s distinctions | Explicit owner revision request | Major |
| Any change that would bind Developer Project code beyond [Section 2.4](#24-applicability)'s stated limit | Explicit owner revision request, with the reasoning preserved in place | Major |

### 19.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-DOCSTD, or
the documentation hierarchy are recorded as recommendations for the owning document's governance and
are applied only after explicit owner approval there — never enacted here.

### 19.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in [Sections 3](#3-coding-philosophy) through [15](#15-human-review-expectations)
      carries a `CS-<NNN>` identifier and a trace.
- [ ] No rule names a programming language, a framework, a library, a formatter, a linter, an IDE, or
      a compiler.
- [ ] No rule can be satisfied in only one technical way, per the single-satisfiability test in
      [Section 2.3](#23-distinguishing-this-standard-from-adjacent-artifacts).
- [ ] No rule restates a definition AEOS-GLOSSARY already owns, a structural decision AEOS-ARCH
      already owns, or a behavioral rule a Specification document already owns.
- [ ] No rule binds a Developer's own Project code beyond the limit [Section 2.4](#24-applicability)
      states.
- [ ] All twenty-one entries in this document's Table of Contents are present, in order, and none is
      silently empty.
- [ ] No Critical or Major finding remains open.

### 19.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with a Specification document on the observable behavior of AEOS's own Implementation | The Specification governs. This document is corrected to reference it rather than restate it. |
| This document appears to fix a programming language, a technology, or a file location beyond what AEOS-TECH or a placement decision already recognizes | The apparent decision is a defect in this document under [Section 2.3](#23-distinguishing-this-standard-from-adjacent-artifacts) and is reported rather than acted upon. |
| This document appears to bind a Developer's own Project code | [Section 2.4](#24-applicability) governs. The apparent binding is a defect in this document and is reported rather than acted upon. |
| A future Implementation Guide states a coding principle that conflicts with a `CS-<NNN>` rule here | The apparent need is reported against this document. It is not resolved by a contradictory statement in the other guide. |

### 19.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Coding Standard. States five coding-philosophy principles and forty-nine further `CS-<NNN>` rules across readability, maintainability, modularity, naming, file organization, error handling, logging, code-level documentation expectations, dependency management, security-related coding, AI-assisted coding expectations, and human review expectations, together with seven non-goals. Distinguishes this document from a Language Style Guide, a Formatter, a Linter, and an Implementation Specification. Scopes this document to AEOS's own Implementation, distinct from a Developer's own Project code, which remains governed by that Project's own Rules under `PR-RUL`. References every Foundation, Architecture, Blueprint, and Implementation Guide document existing at the time of authoring. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, and no specified behavior. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPECSTD. |

---

## Appendix A — CS Rule Index

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 3](#3-coding-philosophy) through [15](#15-human-review-expectations).

| ID | Section | Short label | Traces to |
| :--- | :--- | :--- | :--- |
| `CS-001` | 3 | Clarity over cleverness | AEOS-VISION §6.14 · `PR-NFR-006` |
| `CS-002` | 3 | Explicit over implicit | AEOS-VISION §6.15 |
| `CS-003` | 3 | Product lifetime is the time horizon | AEOS-VISION §6.16 |
| `CS-004` | 3 | TDD Cycle without exemption | AEOS-VISION `V4` · `PR-TDD` |
| `CS-005` | 3 | One artifact serves two audiences | `PR-NFR-009` |
| `CS-006` | 4 | Names state what a thing does | `CS-002` |
| `CS-007` | 4 | One unit, one responsibility | `CS-001` |
| `CS-008` | 4 | Minimize nesting and branching | `CS-001` |
| `CS-009` | 4 | Comments explain why, not what | `CS-002` |
| `CS-010` | 4 | Name literal values once | `CS-001` |
| `CS-011` | 5 | No duplicated logic | `PR-NFR-006` |
| `CS-012` | 5 | Behavior change ships with its test | `PR-TDD-001` · `PR-TDD-003` |
| `CS-013` | 5 | No dependence on undocumented behavior | `PR-RUN-006` |
| `CS-014` | 5 | Isolate and record external workarounds | `CS-003` |
| `CS-015` | 6 | Modules follow Architecture's layer boundaries | AEOS-ARCH |
| `CS-016` | 6 | Depend only on what is required | `PR-NFR-006` |
| `CS-017` | 6 | Extend by addition, not modification | AEOS-VISION `V10` · `PR-NFR-007` |
| `CS-018` | 6 | Minimal public surface | `CS-016` |
| `CS-019` | 7 | Identifiers state what, not how | AEOS-GLOSSARY `N1` |
| `CS-020` | 7 | Glossary terms used without alternative wording | AEOS-GLOSSARY `T1` · `T3` |
| `CS-021` | 7 | No invented, inconsistent abbreviations | `CS-002` |
| `CS-022` | 7 | English identifiers | AEOS-GLOSSARY `N6` |
| `CS-023` | 8 | File location reflects responsibility | `CS-019` |
| `CS-024` | 8 | No file mixes architectural layers | `CS-015` |
| `CS-025` | 8 | Tests locatable without a separate index | `PR-TDD-007` |
| `CS-026` | 8 | No directory name fixed here | `REPOSITORY_LAYOUT.md` `NG-1` |
| `CS-027` | 9 | Fail closed on unverified conditions | AEOS-VISION `V7` · `PR-SAF-002` |
| `CS-028` | 9 | Errors reported with actionable detail | `PR-WFL-010` · `PR-WFL-011` · `PR-TDD-010` |
| `CS-029` | 9 | Interruption leaves a describable state | `PR-SAF-010` |
| `CS-030` | 9 | No silently discarded errors | `CS-027` |
| `CS-031` | 9 | No destructive side effects in recovery | `PR-SAF-003` · `PR-SAF-004` |
| `CS-032` | 10 | Logs distinguish fact from inference | `PR-SAF-011` · AEOS-VISION `V9` |
| `CS-033` | 10 | No credentials or secrets in logs | `PR-SAF-006` · `PR-REP-013` |
| `CS-034` | 10 | Logging supports Transparency, not a substitute for Report | `PR-NFR-001` · `PR-SAF-008` · `PR-WFL-005` |
| `CS-035` | 10 | Logs remain plain, structured text | `PR-REP-016` |
| `CS-036` | 11 | Public interfaces state what and why | `CS-005` |
| `CS-037` | 11 | Code-level documentation stays current | AEOS-DOCSTD `DS-P-03` |
| `CS-038` | 11 | No restating Specification content | AEOS-DOCSTD `DS-P-07` |
| `CS-039` | 12 | Dependencies drawn from AEOS-TECH's recognized set | AEOS-TECH · `SETUP-005` |
| `CS-040` | 12 | Deterministic, cross-platform dependency resolution | AEOS-TECH `TC-07` · `PR-NFR-002` |
| `CS-041` | 12 | Dependencies added deliberately | `CS-016` |
| `CS-042` | 12 | No dependence on unlisted Vendor, Runtime, Model, or Platform | AEOS-VISION `V6` |
| `CS-043` | 13 | No credentials or secrets in Implementation | `PR-SAF-006` · `PR-REP-013` |
| `CS-044` | 13 | Validate input crossing the trust boundary | `PR-SAF-002` · `CS-027` |
| `CS-045` | 13 | No modifying components not installed by AEOS | `PR-SAF-009` · `PR-ENV-009` |
| `CS-046` | 13 | Security status drawn from AEOS-TECH | AEOS-TECH `TC-20` |
| `CS-047` | 14 | AI runtimes are Contributors, held identically | AEOS-GLOSSARY *Contributor* |
| `CS-048` | 14 | AI-generated code follows the TDD Cycle | `PR-TDD` |
| `CS-049` | 14 | No presenting inference as observed fact | `PR-SAF-011` |
| `CS-050` | 14 | Same Approval Gate regardless of authorship | AEOS-VISION §6.1 · §6.9 |
| `CS-051` | 15 | Every change reviewed before merge | `PR-REP-011` |
| `CS-052` | 15 | No merge with an open Critical or Major finding | AEOS-DOCSTD `DS-P-04` |
| `CS-053` | 15 | Reviewers identify, do not redesign | AEOS-GLOSSARY *Review* |
| `CS-054` | 15 | Findings cite the specific rule violated | `CS-051` |

## Appendix B — Coding Standard Review Checklist (Non-Normative)

**This appendix is non-normative.** It restates [Section 19.4](#194-review-policy)'s checklist as a
convenience for a reviewer working through a change to AEOS's own Implementation, rather than a
review of this document itself.

- [ ] The change is decomposed so that each unit does one thing (`CS-007`).
- [ ] Names state what they do or represent, without requiring the reader to read further (`CS-006`,
      `CS-019`).
- [ ] No logic already present elsewhere in AEOS's own Implementation has been duplicated (`CS-011`).
- [ ] The change ships with the failing-then-passing test that demonstrates it (`CS-012`).
- [ ] No new dependency has been introduced without necessity, and any new dependency is drawn from
      AEOS-TECH's recognized set (`CS-039`, `CS-041`).
- [ ] No credential, secret, or undisclosed inference appears in code, comments, or logs (`CS-032`,
      `CS-033`, `CS-043`).
- [ ] An error condition the change introduces is reported, not silently discarded (`CS-028`,
      `CS-030`).
- [ ] Code-level documentation matches the behavior the change actually produces (`CS-037`).
- [ ] Where the change was AI-assisted, it was proposed and reviewed under the same Approval Gate as
      any other change (`CS-050`).
- [ ] Every finding raised against the change cites the specific `CS-<NNN>` rule it violates
      (`CS-054`).

---

**End of Coding Standard**

AEOS-CODESTD · Version 1.0.0 · Coding Standard Source of Truth
