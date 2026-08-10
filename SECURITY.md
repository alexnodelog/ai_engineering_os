# AI Engineering Operating System

## AEOS — Security Policy

*The permanent statement of how a vulnerability in AEOS itself is reported, disclosed, and resolved,
and of the security-relevant procedure the AEOS repository follows for its own dependencies, secrets,
external AI integrations, and releases.*

| Field | Value |
| :--- | :--- |
| **Document** | Security Policy |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-SEC |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Release Engineering Board, AEOS |
| **Audience** | Contributors, security researchers, maintainers, release engineers, and AI runtimes reporting, reviewing, or resolving a security concern in AEOS itself |
| **Suggested path** | `docs/SECURITY.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_PRODUCT_REQUIREMENTS.md` (AEOS-PRD) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) · `AEOS_SUPPORTED_TECHNOLOGIES.md` (AEOS-TECH) · `PROJECT_BOOTSTRAP.md` (AEOS-BOOT) · `REPOSITORY_LAYOUT.md` (AEOS-LAYOUT) · `RUNTIME_ADAPTER_SPEC.md` (AEOS-SPEC-ADP) · `MODEL_REGISTRY.md` (AEOS-SPEC-MDL) · `RUNTIME_REGISTRY.md` (AEOS-RUNTIME-REG) |
| **Supersedes** | None |

