# AI Engineering Operating System

## AEOS — Contribution Guide

*The permanent statement of how a proposed change enters the AEOS repository.*

| Field | Value |
| :--- | :--- |
| **Document** | Contribution Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-CONTRIB |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Contributors — human and AI — proposing a change to the AEOS repository, and the reviewers and owner who evaluate that change |
| **Suggested path** | `docs/developer/CONTRIBUTING.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states, for the AEOS repository, the **procedure by which a proposed change enters
> it**: the philosophy a Contributor is held to, the general shape of a contribution as it moves
> through the repository, how issues, branches, commits, pull requests, reviews, and freezes are
> used, how a contribution to documentation, to the Framework, or to a Runtime integration is
> distinguished, high-level coding expectations, how AI-assisted contribution and Human Approval
> work together, and how a Contributor's copy of the repository stays synchronized with the
> authoritative one.
>
> This document is a **Developer Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that layer:
> task-oriented instruction for contributors — setup, workflow, conventions in practice,
> troubleshooting. It is not a Product document, not a Vision document, not an Architecture document,
> not a Blueprint, not a Specification, not a Runtime document, and not an Implementation Guide. It
> states no product requirement, no architectural decision, no Blueprint arrangement, no specified
> behavior, no runtime procedure, and no terminology; where a statement here appears to do any of
> these, that is a defect in this document and MUST be reported rather than acted upon. It does not
> restate the content of any document it references; it names and applies that content only.
>
> This document addresses the **Contributor**, in the sense AEOS-GLOSSARY fixes that term: a person
> or AI runtime proposing a change to AEOS itself — to its documents, its assets, or its
> Implementation. It does not address the **Developer** — a person who uses AEOS to build or
> maintain their own software project. AEOS-GLOSSARY's *Contributor* entry states that one individual
> may hold both roles, and that a document MUST state which one it addresses; this document addresses
> the Contributor role throughout, and a statement here MUST NOT be read as governing a Developer's
> own governed Project.
>
> **On this document's placement.** AEOS-LAYOUT `LAYOUT-004` and `LAYOUT-005` fix the AEOS repository
> root to `README.md`, `docs/`, the version-control ignore file where applicable, and an optional
> license file, and MUST NOT be read as naming this document there. AEOS-BOOT `BOOT-028` reserves
> `docs/developer/` for the Developer Guides layer AEOS-DOCSTD Section 4.1 names, and AEOS-BOOT
> version 3.0.0's revision history anticipates contribution by name as a future Developer Guide
> topic. This document is accordingly placed at `docs/developer/CONTRIBUTING.md`, and its own
> Suggested-path metadata field governs its placement, per AEOS-BOOT `BOOT-004`.
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
3. [Contribution Philosophy](#3-contribution-philosophy)
4. [Repository Workflow](#4-repository-workflow)
5. [Issue Workflow](#5-issue-workflow)
6. [Branch Strategy](#6-branch-strategy)
7. [Commit Policy](#7-commit-policy)
8. [Pull Request Policy](#8-pull-request-policy)
9. [Review Policy](#9-review-policy)
10. [Freeze Policy](#10-freeze-policy)
11. [Documentation Contribution](#11-documentation-contribution)
12. [Framework Contribution](#12-framework-contribution)
13. [Runtime Contribution](#13-runtime-contribution)
14. [Coding Expectations](#14-coding-expectations)
15. [AI-assisted Contribution](#15-ai-assisted-contribution)
16. [Human Approval](#16-human-approval)
17. [Repository Synchronization](#17-repository-synchronization)
18. [Non-Goals](#18-non-goals)
19. [Traceability](#19-traceability)
20. [References](#20-references)
21. [Document Governance](#21-document-governance)
22. [Appendix A — CONTRIB Rule Index (Non-Normative)](#appendix-a--contrib-rule-index-non-normative)
23. [Appendix B — Contribution Checklist (Non-Normative)](#appendix-b--contribution-checklist-non-normative)

---

## 1. Executive Summary

AEOS asks every Developer who adopts it to hold their own engineering to a stated discipline:
explain before executing, verify before implementing, keep a human deciding, and let the repository
— not a chat session — carry what was learned. AEOS-VISION states, without exemption, that this
discipline applies to AEOS itself: TDD-first development "applies to AEOS itself with no exemption. A
product that enforces a discipline it does not follow is arguing against its own thesis," and Guiding
Principle `G10` states plainly, "test first, including here." The AEOS repository is the first and
most visible proof of whether the product's claims are real.

This document is where that proof is stated as a procedure rather than left to be inferred. It
answers one question: how does a proposed change enter the AEOS repository, so that a Contributor
arriving for the first time — human or AI — can be right on the first attempt, and so that a reviewer
never has to explain an unwritten expectation. It draws its content from documents already frozen or
in force for AEOS — the Vision's guiding principles for contributors, the Product Requirements'
mandatory principles and interaction model, the Document Standard's review and freeze lifecycle, the
Architecture's extension model, and the Bootstrap and Repository Layout guides' structure — and adds
nothing those documents do not already establish. Where a governing document has not yet decided
something this guide would otherwise need, that gap is recorded in
[Section 18](#18-non-goals) rather than filled by assumption.

## 2. Scope and Applicability

### 2.1 Purpose

This document exists to state, once, how a change reaches the AEOS repository: the philosophy a
contribution is measured against, the workflow it moves through, the review and freeze it is subject
to, and the boundaries between a documentation contribution, a Framework contribution, and a Runtime
contribution. It exists so that "how do I contribute to AEOS" has one answer, stated in the
repository, rather than an answer that varies by who is asked.

### 2.2 What This Document Governs

This document governs, for the AEOS repository:

- the philosophy a Contributor is held to, and where the authority for that philosophy lives;
- the general shape of a contribution as it moves through the repository, from intent to a change in
  repository state;
- how issues, branches, commits, pull requests, reviews, and freezes are used to move a contribution
  through that shape;
- how a Documentation contribution, a Framework contribution, and a Runtime contribution are
  distinguished, and which governing document each is measured against;
- high-level expectations for a change to AEOS's own Implementation, stated without prescribing a
  language, framework, or style;
- how an AI-assisted contribution is treated, and how Human Approval applies to it;
- how a Contributor's own copy of the AEOS repository is expected to stay synchronized with the
  authoritative one;
- what this document deliberately does not decide, so that a reader does not search this document, or
  a future one, for a decision that has not yet been made.

This list is complete.

### 2.3 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, reviewed, and frozen | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH |
| How that structure is arranged to be built | AEOS-BLUEPRINT |
| How each defined behavior must work, precisely and testably | Specification documents, under AEOS-SPECSTD |
| How a new AEOS repository is initialized | AEOS-BOOT |
| What belongs at each repository path | AEOS-LAYOUT |
| How AEOS executes, in what environment, with what lifecycle | Runtime documents, beginning with `AEOS_RUNTIME_FLOW.md` |
| What code realizes any capability named above | The codebase and its tests |
| How a Developer's own governed Project accepts contributions | The Project and its own conventions |

A statement in this document that grants a capability, imposes a product requirement, defines a term,
decides a structure, specifies behavior, or states a concrete implementation mechanism is a **defect
in this document**. It MUST be reported rather than acted upon.

### 2.4 Applicability

This document applies to every proposed change to the AEOS repository — a document, a Repository
Asset, or Implementation — regardless of whether the change originates from a human Contributor or an
AI runtime acting as one, consistent with AEOS-DOCSTD Section 2.4's statement that documentation rules
bind human and AI authors identically. It does not apply to work a Developer performs within their own
governed Project, which is governed by that Project's own conventions.

### 2.5 Recorded Deviation

AEOS-DOCSTD Section 7.3 states that the Developer Guide layer SHOULD NOT use normative keywords, on
the ground that a guide instructs and obligations belong to the documents that own them. This document
uses normative keywords in its policy sections — [Repository Workflow](#4-repository-workflow) through
[Human Approval](#16-human-approval) — because those sections state enforceable, checkable
expectations for how a contribution is accepted, and because every obligation stated normatively here
already exists in a governing document; this document restates none of them as a new obligation, and
states only the contribution-time consequence of an obligation stated elsewhere. Removing the keywords
would leave that consequence unenforceable. The deviation is recorded here as AEOS-DOCSTD Section 7.2
requires of a deliberate deviation from a SHOULD, following the precedent AEOS-GLOSSARY Section 2.4
already records for itself.

---

## 3. Contribution Philosophy

This document does not restate the twelve guiding principles AEOS-VISION Section 9 states for
contributors, or the thirteen mandatory product principles AEOS-PRD Section 7 states for the product.
Both already exist, are already authoritative, and restating them here would violate AEOS-DOCSTD
`DS-P-07`. What follows names where each lives and states the contribution-time consequence a
Contributor is held to.

| `CONTRIB-001` | A contribution to the AEOS repository MUST be evaluated against the guiding principles AEOS-VISION Section 9 states (`G1`–`G12`), in addition to any rule this document states specifically. |
| :--- | :--- |
| `CONTRIB-002` | A contribution that violates a mandatory product principle in AEOS-PRD Section 7 is rejected on that basis alone, independent of the contribution's technical merit. |
| `CONTRIB-003` | Test-first development applies to AEOS's own Implementation without exemption, per AEOS-VISION invariant `V4` and AEOS-PRD Section 7.4; a contribution that reverses this order is treated as a process violation, not a shortcut. |
| `CONTRIB-004` | A Contributor who is uncertain whether a contribution fits AEOS's stated purpose or scope asks the owner rather than proceeding on assumption, per Guiding Principle `G12`. |

Four consequences of that philosophy are worth stating plainly, because they are the ones most often
tested at the moment a contribution is actually made:

| Guiding principle | What it means for a contribution here |
| :--- | :--- |
| `G3` Keep architectural responsibilities separate | A contribution that blurs Product, Architecture, Specification, Runtime, and Implementation is rejected even where the change itself is otherwise sound. |
| `G6` Human approval is the default | Where a contribution would reduce what a human decides, that reduction is the change under review — not an incidental detail of it. |
| `G8` Record better ideas; do not apply them silently | An improvement to a frozen document's concepts is proposed as a recommendation for the owner's approval, per [Section 10](#10-freeze-policy); it is never an unannounced change. |
| `G11` Leave the repository more understandable than you found it | A contribution that passes every other check and leaves the AEOS repository harder to understand has still fallen short. |

## 4. Repository Workflow

A contribution to the AEOS repository moves through one general shape, regardless of whether it
touches documentation, the Framework, or a Runtime integration: intent is recorded, work proceeds on
an isolated line, the result is proposed for review, a human decides, and — only once approved — the
AEOS repository's state changes. [Sections 5](#5-issue-workflow) through
[10](#10-freeze-policy) state each stage of that shape in turn.

| `CONTRIB-005` | A contribution MUST leave the AEOS repository in a state that AEOS-BOOT `BOOT-018` through `BOOT-025` and AEOS-LAYOUT's invariants would still verify correctly; a contribution that a bootstrap verification pass would fail is incomplete. |
| :--- | :--- |
| `CONTRIB-006` | A contribution MUST NOT establish CI/CD configuration, select a Distribution Method, or record a credential, consistent with AEOS-BOOT `BOOT-012`, `BOOT-013`, and Non-Goal `NG-4`. |
| `CONTRIB-007` | A contribution MUST place every file it adds or moves according to that file's own kind, per AEOS-LAYOUT `LAYOUT-011` through `LAYOUT-013`, and MUST NOT add an entry at the AEOS repository root beyond what AEOS-LAYOUT `LAYOUT-004` names. |
| `CONTRIB-008` | This document does not fix a specific version-control system, hosting platform, or automation tool for the AEOS repository's own development; AEOS-PRD `PR-DST-001` names GitHub Clone as an official Distribution Method for AEOS, and this document's remaining sections use the vocabulary of that context — "pull request" among them — without requiring it, consistent with AEOS-PRD Section 7.7's vendor independence principle and AEOS-VISION invariant `V6`. |

## 5. Issue Workflow

An issue records an observed defect, an ambiguity, or a proposed change before work begins on it. It
is the repository's form of the Explain-before-Propose discipline AEOS-PRD Section 10 states for the
product itself, applied to the act of contributing: a Contributor states what was found and what is
intended before proposing a change that realizes it.

| `CONTRIB-009` | An issue that reports a finding against a Frozen or Freeze-candidate document MUST classify that finding as Critical, Major, Minor, or Nitpick, per AEOS-DOCSTD Section 12.3, and MUST NOT introduce a fifth class. |
| :--- | :--- |
| `CONTRIB-010` | An issue SHOULD reference every identifier it concerns — a `PR-`, `AR-`, `BP-`, `SP-`, `DS-`, `BOOT-`, `LAYOUT-`, or `CONTRIB-` identifier, as applicable — so that the traceability [Section 19](#19-traceability) requires can be followed from the point the contribution begins. |
| `CONTRIB-011` | An issue that finds two governing documents making conflicting statements about one subject MUST follow AEOS-DOCSTD Section 11.3: determine the owning document, treat its statement as correct for the purpose of proceeding, and report the conflict against the non-owning document. It MUST NOT be resolved by the issue's author. |
| `CONTRIB-012` | An issue proposing an idea that would alter a frozen document's concepts, capability set, or principles is recorded as a recommendation for the owner's approval, per Guiding Principle `G8` and each governing document's own "Relationship to the Architecture Freeze" section; it is not treated as an approved change by virtue of being filed. |

## 6. Branch Strategy

A contribution proceeds on a line of work isolated from the AEOS repository's default line until it is
ready for review, consistent with AEOS-VISION's Incremental Execution philosophy (`6.3`): work that
advances in small, inspectable steps can be interrupted without damage and reviewed before it
compounds. This document states that principle and no more.

| `CONTRIB-013` | Work on a contribution MUST proceed in a way that does not alter the AEOS repository's default line until the contribution has passed the review stated in [Section 9](#9-review-policy). |
| :--- | :--- |
| `CONTRIB-014` | A branch, or its equivalent under the version-control system in use, MUST correspond to one contribution with one stated intent; a branch that accumulates unrelated changes is split before review, consistent with Guiding Principle `G2`'s constraint against unnecessary complexity. |
| `CONTRIB-015` | This document fixes no branch-naming scheme and no default-branch name; a concrete naming convention, where one is adopted, is recorded in a future Implementation Guide rather than assumed here, per [Non-Goal `NG-1`](#18-non-goals). |

## 7. Commit Policy

A commit records what changed and, where the change is not self-evident from a diff, why. This
document states the properties a commit is expected to carry; it fixes no specific commit-message
schema.

| `CONTRIB-016` | A commit SHOULD be small enough to review as one inspectable step, consistent with AEOS-VISION's Incremental Execution philosophy (`6.3`) and AEOS-PRD Section 7.3. |
| :--- | :--- |
| `CONTRIB-017` | A commit that advances a specific requirement, rule, or identifier SHOULD reference that identifier, using the reference forms AEOS-DOCSTD Section 11.2 states. |
| `CONTRIB-018` | A commit that changes AEOS's own Implementation MUST NOT precede the test that verifies it, per AEOS-VISION invariant `V4`, except where a human has explicitly acknowledged the exception, consistent with the Glossary's *TDD Cycle* entry. |
| `CONTRIB-019` | A commit MUST NOT contain a credential, a secret, or Runtime State, per AEOS-LAYOUT `LAYOUT-040`. |
| `CONTRIB-020` | This document fixes no specific commit-message format; a Contributor states what changed and why in language a reviewer who was not present for the reasoning can act on, per AEOS-VISION philosophy `6.2`, Explain Before Execute. |

## 8. Pull Request Policy

A pull request — or the hosting platform's equivalent change-review mechanism, where the AEOS
repository is hosted somewhere other than the context AEOS-PRD `PR-DST-001` names — is the unit at
which a contribution is proposed for review. It is the repository's form of a Proposal, in the sense
AEOS-GLOSSARY fixes that term: a statement of intended change, its rationale, its effect, and the
consequence of declining it.

| `CONTRIB-021` | A pull request MUST state what changed, why, and which identifiers — `PR-`, `AR-`, `BP-`, `SP-`, `DS-`, `BOOT-`, `LAYOUT-`, or `CONTRIB-` — it advances, per [Section 19](#19-traceability). |
| :--- | :--- |
| `CONTRIB-022` | A pull request that touches a Frozen or Freeze-candidate document MUST state whether it proposes an editorial correction, a clarification, or a change requiring the owning document's own change control, using that document's own change-control table to determine which. |
| `CONTRIB-023` | A pull request MUST NOT be self-approved; it is subject to [Section 9](#9-review-policy) and [Section 16](#16-human-approval) regardless of who authored it. |
| `CONTRIB-024` | A pull request that expands beyond what its own description states requires a new pull request or an explicitly updated description, consistent with AEOS-PRD Section 10's Confirm-Execute discipline: an approval authorizes what was proposed, and scope expansion requires a new proposal. |
| `CONTRIB-025` | A pull request MUST NOT be merged while a Critical or Major finding raised against it remains open, per AEOS-DOCSTD `DS-P-04`. |

## 9. Review Policy

Review is the point at which the AEOS repository's stated standard is actually applied, per
AEOS-GLOSSARY's *Review* entry, and the point at which a contribution — including one an AI runtime
authored — becomes owned work. Nothing enters the AEOS repository without a human having had a real
opportunity to reject it, per AEOS-VISION philosophy `6.9`, Review before Execution.

| `CONTRIB-026` | A reviewer of a contribution to an AEOS document MUST classify every finding as Critical, Major, Minor, or Nitpick, per AEOS-DOCSTD Section 12.3, and MUST NOT extend, rename, or reorder that four-class set. |
| :--- | :--- |
| `CONTRIB-027` | A reviewer MUST identify inconsistencies and MUST NOT rewrite or redesign the contribution under review, per AEOS-DOCSTD Section 12.4. |
| `CONTRIB-028` | A reviewer MUST cite the specific location of a finding and, where the finding is a violation of a governing document's rule, that rule's identifier. |
| `CONTRIB-029` | A reviewer who believes a contribution's premise is wrong records that as a single Critical finding and escalates, rather than raising the same objection repeatedly across the contribution, per AEOS-DOCSTD Section 12.4. |
| `CONTRIB-030` | A review examines the result; it does not substitute for the explanation the Contributor owed the reviewer before proposing the change, per AEOS-VISION philosophy `6.9`'s distinction between explanation and review. Both are required. |
| `CONTRIB-031` | A contribution accepted into the AEOS repository is thereby owned by the human who approved it, exactly as AEOS-VISION philosophy `6.9` states for any material entering a repository AEOS governs; authorship by an AI runtime does not change this. |

## 10. Freeze Policy

Freeze is a governance state, not a technical lock, per AEOS-GLOSSARY's *Freeze* entry: a document's
definitions and decisions do not change except through the change control that document itself
defines. AEOS-DOCSTD Section 12 states six lifecycle states and a five-stage lifecycle for every AEOS
document; the summary below orients a Contributor to those states and is marked, per AEOS-DOCSTD
Section 11.2, as a summary rather than the statement of record.

| State (AEOS-DOCSTD Section 12.1) | What it means for a contribution |
| :--- | :--- |
| Draft | Not yet authoritative; a contribution MAY propose any change to it. |
| In Review | Findings are being gathered; a contribution MUST NOT alter the document's content mid-pass. |
| In Revision | Only changes that address a recorded finding are in scope. |
| Approved | Only an editorial correction is in scope without further owner approval. |
| Frozen | Authoritative; a contribution proposing a change of meaning is routed through that document's own change control, not merged by ordinary review alone. |
| Superseded | No further contribution is accepted against it. |

For a Specification document specifically, AEOS-SPECSTD Section 19's freeze checklist governs in
addition to the table above, and is not restated here.

| `CONTRIB-032` | A contribution MUST NOT change the meaning of a Frozen document except through that document's own change-control table, even where the reviewer and the Contributor agree the change is an improvement. |
| :--- | :--- |
| `CONTRIB-033` | A contribution that finds a defect in a Frozen document reports the defect and proposes it as a recommendation under that document's own governance; it does not silently correct the defect in a downstream document instead, per AEOS-DOCSTD Section 11.3. |
| `CONTRIB-034` | A contribution that adds a new rule, requirement, or identifier to a document that already carries an identifier sequence MUST extend that sequence; it MUST NOT reuse, renumber, or repurpose an existing identifier, per AEOS-DOCSTD `E3`. |
| `CONTRIB-035` | A contribution recommending freeze for a document under review confirms, at minimum, that no Critical or Major finding remains open, per AEOS-DOCSTD Section 12.2, Stage 3's exit condition. |
| `CONTRIB-036` | A document that has never been frozen carries no obligation to preserve a prior version's wording; a document that has been frozen and is being revised is bound by that document's own recorded change-control table. |

## 11. Documentation Contribution

A documentation contribution is governed, in full, by AEOS-DOCSTD: the twelve documentation
principles, the documentation hierarchy, the responsibility boundaries, the GitHub-Flavored Markdown
format standard, and the review-and-freeze lifecycle already stated in
[Sections 9](#9-review-policy) and [10](#10-freeze-policy). This section states only the
consequence specific to the moment a document is contributed.

| `CONTRIB-037` | A documentation contribution MUST NOT restate a definition, requirement, or decision owned by another document; it references the owning document instead, per AEOS-DOCSTD `DS-P-07` and Section 11.2. |
| :--- | :--- |
| `CONTRIB-038` | A documentation contribution MUST be written in GitHub-Flavored Markdown and MUST NOT contain raw HTML, except as AEOS-DOCSTD Section 8.3's exception procedure permits. |
| `CONTRIB-039` | A documentation contribution that encounters a gap a governing document has not resolved MUST record the gap as a stated non-goal or open question; it MUST NOT fill the gap by assumption, per AEOS-DOCSTD `DS-P-10` and AEOS-LAYOUT `LP5`. |
| `CONTRIB-040` | A documentation contribution's placement is governed by that document's own Suggested-path metadata field, per AEOS-BOOT `BOOT-004` and AEOS-LAYOUT `LAYOUT-012`; it is not chosen by convenience or by where an editor happened to create the file. |
| `CONTRIB-041` | Before its content is written, a documentation contribution's author reads every governing document it will trace to, per AEOS-DOCSTD Section 2.4's identical binding on human and AI authors. |

## 12. Framework Contribution

A Framework contribution, as this document uses the term informally, is a contribution to AEOS's own
Implementation of the Architecture's internal layers other than a contribution admitted at
Architectural Extension Point `EP-3` — that is, everything realizing the Workflow, Context, Runtime,
Execution, or Repository Layer, or the Adapter Layer's own internal structure, as distinct from the
addition of support for one more external Runtime, which
[Section 13](#13-runtime-contribution) governs. "Framework" is not a term AEOS-GLOSSARY defines; where
this document's use of it appears to conflict with a defined term, the defined term governs.

| `CONTRIB-042` | A Framework contribution MUST NOT alter a Frozen Architectural Invariant stated in AEOS-ARCH Section 8, including the Layer, Dependency, Boundary, Extension, and Architectural Governance Invariants, without an explicit architecture revision under AEOS-ARCH's own change control. |
| :--- | :--- |
| `CONTRIB-043` | A Framework contribution that fits an Architectural Extension Point AEOS-ARCH Section 11.2 names — `EP-1`, `EP-2`, `EP-4`, `EP-5`, or `EP-6` — is admitted without an architecture revision, per Architectural Principle `AR-PRN-009`, Extension over modification. |
| `CONTRIB-044` | A Framework contribution MUST trace to the `PR-`, `AR-`, `BP-`, or `SP-` identifiers it realizes, per AEOS-DOCSTD `H4` and AEOS-ARCH Section 12.4's traceability obligations. |
| `CONTRIB-045` | A Framework contribution's choice of language, library, or tool is constrained by AEOS-TECH's Scope `P` (Product) entries — the technologies AEOS itself is built on — and by that scope's obligation that the choice function equivalently on Windows, macOS, and Linux, per AEOS-TECH `TG-002`. |
| `CONTRIB-046` | A Framework contribution MUST NOT introduce a layer, a dependency, or a permitted interaction that AEOS-ARCH does not already define, per Extension Invariant `AR-EXT-004`. |

## 13. Runtime Contribution

A Runtime contribution is one admitted at Architectural Extension Point `EP-3`: support for an
additional external Runtime, including a Model provider reached through it, attaching at the Adapter
Layer, per AEOS-ARCH Section 11.2 and 11.3. It is governed by the Runtime-layer and
Specification-layer documents already in force for this purpose: `AEOS_RUNTIME_FLOW.md` (AEOS-RTF),
`RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG), `RUNTIME_ADAPTER_SPEC.md` (AEOS-SPEC-ADP),
`RUNTIME_NEGOTIATION_SPEC.md` (AEOS-SPEC-NEG), `RUNTIME_CAPABILITY_SPEC.md` (AEOS-CAP), and
`MODEL_REGISTRY.md` (AEOS-SPEC-MDL). This document does not restate their content; it states only the
boundary a Runtime contribution MUST respect.

