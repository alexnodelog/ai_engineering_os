# AI Engineering Operating System

## AEOS — Environment Setup Guide

*The permanent statement of what a host machine must provide before AEOS-BOOT's own prerequisites
are attempted, and before a Contributor performs any other work in the AEOS repository.*

| Field | Value |
| :--- | :--- |
| **Document** | Environment Setup Guide |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-ENVSETUP |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Contributors, release engineers, maintainers, and AI runtimes preparing a machine to build, test, or contribute to AEOS |
| **Suggested path** | `docs/implementation/ENVIRONMENT_SETUP.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states, precisely and reproducibly, **the environment a host machine must provide
> before AEOS-BOOT's own prerequisites are attempted, and before a Contributor performs any other
> work in the AEOS repository**: the host operating systems this guide provides setup guidance for,
> the software a prepared machine has installed and at what version currency, the environment
> variables a prepared machine resolves, the directory-level conditions a prepared machine satisfies,
> the checks that confirm the result, and the state a successfully prepared machine is in.
>
> This document is an **Implementation Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that
> layer: concrete guidance for building to specification, expressed at the point specification meets
> construction. It is not a Product document, not a Vision document, not an Architecture document,
> not a Blueprint, not a Runtime document, and not a behavioral Specification under AEOS-SPECSTD. It
> states no product requirement, no architectural decision, no Blueprint arrangement, no specified
> behavior, no runtime lifecycle, and no terminology; where a statement here appears to do any of
> these, that is a defect in this document and MUST be reported rather than acted upon. It states no
> repository-initialization procedure — that is AEOS-BOOT's subject — and no repository structure
> beyond what AEOS-BOOT and AEOS-LAYOUT already state; it does not restate the content of either
> document.
>
> **On the word "environment."** This document uses *environment* in its ordinary sense — the host
> machine and the software installed on it — and does not redefine or restate AEOS-GLOSSARY's
> *Environment* entry, which names what AEOS itself inspects at runtime under `PR-ENV` once
> Implementation exists. The two are related — this document prepares the substrate that capability
> will later inspect — but they are not the same subject, and this document grants no `PR-ENV`
> capability and performs no inspection on AEOS's behalf.
>
> AEOS-DOCSTD Section 4.1 positions Implementation Guides beneath Specification and above Developer
> Guides in the documentation hierarchy. This document is written under AEOS-DOCSTD's general
> document template and the Section 4.3 purpose statement for this layer, in the absence of a
> dedicated Implementation Guide Standard — in the same spirit AEOS-BOOT and AEOS-LAYOUT record for
> their own comparable position. It does not, on that account, establish such a Standard.
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
4. [Environment Setup Principles](#4-environment-setup-principles)
5. [Supported Host Operating Systems](#5-supported-host-operating-systems)
6. [Required Software](#6-required-software)
7. [Required Versions](#7-required-versions)
8. [Environment Prerequisites](#8-environment-prerequisites)
9. [Development Tools](#9-development-tools)
10. [IDE and Editor Recommendations](#10-ide-and-editor-recommendations)
11. [Required Environment Variables](#11-required-environment-variables)
12. [Optional Environment Variables](#12-optional-environment-variables)
13. [Directory Prerequisites](#13-directory-prerequisites)
14. [Verification Steps](#14-verification-steps)
15. [Expected Successful Environment State](#15-expected-successful-environment-state)
16. [Common Setup Failures](#16-common-setup-failures)
17. [Troubleshooting Guidance](#17-troubleshooting-guidance)
18. [Non-Goals](#18-non-goals)
19. [Traceability](#19-traceability)
20. [References](#20-references)
21. [Document Governance](#21-document-governance)
22. [Appendix A — Verification Checklist (Non-Normative)](#appendix-a--verification-checklist-non-normative)
23. [Appendix B — SETUP Rule Index (Non-Normative)](#appendix-b--setup-rule-index-non-normative)

---

## 1. Executive Summary

AEOS-BOOT states, precisely, how a new AEOS repository is initialized — but it begins from a
Prerequisite it does not itself establish: a machine capable of creating UTF-8 text files and
directories, and, where the chosen Distribution Method requires it, a version-control system already
present. AEOS-BOOT halts and reports if that Prerequisite is missing; it does not prepare the machine
to meet it. Something has to state, once, what "prepared" means before Bootstrap is ever attempted —
and what a Contributor needs installed to do anything past that point: write code, run a test, use a
package manager, open an editor. This document is that statement.

It closes the gap between an arbitrary machine and one ready for AEOS-BOOT's Prerequisite 3 and
Prerequisite 4, and ready for the ordinary work of contributing to AEOS once the AEOS repository
exists on it. It draws every software item it names from AEOS-TECH's already-governed technology
set, introduces none of its own, and states no exact version pin — version currency is a policy this
document applies, not a number it fixes, so that this document does not require revision every time a
supplier ships a new release. It states nothing about repository structure that AEOS-BOOT and
AEOS-LAYOUT do not already state, and nothing about how AEOS behaves once it runs, which remains
Runtime-layer and product-layer territory.

Four properties bind this document, consistent with the discipline AEOS-BOOT and AEOS-LAYOUT record
for a comparable statement of fact:

| Property | What it requires of this document |
| :--- | :--- |
| **Reproducible** | Given the same host operating system and the same software named in [Section 6](#6-required-software), the verification in [Section 14](#14-verification-steps) produces the same result every time it is performed. |
| **Deterministic** | [Section 14](#14-verification-steps)'s checks do not depend on a choice this document leaves unstated. |
| **Platform-neutral, whenever possible** | Guidance is stated as *what* a prepared machine has, never as an operating-system-specific command, except where [Section 16](#16-common-setup-failures) and [Section 17](#17-troubleshooting-guidance) illustrate a platform-specific symptom non-normatively. |
| **Non-invasive** | This document never directs the removal, reconfiguration, or replacement of software a Contributor's machine already has; it reports what is missing or below the version policy in [Section 7](#7-required-versions) and lets the Contributor decide how to supply it. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the preparation of a host machine for AEOS development, and nothing beyond it:

- the host operating systems this guide provides setup guidance for;
- the software a prepared machine has installed, drawn entirely from AEOS-TECH's recognized set;
- the version-currency policy a prepared machine's software satisfies;
- environmental conditions — network, storage, permissions, encoding — a prepared machine meets;
- development tools and editor options available to a Contributor, without requiring any one of them;
- the environment variables a prepared machine resolves, required and optional;
- directory-level and path conditions a prepared machine satisfies before AEOS-BOOT is attempted;
- the checks by which a prepared machine is verified;
- the state a successfully prepared machine is in;
- common failure modes and where to resolve them;
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
| How each defined behavior must work, precisely and testably | Specification documents |
| How AEOS inspects and reports on a machine once it is running (`PR-ENV`) | Runtime documents and, once implemented, AEOS itself |
| The ordered, reproducible procedure that creates, populates, and verifies a new AEOS repository | AEOS-BOOT |
| The permanent top-level shape of the AEOS repository | AEOS-LAYOUT |
| How a person works day to day within an already-bootstrapped repository | Developer Guides, none of which yet exist for AEOS |
| Selecting or configuring an AI Runtime, a Model, or a credential | The Contributor, and Runtime documents, once they exist |
| Code, algorithms, dependency selection, and version pins | The codebase and its own manifests and lockfiles, per AEOS-TECH Section 2.2 and `TG-083` |

A statement in this document that states a repository-initialization procedure, a runtime behavior,
an architectural decision, a product requirement, or a technology choice absent from AEOS-TECH is a
**defect in this document**. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to a machine before the AEOS repository exists on it, and to the software a
Contributor's ongoing work on AEOS depends on afterward. It is consulted once per machine, ordinarily,
and re-consulted whenever [Section 14](#14-verification-steps) is re-run to confirm a machine remains
prepared. It applies identically to a human Contributor and to an AI runtime performing setup on a
human's behalf, consistent with AEOS-DOCSTD Section 2.4.

This document concerns preparing a machine to build, test, and contribute to **AEOS itself** — the
Contributor's use, per AEOS-GLOSSARY's distinction between *Contributor* and *Developer*. It does not
state how a Developer prepares a machine to use AEOS, once built, to build their own project; that is
a Developer Guide's subject, none of which yet exists for AEOS.

## 3. Relationship to Other Documents

| Document | Governs | This document's obligation |
| :--- | :--- | :--- |
| **AEOS-VISION** | Purpose and enduring intent. | This document takes no position on why AEOS exists. No rule below may require trading away an AEOS-VISION invariant. |
| **AEOS-PRD** | Product definition and the `PR-` requirements. | Every `SETUP-` rule that binds a party traces to one or more `PR-` identifiers already established there; this document adds no new requirement. |
| **AEOS-GLOSSARY** | Terminology. | This document uses *Contributor*, *Platform*, *Tool*, *Repository*, *Distribution Method*, and *Environment* exactly as AEOS-GLOSSARY defines them, disambiguating its own use of "environment" in the authority statement above rather than redefining the term. |
| **AEOS-DOCSTD** | Documentation form and lifecycle. | This document's structure, format, normative vocabulary, and template follow it without exception. |
| **AEOS-TECH** | Recognized technologies and their support tiers. | Every software item this document requires or recommends is drawn from AEOS-TECH's recognized categories and tiers; this document introduces no technology of its own and states no exact version pin, consistent with `TG-060` and `TG-083`. |
| **AEOS-BOOT** | The ordered procedure that initializes a new AEOS repository. | This document prepares the host machine so that AEOS-BOOT's own Prerequisites can be attempted; it does not restate AEOS-BOOT's sequence, directory structure, or document placement, and is not a substitute for it. |
| **AEOS-LAYOUT** | The permanent shape of the AEOS repository. | This document's directory-level and path conditions are stated consistently with AEOS-LAYOUT's naming conventions and repository-root rules; it names no top-level entry AEOS-LAYOUT does not already name. |

## 4. Environment Setup Principles

These principles constrain every rule in this document. A rule that violates one is defective, not
merely unconventional.

| # | Principle |
| :--- | :--- |
| `ESP1` | A prepared machine is defined by what it has, never by the sequence of commands used to reach that state; two machines prepared differently are equally conformant if [Section 14](#14-verification-steps) passes on both. |
| `ESP2` | Every software item this document names is drawn from AEOS-TECH's recognized set; this document invents no technology and narrows no tier AEOS-TECH already assigns. |
| `ESP3` | Version currency is stated as a policy against a supplier's own support lifecycle, never as a pinned number, so that this document does not require revision on every release. |
| `ESP4` | This document never directs modifying, reconfiguring, or removing software a Contributor's machine already has; a difference from expectation is reported, not silently corrected. |
| `ESP5` | Every condition this document states is independently checkable; a condition no reviewer could verify is not a rule here. |
| `ESP6` | A gap this document has not yet decided is recorded as a non-goal, per [Section 18](#18-non-goals), and left open rather than filled by assumption. |

## 5. Supported Host Operating Systems

This document provides setup guidance for the host operating system tiers AEOS-TECH `TC-01` records
at Officially Supported and Conditionally Supported. It restates neither tier's rationale; where this
table and AEOS-TECH Section 8.2 differ, AEOS-TECH governs.

| Host operating system | AEOS-TECH tier | Guidance in this document |
| :--- | :--- | :--- |
| Windows | Officially Supported | Full guidance in [Sections 6](#6-required-software)–[17](#17-troubleshooting-guidance) applies directly. |
| macOS | Officially Supported | Full guidance in [Sections 6](#6-required-software)–[17](#17-troubleshooting-guidance) applies directly. |
| Linux, stable-release distributions | Officially Supported | Full guidance in [Sections 6](#6-required-software)–[17](#17-troubleshooting-guidance) applies directly. |
| Rolling-release Linux | Conditionally Supported | Same guidance applies; the Contributor accepts a faster-moving baseline than [Section 14](#14-verification-steps) was verified against, per AEOS-TECH's stated condition. |
| WSL2 | Conditionally Supported, as a Linux environment | Guidance for Linux applies inside the WSL2 instance; it MUST NOT substitute for native Windows verification. |
| BSD family | Experimental | Not covered. No verified guidance exists in this document. |
| Operating system releases past supplier end-of-life | Not Recommended | Excluded from this document's guidance; `SETUP-001` applies. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-001` | This document provides setup guidance only for a host operating system at the Officially Supported or Conditionally Supported tier of AEOS-TECH `TC-01`, at a release still receiving supplier security support. | AEOS-TECH `TG-062` · `PR-PLT-001` |
| `SETUP-002` | Where a host operating system meets Officially Supported or Conditionally Supported at the platform level but a specific release has reached supplier end-of-life, that release is out of scope for this document regardless of the platform's own tier. | AEOS-TECH `TG-062` |
| `SETUP-003` | Guidance in this document MUST produce identical observable software requirements on Windows, macOS, and Linux; a requirement stated for one and not the others is a defect in this document. | `PR-PLT-002` · `PR-PLT-003` |

