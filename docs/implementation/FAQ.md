# AI Engineering Operating System

## AEOS — Frequently Asked Questions

*The permanent statement of the questions AEOS users most often ask, answered from the documents
that govern each answer.*

| Field | Value |
| :--- | :--- |
| **Document** | Frequently Asked Questions |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-FAQ |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Documentation Governance Board, AEOS |
| **Audience** | AEOS users, contributors, adopters evaluating AEOS, and AI runtimes consuming this repository |
| **Suggested path** | `docs/developer/FAQ.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_SPECIFICATION_STANDARD.md` (AEOS-SPECSTD) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) · `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) · `AEOS_RUNTIME_FLOW.md` (AEOS-RTF) · `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG) · `RUNTIME_CAPABILITY_SPEC.md` (AEOS-CAP) · `RUNTIME_NEGOTIATION_SPEC.md` (AEOS-SPEC-NEG) · `RUNTIME_ADAPTER_SPEC.md` (AEOS-SPEC-ADP) · `MODEL_REGISTRY.md` (AEOS-SPEC-MDL) |
| **Supersedes** | None |

> **Authority of this document.**
> This document is the official Frequently Asked Questions guide for AEOS. It answers common
> questions about AEOS consistently, in one place, using language that traces back to the documents
> that govern each answer.
>
> This document is a **Developer Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that layer:
> task-oriented material for the people and AI runtimes who work with AEOS, describing rather than
> deciding. It is not a Product document, not a Vision document, not an Architecture document, not a
> Blueprint, not a Runtime document, not a behavioral Specification, and not an Implementation Guide.
> It is not an installation guide, not a configuration guide, not a troubleshooting guide, and not an
> architecture document — each of those, where it exists, is a distinct document this one points to
> rather than reproduces. It states no product requirement, no vision, no terminology, no
> architectural decision, no Blueprint arrangement, no specified behavior, and no runtime procedure;
> where an answer here appears to do any of these, that is a defect in this document and is reported
> rather than acted upon.
>
> AEOS-DOCSTD Section 4.1 positions Developer Guides beneath Implementation Guides as the final layer
> of the documentation hierarchy, and PROJECT_BOOTSTRAP.md `BOOT-028` reserves `docs/developer/` for
> that layer's content, created empty at bootstrap time in advance of any Developer Guide being
> authored. No Developer Guide had previously been authored for AEOS at the time this document was
> written; this document is the first, and it is placed at the path `BOOT-028` reserves for it, under
> AEOS-DOCSTD rule `E5`.
>
> Each entry in this document carries the document-local identifier `FAQ-<AREA>-<NNN>`, where `AREA`
> is one of the twelve three-letter category codes [Section 5](#5-frequently-asked-questions)
> defines. Neither `FAQ` nor its category codes are registered AEOS-GLOSSARY layer or area prefixes;
> they are a traceability convention internal to this document alone, in the sense AEOS-TECH's `TG-`
> and `TC-` identifiers and AEOS-RTF's `RTF-` identifier already are for their own documents.
>
> Where this document and a document of higher authority both speak to a subject, the
> higher-authority document governs and any conflict here is a defect to be reported rather than
> acted upon.

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Intended Audience](#3-intended-audience)
4. [FAQ Philosophy](#4-faq-philosophy)
5. [Frequently Asked Questions](#5-frequently-asked-questions)
   - [5.1 General — `FAQ-GEN`](#51-general-faq-gen)
   - [5.2 Installation — `FAQ-INS`](#52-installation-faq-ins)
   - [5.3 Configuration — `FAQ-CFG`](#53-configuration-faq-cfg)
   - [5.4 Runtime — `FAQ-RTM`](#54-runtime-faq-rtm)
   - [5.5 Development Workflow — `FAQ-DEV`](#55-development-workflow-faq-dev)
   - [5.6 AI Runtime Support — `FAQ-AIR`](#56-ai-runtime-support-faq-air)
   - [5.7 Security — `FAQ-SEC`](#57-security-faq-sec)
   - [5.8 Performance — `FAQ-PRF`](#58-performance-faq-prf)
   - [5.9 Repository — `FAQ-REP`](#59-repository-faq-rep)
   - [5.10 Updates — `FAQ-UPD`](#510-updates-faq-upd)
   - [5.11 Troubleshooting References — `FAQ-TRB`](#511-troubleshooting-references-faq-trb)
   - [5.12 Documentation References — `FAQ-DOC`](#512-documentation-references-faq-doc)
6. [Cross-Reference Policy](#6-cross-reference-policy)
7. [Non-Goals](#7-non-goals)
8. [Traceability](#8-traceability)
9. [Document Governance](#9-document-governance)
10. [Appendix A — FAQ Identifier Index](#appendix-a-faq-identifier-index)

---

## 1. Purpose

A person arriving at AEOS for the first time, and an AI runtime reading this repository with no
memory of any prior session, both need the same thing before either can go further: a quick,
trustworthy answer to an ordinary question, in one place, without reading nine governing documents
to get it. AEOS-PRD names this expectation directly — a developer must be able to evaluate, install,
connect, and begin working without guesswork, and to determine at any time what AEOS is doing and why.

This document exists to meet that expectation for the class of question that recurs across users
rather than the question specific to one person's project. It states, briefly, what the governing
AEOS documents already say, and it names the document a reader should turn to for anything this
document does not or should not answer in full. It creates no new answer of its own; it locates the
existing one.

Every answer in this document is grounded in a frozen, freeze-candidate, or draft AEOS document.
Where no governing document yet answers a question this FAQ's structure implies, that absence is
recorded as such — [Section 7](#7-non-goals) and individual entries do this explicitly — rather than
filled with an invented answer. A reader who needs more than this document provides is never left to
guess; they are told which document to read next.

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs its own content only:

- the set of questions it answers and the category each belongs to;
- the form each answer takes, and the governing document each answer cites;
- the policy by which this document refers to other documents rather than restating them;
- what this document deliberately does not cover.

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what it must always remain | AEOS-VISION |
| What AEOS is and what it must do | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written and governed | AEOS-DOCSTD |
| Which technologies AEOS recognizes | AEOS-TECH |
| How AEOS is structured | AEOS-ARCH, AEOS-BLUEPRINT |
| How AEOS executes, and in what lifecycle | AEOS-RTF and the Runtime documents it composes |
| How a new AEOS repository is initialized | AEOS-BOOT |
| Where content is placed in the repository | AEOS-LAYOUT |
| What a host machine must provide before use | AEOS-ENVSETUP |
| Diagnosing a specific failure on a specific machine | The Troubleshooting Guidance of the relevant Implementation Guide, or a dedicated troubleshooting document once one exists |

A statement in this document that grants a capability, imposes a requirement, defines a term, or
implies a structure is a defect in this document. It is reported rather than acted upon.

### 2.3 Position in the Documentation Hierarchy

AEOS-DOCSTD Section 4.1 places Developer Guides as the final layer of the documentation hierarchy,
beneath Implementation Guides, with the purpose of task-oriented instruction for the people who work
within the product's result. This document occupies that layer. It carries no authority over any
subject a higher layer already governs, and where this document and a higher-layer document disagree,
the higher-layer document is correct, consistent with AEOS-DOCSTD Section 4.3's statement that a
guide "describes; it never decides."

This document is the first Developer Guide authored for AEOS. `PROJECT_BOOTSTRAP.md` `BOOT-028`
reserves `docs/developer/` for this layer, created empty at bootstrap time in anticipation of guides
such as this one. This document is placed there under its own Suggested-path metadata field.

### 2.4 Normative Language

AEOS-DOCSTD Section 7.3 states that Developer Guides should not use the RFC 2119 keyword set, because
a guide describes rather than obliges. This document follows that guidance: no sentence in this
document uses MUST, MUST NOT, SHOULD, SHOULD NOT, or MAY as an RFC 2119 or RFC 8174 keyword. Where a
cited document imposes an obligation, that obligation belongs to the cited document; this document
names it rather than restating it in binding language of its own.

### 2.5 Applicability

This document applies to every question about AEOS that recurs across users rather than belonging to
one project's specific configuration. It applies identically whether the reader is a person or an AI
runtime. It is written to be read out of order: a reader may enter at any category in
[Section 5](#5-frequently-asked-questions) without having read the sections before it.

---

## 3. Intended Audience

| Reader | What brings them here | What this document gives them |
| :--- | :--- | :--- |
| **Prospective adopter** | Evaluating AEOS before installing anything. | A vendor-neutral, runtime-neutral answer to what AEOS is, what it is not, and what it requires — matching the discoverability expectation `PR-ONB-001` states. |
| **Solo developer** | Working alone, using AEOS day to day. | Quick answers about workflow, configuration, and repository behavior without re-reading Specification documents. |
| **Engineering lead / architect** | Responsible for how a team builds, not only what it ships. | A single place to point a team toward, consistent with the same source every team member would otherwise read separately. |
| **Platform / DevOps engineer** | Owns environments, installation, and delivery. | Orientation toward Installation, Configuration, and Repository answers, with pointers to AEOS-ENVSETUP and AEOS-BOOT for procedure. |
| **Open-source contributor** | Arrives with unknown prior context. | A starting point that assumes nothing and names every document it draws from. |
| **AI runtime** *(non-human reader)* | Reads the repository to determine how to act within it. | Unambiguous, minimal, machine-consumable answers with explicit source citations, per AEOS-DOCSTD's AI Readability conventions. |

This table reflects the user categories AEOS-PRD Section 8 already defines, extended by the
prospective adopter AEOS-PRD Section 18.14 (`PR-ONB`) names as a distinct reader with needs of their
own, before they have installed anything.

---

## 4. FAQ Philosophy

Five commitments shape every entry in [Section 5](#5-frequently-asked-questions):

| ID | Commitment |
| :--- | :--- |
| `FAQ-PHI-01` | An answer states an outcome and names the document that governs it. It does not explain mechanism, and it does not quote at length. |
| `FAQ-PHI-02` | An answer is never the sole record of a decision. Where this document and its cited source appear to differ, the cited source is correct. |
| `FAQ-PHI-03` | A question this document cannot answer without inventing content is recorded as unanswered, naming who would answer it and under what document, rather than guessed at. |
| `FAQ-PHI-04` | An answer stays at the depth of the layer it draws from. A Vision-level question gets a Vision-level answer; it is not answered with implementation detail borrowed from a lower layer. |
| `FAQ-PHI-05` | Every category is written for a reader who has not read any other AEOS document, and remains accurate for a reader who has read all of them. |

These commitments are why some entries below answer a common question by stating plainly that AEOS
does not yet have a fuller answer, and naming where one will eventually live, instead of supplying
one. AEOS's own discipline of stating an undecided thing as a finished sentence about what is
undecided — not as a placeholder — applies to this document as it applies to every other.

---

## 5. Frequently Asked Questions

Each entry cites the document that governs its answer. A citation identifies a document by its
Document ID and, where one exists, the specific rule, requirement, invariant, or section it draws
from. This document restates none of them in full; the cited document is the statement of record.

### 5.1 General — `FAQ-GEN`

#### FAQ-GEN-001 — What is AEOS?

AEOS is an operating system for AI-assisted, human-supervised software engineering. It orchestrates
the engineering lifecycle — environment preparation, project initialization, requirement analysis,
architecture, TDD, agentic orchestration, generation, review, refactoring, testing, documentation,
deployment, and maintenance — while performing no language-model inference of its own.

**Source.** AEOS-PRD Section 1, Section 5.1.

#### FAQ-GEN-002 — Does AEOS perform AI inference itself?

No. AEOS never performs inference and never contains a model, under any circumstance. Inference is
always delegated to an external AI Runtime the user selects.

**Source.** AEOS-VISION `V1`; AEOS-PRD `PR-RUN-001`.

#### FAQ-GEN-003 — Is AEOS an IDE, editor, application framework, or a replacement operating system?

No, to all four. AEOS operates alongside whatever editing surface a developer already uses, produces
no application framework the user's application must be built on, and the "operating system" in its
name describes what it does for engineering activity, not a claim to replace the machine's own
operating system.

**Source.** AEOS-PRD Section 6; AEOS-VISION Section 8.1, Section 8.6.

#### FAQ-GEN-004 — Who is AEOS for?

Solo developers, engineering leads and architects, platform and DevOps engineers, open-source
maintainers, and the AI runtimes that read the repository to determine how to act within a project.

**Source.** AEOS-PRD Section 8.

#### FAQ-GEN-005 — What does AEOS guarantee?

That the human decides before AEOS acts, that AEOS explains before it executes, that tests come before
implementation, that the repository remains the source of truth after a session ends, and that the
choice of vendor, runtime, model, platform, and distribution method is the user's alone and does not
change how a project is built.

**Source.** AEOS-PRD Section 2 ("What it guarantees"); AEOS-VISION `V2`–`V6`.

#### FAQ-GEN-006 — What license governs AEOS, and who owns a given project built with it?

Neither AEOS-VISION nor AEOS-PRD establishes a license, an ownership statement, or a governance
decision beyond what those two documents already state. A license file, where a project owner wants
one, is that owner's decision, made outside the initialization procedure.

**Source.** AEOS-BOOT `NG-7`.

#### FAQ-GEN-007 — Does AEOS require an internet connection to be useful?

Not for every capability. Non-inference capability is expected to remain usable without network
access, consistent with AEOS-TECH's Offline Friendly principle; capability that depends on an AI
Runtime depends on that Runtime's own connectivity, which varies by which Runtime is selected.

**Source.** AEOS-TECH Section 5.10; AEOS-PRD Section 16.2 ("Graceful degradation").

### 5.2 Installation — `FAQ-INS`

This category orients a reader toward installation. It does not restate installation procedure; that
procedure belongs to AEOS-ENVSETUP and AEOS-BOOT.

#### FAQ-INS-001 — How is AEOS installed?

Through one of four official distribution methods at minimum: GitHub Clone, Native Installer, MCP
Distribution, and Portable Distribution. Every method delivers an identical product architecture.

**Source.** AEOS-PRD Section 15.

#### FAQ-INS-002 — Does the installation method change what AEOS is?

No. No capability is exclusive to a distribution method, and a project is portable across distribution
methods without modification; distribution affects packaging, discovery, and update mechanics only.

**Source.** AEOS-PRD Section 7.11, Section 15.3.

#### FAQ-INS-003 — Will installing AEOS overwrite my existing setup?

No. AEOS inspects a machine before changing anything about it, never assumes a clean machine, and
installation never overwrites an existing installation without approval.

**Source.** AEOS-PRD Section 11; `PR-DST-009`.

#### FAQ-INS-004 — What does my machine need before I install AEOS?

The complete, current statement of host prerequisites — supported operating systems, required
software and versions, and environment prerequisites — is AEOS-ENVSETUP's subject. This document does
not restate it.

**Source.** AEOS-ENVSETUP Sections 5–8.

#### FAQ-INS-005 — How do I know installation succeeded?

Completion of acquisition is observable, and installation reports what it found, what it proposed, and
what it actually did, following the same interaction model every other consequential AEOS action
follows.

**Source.** `PR-ONB-004`, `PR-ONB-005`; AEOS-PRD Section 10.

#### FAQ-INS-006 — What happens if installation is interrupted?

An interrupted or failed installation leaves the machine in a state a developer can understand and
safely retry. Host-specific failure diagnosis belongs to AEOS-ENVSETUP's own troubleshooting guidance.

**Source.** `PR-ONB-007`; `PR-SAF-010`; AEOS-ENVSETUP Section 16–17.

### 5.3 Configuration — `FAQ-CFG`

This category orients a reader toward configuration concepts. It does not restate configuration
procedure; that belongs to AEOS-BOOT and AEOS-ENVSETUP.

#### FAQ-CFG-001 — What configuration does AEOS create automatically?

At initialization, a deliberately small amount: a `README.md` describing the repository, and, where
version control applies, an ignore file. Nothing beyond this is established automatically.

**Source.** AEOS-BOOT Section 6.

#### FAQ-CFG-002 — Where does project-specific configuration live?

In the project's Profile: a versioned Repository Asset describing the project's identity, technology,
build and test approach, selected runtime, and applicable rules.

**Source.** AEOS-PRD `PR-PRJ-004`, `PR-PRJ-005`; AEOS-GLOSSARY, "Profile".

#### FAQ-CFG-003 — Does AEOS store credentials or secrets as part of configuration?

No, never, under any circumstance. Credentials are Runtime State belonging to the user; they are never
written into prompts, logs, reports, documentation, or Repository Assets.

**Source.** AEOS-PRD Section 20.1; `PR-SAF-006`; `PR-REP-013`.

#### FAQ-CFG-004 — Can configuration be changed after a project is set up?

Yes. Configuration expressed as a Repository Asset is versioned and changes through the project's
ordinary review, the same way code does; correcting an already-initialized repository is treated as an
ordinary change, not a re-run of initialization.

**Source.** AEOS-BOOT `NG-8`; AEOS-DOCSTD `DS-P-02`.

#### FAQ-CFG-005 — Are environment variables required?

Where a host requires them, AEOS-ENVSETUP states which are required and which are optional. This
document does not enumerate them, since the set is a host-preparation concern, not a general-audience
one.

**Source.** AEOS-ENVSETUP Sections 11–12.

#### FAQ-CFG-006 — Does configuration differ by platform?

No. Platform differences are absorbed by AEOS; workflow, rule, skill, prompt, and repository semantics
behave identically on Windows, macOS, and Linux.

**Source.** `PR-PLT-003`, `PR-PLT-005`.

### 5.4 Runtime — `FAQ-RTM`

This category concerns AEOS's own internal Runtime layer and execution lifecycle — the mechanics of
how a request is processed. Questions about which external AI Runtimes AEOS works with belong to
[Section 5.6](#56-ai-runtime-support-faq-air).

#### FAQ-RTM-001 — At a high level, what happens when AEOS processes a request?

A request moves through an ordered sequence: request acceptance, repository loading, and workflow
preparation, then, for each step of the prepared workflow, context acquisition where needed, a human
approval checkpoint, execution, and state synchronization — closing in exactly one of completion,
failure handling, or cancellation.

**Source.** AEOS-RTF Section 6.1–6.2.

#### FAQ-RTM-002 — Which part of AEOS is responsible for talking to an AI Runtime?

The Runtime Layer orchestrates external Runtimes in runtime-independent terms, and the Adapter Layer
translates between AEOS and one specific external Runtime. Knowledge of any particular runtime,
vendor, or model is confined to the Adapter Layer alone.

**Source.** AEOS-ARCH Section 4.1; `AR-PRN-005`.

#### FAQ-RTM-003 — What happens if a request is interrupted mid-flight?

Interruption at any point leaves the project in a consistent, describable state. A held request can be
resumed, and its recorded state is checked against actual condition before the resumption proceeds.

**Source.** `PR-SAF-010`; AEOS-RTF Section 6.15–6.16 (Resume Flow, Recovery Flow).

#### FAQ-RTM-004 — Does a failing or unavailable Runtime corrupt my project?

No. A failing or absent layer reduces the options available; it does not corrupt the Repository Layer.
This containment is a stated architectural invariant, not an incidental behavior.

**Source.** `AR-PRN-008`; AEOS-PRD Section 16.2 ("Graceful degradation").

#### FAQ-RTM-005 — Where is the complete runtime lifecycle defined?

`AEOS_RUNTIME_FLOW.md` (AEOS-RTF) is the precise, testable statement of the lifecycle. This document
only orients a reader to its existence and does not restate its rules.

**Source.** AEOS-RTF, in full.

### 5.5 Development Workflow — `FAQ-DEV`

#### FAQ-DEV-001 — What loop does every consequential AEOS action follow?

Inspect, then explain, then propose, then wait for confirmation, then execute exactly what was
approved, then report what happened. This loop is not optional, not configurable away, and not
shortened by urgency.

**Source.** AEOS-PRD Section 10.

#### FAQ-DEV-002 — Is Test-Driven Development mandatory?

Yes, as the default path for AEOS-governed code work, including AEOS's own development. A stated,
confirmed behavior and a failing test come before implementation; skipping the cycle is an explicit,
acknowledged exception, never a silent default.

**Source.** `PR-TDD-001`–`PR-TDD-004`, `PR-TDD-008`, `PR-TDD-012`.

#### FAQ-DEV-003 — Can AEOS take action without asking me first?

Only where the action is pure observation — reading state, changing nothing. Every local change,
external effect, and destructive action requires explicit approval, and a destructive action is never
covered by a general approval.

**Source.** AEOS-PRD Section 10.1.

#### FAQ-DEV-004 — What is an Automation Grant, and does it let AEOS act unsupervised?

An explicit, scoped, recorded, and revocable delegation of approval authority for specific classes of
action. It never authorizes a destructive action, and it can be withdrawn at any time without
justification.

**Source.** AEOS-PRD Section 10.2; `PR-SAF-012`.

#### FAQ-DEV-005 — How do Rules, Skills, and Prompts relate to a workflow?

They are versioned Repository Assets a workflow's steps draw on: Rules state engineering constraints,
Skills package reusable procedures, and Prompts carry deliberate, minimized instruction. Each is
inspectable and editable by a user without modifying AEOS itself.

**Source.** AEOS-PRD Section 12.7–12.9.

#### FAQ-DEV-006 — Does switching AI Runtimes require rewriting my workflow?

No. A workflow runs on a different Runtime by changing the project's runtime selection alone; if
anything else has to change, that is treated as a defect in AEOS, not a limitation of the project.

**Source.** AEOS-PRD Section 16.3; `PR-RUN-005`.

### 5.6 AI Runtime Support — `FAQ-AIR`

This category concerns which external AI Runtimes AEOS works with, and how. Questions about AEOS's
own internal execution mechanics belong to [Section 5.4](#54-runtime-faq-rtm).

#### FAQ-AIR-001 — Which AI Runtimes does AEOS support?

AEOS names no privileged Runtime. Recognized categories include commercial AI services,
AI-assisted development environments, open-source models, and interoperability standards such as MCP,
and the category set is deliberately left open to runtime categories that do not yet exist.

**Source.** AEOS-PRD Section 16.1; `PR-RUN-016`; AEOS-TECH `TC-09`–`TC-11`, `TC-13`.

#### FAQ-AIR-002 — Does AEOS treat commercial AI services and open-source models differently?

No. Both are the same kind of thing to AEOS — external inference, chosen and replaceable by the user —
evaluated by the same criteria, appearing in the same categories, at the same tiers.

**Source.** AEOS-TECH Section 5.12.

#### FAQ-AIR-003 — How does AEOS know what a given Runtime can do?

Through a shared Engineering Capability model that every Runtime, Adapter, and Model declares against,
and a negotiation step that checks compatibility before a workflow step is dispatched.

**Source.** `RUNTIME_CAPABILITY_SPEC.md` (AEOS-CAP); `RUNTIME_NEGOTIATION_SPEC.md` (AEOS-SPEC-NEG).

#### FAQ-AIR-004 — What happens if my selected Runtime can't support something a workflow needs?

Capability fit is reported before work begins, not discovered mid-task. AEOS states what a selected
Runtime cannot support in advance rather than failing silently partway through a step.

**Source.** AEOS-PRD Section 16.2; `PR-RUN-007`, `PR-WFL-016`.

#### FAQ-AIR-005 — Are Runtimes and Models registered anywhere?

Yes. `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG) states the observable registration, discovery, and
availability behavior for Runtimes; `MODEL_REGISTRY.md` (AEOS-SPEC-MDL) does the same for Models.

**Source.** AEOS-RUNTIME-REG; AEOS-SPEC-MDL.

#### FAQ-AIR-006 — Can a Runtime be deprecated or retired?

Yes. A registered Runtime moves through observable lifecycle states — Registered, Available,
Unavailable, Deprecated, and Retired — with deprecation disclosed before selection, never discovered
after the fact.

**Source.** AEOS-RUNTIME-REG Section 10.

### 5.7 Security — `FAQ-SEC`

#### FAQ-SEC-001 — Does AEOS ever send my code to a Runtime I haven't approved?

No. AEOS does not transmit project content to any Runtime the user has not selected and approved, and
it reports what will leave the machine before it leaves.

**Source.** `PR-SAF-007`, `PR-SAF-008`.

#### FAQ-SEC-002 — Where are credentials and secrets stored?

Never in a prompt, a log, a report, documentation, or a Repository Asset. Credentials belong to the
user, are treated as Runtime State, and are never recorded by AEOS.

**Source.** AEOS-PRD Section 20.1 (Trust Boundaries); `PR-SAF-006`.

#### FAQ-SEC-003 — Can AEOS delete or overwrite my work without confirming first?

No. A destructive action requires explicit, specific confirmation and is never covered by a general
approval; uncommitted work is never discarded and history is never rewritten without that exact
confirmation.

**Source.** `PR-SAF-003`; `PR-REP-005`, `PR-REP-006`.

#### FAQ-SEC-004 — What does AEOS do when it is uncertain about something risky?

It fails closed: it stops and asks rather than proceeding, and it states the uncertainty explicitly
rather than presenting an assumption as an observed fact.

**Source.** `PR-SAF-002`, `PR-SAF-011`; AEOS-PRD Section 20.2.

#### FAQ-SEC-005 — Does AEOS modify software it did not install?

No. AEOS does not modify or remove a component outside the project that it did not install; it behaves
as a guest on the user's machine.

**Source.** `PR-SAF-009`; AEOS-PRD Section 11.

#### FAQ-SEC-006 — Is there a dedicated technology category for security tooling?

Yes. AEOS-TECH `TC-20` records the technologies AEOS recognizes for credential storage, dependency
auditing, and related security tooling.

**Source.** AEOS-TECH `TC-20`.

### 5.8 Performance — `FAQ-PRF`

#### FAQ-PRF-001 — Does AEOS send an entire repository to a Runtime for every task?

No. Context Minimization is a governing product principle: AEOS sends the smallest context sufficient
for the step, and can explain why each piece was included on request.

**Source.** AEOS-PRD Section 7.6; `PR-PMT-003`.

#### FAQ-PRF-002 — Does AEOS publish specific speed or latency targets?

No numeric target is stated at the product layer. The stated commitment is qualitative: inspection and
reporting complete quickly enough to precede every action without discouraging their use.

**Source.** `PR-NFR-003`.

#### FAQ-PRF-003 — Does the approval-gate model make AEOS slower than working directly with an AI Runtime?

AEOS does not compete on unverified speed; it competes on how much verified work a person can be
responsible for. Incremental execution and context minimization are the stated means of keeping
individual steps fast within that constraint.

**Source.** AEOS-VISION Section 8.8; AEOS-PRD Section 23.3 ("Perceived slowness").

#### FAQ-PRF-004 — What determines how much context a Runtime receives for a given step?

The Context Layer's single responsibility: selecting the minimum sufficient context for one step, and
retaining a record of why.

**Source.** AEOS-ARCH Section 4.1.

#### FAQ-PRF-005 — Does AEOS track any performance-related metrics?

At the product level, yes — for example, context size per workflow step over time, and the number of
runtime invocations required per completed workflow, both named as Success Metrics.

**Source.** AEOS-PRD Section 21.

### 5.9 Repository — `FAQ-REP`

#### FAQ-REP-001 — Why is "the repository is the product" a governing idea for AEOS?

Because everything a project carries forward — rules, skills, prompts, workflows, profiles, decisions,
documentation — is a Repository Asset living beside the code. Nothing essential exists only outside
the repository, and it remains meaningful when AEOS is not running.

**Source.** AEOS-VISION `V5`; AEOS-PRD Section 7.5, Section 13.

#### FAQ-REP-002 — What is the difference between a Repository Asset and Runtime State?

If losing something costs only repeated work — a cache, a credential, telemetry — it is Runtime State.
If losing it costs product meaning, it is a Repository Asset.

**Source.** AEOS-PRD Section 13.4.

#### FAQ-REP-003 — Where do governing documents live in the repository?

Under `docs/`, organized by documentation layer — foundation, architecture, specification, runtime,
implementation, and, as of this document, developer. Each document's own metadata states its specific
placement.

**Source.** AEOS-LAYOUT; AEOS-BOOT Sections 4–5.

#### FAQ-REP-004 — Does AEOS ever rewrite Git history without asking?

No, never without explicit, specific confirmation of that exact operation.

**Source.** `PR-REP-005`.

#### FAQ-REP-005 — Does AEOS replace my version control or CI/CD system?

No. AEOS integrates with and orchestrates a project's existing systems; it never becomes one of them.

**Source.** AEOS-VISION Section 8.7; AEOS-PRD Section 17.1; `PR-REP-007`.

#### FAQ-REP-006 — Is the top-level repository structure fixed?

A conformant structure is stated in `REPOSITORY_LAYOUT.md`, and the sequence that produces it at
project creation is stated in `PROJECT_BOOTSTRAP.md`. This document does not restate either.

**Source.** AEOS-LAYOUT; AEOS-BOOT.

### 5.10 Updates — `FAQ-UPD`

#### FAQ-UPD-001 — How do frozen AEOS documents change over time?

Only through each document's own recorded change control. Editorial corrections, clarifications, and
substantive changes each require a stated level of approval and a version increment; nothing frozen
changes silently.

**Source.** AEOS-DOCSTD Section 12.5, Section 13.3.

#### FAQ-UPD-002 — How does a supported technology's status change over time?

Through a defined lifecycle: proposal, gating criteria, a time-boxed experimental evaluation, then
promotion to Supported and, where warranted, Preferred — with every step recorded, including a
decision not to proceed.

**Source.** AEOS-TECH Section 10, Appendix B.

#### FAQ-UPD-003 — What happens when a Runtime or Model is deprecated?

Deprecation is disclosed before it affects selection and precedes retirement; a deprecated entry
remains usable by projects already depending on it until it is formally retired.

**Source.** AEOS-RUNTIME-REG Section 10.

#### FAQ-UPD-004 — Does updating AEOS change how my project is built?

Not through distribution or platform alone — those are deployment details, never semantic differences.
A change to how a project is built would come from a change to Rules, Skills, Workflows, or
Specification content, each under its own separate change control.

**Source.** AEOS-PRD Section 14, Section 15.3.

#### FAQ-UPD-005 — Are more distribution or update methods planned beyond the four minimum ones?

Yes. Package managers, Docker images, and IDE marketplace distribution are named as planned, in part
for the native update paths each provides.

**Source.** AEOS-PRD Section 15.2.

### 5.11 Troubleshooting References — `FAQ-TRB`

This category does not diagnose or resolve problems. It states where troubleshooting content lives,
and, where none yet exists for a given subject, says so plainly.

#### FAQ-TRB-001 — Where is troubleshooting guidance for host environment setup?

AEOS-ENVSETUP's Common Setup Failures and Troubleshooting Guidance sections. This document does not
restate their content.

**Source.** AEOS-ENVSETUP Sections 16–17.

#### FAQ-TRB-002 — Is there a general troubleshooting guide covering AEOS as a whole?

Not yet. No such document has been authored at the time of this FAQ's writing. This is recorded here
as an absence, consistent with this document's own philosophy of naming what does not yet exist rather
than answering around it.

**Source.** Not applicable — recorded per [Section 4](#4-faq-philosophy), `FAQ-PHI-03`.

#### FAQ-TRB-003 — What should I do if AEOS reports uncertainty instead of acting?

Treat it as AEOS behaving as designed. Fail-closed behavior means AEOS stops and asks rather than
guessing; the appropriate response is to resolve the uncertainty, not to treat the pause as an error.

**Source.** `PR-SAF-002`; AEOS-PRD Section 20.2.

#### FAQ-TRB-004 — What should I expect if a Runtime becomes unavailable mid-workflow?

Non-inference progress already made is preserved, since a failing layer reduces options rather than
corrupting state. `AEOS_RUNTIME_FLOW.md`'s Failure Handling and Resume Flow behavior states what
happens next.

**Source.** `AR-PRN-008`; AEOS-RTF Section 6.13, Section 6.15.

#### FAQ-TRB-005 — Who resolves a conflict between two AEOS documents?

Not the reader, and not by local reinterpretation. The conflict is reported against the document that
does not own the subject, and is resolved by the owning document's own change control.

**Source.** AEOS-DOCSTD Section 11.3.

### 5.12 Documentation References — `FAQ-DOC`

#### FAQ-DOC-001 — What is the authoritative source for what AEOS must do?

`AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) — the Product Source of Truth.

**Source.** AEOS-PRD Section 25.1.

#### FAQ-DOC-002 — What is the authoritative source for AEOS terminology?

`AEOS_GLOSSARY.md` (AEOS-GLOSSARY). Every AEOS term has exactly one canonical definition, and it lives
there.

**Source.** AEOS-GLOSSARY Section 1, `DS-P-07`.

#### FAQ-DOC-003 — How is AEOS documentation itself organized and governed?

`AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) defines the documentation hierarchy, each layer's
responsibility, writing and format rules, and the freeze lifecycle every AEOS document — including
this one — follows.

**Source.** AEOS-DOCSTD, in full.

#### FAQ-DOC-004 — How do I find where a given governing document lives in the repository?

Each document's own metadata block states its Suggested path. `REPOSITORY_LAYOUT.md` and
`PROJECT_BOOTSTRAP.md` state the structure that path is placed into.

**Source.** AEOS-LAYOUT; AEOS-BOOT Section 5, Appendix A.

#### FAQ-DOC-005 — What do I do if this FAQ and a governing document disagree?

Treat the governing document as correct and report this document's entry as a defect. This document
holds no authority of its own over any subject another document governs.

**Source.** AEOS-DOCSTD Section 4.3; [Section 4](#4-faq-philosophy), `FAQ-PHI-02`.

#### FAQ-DOC-006 — Can this FAQ be relied on as a source of truth?

No. It has no independent authority; its purpose is to orient a reader toward the document that does.

**Source.** [Section 1](#1-purpose); [Section 4](#4-faq-philosophy).

---

## 6. Cross-Reference Policy

Every answer in [Section 5](#5-frequently-asked-questions) names the document it draws from, using
that document's Document ID and, where one exists, the specific identifier or section number the
answer is grounded in. This is a description of what this document already does throughout, stated
here as policy so a reader and a future author can hold new entries to the same standard.

| Policy | In practice |
| :--- | :--- |
| Every answer cites at least one governing document. | An answer with no citation is incomplete and is not published, consistent with `DS-P-10`. |
| A citation names a document and, where applicable, a specific rule, requirement, invariant, or section — never "the documentation" or "elsewhere". | Matches AEOS-DOCSTD's referencing convention in Section 11.2. |
| An answer paraphrases; it does not reproduce governing text at length. | Where exact wording matters, the citation directs the reader to the source rather than this document quoting it. |
| A governing document is cited once per answer's essential point, not repeatedly restated across several entries in the same words. | Keeps this document a set of distinct answers rather than one answer copied under different questions. |
| Where a question spans two categories, it is placed in the category its primary concern belongs to, and the other category may cross-reference it by section link rather than duplicating it. | See [Section 5.4](#54-runtime-faq-rtm) and [Section 5.6](#56-ai-runtime-support-faq-air) for a worked example of this split. |
| Where this document's answer and its cited source appear to differ, the cited source governs. | Consistent with [Section 4](#4-faq-philosophy), `FAQ-PHI-02`. |

---

## 7. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document, in
the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Installation procedure — the ordered steps by which AEOS is actually installed. | AEOS-ENVSETUP; AEOS-BOOT. |
| `NG-2` | Configuration procedure — the ordered steps by which a project or host is configured. | AEOS-BOOT Section 6; AEOS-ENVSETUP. |
| `NG-3` | Troubleshooting procedure — diagnosing or resolving a specific failure. | AEOS-ENVSETUP Sections 16–17, and a future dedicated troubleshooting document, not yet authored. |
| `NG-4` | Architectural decisions — how AEOS is structured, and why. | AEOS-ARCH, AEOS-BLUEPRINT. |
| `NG-5` | Product requirements — what AEOS must do. This document states no new requirement and reinterprets none. | AEOS-PRD. |
| `NG-6` | Terminology — the definition of any AEOS term. | AEOS-GLOSSARY. |
| `NG-7` | AEOS's license, ownership, or a governance decision beyond what AEOS-VISION and AEOS-PRD already state. | The project owner, outside this document, per AEOS-BOOT `NG-7`. |
| `NG-8` | Project-specific configuration values, error messages, or any detail that varies by installation. | The project's own Profile and its own records. |
| `NG-9` | Superseding, reinterpreting, or extending any document this document cites. | Each cited document's own change control. |
| `NG-10` | Recording an open question as a placeholder or a "to be determined" entry. | Not applicable — an unanswerable question is recorded as absent, per [Section 4](#4-faq-philosophy), `FAQ-PHI-03`, never left incomplete. |

---

## 8. Traceability

Every entry in [Section 5](#5-frequently-asked-questions) traces to a governing document, cited inline
at the point of the entry. The table below summarizes, by category, which governing documents each
category draws from principally; it is a summary for orientation, not a substitute for the inline
citation on each entry.

| Category | Code | Principal governing document(s) |
| :--- | :--- | :--- |
| General | `FAQ-GEN` | AEOS-VISION; AEOS-PRD Sections 1, 5–8, 16 |
| Installation | `FAQ-INS` | AEOS-PRD Sections 10–11, 15, 18.14 (`PR-ONB`, `PR-DST`); AEOS-ENVSETUP |
| Configuration | `FAQ-CFG` | AEOS-BOOT Section 6; AEOS-ENVSETUP Sections 11–12; AEOS-PRD Section 12.2, Section 20.1 |
| Runtime | `FAQ-RTM` | AEOS-ARCH Section 4; AEOS-RTF |
| Development Workflow | `FAQ-DEV` | AEOS-PRD Sections 9–10, 12.7–12.9, 16.3; `PR-TDD`, `PR-WFL` |
| AI Runtime Support | `FAQ-AIR` | AEOS-PRD Section 16; AEOS-TECH `TC-09`–`TC-11`, `TC-13`; AEOS-RUNTIME-REG; AEOS-CAP; AEOS-SPEC-NEG; AEOS-SPEC-ADP; AEOS-SPEC-MDL |
| Security | `FAQ-SEC` | AEOS-PRD Section 20; `PR-SAF`; AEOS-TECH `TC-20` |
| Performance | `FAQ-PRF` | AEOS-PRD Sections 7.6, 19, 21; AEOS-ARCH Section 4 |
| Repository | `FAQ-REP` | AEOS-VISION `V5`; AEOS-PRD Section 13; AEOS-LAYOUT; AEOS-BOOT |
| Updates | `FAQ-UPD` | AEOS-DOCSTD Sections 12–13; AEOS-TECH Section 10; AEOS-RUNTIME-REG Section 10 |
| Troubleshooting References | `FAQ-TRB` | AEOS-ENVSETUP Sections 16–17; AEOS-DOCSTD Section 11.3; AEOS-RTF |
| Documentation References | `FAQ-DOC` | AEOS-DOCSTD; AEOS-GLOSSARY; AEOS-LAYOUT; AEOS-BOOT |

This document, as a whole, traces to AEOS-DOCSTD `E5`'s requirement that a new kind of document be
assigned to an existing layer, satisfied by this document's position as a Developer Guide, and to
AEOS-DOCSTD `H6`'s requirement that a document belong to a layer, stated in the authority statement at
the head of this document.

---

## 9. Document Governance

### 9.1 Status

This document is a Developer Guide for the AEOS repository. It carries no source-of-truth authority
over any subject; it orients a reader toward the document that does, per Section 5's Table of
Contents and [Section 8](#8-traceability).

### 9.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Adding, clarifying, or re-citing an existing entry without changing its answer | Owner approval | Minor |
| Adding a category, or an entry whose answer states something not previously covered | Owner approval | Minor |
| Changing an existing entry's answer, or removing an entry | Explicit owner approval with recorded rationale | Major |
| Adopting RFC 2119 normative language, reversing [Section 2.4](#24-normative-language) | Explicit owner revision request | Major |

### 9.3 Identifier Policy

Each `FAQ-<AREA>-<NNN>` identifier, once published, is permanent: it is not reused, renumbered, or
reassigned to a different question. A retired entry stays in place, marked retired, rather than
removed, so that an existing reference to it continues to resolve.

### 9.4 Relationship to Governing Documents

This document introduces no requirement, no terminology, no architectural decision, and no specified
behavior. An idea surfaced by an FAQ answer that would suggest a new capability, a changed requirement,
or a correction to a governing document is recorded as feedback to that document's own owner and
applied only through that document's own change control — never inside this document.

### 9.5 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major finding remains open. A finding that an answer has drifted from its cited source is Critical; a
finding that a citation is imprecise but the answer is correct is Minor.

### 9.6 Precedence

| Situation | Resolution |
| :--- | :--- |
| An answer in this document conflicts with the document it cites | The cited document governs. The entry is a finding against this document, corrected under [Section 9.2](#92-change-control). |
| Two governing documents cited by different entries conflict with each other | Not resolved here. Reported per AEOS-DOCSTD Section 11.3. |
| A reader's question is not yet answered here | Recorded as a candidate addition under [Section 9.2](#92-change-control), never answered here by inference. |
| This document conflicts with AEOS-DOCSTD on documentation form | AEOS-DOCSTD governs. |

### 9.7 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Frequently Asked Questions guide. Establishes this document's position as the first Developer Guide authored for AEOS, placed at `docs/developer/FAQ.md` under `BOOT-028`'s reservation. Defines the FAQ Philosophy (five commitments), the Cross-Reference Policy, and the Non-Goals this document declines. Establishes sixty-nine entries across twelve categories — General, Installation, Configuration, Runtime, Development Workflow, AI Runtime Support, Security, Performance, Repository, Updates, Troubleshooting References, and Documentation References — each carrying a document-local `FAQ-<AREA>-<NNN>` identifier and citing at least one governing AEOS document. Records, deliberately, that no general troubleshooting guide yet exists for AEOS as a whole (`FAQ-TRB-002`) and that AEOS's license and ownership are project-owner decisions outside this document's scope (`FAQ-GEN-006`), rather than inventing an answer to either. Adopts AEOS-DOCSTD's convention of omitting RFC 2119 keywords, consistent with Section 7.3's guidance for the Developer Guide layer, recorded in [Section 2.4](#24-normative-language). Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no runtime procedure. No previously frozen AEOS document required a change as a result of this document's authoring. |

---

## Appendix A — FAQ Identifier Index

**This appendix is non-normative.** It indexes the categories and entry counts stated in the document
body, as a convenience to a reader and a reviewer.

| Code | Category | Entries | Section |
| :--- | :--- | :--- | :--- |
| `FAQ-GEN` | General | 7 | [5.1](#51-general-faq-gen) |
| `FAQ-INS` | Installation | 6 | [5.2](#52-installation-faq-ins) |
| `FAQ-CFG` | Configuration | 6 | [5.3](#53-configuration-faq-cfg) |
| `FAQ-RTM` | Runtime | 5 | [5.4](#54-runtime-faq-rtm) |
| `FAQ-DEV` | Development Workflow | 6 | [5.5](#55-development-workflow-faq-dev) |
| `FAQ-AIR` | AI Runtime Support | 6 | [5.6](#56-ai-runtime-support-faq-air) |
| `FAQ-SEC` | Security | 6 | [5.7](#57-security-faq-sec) |
| `FAQ-PRF` | Performance | 5 | [5.8](#58-performance-faq-prf) |
| `FAQ-REP` | Repository | 6 | [5.9](#59-repository-faq-rep) |
| `FAQ-UPD` | Updates | 5 | [5.10](#510-updates-faq-upd) |
| `FAQ-TRB` | Troubleshooting References | 5 | [5.11](#511-troubleshooting-references-faq-trb) |
| `FAQ-DOC` | Documentation References | 6 | [5.12](#512-documentation-references-faq-doc) |
| **Total** | | **69** | — |

---

**End of Frequently Asked Questions**

AEOS-FAQ · Version 1.0.0 · Traces to `PR-ONB` · `PR-SAF` · `PR-RUN` · `PR-REP` · `PR-DST` · `PR-NFR`,
orienting readers to AEOS-VISION · AEOS-PRD · AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-TECH · AEOS-ARCH ·
AEOS-BLUEPRINT · AEOS-SPECSTD · AEOS-BOOT · AEOS-LAYOUT · AEOS-ENVSETUP · AEOS-RTF · AEOS-RUNTIME-REG ·
AEOS-CAP · AEOS-SPEC-NEG · AEOS-SPEC-ADP · AEOS-SPEC-MDL without restating any of them
