# AI Engineering Operating System

## AEOS — Review Guide

*The permanent statement of how review is conducted, classified, and concluded for work performed
under AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | Review Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-REVIEW |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Review Governance Board, AEOS |
| **Audience** | Contributors, reviewers, maintainers, and AI runtimes performing or subject to review of AEOS repository changes |
| **Suggested path** | `docs/REVIEW_GUIDE.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states the review procedure common to every kind of AEOS repository change
> `PR-REP-011` requires AEOS to support reviewing: its lifecycle, its checkpoints, its severity
> classification, the criteria under which a reviewed artifact is accepted, and the discipline that
> applies identically to a human reviewer, an AI Runtime reviewer, and more than one Runtime
> reviewing together.
>
> This document is not a Vision document, not a Product Requirements document, not an Architecture
> document, not a Blueprint, not a Specification, and not a coding standard. It states no product
> requirement, no architectural decision, no Blueprint arrangement, no specified behavior, and no
> language-specific convention; where a statement here appears to do any of these, that is a defect
> in this document and MUST be reported rather than acted upon.
>
> Where the reviewed artifact is an AEOS document, AEOS-DOCSTD Section 12 governs its lifecycle and
> freeze in full. This document states no rule that competes with, narrows, or varies Section 12, and
> names the document case as already fully governed wherever it applies.
>
> This document's position in the documentation hierarchy AEOS-DOCSTD Section 4.1 defines is reserved
> to the owner's decision under AEOS-DOCSTD rule `H5`. [Section 2.3](#23-position-in-the-documentation-hierarchy)
> states the reservation and the reasoning behind it in full.
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
3. [Review Principles](#3-review-principles)
4. [Distinguishing Review from Adjacent Practices](#4-distinguishing-review-from-adjacent-practices)
5. [Human-in-the-Loop Review Principles](#5-human-in-the-loop-review-principles)
6. [AI Review Principles](#6-ai-review-principles)
7. [Multi-Model Review Guidance](#7-multi-model-review-guidance)
8. [Review Lifecycle](#8-review-lifecycle)
9. [Review Checkpoints](#9-review-checkpoints)
10. [Review Severity Classification](#10-review-severity-classification)
11. [Acceptance Criteria](#11-acceptance-criteria)
12. [Freeze Criteria](#12-freeze-criteria)
13. [Re-Review Policy](#13-re-review-policy)
14. [Review Artifacts](#14-review-artifacts)
15. [Review Traceability](#15-review-traceability)
16. [Non-Goals](#16-non-goals)
17. [Traceability](#17-traceability)
18. [References](#18-references)
19. [Document Governance](#19-document-governance)
20. [Appendix A — Review Checklist (Non-Normative)](#appendix-a--review-checklist-non-normative)
21. [Appendix B — REVIEW Rule Index (Non-Normative)](#appendix-b--review-rule-index-non-normative)

---

## 1. Executive Summary

AEOS-VISION Section 6.9 states a single conviction: nothing enters the repository — code,
configuration, rules, documentation, generated or hand-written — without a human having had a real
opportunity to reject it. AEOS-PRD turns that conviction into a product capability: `PR-REP-011`
requires that AEOS support review workflows evaluating changes against requirements, rules, and
tests, and reporting findings classified as Critical, Major, Minor, or Nitpick. AEOS-DOCSTD Section
12 already states, in full, what that means for one kind of artifact — an AEOS document reaching
Frozen status. It states nothing for any other kind of AEOS work: an Implementation change, a Rule, a
Skill, a Prompt, a Workflow, or a Profile, each of which `PR-REP-011` also requires AEOS to support
reviewing.

This document closes that gap. It states the review procedure common to every kind of artifact review
of AEOS work touches — its lifecycle, its checkpoints, its severity classification, the criteria under
which a reviewed artifact is accepted, and the discipline that applies identically whether the
reviewer is a human or an AI Runtime, and whether one Runtime reviews or several do. It does not
restate AEOS-DOCSTD Section 12; where the reviewed artifact is a document, that section governs
exactly as written, and this document names the document case as the one already fully stated
elsewhere.

This document states no coding standard, no implementation specification, and no architectural
decision. It states a procedure: how review, once it begins, proceeds from the moment an artifact is
proposed to the moment it is accepted or declined.

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs:

- the review procedure common to every kind of AEOS work `PR-REP-011` requires AEOS to support
  reviewing;
- the point at which review occurs relative to the rest of the engineering lifecycle AEOS-PRD Section
  9 states;
- the classification of review findings and its effect on acceptance;
- the criteria under which a reviewed artifact reaches its terminal state;
- the discipline governing a human reviewer, an AI Runtime reviewer, and more than one Runtime
  reviewing the same artifact;
- the record a review MUST leave behind and the identifiers a finding MUST cite.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| The lifecycle and freeze of an AEOS document | AEOS-DOCSTD Section 12 |
| Why review exists as a product conviction | AEOS-VISION Section 6.9 |
| What AEOS must do to support review | AEOS-PRD `PR-REP-011` and related requirements |
| What "Review," "Freeze," "Human Approval," and "Approval Gate" mean | AEOS-GLOSSARY |
| The Specification layer's own freeze checklist | AEOS-SPECSTD Section 19 |
| Coding standards, style rules, or language-specific conventions | Not yet governed by any frozen AEOS document |
| How a repository is bootstrapped or structured | AEOS-BOOT; REPOSITORY_LAYOUT.md |
| Testing, verification of behavior, and test-tooling orchestration | AEOS-PRD `PR-TDD` and the TDD Cycle |

A statement here that redefines a product requirement, the Vision, a glossary term, or a structural
decision is a **defect in this document**. It MUST be reported rather than acted upon.

### 2.3 Position in the Documentation Hierarchy

This document's position in the documentation hierarchy AEOS-DOCSTD Section 4.1 defines is reserved to
the owner's decision under AEOS-DOCSTD rule `H5`, in the same manner AEOS-DOCSTD Section 4.5 reserves
the Runtime layer's position, and in the same manner `AEOS_RUNTIME_FLOW.md`, `PROJECT_BOOTSTRAP.md`,
and `REPOSITORY_LAYOUT.md` record for their own comparable position. Until decided, this document
complies with every rule in AEOS-DOCSTD that does not itself depend on hierarchy position.

Two existing documents bear on where this position will eventually fall. AEOS-BOOT Section 4 names
"review process" as an example of content belonging to `docs/developer/` — task-oriented instruction
on working within an already-built result. AEOS-DOCSTD Section 4.3 states that an Implementation
Guide answers "how is the specified behavior realized," and that a Developer Guide "does not
contain… authority. A guide describes; it never decides." This document decides: it states, with RFC
2119 and RFC 8174 force, when a reviewed artifact is accepted and when it is not. AEOS-BOOT's example
illustrates a different document — task-oriented instruction for an individual reviewer's own working
practice, which this document does not attempt and records as `NG-9` in [Section 16](#16-non-goals).
Until the owner resolves the question, this document uses normative language as AEOS-BOOT and
REPOSITORY_LAYOUT already do at their own reserved position, recording the deviation from AEOS-DOCSTD
Section 7.3's Developer Guide default openly, per Section 7.2's rule that a SHOULD-level deviation
MUST be deliberate and SHOULD be recorded.

This document's Suggested path is `docs/REVIEW_GUIDE.md`, placed directly under `docs/` outside every
layer-specific subdirectory AEOS-BOOT Section 4 names, for the same reason REPOSITORY_LAYOUT records
for its own placement: claiming no layer's subdirectory before the owner assigns one.

### 2.4 Applicability

This document applies to review of any AEOS repository change — a document, an Implementation change,
a Rule, a Skill, a Prompt, a Workflow, or a Profile — performed after this document reaches Frozen
status. It applies identically to a human reviewer and an AI Runtime reviewer, consistent with
AEOS-DOCSTD Section 2.4's rule that a Standard binds whichever kind of participant it addresses.

This document does not retroactively invalidate a review already completed before this document
existed. Where a stated rule and an already-completed review diverge, the divergence is recorded and
reconciled at the artifact's own next Review, not by revisiting the earlier decision.

---

## 3. Review Principles

These principles constrain every rule in this document. A rule that violates one is defective, not
merely unconventional.

| # | Principle |
| :--- | :--- |
| `REVIEW-P-01` | Review examines a result; it does not redesign the artifact under review. |
| `REVIEW-P-02` | Nothing enters the repository without a human having had a real opportunity to reject it. |
| `REVIEW-P-03` | A finding is classified into exactly one of four severities — Critical, Major, Minor, or Nitpick — a closed set that MUST NOT be extended, renamed, or reordered. |
| `REVIEW-P-04` | This document grants no document new authority; where a subject already has an owning document, that document's ownership and rules govern, unchanged. |
| `REVIEW-P-05` | Where the reviewed artifact is a document, AEOS-DOCSTD Section 12 governs its lifecycle and freeze; this document states the discipline common to every artifact kind without restating Section 12. |
| `REVIEW-P-06` | A review's outcome does not depend on which Runtime or Model performed the examination. |
| `REVIEW-P-07` | Review is distinct from testing, verification, and validation, and does not substitute for any of them. |
| `REVIEW-P-08` | A finding cites the specific location and, where a rule or requirement is violated, the specific identifier. |
| `REVIEW-P-09` | Explanation before execution concerns an intent; Review examines a result. Both are required, and neither substitutes for the other. |

---

## 4. Distinguishing Review from Adjacent Practices

The task that motivates this document names six terms that are easy to conflate: Review, Validation,
Testing, Verification, Approval, and Freeze. AEOS-GLOSSARY does not define a bare term "Approval" —
it defines *Human Approval*, the act, and *Approval Gate*, the point at which that act is required —
so the table below states both rather than inventing a single "Approval" entry the Glossary does not
carry. AEOS-GLOSSARY defines four of the resulting seven rows; it does not yet define Validation,
Testing, or Verification as distinct AEOS terms. Consistent with AEOS-DOCSTD `T3`, this document does
not assume the authority to define them here; where they appear below, they carry their ordinary
engineering meaning, not an AEOS-specific one.

| Term | Meaning in AEOS | Authority |
| :--- | :--- | :--- |
| **Review** | The examination of an artifact, change, or document against requirements, rules, and tests, producing findings classified as Critical, Major, Minor, or Nitpick. | AEOS-GLOSSARY, *Review* |
| **Human Approval** | The explicit act by which a person authorizes a specific proposed action before it is executed. | AEOS-GLOSSARY, *Human Approval* |
| **Approval Gate** | The point at which a proposed action requires explicit human confirmation before execution. | AEOS-GLOSSARY, *Approval Gate* |
| **Freeze** | The governance state in which a document's definitions and decisions MUST NOT change except through the change control that document defines. | AEOS-GLOSSARY, *Freeze* |
| **Testing** | Not independently defined by AEOS-GLOSSARY. In AEOS, testing is realized by the TDD Cycle AEOS-PRD `PR-TDD` requires: running the project's existing test tooling and reporting pass or fail against a stated behavior. | AEOS-PRD `PR-TDD`; AEOS-GLOSSARY, *TDD Cycle* |
| **Verification** | Not independently defined by AEOS-GLOSSARY. AEOS-VISION invariant `V4` states that verification precedes implementation; the TDD Cycle's own second position — confirming a new test fails for the intended reason — is AEOS's stated instance of it. Elsewhere the word carries its ordinary engineering meaning: confirming that an artifact meets a specific, previously stated expectation. | AEOS-VISION `V4` |
| **Validation** | Not defined by any frozen AEOS document. Where used in this document or elsewhere in AEOS documentation, it carries its ordinary engineering meaning — confirming that an artifact meets the need it was built for — and asserts no AEOS-specific rule. | Not currently owned by any AEOS document |

Review and Testing examine different things by different means: Testing runs code against a stated
behavior and reports pass or fail; Review examines an artifact — which may itself include test
results among the evidence considered — against requirements, rules, and tests, and produces a
classified finding rather than a pass/fail result. AEOS-VISION Section 6.9 distinguishes Review from
Explain Before Execute the same way: explanation concerns an intent, formed before an action; Review
examines a result, after it exists. Approval is the act that satisfies an Approval Gate — a decision,
not an examination — and may follow a Review's findings without itself being one. Freeze is the
terminal governance state AEOS-DOCSTD Section 12 reserves for documents; [Section 11](#11-acceptance-criteria)
states the parallel terminal state, Acceptance, this document defines for every other artifact kind.

---

## 5. Human-in-the-Loop Review Principles

AEOS-PRD Section 7.1 states Human-in-the-Loop by Default as the product's default posture: a human
decides before AEOS acts consequentially. Review interacts with that posture in a specific way:
examining an artifact and reporting findings is itself an Observation, per AEOS-PRD Section 10.1's
Action Class table — it reads and reports, and changes nothing. Acting on what Review finds —
accepting, rejecting, or requesting revision of the reviewed artifact — is not an Observation, and
requires the Human Approval AEOS-PRD Section 10's Confirm phase and AEOS-GLOSSARY's *Human Approval*
entry both describe.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-001` | Review MAY be performed, and its findings reported, as an Observation-class action requiring no Approval Gate of its own. | `PR-WFL-006` |
| `REVIEW-002` | Accepting, rejecting, or requesting revision of a reviewed artifact in response to a finding MUST be a Human Approval, and MUST NOT be inferred from silence or from an approval of a different action. | `PR-SAF-005`; AEOS-GLOSSARY *Human Approval* |
| `REVIEW-003` | A Critical or Major finding MUST be presented to a human before the reviewed artifact reaches Acceptance or Freeze, regardless of which reviewer produced it. | AEOS-VISION §6.9; `PR-REP-011` |
| `REVIEW-004` | An Automation Grant MAY cover acceptance of a reviewed artifact only where no Critical or Major finding remains open, and MUST NOT cover acceptance of a Destructive-class change under any grant. | `PR-WFL-014`; `PR-SAF-012` |
| `REVIEW-005` | Every finding reaches a disposition of resolved, declined with a recorded reason, or escalated before the artifact proceeds past Revision; an undispositioned finding remains open. | AEOS-DOCSTD §12.2, Stage 3 |

