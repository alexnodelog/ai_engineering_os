# AI Engineering Operating System

## AEOS — Best Practices Guide

*The permanent statement of recommended engineering practice for building software with AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | Best Practices Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-PRACTICES |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Engineering Practice Board, AEOS |
| **Audience** | Contributors, Developers, engineering leads, reviewers, maintainers, and AI runtimes engineering software with AEOS |
| **Suggested path** | `docs/developer/BEST_PRACTICES.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states AEOS's **recommended engineering practices**: guidance, expressed at
> recommendation strength, for how a Contributor or Developer works well within AEOS once the
> product's binding layers — Vision, Product Requirements, Glossary, Document Standard, Technology
> Catalog, Architecture, Blueprint, and Specification — already state what must be true. It states
> what tends to produce good outcomes. It does not state what is required, and a decision to depart
> from it is not, by itself, a defect. It covers practice in the areas [Section 3](#3-scope-and-applicability)
> lists: the Human-in-the-Loop discipline, AI collaboration, repository organization, requirement
> management, Architecture and Blueprint usage, Specification authoring, Runtime usage, context
> management, review, validation, refactoring, documentation, and long-term maintainability.
>
> This document is a **Developer Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that layer:
> task-oriented instruction for contributors and users — setup, workflow, conventions in practice,
> troubleshooting — carrying no authority of its own. It is **not** a Product Requirements document,
> **not** a Vision document, **not** an Architecture document, **not** a Blueprint, **not** a
> Specification under AEOS-SPECSTD, **not** a Runtime document, **not** an Implementation Guide in
> the sense AEOS-BOOT, AEOS-ENVSETUP, and AEOS-LAYOUT occupy that layer, and **not** a Coding
> Standard — no Coding Standard has yet been authored for AEOS. It states no product requirement, no
> architectural decision, no Blueprint arrangement, no specified behavior, no Runtime procedure, no
> repository-initialization or environment-preparation procedure, no repository directory or naming
> convention, no technology selection, and no language-specific coding rule; where a statement here
> appears to do any of these, that is a defect in this document and MUST be reported rather than
> acted upon. It redefines no term AEOS-GLOSSARY already defines and restates no rule any other
> document already states normatively.
>
> **On normative language in this document.** This document originates no obligation. Every
> recommendation in [Sections 5](#5-human-in-the-loop-recommendations) through
> [18](#18-long-term-maintainability-recommendations) uses **SHOULD** or **MAY**, never **MUST** or
> **MUST NOT** — a deliberate restriction this document places on itself, narrower than AEOS-DOCSTD
> Section 7.3 requires of a Developer Guide, not an exception to it. Where a passage cites an
> obligation another document already states, it reproduces that document's own keyword faithfully
> and marks it as a citation; the obligation remains owned by the document that states it, consistent
> with AEOS-DOCSTD Section 7.3's statement that a guide instructs and that obligations belong to the
> documents that own them.
>
> AEOS-DOCSTD Section 4.1 positions Developer Guides beneath Implementation Guides, as the final
> layer in the documentation hierarchy. No Developer Guide has previously been authored for AEOS;
> AEOS-BOOT `BOOT-028` reserved `docs/developer/` for the first one, without naming it, and
> AEOS-BOOT's Non-Goal `NG-6` recorded that none yet existed. This document is that first Developer
> Guide, written under AEOS-DOCSTD's general document template and the Section 4.3 purpose statement
> for this layer, in the same spirit AEOS-BOOT, AEOS-ENVSETUP, and AEOS-LAYOUT record for their own
> comparable position at the Implementation Guide layer, in the absence of a dedicated Developer
> Guide Standard. It does not, on that account, establish such a Standard; that remains reserved to
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
2. [Purpose](#2-purpose)
3. [Scope and Applicability](#3-scope-and-applicability)
4. [Best Practice Philosophy](#4-best-practice-philosophy)
5. [Human-in-the-Loop Recommendations](#5-human-in-the-loop-recommendations)
6. [AI Collaboration Recommendations](#6-ai-collaboration-recommendations)
7. [Repository Organization Recommendations](#7-repository-organization-recommendations)
8. [Requirement Management Recommendations](#8-requirement-management-recommendations)
9. [Architecture Usage Recommendations](#9-architecture-usage-recommendations)
10. [Blueprint Usage Recommendations](#10-blueprint-usage-recommendations)
11. [Specification Authoring Recommendations](#11-specification-authoring-recommendations)
12. [Runtime Usage Recommendations](#12-runtime-usage-recommendations)
13. [Context Management Recommendations](#13-context-management-recommendations)
14. [Review Recommendations](#14-review-recommendations)
15. [Validation Recommendations](#15-validation-recommendations)
16. [Refactoring Recommendations](#16-refactoring-recommendations)
17. [Documentation Recommendations](#17-documentation-recommendations)
18. [Long-Term Maintainability Recommendations](#18-long-term-maintainability-recommendations)
19. [Common Anti-Patterns](#19-common-anti-patterns)
20. [Non-Goals](#20-non-goals)
21. [Traceability](#21-traceability)
22. [References](#22-references)
23. [Document Governance](#23-document-governance)
24. [Appendix A — PRAC Recommendation Index (Non-Normative)](#appendix-a--prac-recommendation-index-non-normative)
25. [Appendix B — Best Practice Self-Check (Non-Normative)](#appendix-b--best-practice-self-check-non-normative)

---

## 1. Executive Summary

AEOS-VISION states what AEOS must always remain. AEOS-PRD states what AEOS must do. AEOS-DOCSTD,
AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, and AEOS-SPECSTD each state, within its own layer, how AEOS is
structured, built, and specified. None of them states how a person actually works well inside the
result — which convention to prefer when a rule leaves room, which habit prevents a defect no
requirement forbids by name, which practice keeps a project maintainable long after the contributor
who wrote it has moved on. That question has answers, and the answers are not all recoverable by
inference from the layers above. This document states them, once, as recommendations a Contributor
or Developer can follow without being told they must.

It exists because the gap between what is required and what is wise is a gap every project
experiences, and leaving it to habit means the answer depends on who is in the room that day.
AEOS-VISION Section 9 already states twelve guiding principles for contributors (`G1`–`G12`), and
AEOS-PRD Section 7 already states thirteen numbered product philosophy principles, that no
engineering decision may contradict. This document does not restate either. It operationalizes
them: it turns durable, binding conviction into concrete, situational guidance a Contributor can
actually apply on an ordinary working day, without layering a new obligation on top of the ones
AEOS-PRD already states.

Six properties bind this document, so that recommending remains its only function:

| Property | What it requires of this document |
| :--- | :--- |
| **Recommendation-only** | Every statement in [Sections 5](#5-human-in-the-loop-recommendations) through [18](#18-long-term-maintainability-recommendations) uses **SHOULD** or **MAY**; none originates a **MUST**. |
| **Runtime-neutral** | No recommendation depends on, or is stated in terms of, one external AI Runtime. |
| **Vendor-neutral** | No recommendation names, prefers, or is written around one Vendor's offering. |
| **Model-neutral** | No recommendation depends on one Model family's behavior, documented or undocumented. |
| **Platform-neutral** | No recommendation depends on one Platform among Windows, macOS, and Linux. |
| **Universal** | Every recommendation applies uniformly to a Contributor working with any Runtime, Adapter, or Model AEOS-TECH recognizes; none is written to fit only some. |

This document is, deliberately, the most easily disagreed-with document in the repository: it is the
one place where a reader is invited to weigh a stated reason against their own situation and choose
differently, and doing so is not a defect in the reader or in this document.

---

## 2. Purpose

The purpose of this document is to give a Contributor or Developer a single place to consult for how
to work well within AEOS, once what is required is already settled elsewhere. It exists for three
readers in particular:

- a Contributor deciding, mid-task, between two ways of satisfying a requirement that is silent on
  which is better;
- an engineering lead who wants a team's practice to be explicit and uniform without writing a new
  Rule for every habit worth sharing;
- an AI runtime that has read the binding layers and now needs to know what a competent human
  collaborator would additionally do, in situations those layers do not settle by name.

This document is consulted, not executed. Unlike AEOS-BOOT or AEOS-ENVSETUP, it states no procedure
with an entry condition and an exit condition, and [Section 8](#8-requirement-management-recommendations)
through [Section 18](#18-long-term-maintainability-recommendations) are not read in sequence once and
then discarded; they are returned to throughout a project's life, at the point a situation they
address actually arises.

## 3. Scope and Applicability

### 3.1 What This Document Governs

This document governs recommended practice, and nothing that carries authority:

- practice for applying the Human-in-the-Loop discipline AEOS-PRD already requires, in situations
  that discipline does not settle by name;
- practice for collaborating with an AI Runtime under AEOS-PRD's Runtime, Vendor, and Model
  independence principles;
- practice for organizing work within a repository whose structure AEOS-LAYOUT already states;
- practice for managing requirements, once AEOS-PRD's own requirement discipline is in force;
- practice for using the Architecture and Blueprint layers as a Contributor rather than as their
  author;
- practice for authoring within AEOS-SPECSTD's Specification discipline;
- practice for working with a Runtime, Adapter, and Model, and for managing Context, once AEOS-PRD's
  Context Minimization principle is in force;
- practice for review, validation, and refactoring, once AEOS-PRD's own review and TDD discipline is
  in force;
- practice for documentation and for long-term maintainability, once AEOS-DOCSTD's own documentation
  discipline is in force;
- naming, without exhaustively cataloguing, recognizable anti-patterns worth avoiding.

This list is complete.

### 3.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, and frozen | AEOS-DOCSTD |
| Which technologies AEOS recognizes, and at what tier | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH |
| How that structure is arranged to be built | AEOS-BLUEPRINT |
| How each defined behavior must work, precisely and testably | Specification documents, under AEOS-SPECSTD |
| How AEOS executes, in what environment, with what lifecycle | Runtime documents |
| How a new AEOS repository is initialized | AEOS-BOOT |
| What a host machine must provide before that | AEOS-ENVSETUP |
| Where a file, document, or Repository Asset is placed | AEOS-LAYOUT |
| Language-specific coding rules | A future Coding Standard; none exists for AEOS 1.0 |
| What code realizes any capability named above | The codebase and its tests |

A statement in this document that redefines a term, imposes an obligation, decides a structure, or
selects a technology is a **defect in this document**. It MUST be reported rather than acted upon.

### 3.3 Applicability

This document applies to a Contributor or Developer working on any AEOS-governed project, and to an
AI runtime doing the same on a human's behalf, consistent with AEOS-DOCSTD Section 2.4. It applies
identically regardless of the Runtime, Adapter, Model, Platform, or Distribution Method in use,
consistent with AEOS-PRD Section 7.7 through 7.11.

Unlike AEOS-BOOT, AEOS-ENVSETUP, and AEOS-LAYOUT, this document states no verification procedure
that gates a release or a freeze: a recommendation is not a testable pass/fail condition in the sense
AEOS-SPECSTD `SS-P-04` requires of a Specification, and treating this document as such a gate would
convert recommendation into obligation without the change control that requires.
[Appendix B](#appendix-b--best-practice-self-check-non-normative) offers a reflective self-check for
a Contributor who wants one; it carries no authority and blocks nothing.

---

## 4. Best Practice Philosophy

### 4.1 What Makes a Recommendation

A recommendation earns its place in this document by being true across enough situations to be worth
stating once — not by being enforced. Where a statement would only be safe to make as an obligation,
it is not a best practice; it is a requirement that has not yet been written as one, and it belongs
at a different layer, proposed there under that layer's own change control, not smuggled into this
one disguised as **SHOULD**.

Every recommendation in this document was selected against one test: **would declining it, for a
stated reason, still leave a competent reviewer able to say the work is sound?** If the answer is
no, the statement is not a recommendation and does not belong here.

### 4.2 Where Each Kind of Statement Lives

The table below positions six labels relative to one another so that a reader can tell, at a glance,
what kind of statement they are looking at and where its authority — if it has any — comes from. It
introduces no new AEOS terminology: *Requirement* and *Implementation* are used exactly as AEOS-PRD
and AEOS-GLOSSARY already use them; *Architecture Rule* and *Specified Behavior* are descriptive
labels for normative statements AEOS-ARCH, AEOS-BLUEPRINT, and Specification documents already make;
*Coding Standard* names a document layer AEOS has not yet authored; *Best Practice* and
*Recommendation* name what this document itself contains, and nothing this document states extends
past its own boundary.

| Label | What it is | Where it is stated | What happens if it is not followed |
| :--- | :--- | :--- | :--- |
| **Requirement** | A numbered, `PR-`identified obligation the product itself must satisfy. | AEOS-PRD Section 18, under formal change control. | A defect in the product. |
| **Architecture Rule** | A structural decision or invariant the Architecture or Blueprint layer states. | AEOS-ARCH, AEOS-BLUEPRINT. | A defect in the structure. |
| **Specified Behavior** | A precise, testable statement of required behavior, traced to a Requirement. | Specification documents, under AEOS-SPECSTD. | A failing test, and a defect in the Implementation. |
| **Coding Standard** | Language-specific rules for how code is written. Not yet authored for AEOS 1.0. | Reserved to a future document, under AEOS-DOCSTD `H5`. | Not yet applicable. |
| **Implementation** | The code and tests that realize the layers above them. | The codebase and its tests. | A defect against whichever layer above it the Implementation fails to realize. |
| **Best Practice / Recommendation** | A `SHOULD`- or `MAY`-level statement of what tends to produce a good outcome. | This document. | Nothing, by itself. A stated reason for declining is good practice in its own right, not a concession. |

### 4.3 How a Recommendation Is Written

Every recommendation in [Sections 5](#5-human-in-the-loop-recommendations) through
[18](#18-long-term-maintainability-recommendations) states three things: the practice itself, in one
sentence a Contributor can act on; the reason it tends to help, briefly; and, where relevant, the
governing document a reader should consult for the binding answer if they want one instead of a
recommendation. A recommendation that cannot state a reason is not yet ready to be one.

### 4.4 Precedence Over This Document

Nothing in this document overrides a Requirement, an Architecture Rule, a Specified Behavior, or a
documentation rule stated elsewhere. Where a recommendation here appears to conflict with a governing
document, the governing document is correct and this document is defective at that point, per
[Section 23.5](#235-precedence).

---

## 5. Human-in-the-Loop Recommendations

AEOS-PRD Section 7.1 requires that full automation is never the default and is granted explicitly,
in scope, and revocably; Section 10 states the Inspect–Explain–Propose–Confirm–Execute–Report loop
every consequential action follows; Section 10.2 states what makes an Automation Grant valid. These
recommendations address situations that discipline leaves to judgment.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-001` | A Contributor SHOULD design a proposal so a human can approve or decline it without first reconstructing context the proposal itself omitted. | AEOS-PRD `7.2` is satisfied only if the explanation is actually sufficient, not merely present. | AEOS-PRD §7.1, §7.2, `V3` |
| `PRAC-002` | A Contributor SHOULD request an Automation Grant no broader, in Action Class, project, and duration, than the work in front of them actually needs, and MAY decline a broader grant even where one is offered. | A grant wider than the task invites automated action nobody actually asked for. | AEOS-PRD §10.2, `V2` |
| `PRAC-003` | A Contributor SHOULD treat a Destructive action as requiring its own explicit confirmation even while a broader grant is active. | AEOS-PRD §10.2 bounds grants against destructive actions; treating the boundary as a formality defeats it. | AEOS-PRD §10.1, §10.2, `V7` |
| `PRAC-004` | Where the Principle Conflict Resolution order (AEOS-PRD §7.14) does not settle a case, a Contributor SHOULD escalate to the human rather than resolve it silently. | Silent resolution is a decision nobody made, made anyway. | AEOS-PRD §7.14; AEOS-VISION `G12` |

