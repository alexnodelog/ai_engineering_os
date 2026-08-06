# AI Engineering Operating System

## AEOS — Troubleshooting Guide

*The permanent AEOS 1.0 statement of how a failure occurring anywhere in an AEOS-governed
repository is recognized, classified, diagnosed, escalated, and resolved.*

| Field | Value |
| :--- | :--- |
| **Document** | Troubleshooting Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-TRBL |
| **Version** | 1.0.0 |
| **Status** | Frozen |
| **Owner** | Product Owner, AEOS |
| **Author** | Troubleshooting Governance Board, AEOS |
| **Audience** | Contributors, maintainers, reviewers, and AI runtimes operating within an AEOS-governed repository |
| **Suggested path** | `docs/developer/TROUBLESHOOTING.md` |
| **Companion documents** | `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states the methodology by which a failure occurring anywhere in an AEOS-governed
> repository — in the Environment, the repository, a Workflow, a Runtime interaction, a Rule, a
> Skill, a Prompt, a review, or the documentation itself — is recognized, classified, diagnosed,
> escalated, and resolved. It is written as a **Developer Guide**, in the sense AEOS-DOCSTD
> Section 4.3 defines that layer: task-oriented instruction describing how a person, and an AI
> runtime acting under a person's supervision, works within an already-built AEOS repository when
> something has gone wrong. This document's methodology is repository-wide: it applies uniformly
> regardless of which architectural layer, Runtime, Platform, or Distribution Method is involved,
> and regardless of which document ultimately owns the failing subject.
>
> This document is **not an Implementation Guide**: it states no algorithm, no realization
> procedure, and no technology-specific or language-specific debugging technique. It is **not a
> Runtime Specification**: it neither restates nor supersedes the runtime lifecycle
> `AEOS_RUNTIME_FLOW.md` (AEOS-RTF) specifies, including that document's own Failure Handling and
> Recovery Flow phases, which govern the automatic behavior of a runtime request and to which this
> document defers entirely. It is **not an Architecture document**: it names no structural layer,
> subsystem, or component of its own and states no structural decision. It defines no product
> requirement, no vision, no terminology, no technology choice, and no specified behavior; where a
> statement here appears to do any of these, that is a defect in this document and MUST be
> reported rather than acted upon.
>
> AEOS-DOCSTD Section 4.1 positions Developer Guides beneath Implementation Guides in the
> documentation hierarchy. This document is written under AEOS-DOCSTD's general document template
> and the Section 4.3 purpose statement for this layer; it is, to the authoring participants'
> knowledge, the first Developer Guide authored for AEOS.
>
> Where this document and a document of higher authority both speak to a subject, the
> higher-authority document governs and any conflict here is a defect to be reported rather than
> acted upon.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document
> are to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning
> only when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Troubleshooting Philosophy](#3-troubleshooting-philosophy)
4. [Failure Classification](#4-failure-classification)
5. [Diagnostic Principles](#5-diagnostic-principles)
6. [Troubleshooting Lifecycle](#6-troubleshooting-lifecycle)
7. [Root Cause Analysis](#7-root-cause-analysis)
8. [Recovery Strategy](#8-recovery-strategy)
9. [Validation After Recovery](#9-validation-after-recovery)
10. [Escalation Policy](#10-escalation-policy)
11. [Safe Failure Handling](#11-safe-failure-handling)
12. [Human Intervention Points](#12-human-intervention-points)
13. [AI-Assisted Troubleshooting](#13-ai-assisted-troubleshooting)
14. [Logging Expectations](#14-logging-expectations)
15. [Common Failure Categories](#15-common-failure-categories)
16. [Non-Goals](#16-non-goals)
17. [Traceability](#17-traceability)
18. [References](#18-references)
19. [Document Governance](#19-document-governance)
20. [Appendix A — Rule Index](#appendix-a--rule-index)
21. [Appendix B — Troubleshooting Checklist (Non-Normative)](#appendix-b--troubleshooting-checklist-non-normative)

---

## 1. Executive Summary

**Purpose.** A repository governed by AEOS is worked on by people and by AI runtimes who did not
build it and who arrive with no memory of a prior session. When something goes wrong for one of
them, the outcome must not depend on who encountered it, which Runtime was in use, or which
Platform the machine happens to run. This document exists so that a failure is met the same way
every time: recognized against a fixed vocabulary, diagnosed by inspection rather than assumption,
recovered from only with the approval its effect requires, and recorded so the next reader —
human or AI — does not repeat the diagnosis from nothing.

AEOS-PRD already states the posture a failure is met with: the safe path is the default, the
product fails closed, and every consequential action follows one interaction loop of inspection,
explanation, proposal, confirmation, execution, and reporting. This document does not restate that
posture; it applies it to the specific circumstance of something having already gone wrong. Where
AEOS-PRD, AEOS-ARCH, a Specification, or AEOS-RTF already governs a subject this document touches,
this document cites that authority and states nothing in its place.

Four properties bind the methodology this document states, consistent with the product principles
AEOS-PRD Section 7 establishes:

| Property | What it requires of this document |
| :--- | :--- |
| **Runtime-neutral** | No step in this methodology depends on which AI Runtime is in use; switching Runtime requires no change to how a failure is classified, diagnosed, or recovered from, consistent with `PR-RUN-002` and `PR-RUN-003`. |
| **Vendor-neutral** | No vendor, Runtime, Model, or Tool is named as privileged, required, or excluded, consistent with AEOS-VISION invariant `V6`. |
| **Platform-neutral** | Every principle is stated as what occurs, never as an operating-system-specific action, consistent with `PR-PLT-003` and `PR-PLT-005`. |
| **Model-neutral** | No principle depends on which Model a Runtime uses to perform inference, consistent with `PR-RUN-006`. |

This document is, deliberately, a methodology and not a manual: it states what a troubleshooting
effort must establish and in what order, never the specific command, log location, or tool that
establishes it. That concreteness belongs to a future Implementation Guide, once one exists for
this subject, or to the project's own conventions.

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the methodology of troubleshooting within an AEOS-governed repository:

- the operational vocabulary this Guide uses to classify a failure, pending its disposition under
  AEOS-GLOSSARY's own change control;
- the guiding principles a troubleshooting effort applies;
- the principles by which diagnosis is conducted;
- the ordered lifecycle a troubleshooting effort passes through, from detection to resolution;
- the methodology of Root Cause Analysis, without prescribing a technique or tool;
- the principles by which a Recovery is proposed, approved, and executed;
- the expectation that a Recovery is validated before an Incident is considered resolved;
- the policy by which a troubleshooting effort escalates to a human;
- the safe-failure posture a troubleshooting effort maintains throughout;
- the points in the lifecycle at which a human decision is mandatory;
- the boundary within which an AI runtime may assist with troubleshooting;
- the principle-level expectations placed on a troubleshooting record;
- an illustrative, non-exhaustive taxonomy of common failure categories and the document that
  owns correction of each.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| The general form, language, and lifecycle of AEOS documentation | AEOS-DOCSTD |
| Which technologies AEOS officially recognizes | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH |
| How the structure is arranged to be built | AEOS-BLUEPRINT |
| Precise, testable behavioral rules and specific error conditions for a defined behavior | The Specification document that owns that behavior, under AEOS-SPECSTD |
| The runtime lifecycle's own Failure Handling and Recovery Flow phases, and the general state model they compose | `AEOS_RUNTIME_FLOW.md` (AEOS-RTF), and the Specification documents it composes |
| How any recovery mechanism, log format, or diagnostic tool is realized | Implementation Guides, once authored, or the project's own conventions |
| Language-specific or tool-specific debugging technique | Not governed by any AEOS document at present |

A statement in this document that grants a capability, imposes a product requirement, defines a
term, states an architectural decision, or restates a Specification's or AEOS-RTF's behavior is a
**defect in this document**. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to every contributor — human or AI runtime — working within an
AEOS-governed repository, at every stage of the Engineering Lifecycle AEOS-PRD Section 9 states,
regardless of Runtime, Platform, or Distribution Method. It applies identically whether the
failure is encountered during environment preparation, project work, a Workflow, a review, or the
maintenance of an already-built project.

### 2.4 Recorded Deviation

AEOS-DOCSTD Section 7.3 states that the Developer Guide layer SHOULD NOT use normative keywords,
on the ground that a guide instructs and obligations belong to the documents that own them. This
document uses normative keywords for its own internal methodology: the rules by which a
troubleshooting effort classifies, diagnoses, escalates, and recovers. Removing the keywords would
leave that methodology's own consistency unenforceable, which the uniform application AEOS-PRD
`PR-NFR-005` (Resilience) and Section 20.2's Failure Posture depend upon requires. Every normative
statement in this document either applies an obligation a `PR-` identifier or AEOS-RTF already
imposes, cited at the point of use, or states a procedural rule of this document's own
methodology; none creates a new product requirement. The deviation is deliberate and is recorded
here as AEOS-DOCSTD Section 7.2 requires of a deliberate deviation from a SHOULD-level rule.

---

## 3. Troubleshooting Philosophy

These principles constrain every section that follows. A rule elsewhere in this document that
conflicts with one of these principles is a defect in that rule.

| # | Principle |
| :--- | :--- |
| `TRBL-P-01` | Troubleshooting begins with inspection, never with assumption. |
| `TRBL-P-02` | The safe path is the default at every step. |
| `TRBL-P-03` | A failure is reported with the same detail as a success. |
| `TRBL-P-04` | A decline is not a failure. |
| `TRBL-P-05` | The human decides how a failure is resolved. |
| `TRBL-P-06` | Nothing consequential happens without being understood first. |
| `TRBL-P-07` | Troubleshooting is vendor-, runtime-, model-, and platform-neutral. |
| `TRBL-P-08` | This Guide describes; it never decides. |

### `TRBL-P-01` — Troubleshooting Begins With Inspection, Never With Assumption

A troubleshooting effort determines the actual state of the Environment, the repository, and the
failing subject before forming any conclusion about cause, consistent with AEOS-PRD Section 11's
statement that AEOS inspects before it acts, and with `PR-ENV-001`.

### `TRBL-P-02` — The Safe Path Is the Default at Every Step

Every step of this methodology defaults to the option that risks least, consistent with
AEOS-VISION invariant `V7` and `PR-SAF-001`.

### `TRBL-P-03` — A Failure Is Reported With the Same Detail as a Success

Silent failure handling is itself a defect in a troubleshooting effort, consistent with
`PR-WFL-011` and AEOS-PRD Section 20.2's fail-visibly posture.

### `TRBL-P-04` — A Decline Is Not a Failure

A Proposal a human declines under AEOS-PRD Section 10's Confirm obligation is a normal outcome,
not a Failure under this document's classification, consistent with `PR-WFL-009` and with
AEOS-RTF `RTF-045`, which records failure and decline as distinct conditions of a runtime request.

### `TRBL-P-05` — The Human Decides How a Failure Is Resolved

Diagnosis may be assisted by an AI runtime; the decision to accept a proposed Recovery remains the
human's, consistent with AEOS-VISION invariant `V2` and AEOS-PRD Section 20.1's statement that the
user's judgment is final.

### `TRBL-P-06` — Nothing Consequential Happens Without Being Understood First

A Recovery is proposed only once its effect, its reversibility, and the consequence of declining
it are stated, consistent with AEOS-VISION invariant `V3` and AEOS-PRD Section 10's Explain and
Propose obligations.

### `TRBL-P-07` — Troubleshooting Is Vendor-, Runtime-, Model-, and Platform-Neutral

No principle or rule in this document depends on a specific vendor, Runtime, Model, or Platform,
consistent with the four properties stated in [Section 1](#1-executive-summary).

### `TRBL-P-08` — This Guide Describes; It Never Decides

Consistent with the Developer Guide boundary AEOS-DOCSTD Section 4.3 states, this document
describes an already-established practice; it grants no capability and creates no product
requirement. Where this document and a governing document disagree, the governing document is
correct and this document is defective at that point.

---

## 4. Failure Classification

> **Non-normative note on terminology.** AEOS-GLOSSARY is the sole authority for AEOS terminology,
> per that document's `T1` through `T5` and AEOS-DOCSTD `DS-P-07`. The seven words below are not
> AEOS-GLOSSARY entries. This document uses them only in the operational sense stated here, scoped
> to troubleshooting classification, consistent with AEOS-GLOSSARY Section 6.6 rule `W4`: where a
> document needs a term the Glossary does not define, it either proposes the addition or rephrases
> using defined terms. This section is that proposal, recorded for the owner's consideration under
> AEOS-GLOSSARY Section 9.2; it carries no authority beyond this document until the owner adopts it.

### 4.1 The Seven Distinctions

| Term | Operational distinction, as used in this Guide only |
| :--- | :--- |
| **Error** | An anomalous condition reported at the moment it occurs — by a tool, a Runtime, a check, or an observation — prior to any determination of cause or consequence. |
| **Failure** | The state a step or an action reaches when it does not produce its intended, specified outcome, because of one or more Errors. Where the step occurs within a runtime lifecycle request, this coincides with the terminal condition AEOS-RTF's Failure Handling phase reaches (`RTF-043` through `RTF-045`), and this Guide's usage is consistent with, and does not extend, that condition. |
| **Bug** | A defect in a Repository Asset — a Rule, Skill, Prompt, Workflow, Specification, or piece of code — identified, through Root Cause Analysis, as the underlying cause of one or more Errors or Failures. |
| **Incident** | An occurrence, bounded in time, in which one or more Failures affect a project's ability to proceed, tracked as a single unit through the lifecycle in [Section 6](#6-troubleshooting-lifecycle) regardless of how many Errors compose it. |
| **Recovery** | The corrective action taken, after diagnosis, to return the project to a consistent, describable state and, where possible, to the outcome originally intended. |
| **Workaround** | A Recovery that avoids the immediate impact of a Failure without addressing its Root Cause. |
| **Resolution** | The recorded outcome that a Root Cause has been addressed — through a fix, a Workaround formally accepted as durable, or a decision not to fix — such that an Incident is closed. |

### 4.2 Classification Rules

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-001` | A contributor or AI runtime MUST use Error, Failure, Bug, Incident, Recovery, Workaround, and Resolution only in the sense Section 4.1 states, until the owner adopts some or all of them into AEOS-GLOSSARY under that document's own change control. | AEOS-GLOSSARY `W4`, `T5` |
| `TRBL-002` | A Decline, as AEOS-PRD Section 10 uses the term, MUST NOT be recorded as a Failure or as an Incident under this classification. | `PR-WFL-009` · AEOS-RTF `RTF-045` |
| `TRBL-003` | A Workaround MUST be reported as a Workaround, never as a Resolution, for as long as its underlying Root Cause remains unaddressed. | AEOS-PRD Section 20.2 |

