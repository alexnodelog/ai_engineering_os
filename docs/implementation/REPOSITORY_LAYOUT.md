# AI Engineering Operating System

## AEOS — Repository Layout Guide

*The permanent statement of the repository structure of AEOS: what belongs at each path, why, and
who is answerable for it.*

| Field | Value |
| :--- | :--- |
| **Document** | Repository Layout Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-LAYOUT |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Contributors, maintainers, release engineers, and AI runtimes reading, extending, or bootstrapping the AEOS repository |
| **Suggested path** | `docs/REPOSITORY_LAYOUT.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document defines the **permanent repository structure of AEOS**: the top-level organization
> of the AEOS repository, the purpose, ownership, and responsibility of each top-level entry, the
> naming conventions a conformant path follows, the rules by which a file, a document, a source
> artifact, a generated artifact, a test, a tooling configuration, or a Repository Asset is placed,
> the invariants every version of the repository's structure MUST preserve, the seams by which the
> structure is extended, and the structures the repository MUST NOT take.
>
> This document defines repository organization only. It is **not** an Architecture document, **not**
> a Blueprint, **not** the Project Bootstrap Guide, and **not** any other Implementation Guide. It
> states no implementation procedure — the ordered actions by which a repository is created,
> populated, and verified are AEOS-BOOT's subject, not this one — no runtime behavior, no
> architectural decision, no product requirement, and no technology selection. Where a statement here
> appears to do any of these, that is a defect in this document and MUST be reported rather than
> acted upon.
>
> Where this document restates a structure AEOS-BOOT already states normatively — the `docs/` subtree
> described in [Section 5](#5-the-documentation-subtree-docs) — AEOS-BOOT governs that structure, and
> this document's restatement is descriptive rather than a competing authority. Everywhere else this
> document governs, this document is the source of truth for the AEOS repository's organization, in
> the sense AEOS-GLOSSARY Section 6 reserves that phrase for a single governing artifact.
>
> This document's position in the documentation hierarchy AEOS-DOCSTD Section 4.1 defines is reserved
> to the owner's decision under AEOS-DOCSTD rule `H5`, in the same manner AEOS-DOCSTD Section 4.5
> reserves the Runtime layer's position. Until decided, this document complies with every rule in
> AEOS-DOCSTD that does not itself depend on hierarchy position, in the spirit `AEOS_RUNTIME_FLOW.md`
> and `PROJECT_BOOTSTRAP.md` record for their own comparable position.
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
3. [Repository Layout Principles](#3-repository-layout-principles)
4. [Repository Organization Overview](#4-repository-organization-overview)
5. [The Documentation Subtree (docs/)](#5-the-documentation-subtree-docs)
6. [Root-Level Entries](#6-root-level-entries)
7. [Naming Conventions](#7-naming-conventions)
8. [File Placement Rules](#8-file-placement-rules)
9. [Document Placement Rules](#9-document-placement-rules)
10. [Source Placement](#10-source-placement)
11. [Test Placement](#11-test-placement)
12. [Generated Artifact Placement](#12-generated-artifact-placement)
13. [Tooling Placement](#13-tooling-placement)
14. [Repository Asset Placement](#14-repository-asset-placement)
15. [Repository Invariants](#15-repository-invariants)
16. [Repository Extensibility](#16-repository-extensibility)
17. [Forbidden Structures](#17-forbidden-structures)
18. [Non-Goals](#18-non-goals)
19. [Traceability](#19-traceability)
20. [References](#20-references)
21. [Document Governance](#21-document-governance)
22. [Appendix A — Top-Level Directory Index (Non-Normative)](#appendix-a--top-level-directory-index-non-normative)
23. [Appendix B — LAYOUT Rule Index (Non-Normative)](#appendix-b--layout-rule-index-non-normative)

---

## 1. Executive Summary

AEOS-VISION invariant `V5` states that the repository is the product. A repository can be that only
if a reader — a human arriving for the first time, or an AI runtime with no memory of any
conversation that produced it — can look at a path and know, without asking anyone, what belongs
there and why. AEOS-BLUEPRINT's Repository Blueprint (`BP-REP`) arranges this abstractly: it names
the custody a repository must provide, the kinds of content it must hold, and the boundary it must
refuse. AEOS-BOOT states, once, the ordered procedure that produces a conformant repository from
nothing. Neither document states the shape itself, independent of the procedure that produced it or
the abstraction it realizes — a shape a contributor can hold in view while working, a reviewer can
check a pull request against, and a tool can rely on without re-deriving it from a bootstrap log.

This document is that shape. It names every top-level entry the AEOS repository takes, states what
each is for, states who is answerable for its content, and states the rules by which anything new is
placed rather than improvised. It restates, without altering, the `docs/` subtree AEOS-BOOT already
defines normatively, so that a reader consulting this guide never receives an answer that
contradicts AEOS-BOOT. It then extends the same discipline to everything AEOS-BOOT deliberately does
not cover — source code, tests, generated output, tooling configuration, and non-document Repository
Assets — stating placement principles for each without inventing a directory name no frozen document
has yet authorized. Where a decision has not yet been made, this document says so plainly, as a
recorded non-goal, rather than filling the gap with a guess.

Four properties bind this document, consistent with the discipline AEOS-BOOT records for a
comparable statement of repository fact:

| Property | What it requires of this document |
| :--- | :--- |
| **Stable** | A path this document assigns a purpose to is not silently relocated; relocation is a Major change under [Section 21](#21-document-governance). |
| **Inspectable** | Every top-level entry's purpose, owner, and responsibility is stated plainly in [Section 4](#4-repository-organization-overview), never left implicit. |
| **Platform-neutral** | Every path in this document is stated in POSIX form, with forward slashes, independent of the operating system reading or writing it, consistent with AEOS-PRD `PR-PLT-003`. |
| **Extensible without invention** | A gap in today's structure is recorded as a non-goal ([Section 18](#18-non-goals)), never filled by an assumption this document was not authorized to make. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the organization of the AEOS repository — the repository that carries AEOS's
own documents and, in time, AEOS's own implementation — and nothing beyond it:

- the complete set of top-level entries the AEOS repository takes, present or future;
- the purpose, ownership, and responsibility of each top-level entry;
- the naming conventions a conformant path in the AEOS repository follows;
- the rules by which a file — a document, a source artifact, a generated artifact, a test, a tooling
  configuration, or a Repository Asset of any other kind — is placed;
- the invariants every version of the repository's structure MUST preserve;
- the seams by which the structure is extended, and what extending it MUST NOT disturb;
- the structures the repository MUST NOT take;
- what this document explicitly does not decide, so that a reader does not search this document, or a
  future document, for a placement decision that has not yet been made.

This list is complete.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, reviewed, and frozen | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH |
| How that structure is arranged to be built, and the abstract custody model a repository provides | AEOS-BLUEPRINT |
| How each defined behavior must work, precisely and testably | Specification documents |
| How AEOS executes, in what environment, with what lifecycle | Runtime documents |
| The ordered, reproducible procedure by which a new AEOS repository is created, populated, and verified | AEOS-BOOT |
| How a person works day to day within an already-built repository | Developer Guides, none of which yet exist for AEOS |
| Code, algorithms, internal module organization, and dependency selection | The codebase and its tests, per AEOS-TECH Section 2.2 |

A statement in this document that states an implementation procedure, a runtime behavior, an
architectural decision, a product requirement, or a technology selection is a **defect in this
document**. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to every top-level entry of the AEOS repository, present or future. It applies
identically to a human contributor and to an AI runtime working within the repository, consistent with
AEOS-DOCSTD Section 2.4. It does not apply to a repository AEOS governs on a user's behalf: per
AEOS-GLOSSARY's definition of *Repository*, unqualified *Repository* names a user's project
repository, while *the AEOS repository* names AEOS's own. This document concerns only the latter.

### 2.4 Relationship to the Repository Blueprint

AEOS-BLUEPRINT's Repository Blueprint (`BP-REP`) arranges custody as an architectural concern: it
states that durable product meaning is held in one place, that a Repository Asset's identity is held
separately from its content, and that Runtime State is refused admission — for every repository AEOS
governs, including its own. `BP-REP` names no directory, no filename, and no path; a physical layout
is one of many arrangements that could realize it. This document is one such realization, scoped to
the AEOS repository alone. It does not restate `BP-REP`'s rules and does not weaken, reinterpret, or
narrow any of them; where a placement rule here would require doing so, that is a defect in this
document, per [Section 2.2](#22-what-this-document-does-not-govern).

### 2.5 Relationship to AEOS-BOOT

AEOS-BOOT states the ordered procedure that produces a conformant AEOS repository once, at the moment
of creation, and the checks that confirm the result. This document states the shape that procedure
produces, independent of the procedure itself, so the shape can be consulted without re-reading a
sequence meant to run once. The two documents describe the same `docs/` subtree from different
vantage points; [Section 5](#5-the-documentation-subtree-docs) states precisely how this document
defers to AEOS-BOOT on that structure, and [Section 21.5](#215-precedence) states what governs if the
two are ever found to disagree.

## 3. Repository Layout Principles

These principles constrain every rule in this document. A rule that violates one is defective, not
merely unconventional.

| # | Principle |
| :--- | :--- |
| `LP1` | Every top-level entry has exactly one stated purpose and exactly one document answerable for its content. |
| `LP2` | A path's meaning does not depend on which tool, editor, or Platform created it. |
| `LP3` | Placement is discovered from a document's or an asset's own declaration wherever one exists, never invented ad hoc, consistent with the discovery-first placement AEOS-BOOT `BOOT-004` already establishes for documents. |
| `LP4` | The documentation subtree and the rest of the AEOS repository are governed by the same invariants, even where only the documentation subtree is populated today. |
| `LP5` | A structural gap is recorded as a non-goal and left open. It is never filled by assumption, however reasonable the assumption appears. |
| `LP6` | A directory is reserved in advance only where a document already in force names the layer or kind it will hold; a directory reserved for a possibility no document has named is a forbidden structure, per [Section 17](#17-forbidden-structures). |
| `LP7` | A rule in this document that could be satisfied only by a specific technology, language, or build tool has reached below repository organization and is a defect in this document. |
| `LP8` | Adding an instance of a kind this document already places — a new document, a new file within an already-placed directory — never requires a revision of this document; adding a new kind, or a new top-level entry, does. |

## 4. Repository Organization Overview

The AEOS repository's top-level entries, and what each is answerable for, are:

| Top-level entry | Purpose | Governing document | Responsibility |
| :--- | :--- | :--- | :--- |
| `docs/` | Houses every AEOS document, organized by documentation-hierarchy layer. | AEOS-DOCSTD (hierarchy) and AEOS-BOOT (subtree structure), restated in [Section 5](#5-the-documentation-subtree-docs) | Every Document-kind Repository Asset remains discoverable at the path its own metadata declares. |
| `README.md` | Repository root orientation for a first-time reader. | AEOS-BOOT `BOOT-009` | States the repository's name, a description drawn from AEOS-VISION and AEOS-PRD, and links to the Foundation documents. |
| Version-control ignore file | Excludes generated and transient content from version control. | AEOS-BOOT `BOOT-010`; this document, [Section 12](#12-generated-artifact-placement) | Excludes no path under `docs/`; excludes every path holding only generated artifacts. |
| *(reserved)* source location | Will house AEOS's own source code, once written. | This document, [Section 10](#10-source-placement) (placement principles only) | Remains outside `docs/` and distinguishable from generated output; concrete name is [Non-Goal `NG-1`](#18-non-goals). |
| *(reserved)* test location | Will house test code, once written. | This document, [Section 11](#11-test-placement) (placement principles only) | Remains distinguishable from the source it does not test; concrete name is [Non-Goal `NG-1`](#18-non-goals). |
| *(reserved)* non-Document Repository Assets | Will house Rules, Skills, Prompts, Templates, Workflows, and Profiles, once any exist for the AEOS repository itself. | This document, [Section 14](#14-repository-asset-placement) (placement principles only) | Remains under version control, inspectable, and excluded from Runtime State; concrete name is [Non-Goal `NG-2`](#18-non-goals). |

A non-normative illustration of this list, and of the `docs/` subtree it summarizes, appears in
[Appendix A](#appendix-a--top-level-directory-index-non-normative).

## 5. The Documentation Subtree (docs/)

### 5.1 Deference to AEOS-BOOT

AEOS-BOOT Section 4 states, normatively, the seven directories a bootstrapped AEOS repository creates
under `docs/`, and the layer of document each houses. That table, and the rules `BOOT-002` and
`BOOT-003` that bind it, are authoritative. This document does not restate the "houses" column's
content independently; it names the seven directories here only so this document remains readable on
its own, and defers to AEOS-BOOT for their normative definition:

`docs/foundation/` · `docs/architecture/` · `docs/product/` · `docs/specification/` ·
`docs/runtime/` · `docs/implementation/` · `docs/developer/`

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-001` | The `docs/` subtree's directory set and each directory's housed layer MUST match AEOS-BOOT Section 4 exactly. A divergence between this section and AEOS-BOOT is a defect in this document, never in AEOS-BOOT. | AEOS-BOOT `BOOT-002` · `BOOT-003` |
| `LAYOUT-002` | This document MUST NOT restate a document-placement rule AEOS-BOOT `BOOT-004` or `BOOT-007` already states; it defers to them entirely for how a document within `docs/` is placed. | AEOS-BOOT `BOOT-004` · `BOOT-007` |