## 6. Required Software

Every item below is drawn from AEOS-TECH's recognized categories. "Required" means a prepared
machine has it regardless of the Contributor's specific task; "Conditional" means it is required only
where the Contributor's work depends on it.

| Software | AEOS-TECH category and tier | Requirement level | Purpose |
| :--- | :--- | :--- | :--- |
| A Git-compatible version-control client | `TC-18`, Preferred (Git) | Required where the chosen Distribution Method depends on version control — ordinarily `PR-DST-001`'s GitHub Clone | Obtains and records changes to the AEOS repository. |
| A Python interpreter | `TC-03`, Preferred and Officially Supported | Required | Executes AEOS's Python-language work and its Preferred package manager, `uv`. |
| A JavaScript engine capable of executing the Preferred TypeScript toolchain and the Preferred `pnpm` package manager | Substrate for `TC-03` TypeScript and `TC-07` `pnpm` | Required | Node.js is the common choice; this document names no other requirement of the engine beyond executing those two Preferred technologies. |
| `uv` | `TC-07`, Preferred (Python) | Required | Python dependency and virtual-environment management with a deterministic, offline-capable lockfile. |
| `pnpm` | `TC-07`, Preferred (JavaScript and TypeScript) | Required | JavaScript and TypeScript dependency management with a deterministic, offline-capable lockfile. |
| Docker and Docker Compose | `TC-08`, Officially Supported | Conditional | Required only where the Contributor's work depends on a containerized service; AEOS itself imposes no container dependency. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-004` | A prepared machine MUST have every item in this section marked Required; an item marked Conditional MUST be present only where the Contributor's work depends on it. | `PR-NFR-002` |
| `SETUP-005` | This document MUST NOT require a technology absent from AEOS-TECH's recognized set, and MUST NOT require a technology below the Conditionally Supported tier for the purpose stated. | AEOS-TECH `TG-024` · `TG-082` |
| `SETUP-006` | Where AEOS-TECH records a Preferred entry within a category this section draws from, this section's Required or Conditional item MUST be that Preferred entry, unless no Preferred entry exists for the category, in which case the Officially Supported set governs. | AEOS-TECH Section 6 |

