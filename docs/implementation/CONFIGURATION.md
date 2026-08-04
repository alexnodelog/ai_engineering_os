# AI Engineering Operating System

## AEOS — Configuration Guide

*The permanent statement of how a Project's configuration is established, confirmed, changed, and
carried forward after AEOS is installed and before the Project is used productively.*

| Field | Value |
| :--- | :--- |
| **Document** | Configuration Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-CONFIG |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Developer Experience Board, AEOS |
| **Audience** | Developers, and AI runtimes configuring AEOS on a Developer's behalf |
| **Suggested path** | `docs/developer/CONFIGURATION.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states, precisely and reproducibly, **how a Project's configuration is established
> after AEOS is installed and before the Project is used productively**: which configuration a
> Project cannot proceed without, which configuration carries a default and MAY be left unset, how a
> proposed value becomes a confirmed and durable one, how a Developer's customization is bounded, how
> a value is validated, where it persists, how it remains portable, how it is carried forward when
> what AEOS expects changes, what happens when configuration is interrupted or found invalid, and
> what a Developer should know about configuration's security-relevant edges.
>
> This document is a **Developer Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that layer:
> task-oriented instruction for a person working within an already-installed result — not the
> procedure by which AEOS itself, or a repository carrying AEOS's own document set, is built. It is
> not a Product document, not a Vision document, not an Architecture document, not a Blueprint, not a
> Runtime document, and not a behavioral Specification under AEOS-SPECSTD. It states no product
> requirement, no architectural decision, no Blueprint arrangement, no specified behavior, and no
> terminology; where a statement here appears to do any of these, that is a defect in this document
> and MUST be reported rather than acted upon. AEOS-SPECSTD `MN5` records that a Specification
> assumes configuration as a precondition and states no procedure for reaching it; this document is
> the Developer-facing statement of that procedure, written beneath the Specification layer, not
> inside it.
>
> AEOS-DOCSTD Section 4.1 positions Developer Guides beneath Implementation Guides, as the layer
> answering *how a person works within the result*. No Developer Guide has previously been authored
> for AEOS. `PROJECT_BOOTSTRAP.md` `BOOT-028` reserves `docs/developer/` for exactly this layer; this
> document is the first to occupy it. It is written under AEOS-DOCSTD's general document template and
> the Section 4.3 purpose statement for this layer, in the absence of a dedicated Developer Guide
> Standard — in the same spirit `PROJECT_BOOTSTRAP.md` and `ENVIRONMENT_SETUP.md` record for their own
> comparable position at the Implementation Guide layer. It does not, on that account, establish such
> a Standard.
>
> **On the word "configuration."** This document uses *configuration* in its ordinary sense — the
> values a Project holds so that AEOS can operate on it without being re-explained — and does not
> introduce it as an AEOS-GLOSSARY term. AEOS-PRD Section 16.4 names four related product concerns —
> Distribution Strategy, Installation Experience, Runtime Connection, and Developer Workflow — and
> states that a requirement belonging to one MUST NOT be satisfied by addressing another. This
> document introduces no fifth product concern alongside them. What it describes falls within
> Installation Experience, AEOS-PRD Section 18.14 (`PR-ONB`): specifically, the part of "what remains
> before AEOS is usable" that consists of a Project's own recorded values, most of which AEOS-PRD
> Section 18.2 (`PR-PRJ`) already requires a Project to carry as its Profile. This document does not
> restate `PR-ONB` or `PR-PRJ`; it states the Developer-facing procedure by which their outcomes are
> reached.
>
> **On normative language at this layer.** AEOS-DOCSTD Section 7.3 records that normative language
> SHOULD NOT be used in Developer Guides, because a guide describes and never decides. This document
> deviates from that recommendation deliberately, as Section 7.2 permits, and records the reason here:
> the configuration values this document addresses are safety- and reproducibility-relevant, and a
> Developer or an AI runtime acting on this document benefits from unambiguous obligation language.
> Every normative statement below traces to the AEOS-PRD, AEOS-ARCH, or AEOS-BLUEPRINT rule that
> actually carries the obligation; this document decides nothing that is not already decided above it,
> and a normative statement here that cannot be so traced is a defect in this document.
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
3. [Configuration Philosophy](#3-configuration-philosophy)
4. [Configuration Lifecycle](#4-configuration-lifecycle)
5. [Required Configuration](#5-required-configuration)
6. [Optional Configuration](#6-optional-configuration)
7. [Runtime-Independent Configuration](#7-runtime-independent-configuration)
8. [User Customization Boundaries](#8-user-customization-boundaries)
9. [Configuration Validation](#9-configuration-validation)
10. [Configuration Persistence](#10-configuration-persistence)
11. [Configuration Portability](#11-configuration-portability)
12. [Configuration Migration](#12-configuration-migration)
13. [Failure Recovery](#13-failure-recovery)
14. [Security Considerations](#14-security-considerations)
15. [Non-Goals](#15-non-goals)
16. [Traceability](#16-traceability)
17. [References](#17-references)
18. [Document Governance](#18-document-governance)
19. [Appendix A — CFG Rule Index (Non-Normative)](#appendix-a--cfg-rule-index-non-normative)
20. [Appendix B — Configuration Checklist (Non-Normative)](#appendix-b--configuration-checklist-non-normative)

---

## 1. Executive Summary

AEOS-PRD `PR-ONB-006` requires that Installation state explicitly, on completion, what remains before
AEOS is usable. Part of what remains is connecting a Runtime, which Runtime documents govern. The
remainder is a small, bounded set of values about the Project itself — what it is, how it is built and
tested, and which rules it agrees to — that AEOS-PRD `PR-PRJ-004` already requires every Project to
carry as its Profile. Nothing before this document states, for a Developer, how that remainder is
reached: which of those values must be addressed before productive use, which may be left at their
default, how a proposed value becomes a confirmed one, what a Developer may change without asking
AEOS's permission, how a value is checked, where it lives, whether it survives a change of machine or
Runtime, what happens when AEOS's own expectations change, what happens when the process is
interrupted, and what a Developer should never mistake for configuration in the first place.

This document closes that gap once. It does not install AEOS, does not prepare a host machine, does
not connect a Runtime, and does not describe how AEOS behaves once configuration is complete and
ordinary engineering work begins — each of those is stated elsewhere, and this document neither
restates nor duplicates any of them. It states nothing about how a configuration value is stored, in
what file, at what path, or by what mechanism; `REPOSITORY_LAYOUT.md` Non-Goal `NG-2` reserves that
question for a future Implementation Guide, and this document does not anticipate it.

Four properties bind the guidance this document states:

| Property | What it requires of this document |
| :--- | :--- |
| **Deterministic** | Given the same Project state and the same Developer answers, the outcome described in [Section 4](#4-configuration-lifecycle) and [Section 9](#9-configuration-validation) is the same every time. |
| **Runtime-independent** | No guidance in this document depends on which Runtime, if any, is connected, consistent with AEOS-PRD Section 7.8 and [Section 7](#7-runtime-independent-configuration). |
| **Platform- and distribution-neutral** | Guidance is stated as what a Developer confirms and what AEOS records, never as an operating-system command or a distribution-specific step, consistent with AEOS-PRD `PR-PLT-003` and `PR-DST-005`. |
| **Non-inventive** | Every required or optional item traces to a capability AEOS-PRD already states. This document creates no new configuration surface AEOS-PRD does not already imply. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the configuration of an AEOS-governed Project between the completion of
Installation and the start of ordinary Developer Workflow, and nothing beyond it:

- which of a Project's configuration values MUST be addressed before configuration is considered
  complete, and which carry a default and MAY be left unset;
- the philosophy that governs how a configuration value moves from unknown to durable;
- the lifecycle states a configuration value passes through, and what triggers each transition;
- the boundary between what a Developer may customize freely and what remains fixed;
- how a configuration value is validated against what AEOS finds, and how a finding is reported;
- which values persist as Repository Assets, which are excluded as Runtime State, and why;
- what keeps a Project's configuration portable across Platforms and Distribution Methods;
- how existing configuration is carried forward when what AEOS expects of a Project changes;
- what a Developer can expect when configuration is interrupted, incomplete, or found invalid;
- security-relevant edges of configuration that a Developer should not mistake for the rest;
- what this document explicitly does not decide, so a reader does not search this document, or a
  future document, for a decision that belongs elsewhere.

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
| Preparing a host machine before AEOS is present, for a Contributor building AEOS itself | `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) |
| Initializing a new repository carrying AEOS's own document set | `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) |
| The permanent top-level shape of the AEOS repository | `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| The minimal, non-runtime configuration a bootstrapped repository carries from the moment of its creation | AEOS-BOOT Section 6 (`BOOT-009`–`BOOT-013`) |
| How a Developer acquires AEOS, and what Installation reports, proposes, and does on its own account | AEOS-PRD Section 18.14 (`PR-ONB`), and a future Installation Implementation Guide |
| Selecting, connecting, authenticating to, or negotiating capability with a Runtime, a Model, or a credential | `AEOS_RUNTIME_FLOW.md`, `RUNTIME_REGISTRY.md`, `RUNTIME_ADAPTER_SPEC.md`, `RUNTIME_NEGOTIATION_SPEC.md`, `MODEL_REGISTRY.md`, and AEOS-PRD Section 18.14 |
| Ongoing engineering work once configuration is complete | AEOS-PRD Sections 9, 10, 18.3 (`PR-WFL`), and 18.4 (`PR-RUN`) |
| Fixing a physical repository location or file format for a Profile or another non-Document Repository Asset | `REPOSITORY_LAYOUT.md` Non-Goal `NG-2`, reserved to a future Implementation Guide |
| What code realizes any capability named above | The codebase and its tests |

A statement in this document that redefines a `PR-`, `AR-`, or `BP-` identifier's obligation, states a
new product capability, or crosses into any row of this table is a **defect in this document**. It
MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to a Project for which Installation, per AEOS-PRD `PR-ONB`, has completed, from
the moment configuration begins until every item in [Section 5](#5-required-configuration) has been
addressed. It does not apply before Installation completes, and it does not apply to the construction
of AEOS's own repository, which AEOS-BOOT governs under a different audience and a different document
layer.

This document does not require that a Runtime already be connected. AEOS-PRD `PR-ONB-014` states that
the absence of a connected Runtime blocks only inference-dependent capability, not the completion of
Installation; this document extends that same commitment to configuration, per
[Section 7](#7-runtime-independent-configuration). Configuration MAY proceed, and MAY complete, before
a Runtime is connected, in parallel with Runtime Connection, or after it; this document states no
required order between the two, because AEOS-PRD states none.

This document applies identically to a human Developer configuring a Project directly and to an AI
runtime configuring a Project on a Developer's behalf, consistent with AEOS-DOCSTD Section 2.4.

## 3. Configuration Philosophy

These principles constrain every rule in this document. A rule that violates one is defective, not
merely unconventional.

| ID | Principle |
| :--- | :--- |
| `CFG-P-01` | Configuration is proposed, never assumed. |
| `CFG-P-02` | A confirmed value is a Repository Asset unless this document excludes it. |
| `CFG-P-03` | A default exists so that an unset optional value never blocks productive use. |
| `CFG-P-04` | Configuration never encodes an assumption about Runtime, Vendor, Model, Platform, or Distribution Method. |
| `CFG-P-05` | Recording or changing a configuration value is a Local Change and is held to that class's approval. |
| `CFG-P-06` | A credential is never configuration. |

### `CFG-P-01` — Configuration is proposed, never assumed

AEOS-PRD Section 11, Environment Philosophy, states that AEOS inspects before it acts and proposes
before it executes. Configuration is one instance of that commitment: a value AEOS derives from
inspecting the Project is reported and proposed, per the Interaction Model, AEOS-PRD Section 10; it
does not become configuration until a Developer confirms it, per `PR-PRJ-006`.

### `CFG-P-02` — A confirmed value is a Repository Asset unless this document excludes it

AEOS-PRD Section 13 draws the line between Repository Assets and Runtime State as a product
distinction: if losing a value costs product meaning, it is a Repository Asset; if losing it costs
only repeated work, it is Runtime State. Confirmed configuration ordinarily carries product meaning
and is therefore a Repository Asset, per [Section 10](#10-configuration-persistence) — except for the
specific values [Section 14](#14-security-considerations) excludes, which are Runtime State regardless
of a Developer's preference.

### `CFG-P-03` — A default exists so that an unset optional value never blocks productive use

AEOS-PRD `PR-ONB-014` and `PR-RUN-010` establish that an absent capability degrades what is available
without blocking the rest. Optional configuration, [Section 6](#6-optional-configuration), applies
the same commitment to configuration: an item with a stated default is never a precondition for
completing configuration.

### `CFG-P-04` — Configuration never encodes an assumption about Runtime, Vendor, Model, Platform, or Distribution Method

AEOS-ARCH `AR-BND-013` prohibits a Repository Asset from containing runtime-specific, vendor-specific,
model-specific, platform-specific, or distribution-specific content. AEOS-PRD Section 7.8 states the
consequence for Runtime by name: switching Runtime is a configuration change, not a migration project.
[Section 7](#7-runtime-independent-configuration) states what this means for the configuration this
document governs.

### `CFG-P-05` — Recording or changing a configuration value is a Local Change

AEOS-PRD Section 10.1 classifies updating configuration as a Local Change: it changes state that is
reversible within the repository, and it requires explicit approval of the proposal before it is
recorded. No configuration value this document governs is recorded, changed, or removed without that
approval, and none is classified as a lesser or greater action class than Section 10.1 already states.

### `CFG-P-06` — A credential is never configuration

AEOS-PRD Section 13.3 names credentials as Runtime State, never a product asset. `PR-SAF-006`,
`PR-RUN-014`, and `PR-REP-013` prohibit a credential from appearing in a Repository Asset in any form,
and AEOS-ARCH `AR-BND-009` and AEOS-BLUEPRINT `BP-REP-004` state the same prohibition as a structural
and an arrangement guard, respectively. This document treats a credential as outside its subject
matter entirely, per [Section 14](#14-security-considerations); no rule below MUST be read as
qualifying, narrowing, or creating an exception to this principle.

## 4. Configuration Lifecycle

A configuration value passes through the states below. The states are the same regardless of which
value is in question; only the content proposed and confirmed differs between one item and another.

| State | Meaning | Entered by |
| :--- | :--- | :--- |
| **Undetermined** | AEOS has not yet inspected or asked about the value. | The value's first mention in this document. |
| **Proposed** | AEOS has reported what it found or has no finding, and has proposed a value or asked the Developer to supply one. | The Inspect and Explain phases of the Interaction Model, AEOS-PRD Section 10. |
| **Confirmed** | The Developer has explicitly approved the proposed value. | The Confirm phase. Silence, ambiguity, and a prior unrelated approval are not confirmation, per AEOS-PRD Section 10. |
| **Recorded** | The confirmed value has been written as the Interaction Model's Execute and Report phases require. | Successful Execute and Report. |
| **Diverged** | A Recorded value no longer matches what AEOS observes in the Project. | Drift detected under `PR-PRJ-010`; see [Section 9](#9-configuration-validation). |
| **Deferred** | The Developer has explicitly declined to supply a value that this document does not require to be set immediately. | An explicit "not yet" answer to an item [Section 5](#5-required-configuration) permits deferring, or to any item in [Section 6](#6-optional-configuration). |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-001` | A configuration value MUST NOT be treated as Recorded unless it was previously Confirmed. | `PR-PRJ-006` |
| `CFG-002` | A value MUST NOT move from Undetermined directly to Recorded; every value passes through Proposed and Confirmed in order. | AEOS-PRD Section 10 |
| `CFG-003` | Configuration is complete only when every item in [Section 5](#5-required-configuration) is Recorded or Deferred, and no item is left Undetermined or Proposed without a pending Developer response. | `PR-ONB-006` · `PR-PRJ-004` |
| `CFG-004` | A Deferred item MUST remain reportable as Deferred, and MUST be revisited the next time the item it depends on is addressed, per whichever document owns that item. | `PR-PRJ-007` |

## 5. Required Configuration

Required configuration is the set of Profile fields, per AEOS-PRD `PR-PRJ-004`, that MUST be
addressed — proposed, and either Confirmed or explicitly Deferred — before configuration is
considered complete. "Addressed" does not mean every field carries a final value; it means the
Developer has been asked and has answered, even where the answer is to defer.

| Field | What is addressed | Deferrable | Traces to |
| :--- | :--- | :--- | :--- |
| **Identity** | What the Project is: its name and the description that lets a new session recognize it without re-explanation. | No | `PR-PRJ-004` |
| **Technology stack** | The languages, frameworks, and tools the Project uses, drawn from AEOS-TECH's recognized set. | No | `PR-PRJ-004`, AEOS-TECH |
| **Build and test approach** | How the Project is built and how it is tested, described in terms AEOS's workflow orchestration can drive. | No | `PR-PRJ-004`, `PR-WFL-001` |
| **Runtime selection** | Which Runtime, if any, the Project currently uses. | Yes | `PR-PRJ-004`, `PR-ONB-014` |
| **Applicable rules** | Which Rules, if any, the Project agrees to at the moment configuration completes. | Yes, as an empty set | `PR-PRJ-004`, `PR-RUL-001` |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-005` | A Project's Profile MUST address every field in this section's table before this document treats configuration as complete. | `PR-PRJ-004` |
| `CFG-006` | Identity, Technology stack, and Build and test approach MUST reach the Confirmed state; this document does not permit deferring them, because AEOS-PRD `PR-PRJ-004` states no default for what a Project *is*. | `PR-PRJ-004` |
| `CFG-007` | A Technology stack value MUST be drawn from AEOS-TECH's recognized set at the status that set records; a value AEOS-TECH does not recognize MUST be reported to the Developer as unrecognized rather than silently accepted, consistent with AEOS-PRD Section 11's finding for an unrecognized state. | AEOS-TECH |
| `CFG-008` | Runtime selection MAY be Deferred; where it is, this document records that fact and takes no further action toward selecting, connecting, or negotiating with a Runtime, which remains the exclusive subject of Runtime Connection documents. | `PR-ONB-014` · `PR-RUN-004` |
| `CFG-009` | Applicable rules MAY be Confirmed as an empty set; an empty set is a complete answer, not a placeholder, per AEOS-DOCSTD `DS-P-10`'s distinction between a finished statement about an incomplete decision and an unfinished section. | `PR-RUL-001` |
| `CFG-010` | The Profile carrying these fields MUST be a versioned Repository Asset, readable and editable by a human and consumable by an AI runtime without transformation. | `PR-PRJ-005` |
| `CFG-011` | AEOS MUST propose a value for a field in this section derived from inspecting the Project before asking the Developer to supply one from nothing, wherever inspection can produce a candidate value. | `PR-PRJ-006`, AEOS-PRD Section 11 |

## 6. Optional Configuration

Optional configuration is anything beyond [Section 5](#5-required-configuration)'s fields that a
Project MAY carry. Every item in this section has a stated default, and none blocks configuration
from being considered complete.

| Item | Default when unset | Traces to |
| :--- | :--- | :--- |
| **Automation grants** | None. No action class is ever automated by default. | AEOS-PRD Section 10.2 |
| **Multi-Runtime orchestration constraints** | A single selected Runtime applies to every workflow step. | `PR-RUN-013` |
| **Project-specific workflows** | The workflows AEOS already provides across the engineering lifecycle apply unmodified. | `PR-WFL-013` |
| **Prompt conventions and overrides** | AEOS's own Context Minimization and composition conventions apply. | `PR-PMT-010` |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-012` | An item in this section MUST NOT be required as a precondition for completing configuration under [Section 4](#4-configuration-lifecycle) `CFG-003`. | `PR-ONB-014` |
| `CFG-013` | An Automation grant MUST NOT be proposed by AEOS as a default; it is created only when a Developer explicitly states it, per AEOS-PRD Section 10.2. | AEOS-PRD Section 10.2 |
| `CFG-014` | Where a Project is not configured for multi-Runtime orchestration under `PR-RUN-013`, this document's silence on the subject MUST be read as the single-Runtime default, not as an unresolved question. | `PR-RUN-013` |
| `CFG-015` | Leaving an item in this section unset MUST NOT degrade a capability this document classifies as Runtime-independent, per [Section 7](#7-runtime-independent-configuration). | `PR-RUN-010` |
| `CFG-016` | A default stated in this section MUST NOT be silently changed by a later configuration action; changing what an unset item defaults to is itself a Local Change requiring the same confirmation as setting the item explicitly. | AEOS-PRD Section 10.1 |

## 7. Runtime-Independent Configuration

Configuration this document governs describes the Project, not the Runtime the Project currently
uses. AEOS-ARCH `AR-BND-013` prohibits a Repository Asset from carrying runtime-specific, vendor-
specific, model-specific, platform-specific, or distribution-specific content, and AEOS-PRD Section
7.8 states the practical consequence for Runtime directly: switching Runtime is a configuration
change, not a migration project.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-017` | A configuration value other than the Runtime selection field itself MUST NOT vary in form or meaning depending on which Runtime is selected. | `AR-BND-013` · `PR-RUN-005` |
| `CFG-018` | Where a Developer needs a value to differ per category of engineering work across more than one Runtime, that difference MUST be expressed as a per-category constraint under `PR-RUN-013`, not as a separate copy of the Project's configuration. | `PR-RUN-013` |
| `CFG-019` | A declared constraint under `CFG-018` MUST be evaluated as `RUNTIME_NEGOTIATION_SPEC.md` `SP-NEG-020` through `SP-NEG-028` already state; this document does not restate that evaluation. | AEOS-SPEC-NEG `SP-NEG-020`–`SP-NEG-028` |
| `CFG-020` | Recording a Runtime selection under `CFG-008` MUST NOT itself alter any other field this document governs; workflows, rules, skills, and prompts remain unchanged, per AEOS-PRD Section 16.3. | AEOS-PRD Section 16.3 |

## 8. User Customization Boundaries

A Developer customizes their own Project's configuration freely, within the boundary AEOS-PRD Section
13.2 already states for every Repository Asset: extensible, meaning a Developer adds, modifies, and
removes their own without modifying AEOS. That boundary has two edges.

| Edge | What it means |
| :--- | :--- |
| **A Developer may change anything this document classifies as configuration.** | Every field in [Section 5](#5-required-configuration) and every item in [Section 6](#6-optional-configuration) is the Developer's to set, change, or revert, subject only to the confirmation `CFG-P-05` already requires. |
| **A Developer does not, through Project configuration, change AEOS itself.** | Product behavior, terminology, architecture, and specified behavior remain governed by the documents that own them; nothing this document permits a Developer to configure alters what AEOS is or does for every Project. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-021` | A Developer's configuration choice MUST apply to the Developer's own Project only, and MUST NOT alter AEOS's behavior for any other Project. | `PR-PRJ-009` |
| `CFG-022` | Where AEOS finds a value it does not recognize during configuration, it MUST report the finding and MUST NOT modify or delete it, consistent with AEOS-PRD Section 11's row for an unrecognized state. | AEOS-PRD Section 11 |
| `CFG-023` | A rule, workflow, or prompt convention a Developer adds under [Section 6](#6-optional-configuration) MUST be addable and removable without a change to AEOS itself. | `PR-WFL-013` · `PR-RUL-001` · `PR-PMT-007` |
| `CFG-024` | This document MUST NOT be read as granting a Developer authority over a subject [Section 2.2](#22-what-this-document-does-not-govern) assigns elsewhere; a configuration choice that appears to do so is a defect in this document, not an available option. | AEOS-DOCSTD `H3` |

## 9. Configuration Validation

Validation applies the finding pattern AEOS-PRD Section 11 already states for the Environment to every
configuration value this document governs, without restating that section.

| Finding | AEOS behavior |
| :--- | :--- |
| **Value absent, and required** | Report the absence. Propose a value where inspection can produce one. Move to Proposed. |
| **Value absent, and optional** | Report the applicable default from [Section 6](#6-optional-configuration). Take no further action unless asked. |
| **Value present and matches what was Recorded** | Report it. Propose no change. |
| **Value present and differs from what was Recorded** | Report both, state the difference and its consequence, and propose reconciliation including "leave as is". Never reconcile silently. Move the item to Diverged. |
| **Value present and unrecognized** | Report the finding. Do not modify. Do not delete. Ask the Developer what it is. |
| **Whether a value matches cannot be determined** | Report the uncertainty explicitly. Fail closed; do not treat an assumption as a finding. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-025` | Validation MUST occur before any configuration value is treated as Confirmed, per the Inspect phase of the Interaction Model. | AEOS-PRD Section 10 |
| `CFG-026` | AEOS MUST distinguish, in every validation report, what it observed from what it inferred. | `PR-SAF-011` |
| `CFG-027` | A Diverged value MUST be reported before any workflow step that depends on it proceeds. | `PR-PRJ-010` |
| `CFG-028` | Reconciling a Diverged value is a Local Change and MUST follow `CFG-P-05`; it is never performed as a side effect of an unrelated action. | AEOS-PRD Section 10.1 |
| `CFG-029` | Where a value's correctness cannot be determined, AEOS MUST stop and report rather than proceed on an assumed value. | `PR-SAF-002` |
| `CFG-030` | A validation report MUST be available to the Developer on request, independent of whether any value is currently Diverged. | `PR-PRJ-007` · `PR-NFR-001` |
| `CFG-031` | Validation MUST NOT require a connected Runtime; every finding in this section's table is reachable by inspecting the Project alone. | `PR-ONB-014` |

## 10. Configuration Persistence

AEOS-PRD Section 13 draws the Repository Asset and Runtime State distinction as a product distinction,
not a storage design, and this document inherits it unchanged.

| Configuration content | Classification | Why |
| :--- | :--- | :--- |
| Every field in [Section 5](#5-required-configuration) and every item in [Section 6](#6-optional-configuration) that a Developer has Confirmed | Repository Asset | Losing it costs product meaning: the Project would need to be re-explained. |
| The values [Section 14](#14-security-considerations) excludes | Runtime State | Losing it costs nothing the Project needs to remain understandable. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-032` | Recorded configuration MUST remain readable and meaningful when AEOS is not running. | `PR-REP-016` · `BP-REP-012` |
| `CFG-033` | Recorded configuration MUST exist in one form serving both human review and AI runtime consumption; a separate machine-only form of the same value MUST NOT be created. | `PR-REP-010` · `BP-REP-013` |
| `CFG-034` | Recorded configuration MUST NOT be required to be present as Runtime State in order for a Project to be understood or reproduced. | `PR-REP-015` · `AR-BND-008` |
| `CFG-035` | This document states no storage location, file format, or persistence mechanism for recorded configuration; that question is `REPOSITORY_LAYOUT.md` Non-Goal `NG-2`, reserved to a future Implementation Guide. | `REPOSITORY_LAYOUT.md` `NG-2` |
| `CFG-036` | Durable custody of recorded configuration MUST be refused to anything this document or [Section 14](#14-security-considerations) classifies as Runtime State, regardless of a Developer's request. | `BP-REP-003` · `BP-REP-004` |

## 11. Configuration Portability

A Project's configuration is portable in the same sense AEOS-PRD `PR-PRJ-008` already requires of the
Project itself: it functions identically on any supported Platform and under any Distribution Method.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-037` | A configuration value MUST carry the same meaning on every Platform AEOS-PRD `PR-PLT-001` names; no value's interpretation depends on Windows, macOS, or Linux. | `PR-PLT-003` |
| `CFG-038` | A Project's recorded configuration MUST move to another supported Platform or Distribution Method unmodified. | `PR-PRJ-008` · `PR-DST-007` |
| `CFG-039` | The arrangement holding recorded configuration MUST be identical under every Distribution Method AEOS-PRD `PR-DST` names. | `BP-REP-014` |
| `CFG-040` | Onboarding a Project's configuration MUST be identical in substance across every officially supported Platform and Distribution Method; only acquisition mechanics, which Distribution Strategy governs, differ. | `PR-ONB-013` |

## 12. Configuration Migration

Migration, as this document uses the term, is what a Developer experiences when AEOS's own
expectations of a Project's configuration change — for example, when a later product revision adds a
field [Section 5](#5-required-configuration) did not previously require. It is a distinct subject from
changing an ordinary value: AEOS-PRD Section 7.8 already states that switching Runtime is a
configuration change, not a migration project, and this document does not reopen that determination.
Migration here means carrying a Project's existing Recorded configuration forward across such a
change, not the ordinary act of setting or changing a value.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-041` | Where a later revision changes what [Section 5](#5-required-configuration) or [Section 6](#6-optional-configuration) requires or offers by default, AEOS MUST report the change against the Project's currently Recorded configuration before treating anything as missing. | `PR-PRJ-010` · AEOS-PRD Section 11 |
| `CFG-042` | Every previously Confirmed value that remains valid under the new requirement MUST be carried forward without being re-asked; only the delta introduced by the change is proposed to the Developer. | `PR-NFR-002` |
| `CFG-043` | Where a required field's change of meaning is triggered by a change AEOS-TECH governs — such as a Technology stack entry being deprecated — this document's migration guidance composes with AEOS-TECH's own change control and does not substitute for it. | AEOS-TECH |
| `CFG-044` | This document states no version-numbering scheme, transformation mechanism, or storage format for carrying configuration forward; the mechanism by which a change is detected and applied is reserved to a future Implementation Guide. | AEOS-SPECSTD `MN1` |

## 13. Failure Recovery

An interrupted or invalid configuration attempt leaves the Project in a state a Developer can inspect
and safely resume, consistent with the general safety commitment AEOS-PRD `PR-SAF-010` states for
every interruption.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-045` | An interruption before a value reaches Confirmed MUST leave that value at its last reached state under [Section 4](#4-configuration-lifecycle); no partial or ambiguous state MUST be presented as Confirmed or Recorded. | `PR-SAF-010` |
| `CFG-046` | Resuming configuration after an interruption MUST begin by reporting every item's current state under [Section 4](#4-configuration-lifecycle), not by re-proposing already-Recorded values as though they were Undetermined. | `PR-WFL-008` |
| `CFG-047` | A value found invalid during [Section 9](#9-configuration-validation) MUST be reported and MUST NOT be silently discarded, corrected, or re-recorded without the confirmation `CFG-P-05` requires. | AEOS-PRD Section 11 |
| `CFG-048` | Where recovery itself cannot determine a value's last confirmed state, AEOS MUST report that uncertainty and treat the value as Undetermined rather than assume its prior value. | `PR-SAF-002` |

## 14. Security Considerations

The distinction this section draws is narrow and absolute: a credential, a secret, and any other
content Section 13.3 of AEOS-PRD classifies as Runtime State are never configuration this document
governs, regardless of how closely a Developer associates them with "getting set up."

| Excluded content | Why it is excluded |
| :--- | :--- |
| **Credentials and secrets** | AEOS-PRD Section 13.3 classifies them as Runtime State; `PR-SAF-006`, `PR-RUN-014`, and `PR-REP-013` prohibit their presence in any Repository Asset. |
| **Machine-specific configuration** | True of one machine, and therefore not true of the Project, per AEOS-PRD Section 13.3. |
| **Cache and temporary execution state** | Reproducible from Repository Assets on demand; not a statement of what the Project is. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `CFG-049` | A credential or secret MUST NOT be proposed, confirmed, or recorded as configuration under this document, in any form, including as a default. | `PR-SAF-006` · `AR-BND-009` · `BP-REP-004` |
| `CFG-050` | Where completing a required field would otherwise require a credential — for example, authenticating a Technology stack entry — this document's scope ends at recording that the field depends on a credential; supplying the credential itself remains the Developer's action and Runtime documents' or Environment documents' subject, never this document's. | `PR-SAF-007` |
| `CFG-051` | AEOS MUST report what a configuration value would cause to leave the machine, if anything, before it leaves, consistent with `PR-SAF-008`; this document creates no configuration value whose disclosure is silent. | `PR-SAF-008` |
| `CFG-052` | Recorded configuration MUST remain human-readable and reviewable in ordinary code review, so that a security-relevant change to it is never invisible to review. | `PR-REP-009` |
| `CFG-053` | AEOS-PRD Section 6 states that AEOS is not a hidden configuration store the user cannot read; every configuration value this document governs is inspectable on request, and none is held anywhere a Developer cannot see. | AEOS-PRD Section 6 · `PR-NFR-001` |

## 15. Non-Goals

| # | Non-Goal | Owned by |
| :--- | :--- | :--- |
| `NG-1` | Preparing a host machine before AEOS is present. | `ENVIRONMENT_SETUP.md` |
| `NG-2` | Initializing a new repository carrying AEOS's own document set. | `PROJECT_BOOTSTRAP.md` |
| `NG-3` | The mechanics of acquiring or installing AEOS itself. | AEOS-PRD `PR-ONB`, and a future Installation Implementation Guide |
| `NG-4` | Selecting, connecting, authenticating to, or negotiating capability with a Runtime, a Model, or a credential. | Runtime documents, and the Developer |
| `NG-5` | Ongoing engineering workflow once configuration is complete. | AEOS-PRD Sections 9, 10, 18.3, and 18.4 |
| `NG-6` | Fixing a physical repository location or file format for a Profile or another non-Document Repository Asset. | `REPOSITORY_LAYOUT.md` `NG-2` |
| `NG-7` | The storage, transport, or persistence mechanism by which a configuration value is recorded. | A future Implementation Guide |
| `NG-8` | Establishing a Project's CI/CD configuration. | The Project's own existing systems, per `PR-REP-007` |
| `NG-9` | Selecting a Technology Stack category or entry AEOS-TECH does not already recognize. | AEOS-TECH Section 2.2 |
| `NG-10` | The version-numbering scheme or transformation mechanism by which configuration is carried forward across a product change. | A future Implementation Guide |
| `NG-11` | Diagnosing or repairing a host machine, a Runtime, or a network condition that prevents configuration from completing. | `ENVIRONMENT_SETUP.md`, Runtime documents |

## 16. Traceability

Every `CFG-` rule states its own trace inline, in [Sections 4](#4-configuration-lifecycle) through
[14](#14-security-considerations). This section consolidates that trace by target.

### 16.1 Product Requirements

| `PR-` identifier | `CFG-` rules that trace to it |
| :--- | :--- |
| `PR-ONB-006` | `003` |
| `PR-ONB-013` | `040` |
| `PR-ONB-014` | `008`, `012`, `015`, `031`, `037`(indirect via `PR-PLT-003`) |
| `PR-PRJ-004` | `005`, `006`, `007`, `009` |
| `PR-PRJ-005` | `010` |
| `PR-PRJ-006` | `001`, `011` |
| `PR-PRJ-007` | `004`, `030` |
| `PR-PRJ-008` | `038` |
| `PR-PRJ-009` | `021` |
| `PR-PRJ-010` | `027`, `041` |
| `PR-RUL-001` | `009`, `023` |
| `PR-WFL-001` | — (Build and test approach field, Section 5 table) |
| `PR-WFL-008` | `046` |
| `PR-WFL-013` | `023` |
| `PR-PMT-007` | `023` |
| `PR-PMT-010` | — (Section 6 table) |
| `PR-RUN-004` | `008` |
| `PR-RUN-005` | `017` |
| `PR-RUN-010` | `015` |
| `PR-RUN-013` | `014`, `018` |
| `PR-RUN-014` | `049` |
| `PR-REP-007` | — (`NG-8`) |
| `PR-REP-009` | `052` |
| `PR-REP-010` | `033` |
| `PR-REP-013` | `049` |
| `PR-REP-015` | `034` |
| `PR-REP-016` | `032` |
| `PR-PLT-001` | `037` |
| `PR-PLT-003` | `037` |
| `PR-DST-005` | — (Executive Summary properties table) |
| `PR-DST-007` | `038` |
| AEOS-PRD Section 10 (Interaction Model) | `002`, `011`, `025` |
| AEOS-PRD Section 10.1 (Action Classes) | `016`, `028` |
| AEOS-PRD Section 10.2 (Automation Grants) | `013` |
| AEOS-PRD Section 11 (Environment Philosophy) | `011`, `022`, `041`, `047` |
| AEOS-PRD Section 16.3 (Runtime Independence in Practice) | `020` |
| `PR-SAF-002` | `029`, `048` |
| `PR-SAF-006` | `049` |
| `PR-SAF-007` | `050` |
| `PR-SAF-008` | `051` |
| `PR-SAF-010` | `045` |
| `PR-SAF-011` | `026` |
| `PR-NFR-001` | `030`, `053` |
| `PR-NFR-002` | `042` |

### 16.2 Architecture and Blueprint

| `AR-` / `BP-` identifier | `CFG-` rules that trace to it |
| :--- | :--- |
| `AR-BND-008` | `034` |
| `AR-BND-009` | `049` |
| `AR-BND-013` | `017` |
| `BP-REP-003` | `036` |
| `BP-REP-004` | `036`, `049` |
| `BP-REP-012` | `032` |
| `BP-REP-013` | `033` |
| `BP-REP-014` | `039` |

### 16.3 Depended-Upon Documents

This document depends on, without restating, `RUNTIME_NEGOTIATION_SPEC.md` `SP-NEG-020` through
`SP-NEG-028` (`CFG-019`), AEOS-TECH's technology recognition and change-control apparatus (`CFG-007`,
`CFG-043`, `NG-9`), `PROJECT_BOOTSTRAP.md` Section 6 (`BOOT-009`–`BOOT-013`), from which this
document's subject begins once bootstrap's own one-time configuration is already established,
`REPOSITORY_LAYOUT.md` `NG-2` (`CFG-035`), AEOS-SPECSTD `MN1` (`CFG-044`), and AEOS-DOCSTD `H3`
(`CFG-024`).

## 17. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-PRD Section 6, Section 7.8, Section 10, Section 10.1, Section 10.2, Section 11, Section 13, Section 16.3, Section 16.4 | The product philosophy, interaction model, action classes, environment philosophy, and Repository Asset / Runtime State distinction this document applies to configuration without restating. |
| AEOS-PRD Section 18.14 (`PR-ONB`), Section 18.2 (`PR-PRJ`), Section 18.4 (`PR-RUN`), Section 18.10 (`PR-REP`), Section 18.11 (`PR-PLT`), Section 18.12 (`PR-DST`), Section 18.13 (`PR-SAF`), Section 19 (`PR-NFR`) | The requirement families every `CFG-` rule traces to, per [Section 16](#16-traceability). |
| AEOS-GLOSSARY, *Profile*, *Project*, *Repository Asset*, *Runtime State*, *Runtime*, *Developer*, *Rule* entries | The terms this document uses without redefinition. |
| AEOS-DOCSTD Section 4.1, 4.3, 7.2, 7.3 | The Developer Guide layer's position and purpose, and the normative-language deviation this document records in its authority statement. |
| AEOS-ARCH Section 4.9, `AR-BND-008`, `AR-BND-009`, `AR-BND-013` | The Repository Layer's boundary this document applies to configuration persistence and runtime independence. |
| AEOS-BLUEPRINT `BP-REP-003`, `BP-REP-004`, `BP-REP-012`, `BP-REP-013`, `BP-REP-014` | The Repository Blueprint's custody, form, and distribution-neutrality rules this document applies to configuration. |
| AEOS-SPECSTD `MN1`, `MN5` | The boundary between a specified precondition and the procedure that reaches it, and between a behavioral statement and an implementation mechanism, which this document observes without becoming a Specification. |
| AEOS-TECH Section 2.2 | The Technology Stack recognition this document defers to rather than restates. |
| `PROJECT_BOOTSTRAP.md` Section 6, `BOOT-028` | The one-time, repository-creation-level configuration this document's subject begins after, and the `docs/developer/` reservation this document occupies. |
| `ENVIRONMENT_SETUP.md` Section 2.2 | The precedent for distinguishing this document's audience (Developers) from that document's audience (Contributors). |
| `REPOSITORY_LAYOUT.md` `NG-2` | The physical-location and file-format question this document declines to anticipate. |
| `RUNTIME_NEGOTIATION_SPEC.md` `SP-NEG-020`–`SP-NEG-028` | The constraint-evaluation behavior this document depends on for per-category Runtime configuration under `PR-RUN-013`. |

## 18. Document Governance

### 18.1 Status

This document is a **Draft** Developer Guide for the AEOS repository. It has not yet been reviewed
under AEOS-DOCSTD Section 12 and carries no authority until it is.

### 18.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Clarification of an existing rule or principle | Owner approval | Minor |
| Addition of a required or optional configuration item, or a new rule | Explicit owner revision request | Major |
| Change to which product concern ([Section 2.2](#22-what-this-document-does-not-govern)) this document's subject falls under | Explicit owner revision request with recorded rationale, and, where it would touch AEOS-PRD Section 16.4's four-concern taxonomy, an AEOS-PRD revision first | Major |
| Removal of a rule or principle | Explicit owner decision, recorded, with the reasoning preserved in place | Major |

### 18.3 Relationship to the Architecture Freeze

This document introduces no architecture and grants no capability. It states no configuration item
that AEOS-PRD does not already imply through `PR-PRJ-004` or an adjacent capability requirement. An
idea arising from this document that would add a genuinely new configuration surface is recorded as a
recommendation for a future release under AEOS-PRD governance, and is applied only after explicit
owner approval.

### 18.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major finding remains open, per AEOS-DOCSTD Section 12.

### 18.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-PRD on product behavior | AEOS-PRD governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-GLOSSARY on the meaning of a term | AEOS-GLOSSARY governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-ARCH or AEOS-BLUEPRINT on a structural or arrangement boundary | AEOS-ARCH or AEOS-BLUEPRINT governs, as applicable. The conflict is a defect in this document and is reported. |
| This document conflicts with `ENVIRONMENT_SETUP.md` or `PROJECT_BOOTSTRAP.md` on a subject [Section 2.2](#22-what-this-document-does-not-govern) assigns to either | The assigned document governs. The conflict is a defect in this document and is reported. |
| A downstream document deviates from this document on how a Project's configuration is addressed | This document governs. The deviation is a finding against the downstream document. |

### 18.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Configuration Guide. Establishes six configuration philosophy principles (`CFG-P-01`–`CFG-P-06`); a six-state configuration lifecycle; required configuration across five Profile fields; optional configuration across four items, each with a stated default; runtime-independent configuration rules binding configuration to AEOS-PRD Section 7.8 and AEOS-ARCH `AR-BND-013`; user customization boundaries; a six-finding configuration validation model adapted from AEOS-PRD Section 11; configuration persistence rules applying the Repository Asset / Runtime State distinction; configuration portability rules; configuration migration guidance that explicitly excludes ordinary Runtime switching from its subject; failure recovery rules; security considerations excluding credentials, secrets, and machine-specific values from configuration entirely; eleven recorded non-goals; and fifty-three `CFG-<NNN>` rules in total, consolidated into a Traceability section against AEOS-PRD, AEOS-ARCH, and AEOS-BLUEPRINT identifiers. Positions itself as AEOS's first Developer Guide, occupying the `docs/developer/` directory `PROJECT_BOOTSTRAP.md` `BOOT-028` reserves. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no repository-initialization or environment-preparation procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, `PROJECT_BOOTSTRAP.md`, or `ENVIRONMENT_SETUP.md`, and introduces no fifth product concern alongside the four AEOS-PRD Section 16.4 already names. |

---

## Appendix A — CFG Rule Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 4](#4-configuration-lifecycle) through [14](#14-security-considerations).

| ID | Section | Short label | Traces to |
| :--- | :--- | :--- | :--- |
| `CFG-001` | 4 | Recorded only after Confirmed | `PR-PRJ-006` |
| `CFG-002` | 4 | No skipping Proposed or Confirmed | AEOS-PRD Section 10 |
| `CFG-003` | 4 | Complete means every required item Recorded or Deferred | `PR-ONB-006` · `PR-PRJ-004` |
| `CFG-004` | 4 | Deferred item stays reportable and revisited | `PR-PRJ-007` |
| `CFG-005` | 5 | Every required field addressed | `PR-PRJ-004` |
| `CFG-006` | 5 | Identity, stack, build/test not deferrable | `PR-PRJ-004` |
| `CFG-007` | 5 | Stack value bounded by AEOS-TECH | AEOS-TECH |
| `CFG-008` | 5 | Runtime selection deferrable | `PR-ONB-014` · `PR-RUN-004` |
| `CFG-009` | 5 | Empty rule set is a complete answer | `PR-RUL-001` |
| `CFG-010` | 5 | Profile is a versioned Repository Asset | `PR-PRJ-005` |
| `CFG-011` | 5 | Propose before asking from nothing | `PR-PRJ-006` |
| `CFG-012` | 6 | Optional item never a precondition | `PR-ONB-014` |
| `CFG-013` | 6 | Automation grant never a default | AEOS-PRD Section 10.2 |
| `CFG-014` | 6 | Silence reads as single-Runtime default | `PR-RUN-013` |
| `CFG-015` | 6 | Unset item never degrades Runtime-independent capability | `PR-RUN-010` |
| `CFG-016` | 6 | Changing a default is itself a Local Change | AEOS-PRD Section 10.1 |
| `CFG-017` | 7 | No value varies by Runtime except selection itself | `AR-BND-013` · `PR-RUN-005` |
| `CFG-018` | 7 | Per-category difference is a constraint, not a copy | `PR-RUN-013` |
| `CFG-019` | 7 | Constraint evaluation deferred to SP-NEG | AEOS-SPEC-NEG `SP-NEG-020`–`028` |
| `CFG-020` | 7 | Runtime selection doesn't alter other fields | AEOS-PRD Section 16.3 |
| `CFG-021` | 8 | Choice applies to own Project only | `PR-PRJ-009` |
| `CFG-022` | 8 | Unrecognized value reported, not modified | AEOS-PRD Section 11 |
| `CFG-023` | 8 | Added items removable without changing AEOS | `PR-WFL-013` · `PR-RUL-001` · `PR-PMT-007` |
| `CFG-024` | 8 | No authority granted beyond Section 2.2 | AEOS-DOCSTD `H3` |
| `CFG-025` | 9 | Validation precedes Confirmed | AEOS-PRD Section 10 |
| `CFG-026` | 9 | Observed distinguished from inferred | `PR-SAF-011` |
| `CFG-027` | 9 | Diverged value reported before dependent step | `PR-PRJ-010` |
| `CFG-028` | 9 | Reconciliation is a Local Change | AEOS-PRD Section 10.1 |
| `CFG-029` | 9 | Undeterminable correctness stops and reports | `PR-SAF-002` |
| `CFG-030` | 9 | Validation report available on request | `PR-PRJ-007` · `PR-NFR-001` |
| `CFG-031` | 9 | Validation needs no connected Runtime | `PR-ONB-014` |
| `CFG-032` | 10 | Readable and meaningful without AEOS running | `PR-REP-016` · `BP-REP-012` |
| `CFG-033` | 10 | One form, no machine-only duplicate | `PR-REP-010` · `BP-REP-013` |
| `CFG-034` | 10 | Not required to exist as Runtime State | `PR-REP-015` · `AR-BND-008` |
| `CFG-035` | 10 | No storage mechanism stated here | `REPOSITORY_LAYOUT.md` `NG-2` |
| `CFG-036` | 10 | Runtime State refused durable custody | `BP-REP-003` · `BP-REP-004` |
| `CFG-037` | 11 | Same meaning on every Platform | `PR-PLT-003` |
| `CFG-038` | 11 | Moves to another Platform/Distribution unmodified | `PR-PRJ-008` · `PR-DST-007` |
| `CFG-039` | 11 | Arrangement identical under every Distribution Method | `BP-REP-014` |
| `CFG-040` | 11 | Onboarding identical in substance across Platform/Distribution | `PR-ONB-013` |
| `CFG-041` | 12 | Requirement change reported against current state | `PR-PRJ-010` · AEOS-PRD Section 11 |
| `CFG-042` | 12 | Still-valid values carried forward, only delta asked | `PR-NFR-002` |
| `CFG-043` | 12 | AEOS-TECH-triggered change composes with AEOS-TECH's own control | AEOS-TECH |
| `CFG-044` | 12 | No mechanism stated here | AEOS-SPECSTD `MN1` |
| `CFG-045` | 13 | Interruption leaves last reached state, not a false Confirmed | `PR-SAF-010` |
| `CFG-046` | 13 | Resume reports current state, doesn't re-propose Recorded values | `PR-WFL-008` |
| `CFG-047` | 13 | Invalid value reported, not silently corrected | AEOS-PRD Section 11 |
| `CFG-048` | 13 | Undeterminable recovery state treated as Undetermined | `PR-SAF-002` |
| `CFG-049` | 14 | Credential never proposed, confirmed, or recorded | `PR-SAF-006` · `AR-BND-009` · `BP-REP-004` |
| `CFG-050` | 14 | Credential-dependent field stops at recording the dependency | `PR-SAF-007` |
| `CFG-051` | 14 | Disclosure reported before it leaves the machine | `PR-SAF-008` |
| `CFG-052` | 14 | Recorded configuration stays reviewable | `PR-REP-009` |
| `CFG-053` | 14 | No hidden configuration store | AEOS-PRD Section 6 · `PR-NFR-001` |

## Appendix B — Configuration Checklist (Non-Normative)

A practical restatement of [Sections 4](#4-configuration-lifecycle) through
[9](#9-configuration-validation), for a Developer or an AI runtime working through configuration
directly. This checklist carries no authority of its own; where it appears to diverge from those
sections, the sections govern.

- [ ] Installation confirmed complete, per AEOS-PRD `PR-ONB`.
- [ ] Identity addressed and Confirmed.
- [ ] Technology stack addressed and Confirmed, drawn from AEOS-TECH's recognized set.
- [ ] Build and test approach addressed and Confirmed.
- [ ] Runtime selection addressed — either Confirmed or explicitly Deferred.
- [ ] Applicable rules addressed — either a Confirmed non-empty set or a Confirmed empty set.
- [ ] Every field above validated against what AEOS currently observes in the Project.
- [ ] No field left Undetermined or Proposed without a pending Developer response.
- [ ] No credential, secret, or machine-specific value present among the fields above.
- [ ] A validation report available and, if requested, reviewed.
- [ ] Optional configuration in [Section 6](#6-optional-configuration) either addressed or left at its stated default, knowingly.

---

**End of Configuration Guide**

AEOS-CONFIG · Version 1.0.0 · Traces to `PR-ONB` · `PR-PRJ` · `PR-RUN` · `PR-WFL` · `PR-REP` · `PR-PLT`
· `PR-DST` · `PR-SAF` · `PR-NFR` · `PR-RUL` · `PR-PMT`, and to `AR-BND-008` · `AR-BND-009` ·
`AR-BND-013` · `BP-REP-003` · `BP-REP-004` · `BP-REP-012` · `BP-REP-013` · `BP-REP-014`, without
restating any of them