---

## 5. Diagnostic Principles

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-004` | Diagnosis MUST begin with inspection of the actual state of the Environment, the repository, and the failing subject, never with an assumed state. | `PR-ENV-001` · `PR-ENV-003` |
| `TRBL-005` | Diagnosis MUST distinguish an observed fact from an inference, and MUST state which is which wherever it is reported. | `PR-ENV-003` · `PR-SAF-011` · AEOS-VISION `V3` |
| `TRBL-006` | Where the actual state cannot be determined, diagnosis MUST report that uncertainty explicitly rather than proceed on an assumption. | `PR-ENV-010` · AEOS-VISION `V7` |
| `TRBL-007` | A Failure MUST be reproduced or otherwise substantiated before it is classified as a Bug rather than an isolated, unconfirmed Error. | AEOS-PRD Section 20.2 |
| `TRBL-008` | Diagnosis MUST consider the Environment, the repository's recorded state, and the specific Repository Asset or Runtime interaction involved, in that order, before a cause is concluded. | `PR-ENV-005` · AEOS-PRD Section 11 |
| `TRBL-009` | A diagnostic conclusion MUST NOT be presented as an observed fact when it is an inference. | `PR-SAF-011` |

This document states no algorithm, heuristic, or tool by which diagnosis is performed; the
technique used belongs to a future Implementation Guide or to contributor and Runtime practice,
consistent with [Section 16](#16-non-goals).

---

## 6. Troubleshooting Lifecycle

Every Incident passes through the same ordered stages. The order is fixed; a stage is not skipped
because the outcome of a later stage seems predictable.

```text
    DETECT -> CLASSIFY -> DIAGNOSE -> PROPOSE RECOVERY -> CONFIRM
                                                              |
                                              declined --------+--------+
                                                              |         |
                                                           approved     |
                                                              |         |
                                                            RECOVER     |
                                                              |         |
                                                           VALIDATE     |
                                                              |         |
                                                REPORT AND RECORD <-----+
