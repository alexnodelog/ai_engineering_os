# AI Engineering Operating System

## AEOS — Project Bootstrap Guide

*The permanent statement of how a brand-new AEOS repository is initialized, and of the repository
state that initialization must produce.*

| Field | Value |
| :--- | :--- |
| **Document** | Project Bootstrap Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-BOOT |
| **Version** | 3.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Contributors, release engineers, maintainers, and AI runtimes initializing a new AEOS repository |
| **Suggested path** | `docs/implementation/PROJECT_BOOTSTRAP.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) |
| **Supersedes** | AEOS-BOOT 2.0.0 |

> **Authority of this document.**
> This document states, precisely and reproducibly, the **initialization procedure by which a
> brand-new AEOS repository is bootstrapped**: the directories a conformant repository creates, the
> documents it places and where each is placed, the non-runtime configuration it establishes at the
> moment of creation, the order in which these actions occur, the checks that confirm the result, and
> the repository state that a completed bootstrap must leave behind.
>
> This document is an **Implementation Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that
> layer: concrete guidance for building to specification, expressed at the point specification meets
> construction. It is not a Product document, not a Vision document, not an Architecture document,
> not a Blueprint, not a Runtime document, and not a behavioral Specification under AEOS-SPECSTD. It
> states no product requirement, no architectural decision, no Blueprint arrangement, no specified
> behavior, no runtime lifecycle, and no terminology; where a statement here appears to do any of
> these, that is a defect in this document and MUST be reported rather than acted upon. It states no
> procedure for how AEOS executes once bootstrapped — that question belongs to the Runtime layer,
> answered by `AEOS_RUNTIME_FLOW.md` and the documents it composes — and it does not restate the
> content of any document it places; it names and locates that content only.
>
> AEOS-DOCSTD Section 4.1 positions Implementation Guides beneath Specification and above Developer
> Guides in the documentation hierarchy. No Implementation Guide has previously been authored for
> AEOS. This document is written under AEOS-DOCSTD's general document template and the Section 4.3
> purpose statement for this layer, in the absence of a dedicated Implementation Guide Standard —
> in the same spirit `AEOS_RUNTIME_FLOW.md` and `RUNTIME_REGISTRY.md` adopted AEOS-SPECSTD's
> discipline voluntarily where no dedicated Standard yet existed for the Runtime layer. It does not,
> on that account, establish such a Standard; a future Implementation Guide Standard remains reserved
> to the owner under AEOS-DOCSTD `H5`, and nothing in this document binds a future Implementation
> Guide to the conventions it adopts for itself alone.
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
3. [Prerequisites](#3-prerequisites)
4. [Repository Directory Structure](#4-repository-directory-structure)
5. [Required Documents and Placement](#5-required-documents-and-placement)
6. [Initial Configuration](#6-initial-configuration)
7. [Bootstrap Sequence](#7-bootstrap-sequence)
8. [Verification](#8-verification)
9. [Expected Repository State](#9-expected-repository-state)
10. [Non-Goals](#10-non-goals)
11. [Traceability](#11-traceability)
12. [References](#12-references)
13. [Document Governance](#13-document-governance)
14. [Appendix A — Document Placement Index](#appendix-a--document-placement-index)
15. [Appendix B — BOOT Rule Index](#appendix-b--boot-rule-index)
16. [Appendix C — Bootstrap Checklist (Non-Normative)](#appendix-c--bootstrap-checklist-non-normative)

---

## 1. Executive Summary

A repository is only the product AEOS-VISION invariant `V5` says it is if what invariant requires can
actually be found in it — by the human who arrives first, and by the AI runtime that arrives with no
memory of anything that was not written down. Every document this repository holds at the moment this
guide is written already exists in frozen, freeze-candidate, or draft form. Further documents will be
authored after it. What does not yet exist, before bootstrap runs, is a directory on disk that
contains the current set of them, structured the way the documentation hierarchy expects, so that the
moment the repository is first obtained — by `git clone`, by an installer, or by any other
Distribution Method AEOS-PRD `PR-DST` recognizes — already leaves a reader able to find every document
the hierarchy requires, in the place the hierarchy expects it.

This document closes that gap. It states, once, the complete and ordered procedure by which a new
AEOS repository is initialized: what directories are created, what documents are placed and where,
what minimal non-runtime configuration a repository carries from the moment of its creation, and what
a reader or an automated check can verify once the procedure completes. It derives the placement of
each document from that document's own metadata rather than from a fixed list maintained here, so
that a future document arriving in an existing directory requires no revision to this guide — only a
new directory, naming a layer this guide does not yet place, does. It states nothing about how AEOS
behaves once it is running against a bootstrapped repository — that is the Runtime layer's question,
not this one — and it introduces no product behavior, no architectural decision, and no terminology of
its own. It is, deliberately, the least interesting document in the repository: a correct bootstrap is
one where nothing that happens is surprising, and every step is repeatable by a different person, on
a different machine, and produces the same result.

Four properties bind the procedure this document states, per the standard this document is written
under:

| Property | What it requires of this document |
| :--- | :--- |
| **Reproducible** | Given the same set of source documents, the procedure in [Section 7](#7-bootstrap-sequence) produces the same repository state, described in [Section 9](#9-expected-repository-state), every time it is followed. |
| **Deterministic** | The order of actions in [Section 7](#7-bootstrap-sequence) is fixed; no step depends on a choice this document leaves unstated. |
| **Platform-neutral** | Every action is stated as *what* occurs, never as an operating-system-specific command; AEOS-PRD `PR-PLT-003` and `PR-PLT-005` require that repository semantics behave identically on every supported Platform, and this procedure is one such semantic. |
| **Distribution-neutral** | Repository initialization does not depend on any one Distribution Method; where a Distribution Method involves version control, [Section 7](#7-bootstrap-sequence) performs that as a separate, conditional step, per `BOOT-027` and consistent with AEOS-PRD `PR-DST-005` and `PR-DST-006`. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the initialization of a new AEOS repository, and nothing beyond it:

- the complete set of top-level directories a bootstrapped repository contains;
- the rule by which the documents a bootstrapped repository contains are identified and placed,
  expressed through each document's own metadata rather than through a fixed list maintained here;
- the minimal, non-runtime configuration a repository carries from the moment of its creation;
- the ordered sequence of actions by which the above is accomplished;
- the checks by which a completed bootstrap is verified;
- the repository state a correctly bootstrapped repository is in once verification passes;
- what bootstrap explicitly does not do, so that a reader does not search this document, or a future
  document, for a capability bootstrap was never meant to have.

This list is complete.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, and frozen | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH |
| How that structure is arranged to be built | AEOS-BLUEPRINT |
| How each defined behavior must work, precisely and testably | Specification documents, under AEOS-SPECSTD |
| How AEOS executes, in what environment, with what lifecycle | Runtime documents, beginning with `AEOS_RUNTIME_FLOW.md` |
| How a person works day to day within a bootstrapped repository | Developer Guides, none of which yet exist for AEOS |
| What code realizes any capability named above | The codebase and its tests |

A statement in this document that redefines Product ownership, an Architecture Layer, a Blueprint
arrangement, a Specification's behavioral content, or a Runtime document's stated lifecycle is a
**defect in this document**. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to the initialization of a new AEOS repository intended to carry forward the
AEOS 1.0 document set. It applies once, at the moment a repository is created; it is not re-run
against an existing, already-bootstrapped repository except to verify that repository under
[Section 8](#8-verification). It applies identically to a human performing bootstrap directly and to
an AI runtime performing it on a human's behalf, consistent with AEOS-DOCSTD Section 2.4.

This document does not retroactively apply to a repository already initialized by other means before
this document existed. Such a repository is brought into conformance, if desired, by verification
under [Section 8](#8-verification) followed by whatever ordinary change corrects any finding — never
by re-running bootstrap against a non-empty repository, which [Section 10](#10-non-goals) excludes.

## 3. Prerequisites

Bootstrap assumes the following are true before it begins. Bootstrap does not establish any of them;
it halts and reports if one is missing, per `BOOT-001`.

| # | Prerequisite |
| :--- | :--- |
| 1 | A target location exists that is either empty, or contains no `docs/` directory and no file this guide would place, so that bootstrap never overwrites content it did not create. |
| 2 | Every document intended for the repository — every document whose own metadata block records a Suggested path under `docs/` — is available as a source file, unmodified, from wherever the AEOS document set is currently held. [Appendix A](#appendix-a--document-placement-index) records the set known at the time of this guide's authoring. |
| 3 | The environment performing bootstrap can create UTF-8 encoded text files and directories; no further capability is assumed. |
| 4 | Where the chosen Distribution Method is one that depends on version control — ordinarily the case for AEOS-PRD `PR-DST-001`'s GitHub Clone distribution — a version-control system capable of recording an initial commit is available. A Distribution Method that does not depend on version control, such as `PR-DST-004`'s portable, self-contained distribution, does not require this prerequisite. |

No prerequisite above names an operating system, a shell, or a package manager. Prerequisite 4 is
conditional on the Distribution Method chosen; [Section 7](#7-bootstrap-sequence) separates repository
initialization, which every Distribution Method requires, from source-control initialization, which
only some do, consistent with AEOS-PRD `PR-DST-005` and `PR-DST-006`'s requirement that no product
capability be exclusive to one distribution method. Consistent with AEOS-PRD `PR-PLT-001` and
`PR-PLT-002`, this document states no capability exclusive to one Platform, and the precise command
that satisfies a prerequisite on a given Platform, or with a given version-control system, is left to
the environment performing bootstrap, never to this document.

## 4. Repository Directory Structure

A bootstrapped repository contains exactly the top-level directories below, each holding the
documents [Section 5](#5-required-documents-and-placement) places within it. No directory is created
for a layer that holds no document at bootstrap time.

| Directory | Houses |
| :--- | :--- |
| `docs/foundation/` | Every Foundation-layer document, present or future — currently Vision, Product Requirements, Glossary, Document Standard, and Supported Technologies. |
| `docs/architecture/` | Every Architecture-layer document, present or future — currently Architecture and Blueprint. |
| `docs/product/` | The Specification Standard, at the path its own metadata records — see the note beneath this table. |
| `docs/specification/` | Every Specification-layer document, present or future: the System Specification and every document that registers a behavior-domain area code beneath it. |
| `docs/runtime/` | Every Runtime-layer document, present or future, written under AEOS-DOCSTD Section 4.5's unassigned-layer provision. |
| `docs/implementation/` | Every Implementation Guide, present or future, including this document once it is placed — documents answering *how specified behavior is realized*, per AEOS-DOCSTD Section 4.1. |

A directory in this table houses every document of its layer that exists at bootstrap time, however
many that is; a new document arriving in an already-listed layer does not enlarge this table and does
not, by itself, require a revision of this document. Only the addition of a layer this table does not
yet name — a new top-level directory — is a change to this section, per
[Section 13.2](#132-change-control).

> **On the Implementation / Developer boundary.** A document that states a procedure by which
> specified behavior is built — this document included — belongs in `docs/implementation/`. A
> document that instructs a person on working within an already-built result — onboarding,
> day-to-day workflow, code style, review process, troubleshooting, contribution, or frequently asked
> questions — belongs in a **Developer Guide**, per AEOS-DOCSTD Section 4.1's own distinction between
> the two layers. AEOS 1.0 names no Developer Guide yet, so this table names no directory for one.
> Should a Developer Guide be authored, [Section 5](#5-required-documents-and-placement)'s discovery
> rule places it, and this table's one-directory-per-layer pattern gives it a directory — most
> naturally `docs/developer/` — the moment that document's own Suggested-path metadata field says so,
> without requiring a revision of this table in advance. This document does not name any such
> Developer Guide, and does not decide the internal organization either layer eventually adopts as it
> grows.

> **On `docs/product/`.** Every other layer in this table is housed in a directory named for that
> layer. AEOS-SPECIFICATION_STANDARD.md's own metadata block records its Suggested path as
> `docs/product/SPECIFICATION_STANDARD.md` rather than `docs/architecture/`, where the remainder of
> this table's pattern would place it. `BOOT-004` requires that a document be placed at the path its
> own metadata records; this document does not resolve the apparent inconsistency, since AEOS-SPECSTD's
> own Suggested path is that document's own content, governed by its own change control under
> AEOS-SPECSTD Section 18, not by this one. The inconsistency is recorded here as a finished statement
> for a future revision of AEOS-SPECSTD to resolve, rather than silently corrected by this document.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `BOOT-001` | Bootstrap MUST halt and report, without creating any directory or placing any document, if a prerequisite in [Section 3](#3-prerequisites) is not met. | `PR-SAF-002` |
| `BOOT-002` | A bootstrapped repository MUST contain exactly the directories in this table's first column, and MUST NOT contain an additional top-level directory bootstrap itself created. | `PR-NFR-002` |
| `BOOT-003` | Directory names and nesting MUST match this table exactly, including case. | `PR-PLT-003` |

## 5. Required Documents and Placement

Bootstrap places every source document made available under Prerequisite 2 that carries a recognized
Status and a Suggested-path metadata field placing it under `docs/`. This is a placement *rule*, not a
placement *list*: it is evaluated against whatever documents are actually presented to bootstrap, so
that an AEOS document authored after this guide is placed correctly without this guide naming it in
advance. [Appendix A](#appendix-a--document-placement-index) records the set known at the time of this
guide's authoring, as a non-normative convenience; it is not the mechanism by which bootstrap decides
what to place, and it is not itself extended merely because a new document exists — see the appendix's
own heading note.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `BOOT-004` | Each document MUST be placed at the path its own metadata block records as its Suggested path. Where [Appendix A](#appendix-a--document-placement-index) and a placed document's own Suggested-path field differ, the document's own field governs, and the difference is a finding against Appendix A, not against the placement. | AEOS-DOCSTD `H2` |
| `BOOT-005` | A document's content MUST be placed unmodified: byte-for-byte identical to its source. Bootstrap performs no edit, no reformatting, and no correction, however small. | AEOS-VISION `V5` |
| `BOOT-006` | A document's Document ID, Version, and Status, as recorded in its own metadata block, MUST NOT be altered by placement. | AEOS-DOCSTD `H2` |
| `BOOT-007` | The set of documents bootstrap places is exactly the set of source documents made available under Prerequisite 2 that meet `BOOT-008`'s Status requirement and carry a Suggested-path field placing them under `docs/`. A source document meeting this test is placed whether or not [Appendix A](#appendix-a--document-placement-index) lists it; omitting one that does meet this test is a defect in the bootstrap run, not a discretionary choice. | `PR-NFR-002` |
| `BOOT-008` | Bootstrap MUST NOT place a document whose own Status field is neither `Frozen`, `Freeze candidate`, nor `Draft`, without the owner's explicit direction recorded outside this procedure. | AEOS-DOCSTD Section 12.1 |

## 6. Initial Configuration

Beyond the documents themselves, a bootstrapped repository carries a small amount of configuration
established once, at creation, and never revisited by this procedure.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `BOOT-009` | The repository root MUST contain a `README.md` stating the repository's name, one paragraph describing AEOS drawn from AEOS-VISION and AEOS-PRD without restating either, and a list of links to the entry-point documents in `docs/foundation/`. | `PR-REP-009` |
| `BOOT-010` | Where Prerequisite 4 applies, the repository root MUST contain a version-control ignore file. It MUST NOT exclude any path under `docs/`. | `PR-REP-009` |
| `BOOT-011` | Bootstrap MUST NOT create, populate, or reference Runtime State, a credential, a secret, or a selection of a specific Runtime, Adapter, or Model. | `PR-SAF-006` · `PR-REP-013` · `PR-REP-015` |
| `BOOT-012` | Bootstrap MUST NOT establish CI/CD configuration. Integration with a project's continuous-integration and delivery systems is reserved to those systems themselves. | `PR-REP-007` |
| `BOOT-013` | Bootstrap MUST NOT select or record a Distribution Method beyond the version-control initialization [Section 7](#7-bootstrap-sequence) performs. | `PR-DST-006` |

`README.md` and the ignore file are the only files bootstrap creates outside `docs/`. Bootstrap makes
no license determination, no ownership declaration, and no governance decision beyond what
AEOS-VISION and AEOS-PRD already state; a license file, where the project owner wants one, is that
owner's decision and is added outside this procedure.

## 7. Bootstrap Sequence

The sequence below is closed and ordered. Each step's entry condition is the prior step's completion;
no step is reordered, skipped, or parallelized in a way that changes what a later step observes.
Followed twice against the same source documents and the same Prerequisite 4 applicability, the
sequence produces the same repository state, per `BOOT-014`. Step 3 and Step 11 are the only steps
conditional on Prerequisite 4; every other step occurs regardless of Distribution Method, per
`BOOT-027`.

| Step | Action | Entry condition |
| :--- | :--- | :--- |
| 1 | Confirm every prerequisite in [Section 3](#3-prerequisites). | None; this is the first step. |
| 2 | Establish the repository root at the target location confirmed by Prerequisite 1. This is repository initialization. | Step 1 complete. |
| 3 | Where Prerequisite 4 applies, initialize the version-control system at the repository root, or confirm a clean, conflict-free existing one. This is source-control initialization, distinct from Step 2. | Step 2 complete. |
| 4 | Create the directory structure in [Section 4](#4-repository-directory-structure), in the order that table lists. | Step 2 complete (Step 3, where applicable). |
| 5 | Place every Foundation document at its designated path. | Step 4 complete. |
| 6 | Place the Architecture and Blueprint documents. | Step 5 complete. |
| 7 | Place the Specification Standard and every Specification-layer document. | Step 6 complete. |
| 8 | Place every Runtime-layer document. | Step 7 complete. |
| 9 | Place this document, `AEOS-BOOT`, at `docs/implementation/PROJECT_BOOTSTRAP.md`. | Step 8 complete. |
| 10 | Establish initial configuration ([Section 6](#6-initial-configuration)). | Step 9 complete. |
| 11 | Where Prerequisite 4 applies, record a single initial commit containing exactly the tree Steps 4 through 10 produced, and nothing else. Where it does not apply, the tree Steps 4 through 10 produced is itself the reproducible initial state. | Step 10 complete. |
| 12 | Perform verification ([Section 8](#8-verification)). | Step 11 complete. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `BOOT-014` | The sequence above MUST be followed in the order stated; a later step MUST NOT begin before its stated entry condition is satisfied. | `PR-NFR-002` |
| `BOOT-015` | Every action in the sequence MUST be expressible as a Platform-neutral operation — create a directory, place a file, record a commit — never as a command specific to one operating system or shell. | `PR-PLT-003` · `PR-PLT-005` |
| `BOOT-016` | Where Prerequisite 4 applies, Step 11's commit MUST contain no file bootstrap did not itself create or place in Steps 4 through 10. | `PR-REP-013` |
| `BOOT-017` | An interruption at any step MUST leave the repository in a state Step 12 can describe as incomplete rather than as silently partial. | `PR-SAF-010` |
| `BOOT-027` | Step 2 (repository initialization) MUST occur regardless of Distribution Method. Step 3 and Step 11 (source-control initialization and its commit) MUST occur only where Prerequisite 4 applies, and their absence MUST NOT prevent Steps 4 through 10 or Step 12 from completing. | `PR-DST-005` · `PR-DST-006` |

## 8. Verification

Verification confirms that a bootstrap run — or an existing repository presented for conformance
review — matches this document. It is read-only: verification inspects the repository and reports; it
never corrects it.

| ID | Check | Traces to |
| :--- | :--- | :--- |
| `BOOT-018` | Every directory in [Section 4](#4-repository-directory-structure) exists, and no undeclared top-level directory bootstrap would have created is present. | `BOOT-002` |
| `BOOT-019` | Every source document that met `BOOT-007`'s test at the time of the bootstrap run is present at the path its own Suggested-path metadata field records. | `BOOT-004` |
| `BOOT-020` | Every placed document is byte-for-byte identical to its source; a diff of any placed document against its source is empty. | `BOOT-005` |
| `BOOT-021` | Every placed document's Document ID, Version, and Status match its own source metadata block exactly. | `BOOT-006` |
| `BOOT-022` | `README.md` exists at the repository root, and every link it contains resolves to a document actually present in the repository. | `BOOT-009` |
| `BOOT-023` | Where Prerequisite 4 applied, the version-control ignore file excludes no path beginning with `docs/`. | `BOOT-010` |
| `BOOT-024` | No file anywhere in the repository contains a credential, a secret, a Runtime State record, or a recorded Runtime, Adapter, or Model selection. | `BOOT-011` |
| `BOOT-025` | Where Prerequisite 4 applied, the repository's first commit contains exactly the tree Steps 4 through 10 of [Section 7](#7-bootstrap-sequence) produced, and no other commit precedes it. | `BOOT-016` |

A verification run that finds no failure against `BOOT-018` through `BOOT-025` confirms the
repository is in the state [Section 9](#9-expected-repository-state) describes. A verification run
that finds a failure reports it; correcting the failure is an ordinary change to the repository, not
a re-run of [Section 7](#7-bootstrap-sequence), per `NG-8` in [Section 10](#10-non-goals).

## 9. Expected Repository State

The tree below is a non-normative illustration of a repository immediately after
[Section 7](#7-bootstrap-sequence) completes and [Section 8](#8-verification) passes, showing the
document set known at the time of this guide's authoring and the ordinary, version-controlled case
under Prerequisite 4. It is a snapshot, not an enumeration: a future document arriving in an
already-listed directory ([Section 4](#4-repository-directory-structure)) enlarges this tree without
requiring a revision of this guide, per `BOOT-026`. Filenames follow each document's own
Suggested-path metadata field; where three Specification documents' own metadata has not been
independently confirmed by this guide, their source filename is used, per the note beneath
[Appendix A](#appendix-a--document-placement-index). `.gitignore` illustrates the version-control
ignore file `BOOT-010` requires only where Prerequisite 4 applies; where it does not, the tree omits
this file and Step 11's non-commit alternative applies.

```text
<repository root>
├── README.md
├── OVERVIEW.md
├── CONTRIBUTING.md
├── LICENSE
├── SECURITY.md
├── CODE_OF_CONDUCT.md
├── CHANGELOG.md
├── .gitignore
└─ docs
   ├─ architecture
   │  ├─ AEOS_ARCHITECTURE.md
   │  └─ AEOS_BLUEPRINT.md
   ├─ foundation
   │  ├─ AEOS_DOCUMENT_STANDARD.md
   │  ├─ AEOS_GLOSSARY.md
   │  ├─ AEOS_PRODUCT_REQUIREMENTS.html
   │  ├─ AEOS_PRODUCT_REQUIREMENTS.md
   │  ├─ AEOS_SUPPORTED_TECHNOLOGIES.md
   │  └─ AEOS_VISION.md
   ├─ framework
   │  ├─ 01_constitution
   │  │  └─ AI_DEVELOPMENT_PHILOSOPHY_v2.0.md.md
   │  ├─ 02_framework_rules
   │  │  └─ global_rules_revisionfinal_v10.md
   │  ├─ 03_technology_standards
   │  │  └─ global_technology_stack_v10.md
   │  ├─ 04_project_rules
   │  │  ├─ project-mobile_v01.md
   │  │  ├─ project-monolithic_v04.md
   │  │  ├─ project-pc-app_v04.md
   │  │  └─ project-personal-full-stack_v01.md
   │  ├─ 05_developer_manuals
   │  │  ├─ COMMANDS.md
   │  │  ├─ PROJECT_BOOTSTRAP_GUIDE.md
   │  │  └─ PROJECT_STRUCTURE.md
   │  ├─ 07_ai-skills
   │  │  ├─ SKILLS.md
   │  │  ├─ skill-create-feature.md
   │  │  ├─ skill-generate-tests.md
   │  │  └─ skill-review-code.md
   │  ├─ 08_prompt_library
   │  │  ├─ CLAUDE_CODE_PROMPTS.md
   │  │  └─ OPENAI_PROMPTS.md
   │  ├─ 09_project_templates
   │  │  └─ TEMPLATE_SPEC.md
   │  ├─ 10_knowledge_base
   │  │  ├─ DECISIONS.md
   │  │  └─ FRAMEWORK_HANDOVER.md
   │  ├─ 11_reference_documents
   │  │  └─ V10_MIGRATION_NOTES.md
   │  ├─ BLUEPRINT_INPUT_FREEZE.md
   │  ├─ FRAMEWORK_BLUEPRINT.md
   │  ├─ FRAMEWORK_README.md
   │  └─ FRAMEWORK_STATUS.md
   ├─ implementation
   │  ├─ BEST_PRACTICES.md
   │  ├─ CODING_STANDARD.md
   │  ├─ CONFIGURATION.md
   │  ├─ DEVELOPMENT_WORKFLOW.md
   │  ├─ ENVIRONMENT_SETUP.md
   │  ├─ FAQ.md
   │  ├─ INSTALLATION.md
   │  ├─ PROJECT_BOOTSTRAP.md
   │  ├─ REPOSITORY_LAYOUT.md
   │  ├─ REVIEW_GUIDE.md
   │  └─ TROUBLESHOOTING.md
   ├─ product
   │  └─ AEOS_SPECIFICATION_STANDARD.md
   ├─ runtime
   │  ├─ AEOS_RUNTIME_FLOW.md
   │  ├─ RUNTIME_CAPABILITY_SPEC.md
   │  └─ RUNTIME_REGISTRY.md
   └─ specification
      ├─ AEOS_CONTEXT_ROUTER.md
      ├─ AEOS_SPEC.md
      ├─ AEOS_STATE_MACHINE.md
      ├─ AEOS_WORKFLOW_ENGINE.md
      ├─ MODEL_REGISTRY.md
      ├─ RUNTIME_ADAPTER_SPEC.md
      └─ RUNTIME_NEGOTIATION_SPEC.md
	
		
