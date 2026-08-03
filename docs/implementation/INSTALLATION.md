# AI Engineering Operating System

## AEOS — Installation Guide

*The permanent statement of how a Developer installs an already-released copy of AEOS, verifies the
result, recovers from an interrupted attempt, and reinstalls or upgrades it.*

| Field | Value |
| :--- | :--- |
| **Document** | Installation Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-INSTALL |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Developers installing AEOS for the first time, Developers reinstalling or upgrading an existing installation, and AI runtimes performing installation on a Developer's behalf |
| **Suggested path** | `docs/implementation/INSTALLATION.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `ENVIRONMENT_SETUP.md` (AEOS-ENVSETUP) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states, precisely and reproducibly, **the procedure by which AEOS is installed once
> its own prerequisites are already satisfied**: the installation scenarios this document recognizes,
> the prerequisites installation depends on but does not itself establish, the ordered sequence by
> which installation is carried out, the checks that confirm a successful installation, the state a
> successfully installed copy of AEOS is in, the failures installation may encounter, how an
> interrupted installation is recovered, how an existing installation is reinstalled, and what an
> upgrade must and must not do to a Developer's own project.
>
> **This document is not a bootstrap guide.** It states no repository-initialization procedure, no
> directory structure, and no document-placement rule; that procedure belongs to AEOS-BOOT alone, and
> where the GitHub Clone scenario in [Section 5](#5-supported-installation-scenarios) obtains a
> repository, it obtains one whose content and structure were already produced under AEOS-BOOT,
> without re-performing or restating that production.
>
> **This document is not an environment setup guide.** It states no host operating system tier, no
> required software, no environment variable, and no directory-level or path condition of its own;
> every prerequisite this document depends on is established by AEOS-ENVSETUP or AEOS-TECH and is
> referenced here, never restated.
>
> **This document is not an architecture document.** It states no structural decision, no Blueprint
> arrangement, and no specified behavior; where a statement here appears to do any of these, that is a
> defect in this document and MUST be reported rather than acted upon.
>
> This document defines installation only — the process by which AEOS is installed after every
> prerequisite in [Section 6](#6-installation-prerequisites-reference-only) is already satisfied. It
> MUST NOT define environment preparation, repository organization, runtime behavior, implementation
> internals, architecture, or any technology selection beyond what `AEOS_SUPPORTED_TECHNOLOGIES.md`
> already recognizes.
>
> This document is an **Implementation Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that
> layer: concrete guidance for building to specification, expressed at the point specification meets
> construction. It is not a Product document, not a Vision document, not a Blueprint, not a Runtime
> document, and not a behavioral Specification under AEOS-SPECSTD. It states no product requirement,
> no architectural decision, no Blueprint arrangement, no specified behavior, no runtime lifecycle, and
> no terminology of its own.
>
> AEOS-DOCSTD Section 4.1 positions Implementation Guides beneath Specification and above Developer
> Guides in the documentation hierarchy. This document is written under AEOS-DOCSTD's general document
> template and the Section 4.3 purpose statement for this layer, in the absence of a dedicated
> Implementation Guide Standard — in the same spirit AEOS-BOOT, AEOS-ENVSETUP, and AEOS-LAYOUT record
> for their own comparable position. It does not, on that account, establish such a Standard.
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
3. [Relationship to Other Documents](#3-relationship-to-other-documents)
4. [Installation Principles](#4-installation-principles)
5. [Supported Installation Scenarios](#5-supported-installation-scenarios)
6. [Installation Prerequisites (Reference Only)](#6-installation-prerequisites-reference-only)
7. [Installation Sequence](#7-installation-sequence)
8. [Verification After Installation](#8-verification-after-installation)
9. [Expected Successful Installation State](#9-expected-successful-installation-state)
10. [Common Installation Failures](#10-common-installation-failures)
11. [Recovery Procedures](#11-recovery-procedures)
12. [Reinstallation Guidance](#12-reinstallation-guidance)
13. [Upgrade Considerations](#13-upgrade-considerations)
14. [Non-Goals](#14-non-goals)
15. [Traceability](#15-traceability)
16. [References](#16-references)
17. [Document Governance](#17-document-governance)
18. [Appendix A — Installation Checklist (Non-Normative)](#appendix-a--installation-checklist-non-normative)
19. [Appendix B — INSTALL Rule Index (Non-Normative)](#appendix-b--install-rule-index-non-normative)

---

## 1. Executive Summary

AEOS-PRD Section 15 commits the product to four official Distribution Methods and to a closed set of
Distribution invariants: identical architecture regardless of method, no capability exclusive to one
method, project portability across methods, and an installation that inspects the environment before
it acts and never overwrites what already exists without approval. None of that is, by itself, a
procedure. AEOS-BOOT states how the AEOS repository itself comes to hold the documents it holds.
AEOS-ENVSETUP states what a machine needs before AEOS-BOOT is even attempted, and before a Contributor
does any other work in the AEOS repository. Neither states what a Developer actually does to obtain,
place, verify, recover, reinstall, or upgrade an already-released copy of AEOS for their own use — the
gap PR-DST-009 assumes is already closed somewhere. This document closes it.

This document states, once, the complete and ordered procedure by which installation is carried out
for each of the four official Distribution Methods, the checks by which a completed installation is
verified, and the repository- and machine-independent state a correctly installed copy of AEOS is in
once those checks pass. It derives every check from the four Distribution invariants AEOS-PRD Section
15.3 already states, rather than inventing new ones, and it introduces no terminology, no
architectural decision, and no technology beyond what AEOS-TECH already recognizes. It is, by design,
narrower than either AEOS-BOOT or AEOS-ENVSETUP: it begins only after both of their subjects are
already settled, and it states nothing about how AEOS behaves once installation completes — that
remains Runtime-layer and product-layer territory.

Four properties bind the procedure this document states, consistent with the discipline AEOS-BOOT and
AEOS-ENVSETUP record for a comparable statement of fact:

| Property | What it requires of this document |
| :--- | :--- |
| **Reproducible** | Given the same target location, the same chosen scenario, and the same distribution artifact, the sequence in [Section 7](#7-installation-sequence) produces the same installation state, described in [Section 9](#9-expected-successful-installation-state), every time it is followed. |
| **Deterministic** | The order of actions in [Section 7](#7-installation-sequence) is fixed; no step depends on a choice this document leaves unstated. |
| **Platform-neutral** | Every action is stated as *what* occurs, never as an operating-system-specific or scenario-internal command, consistent with AEOS-PRD `PR-PLT-003` and `PR-PLT-005`. |
| **Non-invasive** | Installation inspects before it acts and never modifies, replaces, or removes a component it did not itself place, without the explicit, specific confirmation AEOS-PRD `PR-DST-009` and `PR-SAF-003` require. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the installation of AEOS after its prerequisites are already satisfied, and
nothing beyond it:

- the complete set of installation scenarios this document recognizes, one for each official
  Distribution Method AEOS-PRD Section 15.1 lists;
- the installation prerequisites this document depends on, stated by reference only;
- the ordered sequence of actions by which installation is carried out, common to every scenario
  except where a step is explicitly scenario-conditional;
- the checks by which a completed installation is verified;
- the state a successfully installed copy of AEOS is in once verification passes;
- the failures installation may encounter, and the recovery procedure that follows an interruption;
- the guidance by which an existing installation is reinstalled;
- the considerations that apply when an installed copy of AEOS is upgraded to a newer version;
- what installation explicitly does not do, so a reader does not search this document, or a future
  document, for a capability installation was never meant to have.

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
| What a host machine must provide before installation is attempted | AEOS-ENVSETUP |
| The permanent top-level shape of the AEOS repository | AEOS-LAYOUT |
| The ordered procedure that creates, populates, and verifies a new AEOS repository | AEOS-BOOT |
| Installing, configuring, or launching any AEOS Runtime, Adapter, or Model | Runtime documents and future Implementation Guides for those layers |
| Initializing or adopting a Developer's own project once AEOS is installed | `PR-PRJ`, and a future Implementation or Developer Guide |
| How a person works day to day within an installed copy of AEOS | Developer Guides, none of which yet exist for AEOS |
| What code realizes any capability named above | The codebase and its tests |

A statement in this document that states a repository-initialization procedure, an environment
preparation step, a repository-organization rule, a runtime behavior, an architectural decision, or a
technology choice absent from AEOS-TECH is a **defect in this document**. It MUST be reported rather
than acted upon.

### 2.3 Applicability

This document applies from the moment a Developer, or an AI runtime acting on a Developer's behalf,
decides to obtain AEOS through one of the four scenarios [Section 5](#5-supported-installation-scenarios)
recognizes, through the point installation is verified complete. It applies identically to a human
Developer and to an AI runtime performing installation on a human's behalf, consistent with
AEOS-DOCSTD Section 2.4.

This document does not apply to the production of the distributable artifact itself for any scenario
— the built installer package, the tagged repository state, the registered MCP distribution, or the
packaged portable bundle. Producing those artifacts is a Release Engineering and build concern,
outside this document's scope, per [Section 14](#14-non-goals).

## 3. Relationship to Other Documents

| Document | Governs | This document's obligation |
| :--- | :--- | :--- |
| **AEOS-VISION** | Purpose and enduring intent. | This document takes no position on why AEOS exists. No rule below may require trading away an AEOS-VISION invariant. |
| **AEOS-PRD** | Product definition, the Distribution Strategy in Section 15, and the `PR-` requirements. | Every `INSTALL-` rule that binds a party traces to one or more `PR-` identifiers already established there; this document adds no new requirement and narrows none. |
| **AEOS-GLOSSARY** | Terminology. | This document uses *Developer*, *Distribution Method*, *Platform*, *Repository*, *Repository Asset*, and *Runtime State* exactly as AEOS-GLOSSARY defines them, and defines none of them again. |
| **AEOS-DOCSTD** | Documentation form and lifecycle. | This document's structure, format, normative vocabulary, and template follow it without exception. |
| **AEOS-TECH** | Recognized technologies and their support tiers. | This document names no technology of its own; where a scenario depends on a recognized technology, that dependency is stated by reference to AEOS-TECH, never restated. |
| **AEOS-BOOT** | The ordered procedure that initializes a new AEOS repository. | The GitHub Clone scenario in [Section 5](#5-supported-installation-scenarios) obtains a copy of a repository AEOS-BOOT already produced; this document does not restate or re-perform that procedure. |
| **AEOS-ENVSETUP** | What a host machine must provide before AEOS-BOOT's prerequisites, and before AEOS work generally, is attempted. | [Section 6](#6-installation-prerequisites-reference-only) depends on AEOS-ENVSETUP for every machine-level prerequisite; this document restates none of them. |
| **AEOS-LAYOUT** | The permanent shape of the AEOS repository. | This document names no path, directory, or naming convention of its own beyond what AEOS-LAYOUT and AEOS-BOOT already state. |

## 4. Installation Principles

These principles constrain every rule in this document. A rule that violates one is defective, not
merely unconventional.

| # | Principle |
| :--- | :--- |
| `IP1` | Installation inspects the target location before it acts, and never proceeds past an existing installation without explicit, specific approval. |
| `IP2` | Every scenario in [Section 5](#5-supported-installation-scenarios) delivers the identical product architecture; this document states one procedure, with scenario-specific detail confined to how AEOS is obtained and placed. |
| `IP3` | No capability, and no verification outcome, is exclusive to one scenario. |
| `IP4` | Installation fails closed: where a prerequisite is unmet or a state cannot be determined, installation halts and reports rather than proceeding on an assumption. |
| `IP5` | An interruption at any point leaves the target location in a state [Section 11](#11-recovery-procedures) can describe and act on. |
| `IP6` | Installation never modifies or removes a component it did not itself place. |
| `IP7` | A completed installation always reports its own version and origin. |

## 5. Supported Installation Scenarios

This document recognizes exactly four installation scenarios, each corresponding one-to-one with an
official Distribution Method AEOS-PRD Section 15.1 lists. A Distribution Method AEOS-PRD Section 15.2
records as Planned is not yet an installation scenario under this document; [Section 14](#14-non-goals)
records that boundary.

| Scenario | Corresponding Distribution Method | Primary user, per AEOS-PRD Section 15.1 |
| :--- | :--- | :--- |
| **GitHub Clone** | `PR-DST-001` | Contributors, teams standardizing on a pinned revision, users who want full transparency. |
| **Native Installer** | `PR-DST-002` | Developers who want a supported, updatable install with the least setup. |
| **MCP Distribution** | `PR-DST-003` | Users who work primarily inside an AI runtime or IDE and want AEOS available there. |
| **Portable Distribution** | `PR-DST-004` | Locked-down machines, air-gapped environments, ephemeral and shared systems. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-001` | This document recognizes exactly the four scenarios in this section's table, one for each Distribution Method `PR-DST-001` through `PR-DST-004` names. | `PR-DST-001` · `PR-DST-002` · `PR-DST-003` · `PR-DST-004` |
| `INSTALL-002` | Every scenario MUST result in the identical product architecture; a scenario-specific step MUST NOT alter product behavior, capability availability, or terminology. | `PR-DST-005` |
| `INSTALL-003` | No capability MUST be exclusive to one scenario; where two scenarios differ, the difference is confined to how AEOS is obtained and placed, never to what it does once installed. | `PR-DST-006` |