> **Authority of this document.**
> This document states, precisely and reproducibly, **how a security concern in AEOS itself is
> reported, disclosed, and resolved**, and **the security-relevant procedure the AEOS repository
> follows for its own dependencies, secrets, external AI integrations, and releases**: the channel
> through which a vulnerability is reported, the disclosure process that follows a report, the
> repository's security contact, the conduct expected of a reporter, repository-level security
> practice, AI supply-chain considerations specific to AEOS's own orchestration of external Runtimes,
> the dependency and secrets policies AEOS's own implementation follows, release-security practice,
> and the review expectations a security-relevant change is held to.
>
> This document is an **Implementation Guide**, in the sense AEOS-DOCSTD Section 4.3 defines that
> layer: concrete guidance for building to specification, expressed at the point specification meets
> construction. It is not a Product document, not a Vision document, not an Architecture document,
> not a Blueprint, not a Runtime document, and not a behavioral Specification under AEOS-SPECSTD. It
> states no product requirement, no architectural decision, no Blueprint arrangement, no specified
> behavior, no runtime lifecycle, and no terminology; where a statement here appears to do any of
> these, that is a defect in this document and MUST be reported rather than acted upon. AEOS-SPECSTD
> governs the Specification layer of AEOS documentation — documents that state AEOS's own behavior
> precisely and testably under `SP-<AREA>-<NNN>` identifiers — and this document is not one of those;
> where this document follows a practice AEOS-SPECSTD also states, it does so because AEOS-DOCSTD
> already requires it of every document, not because AEOS-SPECSTD's Specification-layer structure
> applies here.
>
> AEOS-DOCSTD Section 4.1 positions Implementation Guides beneath Specification and above Developer
> Guides in the documentation hierarchy. This document is written under AEOS-DOCSTD's general
> document template and the Section 4.3 purpose statement for this layer, in the absence of a
> dedicated Implementation Guide Standard — in the same spirit AEOS-BOOT, AEOS-LAYOUT, and
> `ENVIRONMENT_SETUP.md` record for their own comparable position. It does not, on that account,
> establish such a Standard.
>
> **On this document's placement.** AEOS-LAYOUT `LAYOUT-016` requires that a Document not be placed
> outside `docs/`, and `LAYOUT-004`–`LAYOUT-005` permit no repository-root entry beyond `README.md`,
> `docs/`, a version-control ignore file, and an optional license file. A conventional GitHub security
> policy is recognized when placed at a repository's root, in `.github/`, or directly in `docs/` — not
> in a layer-specific subdirectory beneath it. Placing this document at `docs/implementation/`, where
> AEOS-BOOT's directory table would otherwise house an Implementation Guide, would satisfy the
> documentation hierarchy but would not be recognized by that platform convention; placing it at the
> repository root or in `.github/` would satisfy the platform convention but would violate
> `LAYOUT-004`–`LAYOUT-005`. This document is accordingly placed directly under `docs/`, outside every
> layer-specific subdirectory, exactly as AEOS-LAYOUT Section 5.2 places itself and for the same
> reason: `docs/` is already the common parent AEOS-BOOT `BOOT-002` creates, so a Document placed
> directly within it, at any depth, is placed consistent with AEOS-BOOT `BOOT-004` and `BOOT-007`
> without requiring a revision of AEOS-LAYOUT's directory table. This placement is a deliberate,
> recorded decision, not an assumption; where the owner judges differently, correcting it is an
> ordinary change to this document's own metadata, per [Section 19.2](#192-change-control).
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
4. [Security Principles](#4-security-principles)
5. [Supported Versions](#5-supported-versions)
6. [Reporting Vulnerabilities](#6-reporting-vulnerabilities)
7. [Disclosure Process](#7-disclosure-process)
8. [Security Contact](#8-security-contact)
9. [Responsible Disclosure](#9-responsible-disclosure)
10. [Repository Security](#10-repository-security)
11. [AI Supply Chain Considerations](#11-ai-supply-chain-considerations)
12. [Dependency Policy](#12-dependency-policy)
13. [Secrets Policy](#13-secrets-policy)
14. [Release Security](#14-release-security)
15. [Security Review Expectations](#15-security-review-expectations)
16. [Non-Goals](#16-non-goals)
17. [Traceability](#17-traceability)
18. [References](#18-references)
19. [Document Governance](#19-document-governance)
20. [Appendix A — Reporting Checklist (Non-Normative)](#appendix-a--reporting-checklist-non-normative)
21. [Appendix B — SEC Rule Index (Non-Normative)](#appendix-b--sec-rule-index-non-normative)

---

## 1. Executive Summary

AEOS orchestrates external AI runtimes on a Contributor's or a Developer's behalf, holds no
credential durably, performs no inference of its own, and treats the user's machine, repository,
credentials, and judgment as belonging to the user — AEOS-VISION `V1` and `V8`. A security policy for
AEOS is therefore not a statement about a runtime's own safety, which AEOS does not control, but a
statement about how AEOS itself is reported on, reviewed, and kept from becoming the exception to
its own rule: that a credential, a secret, or an unreviewed dependency never becomes part of what the
repository carries forward.

This document states that policy for the AEOS repository itself — not for a Developer's own governed
Project, which follows its own conventions under AEOS-TECH Section 2.2. It draws every rule from
requirements, rules, and identifiers AEOS-PRD, AEOS-TECH, AEOS-LAYOUT, AEOS-BOOT, and the frozen
Specification and Runtime documents already state, and introduces none of its own. Where something
about AEOS's own security posture is genuinely undecided — a dedicated contact address, a
response-time commitment, a version-specific support table — this document says so as a finished
statement of what is undecided, who decides it, and what happens in the meantime, consistent with
AEOS-DOCSTD `DS-P-10`, rather than filling the gap by assumption.

Four properties bind this document, consistent with the discipline AEOS-BOOT, AEOS-LAYOUT, and
`ENVIRONMENT_SETUP.md` record for a comparable statement of fact:

| Property | What it requires of this document |
| :--- | :--- |
| **Reproducible** | The same report, evaluated against the same rule, reaches the same disposition regardless of who evaluates it. |
| **Deterministic** | A reporting channel, a disclosure stage, or a review classification stated here does not depend on a choice this document leaves unstated. |
| **Platform-neutral, whenever possible** | Guidance is stated as policy and procedure, never as a command specific to one operating system, except where a cited platform feature (for example, a hosting platform's own vulnerability-reporting mechanism) is itself the subject. |
| **Non-invasive** | This document configures nothing on its own. Enabling a platform feature, designating a contact, or adopting a tool remains an action the owner or a maintainer takes outside this document. |

## 2. Scope and Applicability

### 2.1 What This Document Governs

This document governs the security policy and procedure of the AEOS repository itself, and nothing
beyond it:

- the channel through which a vulnerability in AEOS is reported;
- the disclosure process that follows a report, from acknowledgment to public disclosure;
- the repository's security contact, and what this document does and does not designate;
- the conduct expected of a reporter under responsible disclosure;
- repository-level security practice: what MUST never be committed, and what SHOULD be enabled;
- AI supply-chain considerations specific to AEOS's own orchestration of external Runtimes, Models,
  and AI Providers, and to review of a Contributor's proposed change regardless of its origin;
- the dependency policy AEOS's own implementation follows, drawn entirely from AEOS-TECH's
  already-governed set;
- the secrets policy AEOS's own implementation, adapters, and registries follow;
- release-security practice, once AEOS begins issuing releases;
- the review expectations a security-relevant change is held to;
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
| How each defined behavior must work, precisely and testably, including adapter, registry, and negotiation security responsibilities | Specification documents, in particular `RUNTIME_ADAPTER_SPEC.md`, `MODEL_REGISTRY.md`, and `RUNTIME_REGISTRY.md` |
| The ordered, reproducible procedure that creates, populates, and verifies a new AEOS repository | AEOS-BOOT |
| The permanent top-level shape of the AEOS repository | AEOS-LAYOUT |
| The environment a host machine provides before AEOS-BOOT is attempted | `ENVIRONMENT_SETUP.md` |
| How a person works day to day within an already-bootstrapped repository | Developer Guides |
| Security guidance for a Developer's own governed Project, as distinct from the AEOS repository itself | The Developer's own conventions, per AEOS-TECH Section 2.2 |
| Code, algorithms, dependency selection, and version pins | The codebase and its own manifests and lockfiles, per AEOS-TECH Section 2.2 and `TG-083` |

A statement in this document that states a repository-initialization procedure, a runtime behavior,
an architectural decision, a product requirement, or a technology choice absent from AEOS-TECH is a
**defect in this document**. It MUST be reported rather than acted upon.

### 2.3 Applicability

This document applies to the AEOS repository — AEOS's own source, documents, and Repository Assets —
and to anyone reporting, reviewing, or resolving a security concern within it: a Contributor, an AI
runtime acting on a Contributor's behalf, a maintainer, or an external security researcher who is not
a Contributor in the AEOS-GLOSSARY sense. It applies identically to a human party and to an AI runtime
performing a step on a human's behalf, consistent with AEOS-DOCSTD Section 2.4.

This document does not apply to the security posture of a Developer's own governed Project, which the
Developer's own conventions and AEOS-TECH Section 2.2 govern, nor to the internal security practice of
an external Runtime, Model, or AI Provider, which is that party's own responsibility and outside
AEOS's control, consistent with AEOS-GLOSSARY's *Runtime* entry: an integration, never a part of AEOS.

## 3. Relationship to Other Documents

| Document | Governs | This document's obligation |
| :--- | :--- | :--- |
| **AEOS-VISION** | Purpose and enduring intent, including `V1` (no inference) and `V8` (the user's machine, repository, credentials, and judgment belong to the user). | This document takes no position on why AEOS exists. Every rule below is written so that it could not require trading away either invariant. |
| **AEOS-PRD** | Product definition and the `PR-` requirements, including `PR-SAF`, `PR-REP`, `PR-RUN`, `PR-PMT`, `PR-DST`, `PR-NFR`, and `PR-TDD`. | Every `SEC-` rule that binds a party traces to one or more `PR-` identifiers already established there; this document adds no new requirement. |
| **AEOS-GLOSSARY** | Terminology. | This document uses *Contributor*, *Developer*, *Repository Asset*, *Runtime State*, *Runtime*, *Vendor*, *Review*, and *Distribution Method* exactly as AEOS-GLOSSARY defines them, and redefines none of them. *Credential* and *secret* are used in their ordinary sense, consistent with their use elsewhere in the frozen corpus, and are not AEOS-GLOSSARY-defined terms. |
| **AEOS-DOCSTD** | Documentation form and lifecycle. | This document's structure, format, normative vocabulary, and template follow it without exception. |
| **AEOS-TECH** | Recognized technologies and their support tiers, in particular `TC-20` (Security). | Every technology this document names is drawn from AEOS-TECH's recognized categories and tiers; this document introduces no technology of its own, consistent with `TG-080` through `TG-087`. |
| **AEOS-BOOT** | The ordered procedure that initializes a new AEOS repository. | This document restates none of AEOS-BOOT's sequence; it references `BOOT-011`, `BOOT-012`, and `BOOT-024` only by identifier, where a security rule depends on them. |
| **AEOS-LAYOUT** | The permanent shape of the AEOS repository. | This document's own placement, and every repository-security rule stated here, is consistent with `LAYOUT-004`, `LAYOUT-005`, `LAYOUT-016`, and `LAYOUT-033`; it names no top-level entry AEOS-LAYOUT does not already name. |
| **`RUNTIME_ADAPTER_SPEC.md`, `MODEL_REGISTRY.md`, `RUNTIME_REGISTRY.md`** | The precise, testable behavioral rules governing adapter, registry, and negotiation security responsibilities. | [Section 11](#11-ai-supply-chain-considerations) and [Section 13](#13-secrets-policy) cite `SP-ADP-`, `SP-MDL-`, and `RT-REG-` rules already stated there; this document restates none of them independently and states no new behavioral rule. |

## 4. Security Principles

These principles constrain every rule in this document. A rule that violates one is defective, not
merely unconventional.

| # | Principle |
| :--- | :--- |
| `SECP1` | A credential or a secret is never durable in a Repository Asset, regardless of which layer, adapter, registry, or tool produced or would produce it. |
| `SECP2` | A dependency AEOS's own implementation adopts is drawn from AEOS-TECH's already-governed set; this document introduces no technology of its own and narrows no tier AEOS-TECH already assigns. |
| `SECP3` | A security report is safety-relevant input and follows the safe path by default: an uncertain report is triaged, never dismissed, consistent with AEOS-PRD `PR-SAF-001` and `PR-SAF-002`. |
| `SECP4` | Review of a security-relevant change is identical regardless of whether the change originates from a human Contributor or an AI runtime acting as one. |
| `SECP5` | A gap this document has not yet decided — a contact address, a response-time commitment, a version-support table — is recorded as a Non-Goal and left open rather than filled by assumption. |
| `SECP6` | This document states policy and procedure. It grants no product capability, performs no configuration, and enables no platform feature of its own; doing so remains an action taken outside this document. |

## 5. Supported Versions

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-001` | AEOS has not yet reached a first Distribution-bearing release under AEOS-PRD Section 22's Release Phases. This section accordingly states no version-specific support table; stating one before a release exists would be a placeholder this document's own `DS-P-10` obligation prohibits. | AEOS-PRD Section 22 |
| `SEC-002` | Until a first release exists, the security policy this document states applies to the current state of the repository's default branch. A report concerning an unreleased, in-development capability is accepted on the same terms as one concerning released capability. | `SECP3` |
| `SEC-003` | Once AEOS issues a release that reports its version and origin, consistent with `PR-DST-008`, this section MUST be revised to record which versions receive security fixes, under ordinary change control ([Section 19.2](#192-change-control)). | `PR-DST-008` |

## 6. Reporting Vulnerabilities

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-004` | A vulnerability in AEOS MUST NOT be reported through a public GitHub issue, a public discussion, or a public pull request. | `PR-SAF-008`, disclosure before exposure |
| `SEC-005` | The designated reporting channel is the repository's own private vulnerability reporting mechanism, where a maintainer has enabled it, consistent with [Section 8](#8-security-contact). | [Section 8](#8-security-contact) |
| `SEC-006` | A report SHOULD state the specific location affected and, where the reporter can state it, enough detail for a maintainer to reproduce the condition without further correspondence, consistent with the specificity AEOS-DOCSTD `12.4` already requires of a documentation finding. | AEOS-DOCSTD Section 12.4 |
| `SEC-007` | A report concerning a Developer's own governed Project, rather than AEOS itself, is out of this document's scope, per [Section 16](#16-non-goals), and is directed to that Project's own security practice instead. | [Section 16](#16-non-goals) |

## 7. Disclosure Process

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-008` | A received report MUST be acknowledged, triaged, and, where confirmed, tracked privately until a fix is available, consistent with the coordinated-disclosure mechanism named in [Section 8](#8-security-contact). | `PR-SAF-002` |
| `SEC-009` | This document states no fixed acknowledgment or resolution time. A specific target, once the owner records one, is added here under ordinary change control ([Section 19.2](#192-change-control)) rather than assumed. | `SECP5` |
| `SEC-010` | A confirmed vulnerability is disclosed publicly only after a fix is available, or after coordination with the reporter concludes, whichever the owner determines first serves users still exposed. | `PR-SAF-001` |
| `SEC-011` | Public disclosure MUST NOT include exploit detail sufficient for reproduction beyond what a published fix already reveals, until users have had a reasonable opportunity to update. | `PR-SAF-001` |
| `SEC-012` | A declined report — one found not to be a vulnerability, or out of scope under [Section 16](#16-non-goals) — is closed with the reason stated to the reporter. | AEOS-DOCSTD `12.4`, cited-reason discipline |

## 8. Security Contact

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-013` | The repository's private vulnerability reporting feature, where the repository owner or an administrator has enabled it, is the designated channel for a vulnerability report. This document does not itself enable that feature; doing so is an action taken outside this document, consistent with `SECP6`. | `SECP6` |
| `SEC-014` | This document does not designate a dedicated security-contact address. Where the owner establishes one, it is recorded here under ordinary change control, and this rule is retired in place, consistent with AEOS-DOCSTD `E3`. | AEOS-DOCSTD `E3`, `SECP5` |
| `SEC-015` | While private vulnerability reporting is not yet enabled and no dedicated address is designated, a reporter contacts a maintainer through the repository's ordinary, non-public channels rather than a public issue, consistent with [Section 6](#6-reporting-vulnerabilities). | [Section 6](#6-reporting-vulnerabilities) |

## 9. Responsible Disclosure

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-016` | A reporter is asked to give maintainers reasonable time to investigate and address a report before any public disclosure. | [Section 7](#7-disclosure-process) |
| `SEC-017` | A reporter is asked to make a good-faith effort to avoid privacy violation, data destruction, and service disruption while investigating a report. | `PR-SAF-001` |
| `SEC-018` | A reporter is asked to interact only with their own test data or their own instance while investigating, and MUST NOT access, modify, or exfiltrate another party's repository content or credentials. | `PR-SAF-009` |
| `SEC-019` | A report made in good faith and within the bounds `SEC-016` through `SEC-018` state is not treated by the project as a hostile act. | `SECP3` |

## 10. Repository Security

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-020` | No credential, secret, or Runtime State record is committed at any path in the AEOS repository. | AEOS-LAYOUT `LAYOUT-033` · AEOS-BOOT `BOOT-024` |
| `SEC-021` | A change to the AEOS repository is admitted only through review, consistent with [Section 15](#15-security-review-expectations); a repository hosting configuration SHOULD enforce this with a required-review setting where the hosting platform provides one. | `PR-REP-011` |
| `SEC-022` | Secret-scanning and dependency-auditing tooling AEOS-TECH `TC-20` recognizes SHOULD be applied to the AEOS repository itself, not only recommended to a Developer's governed Project. | AEOS-TECH `TC-20` |
| `SEC-023` | A security tool's default configuration path that would add a new repository-root entry or top-level directory is a conflict between that tool's convention and AEOS-LAYOUT `LAYOUT-004`–`LAYOUT-005`, per [Section 16](#16-non-goals), and MUST be reported rather than silently accommodated by an undocumented root-level addition. | AEOS-LAYOUT `LAYOUT-004`–`LAYOUT-005` |
| `SEC-024` | Where AEOS-TECH `TC-18` recognizes the repository's hosting platform, that platform's own built-in vulnerability- and secret-scanning features SHOULD be enabled at the repository owner's discretion; enabling them is a repository configuration action outside this document's own authority, consistent with `SECP6`. | AEOS-TECH `TC-18`, `SECP6` |

## 11. AI Supply Chain Considerations

AEOS performs no inference of its own and orchestrates external Runtimes, Models, and AI Providers as
integrations — AEOS-VISION `V1`, AEOS-GLOSSARY's *Runtime* entry. The supply-chain surface this
section addresses is therefore twofold: the external AI integrations AEOS mediates, and the fact that
a Contributor proposing a change to AEOS itself may be a human or an AI runtime acting on a human's
behalf, per AEOS-GLOSSARY's *Contributor* entry.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-025` | Every external AI Runtime, Model, or AI Provider AEOS integrates with MUST already be recognized, or be under evaluation, in AEOS-TECH's `TC-09`, `TC-10`, or `TC-11` categories; this document does not itself list, recommend, or endorse one, consistent with `TG-080` through `TG-087`. | AEOS-TECH `TC-09`–`TC-11`, `TG-080`–`TG-087` |
| `SEC-026` | AEOS MUST NOT transmit repository content to a Runtime the user has not selected and approved, and MUST report what will leave the machine before it leaves. | `PR-SAF-007` · `PR-SAF-008` |
| `SEC-027` | A Model's registration MUST record the identity of its declaring adapter alongside the Model's identifier, and MUST be available for inspection before a workflow step that depends on it begins. | `MODEL_REGISTRY.md` `SP-MDL-022` · `SP-MDL-023` |
| `SEC-028` | The Model Registry and Runtime Registry behaviors MUST NOT privilege one Model, Runtime, or Vendor over another in what they make discoverable. | `MODEL_REGISTRY.md` `SP-MDL-047` · `SP-MDL-049`; AEOS-VISION `V6` |
| `SEC-029` | A Contributor's proposed change is reviewed identically whether authored by a person or by an AI runtime acting as a Contributor, consistent with `SECP4`; an AI runtime's output MUST NOT be presented as an observed fact. | AEOS-GLOSSARY *Contributor*; `PR-SAF-011` · `PR-REP-011` |
| `SEC-030` | A dependency, Rule, Skill, Prompt, Template, or Workflow an AI runtime proposes is subject to the same [Dependency Policy](#12-dependency-policy) and [Repository Security](#10-repository-security) rules as one a human Contributor proposes; origin MUST NOT exempt an addition from review. | `SECP4`, [Section 12](#12-dependency-policy), [Section 10](#10-repository-security) |

## 12. Dependency Policy

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-031` | Every dependency AEOS's own implementation adopts MUST already be recognized by AEOS-TECH at Officially Supported or Preferred tier, or MUST first be proposed as a Candidate there, consistent with `TG-080`, `TG-082`, and `TG-083`. | AEOS-TECH `TG-080`, `TG-082`, `TG-083` |
| `SEC-032` | A dependency MUST satisfy the gating criteria AEOS-TECH `TG-040` through `TG-046` state, including replaceability, local operability, active maintenance, and no secret exposure. | AEOS-TECH `TG-040`–`TG-046` |
| `SEC-033` | The version of every adopted dependency SHOULD track its supplier's current security-support lifecycle, consistent with `TG-062`, and MUST NOT remain on a version that has reached supplier end-of-life once `TG-063`'s deprecation applies. | AEOS-TECH `TG-062` · `TG-063` |
| `SEC-034` | A security or licensing emergency MAY compress the deprecation period an otherwise-applicable dependency change would require, consistent with `TG-071`; the compression and its reason MUST be recorded. | AEOS-TECH `TG-071` |
| `SEC-035` | Dependency and secret auditing SHOULD be performed using tooling AEOS-TECH `TC-20` recognizes at Officially Supported or Conditionally Supported tier. | AEOS-TECH `TC-20` |
| `SEC-036` | A dependency's removal for a security reason MAY bypass the standard one-minor-release deprecation notice `TG-033` otherwise requires, provided the exception and its rationale are recorded, consistent with `TG-033`'s sole stated exception. | AEOS-TECH `TG-033` |

## 13. Secrets Policy

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-037` | A credential or a secret MUST NOT be placed in a prompt, a log, a report, documentation, or a Repository Asset. | `PR-SAF-006` · `PR-REP-013` · `PR-PMT-008` |
| `SEC-038` | No file at any path in the AEOS repository MUST contain a credential, a secret, or a Runtime State record. | AEOS-LAYOUT `LAYOUT-033` · AEOS-BOOT `BOOT-011` · `BOOT-024` |
| `SEC-039` | A Runtime Adapter MUST NOT record a credential in any durable artifact, including its own declaration, and MUST hold a credential its mediated Runtime requires only as Runtime State, for no longer than the invocation it authorizes. | `RUNTIME_ADAPTER_SPEC.md` `SP-ADP-065` · `SP-ADP-066` |
| `SEC-040` | The Runtime Registry and the Model Registry behaviors MUST NOT hold or expose a credential. | `RUNTIME_REGISTRY.md` `RT-REG-003` · `RT-REG-009` · `RT-REG-011`; `MODEL_REGISTRY.md` `SP-MDL-051` |
| `SEC-041` | Where a secret is stored on a Contributor's or a Developer's own machine, it SHOULD be stored using a platform-native credential store AEOS-TECH `TC-20` lists at Preferred tier, and MUST NOT be committed to the repository in a configuration file, consistent with `TC-20`'s Not Recommended entry. | AEOS-TECH `TC-20` |

## 14. Release Security

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-042` | AEOS has not yet reached a first Distribution-bearing release under AEOS-PRD Section 22. This section states the policy that governs a release once one exists; it does not describe a current operational fact. | AEOS-PRD Section 22, `SECP5` |
| `SEC-043` | Every release MUST report its version and origin. | `PR-DST-008` |
| `SEC-044` | Where a release artifact is produced, it SHOULD be signed, or accompanied by a software bill of materials, using a mechanism AEOS-TECH `TC-20` already recognizes at Conditionally Supported tier; this document introduces no signing or attestation mechanism of its own. | AEOS-TECH `TC-20`, `TG-085` |
| `SEC-045` | A security or licensing emergency MAY compress an otherwise-applicable release or removal notice period, consistent with `TG-071`, and MUST be recorded with its reason. | AEOS-TECH `TG-071` |

## 15. Security Review Expectations

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `SEC-046` | A change addressing a security-relevant defect follows the TDD workflow AEOS-PRD `PR-TDD-001` through `PR-TDD-008` state, including a test demonstrating the defect before the fix, where a test can express it. | `PR-TDD-001`–`PR-TDD-008` |
| `SEC-047` | A review of a security-relevant change classifies findings as Critical, Major, Minor, or Nitpick, consistent with AEOS-GLOSSARY's *Review* entry and `PR-REP-011`, and MUST NOT redesign the change under review. | AEOS-GLOSSARY *Review*; `PR-REP-011` |
| `SEC-048` | A security-relevant Critical or Major finding MUST be resolved, or escalated to the owner, before the change is merged; it MUST NOT be deferred to a later revision by a reviewer's discretion alone. | `PR-SAF-002` |
| `SEC-049` | A security advisory, once published, MUST record what was affected and what was fixed, and, where determinable, the earliest affected point. | `PR-REP-012` |
| `SEC-050` | Credit for a reported vulnerability MAY be recorded at the reporter's request, and MUST NOT be recorded against the reporter's request. | `PR-NFR-001` |

## 16. Non-Goals

This document deliberately does not decide the following.

| # | Non-goal | Owned by, if anywhere |
| :--- | :--- | :--- |
| `NG-1` | Establishing a CVE Numbering Authority role, or assigning CVE identifiers, for AEOS. | The owner, should AEOS later pursue CNA status; not decided here. |
| `NG-2` | Establishing a bug-bounty or monetary reward program. | The owner; a future decision this document does not anticipate. |
| `NG-3` | Stating a fixed acknowledgment or resolution-time commitment. | The owner, recorded here under ordinary change control once set. See `SEC-009`. |
| `NG-4` | Designating a dedicated security-contact address. | The owner. See `SEC-014`. |
| `NG-5` | Stating a version-specific support table. | This document, once AEOS reaches a first Distribution-bearing release under AEOS-PRD Section 22. See [Section 5](#5-supported-versions). |
| `NG-6` | Claiming an external security certification or compliance attestation (for example, SOC 2 or ISO/IEC 27001) for AEOS. | The owner, should such an attestation later be pursued. |
| `NG-7` | Establishing CI/CD security-scanning configuration for the AEOS repository. | The project's own delivery systems, consistent with the reasoning AEOS-BOOT `BOOT-012` states for Bootstrap. |
| `NG-8` | Stating security guidance for a Developer's own governed Project. | The Developer's own conventions and, where applicable, AEOS-TECH, per AEOS-TECH Section 2.2. |
| `NG-9` | Resolving where a security tool's default root-level configuration file is placed, where its convention conflicts with AEOS-LAYOUT `LAYOUT-004` or `LAYOUT-005`. | AEOS-LAYOUT's own change control, or the tool's non-default configuration option, at the owner's discretion. See `SEC-023`. |
| `NG-10` | Cryptographic implementation guidance. | Not applicable — AEOS performs no inference and provides no cryptographic implementation of its own, per AEOS-VISION `V1`. |

## 17. Traceability

Every `SEC-` rule states its own trace inline, in [Sections 5](#5-supported-versions) through
[15](#15-security-review-expectations). This section consolidates that trace by target.

### 17.1 Product Requirements

| `PR-` identifier | `SEC-` rules that trace to it |
| :--- | :--- |
| `PR-SAF-001` | `010`, `011`, `017` |
| `PR-SAF-002` | `008`, `019` (via `SECP3`), `048` |
| `PR-SAF-006` | `037` |
| `PR-SAF-007` | `026` |
| `PR-SAF-008` | `004`, `026` |
| `PR-SAF-009` | `018` |
| `PR-SAF-011` | `029` |
| `PR-REP-011` | `021`, `029`, `047` |
| `PR-REP-012` | `049` |
| `PR-REP-013` | `037` |
| `PR-PMT-008` | `037` |
| `PR-DST-008` | `003`, `043` |
| `PR-NFR-001` | `050` |
| `PR-TDD-001`–`PR-TDD-008` | `046` |

### 17.2 AEOS-TECH

| `TC-` / `TG-` identifier | `SEC-` rules that trace to it |
| :--- | :--- |
| `TC-09`–`TC-11` | `025` |
| `TC-18` | `024` |
| `TC-20` | `022`, `035`, `041`, `044` |
| `TG-033` | `036` |
| `TG-040`–`TG-046` | `032` |
| `TG-062` | `033` |
| `TG-063` | `033` |
| `TG-071` | `034`, `045` |
| `TG-080`, `TG-082`, `TG-083` | `025`, `031` |
| `TG-085` | `044` |

### 17.3 Specification and Runtime Documents

| Document rule | `SEC-` rules that trace to it |
| :--- | :--- |
| `RUNTIME_ADAPTER_SPEC.md` `SP-ADP-065`, `SP-ADP-066` | `039` |
| `MODEL_REGISTRY.md` `SP-MDL-022`, `SP-MDL-023` | `027` |
| `MODEL_REGISTRY.md` `SP-MDL-047`, `SP-MDL-049` | `028` |
| `MODEL_REGISTRY.md` `SP-MDL-051` | `040` |
| `RUNTIME_REGISTRY.md` `RT-REG-003`, `RT-REG-009`, `RT-REG-011` | `040` |

### 17.4 AEOS-BOOT and AEOS-LAYOUT

| Document rule | `SEC-` rules that trace to it |
| :--- | :--- |
| AEOS-BOOT `BOOT-011` | `038` |
| AEOS-BOOT `BOOT-012` | `NG-7` |
| AEOS-BOOT `BOOT-024` | `020`, `038` |
| AEOS-LAYOUT `LAYOUT-004`–`LAYOUT-005` | `023`, `NG-9` |
| AEOS-LAYOUT `LAYOUT-016` | Document placement, [Authority statement](#aeos--security-policy) |
| AEOS-LAYOUT `LAYOUT-033` | `020`, `038` |

## 18. References

| Document or section | What this document cites it for |
| :--- | :--- |
| AEOS-PRD `PR-SAF`, `PR-REP`, `PR-PMT`, `PR-RUN`, `PR-DST`, `PR-NFR`, `PR-TDD` | The requirement families [Section 17](#17-traceability) traces `SEC-` rules to. |
| AEOS-PRD Section 22, Release Phases | The absence of a first release, which [Sections 5](#5-supported-versions) and [14](#14-release-security) state rather than assume around. |
| AEOS-GLOSSARY *Contributor*, *Developer*, *Repository Asset*, *Runtime State*, *Runtime*, *Vendor*, *Review*, *Distribution Method* entries | The terms this document uses without redefinition. |
| AEOS-DOCSTD Section 4.1, 4.3 | The Implementation Guide layer's position and purpose, which this document is written under. |
| AEOS-DOCSTD `DS-P-10`, `E3`, Section 12.4 | The gap-recording, retirement, and finding-specificity discipline [Sections 5](#5-supported-versions), [8](#8-security-contact), [6](#6-reporting-vulnerabilities), and [12.1](#16-non-goals) apply. |
| AEOS-TECH `TC-09`–`TC-11`, `TC-18`, `TC-20` | The recognized technology categories [Sections 10](#10-repository-security), [11](#11-ai-supply-chain-considerations), [12](#12-dependency-policy), [13](#13-secrets-policy), and [14](#14-release-security) draw from without restating. |
| AEOS-TECH `TG-033`, `TG-040`–`TG-046`, `TG-060`–`TG-065`, `TG-071`, `TG-080`–`TG-087` | The dependency lifecycle, gating, and source-of-truth rules [Section 12](#12-dependency-policy) applies by reference. |
| AEOS-BOOT `BOOT-002`, `BOOT-004`, `BOOT-007`, `BOOT-011`, `BOOT-012`, `BOOT-024` | The bootstrap and placement rules this document's own placement and [Sections 10](#10-repository-security) and [13](#13-secrets-policy) remain consistent with. |
| AEOS-LAYOUT Section 5.2, `LAYOUT-004`–`LAYOUT-005`, `LAYOUT-016`, `LAYOUT-033` | The root-level, document-placement, and no-credential rules this document's [Authority statement](#aeos--security-policy) and [Sections 10](#10-repository-security), [13](#13-secrets-policy) remain consistent with. |
| `RUNTIME_ADAPTER_SPEC.md`, `MODEL_REGISTRY.md`, `RUNTIME_REGISTRY.md` | The adapter, model, and registry security responsibilities [Sections 11](#11-ai-supply-chain-considerations) and [13](#13-secrets-policy) cite without restating. |

## 19. Document Governance

### 19.1 Status

This document is a **Draft**. It is the first Security Policy authored for AEOS, and is intended to
become the Security Source of Truth once the owner's review under [Section 19.4](#194-review-policy)
records no Critical or Major finding, at which point it is intended to be placed at `docs/SECURITY.md`
as its own metadata block already states.

### 19.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Addition of a new `SEC-<NNN>` rule that does not alter what an existing rule requires | Owner approval | Minor |
| Designation of a security-contact address or a response-time commitment, resolving `NG-3` or `NG-4` | Owner approval | Minor |
| Addition of the first version-specific support table, once a release exists, resolving `NG-5` | Owner approval | Minor |
| Any change to what an existing `SEC-<NNN>` rule requires, or its retirement | Explicit owner revision request | Major |
| Relocation of this document to a different path, including into a layer-specific subdirectory | Explicit owner revision request | Major |
| Any change that would weaken [Section 13](#13-secrets-policy) or [Section 10](#10-repository-security) | Explicit owner revision request | Major |

### 19.3 Relationship to the Architecture Freeze

This document introduces no architecture, no product requirement, no terminology, and no specified
behavior. Ideas arising from it that would alter AEOS-PRD, AEOS-TECH, AEOS-ARCH, AEOS-BLUEPRINT,
AEOS-DOCSTD, AEOS-BOOT, AEOS-LAYOUT, or a Specification document are recorded as recommendations for
the owning document's governance and are applied only after explicit owner approval there — never
enacted here.

### 19.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning the document, and recommend freezing it when no Critical
or Major finding remains, per AEOS-DOCSTD Section 12.3 and 12.4. A reviewer additionally confirms,
before recommending freeze:

- [ ] Every rule in [Sections 5](#5-supported-versions) through
      [15](#15-security-review-expectations) carries a `SEC-<NNN>` identifier and a trace.
- [ ] No rule states a repository-initialization procedure, a runtime behavior, an architectural
      decision, a product requirement, or a technology absent from AEOS-TECH.
- [ ] No section states a fixed response-time commitment, a dedicated contact address, or a
      version-support table not already recorded as a Non-Goal.
- [ ] No credential, secret, or contact detail is fabricated anywhere in this document.
- [ ] All twenty-one entries in this document's Table of Contents are present, in order, and none
      is silently empty.
- [ ] No Critical or Major finding remains open.

### 19.5 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, or AEOS-TECH | The higher-authority document governs. The conflict is a defect in this document and is reported rather than acted upon. |
| This document conflicts with a Specification document on an adapter, registry, or negotiation security responsibility | The Specification document governs. This document's statement is corrected to reference it rather than restate it. |
| This document conflicts with AEOS-BOOT on a Prerequisite or a repository-initialization procedure | AEOS-BOOT governs. |
| This document conflicts with AEOS-LAYOUT on a path or a naming convention | AEOS-LAYOUT governs, for the same reason. |
| A future Developer Guide states a day-to-day security convention that does not name a condition this document has not already named | Both stand. This document governs AEOS's own security policy; the guide governs practice once a machine and a repository are already prepared. |
| A security tool's convention conflicts with AEOS-LAYOUT's placement rules | The apparent need is reported under `NG-9` and [Section 19.2](#192-change-control). It is not resolved by an undocumented root-level addition. |

### 19.6 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Initial Security Policy. States the reporting channel, disclosure process, and responsible-disclosure expectations for a vulnerability in AEOS itself; a security contact section that designates GitHub's own private vulnerability reporting as the channel and explicitly declines to fabricate a dedicated contact address, recorded as `NG-4`; repository-security, AI supply-chain, dependency, secrets, release-security, and security-review-expectations sections drawn entirely from already-governed `PR-`, `TG-`, `SP-`, `RT-`, `LAYOUT-`, and `BOOT-` identifiers; a Supported Versions section that records AEOS's pre-release status rather than inventing a version table, per `NG-5`; ten explicit Non-Goals; and fifty `SEC-<NNN>` rules in total. States, in its own authority statement, the reasoning for placing this document directly under `docs/` rather than in a layer-specific subdirectory or at the repository root, consistent with the precedent AEOS-LAYOUT Section 5.2 records for its own placement. Introduces no product requirement, no vision, no terminology, no architectural decision, no Blueprint arrangement, and no specified behavior. Redefines nothing stated by AEOS-VISION, AEOS-PRD, AEOS-GLOSSARY, AEOS-DOCSTD, AEOS-TECH, AEOS-BOOT, AEOS-LAYOUT, or any Specification document. |

---

## Appendix A — Reporting Checklist (Non-Normative)

A practical restatement of [Sections 6](#6-reporting-vulnerabilities) through
[9](#9-responsible-disclosure), for a reporter working through a report directly. This checklist
carries no authority of its own; where it appears to diverge from those sections, they govern.

- [ ] The concern is about AEOS itself, not about a Developer's own governed Project (`SEC-007`).
- [ ] No public GitHub issue, discussion, or pull request has been opened for the concern (`SEC-004`).
- [ ] The repository's private vulnerability reporting feature has been checked for availability
      before any other channel is used (`SEC-005`, `SEC-013`).
- [ ] The report states the specific location affected and, where known, reproduction detail
      (`SEC-006`).
- [ ] No investigation has touched another party's repository content or credentials (`SEC-018`).
- [ ] No public disclosure has been made, and none is planned, before maintainers have had
      reasonable time to respond (`SEC-016`).

## Appendix B — SEC Rule Index (Non-Normative)

**This appendix is non-normative.** The normative statement of each rule is its entry in
[Sections 5](#5-supported-versions) through [15](#15-security-review-expectations).

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `SEC-001`–`SEC-003` | Supported versions | 3 | [5](#5-supported-versions) |
| `SEC-004`–`SEC-007` | Reporting vulnerabilities | 4 | [6](#6-reporting-vulnerabilities) |
| `SEC-008`–`SEC-012` | Disclosure process | 5 | [7](#7-disclosure-process) |
| `SEC-013`–`SEC-015` | Security contact | 3 | [8](#8-security-contact) |
| `SEC-016`–`SEC-019` | Responsible disclosure | 4 | [9](#9-responsible-disclosure) |
| `SEC-020`–`SEC-024` | Repository security | 5 | [10](#10-repository-security) |
| `SEC-025`–`SEC-030` | AI supply chain considerations | 6 | [11](#11-ai-supply-chain-considerations) |
| `SEC-031`–`SEC-036` | Dependency policy | 6 | [12](#12-dependency-policy) |
| `SEC-037`–`SEC-041` | Secrets policy | 5 | [13](#13-secrets-policy) |
| `SEC-042`–`SEC-045` | Release security | 4 | [14](#14-release-security) |
| `SEC-046`–`SEC-050` | Security review expectations | 5 | [15](#15-security-review-expectations) |
| **Total** | | **50** | — |

---

**End of Security Policy**

AEOS-SEC · Version 1.0.0 · Traces to `PR-SAF` · `PR-REP` · `PR-PMT` · `PR-DST` · `PR-NFR` · `PR-TDD`,
applying AEOS-TECH `TC-09`–`TC-11`, `TC-18`, `TC-20`, AEOS-LAYOUT, AEOS-BOOT, and the adapter, model,
and registry Specification documents to the security of the AEOS repository itself, without restating
any of them