## 7. Required Versions

AEOS-TECH `TG-060` requires that technology support be stated as a minimum version and a version
range, and prohibits stating an exact pin — pinning is realized in the AEOS repository's own
manifests and lockfiles once Implementation exists, per `TG-083`, not in this document. This section
states the policy a prepared machine's software satisfies; it states no number that would go stale
on a supplier's next release.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-007` | The installed version of every item in [Section 6](#6-required-software) MUST currently be receiving supplier security support, and MUST NOT be a version that has reached supplier end-of-life. | AEOS-TECH `TG-062` |
| `SETUP-008` | Where an item in [Section 6](#6-required-software) offers a Long-Term-Support or stable release channel, that channel SHOULD be installed in preference to a shorter-lived channel, consistent with AEOS-TECH `TG-061`. | AEOS-TECH `TG-061` |
| `SETUP-009` | This document MUST NOT state an exact version pin for any item in [Section 6](#6-required-software); a Contributor determines the currently qualifying version from that technology's own supplier-published support lifecycle at the time of setup. | AEOS-TECH `TG-060` |
| `SETUP-010` | A new major version of an item in [Section 6](#6-required-software) satisfies `SETUP-007` once AEOS-TECH's own evaluation under `TG-064` admits it; this document does not independently evaluate or admit a major version. | AEOS-TECH `TG-064` |

> **Where to check.** Non-normative. Each supplier publishes its own current support lifecycle —
> for example, a language's release-and-support schedule, or a package manager's release notes. This
> document does not reproduce any of them, both because they change on a schedule this document does
> not control and because AEOS-TECH Section 2.2 already assigns technology-specific detail to the
> technology's own documentation rather than to AEOS.

## 8. Environment Prerequisites

Conditions the host machine itself satisfies, independent of any single software item.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-011` | The machine MUST have network access sufficient to download every item in [Section 6](#6-required-software) at least once; no item in [Section 6](#6-required-software) MUST require network access to run once installed. | AEOS-TECH Section 5, offline capability |
| `SETUP-012` | The machine MUST be capable of creating and reading UTF-8 encoded text files and directories, consistent with AEOS-BOOT's own Prerequisite 3. | AEOS-BOOT Prerequisite 3 |
| `SETUP-013` | The Contributor MUST have write permission at the location where the AEOS repository will be placed, before AEOS-BOOT Prerequisite 1 is attempted. | `PR-SAF-009` |
| `SETUP-014` | The machine MUST provide a shell or terminal capable of invoking every installed item in [Section 6](#6-required-software) by name, without requiring an editor. | AEOS-TECH `TC-02`, terminal entry |
| `SETUP-015` | Every tool the Contributor uses to read or write a path in the AEOS repository MUST treat that path as case-sensitive, even where the host file system is not, consistent with AEOS-LAYOUT `LAYOUT-008`. | AEOS-LAYOUT `LAYOUT-008` |
| `SETUP-016` | The location chosen for the AEOS repository MUST NOT contain a space in its path, consistent with AEOS-LAYOUT `LAYOUT-009`. | AEOS-LAYOUT `LAYOUT-009` |

This document states no minimum storage figure: a specific number would either be too small for some
combination of items in [Section 6](#6-required-software) or stale the moment one of them changes its
own footprint, and either failure mode is worse than stating none. A Contributor confirms sufficient
space is available for the items they install and for a clone of the AEOS repository, non-normatively.

## 9. Development Tools

Beyond the Required and Conditional software in [Section 6](#6-required-software), the following are
available to a Contributor and are drawn from AEOS-TECH without this document requiring any of them
individually.

| Category | AEOS-TECH reference | This document's position |
| :--- | :--- | :--- |
| Testing frameworks | `TC-15` — `pytest`, Vitest, Playwright Preferred; Jest Officially Supported | Installed per-project through `uv` and `pnpm` once the AEOS repository exists; no separate machine-level installation is required by this document. |
| Dependency and secret auditing | `TC-20` — package-manager auditing, gitleaks, Trivy Officially Supported | Recommended, not required, at setup time. A Contributor MAY install these before first use. |
| Version-control hosting client | `TC-18` — GitHub Officially Supported for hosting | Using a specific host's client or web interface is a Contributor convenience; the Git client in [Section 6](#6-required-software) is sufficient on its own. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-017` | A Contributor's machine having a development tool from this section installed or absent MUST NOT change what [Section 14](#14-verification-steps) checks or what [Section 15](#15-expected-successful-environment-state) describes. | `PR-NFR-012` |
| `SETUP-018` | Where the Contributor intends to make a commit against the AEOS repository, the version-control client in [Section 6](#6-required-software) MUST be configured with an identity (name and a contact address) the client's own convention requires before AEOS-BOOT's Step 11 is attempted. | AEOS-BOOT Section 7, Step 11 |

## 10. IDE and Editor Recommendations

AEOS-TECH `TC-02` assigns no Preferred editor by policy, and this document does not either: editor
choice is a matter of Contributor preference, and the terminal entry in that category is deliberately
first-class so no editor is ever required.

| Tier | Editors | This document's position |
| :--- | :--- | :--- |
| Officially Supported | Cursor · Terminal or shell with no editor · Visual Studio Code · Windsurf | Any of these, or none, is sufficient for the work this document prepares a machine for. |
| Conditionally Supported | Emacs · GitHub Copilot, as an assistant within a supported editor · JetBrains IDEs · Neovim · Zed | Usable with documentation-only support, per AEOS-TECH's stated condition. |
| Not Recommended | An environment that requires Rules, Workflows, Prompts, or project state to live in editor-owned configuration | Excluded from this document's recommendations, consistent with AEOS-TECH's rationale. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-019` | This document MUST NOT require the installation of any editor. | AEOS-TECH `TC-02`, Preferred: none by policy |
| `SETUP-020` | Where a Contributor selects an editor, [Section 14](#14-verification-steps) MUST NOT depend on which one, or on none being selected. | `PR-NFR-012` |

## 11. Required Environment Variables

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-021` | The executable for every Required item in [Section 6](#6-required-software) MUST resolve on the shell's `PATH` without a fully qualified path being supplied at each invocation. | `SETUP-014` |
| `SETUP-022` | This document defines no AEOS-specific required environment variable. No Runtime, Adapter, or Model selection, and no credential, is part of environment setup. | `PR-SAF-006` · `PR-REP-013`, consistent with AEOS-BOOT `BOOT-011` |

`SETUP-022` is stated rather than the section being omitted because AEOS-DOCSTD `TP4` permits omission
only where a section has nothing to say; here, the sparse requirement — `PATH` resolution and nothing
AEOS-specific — is itself the substantive fact a reader needs, consistent with the reporting discipline
AEOS-SPECSTD `RS13` applies to its own layer.

## 12. Optional Environment Variables

None of the variables below is defined or owned by AEOS. Each is owned by the tool it configures, per
AEOS-LAYOUT `LAYOUT-024`'s principle that tooling configuration is placed and named at its owning
tool's own convention; this document only notes each variable's relevance to the items in
[Section 6](#6-required-software).

| Variable | Owning tool | Relevance |
| :--- | :--- | :--- |
| `UV_*` family | `uv` | Cache location, index configuration, and comparable behavior `uv`'s own documentation defines. |
| `PNPM_HOME` | `pnpm` | Global store and shim location `pnpm`'s own documentation defines. |
| `NODE_OPTIONS` | The JavaScript engine | Engine flags the Contributor's own tooling may need. |
| `NO_COLOR` / `FORCE_COLOR` | Terminal output convention | Adjusts colored output from installed tools; a Contributor preference, not a requirement. |
| `HTTP_PROXY` / `HTTPS_PROXY` / `NO_PROXY` | Network stack | Relevant only where [Section 8](#8-environment-prerequisites)'s network prerequisite is met through a constrained or proxied network. |

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-023` | This document MUST NOT assign a value to any variable in this section; it states relevance only, and defers to each variable's owning tool for meaning and default. | AEOS-LAYOUT `LAYOUT-024` |
| `SETUP-024` | An AI provider credential — including a key for a provider AEOS-TECH `TC-09` recognizes — MUST NOT be configured as part of environment setup under this document. Credential and Runtime selection belong to Runtime documents and to the Contributor, once those documents exist. | `PR-SAF-006` · `PR-SAF-007`, consistent with AEOS-BOOT `BOOT-011` |

## 13. Directory Prerequisites

This section states conditions on the host machine's file system that hold before AEOS-BOOT
Prerequisite 1 is attempted. It fixes no directory name AEOS-BOOT or AEOS-LAYOUT has not already
fixed, and in particular states no source-code or test-code directory name, consistent with
AEOS-LAYOUT Non-Goal `NG-1`.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-025` | The location the Contributor chooses for the AEOS repository MUST satisfy AEOS-BOOT Prerequisite 1 — empty, or containing no `docs/` directory and no file AEOS-BOOT would place — before AEOS-BOOT is attempted. This document adds no further target-location rule. | AEOS-BOOT Prerequisite 1 |
| `SETUP-026` | Every path written in tooling configuration for the AEOS repository MUST use forward slashes, independent of the host operating system, consistent with AEOS-LAYOUT `LAYOUT-007`. | AEOS-LAYOUT `LAYOUT-007` |
| `SETUP-027` | This document reserves no directory name for AEOS's own source code, test code, or non-Document Repository Assets; those remain AEOS-LAYOUT Non-Goals `NG-1` and `NG-2` until a future Implementation Guide resolves them. | AEOS-LAYOUT `NG-1` · `NG-2` |
| `SETUP-028` | Where the Contributor's chosen location is on a case-insensitive file system, the Contributor MUST confirm no two paths AEOS-BOOT would place differ only by case before proceeding, consistent with AEOS-LAYOUT `LAYOUT-008`. | AEOS-LAYOUT `LAYOUT-008` |

## 14. Verification Steps

Verification confirms a machine is prepared. It is read-only: it inspects the machine and reports; it
never installs, modifies, or removes anything.

| ID | Check | Traces to |
| :--- | :--- | :--- |
| `SETUP-029` | Verification MUST NOT install, modify, or remove software, a file, or a configuration value on the host machine. | `PR-SAF-009` |
| `SETUP-030` | Every Required item in [Section 6](#6-required-software), and every Conditional item the Contributor's work depends on, is confirmed present and invocable from the terminal `SETUP-014` requires, and reports a version. | `SETUP-004` · `SETUP-021` |
| `SETUP-031` | Each version reported under `SETUP-030` is checked against [Section 7](#7-required-versions)'s policy before being accepted. | `SETUP-007` |
| `SETUP-032` | The `PATH` condition `SETUP-021` states is confirmed for every Required item. | `SETUP-021` |
| `SETUP-033` | Every condition in [Section 8](#8-environment-prerequisites) and [Section 13](#13-directory-prerequisites) is confirmed before AEOS-BOOT Prerequisite 1 is attempted. | `SETUP-011`–`SETUP-016` · `SETUP-025`–`SETUP-028` |
| `SETUP-034` | A failure against any check above is reported with the specific unmet condition; verification MUST NOT proceed to a later check by assuming an earlier one passed. | `PR-SAF-002` · `PR-NFR-001` |

A verification run that finds no failure against `SETUP-029` through `SETUP-034` confirms the machine
is in the state [Section 15](#15-expected-successful-environment-state) describes. A verification run
that finds a failure reports it; resolving the failure is an ordinary act of installing or updating
the named item, not a re-run of any procedure this document does not itself state.

## 15. Expected Successful Environment State

Non-normative illustration of a machine immediately after [Section 14](#14-verification-steps)
passes:

- Every Required item in [Section 6](#6-required-software) is installed at a version satisfying
  [Section 7](#7-required-versions), and every Conditional item the Contributor's work depends on is
  as well.
- Every Required item's executable resolves on `PATH`, per `SETUP-021`.
- No AEOS-specific environment variable has been set, per `SETUP-022`; no credential has been
  configured, per `SETUP-024`.
- The location chosen for the AEOS repository exists (or does not yet exist) in a state satisfying
  AEOS-BOOT Prerequisite 1, and its path satisfies `SETUP-016` and `SETUP-026`.
- No software the Contributor had before consulting this document has been modified, reconfigured, or
  removed, per `ESP4`.
- AEOS-BOOT's own Prerequisites 3 and, where applicable, 4 are satisfiable without further action.

This is a description of a state, not a directory tree: this document places no file and creates no
repository. AEOS-BOOT's own [Section 9](PROJECT_BOOTSTRAP.md#9-expected-repository-state) illustrates
the repository state that follows once Bootstrap itself runs.

## 16. Common Setup Failures

Non-normative. Each row names a symptom this document's own verification would surface, not a defect
in AEOS.

| Failure | Likely cause | See |
| :--- | :--- | :--- |
| A Required item's version is reported below policy | An existing installation predates the item's supplier moving it past end-of-life support. | [Section 7](#7-required-versions), [Section 17](#17-troubleshooting-guidance) |
| A Required item is not found on `PATH` after installation | The installer placed the executable outside the directories the shell searches, or a new shell session has not started. | [Section 11](#11-required-environment-variables), [Section 17](#17-troubleshooting-guidance) |
| More than one interpreter or engine of the same kind is present, and the wrong one resolves first | Multiple installations exist on the same machine with conflicting `PATH` precedence. | [Section 17](#17-troubleshooting-guidance) |
| A path collision appears only on some machines | The AEOS repository's location is on a case-insensitive file system and two paths differ only by case. | `SETUP-028`, [Section 17](#17-troubleshooting-guidance) |
| Package downloads fail or time out | The network prerequisite in [Section 8](#8-environment-prerequisites) is met through a proxy that a tool's default configuration does not account for. | [Section 12](#12-optional-environment-variables), [Section 17](#17-troubleshooting-guidance) |
| A commit cannot be recorded once the AEOS repository exists | The version-control client's identity configuration `SETUP-018` requires has not been set. | `SETUP-018`, [Section 17](#17-troubleshooting-guidance) |

## 17. Troubleshooting Guidance

This document directs a Contributor to each tool's own diagnostic facilities rather than restating
them, consistent with AEOS-TECH's own non-restatement principle in Section 13.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SETUP-035` | Troubleshooting content in this document MUST NOT restate a tool's own diagnostic or error-message documentation; it MUST instead state which AEOS-specific condition, from [Sections 6](#6-required-software) through [13](#13-directory-prerequisites), the symptom relates to. | AEOS-TECH Section 13 |

For each failure category in [Section 16](#16-common-setup-failures):

- **Version below policy or item missing.** Consult the item's own installation documentation for the
  current release satisfying [Section 7](#7-required-versions)'s policy; this document names no
  installation command of its own, consistent with `ESP1`.
- **`PATH` resolution.** Consult the shell's own documentation for how it constructs `PATH`, and the
  item's own installation documentation for where it places its executable; open a new shell session
  before re-running [Section 14](#14-verification-steps).
- **Multiple installations.** Consult the shell's own documentation for `PATH` precedence; resolving
  which installation is authoritative is a Contributor decision this document does not make on their
  behalf, per `ESP4`.
- **Case-sensitivity path collision.** Choose a location, or a case-sensitive volume, satisfying
  `SETUP-028` before proceeding to AEOS-BOOT.
- **Proxied or constrained network.** Consult [Section 12](#12-optional-environment-variables) for the
  variables a Contributor's own network configuration may already require, and the affected tool's own
  documentation for how it reads them.
- **Commit identity.** Consult the version-control client's own documentation for configuring an
  identity; this document states only that `SETUP-018` must be satisfied, not how.

## 18. Non-Goals

This document deliberately does not decide the following.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | The ordered procedure that initializes the AEOS repository once a machine is prepared. | AEOS-BOOT. |
| `NG-2` | The permanent top-level shape of the AEOS repository. | AEOS-LAYOUT. |
| `NG-3` | Fixing a directory name for AEOS's own source code, test code, or non-Document Repository Assets. | AEOS-LAYOUT Non-Goals `NG-1`, `NG-2`, once a future Implementation Guide resolves them. |
| `NG-4` | Selecting or configuring an AI Runtime, a Model, or a credential for use with AEOS. | The Contributor, and Runtime documents, once they exist. |
| `NG-5` | Establishing CI/CD configuration for the AEOS repository. | The project's own delivery systems, consistent with the principle AEOS-BOOT `BOOT-012` states for Bootstrap. |
| `NG-6` | Stating an exact version pin for any required item. | The AEOS repository's own manifests and lockfiles, once Implementation exists, per AEOS-TECH `TG-083`. |
| `NG-7` | Selecting a Technology Stack for a Developer's own governed Project. | The Project and its own conventions, per AEOS-TECH Section 2.2. |
| `NG-8` | Describing how AEOS inspects or reports on an environment once it is running. | Runtime documents and, once implemented, AEOS itself, under `PR-ENV`. |

## 19. Traceability

Every `SETUP-` rule states its own trace inline, in [Sections 5](#5-supported-host-operating-systems)
through [17](#17-troubleshooting-guidance). This section consolidates that trace by target.

### 19.1 Product Requirements

| `PR-` identifier | `SETUP-` rules that trace to it |
| :--- | :--- |
| `PR-PLT-001` | `001` |
| `PR-PLT-002` | `003` |
| `PR-PLT-003` | `003` |
| `PR-NFR-001` | `034` |
| `PR-NFR-002` | `004` |
| `PR-NFR-012` | `017`, `020` |
| `PR-SAF-002` | `034` |
| `PR-SAF-006` | `022`, `024` |
| `PR-SAF-007` | `024` |
| `PR-SAF-009` | `013`, `029` |
| `PR-REP-013` | `022` |

### 19.2 AEOS-TECH

| `TC-` / `TG-` identifier | `SETUP-` rules that trace to it |
| :--- | :--- |
| `TC-01` | `001`, `002` |
| `TC-02` | `014`, `019` |
| `TC-03` | `006` |
| `TC-07` | `006` |
| `TC-08` | — (Conditional item, [Section 6](#6-required-software)) |
| `TC-18` | `006` |
| `TG-024` | `005` |
| `TG-060` | `009` |
| `TG-061` | `008` |
| `TG-062` | `001`, `002`, `007` |
| `TG-064` | `010` |
| `TG-082` | `005` |
| `TG-083` | `009` |

### 19.3 AEOS-BOOT and AEOS-LAYOUT

| Document rule | `SETUP-` rules that trace to it |
| :--- | :--- |
| AEOS-BOOT Prerequisite 1 | `025` |
| AEOS-BOOT Prerequisite 3 | `012` |
| AEOS-BOOT `BOOT-011` | `022`, `024` |
| AEOS-BOOT Section 7, Step 11 | `018` |
| AEOS-LAYOUT `LAYOUT-007` | `026` |
| AEOS-LAYOUT `LAYOUT-008` | `015`, `028` |
| AEOS-LAYOUT `LAYOUT-009` | `016` |
| AEOS-LAYOUT `LAYOUT-024` | `023` |
| AEOS-LAYOUT `NG-1`, `NG-2` | `027` |

## 20. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-PRD `PR-PLT`, `PR-SAF`, `PR-NFR`, `PR-REP` | The requirement families [Section 19](#19-traceability) traces `SETUP-` rules to. |
| AEOS-PRD `PR-ENV`, and the *Environment* entry it authorizes in AEOS-GLOSSARY | The product-layer subject this document is related to but does not restate, disambiguated in the authority statement above. |
| AEOS-GLOSSARY, *Contributor*, *Developer*, *Environment*, *Platform*, *Tool*, *Distribution Method* entries | The terms this document uses without redefinition. |
| AEOS-DOCSTD Section 4.1, 4.3 | The Implementation Guide layer's position and purpose, which this document is written under. |
| AEOS-TECH Sections 6 through 9, 12 | The support tiers, version policy, and rationale [Sections 5](#5-supported-host-operating-systems) through [10](#10-ide-and-editor-recommendations) draw from without restating. |
| AEOS-BOOT Section 3 | The Prerequisites this document prepares a machine to satisfy. |
| AEOS-BOOT Section 6, `BOOT-011` | The non-goal this document's [Sections 11](#11-required-environment-variables) and [12](#12-optional-environment-variables) apply by the same reasoning. |
| AEOS-LAYOUT Sections 6, 7, 18 | The root-level, naming-convention, and non-goal statements [Section 13](#13-directory-prerequisites) remains consistent with. |

## 21. Document Governance

### 21.1 Status

This document is a **Draft**. It is the first Environment Setup Guide authored for AEOS, and is
intended to become the Environment Setup Source of Truth once the owner's review under
[Section 21.4](#214-review-policy) records no Critical or Major finding, at which point it is intended
to be placed alongside AEOS-BOOT and AEOS-LAYOUT in `docs/implementation/`.

### 21.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `SETUP-<NNN>` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Addition of a Conditional item to [Section 6](#6-required-software) already recognized by AEOS-TECH at the tier this document requires | Owner approval | Minor |
| Any change to what an existing `SETUP-<NNN>` rule requires, or its retirement | Explicit owner revision request | Major |
| Addition or removal of a Required item in [Section 6](#6-required-software) | Explicit owner revision request | Major |
| Any change that would invalidate a machine already confirmed prepared under a prior version | Explicit owner revision request, with the reasoning preserved in place | Major |

### 21.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT,
AEOS-DOCSTD, AEOS-BOOT, or AEOS-LAYOUT are recorded as recommendations for the owning document's
governance and are applied only after explicit owner approval there — never enacted here.

### 21.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in [Sections 5](#5-supported-host-operating-systems) through
      [17](#17-troubleshooting-guidance) carries a `SETUP-<NNN>` identifier and a trace.
- [ ] No item in [Section 6](#6-required-software) is absent from AEOS-TECH's recognized set at the
      tier this document requires.
- [ ] No version number is pinned anywhere in this document, per `SETUP-009`.
- [ ] No rule states a repository-initialization procedure, a runtime behavior, an architectural
      decision, or a product requirement.
- [ ] No AEOS-specific required environment variable, credential, or Runtime selection is introduced,
      per `SETUP-022` and `SETUP-024`.
- [ ] All twenty-three entries in this document's Table of Contents are present, in order, and none
      is silently empty.
- [ ] No Critical or Major finding remains open.

### 21.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, or AEOS-TECH | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with AEOS-BOOT on a Prerequisite or on repository structure | AEOS-BOOT governs. This document's statement is corrected to reference it rather than restate it. |
| This document conflicts with AEOS-LAYOUT on a path or naming convention | AEOS-LAYOUT governs, for the same reason. |
| A future Developer Guide states a day-to-day convention that does not name a condition this document has not already named | Both stand. This document governs machine preparation; the guide governs practice once the machine is prepared. |
| A future Implementation Guide names a required software item that conflicts with [Section 6](#6-required-software) | The apparent need is reported against this document under [Section 21.2](#212-change-control). It is not resolved by a contradictory statement in the other guide. |

### 21.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Environment Setup Guide. States supported host operating system tiers by reference to AEOS-TECH `TC-01`; required and conditional software drawn entirely from AEOS-TECH's recognized categories; a version-currency policy that states no exact pin; environmental, directory, and path prerequisites consistent with AEOS-BOOT and AEOS-LAYOUT; development-tool and editor guidance by reference to AEOS-TECH `TC-02`; required and optional environment variables, excluding any AEOS-specific or credential-bearing variable; a read-only verification procedure; the expected state of a prepared machine; common setup failures and troubleshooting guidance that defers to each tool's own documentation; eight non-goals; and thirty-five `SETUP-<NNN>` rules in total. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, no specified behavior, and no repository-initialization procedure. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-BOOT, or AEOS-LAYOUT. |

---

## Appendix A — Verification Checklist (Non-Normative)

A practical restatement of [Section 14](#14-verification-steps), for a Contributor or an AI runtime
working through setup directly. This checklist carries no authority of its own; where it appears to
diverge from Sections 5 through 15, those sections govern.

- [ ] Host operating system confirmed at the Officially Supported or Conditionally Supported tier
      (`SETUP-001`, `SETUP-002`).
- [ ] Version-control client present, where the Distribution Method requires it, and configured with
      a commit identity (`SETUP-018`).
- [ ] Python interpreter present and on `PATH`.
- [ ] JavaScript engine (for example, Node.js) present and on `PATH`.
- [ ] `uv` present and on `PATH`.
- [ ] `pnpm` present and on `PATH`.
- [ ] Docker and Docker Compose present, where the Contributor's work depends on them.
- [ ] Every reported version checked against [Section 7](#7-required-versions)'s policy.
- [ ] Network access confirmed sufficient for a one-time download of every item above
      (`SETUP-011`).
- [ ] UTF-8 file creation confirmed (`SETUP-012`).
- [ ] Write permission confirmed at the intended AEOS repository location (`SETUP-013`).
- [ ] Chosen path contains no space and uses forward slashes in tooling configuration
      (`SETUP-016`, `SETUP-026`).
- [ ] Case-sensitivity of the chosen location confirmed, where applicable (`SETUP-028`).
- [ ] No AEOS-specific environment variable or credential configured (`SETUP-022`, `SETUP-024`).
- [ ] AEOS-BOOT Prerequisites 1, 3, and, where applicable, 4 confirmed satisfiable.

## Appendix B — SETUP Rule Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 5](#5-supported-host-operating-systems) through [17](#17-troubleshooting-guidance).

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `SETUP-001`–`SETUP-003` | Supported host operating systems | 3 | [5](#5-supported-host-operating-systems) |
| `SETUP-004`–`SETUP-006` | Required software | 3 | [6](#6-required-software) |
| `SETUP-007`–`SETUP-010` | Required versions | 4 | [7](#7-required-versions) |
| `SETUP-011`–`SETUP-016` | Environment prerequisites | 6 | [8](#8-environment-prerequisites) |
| `SETUP-017`–`SETUP-018` | Development tools | 2 | [9](#9-development-tools) |
| `SETUP-019`–`SETUP-020` | IDE and editor recommendations | 2 | [10](#10-ide-and-editor-recommendations) |
| `SETUP-021`–`SETUP-022` | Required environment variables | 2 | [11](#11-required-environment-variables) |
| `SETUP-023`–`SETUP-024` | Optional environment variables | 2 | [12](#12-optional-environment-variables) |
| `SETUP-025`–`SETUP-028` | Directory prerequisites | 4 | [13](#13-directory-prerequisites) |
| `SETUP-029`–`SETUP-034` | Verification steps | 6 | [14](#14-verification-steps) |
| `SETUP-035` | Troubleshooting guidance | 1 | [17](#17-troubleshooting-guidance) |
| **Total** | | **35** | — |

---

**End of Environment Setup Guide**

AEOS-ENVSETUP · Version 1.0.0 · Traces to `PR-PLT` · `PR-NFR` · `PR-SAF` · `PR-REP`, preparing the
host machine for AEOS-BOOT's own Prerequisites without restating AEOS-BOOT, AEOS-LAYOUT, or AEOS-TECH