---

## 6. AI Collaboration Recommendations

AEOS-PRD Section 7.7 through 7.9 require Vendor, Runtime, and Model independence; Section 10.1
classifies actions by effect; Section 12.1 (`C4`) states that AEOS selects, adapts, and coordinates
external Runtimes rather than becoming one. These recommendations address how a Contributor works
with those Runtimes day to day.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-005` | A Contributor SHOULD phrase a task's success criteria in terms a different Runtime could also satisfy, and SHOULD NOT phrase it around one Runtime's known behavior, undocumented or otherwise. | A task specification that only one Runtime can satisfy has quietly reintroduced Runtime dependence. | AEOS-PRD §7.8, §7.9 |
| `PRAC-006` | A Contributor SHOULD state the Action Class of a proposed step explicitly when it is not obvious, rather than let a Runtime infer it. | Misclassifying a Local change as an Observation, or a Destructive action as a Local change, understates the approval it needs. | AEOS-PRD §10.1 |
| `PRAC-007` | A Contributor MAY decline a Runtime's proposal on the grounds that its reasoning was not stated, even where the proposed action itself looks correct. | Explain Before Execute governs the proposal, not only the outcome. | AEOS-PRD §7.2 |
| `PRAC-008` | A Contributor SHOULD report a capability gap between what a workflow step needs and what the selected Runtime, Adapter, or Model offers, rather than let the step proceed degraded and unremarked. | AEOS-PRD names capability divergence as a recognized risk; surfacing it is cheaper than discovering it downstream. | AEOS-PRD §23 (Assumptions, Dependencies, and Risks) |
| `PRAC-009` | Where AEOS-TECH records more than one Officially Supported option in a category relevant to the work, a Contributor MAY prefer the Preferred entry without further justification, and SHOULD state a reason when choosing a Conditionally Supported one instead. | AEOS-TECH `TG-020` ties Preferred to "requires no justification"; the converse is a matching courtesy to the next reader. | AEOS-TECH §6 |

---

## 7. Repository Organization Recommendations

AEOS-LAYOUT is the source of truth for where a file, document, or Repository Asset is placed; this
document does not restate its rules. These recommendations address organizational judgment calls
AEOS-LAYOUT leaves to the Contributor because they depend on the work, not on the repository's
fixed structure.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-010` | A Contributor SHOULD place new content at the path AEOS-LAYOUT's own rules resolve to on the first attempt, rather than place it conveniently and relocate it later. | A relocation is a second change a reviewer must separately verify; AEOS-DOCSTD `DS-P-02` prefers a diff that shows only the substantive change. | AEOS-LAYOUT; AEOS-DOCSTD `DS-P-02` |
| `PRAC-011` | A Contributor SHOULD keep a single Repository Asset's declaration and its supporting material together under the path AEOS-LAYOUT assigns its kind, rather than split related content across directories for convenience. | AEOS-VISION `V5` holds only if what a reader needs is where the structure says it is. | AEOS-VISION `V5`; AEOS-LAYOUT |
| `PRAC-012` | Where a Contributor is uncertain which AEOS-LAYOUT placement rule applies, they SHOULD ask rather than infer one by analogy to an unrelated asset kind. | An inferred placement that turns out wrong compounds: later assets copy the mistake. | AEOS-VISION `G12`; AEOS-LAYOUT |
| `PRAC-013` | A Contributor SHOULD raise a gap in AEOS-LAYOUT — a kind of content it does not yet address — as a finding against AEOS-LAYOUT, and SHOULD NOT invent an undocumented convention to fill it silently. | An undocumented convention is not a convention to the next reader, per AEOS-DOCSTD `A6`. | AEOS-DOCSTD `A6`, `DS-P-08` |