## 6. Installation Prerequisites (Reference Only)

Installation assumes the following are already true before it begins. This document establishes none
of them; it halts and reports if one is unmet, per `INSTALL-005`, and it states each by reference to
the document that owns it, never by restating that document's own content, consistent with the
Ownership Rule AEOS-DOCSTD Section 5.2 states.

| Prerequisite category | Established by | What this document adds |
| :--- | :--- | :--- |
| A host operating system at a supported tier | AEOS-TECH `TC-01`, restated for setup purposes by AEOS-ENVSETUP Section 5 | Nothing; this document names no tier of its own. |
| Software substrate needed to obtain and verify a distribution — for example, a version-control client for the GitHub Clone scenario | AEOS-ENVSETUP Section 6 | Nothing; which item applies is scenario-conditional, per [Section 5](#5-supported-installation-scenarios), and no item beyond AEOS-ENVSETUP's own set is required. |
| Network, storage, permission, and text-encoding conditions | AEOS-ENVSETUP Section 8 | Nothing beyond what AEOS-ENVSETUP already states. |
| Target-location directory and path conditions | AEOS-ENVSETUP Section 13, AEOS-LAYOUT | Nothing; this document introduces no naming convention of its own. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-004` | This document MUST NOT restate the detail of a prerequisite in this section's table; it MUST reference the owning document instead. | AEOS-DOCSTD Section 5.2 |
| `INSTALL-005` | Installation MUST halt and report, without obtaining or placing anything, if a prerequisite referenced in this section is not met. | `PR-SAF-002` |

## 7. Installation Sequence

The sequence below is closed and ordered. Each step's entry condition is the prior step's completion;
no step is reordered, skipped, or parallelized in a way that changes what a later step observes. Only
Step 4 and Step 5 vary by scenario, and each varies only in the manner this section's own sub-tables
state — never in whether the step occurs.

| Step | Action | Entry condition |
| :--- | :--- | :--- |
| 1 | Confirm every prerequisite referenced in [Section 6](#6-installation-prerequisites-reference-only). | None; this is the first step. |
| 2 | Determine the installation scenario: GitHub Clone, Native Installer, MCP Distribution, or Portable Distribution. | Step 1 complete. |
| 3 | Inspect the target location for an existing AEOS installation and report what is found. | Step 2 complete. |
| 4 | Where Step 3 found no existing installation, or found one and explicit, specific approval to proceed has since been given, obtain the AEOS distribution artifact appropriate to the chosen scenario. | Step 3 complete, and, where applicable, approval recorded. |
| 5 | Complete placement or registration of the obtained artifact at the target location, producing a working AEOS installation. | Step 4 complete. |
| 6 | Record the installed version and origin, and make both available to the Developer. | Step 5 complete. |
| 7 | Verify the installation, per [Section 8](#8-verification-after-installation). | Step 6 complete. |
| 8 | Report installation complete, describing the state [Section 9](#9-expected-successful-installation-state) states. | Step 7 complete, with no verification failure. |

### 7.1 Step 3 — Inspection Outcomes

| What Step 3 finds | What installation does |
| :--- | :--- |
| No existing AEOS installation at the target location | Proceed to Step 4 without further confirmation. |
| An existing installation matching what installation would itself produce | Report the finding, propose no change, and stop; Step 4 is not entered except on separate, explicit Developer direction under [Section 12](#12-reinstallation-guidance) or [Section 13](#13-upgrade-considerations). |
| An existing installation differing from what installation would itself produce | Report both states and the difference, and propose reconciliation, including taking no action; Step 4 is not entered without explicit, specific approval. |
| A target-location state that cannot be determined | Halt and report the uncertainty; Step 4 is not entered. |

### 7.2 Step 4 — What Is Obtained, by Scenario

| Scenario | What Step 4 obtains |
| :--- | :--- |
| GitHub Clone | An unmodified copy of the released AEOS repository, already initialized and populated under AEOS-BOOT. |
| Native Installer | The platform-native installer package built for the Developer's host operating system. |
| MCP Distribution | Registration of AEOS as available to the Developer's chosen MCP-capable runtime or client. |
| Portable Distribution | The self-contained, relocatable AEOS distribution, requiring no system-level installation step. |

### 7.3 Step 5 — What Placement Completes, by Scenario

| Scenario | What Step 5 completes |
| :--- | :--- |
| GitHub Clone | The cloned repository exists, intact and unmodified, at the target location. |
| Native Installer | The installer's own procedure finishes and reports success. |
| MCP Distribution | The registration is confirmed reachable from the Developer's runtime or client. |
| Portable Distribution | The distribution is placed at the Developer's chosen location and is executable from it. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-006` | The sequence in this section's table MUST occur in the stated order; a bootstrap-style reordering, skipping, or parallelization that changes what a later step observes MUST NOT be performed. | `PR-NFR-002` |
| `INSTALL-007` | Every step MUST be stated, and performed, as *what* occurs; a step MUST NOT be expressed as an operating-system-specific or scenario-internal command. | `PR-PLT-003` · `PR-PLT-005` |
| `INSTALL-008` | Step 3 MUST occur before Step 4 or Step 5, in every scenario, without exception. | `PR-DST-009` |
| `INSTALL-009` | Where [Section 7.1](#71-step-3--inspection-outcomes) records that Step 3 found a differing existing installation, Step 4 MUST NOT be entered without the explicit, specific approval that row requires. | `PR-DST-009` · `PR-SAF-003` |
| `INSTALL-010` | Where [Section 7.1](#71-step-3--inspection-outcomes) records that Step 3 found a matching existing installation, installation MUST report that finding and propose no change, rather than silently re-running Step 4 or Step 5. | `PR-DST-009` · `PR-NFR-001` |
| `INSTALL-011` | Step 4 is the only step whose concrete action differs by scenario, per [Section 7.2](#72-step-4--what-is-obtained-by-scenario); every other step MUST be identical in kind across every scenario. | `PR-DST-005` |
| `INSTALL-012` | Installation MUST NOT create, populate, or reference Runtime State, a credential, a secret, or a selection of a specific Runtime, Adapter, or Model. | `PR-SAF-006` · `PR-REP-013` · `PR-REP-015` |
| `INSTALL-013` | Installation MUST NOT establish CI/CD configuration. | `PR-REP-007` |
| `INSTALL-014` | Installation MUST NOT perform the repository-initialization procedure AEOS-BOOT states; the GitHub Clone scenario's Step 4 MUST instead obtain an already-bootstrapped repository unmodified. | AEOS-BOOT Section 2.3 |
| `INSTALL-015` | An interruption at any step MUST leave the target location in a state [Section 11](#11-recovery-procedures) can describe and act on. | `PR-SAF-010` |
| `INSTALL-016` | Step 6 MUST complete, and installation MUST NOT report success, until the installed version and origin are recorded and available to the Developer. | `PR-DST-008` |

## 8. Verification After Installation

Verification is read-only. It confirms the state Step 5 and Step 6 of
[Section 7](#7-installation-sequence) produced; it never itself places, modifies, or removes anything.

| Check | Confirms |
| :--- | :--- |
| Version and origin | The version and origin [Section 7](#7-installation-sequence) Step 6 recorded are present and legible to the Developer. |
| Entry point present | The scenario-specific entry point — the cloned repository, the installed application, the MCP registration, or the portable bundle — exists and is reachable. |
| No unintended modification | No component installation did not itself place has been modified or removed. |
| Scenario-appropriate integrity | The obtained artifact matches what [Section 7.2](#72-step-4--what-is-obtained-by-scenario) and [Section 7.3](#73-step-5--what-placement-completes-by-scenario) describe for the chosen scenario. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-017` | Verification MUST be read-only; it MUST NOT modify, create, or remove anything at the target location. | `PR-SAF-005` |
| `INSTALL-018` | Verification MUST confirm that a version and an origin, per `INSTALL-016`, are present and legible. | `PR-DST-008` |
| `INSTALL-019` | Verification MUST confirm that no component installation did not itself place has been modified or removed. | `PR-SAF-009` |
| `INSTALL-020` | What verification checks MUST be identical in kind across every scenario in [Section 5](#5-supported-installation-scenarios); the mechanism by which a check is performed MAY differ by scenario. | `PR-DST-005` |
| `INSTALL-021` | A verification failure MUST be reported; verification MUST NOT silently retry, self-correct, or suppress the finding. | `PR-SAF-002` · `PR-NFR-001` |

## 9. Expected Successful Installation State

Non-normative illustration of an installation immediately after [Section 8](#8-verification-after-installation)
passes:

- A Developer or an AI runtime can determine that AEOS is installed, at what version, from what
  origin, and under which of the four scenarios, per `PR-DST-008`.
- The product architecture is identical to what every other scenario would have produced, per
  `PR-DST-005`; nothing about the Developer's later experience of AEOS depends on which scenario was
  used.
- No pre-existing content at the target location was modified, replaced, or removed without the
  explicit, specific approval [Section 7.1](#71-step-3--inspection-outcomes) requires.
- No Runtime State, credential, secret, or Runtime, Adapter, or Model selection exists as a result of
  installation, per `INSTALL-012`.
- No CI/CD configuration exists as a result of installation, per `INSTALL-013`.
- For the GitHub Clone scenario specifically, the repository is in the state AEOS-BOOT Section 9
  already describes; this section adds nothing to that description and does not restate it.

This is a description of a state, not a procedure: this section states no action of its own.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-022` | An installation for which [Section 8](#8-verification-after-installation) reports no failure is in the state this section describes, and only that state. | `PR-NFR-001` |

## 10. Common Installation Failures

Non-normative. Each row names a symptom [Section 8](#8-verification-after-installation)'s own checks
would surface, not a defect in AEOS.

| Failure | Likely cause | See |
| :--- | :--- | :--- |
| Installation halts at Step 1 | A prerequisite in [Section 6](#6-installation-prerequisites-reference-only) is unmet. | [Section 6](#6-installation-prerequisites-reference-only), AEOS-ENVSETUP |
| Installation halts at Step 3 without proceeding | An existing installation was found and no explicit approval to proceed has yet been given. | [Section 7.1](#71-step-3--inspection-outcomes), [Section 12](#12-reinstallation-guidance) |
| Step 4 does not complete | The obtaining step for the chosen scenario was interrupted — for example, a network transfer did not finish. | [Section 11](#11-recovery-procedures) |
| Step 5 completes but [Section 8](#8-verification-after-installation) reports a failure | The obtained artifact was incomplete or corrupted before placement. | [Section 11](#11-recovery-procedures) |
| Version or origin cannot be determined after Step 6 | Step 6 did not complete before installation was interrupted or reported success prematurely, in violation of `INSTALL-016`. | [Section 11](#11-recovery-procedures) |

## 11. Recovery Procedures

Recovery follows from `IP5`: an interruption at any step of [Section 7](#7-installation-sequence)
leaves the target location in a state this section can describe and act on.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-023` | Recovery MUST begin with [Section 8](#8-verification-after-installation)'s checks, used to determine which step of [Section 7](#7-installation-sequence) last completed. | `PR-SAF-010` |
| `INSTALL-024` | Recovery MUST NOT skip [Section 7](#7-installation-sequence) Step 3's inspection, even when resuming a previously interrupted installation. | `PR-DST-009` |
| `INSTALL-025` | Recovery MUST discard only content this document's own sequence placed; it MUST NOT remove or modify a component that existed before installation began. | `PR-SAF-009` |
| `INSTALL-026` | Where the last completed step cannot be determined, recovery MUST treat the target location as [Section 7.1](#71-step-3--inspection-outcomes) treats a differing existing installation, and MUST NOT proceed without explicit, specific approval. | `PR-SAF-002` · `PR-DST-009` |

Once the last completed step is determined, recovery resumes at the first incomplete step of
[Section 7](#7-installation-sequence)'s sequence; it introduces no step beyond those already stated.

## 12. Reinstallation Guidance

Reinstallation is [Section 7](#7-installation-sequence)'s sequence run again against a target location
where Step 3 finds an existing AEOS installation. It introduces no step this document has not already
stated.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-027` | Reinstallation MUST follow [Section 7](#7-installation-sequence)'s sequence unmodified; it MUST NOT introduce a step this document does not already state. | `PR-NFR-002` |
| `INSTALL-028` | Reinstallation MUST NOT replace existing content at the target location without the explicit, specific confirmation [Section 7.1](#71-step-3--inspection-outcomes) already requires. | `PR-SAF-003` · `PR-DST-009` |
| `INSTALL-029` | Every scenario MUST support a clean removal of its own installation, leaving the target location as though AEOS had never been installed there and disturbing no component installation did not itself place; the scenario-specific mechanism by which removal is carried out is outside this document. | `PR-DST-010` · `PR-SAF-009` |

A Developer MAY choose to remove an existing installation, per `INSTALL-029`, before reinstalling;
doing so is not required, since [Section 7.1](#71-step-3--inspection-outcomes)'s approval discipline
already governs replacement whether or not a prior removal occurred.

## 13. Upgrade Considerations

An upgrade is an installation whose Step 4, per [Section 7.2](#72-step-4--what-is-obtained-by-scenario),
obtains a newer version of AEOS than the one an existing installation already holds.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `INSTALL-030` | An upgrade MUST result in the identical product architecture `PR-DST-005` requires, regardless of which version is replaced. | `PR-DST-005` |
| `INSTALL-031` | An upgrade MUST NOT require any modification to a Developer's own project in order for that project to remain usable. | `PR-DST-007` · `PR-REP-015` |
| `INSTALL-032` | An upgrade MUST follow [Section 7.1](#71-step-3--inspection-outcomes)'s inspect-then-approve discipline identically to [Section 12](#12-reinstallation-guidance)'s reinstallation; it is distinguished only by the version of the artifact Step 4 obtains. | `PR-DST-009` |
| `INSTALL-033` | Completion of an upgrade MUST report the newly installed version and origin, per `INSTALL-016`. | `PR-DST-008` |

Because a Developer's Repository Assets belong to that Developer's own project repository and never to
AEOS's own installation, per AEOS-GLOSSARY's *Repository Asset* and *Runtime State* entries and
`PR-REP-015`, an upgrade of AEOS itself never migrates, inspects, or otherwise touches project content.
The scenario-specific mechanism by which a newer version is obtained — for example, a native
installer's own update facility — is outside this document, per [Section 14](#14-non-goals).

## 14. Non-Goals

This document deliberately does not cover the following. Each is a reasonable expectation this
document does not meet, stated here so a reader does not search for it, or for a future document, in
the wrong place.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Preparing the host machine: required software, environment variables, and directory-level or path prerequisites. | AEOS-ENVSETUP. |
| `NG-2` | The permanent top-level shape and organization of the AEOS repository. | AEOS-LAYOUT. |
| `NG-3` | The ordered procedure that creates, populates, and verifies a new AEOS repository's content and directory structure. | AEOS-BOOT. |
| `NG-4` | Installing, configuring, or launching any AEOS Runtime, Adapter, or Model. | Runtime documents and future Implementation Guides for those layers. |
| `NG-5` | Initializing or adopting a Developer's own project once AEOS is installed. | `PR-PRJ`, and a future Implementation or Developer Guide. |
| `NG-6` | Any structural or architectural decision about how AEOS itself is built. | AEOS-ARCH, AEOS-BLUEPRINT. |
| `NG-7` | Selecting a technology beyond what AEOS-TECH already recognizes. | AEOS-TECH. |
| `NG-8` | Configuring a project's CI/CD systems. | The project's own existing systems, per `PR-REP-007`. |
| `NG-9` | Establishing credentials, secrets, or a Runtime, Adapter, or Model selection. | The Developer, outside this procedure. |
| `NG-10` | The exact, scenario-specific mechanism by which obtaining, placement, removal, or update occurs — commands, installer internals, or packaging format. | Release Engineering, and each Distribution Method's own build and packaging process. |
| `NG-11` | Package Manager, Docker Image, or IDE Marketplace distribution. | Not yet official, per AEOS-PRD Section 15.2; reserved to a future revision of this document once a Distribution Method is promoted to official. |
| `NG-12` | Correcting an installation found to diverge from [Section 9](#9-expected-successful-installation-state) by any means other than [Sections 11](#11-recovery-procedures) through [13](#13-upgrade-considerations). | Those sections; this is not a distinct, unstated procedure. |

## 15. Traceability

Every `INSTALL-` rule states its own trace inline, in
[Sections 5](#5-supported-installation-scenarios) through [13](#13-upgrade-considerations). This
section consolidates that trace by target.

### 15.1 Product Requirements

| `PR-` identifier | `INSTALL-` rules that trace to it |
| :--- | :--- |
| `PR-DST-001` | `001` |
| `PR-DST-002` | `001` |
| `PR-DST-003` | `001` |
| `PR-DST-004` | `001` |
| `PR-DST-005` | `002` · `011` · `020` · `030` |
| `PR-DST-006` | `003` |
| `PR-DST-007` | `031` |
| `PR-DST-008` | `016` · `018` · `033` |
| `PR-DST-009` | `008` · `009` · `010` · `024` · `026` · `028` · `032` |
| `PR-DST-010` | `029` |
| `PR-SAF-002` | `005` · `021` · `026` |
| `PR-SAF-003` | `009` · `028` |
| `PR-SAF-005` | `017` |
| `PR-SAF-006` | `012` |
| `PR-SAF-009` | `019` · `025` · `029` |
| `PR-SAF-010` | `015` · `023` |
| `PR-REP-007` | `013` |
| `PR-REP-013` | `012` |
| `PR-REP-015` | `012` · `031` |
| `PR-PLT-003` | `007` |
| `PR-PLT-005` | `007` |
| `PR-NFR-001` | `010` · `021` · `022` |
| `PR-NFR-002` | `006` · `027` |

### 15.2 AEOS-BOOT, AEOS-ENVSETUP, and AEOS-LAYOUT

| Document rule or section | `INSTALL-` rules that trace to it |
| :--- | :--- |
| AEOS-BOOT Section 2.3 (applicability; not re-run) | `014` |
| AEOS-BOOT Section 9 (expected repository state) | Referenced non-normatively in [Section 9](#9-expected-successful-installation-state); not separately traced. |
| AEOS-ENVSETUP Sections 5, 6, 8, 13 | Referenced by [Section 6](#6-installation-prerequisites-reference-only); not separately traced, per `INSTALL-004`. |
| AEOS-LAYOUT | Referenced by [Section 6](#6-installation-prerequisites-reference-only); not separately traced, per `INSTALL-004`. |

## 16. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-VISION | Purpose and enduring intent this document takes no position against. |
| AEOS-PRD Section 15 (Distribution Strategy) | The four official Distribution Methods, the Distribution invariants, and the `PR-DST` identifiers this document's rules trace to. |
| AEOS-PRD `PR-SAF`, `PR-REP`, `PR-PLT`, `PR-NFR` | The supporting requirement families [Section 15](#15-traceability) traces `INSTALL-` rules to. |
| AEOS-GLOSSARY | The terms this document uses without redefinition, including *Developer*, *Distribution Method*, *Repository Asset*, and *Runtime State*. |
| AEOS-DOCSTD Section 4.1, 4.3, 5.2 | The Implementation Guide layer's position and purpose, and the Ownership Rule, this document is written under. |
| AEOS-TECH `TC-01` | The host operating system tiers [Section 6](#6-installation-prerequisites-reference-only) references without restating. |
| AEOS-BOOT | The repository-initialization procedure the GitHub Clone scenario's Step 4 assumes already occurred. |
| AEOS-ENVSETUP | The machine-preparation prerequisites [Section 6](#6-installation-prerequisites-reference-only) references without restating. |
| AEOS-LAYOUT | The repository shape [Section 6](#6-installation-prerequisites-reference-only) references without restating. |

## 17. Document Governance

### 17.1 Status

This document is a **Draft**. It is the first Installation Guide authored for AEOS, and is intended to
become the Installation Source of Truth once the owner's review under
[Section 17.4](#174-review-policy) records no Critical or Major finding, at which point it is intended
to be placed and frozen alongside AEOS-BOOT and AEOS-ENVSETUP in `docs/implementation/`.

### 17.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `INSTALL-<NNN>` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Promotion of a Planned Distribution Method to a recognized installation scenario, once AEOS-PRD Section 15.2 promotes it to official | Explicit owner revision request | Minor |
| Any change to what an existing `INSTALL-<NNN>` rule requires, or its retirement | Explicit owner revision request | Major |
| Change to [Section 7](#7-installation-sequence)'s sequence, or to the set of recognized scenarios in [Section 5](#5-supported-installation-scenarios) | Explicit owner revision request | Major |
| Any change that would invalidate an installation already completed under a prior version | Explicit owner revision request, with the reasoning preserved in place | Major |

### 17.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-DOCSTD, AEOS-TECH, AEOS-BOOT,
AEOS-ENVSETUP, or AEOS-LAYOUT are recorded as recommendations for the owning document's governance and
are applied only after explicit owner approval there — never enacted here.

### 17.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in [Sections 5](#5-supported-installation-scenarios) through
      [13](#13-upgrade-considerations) carries an `INSTALL-<NNN>` identifier and a trace.
- [ ] No rule restates the content of AEOS-BOOT, AEOS-ENVSETUP, AEOS-LAYOUT, or AEOS-TECH.
- [ ] No rule states an environment-preparation step, a repository-organization rule, a runtime
      behavior, an architectural decision, or a technology choice absent from AEOS-TECH.
- [ ] No installation scenario is named that AEOS-PRD Section 15.1 does not list as official.
- [ ] All seventeen numbered sections in this document's Table of Contents are present, in order, and
      none is silently empty.
- [ ] No Critical or Major finding remains open.

### 17.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, or AEOS-TECH | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-BOOT on repository content, structure, or initialization | AEOS-BOOT governs. This document's statement is corrected to reference it rather than restate it. |
| This document conflicts with AEOS-ENVSETUP on a machine-level prerequisite | AEOS-ENVSETUP governs, for the same reason. |
| This document conflicts with AEOS-LAYOUT on a path or naming convention | AEOS-LAYOUT governs, for the same reason. |
| A future Implementation Guide or Developer Guide states an installation-time convention that conflicts with [Section 7](#7-installation-sequence) | The apparent need is reported against this document under [Section 17.2](#172-change-control). It is not resolved by a contradictory statement in the other guide. |

### 17.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Installation Guide. States four recognized installation scenarios, one per official Distribution Method; a reference-only prerequisites section depending on AEOS-ENVSETUP and AEOS-TECH; an eight-step installation sequence with inspection preceding every scenario's obtaining and placement steps; a read-only verification procedure; the expected successful installation state; common installation failures; recovery procedures for an interrupted installation; reinstallation guidance, including the clean-removal principle; upgrade considerations preserving project independence from AEOS's own version; twelve non-goals; and thirty-three `INSTALL-<NNN>` rules in total. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, no environment-preparation step, and no repository-organization or repository-initialization procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-BOOT, AEOS-ENVSETUP, or AEOS-LAYOUT. |

---

## Appendix A — Installation Checklist (Non-Normative)

A practical restatement of [Section 7](#7-installation-sequence) and
[Section 8](#8-verification-after-installation), for a Developer or an AI runtime working through
installation directly. This checklist carries no authority of its own; where it appears to diverge
from Sections 5 through 13, those sections govern.

- [ ] Prerequisites confirmed ([Section 6](#6-installation-prerequisites-reference-only)).
- [ ] Installation scenario chosen (GitHub Clone, Native Installer, MCP Distribution, or Portable
      Distribution).
- [ ] Target location inspected; existing-installation outcome recorded
      ([Section 7.1](#71-step-3--inspection-outcomes)).
- [ ] Where an existing installation was found and differs, explicit approval obtained before
      proceeding.
- [ ] Distribution artifact obtained, per the chosen scenario
      ([Section 7.2](#72-step-4--what-is-obtained-by-scenario)).
- [ ] Placement or registration completed, per the chosen scenario
      ([Section 7.3](#73-step-5--what-placement-completes-by-scenario)).
- [ ] Version and origin recorded and available to the Developer.
- [ ] Every check in [Section 8](#8-verification-after-installation) passes.
- [ ] Installation reported complete.

## Appendix B — INSTALL Rule Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 5](#5-supported-installation-scenarios) through [13](#13-upgrade-considerations).

| ID | Section | Short label | Traces to |
| :--- | :--- | :--- | :--- |
| `INSTALL-001` | 5 | Four scenarios recognized, one per Distribution Method | `PR-DST-001` · `PR-DST-002` · `PR-DST-003` · `PR-DST-004` |
| `INSTALL-002` | 5 | Identical product architecture across scenarios | `PR-DST-005` |
| `INSTALL-003` | 5 | No capability exclusive to a scenario | `PR-DST-006` |
| `INSTALL-004` | 6 | Prerequisites referenced, never restated | AEOS-DOCSTD Section 5.2 |
| `INSTALL-005` | 6 | Halt and report on unmet prerequisite | `PR-SAF-002` |
| `INSTALL-006` | 7 | Sequence order fixed | `PR-NFR-002` |
| `INSTALL-007` | 7 | Every step stated as what, not as a command | `PR-PLT-003` · `PR-PLT-005` |
| `INSTALL-008` | 7 | Inspection precedes obtaining and placement | `PR-DST-009` |
| `INSTALL-009` | 7 | No proceed past a differing installation without approval | `PR-DST-009` · `PR-SAF-003` |
| `INSTALL-010` | 7 | Matching installation reported, no change proposed | `PR-DST-009` · `PR-NFR-001` |
| `INSTALL-011` | 7 | Only obtaining differs by scenario | `PR-DST-005` |
| `INSTALL-012` | 7 | No Runtime State, credential, or selection | `PR-SAF-006` · `PR-REP-013` · `PR-REP-015` |
| `INSTALL-013` | 7 | No CI/CD configuration established | `PR-REP-007` |
| `INSTALL-014` | 7 | No repository-initialization performed | AEOS-BOOT Section 2.3 |
| `INSTALL-015` | 7 | Interruption leaves a describable state | `PR-SAF-010` |
| `INSTALL-016` | 7 | Version and origin recorded before success reported | `PR-DST-008` |
| `INSTALL-017` | 8 | Verification is read-only | `PR-SAF-005` |
| `INSTALL-018` | 8 | Verify version and origin present | `PR-DST-008` |
| `INSTALL-019` | 8 | Verify no unintended modification | `PR-SAF-009` |
| `INSTALL-020` | 8 | Checks identical in kind across scenarios | `PR-DST-005` |
| `INSTALL-021` | 8 | Failure reported, never silently retried | `PR-SAF-002` · `PR-NFR-001` |
| `INSTALL-022` | 9 | Passing verification defines the expected state | `PR-NFR-001` |
| `INSTALL-023` | 11 | Recovery begins with verification | `PR-SAF-010` |
| `INSTALL-024` | 11 | Recovery does not skip inspection | `PR-DST-009` |
| `INSTALL-025` | 11 | Recovery discards only what this procedure placed | `PR-SAF-009` |
| `INSTALL-026` | 11 | Undetermined state treated as a differing installation | `PR-SAF-002` · `PR-DST-009` |
| `INSTALL-027` | 12 | Reinstallation introduces no new step | `PR-NFR-002` |
| `INSTALL-028` | 12 | No replacement without explicit confirmation | `PR-SAF-003` · `PR-DST-009` |
| `INSTALL-029` | 12 | Clean removal supported; mechanism out of scope | `PR-DST-010` · `PR-SAF-009` |
| `INSTALL-030` | 13 | Upgrade preserves identical architecture | `PR-DST-005` |
| `INSTALL-031` | 13 | Upgrade never requires project modification | `PR-DST-007` · `PR-REP-015` |
| `INSTALL-032` | 13 | Upgrade follows inspect-then-approve discipline | `PR-DST-009` |
| `INSTALL-033` | 13 | Upgrade completion reports new version and origin | `PR-DST-008` |

---

**End of Installation Guide**

AEOS-INSTALL · Version 1.0.0 · Traces to `PR-DST` · `PR-SAF` · `PR-NFR` · `PR-REP` · `PR-PLT`,
referencing AEOS-VISION · AEOS-PRD · AEOS-GLOSSARY · AEOS-DOCSTD · AEOS-TECH · AEOS-BOOT ·
AEOS-ENVSETUP · AEOS-LAYOUT without restating any of them