---

## 6. AI Review Principles

AEOS-DOCSTD Section 2.4 applies identically to human and AI authors; this document applies the same
rule to reviewers. AEOS performs no inference of its own — AEOS-VISION invariant `V1` — but a
connected Runtime MAY be delegated the work of examining an artifact and reporting findings, on the
same terms `PR-RUL-005` already states for rule application "during generation, review, and
refactoring."

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-006` | An AI Runtime MAY perform Review and report findings classified under the same four severities a human reviewer's findings carry, with no distinction of authority by source. | `PR-RUL-005`; AEOS-DOCSTD §2.4 |
| `REVIEW-007` | A finding's validity depends on whether it correctly applies the requirement, rule, or test it cites, never on which Runtime or Model produced it. | AEOS-VISION `V6`; AEOS-GLOSSARY *Vendor* |
| `REVIEW-008` | An AI Runtime performing Review MUST cite the specific rule or requirement identifier a finding applies, on the same terms AEOS-DOCSTD Section 12.4 requires of any reviewer. | AEOS-DOCSTD §12.4 |
| `REVIEW-009` | An AI Runtime's finding does not, by itself, constitute the Human Approval `REVIEW-002` requires, and does not advance a reviewed artifact past Acceptance or Freeze without a human decision. | `REVIEW-002`; AEOS-VISION §6.9 |
| `REVIEW-010` | Where an AI Runtime performs Review, its findings MUST NOT be presented as an observed fact rather than the Runtime's output. | `PR-SAF-011` |

---

## 7. Multi-Model Review Guidance

AEOS-PRD `PR-RUN-013` permits orchestrating more than one Runtime within a single workflow when the
user configures it. Review MAY be one such workflow. This section states how the governance in
Sections 5 and 6 applies unchanged when more than one Runtime or Model reviews the same artifact.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-011` | More than one Runtime or Model MAY independently review the same artifact within one review workflow. | `PR-RUN-013` |
| `REVIEW-012` | Findings from different Runtimes or Models on the same artifact are aggregated by severity classification alone; no finding is weighted, discounted, or preferred because of the Runtime or Model that produced it. | AEOS-VISION `V6`; AEOS-GLOSSARY *Vendor* |
| `REVIEW-013` | Where two Runtimes or Models produce conflicting findings about the same location, the conflict MUST be reported for human resolution and MUST NOT be silently resolved by AEOS choosing one Runtime's or Model's judgment over the other's. | AEOS-DOCSTD §11.3, generalized |
| `REVIEW-014` | Switching or adding the Runtime or Model performing Review MUST require no change to this document, to a Rule, or to the Workflow the review applies. | `PR-RUN-005` |
| `REVIEW-015` | AEOS's support for Review is independent of any specific Model, model family, or model version. | `PR-RUN-006` |