---

## 8. Requirement Management Recommendations

AEOS-PRD Section 18 is the source of truth for product requirements; every `PR-` identifier is
permanent per AEOS-GLOSSARY `I1`. These recommendations address how a Contributor works with
requirements without altering their ownership.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-014` | A Contributor SHOULD state which `PR-` identifier a piece of work satisfies before starting it, not only when reporting it complete. | A trace fixed after the fact is easy to fit to whatever was actually built, which defeats the purpose of tracing. | AEOS-DOCSTD `H4`; AEOS-GLOSSARY, *Source of Truth* |
| `PRAC-015` | Where a Contributor believes a requirement is ambiguous, they SHOULD surface the ambiguity as a question to the human rather than resolve it by an assumption and proceed. | AEOS-PRD Section 9's Requirement analysis stage names exactly this: "surfaces ambiguity as questions rather than assumptions." | AEOS-PRD §9 |
| `PRAC-016` | A Contributor SHOULD NOT treat the absence of a requirement on a subject as permission to do anything; where the product is silent, AEOS-PRD's Section 7 philosophy and AEOS-VISION's invariants still apply. | Silence is not a grant. | AEOS-VISION `V2`, `V7` |
| `PRAC-017` | A Contributor who finds two requirements pulling in different directions SHOULD apply AEOS-PRD §7.14's conflict-resolution order rather than choose the one that is easier to satisfy. | The order exists precisely so the choice is not made on convenience. | AEOS-PRD §7.14 |

---

## 9. Architecture Usage Recommendations

AEOS-ARCH is the source of truth for AEOS's structure; this document does not restate its layer
model, dependency rules, or invariants. These recommendations address how a Contributor works within
that structure rather than beside it.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-018` | A Contributor SHOULD locate which Architecture Layer owns a piece of knowledge before writing code that uses it, rather than let the knowledge appear wherever it was first needed. | AEOS-ARCH's "one place for runtime knowledge" and "one place for platform knowledge" decisions hold only if every Contributor respects them, not only their original author. | AEOS-ARCH §1, §4 |
| `PRAC-019` | Where a task appears to require a layer to depend on a layer AEOS-ARCH's dependency model forbids, a Contributor SHOULD treat that as a signal the task is misunderstood, and SHOULD report it, rather than introduce the dependency to make the task tractable. | AEOS-ARCH `H1`-equivalent architectural invariants are frozen; a workaround that violates one is a defect regardless of how locally reasonable it looked. | AEOS-ARCH §5, §7, §8 |
| `PRAC-020` | A Contributor extending AEOS SHOULD locate the seam AEOS-ARCH's extension model provides before adding a new responsibility, rather than add it to whichever layer was already open in an editor. | AEOS-VISION `V10`: AEOS is extended, not modified. | AEOS-ARCH §11; AEOS-VISION `V10`, `G7` |

