# AI Engineering Operating System

## AEOS — Development Roadmap

*The permanent, human-facing statement of AEOS's development trajectory: what stage the frozen
document set and the product it defines have reached, what remains before each, and in what order
the existing documents anticipate that remainder being addressed.*

| Field | Value |
| :--- | :--- |
| **Document** | Development Roadmap |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-ROADMAP |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Product owner, contributors, maintainers, prospective adopters, and other human readers assessing AEOS's development trajectory |
| **Suggested path** | `ROADMAP.md` (repository root) |
| **Companion documents** | `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_VISION.md` (AEOS-VISION) · `AEOS_ARCHITECTURE.md` (AEOS-ARCH) · `AEOS_BLUEPRINT.md` (AEOS-BLUEPRINT) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states, for a human reader, AEOS's development trajectory: which documents and which
> product capabilities have reached which stage of maturity, what stage is current, what remains, and
> the order the existing frozen and freeze-candidate documents already anticipate for that remainder.
> It is written for humans. It is not an implementation specification, not an architecture document,
> and not a project-management tool: it tracks maturity, not tasks, and it assigns no owner, no
> assignee, and no calendar date.
>
> This document is not a Vision document, not a Product Requirements document, not an Architecture
> document, not a Blueprint, not a Specification under AEOS-SPECSTD, and not an Implementation Guide
> in the AEOS-DOCSTD Section 4.3 sense. It states no product requirement, no architectural decision,
> no Blueprint arrangement, no specified behavior, and no implementation procedure; where a statement
> here appears to do any of these, that is a defect in this document and MUST be reported rather than
> acted upon. AEOS-PRD Section 22 already states that release phases describe capability maturity and
> that dates are set by the owner outside any document; this document restates that discipline for
> itself and adds no scheduling mechanism of its own.
>
> This document's position in the AEOS documentation hierarchy is reserved to the owner, consistent
> with the unassigned-layer provision AEOS-DOCSTD Section 4.5 states for the Runtime layer and that
> `REPOSITORY_LAYOUT.md` follows by analogy for its own position. Until the owner decides, this
> document complies with every AEOS-DOCSTD rule that does not depend on hierarchy position, and it
> declares itself the source of truth for no subject.
>
> Where this document and a document of higher authority — AEOS-VISION, AEOS-PRD, AEOS-ARCH,
> AEOS-BLUEPRINT, a Specification, AEOS-BOOT, or AEOS-LAYOUT — both speak to a subject, the
> higher-authority document governs and any conflict here is a defect in this document, to be
> reported rather than acted upon.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Purpose](#1-purpose)
2. [Scope](#2-scope)
3. [Roadmap Philosophy](#3-roadmap-philosophy)
4. [Current Development Status](#4-current-development-status)
5. [Completed Milestones](#5-completed-milestones)
6. [Current Phase](#6-current-phase)
7. [Remaining Phases](#7-remaining-phases)
8. [Major Deliverables](#8-major-deliverables)
9. [Repository Evolution](#9-repository-evolution)
10. [Framework Evolution](#10-framework-evolution-architecture-and-blueprint)
11. [Runtime Evolution](#11-runtime-evolution)
12. [Adapter Evolution](#12-adapter-evolution)
13. [Documentation Completion](#13-documentation-completion)
14. [Validation Phase](#14-validation-phase)
15. [Release Phase](#15-release-phase)
16. [Long-term Vision](#16-long-term-vision)
17. [Future Expansion](#17-future-expansion)
18. [Out-of-Scope Items](#18-out-of-scope-items)
19. [Success Criteria](#19-success-criteria)
20. [Progress Tracking Method](#20-progress-tracking-method)
21. [Non-goals](#21-non-goals)
22. [Traceability](#22-traceability)
23. [Document Governance](#23-document-governance)

Appendix A. [AEOS Document Status Snapshot](#appendix-a--aeos-document-status-snapshot)

---

## 1. Purpose

A reader arriving at the AEOS repository can read any single document and learn what it governs, but
no single frozen document states where the *project as a whole* currently stands, or what stands
between that point and a complete AEOS 1.0. This document exists to answer exactly that question, for
a human reader, in one place.

It does this by reflecting — never deciding — three things already stated elsewhere: each document's
own recorded Status and Version ([Section 4](#4-current-development-status)), AEOS-PRD's Release
Phases and the requirements assigned to them ([Section 22](#22-traceability) traces every figure used
here), and the extension model AEOS-ARCH and AEOS-BLUEPRINT already define for how the product grows
without structural change. Nothing in this document authorizes a change to any of them.

## 2. Scope

### 2.1 What This Document Covers

- The recorded maturity (Draft, Freeze candidate, or Frozen) of every document in the current AEOS
  document set, and what has already been completed as a result.
- Which AEOS-PRD Release Phase the product is currently working toward, and what each remaining
  phase requires, exactly as AEOS-PRD Section 22 states it.
- How the repository structure, the Architecture and Blueprint layers, the Runtime document group,
  and the Adapter and Model Registry specifications are each expected to mature and grow, using only
  the evolution and extension mechanisms those documents already define for themselves.
- The recorded, not-yet-adopted ideas AEOS-PRD Appendix A holds for a future release, and the product
  exclusions AEOS-PRD Section 17 already states.
- How progress against this document is tracked, and what this document deliberately does not do.

This list is complete.

### 2.2 What This Document Does Not Cover

| Not covered here | Owned by |
| :--- | :--- |
| Why AEOS exists, and what must never change about it | AEOS-VISION |
| What AEOS is and what it must do, including its Release Phases and numbered requirements | AEOS-PRD |
| What AEOS terms mean | AEOS-GLOSSARY |
| How AEOS documentation is written, structured, and frozen | AEOS-DOCSTD |
| How AEOS is structured, and where it may grow without structural change | AEOS-ARCH |
| How that structure is arranged to be built | AEOS-BLUEPRINT |
| How each defined behavior must work, precisely and testably | Specification documents, under AEOS-SPECSTD |
| How a new AEOS repository is initialized | AEOS-BOOT |
| Where a Repository Asset or document is physically placed | AEOS-LAYOUT |
| Calendar dates, staffing, or task assignment for any of the above | The project owner, outside any document |

A statement in this document that redefines a product requirement, an architectural decision, a
Blueprint arrangement, a specified behavior, or a Release Phase's own name or content is a **defect
in this document**. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to the AEOS 1.0 repository as a whole. It is read, not executed: no procedure
in it is performed by a person or an AI runtime, and no verification check depends on it. It is
refreshed as the documents it reflects change status; [Section 20](#20-progress-tracking-method)
states how.

## 3. Roadmap Philosophy

Four positions, already established by the documents this roadmap reflects, govern how it is read.

| # | Position | Source |
| :--- | :--- | :--- |
| 1 | **Documentation precedes implementation.** Nothing is built against a document that has not reached Freeze candidate or Frozen status; a Draft document is a statement of direction, not yet a basis for construction. | AEOS-DOCSTD Section 12 (document lifecycle) |
| 2 | **Freeze is a quality gate, not a deadline.** A document freezes when review finds no Critical or Major finding remaining, never because a date arrived. | AEOS-DOCSTD Section 12; every governed document's own Review Policy |
| 3 | **Phases describe capability maturity, not a calendar.** Dates are the owner's own affair and are not part of any AEOS document, including this one. | AEOS-PRD Section 22 |
| 4 | **Growth happens through the extension points already drawn**, not through a roadmap decision. Where the frozen Architecture and Blueprint already name a seam for addition, this document describes what grows there; where they do not, growth is a revision to those documents, reserved to the owner, and this document does not anticipate it. | AEOS-ARCH Section 11; AEOS-BLUEPRINT Section 16 |

The project's working practice — pursuing completion of each document without further change once it
is internally consistent, rather than iterating indefinitely — is the practical expression of
Position 2, applied document by document. [Section 5](#5-completed-milestones) records what that
practice has produced so far.

## 4. Current Development Status

AEOS 1.0 is defined by seventeen documents across the layers AEOS-DOCSTD Section 4 names, plus the
Runtime document group and the Repository Layout Guide, whose position in the hierarchy the owner has
not yet assigned. Each carries its own recorded Status and Version, reproduced below as of this
roadmap's authoring. This table is a non-normative, point-in-time reflection; each document's own
metadata block governs wherever the two differ, exactly as `PROJECT_BOOTSTRAP.md`'s own placement
index already treats its equivalent table.

| Status | Count | Documents |
| :--- | ---: | :--- |
| **Frozen** | 7 | AEOS-VISION · AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-TECH · AEOS-ARCH · AEOS-BLUEPRINT · AEOS-SPECSTD |
| **Freeze candidate** | 5 | AEOS-PRD · AEOS-SPEC-MDL · AEOS-RTF · AEOS-RUNTIME-REG · AEOS-CAP |
| **Draft** | 5 | AEOS-SPEC-ADP · AEOS-SPEC-NEG · AEOS-BOOT · AEOS-ENVSETUP · AEOS-LAYOUT |
| **Not yet authored** | 1 layer | Developer Guide — `docs/developer/` is reserved for it |

The complete document-by-document snapshot, with Document ID and Version, is
[Appendix A](#appendix-a--aeos-document-status-snapshot).

No AEOS-PRD requirement has yet been implemented in code. Every figure in this document describes
document maturity and planned product maturity; it makes no claim about what has been built, which
[Section 6](#6-current-phase) states directly.

## 5. Completed Milestones

The following are complete, in the sense that no further change to them is anticipated absent an
explicit owner revision request:

- **The four foundational documents are frozen**: AEOS-VISION states why AEOS exists and the ten
  invariants that must survive every future revision; AEOS-GLOSSARY fixes forty-nine terms with
  recorded authority; AEOS-DOCSTD fixes the documentation form every other AEOS document follows;
  AEOS-TECH fixes the recognized technology set.
- **The structural layer is frozen**: AEOS-ARCH states AEOS's eight architectural layers, their
  dependency direction, and the six extension points through which the product admits addition
  without structural change; AEOS-BLUEPRINT states the buildable arrangement of that structure across
  seven Blueprint layers and five extension classes.
- **The form of behavioral specification is frozen**: AEOS-SPECSTD fixes how a Specification document
  is structured, identified, and traced, ahead of the Specification documents themselves reaching
  that same state.
- **The product definition has reached freeze candidate**: AEOS-PRD states ten product capabilities,
  180 numbered requirements across fifteen requirement families, twelve quality attributes, and the
  four Release Phases this roadmap reflects in [Section 15](#15-release-phase).
- **Three Runtime-group documents have reached freeze candidate**: the Runtime Flow Specification,
  the Runtime Registry, and the Runtime Capability Specification, none of which existed as documents
  until this cycle of work produced them.
- **The Project Bootstrap Guide has completed three internal revision cycles** without changing what
  it fundamentally governs: an initial statement of the repository's directory structure and
  placement rules; a revision decoupling document placement from a fixed inventory in favor of
  discovery from each document's own metadata; and a revision adding the reserved `docs/developer/`
  directory and clarifying the Implementation Guide/Developer Guide boundary. It remains Draft
  overall pending owner review, but each of the three cycles closed without leaving an open
  inconsistency for the next to resolve.
- **Cross-document consistency has been checked at each major revision**: when AEOS-BOOT last
  changed, the frozen document set was reviewed and confirmed to require no resulting change — the
  kind of check [Section 14](#14-validation-phase) describes as ongoing practice, not a one-time
  event.

## 6. Current Phase

AEOS has not yet entered Phase 1 in the sense AEOS-PRD Section 22 defines it: no `PR-` requirement
has yet been implemented in code, and Phase 1's own exit criteria — full platform coverage and AEOS
building itself under its own TDD workflow — describes a state this project has not reached. The work
completed to date is the documentation foundation that Phase 1 implementation depends on, and most of
it is in place:

| Prerequisite for beginning Phase 1 implementation | State |
| :--- | :--- |
| A frozen statement of why AEOS exists and what must never change | Complete — AEOS-VISION, Frozen |
| A product definition stable enough to build against | Freeze candidate — AEOS-PRD, pending owner review |
| A frozen structural layer to build to | Complete — AEOS-ARCH and AEOS-BLUEPRINT, Frozen |
| A reproducible procedure for creating the repository itself | Draft — AEOS-BOOT, pending owner review |
| A stated environment-preparation procedure | Draft — AEOS-ENVSETUP, pending owner review |
| At least one Runtime Adapter specified precisely enough to build | Draft — AEOS-SPEC-ADP, pending owner review |

Phase 1's own scope — the `PR-ENV`, `PR-PRJ`, `PR-WFL`, `PR-RUN`, `PR-TDD`, `PR-SAF`, `PR-DST-001`,
and `PR-PLT` requirements marked Phase 1 in AEOS-PRD Section 18 — is [Section 8](#8-major-deliverables)'s
first row. The table above is a Phase-1-specific view; [Section 13](#13-documentation-completion)
holds the single per-document table of what remains for every non-Frozen document, including the six
named here.

## 7. Remaining Phases

AEOS-PRD Section 22 defines four Release Phases. This roadmap restates their names, goals, and exit
criteria verbatim from that section; it does not redefine them, and a phase's number or name here
MUST match AEOS-PRD Section 22 exactly.

| Phase | Name | What remains before it can close |
| :--- | :--- | :--- |
| **1** | Foundation | Every Phase 1 `P0` requirement met on Windows, macOS, and Linux via GitHub Clone distribution, and AEOS building itself under its own TDD workflow. |
| **2** | Orchestration | Every Phase 2 `P0` requirement met; a workflow running unchanged on at least two Runtimes; all four minimum distribution methods shipping. |
| **3** | Lifecycle Completion | Every Phase 3 `P0` requirement met; every engineering-lifecycle stage AEOS-PRD Section 9 names supported end to end. |
| **4** | Ecosystem | Package manager, Docker, and IDE marketplace distributions available with an identical product architecture. |

**Phase invariants**, restated from AEOS-PRD Section 22 without modification: no phase may relax a
product principle; no phase may ship a capability on a subset of supported platforms; no phase may
introduce a distribution-exclusive capability; no phase may weaken an approval gate to meet a date.

## 8. Major Deliverables

The table below is this roadmap's milestone table. It is a non-normative, point-in-time reflection of
AEOS-PRD Section 18's requirement-to-phase assignments and Section 22's phase table; where the two
disagree, AEOS-PRD governs and this table is corrected. It is refreshed at Patch level whenever a
requirement's assigned phase changes or a phase's status changes, per [Section 20](#20-progress-tracking-method).

| Phase | Goal | Major deliverables | Status |
| :--- | :--- | :--- | :--- |
| **1 — Foundation** | A trustworthy core: inspect, explain, propose, confirm, execute — on all three platforms, with TDD enforced and at least one runtime integrated. | `PR-ENV` and `PR-SAF` `P0` items · `PR-PRJ` `P0` items · `PR-WFL` `P0` items · `PR-TDD` `P0` items · one working Runtime Adapter (`PR-RUN` `P0` items, `AEOS-SPEC-ADP`) · GitHub Clone distribution (`PR-DST-001`) · Windows/macOS/Linux parity (`PR-PLT`) | **In progress (documentation).** Supporting documents mostly Frozen or Freeze candidate; `AEOS-BOOT`, `AEOS-ENVSETUP`, `AEOS-SPEC-ADP` remain Draft. No implementation started. |
| **2 — Orchestration** | Full workflow orchestration, agentic sequencing, skills, capability matching, and the four minimum distribution methods. | `PR-WFL` remainder · `PR-SKL` · capability matching (`AEOS-CAP`, `AEOS-RUNTIME-REG`) · Native Installer, MCP Distribution, and Portable Distribution (`PR-DST-002`–`004`) | **Planned.** Supporting Runtime-group specifications at Freeze candidate; `AEOS-SPEC-NEG` remains Draft. |
| **3 — Lifecycle Completion** | Remaining lifecycle stages: deployment, maintenance, drift detection, multi-runtime orchestration, CI/CD integration. | Deployment orchestration (`PR-REP-008`) · drift detection (`PR-ENV-013`, `PR-PRJ-010`, `PR-DOC-008`) · rule precedence at scale (`PR-RUL-004`) · skill portability (`PR-SKL-007`, `PR-SKL-009`) | **Future.** No supporting document targets this phase specifically beyond the requirements already recorded in AEOS-PRD. |
| **4 — Ecosystem** | Broader distribution reach and ecosystem maturity. | Package manager, Docker, and IDE marketplace distributions (`PR-DST-011`–`013`) | **Future.** |

## 9. Repository Evolution

`PROJECT_BOOTSTRAP.md` has already carried the repository structure through three revisions — from a
fixed, hand-maintained directory and document inventory, to placement discovered from each document's
own metadata, to the addition of a reserved `docs/developer/` directory — without changing what it
fundamentally governs. That chronology is recorded in full in `AEOS-BOOT` Section 13.6 and is not
repeated here; what matters for this roadmap is the pattern it establishes, not the history itself.

`REPOSITORY_LAYOUT.md` (Draft, 1.0.0) now supplies the placement authority for everything outside the
`docs/` subtree — naming conventions, root-level entries, and where source, test, and generated
content belong — while continuing to defer to `AEOS-BOOT` for the `docs/` subtree itself.

**Going forward**, repository evolution takes one of two forms. A new document arriving inside an
already-named `docs/` subdirectory requires no revision to either `AEOS-BOOT` or `AEOS-LAYOUT`, per
the discovery-from-metadata model both already use — this is the ordinary case, and this roadmap
anticipates no further work for it. A new top-level directory, a new root-level entry, or a new
placement category is a Major revision to whichever document owns that decision, reserved to the
owner; this roadmap does not anticipate which one, when, or on what trigger, consistent with
[Section 21](#21-non-goals)'s exclusion of task- and schedule-level detail.

**A recorded gap this roadmap surfaces rather than resolves.** `REPOSITORY_LAYOUT.md` Section 6
(`LAYOUT-004`, `LAYOUT-005`) currently permits exactly three things at the repository root —
`README.md`, `docs/`, and a version-control ignore file where the Distribution Method depends on
one — plus an owner-discretionary license file that Section 6 names explicitly. This document's own
placement at the repository root, stated by the owner's explicit instruction, is not yet among the
entries `REPOSITORY_LAYOUT.md` names. Adding it there is a Major revision of that document, reserved
to the owner under its own change control; this roadmap does not perform that revision, and notes it
here so the gap is recorded rather than silently worked around, consistent with this project's own
established practice.

## 10. Framework Evolution (Architecture and Blueprint)

"Framework" is used in this heading only as a plain-language label for the Architecture and Blueprint
layers together; it is not an AEOS-GLOSSARY term, defines nothing, and carries no meaning beyond
naming these two documents as a pair.

Both layers are Frozen at version 1.1.0. AEOS-ARCH states eight architectural layers — six internal,
two external — and a complete set of six extension points (`EP-1` through `EP-6`) through which
addition is admitted without structural change. AEOS-BLUEPRINT states seven Blueprint layers across
four dependency tiers, and five extension classes realizing those same six architectural points in
buildable form.

Future growth within this framework is bounded to those seams:

| Extension point | Admits | Attaches at |
| :--- | :--- | :--- |
| `EP-1` | Repository Assets of an existing kind | Repository Layer |
| `EP-2` | Repository Assets of a new kind | Repository Layer |
| `EP-3` | Runtime adapters, including categories that do not yet exist | Adapter Layer |
| `EP-4` | Engineering Capability declarations | Declared by Workflow assets and adapters |
| `EP-5` | Tool integrations | Execution Layer |
| `EP-6` | Entry surfaces | Workflow Layer, supplied by a distribution method |

An addition that attaches at one of these six points requires no change to AEOS-ARCH or
AEOS-BLUEPRINT. An addition that does not — a new layer, a new dependency direction, a new permitted
interaction, or removal of an Approval Gate — is a Major revision to one or both documents, reserved
to the owner, and is not anticipated by this roadmap.

## 11. Runtime Evolution

Three Runtime-group documents — the Runtime Flow Specification (`AEOS-RTF`), the Runtime Registry
(`AEOS-RUNTIME-REG`), and the Runtime Capability Specification (`AEOS-CAP`) — have each reached
Freeze candidate at version 1.0.0. Their position in the AEOS documentation hierarchy is, like this
roadmap's own, reserved to the owner under AEOS-DOCSTD Section 4.5's unassigned-layer provision; each
complies with every AEOS-DOCSTD rule that does not depend on hierarchy position in the meantime.

The path from here to Frozen is the same path every other document in this set has already followed:

1. Owner review under each document's own Review Policy, recording no Critical or Major finding.
2. Freeze, once review closes clean.
3. Two decisions reserved to the owner, independent of freeze: assigning the Runtime layer a position
   in AEOS-DOCSTD's hierarchy (a Major AEOS-DOCSTD change), and formally registering the `RT` layer
   prefix and `REG` area code `AEOS-RUNTIME-REG` currently proposes under its own authority, per
   AEOS-GLOSSARY `I4`.

Runtime Evolution beyond these three documents is expected to compose further Specification-layer
detail — for behavior these documents describe but do not themselves specify to Specification-layer
precision — rather than to add further Runtime-group documents of this kind; no such document is
currently planned or named.

## 12. Adapter Evolution

Adapter behavior is specified across three Draft-or-freeze-candidate documents: the Runtime Adapter
Specification (`AEOS-SPEC-ADP`, Draft), the Runtime Negotiation Specification (`AEOS-SPEC-NEG`,
Draft), and the Model Registry Specification (`AEOS-SPEC-MDL`, Freeze candidate). Together they state
the observable behavior of the Adapter Blueprint (`BP-ADP`) AEOS-BLUEPRINT already arranges.

Growth in *how many* Runtimes AEOS supports is not a roadmap decision at all: AEOS-ARCH `EP-3`
already admits a new Runtime adapter, including categories of Runtime that do not yet exist, without
any change to the Architecture or Blueprint layers. Adding support for an additional Runtime is,
structurally, an additive Specification-layer and Implementation-layer exercise against an already
frozen seam — not a structural change this roadmap needs to anticipate.

What remains before the current three Adapter-area documents close is ordinary: owner review of
`AEOS-SPEC-ADP` and `AEOS-SPEC-NEG` against the Freeze checklist AEOS-SPECSTD Section 19.2 states,
and `AEOS-SPEC-MDL`'s completion of the same review from its current Freeze-candidate position.

## 13. Documentation Completion

Ten documents remain short of Frozen. This section states what closes each, without restating any of
their content.

| Document | Status | What remains |
| :--- | :--- | :--- |
| `AEOS-PRD` | Freeze candidate | Owner review recording no Critical or Major finding. |
| `AEOS-SPEC-MDL` | Freeze candidate | Owner review against the AEOS-SPECSTD Section 19.2 checklist. |
| `AEOS-RTF` | Freeze candidate | Owner review; hierarchy position decision ([Section 11](#11-runtime-evolution)). |
| `AEOS-RUNTIME-REG` | Freeze candidate | Owner review; hierarchy position and `RT` prefix decisions ([Section 11](#11-runtime-evolution)). |
| `AEOS-CAP` | Freeze candidate | Owner review; hierarchy position decision. |
| `AEOS-SPEC-ADP` | Draft | Drafting completion, then owner review against the AEOS-SPECSTD checklist. |
| `AEOS-SPEC-NEG` | Draft | Drafting completion, then owner review against the AEOS-SPECSTD checklist. |
| `AEOS-BOOT` | Draft | Owner review against its own Section 13.4 checklist. |
| `AEOS-ENVSETUP` | Draft | Owner review. |
| `AEOS-LAYOUT` | Draft | Owner review; resolution of the recorded gap in [Section 9](#9-repository-evolution). |

No Developer Guide has been authored. `docs/developer/` is reserved and empty, per `AEOS-BOOT`
`BOOT-028`. Authoring one is future work with no assigned phase or trigger condition recorded in any
frozen document; this roadmap does not assign it one.

## 14. Validation Phase

Two forms of validation are already established practice, not a future stage this roadmap invents:

**Document-level review.** Every document in this set carries its own Review Policy: findings
classified Critical, Major, Minor, or Nitpick; freeze recommended only once no Critical or Major
finding remains open. This has already run to completion, more than once, for every Frozen document
in [Appendix A](#appendix-a--aeos-document-status-snapshot).

**Cross-document consistency review.** When a document changes, the frozen and freeze-candidate set
around it is checked for whether the change requires anything of them. This practice produced, for
example, the confirmation that no previously frozen document required change as a result of
`AEOS-BOOT`'s most recent revision.

**Implementation-level validation, once building begins**, follows directly from AEOS-VISION
invariant `V4` and AEOS-PRD Section 25.6: every `P0` requirement is covered by at least one test
written before its implementation, and AEOS-PRD Section 22's Phase 1 exit criteria requires that AEOS
build itself under that same test-first workflow. No implementation-level validation has begun, since
no implementation has begun ([Section 6](#6-current-phase)).

## 15. Release Phase

Restated in full from AEOS-PRD Section 22, without modification. This roadmap adds no phase, renames
none, and reorders none.

| Phase | Name | Goal | Exit criteria |
| :--- | :--- | :--- | :--- |
| 1 | Foundation | A trustworthy core: inspect, explain, propose, confirm, execute — on all three platforms, with TDD enforced and at least one runtime integrated. | All Phase 1 `P0` requirements met on Windows, macOS, and Linux via GitHub Clone. AEOS builds itself under its own TDD workflow. |
| 2 | Orchestration | Full workflow orchestration, agentic sequencing, skills, capability matching, and the four minimum distribution methods. | All Phase 2 `P0` requirements met. A workflow runs unchanged on at least two runtimes. All four minimum distributions ship. |
| 3 | Lifecycle Completion | Remaining lifecycle stages — deployment, maintenance, drift detection, multi-runtime orchestration, CI/CD integration. | All Phase 3 `P0` requirements met. Every lifecycle stage in AEOS-PRD Section 9 is supported end to end. |
| 4 | Ecosystem | Broader distribution reach and ecosystem maturity. | Package manager, Docker, and IDE marketplace distributions available with identical product architecture. |

A future phase beyond 4, or any change to the four above, is a Major revision to AEOS-PRD Section 22,
reserved to the owner. This roadmap does not anticipate one.

## 16. Long-term Vision

AEOS-VISION states why AEOS exists and records ten invariants — `V1` through `V10` — that must
survive every future revision of the product. This roadmap does not restate the reasoning behind
them; it states only that every phase in [Section 15](#15-release-phase) is bound by them, per the
phase invariants already restated there, and lists them here for reference:

| # | Invariant |
| :--- | :--- |
| `V1` | AEOS performs no inference. |
| `V2` | The human decides. |
| `V3` | Nothing consequential happens without being understandable first. |
| `V4` | Verification precedes implementation. |
| `V5` | The repository is the product. |
| `V6` | No vendor, runtime, model, platform, or distribution is privileged or required. |
| `V7` | The safe path is the default. |
| `V8` | The user's machine, repository, credentials, and judgment belong to the user. |
| `V9` | What AEOS does can be inspected. |
| `V10` | AEOS is extended, not modified. |

A roadmap phase that could only close by trading away one of these has not been correctly scoped; the
Phase invariants in [Section 15](#15-release-phase) exist precisely to prevent that outcome, and this
roadmap adds no exception to them.

## 17. Future Expansion

AEOS-PRD Appendix A records six ideas explicitly held outside the current product definition. None of
the six is committed to any phase in this document, and none may be treated as planned work without
an explicit owner revision request adopting it into AEOS-PRD first.

| # | Recommendation | Why it is not yet adopted |
| :--- | :--- | :--- |
| R1 | Shareable rule and skill collections distributable between organizations. | Introduces a distribution concept beyond the current asset model. |
| R2 | Team-level policy governing which automation grants individual developers may issue. | Adds an authority tier above the project owner. |
| R3 | Measured context-effectiveness feedback to inform prompt composition over time. | Introduces a measurement and feedback concept not present in the current definition. |
| R4 | AEOS-supplied workflow templates for common project archetypes. | Risks becoming a framework layer, in tension with product positioning. |
| R5 | Cross-project analytics on discipline and supervision metrics. | Raises privacy and scope questions. |
| R6 | Runtime capability benchmarking to advise runtime selection. | Risks appearing to rank vendors, in tension with Vendor Independence. |

This is distinct from growth *within* the current product definition, which occurs through the six
Architecture extension points [Section 10](#10-framework-evolution-architecture-and-blueprint) already names and requires no
new product recommendation to proceed.

## 18. Out-of-Scope Items

Restated from AEOS-PRD Section 17. These are permanent product exclusions, not items awaiting a
future phase.

| Excluded | Reason |
| :--- | :--- |
| Language-model inference | AEOS performs no inference; it orchestrates runtimes that do. |
| Model training, fine-tuning, or hosting | Belongs to model vendors and open-source model providers. |
| Replacing version control, CI/CD, or hosting platforms | AEOS integrates with these systems; it does not become one. |
| Being an IDE, editor, or application framework | AEOS operates alongside editing surfaces and produces no application framework. |
| Implementation detail | Architecture, interfaces, data formats, schemas, algorithms, file layout, and technology choices belong to downstream documents. |

## 19. Success Criteria

AEOS-PRD Section 21 states fourteen success metrics across seven categories; AEOS-PRD Section 22
states each phase's exit criteria, restated in [Section 15](#15-release-phase). This roadmap adds no
metric of its own and restates the category list only:

| Category | What it measures |
| :--- | :--- |
| Adoption | Whether projects govern real work under AEOS, and keep doing so. |
| Discipline | Whether the TDD cycle is the actual path, not the documented one. |
| Supervision | Whether the approval gate holds in practice, and decisions are genuine. |
| Independence | Whether runtime independence and vendor neutrality are exercised, not merely claimed. |
| Portability | Whether platform and distribution independence hold on real machines. |
| Efficiency | Whether Context Minimization and orchestration reduce cost over time. |
| Trust | Whether unintended destructive change occurs. Target: zero. |

## 20. Progress Tracking Method

This roadmap tracks maturity through the mechanisms the documents it reflects already provide, and
introduces none of its own:

- **Document maturity** is each document's own Status field (Draft, Freeze candidate, Frozen) and
  Version field, exactly as recorded in that document's metadata block. [Section 4](#4-current-development-status)
  and [Appendix A](#appendix-a--aeos-document-status-snapshot) reproduce these; the source document
  governs wherever the two differ.
- **Product maturity** is each requirement's own Priority (`P0`/`P1`/`P2`) and Phase field in
  AEOS-PRD Section 18, and each phase's exit criteria in AEOS-PRD Section 22.
- **This document's own tables** ([Section 4](#4-current-development-status),
  [Section 8](#8-major-deliverables), [Appendix A](#appendix-a--aeos-document-status-snapshot)) are
  non-normative, point-in-time snapshots, refreshed as a Patch-level edit whenever a reflected
  document's Status or Version changes — the same pattern `AEOS-BOOT`'s Appendix A and
  `AEOS-LAYOUT`'s Appendix A already use for their own illustrative content.

No dashboard, ticketing system, or external tracking tool is named or required by this document.

## 21. Non-goals

Behavior a reader might reasonably expect this document to cover, which it deliberately does not.

| # | Non-goal | Reason |
| :--- | :--- | :--- |
| 1 | Assigning a calendar date to any phase, milestone, or deliverable. | AEOS-PRD Section 22 reserves dates to the owner, outside any document. |
| 2 | Assigning an owner, an assignee, or a team to any item of work. | This is not a project-management tool. |
| 3 | Redefining a Release Phase's name, number, goal, or exit criteria. | AEOS-PRD Section 22 owns Release Phases; this document only reflects them. |
| 4 | Redefining a product capability, requirement, or requirement identifier. | AEOS-PRD Sections 12 and 18 own these. |
| 5 | Redefining an architectural layer, extension point, or Blueprint arrangement. | AEOS-ARCH and AEOS-BLUEPRINT own these. |
| 6 | Authorizing, by its own statement, a change to any frozen or freeze-candidate document. | Change control belongs to each document's own governance section. |
| 7 | Tracking task-level, code-level, or issue-level progress. | That belongs to the project's issue tracker and pull requests, neither of which is an AEOS document. |
| 8 | Serving as a source of truth for any subject. | AEOS-GLOSSARY Section 6 already registers the complete list of subjects with a source of truth; this document is not on it and does not add itself to it. |

## 22. Traceability

Every figure and table in this document traces to a document of higher authority. Nothing here is
asserted on this document's own authority.

| Section | Traces to |
| :--- | :--- |
| [4](#4-current-development-status), [Appendix A](#appendix-a--aeos-document-status-snapshot) | Each listed document's own metadata block; `README.md`'s Documentation Hierarchy table |
| [6](#6-current-phase), [7](#7-remaining-phases), [15](#15-release-phase) | AEOS-PRD Section 22 |
| [8](#8-major-deliverables) | AEOS-PRD Sections 18 and 22 |
| [9](#9-repository-evolution) | `AEOS-BOOT` Section 13.6 (Revision History); `REPOSITORY_LAYOUT.md` Sections 5 and 6 |
| [10](#10-framework-evolution-architecture-and-blueprint) | AEOS-ARCH Sections 4 and 11; AEOS-BLUEPRINT Sections 4 and 16 |
| [11](#11-runtime-evolution) | `AEOS_RUNTIME_FLOW.md`, `RUNTIME_REGISTRY.md`, `RUNTIME_CAPABILITY_SPEC.md` metadata and Section 2.4 of each; AEOS-DOCSTD Section 4.5; AEOS-GLOSSARY `I4` |
| [12](#12-adapter-evolution) | `RUNTIME_ADAPTER_SPEC.md`, `RUNTIME_NEGOTIATION_SPEC.md`, `MODEL_REGISTRY.md` metadata; AEOS-ARCH `EP-3`; AEOS-BLUEPRINT `BP-ADP` |
| [13](#13-documentation-completion) | Each listed document's own Document Governance section |
| [14](#14-validation-phase) | AEOS-DOCSTD Section 12; AEOS-VISION `V4`; AEOS-PRD Section 25.6 |
| [16](#16-long-term-vision) | AEOS-VISION Section 12 |
| [17](#17-future-expansion) | AEOS-PRD Appendix A |
| [18](#18-out-of-scope-items) | AEOS-PRD Section 17 |
| [19](#19-success-criteria) | AEOS-PRD Section 21 |

## 23. Document Governance

### 23.1 Status

This document is a **Draft**. It follows the ordinary five-stage lifecycle AEOS-DOCSTD Section 12.2
defines for every AEOS document — Draft, Review, Revision, Approval, Freeze — with no exception made
for it here, and is intended to reach Frozen once the owner's review under
[Section 23.4](#234-review-policy) records no Critical or Major finding.

Reaching Frozen governs this document's *structure and governance model* — its sections, its
authority statement, its non-goals, and what it claims to reflect — not its tracked-data tables. That
distinction is not an exception to AEOS-DOCSTD: Section 12.5 already defines Frozen as authoritative
and unchanging *except through the document's own change control*, and every other Frozen AEOS
document already exercises that same allowance — AEOS-VISION and AEOS-GLOSSARY have each taken a
Patch revision since reaching Frozen, without losing Frozen status in the interval. This document's
Patch-level refresh rule in [Section 23.2](#232-change-control) is that same ordinary mechanism,
applied to a document whose subject — the maturity of other documents — is expected to keep changing
after this one is itself stable.

### 23.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Refreshing a table in this document to match a reflected document's current Status, Version, or phase assignment | Contributor change, owner acceptance | Patch |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new section, or restatement of a AEOS-PRD table this document already reflects, following that table's own change | Owner approval | Minor |
| Any change to what this document claims about its own scope, authority, or non-goals | Explicit owner revision request | Major |
| Any statement that would redefine a product requirement, architectural decision, Blueprint arrangement, or Release Phase | Not permitted here; the change belongs to the owning document's own change control | — |

### 23.3 Relationship to the Architecture and Product Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. An idea surfaced while maintaining this document that would alter AEOS-PRD, AEOS-ARCH,
AEOS-BLUEPRINT, AEOS-DOCSTD, or AEOS-LAYOUT is recorded as a proposal against the owning document's
own governance and is never enacted here.

### 23.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend advancing its status when no
Critical or Major finding remains. A finding is **Critical** where this document states a figure that
contradicts the document it claims to reflect, redefines a Release Phase, a requirement, an
architectural decision, or a Blueprint arrangement, or asserts a calendar date.

### 23.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-ARCH, AEOS-BLUEPRINT, or a Specification | The higher-authority document governs. The conflict is a defect in this document and is reported. |
| This document's figures conflict with a reflected document's own metadata or content | The reflected document governs, per [Section 20](#20-progress-tracking-method). This document is corrected. |
| This document's stated repository-root placement conflicts with `REPOSITORY_LAYOUT.md` Section 6 | The gap is recorded in [Section 9](#9-repository-evolution) rather than resolved here; resolving it is a Major revision of `REPOSITORY_LAYOUT.md`, reserved to the owner. |
| A future document states a development-status figure that conflicts with this one | The apparent need is reported against this document. It is not resolved by a contradictory statement elsewhere. |

### 23.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Development Roadmap. States the current maturity of all seventeen documents in the AEOS document set, the four AEOS-PRD Release Phases and their exit criteria, a milestone table of major deliverables by phase, the evolution model for the repository structure, the Architecture/Blueprint framework, the Runtime document group, and the Adapter and Model Registry specifications, the recorded future-expansion recommendations and product exclusions, success criteria, a progress-tracking method, eight non-goals, and a complete traceability table. Records, without resolving, a gap between this document's owner-instructed repository-root placement and `REPOSITORY_LAYOUT.md`'s current root-level entry rules. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no implementation procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT, AEOS-SPECSTD, AEOS-BOOT, or AEOS-LAYOUT. |

---

## Appendix A — AEOS Document Status Snapshot

**This appendix is non-normative.** It records the complete document set known at the time of this
roadmap's authoring, for a reader's convenience. Each document's own metadata block governs wherever
the two differ, and this appendix is refreshed as a Patch-level edit whenever convenient, per
[Section 23.2](#232-change-control).

| Document | ID | Version | Status |
| :--- | :--- | :--- | :--- |
| Vision Document | `AEOS-VISION` | 1.0.1 | Frozen |
| Product Requirements Document | `AEOS-PRD` | 1.2.0 | Freeze candidate |
| Glossary | `AEOS-GLOSSARY` | 1.0.1 | Frozen |
| Document Standard | `AEOS-DOCSTD` | 3.0.0 | Frozen |
| Supported Technologies | `AEOS-TECH` | 1.0.1 | Frozen |
| Architecture | `AEOS-ARCH` | 1.1.0 | Frozen |
| Blueprint | `AEOS-BLUEPRINT` | 1.1.0 | Frozen |
| Specification Standard | `AEOS-SPECSTD` | 1.1.0 | Frozen |
| Runtime Adapter Specification | `AEOS-SPEC-ADP` | 1.0.0 | Draft |
| Runtime Negotiation Specification | `AEOS-SPEC-NEG` | 1.0.0 | Draft |
| Model Registry Specification | `AEOS-SPEC-MDL` | 1.0.0 | Freeze candidate |
| Runtime Flow Specification | `AEOS-RTF` | 1.0.0 | Freeze candidate |
| Runtime Registry | `AEOS-RUNTIME-REG` | 1.0.0 | Freeze candidate |
| Runtime Capability Specification | `AEOS-CAP` | 1.0.0 | Freeze candidate |
| Project Bootstrap Guide | `AEOS-BOOT` | 3.0.0 | Draft |
| Environment Setup Guide | `AEOS-ENVSETUP` | 1.0.0 | Draft |
| Repository Layout Guide | `AEOS-LAYOUT` | 1.0.0 | Draft |
| Developer Guide | — | — | Not yet authored |

---

**End of AEOS Development Roadmap**

AEOS-ROADMAP · Version 1.0.0 · Descriptive only — declares itself the source of truth for no subject
and authorizes no change to AEOS-VISION · AEOS-PRD · AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-TECH ·
AEOS-ARCH · AEOS-BLUEPRINT · AEOS-SPECSTD · AEOS-SPEC-ADP · AEOS-SPEC-NEG · AEOS-SPEC-MDL · AEOS-RTF ·
AEOS-RUNTIME-REG · AEOS-CAP · AEOS-BOOT · AEOS-ENVSETUP · or AEOS-LAYOUT