| `CONTRIB-047` | A Runtime contribution MUST attach at the Adapter Layer and MUST NOT require a change to the Workflow, Context, or Repository Layer, per AEOS-ARCH Section 4.6 and 4.7's stated dependencies and prohibited responsibilities. |
| :--- | :--- |
| `CONTRIB-048` | A Runtime contribution MUST NOT hold engineering policy, an Approval Gate, or context-selection logic; those remain the Workflow and Context Layers' responsibility, per AEOS-ARCH Section 4.7's prohibited responsibilities for the Adapter Layer. |
| `CONTRIB-049` | A Runtime contribution MUST NOT privilege the Vendor, Runtime, or Model it adds over any other, per AEOS-VISION invariant `V6` and AEOS-PRD Section 7.7; its acceptance criteria are the same regardless of the Vendor behind it. |
| `CONTRIB-050` | A Runtime contribution MUST NOT record a credential in any durable artifact, per Boundary Invariant `AR-BND-009`. |
| `CONTRIB-051` | A Runtime contribution declares the Engineering Capabilities it performs, consistent with AEOS-GLOSSARY's *Runtime Adapter* and *Engineering Capability* entries, so that a Workflow step's requirement can be matched against it before work begins. |

## 14. Coding Expectations

This section states expectations at the level AEOS-DOCSTD Section 4.3 permits a Developer Guide to
reach; it prescribes no language, framework, test tool, or style rule. A dedicated Coding Standard, if
and when one exists, is the correct home for that level of detail, per [Non-Goal `NG-3`](#18-non-goals).

| `CONTRIB-052` | A test precedes the Implementation it verifies, per AEOS-VISION invariant `V4` and the Glossary's *TDD Cycle* entry: define behavior, write the failing test, confirm it fails for the right reason, implement the minimum that passes, refactor under a green suite. |
| :--- | :--- |
| `CONTRIB-053` | Skipping the test-first cycle is an explicit exception a human acknowledges; it is never silent and never the default, per the Glossary's *TDD Cycle* entry. |
| `CONTRIB-054` | A contribution to Implementation is judged, where two solutions are otherwise equivalent, by AEOS-VISION's Simplicity over Cleverness (`6.14`) and Explicit over Implicit (`6.15`) philosophies: the plainer, more explicit solution is preferred. |
| `CONTRIB-055` | Every significant Implementation decision is defensible against AEOS-VISION's Long-term Maintainability test (`6.16`): whether someone who inherits it in five years can understand it, change it, and be glad it was done this way. |
| `CONTRIB-056` | A contribution's technology choice is recorded so that its recognition status under AEOS-TECH can be checked; an unrecognized technology is not thereby prohibited, but its adoption is reported rather than assumed. |

## 15. AI-assisted Contribution

AEOS-DOCSTD Section 2.4 states that its rules bind human and AI authors identically, and that "an AI
runtime generating an AEOS document MUST comply with this Standard; a contributor accepting
AI-generated documentation into the repository is responsible for that compliance." This document
extends that same treatment to every kind of contribution, not documentation alone, consistent with
AEOS-GLOSSARY's definition of *Contributor* as "a person or AI runtime proposing a change to AEOS
itself."

| `CONTRIB-057` | An AI-assisted contribution is subject to every rule in this document that a human-authored contribution is subject to; no rule in this document is relaxed on the ground that a runtime produced the change. |
| :--- | :--- |
| `CONTRIB-058` | An AI runtime contributing to AEOS acts as an Engineering Partner, per AEOS-VISION philosophy `6.7`: a capable participant contributing work and reasoning within a discipline it did not set, neither a mechanical tool nor an unaccountable authority. |
| `CONTRIB-059` | The human who submits or accepts an AI-assisted contribution is accountable for it, per AEOS-VISION philosophy `6.1`: responsibility for what enters the AEOS repository cannot be delegated to a system that cannot be held responsible for it. |
| `CONTRIB-060` | A placeholder, an invented identifier, an unauthorized term, or an assumed gap-filling in an AI-assisted contribution is a defect exactly as it would be in a human-authored one, per AEOS-DOCSTD `DS-P-10` and the non-invention rules `T1` through `T5`. |
| `CONTRIB-061` | An AI runtime contributing to the AEOS repository reads every document it will trace to before proposing a change, and states an assumption explicitly rather than silently, consistent with AEOS-VISION philosophy `6.15`, Explicit over Implicit. |

## 16. Human Approval

Human Approval is the explicit act by which a person authorizes a specific proposed action before it
is executed, per AEOS-GLOSSARY's *Human Approval* entry. Applied to a contribution, it is the act a
review under [Section 9](#9-review-policy) exists to produce, and it is what makes a contribution —
however it was authored — owned work.

| `CONTRIB-062` | Silence is not Human Approval of a contribution. Ambiguity is not Human Approval. Approval of a different contribution is not Human Approval of this one, per AEOS-GLOSSARY's *Human Approval* entry. |
| :--- | :--- |
| `CONTRIB-063` | Human Approval authorizes exactly the contribution that was proposed; a contribution that grows beyond its stated scope after approval requires a new approval, per AEOS-PRD Section 10's Confirm phase. |
| `CONTRIB-064` | Nothing enters the AEOS repository without a human having had a real opportunity to reject it, per AEOS-VISION philosophy `6.9`; an Automation Grant, where one exists for a repository operation, MUST NOT satisfy this requirement for a destructive action, per AEOS-PRD `PR-SAF-012`. |
| `CONTRIB-065` | Where a contribution reduces what a human decides about the AEOS repository, that reduction is itself the change under review, per Guiding Principle `G6`, not an incidental detail of a larger change. |

## 17. Repository Synchronization

A Contributor's working copy of the AEOS repository is expected to reflect the repository's
authoritative state before work begins and before a pull request is proposed, consistent with
AEOS-DOCSTD `DS-P-08`: sources of truth never conflict, and a divergence is resolved at the owning
copy — the authoritative one — never by a local reinterpretation.

| `CONTRIB-066` | A Contributor updates their working copy from the AEOS repository's authoritative state before beginning work and again before proposing a pull request. |
| :--- | :--- |
| `CONTRIB-067` | Where a Contributor's working copy has diverged because another accepted contribution landed first, the Contributor reconciles by updating from the authoritative state; a conflict is never resolved by silently preferring the Contributor's own version, per AEOS-DOCSTD Section 11.3. |
| `CONTRIB-068` | A Contributor who finds the AEOS repository's actual state diverging from what a governing document describes reports the divergence rather than silently correcting it locally, per AEOS-DOCSTD `DS-P-08` and the read-only verification model AEOS-BOOT Section 8 states. |
| `CONTRIB-069` | This document fixes no specific synchronization mechanism — rebase, merge, or any other — for the version-control system in use; the mechanism is left to the system's own ordinary practice, per [Non-Goal `NG-1`](#18-non-goals). |

## 18. Non-Goals

This document deliberately does not decide the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search this document, or a future one, for a
decision that has not yet been made.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Fixing a specific version-control system, hosting platform, branch-naming scheme, or synchronization mechanism for the AEOS repository's own development. | A future Implementation Guide, or the version-control system's own ordinary practice, once a concrete choice is made. |
| `NG-2` | Establishing CI/CD configuration or a specific commit-message schema. | The AEOS repository's own tooling configuration, consistent with AEOS-BOOT `BOOT-012` and Non-Goal `NG-4`. |
| `NG-3` | Stating language-specific coding standards, style rules, or a specific test framework. | A dedicated Coding Standard, if and when one is authored as a further Developer Guide. |
| `NG-4` | Defining product requirements, architecture, Blueprint arrangement, specified behavior, or terminology. | AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, Specification documents under AEOS-SPECSTD, and AEOS-GLOSSARY, respectively. |
| `NG-5` | Establishing credentials, secrets, a license, or an ownership or governance decision beyond what AEOS-VISION and AEOS-PRD already state. | The project owner, outside this document, consistent with AEOS-BOOT Non-Goal `NG-7`. |
| `NG-6` | Stating the concrete mechanism by which a new Runtime adapter is declared, registered, or distributed. | `RUNTIME_ADAPTER_SPEC.md`, `RUNTIME_REGISTRY.md`, and a future Implementation Guide for the Adapter layer. |
| `NG-7` | Governing how a Developer's own governed Project accepts contributions from others. | The Project and its own conventions; this document addresses the Contributor to AEOS itself, not the Developer. |
| `NG-8` | Positioning this document's own filename convention beyond what AEOS-LAYOUT `LAYOUT-010` already states: that a document's own Suggested-path metadata field governs. | AEOS-LAYOUT. |

## 19. Traceability

Every `CONTRIB-` rule in this document traces to a rule, principle, or invariant in a governing
document, stated inline in [Sections 3](#3-contribution-philosophy) through
[17](#17-repository-synchronization) and indexed in full in
[Appendix A](#appendix-a--contrib-rule-index-non-normative). No `CONTRIB-` rule states an obligation
that does not already exist in a governing document; each states only that obligation's consequence at
the moment a contribution is made.

AEOS-PRD Section 25.6 and AEOS-SPECSTD Section 20.6 already state that issues and pull requests
reference the `PR-` and `SP-` identifiers they advance. This document extends that same obligation to
every identifier family a contribution may touch:

| Contribution artifact | Obligation |
| :--- | :--- |
| Issue | References every identifier it concerns, per `CONTRIB-010`. |
| Pull request | States every identifier it advances, per `CONTRIB-021`. |
| Commit | References the identifier it advances where one exists, per `CONTRIB-017`. |
| This document's own `CONTRIB-` identifiers | Referenced by any future document, issue, or pull request that depends on a specific contribution-process rule, using the reference form AEOS-DOCSTD Section 11.2 states. |

| Trace target | Rules in this document that trace to it |
| :--- | :--- |
| AEOS-VISION (`G1`–`G12`, `6.1`–`6.16`, `V1`–`V10`) | `CONTRIB-001` · `CONTRIB-003` · `CONTRIB-004` · `CONTRIB-006` · `CONTRIB-008` · `CONTRIB-013` · `CONTRIB-016` · `CONTRIB-018` · `CONTRIB-020` · `CONTRIB-030` · `CONTRIB-031` · `CONTRIB-047`(`V6`) · `CONTRIB-049` · `CONTRIB-052` · `CONTRIB-053` · `CONTRIB-054` · `CONTRIB-055` · `CONTRIB-058` · `CONTRIB-059` · `CONTRIB-061` · `CONTRIB-064` · `CONTRIB-065` |
| AEOS-PRD (Section 7, Section 10, `PR-`) | `CONTRIB-002` · `CONTRIB-008` · `CONTRIB-016` · `CONTRIB-024` · `CONTRIB-049` · `CONTRIB-063` · `CONTRIB-064` |
| AEOS-GLOSSARY | `CONTRIB-004`(role addressed) · `CONTRIB-018` · `CONTRIB-021` · `CONTRIB-026` · `CONTRIB-031` · `CONTRIB-051` · `CONTRIB-057` · `CONTRIB-059` · `CONTRIB-062` · `CONTRIB-064` |
| AEOS-DOCSTD | `CONTRIB-009` · `CONTRIB-011` · `CONTRIB-017` · `CONTRIB-022` · `CONTRIB-025` · `CONTRIB-026` · `CONTRIB-027` · `CONTRIB-028` · `CONTRIB-029` · `CONTRIB-032` through `CONTRIB-036` · `CONTRIB-037` through `CONTRIB-041` · `CONTRIB-060` · `CONTRIB-066` through `CONTRIB-068` |
| AEOS-ARCH | `CONTRIB-042` through `CONTRIB-051` |
| AEOS-TECH | `CONTRIB-045` · `CONTRIB-056` |
| AEOS-BOOT | `CONTRIB-005` · `CONTRIB-006` · `CONTRIB-040` · `CONTRIB-068` |
| AEOS-LAYOUT | `CONTRIB-005` · `CONTRIB-007` · `CONTRIB-015`(via `LP5`) · `CONTRIB-019` · `CONTRIB-040` · `CONTRIB-069` |
| AEOS-SPECSTD | `CONTRIB-010`(via traceability precedent) · [Section 10](#10-freeze-policy) |

## 20. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-VISION Section 9 | The guiding principles for contributors this document applies without restating |
| AEOS-VISION Section 6, invariants `V1`–`V10` | The philosophy and invariants named throughout [Sections 3](#3-contribution-philosophy), [14](#14-coding-expectations), [15](#15-ai-assisted-contribution), and [16](#16-human-approval) |
| AEOS-PRD Section 7, Section 10 | The mandatory product principles and the interaction model this document draws the review and approval discipline from |
| AEOS-GLOSSARY, *Contributor*, *Developer*, *Human Approval*, *Approval Gate*, *Review*, *Freeze*, *Repository*, *Runtime Adapter*, *TDD Cycle* entries | The precise definitions this document uses without restating |
| AEOS-DOCSTD Sections 2.4, 7, 11, 12 | The document lifecycle, normative-language rule, and source-of-truth discipline this document's own policy sections apply |
| AEOS-ARCH Sections 3, 8, 11 | The architectural principles, invariants, and extension points [Sections 12](#12-framework-contribution) and [13](#13-runtime-contribution) apply |
| AEOS-SPECSTD Section 19, Section 20.6 | The Specification-domain freeze checklist and the pull-request traceability obligation this document extends |
| AEOS-BOOT Sections 4, 6, 8, and its Non-Goals | The repository structure and configuration boundaries [Section 4](#4-repository-workflow) applies |
| AEOS-LAYOUT Sections 4, 6, 8 | The root-entry and file-placement rules this document's own placement and [Section 4](#4-repository-workflow) apply |
| AEOS-TECH Section 4, `TG-002` | The technology-scope distinction [Section 12](#12-framework-contribution) applies to a Framework contribution's technology choice |

## 21. Document Governance

### 21.1 Status

This document is a **Draft**. It is the first Contribution Guide authored for AEOS, and is intended to
become the Contribution Source of Truth once the owner's review under
[Section 21.4](#214-review-policy) records no Critical or Major finding, at which point it is intended
to be placed and frozen alongside the AEOS 1.0 Developer Guide set.

### 21.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a `CONTRIB-` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Any change to what an existing `CONTRIB-` rule requires, or its retirement | Explicit owner revision request | Major |
| Change to the scope stated in [Section 2](#2-scope-and-applicability) | Explicit owner revision request | Major |
| Any change that would contradict a governing document this document traces to | Not permitted here; the change belongs to that document's own change control | — |

### 21.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD,
AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-BOOT, or AEOS-LAYOUT are recorded as recommendations for
the owning document's governance and are applied only after explicit owner approval there — never
enacted here.

### 21.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms, before
recommending freeze:

- [ ] Every rule in [Sections 3](#3-contribution-philosophy) through
      [17](#17-repository-synchronization) carries a `CONTRIB-<NNN>` identifier and a trace.
- [ ] All nineteen items the task governing this document's authorship required — Purpose, Scope,
      Contribution Philosophy, Repository Workflow, Issue Workflow, Branch Strategy, Commit Policy,
      Pull Request Policy, Review Policy, Freeze Policy, Documentation Contribution, Framework
      Contribution, Runtime Contribution, Coding Expectations, AI-assisted Contribution, Human
      Approval, Repository Synchronization, Non-goals, and Traceability — are present as clearly
      identifiable sections or subsections.
- [ ] No rule restates a product requirement, an architectural decision, a Blueprint arrangement, a
      specified behavior, or a terminology definition already owned elsewhere.
- [ ] No rule states a concrete implementation mechanism, a specific tool, or a specific hosting
      platform beyond what AEOS-PRD `PR-DST-001` already names.
- [ ] All twenty-three entries in this document's Table of Contents are present, in order, and none is
      silently empty.
- [ ] No Critical or Major finding remains open.

### 21.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPECSTD | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-BOOT on repository structure or configuration | AEOS-BOOT governs. The conflict is a defect in this document's [Section 4](#4-repository-workflow), corrected to match AEOS-BOOT rather than the reverse. |
| This document conflicts with AEOS-LAYOUT on placement or root-entry rules | AEOS-LAYOUT governs. The conflict is a defect in this document's placement statement, corrected to match AEOS-LAYOUT rather than the reverse. |
| A future Implementation Guide states a concrete contribution mechanism this document leaves as a Non-Goal | Both stand. This document governs philosophy and policy; the guide governs the mechanism, consistent with AEOS-DOCSTD's derivation chain. |
| A future document names a path or mechanism that conflicts with this document | The apparent need is reported against this document under [Section 21.2](#212-change-control). It is not resolved by a contradictory statement in the other document. |

### 21.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Contribution Guide. States a Purpose and Scope, a Contribution Philosophy referencing AEOS-VISION Section 9 and AEOS-PRD Section 7 without restating either, a general Repository Workflow, an Issue Workflow, a Branch Strategy, a Commit Policy, a Pull Request Policy, a Review Policy, a Freeze Policy summarizing AEOS-DOCSTD Section 12 as a marked non-normative summary, a Documentation Contribution section, a Framework Contribution section applying AEOS-ARCH's extension model, a Runtime Contribution section naming the existing Runtime-layer and Specification-layer documents it defers to, high-level Coding Expectations, an AI-assisted Contribution section applying AEOS-DOCSTD Section 2.4 to every kind of contribution, a Human Approval section applying AEOS-GLOSSARY's *Human Approval* entry, a Repository Synchronization section, eight recorded Non-Goals, a Traceability section extending AEOS-PRD Section 25.6 and AEOS-SPECSTD Section 20.6's obligation to every identifier family this document's contributions may touch, and sixty-nine `CONTRIB-<NNN>` rules in total. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no implementation procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-BOOT, or AEOS-LAYOUT. |

---

## Appendix A — CONTRIB Rule Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 3](#3-contribution-philosophy) through [17](#17-repository-synchronization).

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `CONTRIB-001`–`CONTRIB-004` | Contribution philosophy | 4 | [3](#3-contribution-philosophy) |
| `CONTRIB-005`–`CONTRIB-008` | Repository workflow | 4 | [4](#4-repository-workflow) |
| `CONTRIB-009`–`CONTRIB-012` | Issue workflow | 4 | [5](#5-issue-workflow) |
| `CONTRIB-013`–`CONTRIB-015` | Branch strategy | 3 | [6](#6-branch-strategy) |
| `CONTRIB-016`–`CONTRIB-020` | Commit policy | 5 | [7](#7-commit-policy) |
| `CONTRIB-021`–`CONTRIB-025` | Pull request policy | 5 | [8](#8-pull-request-policy) |
| `CONTRIB-026`–`CONTRIB-031` | Review policy | 6 | [9](#9-review-policy) |
| `CONTRIB-032`–`CONTRIB-036` | Freeze policy | 5 | [10](#10-freeze-policy) |
| `CONTRIB-037`–`CONTRIB-041` | Documentation contribution | 5 | [11](#11-documentation-contribution) |
| `CONTRIB-042`–`CONTRIB-046` | Framework contribution | 5 | [12](#12-framework-contribution) |
| `CONTRIB-047`–`CONTRIB-051` | Runtime contribution | 5 | [13](#13-runtime-contribution) |
| `CONTRIB-052`–`CONTRIB-056` | Coding expectations | 5 | [14](#14-coding-expectations) |
| `CONTRIB-057`–`CONTRIB-061` | AI-assisted contribution | 5 | [15](#15-ai-assisted-contribution) |
| `CONTRIB-062`–`CONTRIB-065` | Human approval | 4 | [16](#16-human-approval) |
| `CONTRIB-066`–`CONTRIB-069` | Repository synchronization | 4 | [17](#17-repository-synchronization) |

## Appendix B — Contribution Checklist (Non-Normative)

A practical restatement of [Sections 4](#4-repository-workflow) through
[17](#17-repository-synchronization), for a Contributor — human or AI — preparing a change. This
checklist carries no authority of its own; where it appears to diverge from those sections, those
sections govern.

- [ ] The contribution's intent is recorded as an issue, or its intent is otherwise clear from its own
      description (Repository Workflow, Issue Workflow).
- [ ] Work proceeds on a line isolated from the AEOS repository's default line (Branch Strategy).
- [ ] Where the contribution touches Implementation, a test precedes it, or the exception is explicit
      and human-acknowledged (Coding Expectations).
- [ ] Commits are small, reference the identifiers they advance where one exists, and contain no
      credential or secret (Commit Policy).
- [ ] The pull request states what changed, why, and which identifiers it advances (Pull Request
      Policy, Traceability).
- [ ] Where the contribution touches a Frozen or Freeze-candidate document, the correct change-control
      path is identified before the pull request is opened (Freeze Policy).
- [ ] Where the contribution is a documentation change, it is written in GitHub-Flavored Markdown, is
      placed per its own Suggested-path field, and restates no definition owned elsewhere
      (Documentation Contribution).
- [ ] Where the contribution is a Framework change, it either respects every Frozen Architectural
      Invariant or is proposed as an explicit architecture revision (Framework Contribution).
- [ ] Where the contribution is a Runtime change, it attaches at the Adapter Layer only and privileges
      no Vendor, Runtime, or Model (Runtime Contribution).
- [ ] Where the contribution was AI-assisted, a human has reviewed it as though it were their own work
      and is prepared to be accountable for it (AI-assisted Contribution, Human Approval).
- [ ] The Contributor's working copy reflects the AEOS repository's authoritative state before the
      pull request is opened (Repository Synchronization).

---

**End of Contribution Guide**

AEOS-CONTRIB · Version 1.0.0 · Contribution Source of Truth (Draft)