---

## 10. Blueprint Usage Recommendations

AEOS-BLUEPRINT is the source of truth for the internal decomposition of each Architecture Layer.
These recommendations address how a Contributor uses that decomposition when building against it.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-021` | A Contributor SHOULD identify which subsystem, per AEOS-BLUEPRINT's ownership matrix, owns a piece of work before starting it, rather than after a review finds it in the wrong place. | Attribution only works if ownership was checked in advance, not reconstructed after the fact. | AEOS-BLUEPRINT §15 |
| `PRAC-022` | Where a Contributor's work would require two peer subsystems to collaborate directly, they SHOULD treat that as a signal to re-examine the design against AEOS-BLUEPRINT's arrangement rules before proceeding. | Peer-to-peer collaboration is exactly what a decomposition exists to prevent; routing around it defeats the Blueprint silently. | AEOS-BLUEPRINT §5, §6 |
| `PRAC-023` | A Contributor extending a subsystem SHOULD use the seam AEOS-BLUEPRINT's extension model names for it, and SHOULD report a gap where no seam fits, rather than reopen the subsystem's own boundary. | A seam that is bypassed once is a precedent for bypassing it again. | AEOS-BLUEPRINT §16 |

---

## 11. Specification Authoring Recommendations

AEOS-SPECSTD states ten Specification Principles (`SS-P-01` through `SS-P-10`); this document does
not restate them. These recommendations address the judgment an author exercises while writing
within that discipline.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-024` | An author SHOULD attempt to derive a concrete test from a rule as they write it, not after the Specification is declared complete. | `SS-P-10` requires a test to be derivable without further interpretation; the earliest way to find out is to try. | AEOS-SPECSTD `SS-P-10` |
| `PRAC-025` | Where a rule under authorship reads more like a description of behavior than a testable obligation, the author SHOULD rewrite it before moving on, rather than flag it for a later pass. | `SS-P-04` distinguishes a testable statement from a description; the distinction is easiest to preserve at the moment of writing. | AEOS-SPECSTD `SS-P-04` |
| `PRAC-026` | An author SHOULD write a rule's trace to its `PR-` identifier in the same pass as the rule itself, not as a separate bookkeeping step afterward. | A trace added after the fact is easy to assign loosely; `SS-P-03` requires every rule to trace, and precision degrades with delay. | AEOS-SPECSTD `SS-P-03` |
| `PRAC-027` | Before proposing a new behavior-domain area code, an author SHOULD confirm no existing Specification already owns the behavior, per `SS-P-02`'s one-domain-one-document rule. | A second document quietly co-owning a domain is a source-of-truth conflict AEOS-DOCSTD `DS-P-08` prohibits. | AEOS-SPECSTD `SS-P-02`; AEOS-DOCSTD `DS-P-08` |
| `PRAC-028` | An author extending a frozen Specification SHOULD add rather than reinterpret an existing rule, consistent with `SS-P-09`, and SHOULD raise a needed reinterpretation as a proposed revision rather than an addition that quietly changes existing meaning. | Extension that is actually revision in disguise escapes the review a revision requires. | AEOS-SPECSTD `SS-P-09` |

---

## 12. Runtime Usage Recommendations