---

## 8. Review Lifecycle

AEOS-DOCSTD Section 12.2 states five stages every AEOS document's lifecycle follows: Draft, Review,
Revision, Approval, Freeze. This document applies the same five stages, under the same names, to
every kind of AEOS work `PR-REP-011` requires review support for — substituting Acceptance for Freeze
wherever the artifact is not a document, since AEOS-GLOSSARY reserves *Freeze* for a document's
definitions and decisions specifically.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-016` | A reviewed artifact MUST pass through Draft, Review, and Revision, in that order, before reaching Acceptance or Freeze. | AEOS-DOCSTD §12.2 |
| `REVIEW-017` | Stages Review and Revision repeat until no Critical or Major finding remains open. | AEOS-DOCSTD §12.2 |
| `REVIEW-018` | Where the reviewed artifact is a document, its lifecycle states and their meaning are exactly those AEOS-DOCSTD Section 12.1 defines — Draft, In Review, In Revision, Approved, Frozen, Superseded — and are not restated by this document. | AEOS-DOCSTD `DS-P-07` |
| `REVIEW-019` | Where the reviewed artifact is not a document, its terminal state is Acceptance, reached under [Section 11](#11-acceptance-criteria), and is not named Freeze. | AEOS-GLOSSARY *Freeze* |

---

## 9. Review Checkpoints

A checkpoint is a point already stated elsewhere at which Review occurs. This section names each one;
it does not invent a checkpoint no other AEOS document already states.

| Artifact kind | Checkpoint | Stated by |
| :--- | :--- | :--- |
| Document | Stage 2 (Review) of AEOS-DOCSTD Section 12.2, following Draft and preceding Revision. | AEOS-DOCSTD §12.2 |
| Implementation change | Stage 8 (Review) of AEOS-PRD Section 9's engineering lifecycle, following Code generation and preceding Refactoring. | AEOS-PRD §9 |
| Documentation change | The proposal-and-approval point AEOS-PRD `PR-DOC-006` states before writing, and the Review this document's Section 8 states after. | `PR-DOC-006` |
| Any Repository Asset `PR-REP-011` names | The point at which the asset is proposed for acceptance, before the Confirm phase of AEOS-PRD Section 10's Interaction Model collects Human Approval for it. | `PR-REP-011`; AEOS-PRD §10 |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-020` | A Review checkpoint MUST occur after an artifact is generated or revised and before it is proposed for the Human Approval that would accept it. | AEOS-PRD §10, Confirm phase |
| `REVIEW-021` | At least one Review checkpoint MUST have occurred for the accepted state before a document reaches Freeze or another Repository Asset reaches Acceptance. | AEOS-DOCSTD `DS-P-04`, generalized |