```

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `BOOT-026` | The repository's actual state MUST conform to the directory structure [Section 4](#4-repository-directory-structure) defines and MUST contain every document `BOOT-007` requires; the tree above illustrates one valid instance of that state and MUST NOT be read as an exhaustive or permanently fixed enumeration. A divergence from the *structure* the tree illustrates is a defect; a repository that additionally holds a document the tree does not list, because that document did not yet exist when this guide was authored, is not. | `PR-NFR-001` |

This tree holds no Runtime State, no credential, no build artifact, and no file this document did
not direct Step 4 through Step 10 of [Section 7](#7-bootstrap-sequence) to create. Consistent with
AEOS-PRD `PR-REP-016`, every file in it remains readable and meaningful whether or not AEOS is
running.

## 10. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document, in
the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Installing, configuring, or launching any AEOS Runtime, Adapter, or Model. | Runtime documents and future Implementation Guides for the Adapter and Runtime layers. |
| `NG-2` | Performing Workflow execution or engaging the system interaction loop. | `AEOS_SPEC.md`, `AEOS_RUNTIME_FLOW.md`. |
| `NG-3` | Packaging the repository for a Distribution Method beyond the repository initialization, and, where Prerequisite 4 applies, the source-control initialization, [Section 7](#7-bootstrap-sequence) performs. | AEOS-PRD `PR-DST`, and future Implementation Guides. |
| `NG-4` | Configuring a project's CI/CD systems. | The project's own existing systems, per `PR-REP-007`. |
| `NG-5` | Authoring or revising the content of any document this guide places. | Each document's own owner and change control. |
| `NG-6` | Producing Developer Guides. None yet exist for AEOS 1.0. | A future Developer Guide, once authored. |
| `NG-7` | Establishing credentials, secrets, or a license, ownership, or governance decision beyond what AEOS-VISION and AEOS-PRD already state. | The project owner, outside this procedure. |
| `NG-8` | Correcting an existing, already-bootstrapped repository found to diverge from [Section 9](#9-expected-repository-state). | An ordinary change to the repository, verified again under [Section 8](#8-verification); never a re-run of [Section 7](#7-bootstrap-sequence). |
| `NG-9` | Defining the Implementation Guide layer's Standard, identifier conventions, or structure for documents other than this one. | Reserved to the owner under AEOS-DOCSTD `H5`. |

## 11. Traceability

Every `BOOT-` rule in this document traces to one or more `PR-` identifiers, stated inline in
[Sections 4](#4-repository-directory-structure) through [9](#9-expected-repository-state) and
indexed in full in [Appendix B](#appendix-b--boot-rule-index). The table below summarizes trace
density by `PR-` prefix, consistent with the practice AEOS-SPECSTD `CM2` establishes for
Specification documents and which this document adopts for itself under
[Section 2.3](#23-applicability) of AEOS-DOCSTD's general discipline.

| `PR-` prefix | Rules in this document that trace to it |
| :--- | :--- |
| `PR-REP` | `BOOT-005` · `BOOT-006` · `BOOT-007` · `BOOT-009` · `BOOT-010` · `BOOT-011` · `BOOT-012` · `BOOT-016` |
| `PR-SAF` | `BOOT-001` · `BOOT-011` · `BOOT-017` |
| `PR-PLT` | `BOOT-003` · `BOOT-015` |
| `PR-DST` | `BOOT-013` · `BOOT-027` |
| `PR-NFR` | `BOOT-002` · `BOOT-004` (via AEOS-DOCSTD `H2`) · `BOOT-007` · `BOOT-014` · `BOOT-026` |

Two rules trace to AEOS-DOCSTD directly rather than to AEOS-PRD, because they state an obligation
AEOS-DOCSTD itself imposes on every derivative document (`H2`, `H4`) rather than a product
requirement: `BOOT-004` and `BOOT-006`. This document, as a whole, traces to AEOS-DOCSTD `H4`'s
requirement that every derivative document trace to the layer above it, satisfied here by tracing
each rule to the `PR-` requirement it ultimately serves, and to AEOS-DOCSTD `H6`'s requirement that a
document belong to a layer, satisfied by this document's position as an Implementation Guide, stated
in the authority statement at the head of this document.

## 12. References

| Document | Document ID | Cited for |
| :--- | :--- | :--- |
| `AEOS_VISION.md` | AEOS-VISION | Invariant `V5` (the repository is the product), cited in [Section 1](#1-executive-summary) and [Section 9](#9-expected-repository-state). |
| `AEOS_PRODUCT_REQUIREMENTS.md` | AEOS-PRD | The `PR-REP`, `PR-SAF`, `PR-PLT`, `PR-DST`, and `PR-NFR` identifiers this document's rules trace to, indexed in [Section 11](#11-traceability). |
| `AEOS_GLOSSARY.md` | AEOS-GLOSSARY | Terminology used without redefinition throughout this document, including *Repository Asset*, *Runtime State*, and *Platform*. |
| `AEOS_DOCUMENT_STANDARD.md` | AEOS-DOCSTD | The documentation hierarchy, the Implementation Guide layer's purpose (Section 4.3), the general document template, and the hierarchy rules (`H2`, `H4`, `H5`, `H6`) this document is written under. |
| `AEOS_SUPPORTED_TECHNOLOGIES.md` | AEOS-TECH | Referenced for completeness as a placed Foundation document; this document names no technology choice of its own. |
| `AEOS_ARCHITECTURE.md` | AEOS-ARCH | Referenced for completeness as a placed Architecture document; this document states no structural decision of its own. |
| `AEOS_BLUEPRINT.md` | AEOS-BLUEPRINT | Referenced for completeness as a placed Architecture-layer document; this document states no arrangement of its own. |
| `AEOS_SPECIFICATION_STANDARD.md` | AEOS-SPECSTD | The Specification layer's own Suggested-path convention, cited in the note beneath [Section 4](#4-repository-directory-structure)'s table. |
| `AEOS_SPEC.md` and its companions | AEOS-SPEC, AEOS-SPEC-WFL, AEOS-SPEC-CTX, AEOS-SPEC-STM | Placed as Specification-layer documents; none of their behavioral content is restated here. |
| `RUNTIME_NEGOTIATION_SPEC.md`, `RUNTIME_ADAPTER_SPEC.md`, `MODEL_REGISTRY.md` | AEOS-SPEC-NEG, AEOS-SPEC-ADP, AEOS-SPEC-MDL | Placed as Specification-layer documents at their recorded lifecycle states, per [Appendix A](#appendix-a--document-placement-index). |
| `AEOS_RUNTIME_FLOW.md`, `RUNTIME_REGISTRY.md`, `RUNTIME_CAPABILITY_SPEC.md` | AEOS-RTF, AEOS-RUNTIME-REG, AEOS-CAP | Placed as Runtime-layer documents; [Section 1](#1-executive-summary) and the authority statement distinguish their subject from this document's. |

## 13. Document Governance

### 13.1 Status

This document is a **Draft**. It is the first Implementation Guide authored for AEOS, and is intended
to become the Project Bootstrap Source of Truth once the owner's review under
[Section 13.4](#134-review-policy) records no Critical or Major finding, at which point it is
intended to be placed and frozen alongside the AEOS 1.0 document set: AEOS-VISION, AEOS-PRD,
AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-SPEC,
AEOS-SPEC-WFL, AEOS-SPEC-CTX, AEOS-SPEC-STM, and the Runtime documents this guide places.

### 13.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Refreshing [Appendix A](#appendix-a--document-placement-index) or [Section 9](#9-expected-repository-state)'s illustrative content to reflect a document that fits an existing directory in [Section 4](#4-repository-directory-structure) | Contributor change, owner acceptance | Patch |
| Addition of a new `BOOT-<NNN>` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Any change to what an existing `BOOT-<NNN>` rule requires, or its retirement | Explicit owner revision request | Major |
| Change to the directory structure in [Section 4](#4-repository-directory-structure) | Explicit owner revision request | Major |
| Any change that would invalidate a repository already bootstrapped under a prior version | Explicit owner revision request, with the reasoning preserved in place | Major |

### 13.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-DOCSTD, or
the documentation hierarchy are recorded as recommendations for the owning document's governance and
are applied only after explicit owner approval there — never enacted here.

### 13.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in [Sections 4](#4-repository-directory-structure) through
      [9](#9-expected-repository-state) carries a `BOOT-<NNN>` identifier and a trace.
- [ ] [Appendix A](#appendix-a--document-placement-index) lists every document known at the time of
      review, consistent with its status as a non-normative, point-in-time index; `BOOT-007`, not
      Appendix A, is confirmed as the rule actually constraining what [Section 7](#7-bootstrap-sequence)
      places.
- [ ] No rule restates the content of any placed document.
- [ ] No rule states a runtime behavior, an architectural decision, or a product requirement.
- [ ] All sixteen entries in this document's Table of Contents are present, in order, and none is
      silently empty.
- [ ] No Critical or Major finding remains open.

### 13.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document's [Appendix A](#appendix-a--document-placement-index) conflicts with a placed document's own Suggested-path metadata field | The document's own field governs, per `BOOT-004`. The conflict is a finding against Appendix A. |
| This document conflicts with a Specification document on the mechanics of that document's own behavior domain | The owning Specification governs; this document is corrected to reference it rather than restate it. |
| A future Implementation Guide states a directory or placement convention that conflicts with [Section 4](#4-repository-directory-structure) | The apparent need is reported against this document. It is not resolved by a contradictory statement in the other guide. |

### 13.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Project Bootstrap Guide. States the prerequisites, six-directory structure, document placement rules, initial non-runtime configuration, eleven-step bootstrap sequence, eight-check verification procedure, and expected repository state for a new AEOS repository, together with twenty-six `BOOT-<NNN>` rules and nine non-goals. References every Foundation, Architecture, Blueprint, Runtime, and Specification document existing at the time of authoring. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no runtime procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPECSTD. |
| 2.0.0 | Draft | Implementation review pass. Reduces long-term maintenance coupling to the AEOS document inventory: `BOOT-007`, `BOOT-019`, and `BOOT-021` now derive the set of documents bootstrap places, and the checks against them, from each source document's own metadata rather than from [Appendix A](#appendix-a--document-placement-index); Appendix A and [Section 9](#9-expected-repository-state)'s tree are recast as non-normative, point-in-time illustrations, and `BOOT-026` is revised accordingly, so that a future AEOS document arriving in an existing directory requires no revision of this guide. Separates repository initialization from source-control initialization: Prerequisite 4 and Steps 3 and 11 of [Section 7](#7-bootstrap-sequence) make version control conditional on the Distribution Method, `BOOT-010`, `BOOT-016`, `BOOT-023`, and `BOOT-025` are made conditional accordingly, and new rule `BOOT-027` states the split normatively, consistent with AEOS-PRD `PR-DST-004` through `PR-DST-006`; Git remains fully supported as the ordinary case. [Section 4](#4-repository-directory-structure)'s directories are reworded to house each layer's documents "present or future" without enlarging the directory set. [Section 13.2](#132-change-control) downgrades routine Appendix A / Section 9 refreshes to Patch level. No `BOOT-<NNN>` identifier is renumbered or retired; no Foundation, Architecture, Blueprint, Runtime, or Specification document's ownership, the bootstrap scope stated in [Section 2](#2-scope-and-applicability), or the governance model in this section is altered. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no runtime procedure. |
| 3.0.0 | Draft | Adds a non-normative note to [Section 4](#4-repository-directory-structure) distinguishing Implementation Guide content (how specified behavior is realized) from Developer Guide content (how a person works within the result), per AEOS-DOCSTD Section 4.1's own wording, so that future guides on topics such as environment setup, development workflow, code review, or contribution have an unambiguous, correctly layered home when they are actually authored — `docs/implementation/` for the former, a future Developer Guide's own directory for the latter. Does not create a directory, reserve a path, or otherwise assert structure for any Developer Guide, since none is yet authored: an earlier draft of this revision added such a directory and a rule requiring it be created empty at bootstrap time, and both were removed on review, because (a) most version-control systems, Git included, cannot represent an empty directory in a commit, making the requirement unsatisfiable under Prerequisite 4's ordinary case, and (b) reserving structure for a document that does not yet exist is the same error this revision's own note declines to make for any other hypothetical future document, and [Section 5](#5-required-documents-and-placement)'s discovery rule already places a real Developer Guide, directory included, the moment one is authored, without requiring this guide's revision. Does not name, place, or otherwise assert the existence of any Developer Guide, and does not add a directory outside `docs/`: candidate structures proposed outside the frozen AEOS document set were evaluated and not adopted, because naming unauthored documents would reintroduce the coupling Version 2.0.0 removed, and a root-level directory split beyond `README.md` has no basis in AEOS-VISION, AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-DOCSTD. No `BOOT-<NNN>` identifier already present in Version 2.0.0 is renumbered or retired; no Foundation, Architecture, Blueprint, Runtime, or Specification document's ownership, the bootstrap scope stated in [Section 2](#2-scope-and-applicability), or the governance model in this section is altered. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no runtime procedure. |

---

## Appendix A — Document Placement Index

**This appendix is non-normative.** Placement itself is governed by `BOOT-004` and `BOOT-007`, which
derive a document's location from its own Suggested-path metadata field and its own recorded Status,
not from this index. This appendix records, as a convenience to a reader and a reviewer, the set of
documents known to exist at the time of this document's authoring, and is refreshed as a Patch-level
edit ([Section 13.2](#132-change-control)) whenever convenient. A document's absence from this index is
never, by itself, a reason to withhold its placement under `BOOT-007`, and a document's presence here
does not excuse bootstrap from checking its own metadata field before placing it. Status and Version
below are reproduced from each document's own metadata block as of authoring; where a document's own
metadata later changes, that document's own field governs, per `BOOT-004` and
[Section 13.5](#135-precedence).

### Foundation Documents

| Source filename | Document ID | Version | Status | Placement |
| :--- | :--- | :--- | :--- | :--- |
| `AEOS_VISION.md` | AEOS-VISION | 1.0.1 | Frozen | `docs/foundation/VISION.md` |
| `AEOS_PRODUCT_REQUIREMENTS.md` | AEOS-PRD | 1.1.1 | Frozen | `docs/foundation/PRD.md` |
| `AEOS_GLOSSARY.md` | AEOS-GLOSSARY | 1.0.1 | Frozen | `docs/foundation/GLOSSARY.md` |
| `AEOS_DOCUMENT_STANDARD.md` | AEOS-DOCSTD | 3.0.0 | Frozen | `docs/foundation/DOCUMENT_STANDARD.md` |
| `AEOS_SUPPORTED_TECHNOLOGIES.md` | AEOS-TECH | 1.0.1 | Frozen | `docs/foundation/SUPPORTED_TECHNOLOGIES.md` |

### Architecture Documents

| Source filename | Document ID | Version | Status | Placement |
| :--- | :--- | :--- | :--- | :--- |
| `AEOS_ARCHITECTURE.md` | AEOS-ARCH | 1.1.0 | Frozen | `docs/architecture/ARCHITECTURE.md` |
| `AEOS_BLUEPRINT.md` | AEOS-BLUEPRINT | 1.1.0 | Frozen | `docs/architecture/BLUEPRINT.md` |

### Specification Documents

| Source filename | Document ID | Area code | Version | Status | Placement |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `AEOS_SPECIFICATION_STANDARD.md` | AEOS-SPECSTD | — | 1.1.0 | Frozen | `docs/product/SPECIFICATION_STANDARD.md`\* |
| `AEOS_SPEC.md` | AEOS-SPEC | `SYS` | 1.0.1 | Frozen | `docs/specification/SYSTEM_SPECIFICATION.md` |
| `AEOS_WORKFLOW_ENGINE.md` | AEOS-SPEC-WFL | `WFL` | — † | Freeze candidate † | `docs/specification/AEOS_WORKFLOW_ENGINE.md` † |
| `AEOS_CONTEXT_ROUTER.md` | AEOS-SPEC-CTX | `CTX` | — † | Freeze candidate † | `docs/specification/AEOS_CONTEXT_ROUTER.md` † |
| `AEOS_STATE_MACHINE.md` | AEOS-SPEC-STM | `STM` | — † | Freeze candidate † | `docs/specification/AEOS_STATE_MACHINE.md` † |
| `RUNTIME_NEGOTIATION_SPEC.md` | AEOS-SPEC-NEG | `NEG` | 1.0.0 | Draft | `docs/specification/RUNTIME_NEGOTIATION_SPEC.md` |
| `RUNTIME_ADAPTER_SPEC.md` | AEOS-SPEC-ADP | `ADP` | 1.0.0 | Draft | `docs/specification/RUNTIME_ADAPTER_SPEC.md` |
| `MODEL_REGISTRY.md` | AEOS-SPEC-MDL | `MDL` | 1.0.0 | Freeze candidate | `docs/specification/MODEL_REGISTRY.md` |

\* Placed per AEOS-SPECSTD's own Suggested-path field; see the note beneath
[Section 4](#4-repository-directory-structure)'s table.

† AEOS_RUNTIME_FLOW.md Section 12.1 records `AEOS-SPEC-WFL`, `AEOS-SPEC-CTX`, and `AEOS-SPEC-STM` as
already authored and intended for freeze alongside it, but this document has not independently
confirmed their Version and Suggested-path metadata fields. Their placement here follows the
`docs/specification/<source filename>` convention every other row in this table observes. `BOOT-004`
requires that each document's own metadata field govern at the time of placement; this row is
superseded by that field wherever the two differ.

### Runtime Documents

Written under AEOS-DOCSTD Section 4.5's unassigned-layer provision.

| Source filename | Document ID | Version | Status | Placement |
| :--- | :--- | :--- | :--- | :--- |
| `AEOS_RUNTIME_FLOW.md` | AEOS-RTF | 1.0.0 | Freeze candidate | `docs/runtime/RUNTIME_FLOW.md` |
| `RUNTIME_REGISTRY.md` | AEOS-RUNTIME-REG | 1.0.0 | Freeze candidate | `docs/runtime/RUNTIME_REGISTRY.md` |
| `RUNTIME_CAPABILITY_SPEC.md` | AEOS-CAP | 1.0.0 | Freeze candidate | `docs/runtime/RUNTIME_CAPABILITY_SPEC.md` |

### Implementation Documents

| Source filename | Document ID | Version | Status | Placement |
| :--- | :--- | :--- | :--- | :--- |
| `PROJECT_BOOTSTRAP.md` | AEOS-BOOT | 3.0.0 | Draft | `docs/implementation/PROJECT_BOOTSTRAP.md` |

## Appendix B — BOOT Rule Index

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 4](#4-repository-directory-structure) through
[9](#9-expected-repository-state).

| ID | Section | Short label | Traces to |
| :--- | :--- | :--- | :--- |
| `BOOT-001` | 4 | Halt and report on missing prerequisite | `PR-SAF-002` |
| `BOOT-002` | 4 | Exactly the directories this document states | `PR-NFR-002` |
| `BOOT-003` | 4 | Directory names and nesting match exactly | `PR-PLT-003` |
| `BOOT-004` | 5 | Placement follows a document's own Suggested path | AEOS-DOCSTD `H2` |
| `BOOT-005` | 5 | Content placed unmodified | AEOS-VISION `V5` |
| `BOOT-006` | 5 | Document ID, Version, Status unaltered | AEOS-DOCSTD `H2` |
| `BOOT-007` | 5 | Exactly the document set meeting the Status and Suggested-path test | `PR-NFR-002` |
| `BOOT-008` | 5 | No placement without a recognized Status | AEOS-DOCSTD Section 12.1 |
| `BOOT-009` | 6 | Root `README.md` required | `PR-REP-009` |
| `BOOT-010` | 6 | Ignore file excludes no `docs/` path (conditional on Prerequisite 4) | `PR-REP-009` |
| `BOOT-011` | 6 | No Runtime State, credential, or selection | `PR-SAF-006` · `PR-REP-013` · `PR-REP-015` |
| `BOOT-012` | 6 | No CI/CD configuration established | `PR-REP-007` |
| `BOOT-013` | 6 | No Distribution Method selection beyond version control | `PR-DST-006` |
| `BOOT-014` | 7 | Sequence order fixed | `PR-NFR-002` |
| `BOOT-015` | 7 | Every action Platform-neutral | `PR-PLT-003` · `PR-PLT-005` |
| `BOOT-016` | 7 | Initial commit contains only what bootstrap placed (conditional on Prerequisite 4) | `PR-REP-013` |
| `BOOT-017` | 7 | Interruption leaves a describable state | `PR-SAF-010` |
| `BOOT-018` | 8 | Directories verified present and undeclared ones absent | `BOOT-002` |
| `BOOT-019` | 8 | Every qualifying source document present at its recorded path | `BOOT-004` |
| `BOOT-020` | 8 | Every placed document byte-identical to source | `BOOT-005` |
| `BOOT-021` | 8 | Metadata fields match each document's own source | `BOOT-006` |
| `BOOT-022` | 8 | README links resolve | `BOOT-009` |
| `BOOT-023` | 8 | Ignore file excludes no `docs/` path (conditional on Prerequisite 4) | `BOOT-010` |
| `BOOT-024` | 8 | No credential, secret, or Runtime State present | `BOOT-011` |
| `BOOT-025` | 8 | First commit matches what Steps 4–10 placed (conditional on Prerequisite 4) | `BOOT-016` |
| `BOOT-026` | 9 | Repository state conforms to Section 4's structure and `BOOT-007`'s set | `PR-NFR-001` |
| `BOOT-027` | 7 | Repository init unconditional; source-control init conditional | `PR-DST-005` · `PR-DST-006` |

## Appendix C — Bootstrap Checklist (Non-Normative)

A practical restatement of [Section 7](#7-bootstrap-sequence) and [Section 8](#8-verification), for
a contributor or an AI runtime working through bootstrap directly. This checklist carries no
authority of its own; where it appears to diverge from Sections 3 through 9, those sections govern.

- [ ] Prerequisites confirmed (target location clean; every intended source document on hand;
      where the Distribution Method depends on version control, that system available).
- [ ] Repository root established (repository initialization — always).
- [ ] Where applicable, version control initialized at the repository root (source-control
      initialization — conditional on Prerequisite 4).
- [ ] `docs/foundation/`, `docs/architecture/`, `docs/product/`, `docs/specification/`,
      `docs/runtime/`, and `docs/implementation/` created.
- [ ] Foundation documents placed.
- [ ] Architecture and Blueprint documents placed.
- [ ] Specification Standard and every Specification document placed.
- [ ] Every Runtime document placed.
- [ ] This document placed at `docs/implementation/PROJECT_BOOTSTRAP.md`.
- [ ] `README.md` written, linking every `docs/foundation/` document.
- [ ] Where applicable, ignore file written, excluding nothing under `docs/`.
- [ ] Where applicable, initial commit recorded.
- [ ] Every check in [Section 8](#8-verification) passes.

---

**End of Project Bootstrap Guide**

AEOS-BOOT · Version 3.0.0 · Traces to `PR-REP` · `PR-SAF` · `PR-PLT` · `PR-DST` · `PR-NFR`, placing
AEOS-VISION · AEOS-PRD · AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-TECH · AEOS-ARCH · AEOS-BLUEPRINT ·
AEOS-SPECSTD · AEOS-SPEC · AEOS-SPEC-WFL · AEOS-SPEC-CTX · AEOS-SPEC-STM · AEOS-SPEC-NEG ·
AEOS-SPEC-ADP · AEOS-SPEC-MDL · AEOS-RTF · AEOS-RUNTIME-REG · AEOS-CAP without restating any of them