AEOS-PRD Section 7.8 and 7.9 require Runtime and Model independence; AEOS-TECH Section 6 states the
support tiers a Runtime, Adapter, or Model provider is recognized under. These recommendations
address how a Contributor selects and works with them.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-029` | A Contributor configuring a project SHOULD select a Runtime, Adapter, and Model combination without assuming any one of the three is fixed by the others. | AEOS-PRD §7.8: switching Runtime is a configuration change, not a migration project; treating a combination as fixed reintroduces the coupling that principle removes. | AEOS-PRD §7.8, §7.9 |
| `PRAC-030` | Where a Contributor observes behavior that appears specific to one Model's quirks, they SHOULD record the observation rather than encode a workaround for it into shared, Model-neutral material. | AEOS-PRD §7.9: no capability may depend on undocumented behavior of a specific Model version. | AEOS-PRD §7.9 |
| `PRAC-031` | A Contributor SHOULD prefer a Preferred or Officially Supported Model provider for new work, per AEOS-TECH's tiers, and MAY choose a Conditionally Supported one where the stated condition is acceptable for the project. | AEOS-TECH ties tier to verification and release-blocking treatment of regressions; a Contributor benefits from that coverage by staying within it where the choice is otherwise open. | AEOS-TECH §6, §8 |
| `PRAC-032` | A Contributor SHOULD treat a locally or privately hosted Model as a first-class option when privacy, cost, latency, or offline operation matter to the project, rather than as a fallback considered only once a hosted option is unavailable. | AEOS-VISION §10.6 names local and private AI integration a first-class direction, not a contingency. | AEOS-VISION §10.6 |

---

## 13. Context Management Recommendations

AEOS-PRD Section 7.6 requires Context Minimization: the smallest Context sufficient for the task,
with each inclusion explainable. These recommendations address how a Contributor applies that
principle in practice.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-033` | Before assembling Context for a step, a Contributor SHOULD be able to state, for each item, the specific reason it is included, and SHOULD remove an item they cannot justify. | AEOS-PRD `PR-PMT-003` requires the smallest sufficient Context; an unjustifiable inclusion is evidence the Context is not yet minimal. | AEOS-PRD `PR-PMT-003`, §7.6 |
| `PRAC-034` | A Contributor SHOULD scope Context to the active step rather than accumulate it across steps by default. | AEOS-PRD §7.6: Context is selected deliberately and scoped to the active step, not accumulated by default. | AEOS-PRD §7.6 |
| `PRAC-035` | Where a Contributor is uncertain whether an item belongs in Context, they SHOULD exclude it and add it back only if its absence is later shown to matter, rather than include it preemptively. | Smaller Context that later proves insufficient is a cheap, visible correction; excess Context that turns out unnecessary is a cost nobody notices to remove. | AEOS-PRD §7.6 |
| `PRAC-036` | A Contributor SHOULD be able to explain a Context selection to the user on request, in plain terms, without reconstructing the reasoning after the fact. | AEOS-PRD §7.6's stated implication: every Context selection must be explainable to the user on request. | AEOS-PRD §7.6 |

---

## 14. Review Recommendations

AEOS-GLOSSARY defines *Review* as examination against requirements, rules, and tests, producing
severity-classified findings; AEOS-DOCSTD Section 12.3 states the Critical / Major / Minor / Nitpick
classification this document also uses for itself. These recommendations address how a Contributor
conducts a review well.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-037` | A reviewer SHOULD classify every finding by severity before discussing any of them, rather than negotiate severity as part of the conversation about what to fix. | Severity decided after the fact tends to drift toward whatever resolution is already agreed, which defeats the point of classifying at all. | AEOS-DOCSTD §12.3, §12.4 |
| `PRAC-038` | A reviewer SHOULD identify what is inconsistent with a stated requirement, rule, or test, and SHOULD NOT redesign the work under review in the course of doing so. | AEOS-DOCSTD `12.4` states this exact boundary for documentation review, and the same boundary serves code review for the same reason: redesign substitutes the reviewer's judgment for the author's without the accountability either would carry alone. | AEOS-DOCSTD §12.4 |
| `PRAC-039` | A reviewer SHOULD cite the specific requirement, rule, or test a finding violates, rather than describe a finding only in terms of feeling wrong. | An uncited finding cannot be independently checked, and independent checkability is what makes a finding a finding rather than an opinion. | AEOS-DOCSTD §12.4 |
| `PRAC-040` | A Contributor receiving a finding SHOULD resolve it, decline it with a recorded reason, or escalate it — and SHOULD NOT leave it acknowledged but unaddressed. | An acknowledged-but-unaddressed finding is indistinguishable, to the next reader, from one nobody noticed. | AEOS-DOCSTD §12.2 (Stage 3) |
| `PRAC-041` | A reviewer who believes the work's premise is wrong SHOULD record that as a single finding and escalate, rather than raise the same objection repeatedly against individual details. | AEOS-DOCSTD `12.4` states this practice for documentation review; the same discipline keeps a code review from re-litigating one disagreement across dozens of comments. | AEOS-DOCSTD §12.4 |

---

## 15. Validation Recommendations

AEOS-PRD Section 7.4 requires TDD-first development: the failing test precedes the implementation.
These recommendations address validation practice around that discipline.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-042` | A Contributor SHOULD confirm a failing test fails for the reason it is expected to, not merely that it fails, before writing the implementation. | AEOS-PRD's TDD Cycle names this step explicitly: "confirm it fails for the right reason." A test that fails for the wrong reason validates nothing once made to pass. | AEOS-PRD §7.4; AEOS-GLOSSARY, *TDD Cycle* |
| `PRAC-043` | A Contributor SHOULD treat a request to skip the TDD Cycle as an exception requiring explicit human acknowledgment, and SHOULD surface it as such rather than comply silently. | AEOS-PRD §7.4 states this in exactly these terms for AEOS itself; the same standard is reasonable to hold work built with AEOS to. | AEOS-PRD §7.4 |
| `PRAC-044` | A Contributor SHOULD run the project's own test tooling and report the actual result, rather than infer a pass from the implementation looking correct. | AEOS-PRD Section 9's Testing stage: runs the project's own test tooling, reports results, blocks progress on failure. | AEOS-PRD §9 |

---

## 16. Refactoring Recommendations

AEOS-PRD's TDD Cycle ends in "refactor under a green suite." These recommendations address
refactoring practice around that step.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-045` | A Contributor SHOULD refactor only under a currently green test suite, and SHOULD stop and restore green before continuing if a refactor turns it red for a reason unrelated to the refactor's own intent. | A refactor that changes behavior while claiming only to change structure has stopped being a refactor. | AEOS-PRD §7.4, §9 (Refactoring stage) |
| `PRAC-046` | A Contributor SHOULD state the refactoring's intended scope before starting, consistent with AEOS-PRD Section 9's human decision point for that stage, and SHOULD NOT let the scope grow silently once approved. | Scope expansion mid-refactor is exactly what AEOS-PRD §10's Execute phase already prohibits for any consequential action: "no more" than what was approved. | AEOS-PRD §9, §10 |
| `PRAC-047` | A Contributor SHOULD prefer the smallest refactor that removes the specific structural problem motivating it, and SHOULD raise a larger restructuring as its own proposal rather than fold it into an unrelated task. | AEOS-VISION `G2`: complexity that serves an anticipated requirement is speculation; a refactor scoped to more than its trigger carries the same risk. | AEOS-VISION `G2` |

---

## 17. Documentation Recommendations

AEOS-DOCSTD is the source of truth for how AEOS documentation is written; this document does not
restate its principles, style rules, or format standard. These recommendations address documentation
judgment AEOS-DOCSTD leaves to the author because it depends on the content, not the form.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-048` | Before writing a definition, an author SHOULD check whether AEOS-GLOSSARY already defines the term, and reference it rather than restate it, even where restating would read more smoothly in context. | AEOS-DOCSTD `DS-P-07`: definitions are never duplicated, and a duplicated definition does not stay identical. | AEOS-DOCSTD `DS-P-07`, `T3` |
| `PRAC-049` | An author SHOULD write the reason for a rule alongside the rule itself where the rule is likely to be argued with, rather than assume the reason is self-evident. | AEOS-DOCSTD `R8`: a rule whose reason nobody recorded is a rule someone will eventually remove. | AEOS-DOCSTD `R8` |
| `PRAC-050` | An author SHOULD generate or update documentation from the repository's actual current state at the moment of writing, and SHOULD NOT describe a state the author expects will soon be true. | AEOS-PRD `C6`: documentation generation is defined as producing and maintaining documentation from repository truth, not from intent. | AEOS-PRD `C6`; AEOS-DOCSTD `DS-P-12` |