```

| Stage | What happens | Exit condition | Governed by |
| :--- | :--- | :--- | :--- |
| **Detect** | An Error or an unexpected outcome is observed. | The observation is recorded and named. | [Section 4](#4-failure-classification) |
| **Classify** | The observation is classified under [Section 4](#4-failure-classification) and, where applicable, [Section 15](#15-common-failure-categories). | A classification is recorded. | Sections 4, 15 |
| **Diagnose** | Root Cause Analysis is performed under [Section 5](#5-diagnostic-principles) and [Section 7](#7-root-cause-analysis). | A Root Cause is identified, or its absence is explicitly reported. | Sections 5, 7 |
| **Propose Recovery** | A Recovery is proposed, stating its rationale, effect, reversibility, and the consequence of declining, consistent with AEOS-PRD Section 10. | A proposal is stated in full. | Section 8 |
| **Confirm** | The proposal receives the approval its Action Class requires, per AEOS-PRD Section 10.1. | Approval or decline is recorded. | Sections 8, 10, 12 |
| **Recover** | The approved action is executed exactly as approved. | The action completes, partially completes, or itself fails. | Section 8 |
| **Validate** | The outcome is checked against the intended result. | Validation passes, fails, or is inconclusive. | Section 9 |
| **Report and Record** | What occurred is reported with the same detail as a success and durably recorded. | A Resolution, or a continuing Incident, is recorded. | Sections 9, 14 |

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-010` | A contributor or AI runtime MUST NOT proceed to Propose Recovery before Classify and Diagnose have each reached their exit condition, except where the immediate action is itself the safe default stated in [Section 11](#11-safe-failure-handling). | AEOS-VISION `V7` |
| `TRBL-011` | Report and Record MUST occur regardless of whether the Incident reaches Resolution, is escalated, or remains open. | `PR-WFL-011` · AEOS-PRD Section 20.2 |
| `TRBL-012` | A decline at Confirm MUST proceed directly to Report and Record without a Recover attempt. | `PR-WFL-009` |

---

## 7. Root Cause Analysis

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-013` | Root Cause Analysis MUST identify the Repository Asset, Environment condition, or external factor that, if corrected, would prevent recurrence — not merely the symptom observed at Detect. | AEOS-PRD Section 20.2 |
| `TRBL-014` | Root Cause Analysis MUST distinguish a single Root Cause from a contributing factor where more than one condition is present, and MUST record both. | `PR-DOC-010` |
| `TRBL-015` | Where a Root Cause cannot be determined, Root Cause Analysis MUST report that fact explicitly rather than record a plausible but unconfirmed cause as though it were established. | `PR-SAF-011` · AEOS-VISION `V7` |
| `TRBL-016` | A confirmed Root Cause MUST be attributed to the document or Repository Asset that owns the failing subject, so that correction proceeds under that owner's own change control. | AEOS-DOCSTD `DS-P-06` |

This document states no algorithm, heuristic, or tool for performing Root Cause Analysis. The
technique used — bisection, log inspection, comparison against a Specification, or any other — is
a matter of contributor and Runtime practice, or of a future Implementation Guide, and is a
recorded non-goal of this document per [Section 16](#16-non-goals).

---

## 8. Recovery Strategy

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-017` | A proposed Recovery MUST state its rationale, its effect, its reversibility, and the consequence of declining it. | AEOS-PRD Section 10 |
| `TRBL-018` | A proposed Recovery MUST be classified by Action Class, per AEOS-PRD Section 10.1, and MUST receive the approval that class requires before execution. | `PR-WFL-006` |
| `TRBL-019` | A Recovery classified Destructive MUST receive explicit, specific confirmation of that exact action and MUST NOT be covered by a general approval. | `PR-SAF-003` |
| `TRBL-020` | Where more than one Recovery option addresses the confirmed Root Cause, the option offered SHOULD be the most reversible, and reversibility MUST be stated for each option offered. | `PR-SAF-004` |
| `TRBL-021` | A Recovery that only avoids the immediate impact of a Failure without addressing its confirmed Root Cause MUST be reported as a Workaround, not as a Resolution. | Section 4.1 |
| `TRBL-022` | Recovery MUST execute exactly what was approved; a scope expansion discovered during Recovery MUST be proposed anew rather than performed under the original approval. | `PR-SAF-005` |
| `TRBL-023` | Recovery MUST NOT automatically retry a failed Runtime invocation in a way that incurs unapproved cost. | `PR-RUN-011` |

---

## 9. Validation After Recovery

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-024` | A Recovery MUST be validated against the outcome its proposal stated before the Incident is recorded as reaching Resolution. | AEOS-PRD Section 20.2 |
| `TRBL-025` | Where the project is governed by the TDD Workflow and the Recovery touches code, validation MUST include the project's own test tooling. | `PR-TDD-009` |
| `TRBL-026` | Validation that fails MUST return the Incident to Diagnose rather than be recorded as a Resolution. | Section 6 |
| `TRBL-027` | A partially successful Recovery MUST be reported with the same detail as a fully successful one, stating exactly what was validated and what was not. | `PR-WFL-011` · AEOS-PRD Section 20.2 |

---

## 10. Escalation Policy

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-028` | Diagnosis or Recovery that would exceed the approval already held, that remains inconclusive after reasonable inspection, or that falls in the Destructive Action Class MUST escalate to a Human Intervention Point rather than proceed on inference. | `PR-SAF-002` · Section 12 |
| `TRBL-029` | Escalation MUST state what was found, what was attempted, and what decision is being asked of the human. | AEOS-PRD Section 10 |
| `TRBL-030` | A decline at escalation halts the Incident's progression without penalty and without a further attempt at the same Recovery. | `PR-WFL-009` · AEOS-PRD Section 20.2 |
| `TRBL-031` | Where an Incident cannot be escalated to a human because none is presently available, and the Incident's Recovery falls in the Local Change, External Effect, or Destructive Action Class, the Incident MUST fail closed and MUST NOT be resolved by an AI runtime acting alone. | AEOS-VISION `V2` · `PR-SAF-002` |

---

## 11. Safe Failure Handling

AEOS-PRD Section 20.2 states the product's Failure Posture in five parts. This section applies
each part to a troubleshooting effort specifically; it restates none of the posture's product-level
meaning.

| # | Rule | Applies |
| :--- | :--- | :--- |
| `TRBL-032` | **Fail closed.** An Incident with unresolved uncertainty MUST stop rather than proceed toward a Recovery attempt. | `PR-SAF-002` |
| `TRBL-033` | **Fail visibly.** An Incident's state MUST be reported with the same detail as an unaffected operation; a troubleshooting effort that resolves silently is itself defective. | `PR-WFL-011` |
| `TRBL-034` | **Fail consistently.** An interrupted troubleshooting effort MUST leave a state that is describable. | `PR-SAF-010` |
| `TRBL-035` | **Fail without cost.** Diagnosis and Recovery MUST NOT incur unapproved Runtime cost, including an automatic retry. | `PR-RUN-011` |
| `TRBL-036` | **Fail without blame.** Declining a proposed Recovery is a normal outcome and MUST NOT be treated, recorded, or reported as an error condition attributable to the contributor or the AI runtime that proposed it. | `PR-WFL-009` · AEOS-PRD Section 20.2 |

---

## 12. Human Intervention Points

A human decision is mandatory at each of the following points in the lifecycle stated in
[Section 6](#6-troubleshooting-lifecycle).

| Point | Why the decision is mandatory | Traces to |
| :--- | :--- | :--- |
| **Confirm, for every proposed Recovery** | Every consequential action follows the Confirm obligation of AEOS-PRD Section 10. | AEOS-PRD Section 10 |
| **Any Recovery classified Destructive** | Destructive actions require explicit, specific confirmation never covered by a general approval or an Automation Grant. | `PR-SAF-003` · `PR-SAF-012` |
| **Escalation under `TRBL-028`** | Diagnosis or Recovery has exceeded what standing approval or available certainty covers. | Section 10 |
| **A Root Cause Analysis that concludes no Root Cause** | Whether to continue investigating or accept the Incident as unresolved is a judgment the product reserves to the human. | AEOS-PRD Section 20.1 |
| **A Recovery that would modify version-control history or discard uncommitted work** | Both require explicit, specific confirmation of that exact operation. | `PR-REP-005` · `PR-REP-006` |

---

## 13. AI-Assisted Troubleshooting

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-037` | An AI runtime assisting with troubleshooting is bound by the same Interaction Model, Action Class, and approval obligations as any other consequential AEOS action; troubleshooting confers no exception. | AEOS-PRD Section 10 |
| `TRBL-038` | An AI runtime MUST distinguish an observed fact from an inference throughout Diagnose and Root Cause Analysis, and MUST NOT present an inference as an observed fact. | `PR-SAF-011` |
| `TRBL-039` | An Automation Grant MUST NOT be treated as authorizing a Recovery classified Destructive. | `PR-SAF-012` |
| `TRBL-040` | Selection among available Runtimes for a troubleshooting task follows the same user-selection and non-substitution rule as any other Runtime invocation. | `PR-RUN-004` |
| `TRBL-041` | An AI runtime MUST NOT place a credential, secret, or Runtime State into a troubleshooting record. | `PR-SAF-006` · `PR-REP-013` |

Nothing in this section restates or narrows AEOS-RTF's specification of Failure Handling or
Recovery Flow. Where a Failure occurs within a runtime lifecycle request, the request's own
progression through those phases is governed by AEOS-RTF; this section governs only the
contributor-facing and AI-runtime-facing conduct of troubleshooting that surrounds it.

---

## 14. Logging Expectations

| # | Rule | Traces to |
| :--- | :--- | :--- |
| `TRBL-042` | A record of an Incident — its classification, its diagnosis, the Recovery proposed and its approval status, and its validation outcome — MUST be durable, human-readable, and consumable by an AI runtime without transformation. | AEOS-PRD Section 13.2 |
| `TRBL-043` | A troubleshooting record MUST NOT contain a credential or a secret. | `PR-SAF-006` · `PR-REP-013` |
| `TRBL-044` | Where an Incident's record would duplicate a Repository Asset's own content, the record MUST reference the asset rather than restate it. | AEOS-DOCSTD `DS-P-07` |

This document does not prescribe a log format, storage location, or tooling for a troubleshooting
record; that choice belongs to a future Implementation Guide or to the project's own conventions,
consistent with `PR-DOC-009` and recorded as a non-goal in [Section 16](#16-non-goals).

---

## 15. Common Failure Categories

The table below is **illustrative and non-exhaustive**. A category's absence from this table is
not a statement that the category cannot occur, and its presence confers no priority over another.
Each category names the document under whose change control the underlying condition is corrected;
this document corrects nothing itself.

| Category | Illustrative examples | Corrected under |
| :--- | :--- | :--- |
| **Environment** | A missing or misconfigured component; an unrecognized component; drifted Environment state. | `PR-ENV`, and the project's own recorded Environment findings |
| **Repository** | Uncommitted work at risk; divergent version-control history; a repository change outside AEOS's knowledge. | `PR-REP` |
| **Runtime interaction** | A reported Runtime error; a Runtime found Unavailable; a Runtime that cannot support a requested Engineering Capability. | `PR-RUN`, `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG), AEOS-RTF |
| **Workflow** | A failed or declined step; a step whose dependency did not complete. | `PR-WFL`, AEOS-RTF |
| **Repository Asset defect** | A Rule, Skill, Prompt, Workflow, or Specification that does not behave as declared. | The asset's own owning document, per AEOS-DOCSTD `DS-P-06` |
| **Documentation drift** | A document that no longer matches the repository's actual state. | `PR-DOC` |
| **Review finding** | A Critical or Major finding blocking freeze or acceptance. | AEOS-DOCSTD Section 12.3, `PR-REP-011` |
| **Distribution and onboarding** | An interrupted or failed installation; a runtime connection that cannot be established. | `PR-DST`, `PR-ONB` |

---

## 16. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document,
in the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Defining, restating, or superseding AEOS-RTF's Failure Handling or Recovery Flow phases. | `AEOS_RUNTIME_FLOW.md` (AEOS-RTF) |
| `NG-2` | Defining precise, testable error conditions for any specific behavior. | The Specification document that owns that behavior, under AEOS-SPECSTD |
| `NG-3` | Prescribing a specific tool, command, log format, or storage mechanism for a troubleshooting record. | A future Implementation Guide, or the project's own conventions |
| `NG-4` | Providing language-specific or platform-specific debugging technique. | A future Implementation Guide, or the project's own tooling documentation |
| `NG-5` | Canonically defining Error, Failure, Bug, Incident, Recovery, Workaround, or Resolution as AEOS-GLOSSARY terms. | AEOS-GLOSSARY, under its own change control, per [Section 4](#4-failure-classification) |
| `NG-6` | Establishing a new documentation-hierarchy layer, or altering the Developer Guide layer's boundary. | AEOS-DOCSTD, under `H5` |
| `NG-7` | Authorizing automated Recovery from a Destructive-classified Failure without human confirmation. | Not permitted under `PR-SAF-003` and `PR-SAF-012` by any document |
| `NG-8` | Vendor-, Runtime-, Model-, or Platform-specific troubleshooting instruction. | Excluded by `TRBL-P-07` and out of this document's scope entirely |
| `NG-9` | Correcting the underlying cause of any specific Incident. | The document or Repository Asset [Section 15](#15-common-failure-categories) names for the relevant category |

---

## 17. Traceability

Every `TRBL-` rule in this document traces to one or more `PR-` identifiers, to AEOS-RTF, or to
AEOS-DOCSTD, stated inline at the point of each rule and indexed in full in
[Appendix A](#appendix-a--rule-index). The table below summarizes trace density by prefix,
consistent with the practice AEOS-SPECSTD `CM2` establishes for Specification documents and which
`AEOS_BOOTSTRAP.md`-adjacent Implementation Guides have adopted voluntarily for themselves, under
AEOS-DOCSTD Section 2.3's general discipline.

| Prefix | Rules in this document that trace to it |
| :--- | :--- |
| `PR-ENV` | `TRBL-004` · `TRBL-005` · `TRBL-006` · `TRBL-008` |
| `PR-WFL` | `TRBL-002` · `TRBL-011` · `TRBL-012` · `TRBL-018` · `TRBL-027` · `TRBL-030` · `TRBL-033` · `TRBL-036` |
| `PR-RUN` | `TRBL-023` · `TRBL-035` · `TRBL-040` |
| `PR-TDD` | `TRBL-025` |
| `PR-DOC` | `TRBL-014` |
| `PR-REP` | `TRBL-041` · `TRBL-043` |
| `PR-SAF` | `TRBL-005` · `TRBL-009` · `TRBL-015` · `TRBL-019` · `TRBL-020` · `TRBL-022` · `TRBL-028` · `TRBL-031` · `TRBL-032` · `TRBL-034` · `TRBL-038` · `TRBL-039` · `TRBL-041` · `TRBL-043` |

`PR-ONB`, `PR-PLT`, and `PR-NFR` are cited in prose — [Section 1](#1-executive-summary) and
[Section 15](#15-common-failure-categories) — but no `TRBL-<NNN>` rule traces to any of them
directly; they are omitted from the table above on that basis. `PR-REP-005` and `PR-REP-006` are
cited in [Section 12](#12-human-intervention-points) against a Human Intervention Point rather than
a `TRBL-<NNN>` rule, and are likewise omitted from the `PR-REP` row above, which lists only rules
that carry the identifier directly.

Fourteen rules — `TRBL-001`, `TRBL-003`, `TRBL-007`, `TRBL-010`, `TRBL-013`, `TRBL-016`,
`TRBL-017`, `TRBL-021`, `TRBL-024`, `TRBL-026`, `TRBL-029`, `TRBL-037`, `TRBL-042`, and `TRBL-044`
— trace to AEOS-VISION, AEOS-PRD by section rather than by `PR-` identifier, AEOS-GLOSSARY,
AEOS-DOCSTD, or this document's own Section 4 or Section 6, consistent with the section-reference
form AEOS-DOCSTD Section 11.2 recognizes. Each states an obligation a named section, invariant, or
this document's own methodology already imposes, rather than a product requirement stated only
here. This document, as a whole, traces to AEOS-DOCSTD `H4`'s requirement that every derivative
document trace to the layer above it, satisfied by tracing each rule to the `PR-` requirement,
AEOS-PRD section, AEOS-VISION invariant, or AEOS-RTF behavior it ultimately applies, and to
AEOS-DOCSTD `H6`'s requirement that a document belong to a layer, satisfied by this document's
position as a Developer Guide, stated in the authority statement at the head of this document.

---

## 18. References

| Document | Document ID | Cited for |
| :--- | :--- | :--- |
| `AEOS_VISION.md` | AEOS-VISION | Invariants `V2`, `V3`, `V6`, and `V7`, cited throughout [Section 3](#3-troubleshooting-philosophy) and [Section 11](#11-safe-failure-handling). |
| `AEOS_PRODUCT_REQUIREMENTS.md` | AEOS-PRD | The `PR-ENV`, `PR-WFL`, `PR-RUN`, `PR-TDD`, `PR-DOC`, `PR-REP`, `PR-SAF`, `PR-ONB`, and `PR-NFR` identifiers this document's rules trace to, indexed in [Section 17](#17-traceability); the Interaction Model, Action Classes, and Failure Posture this document applies without restating. |
| `AEOS_GLOSSARY.md` | AEOS-GLOSSARY | Terminology used without redefinition throughout this document, and the proposal mechanism [Section 4](#4-failure-classification) follows. |
| `AEOS_DOCUMENT_STANDARD.md` | AEOS-DOCSTD | The documentation hierarchy, the Developer Guide layer's purpose (Section 4.3), the general document template, and the hierarchy and evolution rules (`DS-P-06`, `DS-P-07`, `H4`, `H5`, `H6`) this document is written under. |
| `AEOS_SUPPORTED_TECHNOLOGIES.md` | AEOS-TECH | Referenced for completeness; this document names no technology choice of its own. |
| `AEOS_ARCHITECTURE.md` | AEOS-ARCH | Referenced for completeness; this document states no structural decision of its own. |
| `AEOS_BLUEPRINT.md` | AEOS-BLUEPRINT | Referenced for completeness; this document states no arrangement of its own. |
| `AEOS_SPECIFICATION_STANDARD.md` | AEOS-SPECSTD | The boundary between this document and any Specification document, cited in [Section 2.2](#22-what-this-document-does-not-govern). |
| `AEOS_RUNTIME_FLOW.md` | AEOS-RTF | The Failure Handling and Recovery Flow phases this document defers to entirely, cited in the authority statement and in [Sections 4](#4-failure-classification), [13](#13-ai-assisted-troubleshooting), and [16](#16-non-goals). |
| `RUNTIME_REGISTRY.md` | AEOS-RUNTIME-REG | The Unavailable lifecycle state referenced in [Section 15](#15-common-failure-categories), cited and not restated. |

---

## 19. Document Governance

### 19.1 Status

This document is a **Draft**. It is, to the authoring participants' knowledge, the first Developer
Guide authored for AEOS, and is intended to become the Troubleshooting Source of Truth once the
owner's review under [Section 19.4](#194-review-policy) records no Critical or Major finding, at
which point it is intended to be placed at `docs/developer/TROUBLESHOOTING.md` and frozen alongside
the rest of the AEOS 1.0 document set.

### 19.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `TRBL-<NNN>` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Any change to what an existing `TRBL-<NNN>` rule requires, or its retirement | Explicit owner revision request | Major |
| Addition, removal, or redefinition of an operational term in [Section 4.1](#41-the-seven-distinctions) | Explicit owner revision request | Major |
| Change to a principle in [Section 3](#3-troubleshooting-philosophy) | Explicit owner revision request | Major |
| Addition or removal of a Non-Goal in [Section 16](#16-non-goals) | Owner approval | Minor |

### 19.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-RTF, or
AEOS-DOCSTD are recorded as recommendations for the owning document's governance and are applied
only after explicit owner approval there — never enacted here. The proposal recorded in
[Section 4](#4-failure-classification) becomes authoritative terminology only if and when
AEOS-GLOSSARY adopts it under that document's own change control.

### 19.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no
Critical or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally
confirms, before recommending freeze:

- [ ] Every rule in [Sections 4](#4-failure-classification) through
      [14](#14-logging-expectations) carries a `TRBL-<NNN>` or `TRBL-P-<NN>` identifier and a
      trace.
- [ ] No rule restates AEOS-RTF's Failure Handling or Recovery Flow specification.
- [ ] No rule states a runtime behavior, an architectural decision, a product requirement, or a
      specific error condition.
- [ ] [Section 4](#4-failure-classification) is explicitly marked non-canonical pending
      AEOS-GLOSSARY adoption.
- [ ] [Section 15](#15-common-failure-categories) is explicitly marked illustrative and
      non-exhaustive.
- [ ] All twenty-one entries in this document's Table of Contents are present, in order, and none
      is silently empty.
- [ ] No Critical or Major finding remains open.

### 19.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, or AEOS-BLUEPRINT | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-RTF on the behavior of a runtime lifecycle request | AEOS-RTF governs. This document is corrected to reference it rather than restate it. |
| This document conflicts with a Specification document on the precise behavior or error conditions of that document's own domain | The owning Specification governs. |
| A future Implementation Guide states a diagnostic or recovery mechanism that conflicts with a principle in [Section 3](#3-troubleshooting-philosophy) | The apparent need is reported against this document. It is not resolved by a contradictory statement in the other guide. |
| A term in [Section 4.1](#41-the-seven-distinctions) is adopted by AEOS-GLOSSARY with a different meaning | AEOS-GLOSSARY governs from the moment of adoption. This document is corrected to reference it rather than restate it. |

### 19.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Troubleshooting Guide. States eight troubleshooting principles; seven operational failure-classification terms proposed to AEOS-GLOSSARY and not asserted as canonical; six diagnostic principles; an eight-stage troubleshooting lifecycle; Root Cause Analysis, Recovery Strategy, and Validation-After-Recovery rules; an escalation policy; the product's five-part Failure Posture applied to troubleshooting; five mandatory human intervention points; five AI-assisted-troubleshooting boundary rules; three logging-expectation rules; an eight-category, explicitly non-exhaustive illustration of common failure categories; nine non-goals; and forty-four `TRBL-<NNN>` rules in total, indexed in Appendix A. References AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-RTF, and AEOS-RUNTIME-REG. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no runtime procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, or AEOS-RTF. |

---

## Appendix A — Rule Index

**This appendix is non-normative.** It indexes the identifiers this document's body states
normatively; where the two disagree, the body governs.

| Rule | Section | One-line summary | Traces to |
| :--- | :--- | :--- | :--- |
| `TRBL-P-01` | 3 | Inspection before assumption | `PR-ENV-001` |
| `TRBL-P-02` | 3 | Safe path is the default | AEOS-VISION `V7` · `PR-SAF-001` |
| `TRBL-P-03` | 3 | Failure reported like success | `PR-WFL-011` |
| `TRBL-P-04` | 3 | Decline is not failure | `PR-WFL-009` · AEOS-RTF `RTF-045` |
| `TRBL-P-05` | 3 | Human decides resolution | AEOS-VISION `V2` |
| `TRBL-P-06` | 3 | Understood before consequential | AEOS-VISION `V3` |
| `TRBL-P-07` | 3 | Neutral across vendor, runtime, model, platform | Section 1 |
| `TRBL-P-08` | 3 | This Guide describes, never decides | AEOS-DOCSTD Section 4.3 |
| `TRBL-001` | 4.2 | Seven terms used operationally only | AEOS-GLOSSARY `W4` |
| `TRBL-002` | 4.2 | Decline not recorded as Failure or Incident | `PR-WFL-009` · AEOS-RTF `RTF-045` |
| `TRBL-003` | 4.2 | Workaround never reported as Resolution | AEOS-PRD Section 20.2 |
| `TRBL-004` | 5 | Diagnosis begins with actual state | `PR-ENV-001` · `PR-ENV-003` |
| `TRBL-005` | 5 | Fact distinguished from inference | `PR-ENV-003` · `PR-SAF-011` |
| `TRBL-006` | 5 | Undeterminable state reported explicitly | `PR-ENV-010` |
| `TRBL-007` | 5 | Failure substantiated before Bug classification | AEOS-PRD Section 20.2 |
| `TRBL-008` | 5 | Diagnosis order: Environment, repository, subject | `PR-ENV-005` · AEOS-PRD Section 11 |
| `TRBL-009` | 5 | Inference never presented as fact | `PR-SAF-011` |
| `TRBL-010` | 6 | Classify and Diagnose precede Propose Recovery | AEOS-VISION `V7` |
| `TRBL-011` | 6 | Report and Record always occurs | `PR-WFL-011` |
| `TRBL-012` | 6 | Decline proceeds directly to Report and Record | `PR-WFL-009` |
| `TRBL-013` | 7 | Root Cause identifies the correctable condition | AEOS-PRD Section 20.2 |
| `TRBL-014` | 7 | Root Cause distinguished from contributing factor | `PR-DOC-010` |
| `TRBL-015` | 7 | Undetermined Root Cause reported explicitly | `PR-SAF-011` |
| `TRBL-016` | 7 | Root Cause attributed to its owning document | AEOS-DOCSTD `DS-P-06` |
| `TRBL-017` | 8 | Recovery states rationale, effect, reversibility | AEOS-PRD Section 10 |
| `TRBL-018` | 8 | Recovery classified by Action Class before execution | `PR-WFL-006` |
| `TRBL-019` | 8 | Destructive Recovery requires specific confirmation | `PR-SAF-003` |
| `TRBL-020` | 8 | Most reversible option preferred and stated | `PR-SAF-004` |
| `TRBL-021` | 8 | Root-cause-blind Recovery reported as Workaround | Section 4.1 |
| `TRBL-022` | 8 | Recovery executes exactly what was approved | `PR-SAF-005` |
| `TRBL-023` | 8 | No automatic retry at unapproved cost | `PR-RUN-011` |
| `TRBL-024` | 9 | Recovery validated against stated outcome | AEOS-PRD Section 20.2 |
| `TRBL-025` | 9 | TDD-governed code Recovery validated by project tests | `PR-TDD-009` |
| `TRBL-026` | 9 | Failed validation returns to Diagnose | Section 6 |
| `TRBL-027` | 9 | Partial success reported with full detail | `PR-WFL-011` |
| `TRBL-028` | 10 | Exceeded approval or inconclusive diagnosis escalates | `PR-SAF-002` |
| `TRBL-029` | 10 | Escalation states finding, attempt, and ask | AEOS-PRD Section 10 |
| `TRBL-030` | 10 | Decline at escalation halts without penalty | `PR-WFL-009` |
| `TRBL-031` | 10 | No human available: fail closed, no solo AI resolution | AEOS-VISION `V2` |
| `TRBL-032` | 11 | Fail closed | `PR-SAF-002` |
| `TRBL-033` | 11 | Fail visibly | `PR-WFL-011` |
| `TRBL-034` | 11 | Fail consistently | `PR-SAF-010` |
| `TRBL-035` | 11 | Fail without cost | `PR-RUN-011` |
| `TRBL-036` | 11 | Fail without blame | `PR-WFL-009` · AEOS-PRD Section 20.2 |
| `TRBL-037` | 13 | AI runtime bound by the same obligations | AEOS-PRD Section 10 |
| `TRBL-038` | 13 | AI runtime never presents inference as fact | `PR-SAF-011` |
| `TRBL-039` | 13 | Automation Grant never authorizes Destructive Recovery | `PR-SAF-012` |
| `TRBL-040` | 13 | Runtime selection follows user-selection rule | `PR-RUN-004` |
| `TRBL-041` | 13 | No credential or Runtime State in a record | `PR-SAF-006` · `PR-REP-013` |
| `TRBL-042` | 14 | Incident record durable, human-readable, AI-consumable | AEOS-PRD Section 13.2 |
| `TRBL-043` | 14 | No credential or secret in a record | `PR-SAF-006` · `PR-REP-013` |
| `TRBL-044` | 14 | Record references an asset rather than restating it | AEOS-DOCSTD `DS-P-07` |

---

## Appendix B — Troubleshooting Checklist (Non-Normative)

A practical restatement of [Section 6](#6-troubleshooting-lifecycle), for a contributor or an AI
runtime working through an Incident directly. This checklist carries no authority of its own;
where it appears to diverge from Sections 3 through 14, those sections govern.

- [ ] The anomaly is described in terms of what was observed, not what is assumed to have caused
      it (Detect).
- [ ] The anomaly is classified using [Section 4.1](#41-the-seven-distinctions)'s vocabulary and,
      where it fits, a category from [Section 15](#15-common-failure-categories) (Classify).
- [ ] The actual state of the Environment, the repository, and the failing subject has been
      inspected, not assumed (Diagnose).
- [ ] A Root Cause has been identified and attributed to its owning document, or its absence has
      been explicitly reported (Diagnose, Root Cause Analysis).
- [ ] A Recovery has been proposed with its rationale, effect, reversibility, and the consequence
      of declining stated in full (Propose Recovery).
- [ ] The proposal's Action Class has been identified and the corresponding approval obtained, with
      explicit, specific confirmation where the class is Destructive (Confirm).
- [ ] Where approval was declined, the Incident has proceeded to Report and Record without a
      Recovery attempt.
- [ ] Where approval was granted, exactly the approved action was executed, with any scope
      expansion proposed anew (Recover).
- [ ] The outcome has been checked against the proposal's stated result, including the project's
      own test tooling where TDD governs the change (Validate).
- [ ] The Incident has been reported with the same detail whether it reached Resolution, remains a
      Workaround, or remains open, and the record contains no credential or secret (Report and
      Record).

---

**End of Troubleshooting Guide**

AEOS-TRBL · Version 1.0.0 · Traces to `PR-ENV` · `PR-WFL` · `PR-RUN` · `PR-TDD` · `PR-DOC` ·
`PR-REP` · `PR-SAF` · `PR-ONB` · `PR-NFR`, deferring entirely to AEOS-RTF for runtime-lifecycle
Failure Handling and Recovery Flow, to Specification documents for precise error conditions, and
proposing rather than asserting seven terms for AEOS-GLOSSARY's consideration