### 5.2 This Document's Own Placement

This document is placed directly under `docs/`, outside every layer-specific subdirectory named in
[Section 5.1](#51-deference-to-aeos-boot). This placement is deliberate: this document's own position
in the documentation hierarchy is reserved to the owner under `H5` ([Non-Goal `NG-7`](#18-non-goals)),
so it claims no layer's subdirectory as its own. Because `docs/` itself is already created as the
common parent of the seven layer subdirectories, placing a file directly within it requires no change
to AEOS-BOOT's directory table; AEOS-BOOT `BOOT-007` already places any document whose own
Suggested-path metadata field records a location under `docs/`, at any depth.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-003` | This document's Suggested-path metadata field MUST record a location directly under `docs/`, and MUST NOT record a location inside a directory [Section 5.1](#51-deference-to-aeos-boot) reserves for a specific documentation-hierarchy layer, until the owner assigns this document such a layer. | AEOS-DOCSTD `H5` · `H6` |

## 6. Root-Level Entries

The AEOS repository root holds exactly three things: `README.md`, the version-control ignore file
where Prerequisite 4 of AEOS-BOOT applies, and `docs/`. A project owner MAY add a license file at the
root at their own discretion, consistent with AEOS-BOOT's Non-Goal `NG-7`; this document neither
requires nor forbids one, and states no further rule about it.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-004` | The AEOS repository root MUST contain `README.md` and `docs/`, and MUST contain a version-control ignore file wherever the chosen Distribution Method depends on version control. | `PR-REP-009` · `PR-DST-005` |
| `LAYOUT-005` | A file or directory MUST NOT be added at the repository root unless this document names it in [Section 4](#4-repository-organization-overview) or a Major revision of this document adds it. | `PR-NFR-001` |

## 7. Naming Conventions

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-006` | A directory name MUST be lowercase, MUST use hyphens rather than underscores or spaces where a separator is unavoidable, and MUST name the layer or function it houses using a singular noun, consistent with the existing `docs/` subtree's convention. | `PR-PLT-003` |
| `LAYOUT-007` | Every path in an AEOS document or tool configuration MUST be written using forward slashes, independent of the Platform reading or writing it. | `PR-PLT-003` |
| `LAYOUT-008` | A path MUST be treated as case-sensitive by every tool that reads it, even on a Platform whose file system is not, so that the repository remains portable across every Platform AEOS-TECH `TC-01` recognizes. | `PR-PLT-003` · `PR-PLT-005` |
| `LAYOUT-009` | A path MUST NOT contain a space. | `PR-PLT-003` |
| `LAYOUT-010` | A Document's placed filename MUST follow that document's own Suggested-path metadata field; this document fixes no filename convention beyond what each document already declares for itself. | AEOS-BOOT `BOOT-004` |

A file extension is governed by the kind of content it holds, not by this document: `.md` for a
Document, per AEOS-DOCSTD; any other extension is governed by the tooling or technology that owns the
file, per AEOS-TECH.

## 8. File Placement Rules

These rules state how the placement of any file in the AEOS repository is determined, before the
kind-specific rules of [Sections 9](#9-document-placement-rules) through
[14](#14-repository-asset-placement) are consulted.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-011` | A file's placement MUST be determined by what the file is — its kind — never by authoring convenience or the order in which it was created. | `LP1` |
| `LAYOUT-012` | Where a file carries its own declared placement metadata, as every Document does under AEOS-DOCSTD, that metadata governs its placement over any general rule in this document. | AEOS-BOOT `BOOT-004` |
| `LAYOUT-013` | A file MUST NOT be placed in a directory whose stated purpose, per [Section 4](#4-repository-organization-overview) or the kind-specific sections below, does not include that file's kind. | `LP1` |
| `LAYOUT-014` | Relocating an existing file for a naming preference alone, with no change to what the file is, is not a placement decision this document authorizes; it is a Major change to this document if it changes a rule stated here, or an ordinary change to the affected file's own governance otherwise. | `PR-NFR-001` |

## 9. Document Placement Rules

A Document-kind Repository Asset — a Foundation, Architecture, Specification, Runtime, Implementation
Guide, or Developer Guide document — is placed under `docs/`, at the location its own Suggested-path
metadata field records, following the discovery-first placement AEOS-BOOT `BOOT-004` and `BOOT-007`
already state normatively. This document adds no additional document-placement rule; it defers
entirely, consistent with [Section 5.1](#51-deference-to-aeos-boot).

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-015` | A Document MUST be placed exactly where AEOS-BOOT `BOOT-004` and `BOOT-007` place it; this document states no competing rule. | AEOS-BOOT `BOOT-004` · `BOOT-007` |
| `LAYOUT-016` | A Document MUST NOT be placed outside `docs/`. | AEOS-DOCSTD Section 4.1 |

## 10. Source Placement

AEOS-TECH Section 2.2 assigns "code, algorithms, file layout, and dependency pins" to the codebase
and its tests, not to this document or to any Foundation, Architecture, or Blueprint document. This
document accordingly states only where source code sits relative to the rest of the repository, never
how it is organized within itself; the top-level name reserved for it is not yet fixed, and is
recorded as [Non-Goal `NG-1`](#18-non-goals).

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-017` | Source code, where the AEOS repository contains any, MUST NOT be placed under `docs/` and MUST NOT be placed inside a directory this document reserves for a different kind. | `LP1` · `LP7` |
| `LAYOUT-018` | The internal organization of source code — its package layout, module boundaries, and directory names beneath its own top-level location — is owned by the conventions of the Programming Language AEOS-TECH `TC-03` recognizes for it, not by this document. | AEOS-TECH Section 2.2 · `LP7` |

## 11. Test Placement

AEOS-TECH's testability criterion for Scope P and Scope G technologies governs what a credible test
runner looks like; this document states only that test code remains distinguishable from the source
it does not test and from documentation, never how a project's own tooling organizes it.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-019` | Test code, where the AEOS repository contains any, MUST NOT be placed under `docs/`. | `LP1` |
| `LAYOUT-020` | Adding test code MUST NOT, by itself, require a revision of this document, provided it does not introduce a new top-level entry. | `LP8` |

## 12. Generated Artifact Placement

A generated artifact — any file produced by building, compiling, testing, linting, or packaging the
AEOS repository's own content — is Runtime State under AEOS-GLOSSARY's definition: losing it costs
only repeated work, never product meaning. `BP-REP-003` requires the Custody Boundary Guard to refuse
its admission into custody, and `AR-BND-008` requires that Runtime State never be necessary to
understand, reproduce, or resume a project.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-021` | A generated artifact MUST NOT be committed to the AEOS repository as durable content. | `BP-REP-003` · `AR-BND-008` · `PR-REP-015` |
| `LAYOUT-022` | Where a version-control ignore file exists, it MUST exclude every path that holds only generated artifacts, and MUST NOT exclude any path under `docs/`, consistent with AEOS-BOOT `BOOT-010`. | AEOS-BOOT `BOOT-010` |
| `LAYOUT-023` | The AEOS repository MUST remain understandable, buildable, and reproducible from its committed content alone, without any generated artifact present. | `PR-REP-016` |

## 13. Tooling Placement

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-024` | Configuration for a development tool — a linter, a formatter, a type checker, a package manager, or an editor integration — MUST be placed at the location that tool's own convention requires; this document does not relocate a tool-required path. | `LP7` |
| `LAYOUT-025` | Tooling configuration that is durable, versioned, and required to understand or reproduce the AEOS repository, per AEOS-GLOSSARY's Repository Asset test, MUST remain under version control at its tool-required path, and MUST NOT be excluded by a version-control ignore file. | AEOS-GLOSSARY, Repository Asset |
| `LAYOUT-026` | Tooling configuration MUST NOT be placed under `docs/`. | `LP1` |

## 14. Repository Asset Placement

`BP-REP` names the Repository Asset kinds a repository may hold beyond Document — Rule, Skill,
Prompt, Template, Workflow, and Profile among them, with the set left open. `BP-REP` states their
custody, identity, and admission rules; it names no directory for any of them, and no other frozen
document does either. This document accordingly states only the invariant every such asset MUST
satisfy wherever it is eventually placed; the concrete top-level location is
[Non-Goal `NG-2`](#18-non-goals).

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-027` | Every Repository Asset, of whatever kind, MUST reside within the AEOS repository under version control; it MUST NOT exist solely as Runtime State or solely outside the repository. | `BP-REP-001` · `BP-REP-012` |
| `LAYOUT-028` | A Document-kind Repository Asset is placed per [Section 9](#9-document-placement-rules). The physical placement of a Repository Asset of any other registered kind, for the AEOS repository itself, is reserved and MUST NOT be assumed from this document's silence. | [Non-Goal `NG-2`](#18-non-goals) |

## 15. Repository Invariants

These invariants MUST hold across every version of the AEOS repository's structure. An invariant
listed here is stable in the same sense AEOS-ARCH Section 8 invariants are stable: it survives a
routine revision of this document and changes only through the Major change control
[Section 21.2](#212-change-control) states.

| ID | Invariant | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-029` | The AEOS repository MUST remain readable and meaningful when AEOS is not running. | `PR-REP-016` · `AR-LAY-010` |
| `LAYOUT-030` | No two top-level entries MUST share a stated purpose. | `LP1` |
| `LAYOUT-031` | A path's purpose MUST be discoverable from this document and from the metadata of the content it holds, never from unwritten convention alone. | `LP3` |
| `LAYOUT-032` | The `docs/` subtree's structure MUST NOT diverge from AEOS-BOOT Section 4 without a coordinated revision of both documents. | `LAYOUT-001` |
| `LAYOUT-033` | No credential, secret, or Runtime State record MUST ever be committed at any path in the AEOS repository. | `PR-SAF-006` · `PR-REP-013` · `PR-REP-015` |
| `LAYOUT-034` | A generated artifact MUST NOT appear under version control at any path. | `LAYOUT-021` |

## 16. Repository Extensibility

The AEOS repository's structure is extended through two seams, and through no other means.

| Extension seam | What is added | What MUST NOT change |
| :--- | :--- | :--- |
| **Instance admission** | A new instance of a kind this document already places — a new document within an already-listed `docs/` directory, a new file of an already-placed kind. | No revision of this document is required, per `LP8`. |
| **Kind or entry admission** | A new top-level entry, a new placed kind, or a new naming convention. | Requires a Major revision of this document, per [Section 21.2](#212-change-control); it MUST NOT be introduced by exception, by a downstream document's contradictory statement, or by informal convention. |

A new `docs/` subdirectory reserved in advance for a layer AEOS-DOCSTD already names, in the manner
AEOS-BOOT `BOOT-028` reserves `docs/developer/`, is AEOS-BOOT's decision to make, not this document's;
this document restates the result under [Section 5.1](#51-deference-to-aeos-boot) once AEOS-BOOT
states it.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-035` | Adding an instance of an already-placed kind MUST NOT require a revision of this document. | `LP8` |
| `LAYOUT-036` | Adding a new top-level entry, a new placed kind, or a new naming convention MUST be a Major revision of this document, and MUST NOT be introduced any other way. | `PR-NFR-001` |

## 17. Forbidden Structures

The following structures MUST NOT exist in the AEOS repository. Each is forbidden because it either
duplicates an authority this document or AEOS-BOOT already assigns, or admits content this document's
invariants exclude.

| ID | Forbidden structure | Traces to |
| :--- | :--- | :--- |
| `LAYOUT-037` | A second documentation root duplicating or competing with `docs/` — for example, a `documentation/` or `wiki/` directory holding AEOS documents. | `LAYOUT-030` |
| `LAYOUT-038` | A document of one documentation-hierarchy layer nested inside the directory [Section 5.1](#51-deference-to-aeos-boot) reserves for a different layer. | AEOS-BOOT `BOOT-003` |
| `LAYOUT-039` | An empty top-level directory reserved for a layer or kind no document already in force names, per `LP6`. | `LP6` |
| `LAYOUT-040` | A credential, a secret, or a Runtime State record committed at any path. | `LAYOUT-033` |
| `LAYOUT-041` | A second, machine-only form of a Repository Asset alongside its human-readable form. | `BP-REP-013` |
| `LAYOUT-042` | A directory whose name does not correspond to any purpose this document states. | `LAYOUT-013` |

## 18. Non-Goals

This document deliberately does not decide the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search this document, or a future document,
for a decision that has not yet been made.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Fixing a top-level name for AEOS's own source code or test locations. | A future Implementation Guide or Blueprint-derived placement decision, once AEOS's own implementation begins. |
| `NG-2` | Fixing a top-level or physical location for a Repository Asset of a kind other than Document — a Rule, Skill, Prompt, Template, Workflow, or Profile — for the AEOS repository itself. | A future Implementation Guide, once such an asset exists for AEOS's own repository. |
| `NG-3` | Stating the ordered procedure that creates, populates, or verifies this structure. | AEOS-BOOT. |
| `NG-4` | Defining what makes a file a Repository Asset, or the meaning of any asset kind. | AEOS-PRD · AEOS-GLOSSARY. |
| `NG-5` | Selecting a technology, language, framework, or build system. | AEOS-TECH. |
| `NG-6` | Stating how a user's own governed Project repository is organized. | The Project and its own conventions, per AEOS-BLUEPRINT's Project convention admission extension point (`BP-REP` Section 7.7). |
| `NG-7` | Positioning this document within the AEOS-DOCSTD documentation hierarchy. | Reserved to the owner, under AEOS-DOCSTD `H5`. |
| `NG-8` | Restating AEOS-BOOT Section 4's `docs/` table beyond what [Section 5](#5-the-documentation-subtree-docs) needs to remain self-contained. Where the two might appear to diverge, AEOS-BOOT governs, per [Section 21.5](#215-precedence). | AEOS-BOOT. |

## 19. Traceability

Every `LAYOUT-` rule in this document traces to one or more `PR-` identifiers, an AEOS-ARCH or
AEOS-BLUEPRINT identifier, an AEOS-BOOT rule, or a principle stated in
[Section 3](#3-repository-layout-principles), stated inline in
[Sections 5](#5-the-documentation-subtree-docs) through [17](#17-forbidden-structures) and indexed in
full in [Appendix B](#appendix-b--layout-rule-index-non-normative).

| Trace target | Rules in this document that trace to it |
| :--- | :--- |
| `PR-REP` | `LAYOUT-004` · `LAYOUT-021` · `LAYOUT-022` · `LAYOUT-023` · `LAYOUT-029` · `LAYOUT-033` |
| `PR-SAF` | `LAYOUT-033` |
| `PR-PLT` | `LAYOUT-006` · `LAYOUT-007` · `LAYOUT-008` · `LAYOUT-009` · `LAYOUT-017` |
| `PR-DST` | `LAYOUT-004` |
| `PR-NFR` | `LAYOUT-005` · `LAYOUT-014` · `LAYOUT-036` |
| `AR-` (AEOS-ARCH) | `LAYOUT-021` · `LAYOUT-029` |
| `BP-` (AEOS-BLUEPRINT) | `LAYOUT-021` · `LAYOUT-027` · `LAYOUT-041` |
| `BOOT-` (AEOS-BOOT) | `LAYOUT-001` · `LAYOUT-002` · `LAYOUT-010` · `LAYOUT-015` · `LAYOUT-022` · `LAYOUT-032` · `LAYOUT-038` |

This document, as a whole, traces to AEOS-DOCSTD `H4`'s requirement that every derivative document
trace to the layer above it — satisfied here by tracing to AEOS-BLUEPRINT and to the `PR-` identifiers
each rule ultimately serves — and to AEOS-DOCSTD `H6`'s requirement that a document belong to a layer,
addressed by the reserved-position statement in this document's authority statement and in
[Non-Goal `NG-7`](#18-non-goals).

## 20. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-VISION invariant `V5` | The repository-as-product invariant this document's purpose serves |
| AEOS-GLOSSARY, *Repository* and *Repository Asset* entries | The distinction between the AEOS repository and a governed Project's repository, and the definition of a Repository Asset this document places |
| AEOS-DOCSTD Section 4.1, `H5`, `H6` | The documentation hierarchy and this document's reserved position within it |
| AEOS-DOCSTD Section 4.5 | The unassigned-layer provision this document's own hierarchy position follows by analogy |
| AEOS-TECH Section 2.2 | The explicit assignment of code, algorithms, and file layout to the codebase and its tests |
| AEOS-ARCH `AR-LAY-010`, `AR-BND-008` | The invariants requiring the repository, and its Repository Layer content, to remain meaningful without a running Runtime |
| AEOS-BLUEPRINT Section 7 (`BP-REP`) | The abstract custody model this document's placement rules realize physically |
| AEOS-BOOT Section 4 | The `docs/` subtree this document restates by reference rather than independently |
| AEOS-BOOT Sections 6, 10 | The initial-configuration and non-goal statements this document's root-level and generated-artifact rules remain consistent with |

## 21. Document Governance

### 21.1 Status

This document is a **Draft**. It is the first Repository Layout Guide authored for AEOS. It defines
its own change control, in the absence of a governing Standard for the position it occupies,
consistent with AEOS-DOCSTD Section 13.3's default for a document that does not otherwise state one —
the same default `AEOS_RUNTIME_FLOW.md` and AEOS-BOOT record for their own comparable position.

### 21.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Refreshing [Appendix A](#appendix-a--top-level-directory-index-non-normative)'s illustrative content to reflect a document that fits an existing, already-named path | Contributor change, owner acceptance | Patch |
| Addition of a new `LAYOUT-` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Resolving a Non-Goal in [Section 18](#18-non-goals) by naming a concrete path, once a governing decision authorizes it | Owner approval | Minor |
| Any change to what an existing `LAYOUT-` rule requires, or its retirement | Explicit owner revision request | Major |
| Addition of a new top-level entry, a new placed kind, or a new naming convention | Explicit owner revision request | Major |
| Any change that would contradict AEOS-BOOT Section 4 | Not permitted here; the change belongs to AEOS-BOOT's own change control | — |

### 21.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-DOCSTD, or
AEOS-BOOT are recorded as recommendations for the owning document's governance and are applied only
after explicit owner approval there — never enacted here.

### 21.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in [Sections 5](#5-the-documentation-subtree-docs) through
      [17](#17-forbidden-structures) carries a `LAYOUT-` identifier and a trace.
- [ ] [Section 5](#5-the-documentation-subtree-docs) does not restate any `docs/` placement rule
      AEOS-BOOT already states normatively, beyond what is needed for this document to remain
      self-contained.
- [ ] No rule states an implementation procedure, a runtime behavior, an architectural decision, a
      product requirement, or a technology selection.
- [ ] Every placement category [Section 18](#18-non-goals) records as unresolved remains unresolved
      in [Sections 10](#10-source-placement), [11](#11-test-placement), and
      [14](#14-repository-asset-placement) — no concrete path is asserted for it.
- [ ] All twenty-three entries in this document's Table of Contents are present, in order, and none
      is silently empty.
- [ ] No Critical or Major finding remains open.

### 21.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document's statement of the `docs/` subtree conflicts with AEOS-BOOT Section 4 | AEOS-BOOT governs. The conflict is a defect in this document's [Section 5](#5-the-documentation-subtree-docs), corrected to match AEOS-BOOT rather than the reverse. |
| A future Developer Guide states a day-to-day convention that does not name a path this document has not already named | Both stand. This document governs structure; the guide governs practice within it, consistent with AEOS-DOCSTD's derivation chain. |
| A future Developer Guide, Specification, or Implementation Guide names a path that conflicts with this document | The apparent need is reported against this document under [Section 21.2](#212-change-control). It is not resolved by a contradictory statement in the other document. |
| This document names or implies a technology choice | AEOS-TECH governs. The statement is a defect here. |

### 21.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Repository Layout Guide. States eight repository layout principles; a seven-entry top-level organization overview with purpose, governing document, and responsibility for each entry; deference to AEOS-BOOT Section 4 for the `docs/` subtree; naming conventions; general file-placement rules; document-placement rules that defer entirely to AEOS-BOOT; placement principles, without concrete directory names, for source code, test code, generated artifacts, tooling configuration, and non-Document Repository Assets; six repository invariants; a two-seam extensibility model; six forbidden structures; eight recorded non-goals; and forty-two `LAYOUT-<NNN>` rules in total. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no implementation procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-BOOT. |

---

## Appendix A — Top-Level Directory Index (Non-Normative)

**This appendix is non-normative.** Placement itself is governed by [Sections 5](#5-the-documentation-subtree-docs)
through [14](#14-repository-asset-placement); this appendix illustrates one valid instance of the
structure those sections describe, at the time of this document's authoring. Source, test, tooling,
and non-Document Repository Asset locations are intentionally absent from this illustration, per
[Section 18](#18-non-goals)'s recorded non-goals — showing a concrete name for any of them here would
assert a decision this document does not make.

```text
<AEOS repository root>
├── README.md
├── .gitignore                        (where Distribution Method depends on version control)
└── docs/
    ├── REPOSITORY_LAYOUT.md          (this document)
    ├── foundation/
    │   ├── VISION.md
    │   ├── PRD.md
    │   ├── GLOSSARY.md
    │   ├── DOCUMENT_STANDARD.md
    │   └── SUPPORTED_TECHNOLOGIES.md
    ├── architecture/
    │   ├── ARCHITECTURE.md
    │   └── BLUEPRINT.md
    ├── product/
    │   └── SPECIFICATION_STANDARD.md
    ├── specification/
    │   ├── SYSTEM_SPECIFICATION.md
    │   ├── RUNTIME_NEGOTIATION_SPEC.md
    │   ├── RUNTIME_ADAPTER_SPEC.md
    │   └── MODEL_REGISTRY.md
    ├── runtime/
    │   ├── RUNTIME_FLOW.md
    │   ├── RUNTIME_REGISTRY.md
    │   └── RUNTIME_CAPABILITY_SPEC.md
    ├── implementation/
    │   └── PROJECT_BOOTSTRAP.md
    └── developer/
        └── (reserved — empty until a Developer Guide is authored)
```

## Appendix B — LAYOUT Rule Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 5](#5-the-documentation-subtree-docs) through [17](#17-forbidden-structures).

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `LAYOUT-001`–`LAYOUT-002` | Deference to AEOS-BOOT for the `docs/` subtree | 2 | [5.1](#51-deference-to-aeos-boot) |
| `LAYOUT-003` | This document's own placement | 1 | [5.2](#52-this-documents-own-placement) |
| `LAYOUT-004`–`LAYOUT-005` | Root-level entries | 2 | [6](#6-root-level-entries) |
| `LAYOUT-006`–`LAYOUT-010` | Naming conventions | 5 | [7](#7-naming-conventions) |
| `LAYOUT-011`–`LAYOUT-014` | General file placement | 4 | [8](#8-file-placement-rules) |
| `LAYOUT-015`–`LAYOUT-016` | Document placement | 2 | [9](#9-document-placement-rules) |
| `LAYOUT-017`–`LAYOUT-018` | Source placement | 2 | [10](#10-source-placement) |
| `LAYOUT-019`–`LAYOUT-020` | Test placement | 2 | [11](#11-test-placement) |
| `LAYOUT-021`–`LAYOUT-023` | Generated artifact placement | 3 | [12](#12-generated-artifact-placement) |
| `LAYOUT-024`–`LAYOUT-026` | Tooling placement | 3 | [13](#13-tooling-placement) |
| `LAYOUT-027`–`LAYOUT-028` | Repository Asset placement | 2 | [14](#14-repository-asset-placement) |
| `LAYOUT-029`–`LAYOUT-034` | Repository invariants | 6 | [15](#15-repository-invariants) |
| `LAYOUT-035`–`LAYOUT-036` | Repository extensibility | 2 | [16](#16-repository-extensibility) |
| `LAYOUT-037`–`LAYOUT-042` | Forbidden structures | 6 | [17](#17-forbidden-structures) |

---

**End of Repository Layout Guide**

AEOS-LAYOUT · Version 1.0.0 · Traces to `PR-REP` · `PR-SAF` · `PR-PLT` · `PR-DST` · `PR-NFR`, realizing
AEOS-BLUEPRINT `BP-REP` and restating AEOS-BOOT Section 4 without altering either