---

## 18. Long-Term Maintainability Recommendations

AEOS-VISION `G5` and `G11` already bind maintainability decisions to the project's lifetime rather
than its deadline. These recommendations address specific situations that principle recurs in.

| ID | Recommendation | Why | Traces to |
| :--- | :--- | :--- | :--- |
| `PRAC-051` | A Contributor SHOULD justify a design decision against the project's expected lifetime, and SHOULD treat "it can be cleaned up later" as a prediction to be stated and dated, not a resolution. | AEOS-VISION `G5`: "we can clean it up later" is usually wrong. | AEOS-VISION `G5` |
| `PRAC-052` | A Contributor SHOULD leave a piece of the repository more understandable than they found it whenever they touch it, even where the task at hand did not require the improvement. | AEOS-VISION `G11`: understandability is a contribution in its own right, and its absence is a defect even when every test passes. | AEOS-VISION `G11` |
| `PRAC-053` | Before reimplementing something a mature, already-integrated system provides, a Contributor SHOULD state the reason the existing system is insufficient, and SHOULD prefer orchestration or integration where no such reason exists. | AEOS-VISION `G7`: do not rebuild what mature systems already do well. | AEOS-VISION `G7` |
| `PRAC-054` | A Contributor preserving backward compatibility for an existing project SHOULD state the benefit of a breaking change against the cost to people not present to object, together with a migration path, before making it. | AEOS-VISION `G4`. | AEOS-VISION `G4` |
| `PRAC-055` | A Contributor with an idea that would improve a frozen document's concepts SHOULD record it as a recommendation for the owning document's governance, and SHOULD NOT apply it silently, however small it seems. | AEOS-VISION `G8`; AEOS-DOCSTD `E7`. | AEOS-VISION `G8`; AEOS-DOCSTD `E7` |

---

## 19. Common Anti-Patterns

Each pattern below is a recognizable way of failing one or more recommendations above. Naming a
pattern is not a new obligation; it is a convenience for recognizing a mistake before it is made
rather than after, consistent with this document's status as recommendation rather than rule.

| ID | Anti-pattern | Why it fails | Related |
| :--- | :--- | :--- | :--- |
| `AP-001` | **Bulk context transfer.** Sending an entire file, module, or conversation history "to be safe," rather than the items a step actually needs. | AEOS-PRD §7.6 names this a failure of design, not a feature. | `PRAC-033`, `PRAC-034` |
| `AP-002` | **Silent conflict resolution.** Encountering two documents, or a document and observed behavior, in disagreement, and quietly acting on whichever is convenient rather than reporting the conflict. | AEOS-DOCSTD `DS-P-08`: creates a third version of the truth. | `PRAC-013`, `PRAC-017` |
| `AP-003` | **Convenience restatement.** Repeating a definition, requirement, or rule owned elsewhere in a new document "so the reader does not have to look it up." | AEOS-DOCSTD Section 5.2 names this the first common ownership violation; the copy does not stay identical to its source. | `PRAC-048` |
| `AP-004` | **Scope creep under an active approval.** Letting an approved step's actual effect grow beyond what was approved, on the reasoning that the human would probably have said yes. | AEOS-PRD §10's Execute phase: perform exactly what was approved, no more. | `PRAC-002`, `PRAC-003`, `PRAC-046` |
| `AP-005` | **Skipping the failing test "just this once."** Writing the implementation first because the test feels obvious, and adding a passing test afterward. | AEOS-PRD §7.4: a test written to already-existing implementation does not verify anything the implementation did not already assume. | `PRAC-042`, `PRAC-043` |
| `AP-006` | **Runtime-specific phrasing.** Writing a task, prompt, or Rule around one Runtime's known quirks so that it reads oddly, or fails, on another. | AEOS-PRD §7.8, §7.9: reintroduces Runtime or Model dependence the product's independence principles remove. | `PRAC-005`, `PRAC-030` |
| `AP-007` | **Treating a Draft as authoritative.** Building on, or citing, a document whose Status is `Draft` or `In Review` as though it were `Frozen`. | AEOS-DOCSTD `12.1`: a Draft is not authoritative and MUST NOT be referenced as a source of truth. | `PRAC-014`, `PRAC-015` |
| `AP-008` | **Undocumented local convention.** Adopting a placement, naming, or process habit that works for one Contributor and is never written down anywhere another Contributor, or an AI runtime, could find it. | AEOS-DOCSTD `A6`: an unstated convention is not a convention to a runtime; it is a guess with a plausible tone. | `PRAC-012`, `PRAC-013` |
| `AP-009` | **Forking instead of extending.** Copying a subsystem to modify it in place, rather than using the seam its owning layer's extension model provides. | AEOS-VISION `V10`, `G7`. | `PRAC-020`, `PRAC-023` |
| `AP-010` | **Deadline-justified maintainability debt.** Making a structural shortcut and recording no reason, no owner, and no date by which it will be revisited. | AEOS-VISION `G5`: decisions are justified against the project's lifetime, not the task's deadline. | `PRAC-051` |

---

## 20. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document, in
the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Stating language-specific coding rules. | A future Coding Standard; none exists for AEOS 1.0. |
| `NG-2` | Preparing a host machine for AEOS work. | AEOS-ENVSETUP. |
| `NG-3` | Initializing a new AEOS repository. | AEOS-BOOT. |
| `NG-4` | Stating repository directory structure or naming conventions. | AEOS-LAYOUT. |
| `NG-5` | Selecting or ranking a technology, Vendor, Runtime, Adapter, or Model. | AEOS-TECH, and the project's own configuration. |
| `NG-6` | Deciding a structural or Blueprint arrangement. | AEOS-ARCH, AEOS-BLUEPRINT. |
| `NG-7` | Stating a specified, testable behavior. | Specification documents, under AEOS-SPECSTD. |
| `NG-8` | Granting, configuring, or recording an Automation Grant. | The project owner, under AEOS-PRD §10.2. |
| `NG-9` | Serving as a verification gate for a release, a freeze, or a review. | Each project's own review process; AEOS-DOCSTD Section 12 for AEOS's own documents. |
| `NG-10` | Defining the Developer Guide layer's Standard, identifier conventions, or structure for documents other than this one. | Reserved to the owner under AEOS-DOCSTD `H5`. |