---

## 10. Review Severity Classification

AEOS-GLOSSARY's *Review* entry states the four severities a Review finding carries — Critical, Major,
Minor, Nitpick — as a closed set that MUST NOT be extended, renamed, or reordered, tracing to
`PR-REP-011`. AEOS-DOCSTD Section 12.3 states the definition of each class for a document. This
document restates none of those definitions; it states only how the same closed set applies beyond
the document case.

| Class | Effect on Freeze (document, per AEOS-DOCSTD §12.3) | Effect on Acceptance (this document, §11) |
| :--- | :--- | :--- |
| Critical | Blocks | Blocks |
| Major | Blocks | Blocks |
| Minor | Does not block; recorded and normally addressed | Does not block; recorded |
| Nitpick | Does not block; addressed at author's discretion | Does not block; addressed at author's or accepting human's discretion |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-022` | A finding classified for a non-document artifact under this document's Section 11 carries the same definition of Critical, Major, Minor, and Nitpick that AEOS-DOCSTD Section 12.3 states for a document, applied to the artifact under review in place of a document. | AEOS-DOCSTD §12.3, generalized |

---

## 11. Acceptance Criteria

Freeze, per AEOS-GLOSSARY, is the terminal governance state a document reaches. This section states
the parallel terminal state, Acceptance, for every artifact kind [Section 2.4](#24-applicability)
covers that is not a document.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-023` | An artifact reaches Acceptance when its Review records no open Critical or Major finding and the Human Approval `REVIEW-002` requires has been obtained for its accepted state. | AEOS-DOCSTD `DS-P-04`, generalized |
| `REVIEW-024` | Acceptance authorizes exactly the reviewed state; a subsequent change to the artifact requires a new Review before its own Acceptance. | AEOS-GLOSSARY *Proposal* |
| `REVIEW-025` | A Minor or Nitpick finding does not block Acceptance and MAY be resolved at the author's or the accepting human's discretion. | AEOS-DOCSTD §12.3, generalized |
| `REVIEW-026` | Where the reviewed artifact is a document, Acceptance under this section is satisfied by, and only by, that document reaching Frozen status under AEOS-DOCSTD Section 12.1 and 12.2. | AEOS-DOCSTD §12.1, §12.2 |

---

## 12. Freeze Criteria

AEOS-DOCSTD Section 12 states the complete freeze criteria for a document — its lifecycle states, its
stage order, its finding classification, its review conduct, and what a freeze means. This document
restates none of it.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-027` | Freeze criteria for a document are stated exclusively by AEOS-DOCSTD Section 12; a statement in this document that appears to add, narrow, or vary a freeze criterion for a document is a defect in this document and MUST be reported rather than acted upon. | AEOS-DOCSTD `DS-P-07`, `DS-P-08` |

---

## 13. Re-Review Policy

A change to an artifact that has already reached Acceptance or Freeze does not inherit its prior
Review. AEOS-DOCSTD `E7` and `E8` already state this for a frozen document's content; this section
applies the same discipline to every artifact kind.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-028` | A change to an artifact that has reached Acceptance or Freeze MUST undergo a new Review before its own Acceptance or Freeze; a prior Review does not carry forward to changed content. | AEOS-DOCSTD `E7`, `E8`, generalized |
| `REVIEW-029` | Where a Revision addresses every open Critical and Major finding, the artifact returns to Review rather than proceeding directly to Acceptance or Freeze. | AEOS-DOCSTD §12.2 |
| `REVIEW-030` | A re-review examines the changed content and every finding a prior Review left open; it does not re-examine content the change did not touch and an earlier Review already accepted, unless the earlier finding is itself in question. | `REVIEW-P-01` |

---

## 14. Review Artifacts

AEOS-PRD `PR-WFL-015` requires that AEOS record an auditable history of proposals, decisions, and
executions within a project. This section states what a Review, specifically, leaves behind in that
history.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-031` | A completed Review leaves behind, at minimum, the artifact examined, the findings recorded and their classification, the disposition each finding reached under `REVIEW-005`, and the resulting Acceptance, Freeze, or decline. | `PR-WFL-015` |
| `REVIEW-032` | A Review's record MUST identify whether each finding was produced by a human reviewer or an AI Runtime, without asserting a preference between the two. | `PR-SAF-011`; `REVIEW-P-06` |
| `REVIEW-033` | A Review record is retained as part of the repository's history AEOS-PRD `PR-REP-002` already requires; nothing essential about a completed Review exists only outside the repository. | `PR-REP-002`; AEOS-DOCSTD `DS-P-01` |

---

## 15. Review Traceability

AEOS-DOCSTD Section 12.4 already requires that a reviewer cite the specific location and, where the
finding is a Standard violation, the specific rule identifier. This section applies the same
requirement beyond documentation review, and states what an Acceptance or Freeze decision itself must
remain traceable to.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `REVIEW-034` | A finding MUST cite the specific location in the reviewed artifact and, where a rule, requirement, or specification identifier is violated, that identifier, on the same terms AEOS-DOCSTD Section 12.4 already requires of a reviewer. | AEOS-DOCSTD §12.4 |
| `REVIEW-035` | An Acceptance or Freeze decision MUST be traceable to the set of findings open at the time of the decision and their disposition, so that a later reader can determine what was known and accepted without re-deriving it from a chat transcript or other record outside the repository. | AEOS-DOCSTD `DS-P-01`; `PR-REP-002` |

---

## 16. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document, in
the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Defining a coding standard, style rules, or language-specific conventions. | Not yet governed by any frozen AEOS document. |
| `NG-2` | Restating AEOS-DOCSTD Section 12's lifecycle, finding classification, or freeze criteria for documents. | AEOS-DOCSTD Section 12. |
| `NG-3` | Restating AEOS-SPECSTD Section 19's Specification-specific freeze checklist. | AEOS-SPECSTD Section 19. |
| `NG-4` | Stating the ordered procedure by which a repository is bootstrapped or structured. | AEOS-BOOT; REPOSITORY_LAYOUT.md. |
| `NG-5` | Orchestrating test tooling or verifying behavior. | AEOS-PRD `PR-TDD`. |
| `NG-6` | Defining "Validation," "Testing," or "Verification" as AEOS-specific terms. | Reserved to AEOS-GLOSSARY, if the owner decides a definition is needed. |
| `NG-7` | Naming a technology, vendor, Runtime, or Model as preferred for performing Review. | AEOS-TECH; AEOS-PRD `PR-RUN`. |
| `NG-8` | Positioning this document within the AEOS-DOCSTD documentation hierarchy. | Reserved to the owner, under AEOS-DOCSTD `H5`. |
| `NG-9` | Providing task-oriented, day-to-day instruction for an individual performing a review. | A future Developer Guide, once authored, per AEOS-BOOT's own boundary note. |
| `NG-10` | Assigning a Review responsibility to an architectural component. | Reserved to AEOS-ARCH, which currently names no such component. |

---

## 17. Traceability

Every `REVIEW-` rule and `REVIEW-P-` principle in this document traces to one or more `PR-`
identifiers, an AEOS-VISION section, an AEOS-DOCSTD rule, or a principle stated in
[Section 3](#3-review-principles), stated inline in Sections 5 through 15 and indexed in full in
[Appendix B](#appendix-b--review-rule-index-non-normative).

| Trace target | Rules in this document that trace to it |
| :--- | :--- |
| `PR-WFL` | `REVIEW-001` · `REVIEW-004` · `REVIEW-031` |
| `PR-SAF` | `REVIEW-002` · `REVIEW-004` · `REVIEW-010` · `REVIEW-032` |
| `PR-REP` | `REVIEW-P-03` · `REVIEW-003` · `REVIEW-033` · `REVIEW-035` |
| `PR-RUL` | `REVIEW-006` |
| `PR-RUN` | `REVIEW-P-06` · `REVIEW-011` · `REVIEW-014` · `REVIEW-015` |
| AEOS-VISION | `REVIEW-P-02` · `REVIEW-P-07` · `REVIEW-P-09` · `REVIEW-003` · `REVIEW-007` · `REVIEW-009` · `REVIEW-012` |
| AEOS-PRD (section reference, no `PR-` identifier) | `REVIEW-020` |
| AEOS-DOCSTD | `REVIEW-P-04` · `REVIEW-P-05` · `REVIEW-P-08` · `REVIEW-005` · `REVIEW-006` · `REVIEW-008` · `REVIEW-013` · `REVIEW-016` · `REVIEW-017` · `REVIEW-018` · `REVIEW-021` · `REVIEW-022` · `REVIEW-023` · `REVIEW-025` · `REVIEW-026` · `REVIEW-027` · `REVIEW-028` · `REVIEW-029` · `REVIEW-034` · `REVIEW-035` |
| AEOS-GLOSSARY | `REVIEW-P-01` · `REVIEW-002` · `REVIEW-007` · `REVIEW-012` · `REVIEW-019` · `REVIEW-024` |
| This document (self-reference) | `REVIEW-009` → `REVIEW-002` · `REVIEW-030` → `REVIEW-P-01` · `REVIEW-032` → `REVIEW-P-06` |

This document, as a whole, traces to AEOS-DOCSTD `H4`'s requirement that every derivative document
trace to the layer above it — satisfied here by tracing each rule to AEOS-VISION, AEOS-DOCSTD, or the
`PR-` requirement it ultimately serves — and to AEOS-DOCSTD `H6`'s requirement that a document belong
to a layer, addressed by the reserved-position statement in [Section 2.3](#23-position-in-the-documentation-hierarchy).

---

## 18. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-VISION Section 6.9 | The Review before Execution conviction this document's purpose serves |
| AEOS-VISION invariant `V1` | The statement that AEOS performs no inference of its own, cited in Section 6 |
| AEOS-VISION invariant `V4` | The verification-precedes-implementation invariant informing Section 4's distinction |
| AEOS-VISION invariant `V6` | The vendor, runtime, and model neutrality informing Sections 6 and 7 |
| AEOS-PRD `PR-REP-011` | The product requirement this document's review procedure realizes |
| AEOS-PRD `PR-RUN-005`, `PR-RUN-006`, `PR-RUN-013` | The runtime and model independence requirements Section 7 applies to Review specifically |
| AEOS-PRD `PR-RUL-005` | The requirement that rules apply during review, cited in Section 6 |
| AEOS-PRD Section 9 | The engineering lifecycle's Review stage, cited in Section 9 |
| AEOS-PRD Section 10 | The Interaction Model's Confirm phase and Action Class table, cited in Section 5 |
| AEOS-GLOSSARY, *Review*, *Freeze*, *Human Approval*, and *Approval Gate* entries | The terminology this document uses without redefinition |
| AEOS-DOCSTD Section 12 | The document review and freeze lifecycle this document generalizes without restating |
| AEOS-DOCSTD Section 2.4 | The rule that a Standard binds human and AI authors identically, applied to reviewers in Section 6 |
| AEOS-DOCSTD `H5`, `H6` | The hierarchy position this document reserves, per Section 2.3 |
| AEOS-SPECSTD Section 19 | The Specification layer's own freeze checklist, deferred to and not restated |
| AEOS-BOOT Section 4 | The Implementation Guide / Developer Guide boundary note this document's Section 2.3 addresses |
| REPOSITORY_LAYOUT.md Section 5.2 | The reserved-hierarchy-position pattern this document's Section 2.3 follows |

---

## 19. Document Governance

### 19.1 Status

This document is a **Draft**. It is the first Review Guide authored for AEOS. It defines its own
change control, in the absence of a governing Standard for the position it occupies, consistent with
AEOS-DOCSTD Section 13.3's default for a document that does not otherwise state one — the same
default `AEOS_RUNTIME_FLOW.md`, AEOS-BOOT, and REPOSITORY_LAYOUT record for their own comparable
position.

### 19.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `REVIEW-` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Resolving a Non-Goal in [Section 16](#16-non-goals) by stating a concrete rule, once a governing decision authorizes it | Owner approval | Minor |
| Any change to what an existing `REVIEW-` rule or `REVIEW-P-` principle requires, or its retirement | Explicit owner revision request | Major |
| Assignment of this document to a hierarchy layer under `H5` | Explicit owner revision request | Major |
| Any change that would contradict AEOS-DOCSTD Section 12 | Not permitted here; the change belongs to AEOS-DOCSTD's own change control | — |

### 19.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-VISION, AEOS-GLOSSARY, or AEOS-DOCSTD
are recorded as recommendations for the owning document's governance and are applied only after
explicit owner approval there — never enacted here.

### 19.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in Sections 5 through 15 carries a `REVIEW-` identifier and a trace.
- [ ] No rule restates AEOS-DOCSTD Section 12's lifecycle, finding classification, or freeze criteria
      for documents.
- [ ] No rule states a coding standard, an implementation procedure beyond review, a runtime
      behavior, an architectural decision, or a product requirement.
- [ ] Every artifact kind [Section 9](#9-review-checkpoints)'s checkpoint table names cites the
      document that actually states the checkpoint, rather than asserting a new one.
- [ ] All twenty-one entries in this document's Table of Contents are present, in order, and none is
      silently empty.
- [ ] No Critical or Major finding remains open.

### 19.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-DOCSTD Section 12 on the lifecycle or freeze of a document | AEOS-DOCSTD governs. The conflict is a defect in this document's affected section, corrected to match AEOS-DOCSTD rather than the reverse. |
| This document conflicts with AEOS-SPECSTD Section 19 on the freeze of a Specification document | AEOS-SPECSTD governs for that layer; this document is corrected to reference it rather than restate it. |
| A future Developer Guide states day-to-day review practice that does not vary what this document requires | Both stand. This document governs the procedure and its criteria; the guide governs an individual's practice within it. |
| A future document names a hierarchy layer for this document under `H5` | This document's Section 2.3 is updated to match at the next revision; the layer assignment itself is made there, not here. |

### 19.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Review Guide. States nine review principles; a seven-row distinction among Review, Human Approval, Approval Gate, Freeze, Testing, Verification, and Validation, the last three carrying no AEOS-specific definition and recorded as such; five Human-in-the-Loop review principles; five AI review principles; five multi-model review principles; a four-rule generalization of AEOS-DOCSTD Section 12's five-stage lifecycle beyond the document layer; a review-checkpoint table across four artifact kinds with two supporting rules; a severity-classification table extending AEOS-DOCSTD Section 12.3's closed four-class set to Acceptance; four acceptance criteria; a single deferential freeze-criteria rule; three re-review rules; three review-artifact rules; two review-traceability rules; ten non-goals; and thirty-five `REVIEW-<NNN>` rules in total, all traced. Reserves this document's position in the AEOS-DOCSTD documentation hierarchy to the owner under `H5`, following the pattern `AEOS_RUNTIME_FLOW.md`, AEOS-BOOT, and REPOSITORY_LAYOUT record for their own comparable position. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no coding standard. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, or AEOS-SPECSTD. |

---

## Appendix A — Review Checklist (Non-Normative)

**This appendix is non-normative.** It restates the obligations Sections 5 through 15 already state
normatively, arranged as a checklist a reviewer MAY follow in order. Where this checklist and a
numbered section appear to differ, the numbered section governs.

- [ ] The artifact and the specific change under review are identified.
- [ ] Every finding is classified as Critical, Major, Minor, or Nitpick — no fifth category is used.
- [ ] Every finding cites the specific location and, where applicable, the specific rule or
      requirement identifier violated (`REVIEW-034`).
- [ ] Where more than one Runtime or Model reviewed the artifact, conflicting findings on the same
      location are recorded for human resolution rather than silently reconciled (`REVIEW-013`).
- [ ] Every finding has reached a disposition — resolved, declined with a recorded reason, or
      escalated — before the artifact proceeds past Revision (`REVIEW-005`).
- [ ] No open Critical or Major finding remains before Acceptance or Freeze is proposed
      (`REVIEW-003`, `REVIEW-023`).
- [ ] The Human Approval accepting the artifact is explicit and attaches to exactly the reviewed
      state (`REVIEW-002`, `REVIEW-024`).
- [ ] Where the artifact is a document, its Freeze is confirmed against AEOS-DOCSTD Section 12, not
      against this checklist (`REVIEW-026`).
- [ ] The review record — artifact, findings, dispositions, and outcome — is retained in the
      repository (`REVIEW-031`, `REVIEW-033`).

---

## Appendix B — REVIEW Rule Index (Non-Normative)

**This appendix is non-normative.** It indexes every rule and principle this document states, for a
reader locating one without reading the document in full.

| ID | Section | Summary | Traces to |
| :--- | :--- | :--- | :--- |
| `REVIEW-P-01` | 3 | Review examines a result; it does not redesign | AEOS-GLOSSARY *Review* |
| `REVIEW-P-02` | 3 | Nothing enters without opportunity to reject | AEOS-VISION §6.9 |
| `REVIEW-P-03` | 3 | Four severities, a closed set | `PR-REP-011` |
| `REVIEW-P-04` | 3 | This document grants no new authority | AEOS-DOCSTD `DS-P-06` |
| `REVIEW-P-05` | 3 | AEOS-DOCSTD §12 governs documents; not restated here | AEOS-DOCSTD §12 |
| `REVIEW-P-06` | 3 | Outcome independent of Runtime or Model | `PR-RUN-008` |
| `REVIEW-P-07` | 3 | Review is distinct from testing, verification, validation | AEOS-VISION §6.9 |
| `REVIEW-P-08` | 3 | A finding cites location and identifier | AEOS-DOCSTD §12.4 |
| `REVIEW-P-09` | 3 | Explanation concerns intent; Review examines a result | AEOS-VISION §6.9 |
| `REVIEW-001` | 5 | Review itself is an Observation-class action | `PR-WFL-006` |
| `REVIEW-002` | 5 | Acting on a finding requires Human Approval | `PR-SAF-005` |
| `REVIEW-003` | 5 | Critical/Major findings reach a human before acceptance | AEOS-VISION §6.9 |
| `REVIEW-004` | 5 | Automation Grants never cover Destructive acceptance | `PR-SAF-012` |
| `REVIEW-005` | 5 | Every finding reaches a disposition | AEOS-DOCSTD §12.2 |
| `REVIEW-006` | 6 | An AI Runtime MAY perform Review | `PR-RUL-005` |
| `REVIEW-007` | 6 | Finding validity is independent of its source | AEOS-VISION `V6` |
| `REVIEW-008` | 6 | AI findings cite identifiers on the same terms | AEOS-DOCSTD §12.4 |
| `REVIEW-009` | 6 | An AI finding is not itself Human Approval | `REVIEW-002` |
| `REVIEW-010` | 6 | AI findings are not presented as observed fact | `PR-SAF-011` |
| `REVIEW-011` | 7 | More than one Runtime or Model MAY review | `PR-RUN-013` |
| `REVIEW-012` | 7 | Findings aggregated by classification, not source | AEOS-VISION `V6` |
| `REVIEW-013` | 7 | Conflicting findings are reported, not silently resolved | AEOS-DOCSTD §11.3 |
| `REVIEW-014` | 7 | Switching reviewers requires no other change | `PR-RUN-005` |
| `REVIEW-015` | 7 | Review support is model-independent | `PR-RUN-006` |
| `REVIEW-016` | 8 | Draft, Review, Revision precede the terminal state | AEOS-DOCSTD §12.2 |
| `REVIEW-017` | 8 | Review/Revision repeat until no Critical/Major remains | AEOS-DOCSTD §12.2 |
| `REVIEW-018` | 8 | Document states are exactly AEOS-DOCSTD §12.1's | AEOS-DOCSTD `DS-P-07` |
| `REVIEW-019` | 8 | Non-document terminal state is Acceptance, not Freeze | AEOS-GLOSSARY *Freeze* |
| `REVIEW-020` | 9 | A checkpoint precedes the accepting Human Approval | AEOS-PRD §10 |
| `REVIEW-021` | 9 | At least one checkpoint precedes Acceptance or Freeze | AEOS-DOCSTD `DS-P-04` |
| `REVIEW-022` | 10 | Non-document findings carry AEOS-DOCSTD §12.3's definitions | AEOS-DOCSTD §12.3 |
| `REVIEW-023` | 11 | Acceptance requires no open Critical/Major and Approval | AEOS-DOCSTD `DS-P-04` |
| `REVIEW-024` | 11 | Acceptance authorizes exactly the reviewed state | AEOS-GLOSSARY *Proposal* |
| `REVIEW-025` | 11 | Minor/Nitpick findings do not block Acceptance | AEOS-DOCSTD §12.3 |
| `REVIEW-026` | 11 | Document Acceptance equals AEOS-DOCSTD Freeze | AEOS-DOCSTD §12.1 |
| `REVIEW-027` | 12 | Document freeze criteria are AEOS-DOCSTD §12's alone | AEOS-DOCSTD `DS-P-07` |
| `REVIEW-028` | 13 | A changed artifact requires a new Review | AEOS-DOCSTD `E7` |
| `REVIEW-029` | 13 | A resolved Revision returns to Review, not directly to acceptance | AEOS-DOCSTD §12.2 |
| `REVIEW-030` | 13 | Re-review scope is the change and open findings | `REVIEW-P-01` |
| `REVIEW-031` | 14 | A completed Review leaves a minimum record | `PR-WFL-015` |
| `REVIEW-032` | 14 | The record identifies human or AI Runtime origin | `PR-SAF-011` |
| `REVIEW-033` | 14 | The record is retained in the repository | `PR-REP-002` |
| `REVIEW-034` | 15 | A finding cites location and identifier | AEOS-DOCSTD §12.4 |
| `REVIEW-035` | 15 | Acceptance/Freeze decisions are traceable to findings | AEOS-DOCSTD `DS-P-01` |

---

**End of Review Guide**

AEOS-REVIEW · Version 1.0.0 · Review Source of Truth for artifact kinds not governed by AEOS-DOCSTD
Section 12