---

## 21. Traceability

Every `PRAC-` recommendation and `AP-` anti-pattern above states its own trace inline. The table
below summarizes trace density by governing document, consistent with the practice AEOS-BOOT Section
11 records for itself.

| Governing document or family | Recommendations that trace to it |
| :--- | :--- |
| AEOS-VISION (`G1`–`G12`, `V1`–`V10`) | `PRAC-001` · `PRAC-002` · `PRAC-003` · `PRAC-004` · `PRAC-011` · `PRAC-013` · `PRAC-016` · `PRAC-020` · `PRAC-032` · `PRAC-047` · `PRAC-051` · `PRAC-052` · `PRAC-053` · `PRAC-054` · `PRAC-055` |
| AEOS-PRD (philosophy sections, capabilities, `PR-` identifiers) | `PRAC-001`–`PRAC-009` · `PRAC-014`–`PRAC-017` · `PRAC-029`–`PRAC-036` · `PRAC-042`–`PRAC-044` · `PRAC-045`–`PRAC-046` · `PRAC-050` |
| AEOS-GLOSSARY (terminology, unaltered) | `PRAC-014` · `PRAC-042`; terminology throughout used without redefinition |
| AEOS-DOCSTD (principles, rules, review policy) | `PRAC-010` · `PRAC-012` · `PRAC-013` · `PRAC-037`–`PRAC-041` · `PRAC-048`–`PRAC-050` · `PRAC-055` |
| AEOS-TECH (support tiers) | `PRAC-009` · `PRAC-031` |
| AEOS-ARCH | `PRAC-018`–`PRAC-020` |
| AEOS-BLUEPRINT | `PRAC-021`–`PRAC-023` |
| AEOS-SPECSTD (`SS-P-01`–`SS-P-10`) | `PRAC-024`–`PRAC-028` |
| AEOS-LAYOUT | `PRAC-010`–`PRAC-013` |

This document, as a whole, traces to AEOS-DOCSTD `H4`'s requirement that every derivative document
trace to the layer above it — satisfied here by tracing each recommendation to the guiding
principle, philosophy statement, invariant, or governing document it operationalizes — and to
AEOS-DOCSTD `H6`'s requirement that a document belong to a layer, satisfied by this document's
position as the first Developer Guide, stated in the authority statement at the head of this
document. No `PRAC-` or `AP-` identifier states an obligation of its own; the trace records what
each recommendation is *consistent with*, not what it is *required by*.

## 22. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-VISION Section 9 (`G1`–`G12`) and Section 12 (`V1`–`V10`) | The guiding principles and invariants this document operationalizes without restating, cited throughout [Sections 5](#5-human-in-the-loop-recommendations)–[18](#18-long-term-maintainability-recommendations). |
| AEOS-PRD Section 7 (Product Philosophy), Section 9 (Engineering Lifecycle), Section 10 (Interaction Model), Section 12 (Product Capabilities) | The product philosophy, lifecycle stages, and capabilities this document's recommendations apply in practice. |
| AEOS-GLOSSARY | Terminology used without redefinition throughout this document, including *Human-in-the-Loop*, *Context*, *Context Minimization*, *Review*, *Rule*, *Repository Asset*, *TDD Cycle*, and *Implementation*. |
| AEOS-DOCSTD Section 4.1, 4.3, 7.3, 12 | The Developer Guide layer's position and purpose, this document's self-imposed normative-language restriction, and the review and finding-classification discipline [Section 23.4](#234-review-policy) adopts for this document. |
| AEOS-TECH Section 6 | The support-tier vocabulary [Section 12](#12-runtime-usage-recommendations) draws from without restating. |
| AEOS-ARCH | Referenced for the layer model and invariants [Section 9](#9-architecture-usage-recommendations) recommends practice around, without restating them. |
| AEOS-BLUEPRINT | Referenced for the subsystem decomposition [Section 10](#10-blueprint-usage-recommendations) recommends practice around, without restating it. |
| AEOS-SPECSTD, `SS-P-01`–`SS-P-10` | The Specification Principles [Section 11](#11-specification-authoring-recommendations) operationalizes without restating. |
| AEOS-BOOT, AEOS-ENVSETUP, AEOS-LAYOUT | Referenced as the existing Implementation Guides this document's own position, in the authority statement, is written in the spirit of; none of their content is restated here. |
| RFC 2119, RFC 8174 | The normative keyword vocabulary this document restricts itself to using at recommendation strength only, per the conformance notice above. |

---

## 23. Document Governance

### 23.1 Status

This document is a **Draft**. It is the first Developer Guide authored for AEOS, and is intended to
become the Best Practices Source of Truth once the owner's review under
[Section 23.4](#234-review-policy) records no Critical or Major finding, at which point it is
intended to be placed at `docs/developer/BEST_PRACTICES.md` — the directory AEOS-BOOT `BOOT-028`
reserved for it.

### 23.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `PRAC-<NNN>` or `AP-<NNN>` item that does not alter what an existing item recommends | Owner approval | Minor |
| Any change to what an existing `PRAC-<NNN>` or `AP-<NNN>` item recommends, or its retirement | Explicit owner revision request | Major |
| Addition or removal of a recommendation section ([Sections 5](#5-human-in-the-loop-recommendations)–[18](#18-long-term-maintainability-recommendations)) | Explicit owner revision request | Major |
| Any change that would convert a `SHOULD` or `MAY` recommendation into a `MUST` obligation | Explicit owner revision request, reasoned against [Section 4](#4-best-practice-philosophy)'s test for what belongs in this document, since a promoted recommendation ordinarily moves to the document that should own it as a Requirement or Rule, rather than remain here as one | Major |

### 23.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, no structural
decision, and no specified behavior. Ideas arising from it that would alter AEOS-VISION, AEOS-PRD,
AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPECSTD are recorded as
recommendations for the owning document's governance and are applied only after explicit owner
approval there — never enacted here.

### 23.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no
Critical or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally
confirms, before recommending freeze:

- [ ] Every recommendation in [Sections 5](#5-human-in-the-loop-recommendations) through
      [18](#18-long-term-maintainability-recommendations) uses **SHOULD** or **MAY**, and no capitalized
      **MUST** or **MUST NOT** appears outside a direct citation of another document's own obligation.
- [ ] Every `PRAC-<NNN>` and `AP-<NNN>` identifier carries a trace, consistent with
      [Section 21](#21-traceability).
- [ ] No recommendation restates a definition, requirement, architecture decision, Blueprint
      arrangement, specified behavior, or documentation rule that a governing document already
      states; each references rather than restates, per `PRAC-048`'s own standard applied reflexively.
- [ ] No recommendation depends on one Runtime, Vendor, Model, or Platform, per
      [Section 1](#1-executive-summary)'s stated properties.
- [ ] All twenty-five entries in this document's Table of Contents are present, in order, and none
      is silently empty.
- [ ] No Critical or Major finding remains open.

### 23.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPECSTD | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-BOOT, AEOS-ENVSETUP, or AEOS-LAYOUT on a procedure or placement rule | The Implementation Guide governs; this document's statement is corrected to reference it rather than restate it. |
| A `PRAC-<NNN>` recommendation and a project's own documented, reasoned practice differ | Neither is automatically correct. The project's practice stands where it is reasoned and recorded; declining a recommendation for a stated reason is not, by itself, a defect, per [Section 4.1](#41-what-makes-a-recommendation). |
| A future Coding Standard states a language-specific rule this document's guidance touches at the general level | The Coding Standard governs for that language once it exists. This document's more general recommendation stands elsewhere. |

### 23.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Best Practices Guide. States a Best Practice Philosophy distinguishing Best Practice, Requirement, Architecture Rule, Coding Standard, Implementation, and Recommendation without introducing new AEOS terminology; fifty-five `PRAC-<NNN>` recommendations across fourteen subject areas (Human-in-the-Loop, AI Collaboration, Repository Organization, Requirement Management, Architecture Usage, Blueprint Usage, Specification Authoring, Runtime Usage, Context Management, Review, Validation, Refactoring, Documentation, and Long-Term Maintainability), each traced to AEOS-VISION, AEOS-PRD, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, or AEOS-LAYOUT; ten named `AP-<NNN>` anti-patterns; ten non-goals; a traceability summary; and a document-governance apparatus consistent with AEOS-DOCSTD. Positions itself as the first Developer Guide authored for AEOS, occupying the `docs/developer/` directory AEOS-BOOT `BOOT-028` reserved. Restricts its own normative language to `SHOULD` and `MAY`, originating no `MUST`. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, no runtime procedure, and no technology selection. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-BOOT, AEOS-ENVSETUP, or AEOS-LAYOUT. |

---

## Appendix A — PRAC Recommendation Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each recommendation is its entry in
[Sections 5](#5-human-in-the-loop-recommendations) through [19](#19-common-anti-patterns).

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `PRAC-001`–`PRAC-004` | Human-in-the-Loop | 4 | [5](#5-human-in-the-loop-recommendations) |
| `PRAC-005`–`PRAC-009` | AI Collaboration | 5 | [6](#6-ai-collaboration-recommendations) |
| `PRAC-010`–`PRAC-013` | Repository Organization | 4 | [7](#7-repository-organization-recommendations) |
| `PRAC-014`–`PRAC-017` | Requirement Management | 4 | [8](#8-requirement-management-recommendations) |
| `PRAC-018`–`PRAC-020` | Architecture Usage | 3 | [9](#9-architecture-usage-recommendations) |
| `PRAC-021`–`PRAC-023` | Blueprint Usage | 3 | [10](#10-blueprint-usage-recommendations) |
| `PRAC-024`–`PRAC-028` | Specification Authoring | 5 | [11](#11-specification-authoring-recommendations) |
| `PRAC-029`–`PRAC-032` | Runtime Usage | 4 | [12](#12-runtime-usage-recommendations) |
| `PRAC-033`–`PRAC-036` | Context Management | 4 | [13](#13-context-management-recommendations) |
| `PRAC-037`–`PRAC-041` | Review | 5 | [14](#14-review-recommendations) |
| `PRAC-042`–`PRAC-044` | Validation | 3 | [15](#15-validation-recommendations) |
| `PRAC-045`–`PRAC-047` | Refactoring | 3 | [16](#16-refactoring-recommendations) |
| `PRAC-048`–`PRAC-050` | Documentation | 3 | [17](#17-documentation-recommendations) |
| `PRAC-051`–`PRAC-055` | Long-Term Maintainability | 5 | [18](#18-long-term-maintainability-recommendations) |
| **Total** | | **55** | — |
| `AP-001`–`AP-010` | Common Anti-Patterns | 10 | [19](#19-common-anti-patterns) |

## Appendix B — Best Practice Self-Check (Non-Normative)

A reflective aid for a Contributor or an AI runtime who wants to check their own recent work against
this document. This checklist carries no authority, gates nothing, and blocks no freeze, review, or
release; where it appears to diverge from Sections 5 through 19, those sections govern.

- [ ] Every consequential proposal could be approved or declined without the human reconstructing
      missing context (`PRAC-001`).
- [ ] Every Automation Grant in use is as narrow as the task in front of you (`PRAC-002`).
- [ ] No Destructive action proceeded on a general grant without its own confirmation (`PRAC-003`).
- [ ] No task description or Rule was written around one Runtime's, Vendor's, or Model's specific
      behavior (`PRAC-005`, `PRAC-030`).
- [ ] New content landed at the path AEOS-LAYOUT's own rules resolve to, on the first attempt
      (`PRAC-010`).
- [ ] Every piece of work states which `PR-` identifier it satisfies (`PRAC-014`).
- [ ] No architectural or Blueprint boundary was worked around to make a task easier (`PRAC-019`,
      `PRAC-022`).
- [ ] Every Specification rule written could be turned into a test as it was written (`PRAC-024`).
- [ ] Every Context item included could be individually justified (`PRAC-033`).
- [ ] Every review finding was classified, cited, and either resolved, declined with a reason, or
      escalated (`PRAC-037`, `PRAC-040`).
- [ ] Every failing test was confirmed to fail for the right reason before implementation began
      (`PRAC-042`).
- [ ] Every refactor happened under a green suite and stayed within its stated scope (`PRAC-045`,
      `PRAC-046`).
- [ ] No definition already owned by AEOS-GLOSSARY was restated (`PRAC-048`).
- [ ] Anything touched was left at least as understandable as it was found (`PRAC-052`).

---

**End of Best Practices Guide**

AEOS-PRACTICES · Version 1.0.0 · Recommends practice consistent with AEOS-VISION · AEOS-PRD ·
AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-TECH · AEOS-ARCH · AEOS-BLUEPRINT · AEOS-SPECSTD · AEOS-BOOT ·
AEOS-ENVSETUP · AEOS-LAYOUT, without restating or overriding any of them
