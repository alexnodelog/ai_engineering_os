# AI Engineering Operating System

## AEOS — Supported Technologies

*The permanent technology source of truth for AEOS.*

| Field | Value |
| :--- | :--- |
| **Document** | Supported Technologies |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-TECH |
| **Version** | 1.0.1 |
| **Status** | Freeze candidate — awaiting owner approval |
| **Owner** | Product Owner, AEOS |
| **Author** | Chief Technology Officer and Technology Governance Authority, AEOS |
| **Audience** | Developers, architects, maintainers, contributors, and AI Runtimes consuming this repository |
| **Suggested path** | `docs/product/SUPPORTED_TECHNOLOGIES.md` |
| **Companion documents** | AEOS-PRD · AEOS-VISION · AEOS-GLOSSARY · AEOS-DOCSTD |
| **Supersedes** | AEOS-TECH 1.0.0 |

> **Authority of this document.**
> This document defines *which technologies AEOS officially supports* and *how that set is
> governed*. It defines no architecture, no implementation, no Project Template, no runtime
> behavior, and no installation procedure. Where a downstream document names a technology, it
> references this document rather than restating or extending it. Where this document and AEOS-PRD
> both speak to a subject, AEOS-PRD governs product behavior and this document governs only the
> technology choice made within the behavior AEOS-PRD already defines.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Relationship to Frozen Documents](#3-relationship-to-frozen-documents)
4. [Technology Scopes](#4-technology-scopes)
5. [Technology Principles](#5-technology-principles)
6. [Support Tiers](#6-support-tiers)
7. [Technology Categories](#7-technology-categories)
8. [Officially Supported Technologies by Category](#8-officially-supported-technologies-by-category)
9. [The Initial Official Technology Set](#9-the-initial-official-technology-set)
10. [Technology Lifecycle](#10-technology-lifecycle)
11. [Technology Evaluation Criteria](#11-technology-evaluation-criteria)
12. [Versioning Policy](#12-versioning-policy)
13. [Source of Truth Rules](#13-source-of-truth-rules)
14. [Future Evolution](#14-future-evolution)
15. [Document Governance](#15-document-governance)
16. [Appendix A — Technology Matrix](#appendix-a--technology-matrix)
17. [Appendix B — Technology Selection Decision Flow](#appendix-b--technology-selection-decision-flow)
18. [Appendix C — Governance Statement Index](#appendix-c--governance-statement-index)

---

## 1. Executive Summary

A product that intends to remain maintainable for a decade cannot leave its technology choices to
the moment each choice is needed. Technology decided per task is technology decided by whoever
happened to be present, for reasons nobody recorded, in a form nobody can review. The result is not
a stack; it is an accumulation. It is discovered years later, by people who did not choose it, in
the form of a migration nobody budgeted for.

Technology governance is how AEOS avoids that outcome. It states, in one place, which technologies
AEOS depends on, which it is tested and documented to work alongside, which it advises against, and
by what process any of those answers may change. A Contributor — human or AI Runtime — should never
have to guess, and should never have to negotiate the answer twice.

AEOS remains technology-neutral while officially supporting a curated technology set. Those two
statements are compatible, and the distinction between them is the most important thing in this
document.

| Neutrality means | Official support means |
| :--- | :--- |
| No Vendor, Runtime, Model, Platform, or Distribution Method is privileged or required. | AEOS documents, verifies, and maintains its work against a stated set of technologies. |
| A technology's absence never disables AEOS; it reduces the options available to the user. | A supported technology is one a user can rely on working, and whose breakage is a defect AEOS owns. |
| Being named here confers no privilege. Being unnamed implies no exclusion. | Support is a maintenance commitment, not an endorsement and not a recommendation to prefer one Vendor over another. |
| Users MAY use any technology they choose with AEOS. | Unlisted technologies are permitted and unsupported — never prohibited. |

The curated set exists because a commitment that covers everything covers nothing. Verification,
documentation, and cross-platform testing are finite. AEOS states where it has spent them so that
users know exactly what they are relying on, and so that the boundary of that reliance is a
published fact rather than an assumption.

---

## 2. Scope and Applicability

### 2.1 What This Document Governs

| Governed here | Section |
| :--- | :--- |
| Officially supported technologies, by category and support tier | [8](#8-officially-supported-technologies-by-category), [9](#9-the-initial-official-technology-set) |
| Technology selection principles | [5](#5-technology-principles) |
| Technology governance — who decides, and by what process | [13](#13-source-of-truth-rules), [15](#15-document-governance) |
| Technology lifecycle — how a technology enters, matures, and leaves the set | [10](#10-technology-lifecycle) |
| Technology categories and their selection criteria | [7](#7-technology-categories) |
| Evaluation criteria for technologies not yet in the set | [11](#11-technology-evaluation-criteria) |
| Versioning policy for supported technologies | [12](#12-versioning-policy) |

### 2.2 What This Document Does Not Govern

| Not governed here | Owned by |
| :--- | :--- |
| Structure, layers, components, and boundaries | Architecture documents |
| Code, algorithms, file layout, and dependency pins | The codebase and its tests |
| Project Templates and scaffolds | Template assets |
| Execution, lifecycle, and orchestration mechanics | Runtime documents |
| Installation and Distribution Methods | AEOS-PRD and distribution documents |
| Product requirements, capabilities, and scope | AEOS-PRD |
| The meaning of AEOS terms | AEOS-GLOSSARY |
| The form and lifecycle of AEOS documentation | AEOS-DOCSTD |

> **The reading rule.**
> A statement in this document that tells a reader *how to build something* is a defect in this
> document. It MUST be reported rather than acted upon. This document answers only *what may be
> built with*.

### 2.3 Applicability

This document applies to every technology decision made in the AEOS repository, and to every AEOS
document that names a technology. It applies identically to human and AI authors.

Support recorded here is a statement about AEOS, not about the technologies themselves. A user MAY
use any technology with AEOS, whether or not it appears in this document.

### 2.4 Recorded Deviations from AEOS-DOCSTD

AEOS-DOCSTD Section 7.3 states that a Technology Catalog SHOULD NOT use normative language. This
document deviates from that recommendation deliberately, and records the deviation here as
AEOS-DOCSTD Section 7.2 requires.

| Deviation | Reason | Status |
| :--- | :--- | :--- |
| Normative keywords are used throughout, in the governance statements identified `TG-001` to `TG-087`. | The statements bind how technologies enter, move through, and leave the supported set, and how downstream documents reference this one. Restating them without keywords would weaken obligations that AEOS-PRD and this document's own change control depend on. | Deliberate. Recorded for the owner's decision at review. |

Two further points of form are recorded so that a reader does not read them as error:

- This document places [Section 3](#3-relationship-to-frozen-documents) between its scope section and its principles, so that precedence is established before any principle is applied. AEOS-DOCSTD Appendix B rule O5 requires that such a departure be stated in the scope section.
- The support tier named **Not Recommended** is a label, not a normative keyword. It is written in title case and carries no RFC 2119 meaning.

---

## 3. Relationship to Frozen Documents

Four documents govern this one. This document introduces nothing that contradicts them and restates
nothing they already define.

### 3.1 Governing Documents

| Document | Governs | This document's obligation |
| :--- | :--- | :--- |
| AEOS-VISION | Purpose, philosophy, invariants. | No technology decision may trade away an invariant. A technology that would make AEOS perform inference, privilege a Vendor, or require a fork is excluded regardless of its merits. |
| AEOS-PRD | Product behavior, capabilities, requirements, scope. | Every technology decision here MUST be compatible with the requirements and product principles of AEOS-PRD. Where a technology choice would require weakening a requirement, the technology loses. |
| AEOS-GLOSSARY | Terminology. | Terms defined there are used here with their defined meaning and are not redefined. The support tiers, lifecycle states, and technology scopes recorded here are statuses this document owns, as AEOS-DOCSTD Section 4.3 states. |
| AEOS-DOCSTD | Document structure, formatting, and quality. | This document's structure, format, normative vocabulary, and review classification follow it, subject to the deviation recorded in [Section 2.4](#24-recorded-deviations-from-aeos-docstd). |

### 3.2 Precedence

| Situation | Resolution |
| :--- | :--- |
| A technology in this document conflicts with an AEOS-VISION invariant | The invariant governs. The entry is removed and the conflict is recorded as a defect in this document. |
| A technology in this document conflicts with an AEOS-PRD requirement | AEOS-PRD governs. The entry is corrected; the requirement is not reinterpreted. |
| A technology in this document conflicts with AEOS-GLOSSARY on the meaning of a term | AEOS-GLOSSARY governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-DOCSTD on documentation form | AEOS-DOCSTD governs, except where a deviation is recorded under [Section 2.4](#24-recorded-deviations-from-aeos-docstd). |
| An architecture, specification, or Template document names a technology absent from this document | This document governs. The downstream document is corrected, or the technology is proposed for evaluation under [Section 10](#10-technology-lifecycle). |
| Two entries in this document conflict with one another | Escalate to the owner. A Contributor reports the inconsistency and MUST NOT resolve it by preference. |

---

## 4. Technology Scopes

"Supported" does not mean the same thing for a technology AEOS is built on, a technology AEOS talks
to, and a technology a user's Project happens to use. Three scopes are distinguished so that a
single tier label cannot be misread.

| Scope | Meaning | What support obliges |
| :--- | :--- | :--- |
| **P — Product** | Technologies AEOS itself is built on, or requires in order to run. | AEOS takes a real dependency. The technology MUST be available on all three supported Platforms, and a break in it is a release-blocking defect. |
| **I — Integration** | External systems AEOS orchestrates and never contains: AI Runtimes, version control, delivery pipelines, and interoperability standards. | AEOS is documented and verified to orchestrate the technology. AEOS MUST remain whole and functional in its absence, and MUST NOT require any single entry. |
| **G — Governed project** | Technologies AEOS is documented and tested to govern inside a user's Project. | AEOS provides documentation and verification for governing work in that technology. Support is descriptive, never prescriptive: AEOS MUST NOT require a Project to adopt a listed technology, and MUST NOT refuse to govern a Project that uses an unlisted one. |

| ID | Governance statement |
| :--- | :--- |
| `TG-001` | Every entry in this document MUST carry at least one scope. An entry without a scope is incomplete. |
| `TG-002` | Scope **P** entries MUST be available and functionally equivalent on Windows, macOS, and Linux. A technology that is not MUST NOT be a Scope **P** dependency. |
| `TG-003` | No AEOS capability may depend on a Scope **I** entry. Unavailability of an integration reduces the user's options and MUST NOT corrupt Project state or disable non-inference capability. |
| `TG-004` | Scope **G** support is descriptive. AEOS MUST NOT require, prefer, or penalize a user Project's technology choice on the basis of this document. |
| `TG-005` | Listing a commercial Vendor and listing an open-source alternative in the same tier constitutes equal support. Order within a tier is alphabetical and confers no ranking. |

---

## 5. Technology Principles

These principles decide technology questions the way the product principles of AEOS-PRD decide
behavior questions. They are mandatory for technology selection and are subordinate to — never a
reinterpretation of — the product principles and invariants they serve.

The thirteen principles are stated in [Section 5.1](#51-vendor-neutral) through
[Section 5.13](#513-technology-may-evolve-philosophy-does-not). The rules that bind their
application are stated in [Section 5.14](#514-rules-of-application).

### 5.1 Vendor Neutral

No Vendor is privileged and no Vendor is required. Support for a Vendor's technology is a
maintenance commitment, not a partnership, a preference, or a default.

**In practice.** Where a category contains commercial Vendors, the set MUST contain more than one,
and MUST include at least one option the user can operate without a commercial relationship. A
category that can be satisfied by only a single Vendor MUST be recorded as a concentration risk in
that category's *Future evolution*.

### 5.2 Platform Neutral

Windows, macOS, and Linux are equal citizens. Technology selection is the most common way that
equality is lost, because cross-platform gaps are usually inherited rather than chosen.

**In practice.** A technology available on only one or two Platforms MAY be Conditionally Supported
for the Platforms where it exists. It MUST NOT be Preferred, and no AEOS capability may depend on
it.

### 5.3 Runtime Neutral

AEOS performs no inference and contains no Model. Every AI Runtime is an integration, chosen by the
user and replaceable by the user.

**In practice.** Switching between listed AI Providers, Models, or Inference Runtimes MUST NOT
require changes to a Project's Rules, Skills, Prompts, Workflows, or repository structure. A
technology whose adoption would make that untrue is excluded, however capable it is.

### 5.4 Open Standards First

Where an open, independently implementable specification exists and is adequate, it is chosen over a
proprietary equivalent, including a more capable one.

**In practice.** A standard qualifies as open when its specification is public, its evolution is not
controlled by a single Vendor's commercial interest, and at least two independent implementations
exist. Proprietary protocols with no published specification are Not Recommended by default.

### 5.5 Long-term Stability

The correct horizon for a technology decision is the lifetime of the Projects built on it, not the
period in which the decision was made.

**In practice.** Preference goes to technologies with a public release history, a stated
compatibility policy, and a record of handling their own breaking changes responsibly. A technology
that has broken its users without a migration path has told AEOS what to expect.

### 5.6 Maintainability over Novelty

A technology is adopted for what it costs to keep, not for what it costs to start.

**In practice.** Where two technologies are otherwise equivalent, the one that is plainer, more
widely understood, and easier for a stranger to maintain wins. Novelty MUST earn its place by
solving a problem the familiar option cannot.

### 5.7 Mature Ecosystems Preferred

Maturity is measured in years of production use, breadth of independent tooling, and the existence
of answers to ordinary questions that were not written by the supplier.

**In practice.** A technology with fewer than two years of stable public release history is
Experimental at best, whatever its rate of adoption.

### 5.8 Community Support

A technology maintained by one person, one company, or one funding round is a dependency on that
party's continued attention.

**In practice.** Preference goes to technologies with plural maintainership, public governance, and
a contribution process that a stranger has successfully used. Single-maintainer technologies MAY be
supported, and their concentration risk MUST be stated.

### 5.9 Education Friendly

AEOS is used by students, educators, and self-taught Developers, and a technology that is
inaccessible to them narrows who can participate in engineering.

**In practice.** Preference goes to technologies that are free to learn, installable on ordinary
hardware, documented in a form a beginner can follow, and observable enough that a learner can see
what happened rather than only that it worked.

### 5.10 Offline Friendly

Engineering happens on aircraft, in secure facilities, in classrooms with hostile networks, and in
countries where connectivity is expensive.

**In practice.** Non-inference capability MUST remain usable without network access. A technology
that requires a network round trip to perform a local operation, or that cannot be installed once
and reused, is Not Recommended for Scope **P**.

### 5.11 Local-first Where Practical

Where a capability can be delivered from the user's machine at acceptable cost, it should be.
Local-first protects privacy, cost, latency, and the user's ability to work when a service is
unavailable — none of which are recoverable once the dependency is embedded.

**In practice.** Where a hosted service and a local equivalent both satisfy a need, the local option
is Preferred and the hosted option is Supported. "Practical" is judged on capability and cost, not
on convenience of integration.

**Its limit.** This principle does not apply across the AI categories `TC-09`, `TC-10`, and `TC-11`,
where [Section 5.3](#53-runtime-neutral) and [Section 5.12](#512-commercial-ai-and-open-source-ai-treated-equally)
govern instead. Local inference is not ranked above hosted inference by AEOS; the choice between
them is the user's, made on privacy, cost, capability, and locality.

### 5.12 Commercial AI and Open-source AI Treated Equally

Commercial AI services and open-source Models are the same kind of thing to AEOS: external
inference, chosen by the user, replaceable by the user.

**In practice.** Both appear in the same categories, at the same tiers, evaluated by the same
criteria. Documentation MUST NOT present either as the normal case and the other as the fallback,
and MUST NOT assume a commercial provider is more capable or an open-source Model less serious.

### 5.13 Technology May Evolve; Philosophy Does Not

Every entry in this document is expected to change. Nothing in AEOS-VISION is.

**In practice.** A proposal that adds, promotes, demotes, or removes a technology is ordinary
governance. A proposal that requires an invariant to bend in order to accommodate a technology is
not a technology proposal at all, and is refused at this document's boundary rather than escalated.

### 5.14 Rules of Application

| ID | Governance statement |
| :--- | :--- |
| `TG-010` | Every technology entry MUST be justifiable against the principles in this section, and its rationale MUST be recorded alongside it. |
| `TG-011` | Where two principles conflict, the author MUST resolve in this order: Runtime Neutral, Platform Neutral, Vendor Neutral, Offline Friendly, Long-term Stability, then the remainder on the merits of the case. |
| `TG-012` | A technology proposal that requires an AEOS-VISION invariant to change MUST be refused, and MUST NOT be escalated. |

---

## 6. Support Tiers

A tier states how strong a commitment AEOS has made, and therefore what a user may rely on. The five
tiers below are the complete set.

| Tier | Definition | What the user may rely on | What AEOS owes |
| :--- | :--- | :--- | :--- |
| **Preferred** | The default choice within its category for AEOS work not yet started. A strict subset of Officially Supported. A category MAY have no Preferred entry. | This is what AEOS itself uses and documents first; choosing it requires no justification. | Documentation, cross-platform verification, worked examples, and a migration path if it is ever demoted. |
| **Officially Supported** | Verified on all three Platforms within its scope, documented, and maintained. | It works, and a break is a defect AEOS owns. | Documentation, verification, and release-blocking treatment of regressions. |
| **Conditionally Supported** | Supported subject to a stated condition: Platform availability, Project applicability, licensing, or external maintenance. | It works where the stated condition holds, and the condition is published rather than discovered. | Documentation including the condition. Regressions are tracked but are not release-blocking. |
| **Experimental** | Under evaluation. May change or be withdrawn without a deprecation cycle. | Nothing beyond the ability to try it. It MUST NOT be a dependency of any `P0` capability. | A statement that it is provisional, and a recorded evaluation outcome. |
| **Not Recommended** | AEOS advises against the technology for AEOS-related purposes and provides no assets, documentation, or verification for it. | Clarity. Use is permitted and unsupported. | A stated reason. Never a prohibition, and never a judgment of the technology's quality in general. |

| ID | Governance statement |
| :--- | :--- |
| `TG-020` | Every entry MUST carry exactly one tier. A Preferred entry is additionally Officially Supported by definition. |
| `TG-021` | A technology MUST satisfy every gating criterion in [Section 11.1](#111-gating-criteria) before it may be assigned any tier above Experimental. |
| `TG-022` | A Conditionally Supported entry MUST state its condition explicitly. Where a condition cannot be stated, the entry is Experimental. |
| `TG-023` | The author MUST write Not Recommended rationale as a statement about fit with AEOS, and MUST NOT write it as a judgment of a technology's quality or a Vendor's conduct. |
| `TG-024` | A technology absent from this document is unsupported, and MUST NOT be treated as prohibited. AEOS MUST NOT refuse to operate because a Project uses an unlisted technology. |

---

## 7. Technology Categories

AEOS organizes technology into twenty categories. This section states the purpose, selection
criteria, and expected evolution of each. Tier assignments are in
[Section 8](#8-officially-supported-technologies-by-category).

### 7.1 Category Index

The twenty categories below are the complete set.

| ID | Category | Scope | Detail |
| :--- | :--- | :--- | :--- |
| `TC-01` | Operating Systems | P · G | [7.2](#72-tc-01--operating-systems) |
| `TC-02` | Development Environments | I · G | [7.3](#73-tc-02--development-environments) |
| `TC-03` | Programming Languages | P · G | [7.4](#74-tc-03--programming-languages) |
| `TC-04` | Frameworks | G | [7.5](#75-tc-04--frameworks) |
| `TC-05` | Databases | G | [7.6](#76-tc-05--databases) |
| `TC-06` | ORM and Data Access | G | [7.7](#77-tc-06--orm-and-data-access) |
| `TC-07` | Package Managers | P · G | [7.8](#78-tc-07--package-managers) |
| `TC-08` | Containers | I · G | [7.9](#79-tc-08--containers) |
| `TC-09` | AI Providers | I | [7.10](#710-tc-09--ai-providers) |
| `TC-10` | Open-source Models | I | [7.11](#711-tc-10--open-source-models) |
| `TC-11` | Inference Runtimes | I | [7.12](#712-tc-11--inference-runtimes) |
| `TC-12` | Vector Databases | G | [7.13](#713-tc-12--vector-databases) |
| `TC-13` | MCP and Interoperability Standards | I | [7.14](#714-tc-13--mcp-and-interoperability-standards) |
| `TC-14` | Build Systems | P · G | [7.15](#715-tc-14--build-systems) |
| `TC-15` | Testing Frameworks | P · G | [7.16](#716-tc-15--testing-frameworks) |
| `TC-16` | Documentation | P · G | [7.17](#717-tc-16--documentation) |
| `TC-17` | CI/CD | I · G | [7.18](#718-tc-17--cicd) |
| `TC-18` | Version Control | I · G | [7.19](#719-tc-18--version-control) |
| `TC-19` | Monitoring | G | [7.20](#720-tc-19--monitoring) |
| `TC-20` | Security | P · G | [7.21](#721-tc-20--security) |

### 7.2 TC-01 — Operating Systems

**Scope:** Product · Governed project

**Purpose.** The host environments on which AEOS runs and on which governed Projects are built.

**Selection criteria.** An operating system is supported when AEOS can deliver identical product
capability on it, when its releases receive supplier security support, and when the tooling in every
other category is available on it. Support is stated per release family rather than per distribution
build, because distributions vary faster than the guarantees that matter.

**Future evolution.** The three supported families are stable and no change is anticipated. Growth
will occur inside them, as operating system suppliers publish further major releases and further
long-term-support lines, and is handled by the versioning policy rather than by adding categories.
Container-hosted and remote development hosts are treated as instances of these families, not as a
fourth Platform.

### 7.3 TC-02 — Development Environments

**Scope:** Integration · Governed project

**Purpose.** The editing surfaces Developers work in alongside AEOS.

**Selection criteria.** AEOS is editor-agnostic, so an environment is supported when AEOS can be
used from it without the environment holding product state. The decisive criterion is negative: an
environment that requires Rules, Workflows, Prompts, or Project state to live in its own
configuration rather than in the repository cannot be Officially Supported, because that arrangement
contradicts Repository as Product regardless of how convenient it is.

**Future evolution.** This category changes faster than any other, as AI-assisted editors appear,
merge, and are absorbed. The list will change often and the criterion will not. AEOS MUST remain
fully usable from a terminal with no editor at all, which is what keeps this category from becoming
a dependency.

### 7.4 TC-03 — Programming Languages

**Scope:** Product · Governed project

**Purpose.** The languages AEOS itself is written in, under Scope **P**, and the languages AEOS is
documented and tested to govern in user Projects, under Scope **G**.

**Selection criteria.** For Scope **P**: a cross-platform toolchain, a mature test runner, a stable
packaging story, and a large pool of maintainers, including maintainers who did not write the
original code. For Scope **G**: a mainstream test-first workflow must exist, because the TDD Cycle
is the default development workflow and a language without a credible test runner cannot be governed
test-first.

**Future evolution.** The Scope **P** set is deliberately small and is expected to stay that way;
adding an implementation language multiplies the maintenance surface permanently. The Scope **G**
set is expected to broaden steadily, driven by demand, and broadening it costs documentation and
verification rather than architecture.

### 7.5 TC-04 — Frameworks

**Scope:** Governed project

**Purpose.** Application frameworks used inside governed Projects.

**Selection criteria.** A framework is listed when AEOS can demonstrate that its Workflows, Rules,
and TDD Cycle apply cleanly to Projects built with it. Testability is the dominant criterion:
frameworks that resist test-first development are poor fits for AEOS regardless of their adoption.
AEOS produces no application framework of its own.

**Future evolution.** Expected to broaden as demand appears. The risk to watch is drift toward
opinionation: a long framework list can begin to read as a recommended Technology Stack, which is
not what it is. Each entry MUST remain clearly Scope **G**.

### 7.6 TC-05 — Databases

**Scope:** Governed project

**Purpose.** Persistent data stores used inside governed Projects.

**Selection criteria.** Local operability without a hosted account, a text-expressible schema and
migration story, and cross-platform availability. A database that cannot be run on a Developer's
machine offline fails Offline Friendly and cannot be Preferred.

**Future evolution.** Stable. Embedded and single-file databases are becoming more capable and are
expected to cover a growing share of real Projects, which suits Local-first Where Practical.

### 7.7 TC-06 — ORM and Data Access

**Scope:** Governed project

**Purpose.** The layer through which governed Projects express and evolve their data access. ORM
abbreviates object-relational mapping.

**Selection criteria.** Migrations MUST be expressible as reviewable, diffable, version-controlled
artifacts. This is the criterion that matters most, because migrations are where data loss lives.
Beyond that: the ability to drop to raw queries, and behavior that a maintainer can predict without
reading the library's source.

**Future evolution.** Expected to broaden cautiously. An ORM is a deep commitment for a Project and
a shallow one for AEOS, so the list stays short and the rationale stays explicit. Using no ORM is
always a valid choice and is never treated as a gap.

### 7.8 TC-07 — Package Managers

**Scope:** Product · Governed project

**Purpose.** Dependency resolution, installation, and reproducibility for AEOS itself and for
governed Projects.

**Selection criteria.** Deterministic lockfiles, an offline or cached installation mode,
cross-platform behavioral parity, and a lockfile format that reviews sensibly in a change request.
Reproducibility is the whole purpose of this category; a package manager that resolves differently
on two machines has failed its only job.

**Future evolution.** Active. This category has changed substantially in both the Python and
JavaScript ecosystems, and further consolidation is likely. AEOS expects to revisit it more often
than most categories, and the versioning policy exists partly to absorb that.

### 7.9 TC-08 — Containers

**Scope:** Integration · Governed project

**Purpose.** Isolated, reproducible environments for development, verification, and delivery.

**Selection criteria.** Conformance to open container specifications, cross-platform availability,
and the ability to be used without a commercial subscription for individual and educational use. The
governing constraint: containers MUST remain optional, because locked-down machines, air-gapped
environments, and shared classroom systems frequently cannot run them.

**Future evolution.** Stable and standard-driven. Alternative runtimes conforming to the same
specifications are expected to gain support as they mature, precisely because the specifications
make substitution inexpensive.

### 7.10 TC-09 — AI Providers

**Scope:** Integration

**Purpose.** Commercial services that perform inference on behalf of the user. They are
integrations. AEOS performs no inference of its own under any circumstance.

**Selection criteria.** A documented and versioned interface, a stated deprecation policy,
availability to individual Developers without enterprise negotiation, and — decisively —
substitutability: the provider MUST be replaceable by another listed provider without changing a
Project's Rules, Skills, Prompts, Workflows, or repository structure. A provider whose integration
would require special-case handling in a Project's assets fails this criterion no matter how capable
it is.

**Future evolution.** The most volatile category in this document. Providers will consolidate,
disappear, change pricing, and be replaced. AEOS expects to absorb that change rather than track it,
and the value of this category is measured by how little a change here disturbs a Project.

### 7.11 TC-10 — Open-source Models

**Scope:** Integration

**Purpose.** Openly licensed Model weights the user may run themselves. They are first-class
Runtimes, not fallbacks.

**Selection criteria.** Openly published weights under a license permitting commercial and
educational use, availability across a range of sizes so that ordinary hardware is viable, an active
release lineage, and support by more than one listed Inference Runtime.

**Future evolution.** Fast-moving and improving. Model families listed here will be superseded by
their own successors, which is expected and is handled by the versioning policy rather than by
relisting. AEOS is not tuned to any Model family and MUST NOT become so.

### 7.12 TC-11 — Inference Runtimes

**Scope:** Integration

**Purpose.** The local and self-hosted systems that execute open-source Models.

**Selection criteria.** Support for multiple Model families, an installation path an ordinary
Developer can complete, and offline operation once Models are present. Cross-platform availability
determines the tier ceiling: a runtime that exists on one Platform may be useful and MUST NOT be
Preferred.

**Future evolution.** Active and consolidating around a small number of serving stacks. Hardware
acceleration remains the main source of Platform asymmetry, and is the reason several entries in
this category are conditional rather than official.

### 7.13 TC-12 — Vector Databases

**Scope:** Governed project

**Purpose.** Similarity search over embeddings inside governed Projects.

**Selection criteria.** The strongest preference in this category is for capability added to an
already-supported database rather than for a new system: a Project that gains vector search without
gaining a database gains less operational cost. Beyond that: local operation without a hosted
account, and persistence in an inspectable form.

**Future evolution.** Consolidating. The absorption of vector search into general-purpose databases
is expected to continue, which will likely shrink rather than grow this category — a good outcome
for maintainability.

### 7.14 TC-13 — MCP and Interoperability Standards

**Scope:** Integration

**Purpose.** Open specifications through which AEOS and AI Runtimes interoperate without
Vendor-specific coupling. MCP abbreviates Model Context Protocol.

**Selection criteria.** A public specification, evolution not controlled by a single Vendor's
commercial interest, at least two independent implementations, and a versioning policy for the
specification itself. This category serves Open Standards First directly, and its entries are held
to that principle more strictly than anywhere else.

**Future evolution.** Standards in this space are young. AEOS expects the present generation to be
extended and eventually superseded, and expects to support more than one standard at a time during
transitions. Supporting a standard MUST NOT make it required: no product capability may be exclusive
to any Distribution Method or interoperability standard.

### 7.15 TC-14 — Build Systems

**Scope:** Product · Governed project

**Purpose.** Turning source into runnable and distributable artifacts, and running repeatable
development tasks.

**Selection criteria.** Identical invocation and identical results on all three Platforms. This
criterion eliminates more candidates than any other in this category, because a great many build and
task tools assume a POSIX shell. Beyond that, language-native tooling is preferred over a general
build system, on the grounds that fewer layers are easier to maintain.

**Future evolution.** Stable for single-language Projects. Monorepo and multi-language orchestration
is the area most likely to change, and is deliberately kept at a lower tier until a cross-platform
option proves itself over several years.

### 7.16 TC-15 — Testing Frameworks

**Scope:** Product · Governed project

**Purpose.** Executing the tests that make test-first development real. This is the most
consequential category in the document: the TDD Cycle is the default development workflow, and a
Project's test runner is the mechanism through which that default is enforced.

**Selection criteria.** A machine-readable result format, a non-zero exit code on failure, failure
output detailed enough to act on, deterministic ordering options, and cross-platform parity. AEOS
orchestrates the Project's own test tooling and provides no test framework of its own.

**Future evolution.** Stable. Runners are long-lived and their result formats are converging on
common standards, which is what allows AEOS to orchestrate them without special-casing each one.

### 7.17 TC-16 — Documentation

**Scope:** Product · Governed project

**Purpose.** The formats in which AEOS documentation and governed-Project documentation are
authored, reviewed, and consumed by humans and AI Runtimes from the same artifact.

**Selection criteria.** Plain-text source, meaningful diffs, correct rendering in the environments
AEOS-DOCSTD names, and readability by an AI Runtime without a separate machine-only version
existing. A format that requires a build step to be readable, or that only reviews as a binary blob,
cannot be a source of truth here.

**Future evolution.** Stable. Documentation sites are a presentation concern and may change freely;
the source format is expected to remain plain text indefinitely, because the two-audience
requirement admits nothing else.

### 7.18 TC-17 — CI/CD

**Scope:** Integration · Governed project

**Purpose.** Continuous integration and continuous delivery systems that AEOS orchestrates and never
replaces.

**Selection criteria.** Configuration expressed as version-controlled text in the Project's own
repository, hosted runners for all three Platforms, and the ability to run the same pipeline locally
or on self-hosted infrastructure. A pipeline whose definition lives in a web console cannot be
Officially Supported, because the Project's own truth would then live outside the Project.

**Future evolution.** Stable in shape, competitive in market. Broadening this list costs
documentation and verification, and the neutrality obligation of AEOS makes broadening it a matter
of demand rather than preference.

### 7.19 TC-18 — Version Control

**Scope:** Integration · Governed project

**Purpose.** The system of record for the repository, which is the Product.

**Selection criteria.** Distributed operation with complete local history, content-addressed
integrity, offline operation, and universal tooling support. Hosting platforms are evaluated
separately from the version control system itself, and a Project with no remote at all MUST remain
fully supported.

**Future evolution.** The most stable category in this document. Hosting platforms may change; the
underlying version control system is not expected to.

### 7.20 TC-19 — Monitoring

**Scope:** Governed project

**Purpose.** Observability of governed Projects in the environments their owners choose.

**Selection criteria.** Open instrumentation standards, a self-hosted option, and the ability to
operate without transmitting Project content to a third party. This category is governed by a
constraint rather than a preference: AEOS ships no default telemetry, and monitoring is opt-in and
user-owned. Telemetry is Runtime State — it describes usage, never the Product — and MUST NOT become
a Repository Asset.

**Future evolution.** Converging on open instrumentation standards, which is the outcome AEOS wants:
Vendor-neutral instrumentation makes the backend an ordinary, replaceable choice.

### 7.21 TC-20 — Security

**Scope:** Product · Governed project

**Purpose.** Credential handling, dependency and secret scanning, provenance, and supply-chain
integrity for AEOS and for governed Projects.

**Selection criteria.** Local execution without uploading source to a third party by default, open
and standard output formats, cross-platform availability, and free availability for individual and
educational use. The absolute constraint: credentials and secrets are never written into Prompts,
logs, reports, documentation, or Repository Assets, and no technology may be listed that would
require otherwise.

**Future evolution.** Active, standards-driven, and increasingly required by regulation. Software
bills of materials and artifact signing are expected to move from good practice to expectation, and
this category will grow accordingly.

---

## 8. Officially Supported Technologies by Category

This section records the tier assignments. Rationale accompanies each assignment; the extended
reasoning for the headline choices is in [Section 9](#9-the-initial-official-technology-set).

### 8.1 How to Read These Tables

- Order within a tier is alphabetical and confers no ranking, as `TG-005` requires.
- The entries listed at each tier are the complete set for that tier at this version of this document. A technology absent from a table is unsupported, not prohibited, as `TG-024` states.
- *None* in a Preferred row is a recorded decision, not an omission awaiting a value. Where the absence is a matter of policy rather than of present evidence, the row states *None by policy*.
- Where this section and [Appendix A](#appendix-a--technology-matrix) differ, this section governs.

### 8.2 TC-01 — Operating Systems

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | Naming a preferred Platform would contradict the equal-citizen commitment. This absence is deliberate and permanent. |
| **Officially Supported** | Linux · macOS · Windows | All three host serious engineering work, receive supplier security support, and carry the tooling in every other category. Supported releases are those receiving supplier security support, as `TG-062` states. |
| **Conditionally Supported** | Rolling-release Linux distributions · WSL2 | WSL2 is supported *as a Linux environment* and never as a substitute for native Windows support. Rolling releases move faster than verification can follow, and are supported where the user accepts that. |
| **Experimental** | BSD family | Technically plausible, not verified. |
| **Not Recommended** | Operating system releases past supplier end-of-life | Unpatched hosts cannot be secured, and verification against them would misrepresent what AEOS can promise. |

### 8.3 TC-02 — Development Environments

**Scope:** Integration · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | Editors are a matter of long-held personal preference. Preferring one would make AEOS an adoption obstacle for everyone using another. |
| **Officially Supported** | Cursor · Terminal or shell with no editor · Visual Studio Code · Windsurf | The three named editors are cross-platform, keep Project state in the repository, and are widely used for AI-assisted work. The terminal entry is listed first-class deliberately: it is the guarantee that this category never becomes a dependency. |
| **Conditionally Supported** | Emacs · GitHub Copilot, as an assistant within a supported editor · JetBrains IDEs · Neovim · Zed | All are capable and cross-platform; none is covered by AEOS verification. Condition: the user accepts documentation-only support. |
| **Experimental** | Browser-hosted and remote development environments | Promising for education and locked-down machines; local tooling assumptions are not yet verified against them. |
| **Not Recommended** | Environments that require Rules, Workflows, Prompts, or Project state to live in editor-owned configuration | Product state outside the repository contradicts Repository as Product. This is a fit judgment about state ownership, not about editor quality. |

### 8.4 TC-03 — Programming Languages

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | Python, Scope P · G · TypeScript, Scope P · G | Between them they cover the tooling, service, and interface work AEOS performs and governs, with mature test-first ecosystems on all three Platforms. |
| **Officially Supported** | Python · TypeScript | Deliberately equal to the Preferred set. A small Scope **P** language set is a permanent maintenance decision, not a starting position. |
| **Conditionally Supported** | C#, Scope G · Go, Scope G · Java, Scope G · JavaScript, Scope G · Rust, Scope G | All have strong test-first ecosystems and are common in real Projects AEOS will be asked to adopt. Condition: Scope **G** only — AEOS governs Projects in these languages and is not written in them. |
| **Experimental** | Other languages with a mainstream test runner | Governance is plausible; documentation and verification do not exist. |
| **Not Recommended** | Languages with no maintained cross-platform toolchain, or no credible automated test runner | The TDD Cycle is the default workflow. A language that cannot be driven test-first cannot be governed by AEOS as designed. |

### 8.5 TC-04 — Frameworks

**Scope:** Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | Framework choice belongs to the Project. A preferred framework would read as a recommended Technology Stack, which this document does not supply. |
| **Officially Supported** | FastAPI · React · React Native | One service framework and one interface family, each testable without a running browser or server, each with large independent ecosystems. React Native extends the same interface model to mobile without introducing a second paradigm. |
| **Conditionally Supported** | Django · Express · Flask · Next.js · Svelte · Vue | All are mature and testable. Condition: documentation-only support, and framework-specific build conventions are the Project's responsibility. |
| **Experimental** | Frameworks with fewer than two years of stable release history | Rate of adoption is not maturity. Evaluated on the standard cycle. |
| **Not Recommended** | Frameworks whose primary workflow is a hosted visual builder, or that cannot be tested without a proprietary service | Being untestable without a service contradicts test-first development and Offline Friendly. This is not a comment on their suitability elsewhere. |

### 8.6 TC-05 — Databases

**Scope:** Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | PostgreSQL · SQLite | SQLite for embedded, zero-administration, fully offline work; PostgreSQL for everything that outgrows it. The pair covers most of the range with one dialect family and one migration story. |
| **Officially Supported** | PostgreSQL · SQLite | Both are open source, cross-platform, decades old, exhaustively documented, and runnable on a laptop without an account. |
| **Conditionally Supported** | DuckDB · MariaDB · MySQL | Mature and locally operable. Condition: documentation-only support; DuckDB is analytical rather than transactional and should be chosen accordingly. |
| **Experimental** | Distributed SQL and embedded alternatives with wire compatibility to a supported database | Compatibility makes evaluation inexpensive; verification has not been performed. |
| **Not Recommended** | Databases with no local development mode, or whose schema and migrations cannot be expressed as version-controlled text | A Project whose schema lives in a console cannot be reproduced from its repository. |

### 8.7 TC-06 — ORM and Data Access

**Scope:** Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None* | Data access is among the most consequential decisions a Project makes, and the least suitable for a default supplied by a governance document. Using no ORM is a valid choice. |
| **Officially Supported** | Alembic, for migrations · SQLAlchemy | Long-lived, extensively documented, works with both Preferred databases, and expresses migrations as reviewable version-controlled files — the criterion that matters most in this category. |
| **Conditionally Supported** | Drizzle ORM · Prisma · SQLModel | All express migrations as reviewable artifacts. Condition: documentation-only support; the generated client and engine of Prisma add a build step Projects must account for. |
| **Experimental** | Query builders and lightweight mappers in supported languages | Frequently a better fit than a full ORM; not documented or verified. |
| **Not Recommended** | Tools that apply schema changes without producing a reviewable migration artifact | Unreviewable schema change is where irrecoverable data loss originates. This is the strongest recommendation against anything in this document. |

### 8.8 TC-07 — Package Managers

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | pnpm, for JavaScript and TypeScript · uv, for Python | Both produce deterministic lockfiles, install from cache offline, and behave identically on all three Platforms — the three properties this category exists to guarantee. |
| **Officially Supported** | pnpm · uv | Reproducibility is not a preference in AEOS; it is a quality attribute. These are the tools that deliver it most reliably. |
| **Conditionally Supported** | npm · pip with virtual environments · Poetry · Yarn | All are widespread and will be encountered in adopted Projects, which AEOS must handle gracefully. Condition: a committed lockfile MUST be present. |
| **Experimental** | Emerging resolvers in supported language ecosystems | This category has moved substantially and is expected to continue. |
| **Not Recommended** | Workflows without a committed lockfile · Global installation as a Project's dependency strategy | Without a lockfile, no two machines are running the same Project, and reproducibility claims become untrue without anyone noticing. |

### 8.9 TC-08 — Containers

**Scope:** Integration · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | Containers MUST remain optional. A Preferred container technology would imply a default that many supported environments cannot run. |
| **Officially Supported** | Docker · Docker Compose | The most widely available implementation of the open container specifications on all three Platforms, and the one Contributors are most likely to already have. |
| **Conditionally Supported** | containerd · Podman | Specification-conformant and often preferable on Linux and in restricted environments. Condition: cross-platform behavioral parity is the user's responsibility. |
| **Experimental** | Development-container specifications for environment definition | Attractive for teaching and onboarding; not verified across all three Platforms. |
| **Not Recommended** | Making containers a prerequisite for any AEOS capability | Air-gapped, locked-down, and shared classroom machines frequently cannot run them. Requiring containers would exclude exactly the users Offline Friendly protects. |

### 8.10 TC-09 — AI Providers

**Scope:** Integration

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy, permanently* | A Preferred AI Provider would convert Vendor independence from a structural fact into a marketing position. This is the load-bearing absence in this document. |
| **Officially Supported** | Anthropic · Google · OpenAI | Each publishes a documented, versioned interface with a stated deprecation policy, is available to individual Developers without enterprise negotiation, and is substitutable for the others without changing a Project's assets. Listed alphabetically; the ordering carries no meaning. |
| **Conditionally Supported** | Managed enterprise endpoints for a supported provider · Providers offering an interface compatible with a supported provider | Condition: availability, quotas, regional restrictions, and Model catalog are the operator's responsibility, not the responsibility of AEOS. Compatibility layers are supported to the extent they are genuinely compatible. |
| **Experimental** | Commercial providers meeting the gating criteria and not yet evaluated | Evaluated on the standard cycle. Entry is expected to be routine, not exceptional. |
| **Not Recommended** | Providers with no published interface stability or deprecation policy, or whose terms prohibit substitution | An integration that cannot be replaced is a dependency acquired by accident — the exact failure mode independence exists to prevent. |

> AEOS performs no inference. Every entry in this category is external, user-selected,
> user-approved before invocation, and replaceable. The absence of every provider listed here
> reduces the user's options and disables nothing.

### 8.11 TC-10 — Open-source Models

**Scope:** Integration

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | Model selection belongs to the user and depends on hardware, privacy, cost, and task. AEOS is not tuned to any Model family and MUST NOT become so. |
| **Officially Supported** | Gemma · Llama · Mistral · Qwen | Each publishes openly licensed weights across several sizes, has an active release lineage, and is runnable by more than one supported Inference Runtime. The size range matters: it is what makes these usable on a student's laptop and on a server. |
| **Conditionally Supported** | Other openly licensed families with permissive commercial and educational terms | Condition: the user verifies license terms for their use, which vary meaningfully between families and releases. |
| **Experimental** | Model families not yet evaluated · Specialized code Models | Frequently excellent and frequently short-lived. Evaluated on the standard cycle. |
| **Not Recommended** | Weights published without clear license terms, or under terms prohibiting commercial or educational use | Unclear licensing transfers legal risk to the user silently. This is a statement about terms, not about quality. |

### 8.12 TC-11 — Inference Runtimes

**Scope:** Integration

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | Hardware determines the right answer here more than governance can. Preferring one would penalize users whose machines suit another. |
| **Officially Supported** | LM Studio · Ollama · Transformers | All three run on all three Platforms and support all four supported Model families. Ollama offers the simplest offline path, LM Studio the most approachable one for learners, and Transformers the reference implementation everything else is measured against. |
| **Conditionally Supported** | llama.cpp · MLX · vLLM | Condition: Platform and hardware limits. vLLM targets server-class Linux with accelerators; MLX runs on macOS on Apple silicon; llama.cpp is broadly portable but requires more assembly. Each is excellent within its condition, and none may be Preferred while the condition holds. |
| **Experimental** | Serving stacks with compatible interfaces, not yet evaluated | Interface compatibility makes substitution inexpensive and evaluation cheap. |
| **Not Recommended** | Runtimes requiring a network round trip to serve a locally stored Model | This defeats the reason to run locally: privacy, offline capability, and cost. |

### 8.13 TC-12 — Vector Databases

**Scope:** Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | pgvector · sqlite-vec | Both add vector search to a database the Project already supports, runs, backs up, and understands. Gaining a capability without gaining a system is the best available outcome in this category. |
| **Officially Supported** | pgvector · sqlite-vec | Open source, local-first, cross-platform, and persisted in an inspectable form alongside the Project's other data. |
| **Conditionally Supported** | Chroma · Qdrant | Both offer genuine local modes and are appropriate at scales the embedded options do not reach. Condition: the Project accepts operating an additional system. |
| **Experimental** | Other locally operable vector stores | A fast-moving field that is consolidating; evaluation is deferred rather than declined. |
| **Not Recommended** | Hosted-only vector services with no local development mode | These cannot be developed against offline, and make a Project's data layer unreproducible from its repository. |

### 8.14 TC-13 — MCP and Interoperability Standards

**Scope:** Integration

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | MCP | The open, independently implemented, Vendor-neutral standard addressing Runtime interoperability at the level AEOS needs. Preferred here because standards, unlike Vendors, are the thing this document is meant to prefer. |
| **Officially Supported** | MCP · OpenAPI-described interfaces | Both are public specifications with multiple independent implementations and their own versioning policies. OpenAPI covers integration surfaces that predate and sit outside MCP. |
| **Conditionally Supported** | Language Server Protocol · Vendor extensions to a supported standard | Condition: an extension is supported only where the base standard remains sufficient without it. The Language Server Protocol is relevant to editor integration rather than to inference. |
| **Experimental** | Agent and tool interoperability specifications not yet evaluated | The field is young. AEOS expects to support more than one standard concurrently during transitions. |
| **Not Recommended** | Proprietary protocols with no published specification or no independent implementation | A protocol only one party can implement is that party's product, not a standard, and adopting it would import the coupling AEOS exists to avoid. |

> Support for a standard never makes it required. No product capability is exclusive to MCP, to any
> other standard, or to any Distribution Method.

### 8.15 TC-14 — Build Systems

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | Language-native build tooling, invoked by the language's package manager | Fewer layers, no shell assumptions, and identical behavior on all three Platforms. Preferring the native path is preferring the plainer solution. |
| **Officially Supported** | Language-native build tooling · Vite | Vite is the front-end build path used with the supported interface frameworks; it is cross-platform, configuration-light, and fast enough that verification stays inexpensive. |
| **Conditionally Supported** | just · Make | Condition: cross-platform parity is the Project's responsibility. Make in particular assumes a POSIX shell, which is exactly the assumption this category is most alert to. |
| **Experimental** | Monorepo orchestration systems | Real value at scale, significant complexity, and cross-platform behavior that has not been verified over enough years. |
| **Not Recommended** | Build definitions requiring a Platform-specific shell or non-portable path handling | The most common way Platform equality is lost in practice, and the hardest to notice until a Contributor on another Platform cannot build. |

### 8.16 TC-15 — Testing Frameworks

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | Playwright, for end-to-end testing · pytest, for Python · Vitest, for TypeScript unit and integration testing | One preferred runner per layer, each with machine-readable results, actionable failure output, and cross-platform parity. These are the runners the test-first development of AEOS itself uses. |
| **Officially Supported** | Jest · Playwright · pytest · Vitest | Jest is Officially Supported rather than Preferred: it is present in a large number of existing Projects AEOS will be asked to adopt, and adoption safety matters more than tidiness. TypeScript work not yet started should prefer Vitest. |
| **Conditionally Supported** | Cypress · Testing Library · unittest | Condition: documentation-only support. Testing Library complements rather than replaces a runner and is listed for clarity about that relationship. |
| **Experimental** | Property-based testing libraries · Runners in supported languages not yet evaluated | Property-based testing is a strong fit for the philosophy of AEOS and is not documented in AEOS assets. |
| **Not Recommended** | Runners without machine-readable output, or that do not return a non-zero exit code on failure | AEOS gates workflow progression on test results. A runner that cannot report failure unambiguously cannot be gated on, which makes it unusable for the central discipline of the Product. |

### 8.17 TC-16 — Documentation

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | GitHub-Flavored Markdown, as AEOS-DOCSTD requires | Plain text, meaningful diffs, correct rendering in the environments AEOS-DOCSTD names, and directly consumable by AI Runtimes — one artifact serving both audiences, which is the requirement no other format meets as well. |
| **Officially Supported** | GitHub-Flavored Markdown · Mermaid, for diagrams | Mermaid keeps diagrams in text: reviewable in a change request, readable by an AI Runtime, and rendered without a build step. |
| **Conditionally Supported** | Docusaurus · MkDocs · Sphinx | Condition: presentation only. A documentation site MAY be generated from repository sources; it MUST NOT become the source of truth. |
| **Experimental** | Structured documentation formats with stronger machine semantics | Potentially valuable for AI consumption; these must not cost human readability, which no candidate has demonstrated. |
| **Not Recommended** | Binary or proprietary document formats as a source of truth · Externally hosted wikis as a source of truth | Neither diffs, neither reviews, and neither survives the tool that produced it. Both contradict Repository as Product. |

### 8.18 TC-17 — CI/CD

**Scope:** Integration · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | *None by policy* | A Project's pipeline is deeply embedded in how its organization operates. Preferring one would make AEOS adoption a migration. |
| **Officially Supported** | GitHub Actions | Configuration lives as text in the repository, hosted runners exist for all three Platforms — which is what allows AEOS to verify Platform equality rather than assert it — and it is available to open-source Projects at no cost. |
| **Conditionally Supported** | Azure Pipelines · CircleCI · GitLab CI · Jenkins | All express pipelines as version-controlled text. Condition: documentation-only support; Platform runner availability is the operator's responsibility. |
| **Experimental** | Local pipeline emulation for offline verification | Valuable for Offline Friendly; fidelity to hosted execution is not established. |
| **Not Recommended** | Pipelines defined only in a web console rather than in the repository | The Project's own build truth would live outside the Project, and could not be reviewed, diffed, or reproduced. |

> AEOS orchestrates the Project's delivery systems and never replaces them. Deployment is a
> destructive action requiring explicit, specific confirmation every time, regardless of which
> system in this category performs it.

### 8.19 TC-18 — Version Control

**Scope:** Integration · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | Git | Distributed, complete local history, content-addressed integrity, fully offline, universally tooled, and effectively without an alternative in practice. The repository is the Product, and Git is how it endures. |
| **Officially Supported** | Git · GitHub, for hosting | Git is the system of record. GitHub is supported as a hosting platform because AEOS documentation targets its rendering and its automation surfaces — a documentation and verification commitment, not a requirement. |
| **Conditionally Supported** | Azure Repos · Bitbucket · Gitea · GitLab | All host Git faithfully. Condition: platform-specific automation and rendering are the Project's responsibility. |
| **Experimental** | Git-compatible version control front-ends | Interesting where they preserve full Git interoperability, which is the only property that matters for support. |
| **Not Recommended** | Centralized version control without complete local history | Offline work, local verification, and Repository as Product all depend on the full history being present on the machine. |

> A repository with no remote is fully supported. Hosting is a convenience, never a requirement,
> and no AEOS capability may depend on the presence of one.

### 8.20 TC-19 — Monitoring

**Scope:** Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | Structured local logging under the Project's control | The least that suffices, owned entirely by the user, requiring no network and no third party. Observability should start here and grow only when a Project genuinely needs more. |
| **Officially Supported** | OpenTelemetry, for instrumentation · Structured local logging | OpenTelemetry is Vendor-neutral instrumentation with a self-hosted path, which makes the backend an ordinary replaceable choice rather than a lock-in point. |
| **Conditionally Supported** | Grafana · Prometheus · Sentry | Condition: opt-in, Project-owned, and configured so that Project content and credentials are not transmitted without explicit approval. |
| **Experimental** | Local-first observability backends | Aligned with Local-first Where Practical; not verified across all three Platforms. |
| **Not Recommended** | Any monitoring that transmits Project content off-machine by default · Default-on telemetry in AEOS itself | AEOS ships no default telemetry. Anything crossing the machine boundary is disclosed and approved first, without exception. |

### 8.21 TC-20 — Security

**Scope:** Product · Governed project

| Tier | Technologies | Rationale |
| :--- | :--- | :--- |
| **Preferred** | Platform-native credential stores: Linux Secret Service · macOS Keychain · Windows Credential Manager | Credentials belong to a person or an organization, never to a repository. The Platform's own store is the mechanism the user already trusts and already controls. |
| **Officially Supported** | Dependency auditing built into supported package managers · gitleaks · Platform-native credential stores · Trivy | All run locally without uploading source, produce machine-readable output, and are available on all three Platforms at no cost to individuals and educators. |
| **Conditionally Supported** | CycloneDX and SPDX software bills of materials · Dependabot · Renovate · Sigstore artifact signing | Condition: several depend on a hosting platform or a public transparency service, so availability follows the Project's hosting choices rather than the choices of AEOS. |
| **Experimental** | Commercial static analysis and supply-chain platforms | Often valuable; evaluated case by case against the criterion that source is not uploaded by default. |
| **Not Recommended** | Storing credentials or secrets in Repository Assets, in committed environment files, or in documentation · Scanners that transmit source without explicit approval | This is absolute. Secrets are never written into Prompts, logs, reports, documentation, or Repository Assets, and no technology may be adopted that would require otherwise. |

---

## 9. The Initial Official Technology Set

The complete tier assignments are in [Section 8](#8-officially-supported-technologies-by-category)
and summarized in [Appendix A](#appendix-a--technology-matrix). This section explains why the
headline choices were made, because a list without reasoning cannot be argued with, corrected, or
inherited.

Read together, the set expresses one preference above all others: the smallest number of
technologies that can honestly cover the work, each chosen because it will still be maintainable
when nobody involved in choosing it is still present.

### 9.1 Why Windows, macOS, and Linux, with No Preferred Platform

All three host serious engineering work, and each hosts Developers accustomed to being second-class
somewhere. Naming a preferred Platform would make that experience official.

The practical consequence is felt in every other category: it eliminates otherwise excellent tools
that assume a POSIX shell, and it is the reason several Inference Runtimes and build tools sit at
Conditionally Supported despite being the best available option on the Platform where they run. The
cost is real and is paid deliberately.

### 9.2 Why Visual Studio Code, Cursor, and Windsurf, and Why the Terminal Sits Alongside Them

The three named editors are cross-platform, widely used for AI-assisted work, and — decisively — do
not require Project state to live in editor configuration.

The terminal entry matters more than the editors. It is the guarantee that this category never
becomes a dependency: a Contributor with none of these installed, on a machine where none can be
installed, can still use AEOS completely. Every editor in this category is a convenience, and
listing the terminal at the same tier is how that stays true as the category changes.

### 9.3 Why Python and TypeScript

Two languages, chosen to cover the range with the smallest permanent maintenance surface.

Python brings the strongest ecosystem for tooling, automation, and AI-adjacent work, a test-first
culture of long standing, and a low barrier for learners and researchers. TypeScript brings type
safety to the interface and service layers, is the default of the web and mobile ecosystems AEOS
governs, and its type system makes generated code more reviewable — which matters
disproportionately when a substantial share of code is produced by an AI Runtime.

Both are cross-platform, both have first-class test runners, and both are read fluently by every
AI Runtime AEOS integrates with, which is a real criterion when AI Runtimes are first-class readers
of the repository. A third implementation language would multiply the maintenance surface
permanently in exchange for a benefit that has not been demonstrated.

### 9.4 Why FastAPI, React, and React Native

One service framework and one interface family, both testable without a running server or browser —
the property that determines whether a framework can be governed test-first at all.

The explicitness of FastAPI suits AI-assisted work: its type-driven interfaces make generated code
easier to review, and its documentation generation follows from the code rather than being
maintained beside it. The component model of React tests cleanly at the unit level and is the
largest independent interface ecosystem. React Native extends the same model to mobile without
asking a Project to learn a second paradigm, which is the reason it is here rather than a native
mobile toolkit, since neither of those can be developed on all three Platforms.

These are Scope **G** entries. A Project using none of them is fully supported, and this list is not
a recommended Technology Stack.

### 9.5 Why SQLite and PostgreSQL

A pair that covers the range from a single file to production scale within one dialect family and
one migration story.

SQLite is the most widely deployed database in existence, requires no server, no account, and no
network, and makes fully offline development ordinary rather than special. PostgreSQL is where a
Project goes when it outgrows that: open source, decades old, exhaustively documented, and available
from every hosting provider without Vendor coupling. It also carries the Preferred vector extension,
which lets a Project add similarity search without adding a system.

Both are open source, both run on all three Platforms, and both can be operated by a student on a
laptop and by an organization in production without changing the Project's data access.

### 9.6 Why uv and pnpm

Reproducibility is a stated quality attribute of AEOS, and this category is where reproducibility is
won or lost.

Both produce deterministic lockfiles, install from a local cache without a network, and behave
identically on all three Platforms. Both are substantially faster than their predecessors, which
sounds like a convenience and is not: a slow install is a slow verification cycle, and a slow
verification cycle is the most common reason a team quietly stops running the full suite.

Their predecessors remain Conditionally Supported because adopted Projects will use them, and
adoption safety outranks tidiness in every judgment this document makes.

### 9.7 Why Docker, and Why Containers Are Never Required

Docker is the most widely available implementation of the open container specifications on all three
Platforms and the one a Contributor is most likely to already have.

The more important half of the decision is the constraint. Containers MUST remain optional, because
locked-down corporate machines, air-gapped environments, and shared classroom systems frequently
cannot run them — and those are precisely the users Offline Friendly and Education Friendly exist to
protect. The conformance of Docker to open specifications is also what makes Podman and containerd
inexpensive to support, so this choice buys optionality rather than spending it.

### 9.8 Why Anthropic, Google, and OpenAI, Listed Alphabetically and Ranked Never

Three providers, not one, because a single-provider category would be a preference wearing the
costume of a technology decision.

Each publishes a documented and versioned interface with a stated deprecation policy, is available
to an individual Developer without enterprise negotiation, and — the criterion that decided it — is
substitutable for the others without changing a Project's Rules, Skills, Prompts, Workflows, or
repository structure. That substitutability is the entire point. If switching between them required
touching a Project's assets, Runtime independence would be aspirational and AEOS would know it.

The permanently empty Preferred row in this category is the load-bearing absence in this document.

### 9.9 Why Gemma, Llama, Mistral, and Qwen

Four families, listed at the same tier as the commercial providers above, because commercial AI and
open-source AI are the same kind of thing to AEOS: external inference, chosen by the user.

Each publishes openly licensed weights, has an active release lineage rather than a single drop, and
is runnable by more than one supported Inference Runtime. Each is also available across a range of
sizes, which is the criterion that decides whether local inference is a real option or a
demonstration: a family that exists only at sizes requiring server-class hardware cannot serve a
student on a laptop, a Developer on a train, or an organization that cannot send its code anywhere.

### 9.10 Why Ollama, LM Studio, vLLM, and Transformers

Four runtimes covering four genuinely different situations, deliberately spanning the tier boundary.

Ollama is the simplest path to a working local Model on any of the three Platforms, with a single
install and offline operation thereafter. LM Studio adds a graphical interface, which matters more
than it sounds: it is often the difference between a learner running a Model and a learner reading
about one. Transformers is the reference implementation, the widest Model support, and the runtime a
researcher will reach for when reproducibility matters more than throughput.

vLLM is Conditionally Supported rather than Officially Supported, and the reason is instructive. It
is the strongest option for serving at throughput, and it targets server-class Linux with
accelerators. Platform neutrality is not a claim AEOS can make selectively, so a technology that
cannot be Preferred on all three Platforms is not Preferred on any — the condition is published
rather than quietly absorbed. MLX sits at the same tier for the same reason on the opposite
Platform.

### 9.11 Why Git and GitHub

Git is Preferred and effectively without an alternative: distributed, complete local history,
content-addressed integrity, fully offline, and universally tooled. The repository is the Product,
and Git is the mechanism by which the repository outlives everything around it.

GitHub is supported as a hosting platform, at a lower level of commitment and for a narrower reason:
AEOS documentation targets its Markdown rendering and its automation surfaces, and that targeting is
a verification commitment rather than a requirement. Four alternative hosts are Conditionally
Supported, and a repository with no remote at all is fully supported. Nothing AEOS does may depend
on where a repository is hosted, or on whether it is hosted.

### 9.12 Why pytest, Vitest, Playwright, and Jest

This is the most consequential set in the document. The TDD Cycle is the default development
workflow, AEOS gates workflow progression on test results, and these are the tools through which
that gate operates.

pytest is the Python standard by a wide margin: minimal ceremony to write a first test, powerful
fixtures when a Project needs them, and machine-readable output AEOS can act on. Vitest is preferred
for TypeScript work not yet started — fast, aligned with the supported build tooling, and largely
interface-compatible with Jest, which makes migration inexpensive. Playwright covers end-to-end
testing across browsers on all three Platforms, which no alternative matches.

Jest is Officially Supported but not Preferred, and the distinction is deliberate rather than
diplomatic. A large number of existing Projects use it, and adoption is the harder and more common
case AEOS must handle safely. A Project on Jest is fully supported and under no obligation to move;
a Project not yet started should choose Vitest. Stating both plainly is more useful than pretending
the answer is uniform.

---

## 10. Technology Lifecycle

Every technology in this document occupies a lifecycle state. The state describes where a technology
stands in its relationship with AEOS; the support tier describes how strong the commitment of AEOS
is. The two are related but not identical, and the mapping is stated in
[Section 10.4](#104-relationship-to-support-tiers) so that neither vocabulary can be mistaken for
the other.

### 10.1 Lifecycle States

The five states below are the complete set.

| State | Meaning | Obligations while in this state |
| :--- | :--- | :--- |
| **Candidate** | Proposed and under evaluation. Not yet a commitment. | Evaluation against the criteria in [Section 11](#11-technology-evaluation-criteria) MUST be recorded, including the outcome where the candidate is declined. |
| **Supported** | Documented, verified within its scope, and maintained. | Documentation MUST exist. Regressions are defects AEOS owns within the limits of the assigned tier. |
| **Preferred** | The default choice within its category for AEOS work not yet started. | Everything Supported requires, plus worked examples and first-position documentation. At most a small number per category. |
| **Deprecated** | Still supported, scheduled for removal, and no longer chosen for work not yet started. | A stated reason, a stated replacement or the explicit absence of one, a migration path, and a minimum notice period before removal. |
| **Removed** | No longer supported. Retained in the revision history rather than deleted. | The entry and its removal rationale MUST be preserved so that a future reader can distinguish a considered removal from an oversight. |

### 10.2 Movement Between States

The permitted transitions are stated in the table below, and the same information is drawn in the
diagram that follows it.

| From | To | Condition |
| :--- | :--- | :--- |
| Candidate | Supported | Evaluation passed and the owner approved. |
| Candidate | *withdrawn* | Evaluation failed or the owner declined. The rationale is preserved in place. |
| Supported | Preferred | Proven in sustained use and the owner approved. |
| Preferred | Supported | Demoted because a better option was adopted, or because the criteria are no longer met. |
| Supported | Deprecated | Demoted because a better option was adopted, because the criteria are no longer met, or because a stated condition changed. |
| Deprecated | Supported | The reason for deprecation no longer holds and the owner approved reinstatement. |
| Deprecated | Removed | The notice period elapsed. |
| Removed | Candidate | Re-entry, evaluated afresh, inheriting no former tier. |

```text
                 +-------------+
                 |  CANDIDATE  |
                 +------+------+
                        |
          evaluation passed, owner approved
                        |
                        v
                 +-------------+   demoted, condition changed   +-------------+
                 |  SUPPORTED  |<------------------------------>| DEPRECATED  |
                 +------+------+                                +------+------+
                        |  ^                                           |
    proven in use,      |  |  demoted: better option, or               | notice period
    owner approved      |  |  criteria no longer met                   | elapsed
                        v  |                                           v
                 +-------------+                                 +-------------+
                 |  PREFERRED  |                                 |   REMOVED   |
                 +-------------+                                 +-------------+
```

### 10.3 Lifecycle Rules

| ID | Governance statement |
| :--- | :--- |
| `TG-030` | The owner MUST approve every promotion, demotion, deprecation, and removal, and each MUST be recorded in this document's revision history. |
| `TG-031` | A technology MUST spend a stated evaluation period as a Candidate before promotion to Supported. The author MUST record the period with the evaluation. |
| `TG-032` | A technology MUST be Supported before it may become Preferred. Direct promotion from Candidate to Preferred MUST NOT occur. |
| `TG-033` | Removal MUST be preceded by Deprecation for at least one AEOS minor release. Emergency removal for a security or licensing reason is the sole exception, and the author MUST record it as such with its rationale. |
| `TG-034` | Deprecation MUST state a replacement, or MUST state that none exists. Silent deprecation is a defect. |
| `TG-035` | Removed entries MUST NOT be deleted from the document's history. A future reader MUST be able to distinguish a considered removal from an omission. |
| `TG-036` | A technology MAY re-enter as a Candidate after removal. The author MUST evaluate re-entry afresh, and the technology MUST NOT inherit its former tier. |

### 10.4 Relationship to Support Tiers

| Lifecycle state | Compatible support tiers |
| :--- | :--- |
| Candidate | Experimental |
| Supported | Officially Supported · Conditionally Supported |
| Preferred | Officially Supported, carrying the Preferred marker |
| Deprecated | Conditionally Supported, on the condition that it is scheduled for removal |
| Removed | Not Recommended, or absent from the document entirely |

---

## 11. Technology Evaluation Criteria

Evaluation has two parts. Gating criteria are pass or fail. Weighted criteria distinguish between
candidates that have already passed the gates.

### 11.1 Gating Criteria

Every criterion below MUST hold before a technology may be assigned any tier above Experimental.
Failing one is disqualifying regardless of how strongly the others are satisfied. The seven gates
below are the complete set.

| ID | Gate | Why it is absolute |
| :--- | :--- | :--- |
| `TG-040` | **Replaceable.** No AEOS capability may depend on the technology exclusively; an alternative or a stated degradation path MUST exist. | Independence is structural or it is marketing. A capability with one possible provider is a dependency acquired by accident. |
| `TG-041` | **Cross-platform.** The technology MUST be available and functionally equivalent on Windows, macOS, and Linux, or MUST be restricted to Conditionally Supported with the Platform limit stated. | Platform equality is a release gate in AEOS, not a preference. |
| `TG-042` | **Text-based artifacts.** Its configuration and outputs MUST be human-readable, diffable, and consumable by an AI Runtime without a separate machine-only format. | Two audiences from one artifact. A format only one of them can read has failed half its readers. |
| `TG-043` | **Locally operable.** The technology MUST be usable without transmitting Project content to a third party, except where the technology is itself an AI Provider or inference service, which is governed by the approval gates and disclosure obligations of AEOS-PRD. | What leaves the machine is disclosed and approved beforehand, without exception. |
| `TG-044` | **Actively maintained.** The technology MUST have a public release history and evidence of ongoing maintenance. | An unmaintained dependency is a future incident with a known cause. |
| `TG-045` | **Usable license.** The license MUST permit use by individuals, organizations, and educators without per-seat negotiation for the technology itself. Paid access to an external service is a separate matter and does not fail this gate. | Education Friendly and Community Support both fail if a technology cannot be picked up by someone without a purchasing department. |
| `TG-046` | **No secret exposure.** The technology MUST NOT require credentials or secrets to be written into Prompts, logs, reports, documentation, or Repository Assets. | This is absolute across the entire Product, without exception or negotiation. |

### 11.2 Weighted Criteria

The ten criteria below are applied after the gates, to choose between candidates and to assign a
tier. They are the complete set.

| Criterion | What is assessed | Evidence that satisfies it |
| :--- | :--- | :--- |
| **Community** | Plural maintainership and independent participation. | Multiple active maintainers, a public contribution process a stranger has used, and independent forums and tooling. |
| **Documentation** | Whether a competent stranger can succeed without the supplier's help. | Complete reference documentation, worked examples, and substantial material not written by the maintainers. |
| **Longevity** | Evidence it will still be present in five years. | Years of stable releases, a stated compatibility policy, and a record of handling its own breaking changes responsibly. |
| **Security** | How the technology handles its own vulnerabilities. | A published disclosure process, timely advisories, and a maintained supported-version policy. |
| **License** | Terms beyond the gating minimum. | A license approved by the Open Source Initiative, a stable licensing history, and no record of relicensing against existing users. |
| **Maintainability** | What it costs to keep, not to start. | Predictable behavior, comprehensible failure modes, and a small operational surface. |
| **Cross-platform support** | Quality of parity beyond mere availability. | Identical behavior, path handling, and results across all three Platforms, not merely successful installation. |
| **Offline capability** | Behavior without a network. | Installable once and reusable; local operations require no network round trip. |
| **AI compatibility** | How well an AI Runtime can work with it. | Text-based configuration, structured output, stable interfaces, and documentation an AI Runtime can consume directly. |
| **Educational value** | Whether a learner can use it and learn from it. | Free to learn, runs on ordinary hardware, observable enough that a learner can see what happened rather than only that it worked. |

### 11.3 Rules of Evaluation

| ID | Governance statement |
| :--- | :--- |
| `TG-050` | The author MUST record every evaluation outcome against both the gating and the weighted criteria, including for declined candidates. |
| `TG-051` | The author MUST NOT score weighted criteria numerically. A written judgment that a future reader can disagree with is required; a total that hides the reasoning is not. |
| `TG-052` | Where a candidate would displace an incumbent, the evaluation MUST state the migration cost imposed on existing Projects, and MUST weigh it as a cost of adoption. |

---

## 12. Versioning Policy

### 12.1 Versions of Supported Technologies

| ID | Governance statement |
| :--- | :--- |
| `TG-060` | This document MUST state minimum supported versions and supported version ranges, and MUST NOT state exact pins. Pinning is an implementation concern realized in lockfiles and manifests. |
| `TG-061` | Where a technology offers a long-term-support or stable channel, that channel is Preferred. Where it does not, the most recent release with a stable-release history is Preferred. |
| `TG-062` | AEOS SHOULD support every version of a technology still receiving supplier security support, and MUST NOT claim support for a version that has reached supplier end-of-life. |
| `TG-063` | A version that reaches supplier end-of-life becomes Deprecated at the next AEOS minor release without requiring a separate proposal. |
| `TG-064` | A new major version of a supported technology enters as a Candidate, and the author MUST evaluate it before the supported range is extended. A major version MUST NOT be inherited automatically. |
| `TG-065` | Where the major versions of a technology are not compatible with one another, this document MUST state which majors are supported, and MUST NOT imply that any major is interchangeable with another. |

### 12.2 Breaking Changes and Compatibility

A change to this document is breaking when it invalidates a technology choice a Project has already
made in reliance on it.

| Change | Breaking | Required handling | Document version impact |
| :--- | :--- | :--- | :--- |
| Adding a technology at any tier | No | Owner approval and recorded rationale. | Minor |
| Promoting a technology to a higher tier | No | Owner approval and recorded evaluation. | Minor |
| Extending a supported version range | No | Owner approval and recorded evaluation. | Minor |
| Demoting a technology to a lower tier | Yes | Stated reason, and Deprecation before any removal. | Major |
| Narrowing a supported version range | Yes | Deprecation notice and a migration path. | Major |
| Removing a technology | Yes | One AEOS minor release of Deprecation, a migration path, and preserved rationale. | Major |
| Editorial correction with no change of meaning | No | Contributor change, owner acceptance. | Patch |

| ID | Governance statement |
| :--- | :--- |
| `TG-070` | Existing Projects are commitments already made. A breaking change MUST state the benefit that exceeds the cost imposed on people who are not present to object, and MUST provide a migration path. |
| `TG-071` | A security or licensing emergency MAY compress the deprecation period. The author MUST record the compression and its reason; the requirement to record is never compressed. |
| `TG-072` | This document's own version follows the change control in [Section 15.2](#152-change-control), which is consistent with the change control defined by AEOS-PRD and AEOS-DOCSTD. |

---

## 13. Source of Truth Rules

| ID | Governance statement |
| :--- | :--- |
| `TG-080` | **Technology decisions originate here.** A technology becomes supported by being added to this document, and by no other means. |
| `TG-081` | **Architecture references this document.** Architecture documents MUST cite categories and entries, and MUST NOT restate, extend, or narrow them. |
| `TG-082` | **Templates reference this document.** Template assets MUST NOT introduce a technology absent from this document. |
| `TG-083` | **Implementation follows this document.** Manifests, lockfiles, and pins realize the set stated here, and MUST NOT extend it. |
| `TG-084` | **Specifications reference this document.** A specification MUST NOT name a technology as a requirement unless it is Officially Supported or Preferred here. |
| `TG-085` | **A conflict is a defect in the downstream document.** Where a downstream document names a technology this document does not, the downstream document is corrected or the technology is proposed as a Candidate. A Contributor MUST NOT resolve it by reinterpreting this document. |
| `TG-086` | **Absence is not prohibition.** A technology absent from this document is unsupported. AEOS MUST NOT refuse to operate on a Project because of it. |
| `TG-087` | **Product documents outrank this one.** Where a technology decision would conflict with AEOS-PRD or an AEOS-VISION invariant, the technology decision MUST be withdrawn. |

---

## 14. Future Evolution

Technology may evolve. The product philosophy does not.

Every entry in this document is expected to change. Providers will consolidate and disappear. Model
families will be superseded by their own successors. Package managers will be replaced by faster
ones with better lockfiles. Editors will merge. Standards will be extended and eventually
superseded. None of that is an engineering event for a Project governed by AEOS, and the measure of
this document's success is how little disturbance each of those changes causes.

What does not change is the reasoning by which entries are chosen: no Vendor privileged, no Platform
second-class, no Runtime required, open standards preferred, offline work protected, commercial and
open-source AI treated as the same kind of thing, and the whole set kept as small as honesty allows.

> A proposal that adds, promotes, demotes, or removes a technology is ordinary governance and is
> welcome. A proposal that requires an AEOS-VISION invariant to bend in order to accommodate a
> technology is not a technology proposal, and is refused at this document's boundary rather than
> escalated for a decision it is not entitled to request.

The direction of travel for the set as a whole is toward fewer systems doing more, not more systems
doing less: vector search absorbed into supported databases rather than added beside them, build
tooling native to the language rather than layered above it, observability instrumented through open
standards rather than through a Vendor's agent. Each of those reduces what a maintainer must
understand five years from now, which is the only measure this document ultimately answers to.

---

## 15. Document Governance

### 15.1 Status

This document is the Technology Source of Truth for the AEOS repository. Every architecture,
blueprint, specification, and Template document references it instead of defining supported
technologies of its own.

### 15.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, owner acceptance | Patch |
| Adding a technology, or promoting one to a higher tier | Owner approval with a recorded evaluation | Minor |
| Extending a supported version range | Owner approval with a recorded evaluation | Minor |
| Adding a technology category | Explicit owner revision request | Minor |
| Demoting, deprecating, or removing a technology | Explicit owner approval with recorded rationale and a migration path | Major |
| Changing a technology principle, support tier definition, or lifecycle state | Explicit owner revision request | Major |

### 15.3 Identifier Policy

The `TC-` category identifiers and the `TG-` governance statement identifiers are permanent. They
MUST NOT be reused, renumbered, or repurposed. A retired statement is marked retired in place,
retaining its identifier and its rationale.

### 15.4 Relationship to the Architecture Freeze

This document introduces no architecture and grants no capability. Technology proposals arising from
architecture work are recorded as Candidates under [Section 10](#10-technology-lifecycle) and
applied only after explicit owner approval.

### 15.5 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical
or Major findings remain.

### 15.6 Traceability

| Layer | Obligation |
| :--- | :--- |
| Architecture documents | Every technology named traces to a `TC-` category and an entry in [Section 8](#8-officially-supported-technologies-by-category). |
| Specifications | Every required technology is Officially Supported or Preferred here. |
| Templates | Every technology used traces to an entry here. |
| Implementation | Manifests and lockfiles realize entries here and introduce none. |
| Issues and change requests | Reference the `TC-` or `TG-` identifiers they affect. |

### 15.7 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Superseded | Initial technology governance definition. Established three technology scopes, thirteen technology principles, five support tiers, twenty technology categories, the initial official technology set with rationale, five lifecycle states, gating and weighted evaluation criteria, the versioning policy, and the source of truth rules. Introduced no architecture, no requirement, and no capability. |
| 1.0.1 | Freeze candidate | Editorial correction with no change of meaning. Converted the document from HTML-in-Markdown to GitHub-Flavored Markdown as AEOS-DOCSTD requires under rules F12 to F16, replacing all HTML tables with Markdown tables and all collapsible blocks with numbered subsections under rule F13 and Section 8.3. Adopted the required conformance notice verbatim under rule 7.5 and the metadata and closing blocks of the template under Appendix A. Renamed Section 2 to Scope and Applicability and Section 4 to Technology Scopes, added Section 2.4 recording the deviation from Section 7.3 permitted by Section 7.2, added transition and step tables accompanying both diagrams under Section 8.4, applied Reserved Term capitalization under convention A3, and replaced time-relative wording under convention S4. No technology entry, tier, scope, principle, lifecycle state, evaluation criterion, governance statement, identifier, ownership, or responsibility was changed. |

---

## Appendix A — Technology Matrix

This appendix is non-normative. It summarizes
[Section 8](#8-officially-supported-technologies-by-category), which is authoritative; where this
table and Section 8 differ, Section 8 governs. Preferred entries are a subset of Officially
Supported and are not repeated in the Supported column. Order within a cell is alphabetical.

| Category | Preferred | Supported | Conditional | Experimental |
| :--- | :--- | :--- | :--- | :--- |
| `TC-01` Operating Systems | *None by policy* | Linux · macOS · Windows | Rolling-release Linux · WSL2 | BSD family |
| `TC-02` Development Environments | *None by policy* | Cursor · Terminal · Visual Studio Code · Windsurf | Emacs · GitHub Copilot · JetBrains IDEs · Neovim · Zed | Browser-hosted and remote environments |
| `TC-03` Programming Languages | Python · TypeScript | *Preferred set only* | C# · Go · Java · JavaScript · Rust | Other languages with a mainstream test runner |
| `TC-04` Frameworks | *None by policy* | FastAPI · React · React Native | Django · Express · Flask · Next.js · Svelte · Vue | Frameworks under two years of stable release history |
| `TC-05` Databases | PostgreSQL · SQLite | *Preferred set only* | DuckDB · MariaDB · MySQL | Wire-compatible distributed and embedded alternatives |
| `TC-06` ORM and Data Access | *None* | Alembic · SQLAlchemy | Drizzle ORM · Prisma · SQLModel | Query builders and lightweight mappers |
| `TC-07` Package Managers | pnpm · uv | *Preferred set only* | npm · pip with virtual environments · Poetry · Yarn | Emerging resolvers |
| `TC-08` Containers | *None by policy* | Docker · Docker Compose | containerd · Podman | Development-container specifications |
| `TC-09` AI Providers | *None by policy, permanently* | Anthropic · Google · OpenAI | Compatible-interface providers · Managed enterprise endpoints | Commercial providers not yet evaluated |
| `TC-10` Open-source Models | *None by policy* | Gemma · Llama · Mistral · Qwen | Other openly licensed families | Families not yet evaluated · Specialized code Models |
| `TC-11` Inference Runtimes | *None by policy* | LM Studio · Ollama · Transformers | llama.cpp · MLX · vLLM | Compatible-interface serving stacks |
| `TC-12` Vector Databases | pgvector · sqlite-vec | *Preferred set only* | Chroma · Qdrant | Other locally operable vector stores |
| `TC-13` MCP and Interoperability Standards | MCP | OpenAPI-described interfaces | Language Server Protocol · Vendor extensions to a supported standard | Agent and tool specifications not yet evaluated |
| `TC-14` Build Systems | Language-native build tooling | Vite | just · Make | Monorepo orchestration systems |
| `TC-15` Testing Frameworks | Playwright · pytest · Vitest | Jest | Cypress · Testing Library · unittest | Property-based testing · Runners not yet evaluated |
| `TC-16` Documentation | GitHub-Flavored Markdown | Mermaid | Docusaurus · MkDocs · Sphinx | Structured formats with stronger machine semantics |
| `TC-17` CI/CD | *None by policy* | GitHub Actions | Azure Pipelines · CircleCI · GitLab CI · Jenkins | Local pipeline emulation |
| `TC-18` Version Control | Git | GitHub, for hosting | Azure Repos · Bitbucket · Gitea · GitLab | Git-compatible front-ends |
| `TC-19` Monitoring | Structured local logging | OpenTelemetry | Grafana · Prometheus · Sentry | Local-first observability backends |
| `TC-20` Security | Platform-native credential stores | Package manager dependency auditing · gitleaks · Trivy | CycloneDX and SPDX bills of materials · Dependabot · Renovate · Sigstore | Commercial static analysis and supply-chain platforms |

---

## Appendix B — Technology Selection Decision Flow

This appendix is non-normative. It describes the process by which a technology is proposed,
evaluated, and admitted, and introduces no obligation absent from the document body. Each step
produces a recorded outcome, including where the answer is no.

### B.1 The Steps

| Step | Action | Outcome where the step is not passed |
| :--- | :--- | :--- |
| 1 | A need is identified: a capability or Project requires a technology decision. | Not applicable. |
| 2 | The author determines whether a supported technology already suffices. | Where one does, it is used, no proposal is raised, and the consideration is recorded. |
| 3 | The author applies the gating criteria `TG-040` to `TG-046`. | Where any gate fails, the technology is Not Recommended and the failing gate is recorded. |
| 4 | The author applies the weighted criteria of [Section 11.2](#112-weighted-criteria), counting migration cost as a cost of adoption. | Not applicable; the evaluation informs the proposal. |
| 5 | The author proposes the technology to the owner, explaining and awaiting a decision. | Where the owner declines, the technology is recorded as considered and declined, with the reason. |
| 6 | On approval, the technology becomes a Candidate at the Experimental tier for a stated evaluation period. | Not applicable. |
| 7 | At the end of the evaluation period, the author reviews whether it held up in sustained use. | Where it did not, the Candidate is withdrawn and the rationale is preserved in place. |
| 8 | The technology becomes Supported, either Officially or Conditionally, and is documented and verified. | Not applicable. |
| 9 | Where it is proven in sustained use and the owner approves, the technology becomes Preferred under `TG-032`. | Where it is not, the technology remains Supported, which is the intended outcome for most entries. |

### B.2 The Flow

```text
        +--------------------------------------+
        | 1. NEED IDENTIFIED                   |
        |    A capability or Project requires  |
        |    a technology decision.            |
        +-------------------+------------------+
                            |
                            v
        +--------------------------------------+   yes    +---------------------------+
        | 2. DOES A SUPPORTED TECHNOLOGY       +--------->| Use it. No proposal.      |
        |    ALREADY SUFFICE?                  |          | Record the consideration. |
        +-------------------+------------------+          +---------------------------+
                            | no
                            v
        +--------------------------------------+  fails   +---------------------------+
        | 3. GATING CRITERIA  TG-040..TG-046   +--------->| Tier: Not Recommended.    |
        |    Pass or fail. No exceptions.      |          | Record which gate failed. |
        +-------------------+------------------+          +---------------------------+
                            | passes
                            v
        +--------------------------------------+
        | 4. WEIGHTED EVALUATION  Sec. 11.2    |
        |    Written judgment, not a score.    |
        |    Migration cost counted as cost.   |
        +-------------------+------------------+
                            |
                            v
        +--------------------------------------+ declined +---------------------------+
        | 5. PROPOSAL TO THE OWNER             +--------->| Recorded as considered    |
        |    Explain, propose, await decision. |          | and declined, with reason.|
        +-------------------+------------------+          +---------------------------+
                            | approved
                            v
        +--------------------------------------+
        | 6. CANDIDATE                         |
        |    Tier: Experimental.               |
        |    Stated evaluation period.         |
        +-------------------+------------------+
                            |
                            v
        +--------------------------------------+ not proven  +-------------------------+
        | 7. EVALUATION PERIOD REVIEW          +------------>| Withdrawn. Rationale    |
        |    Held up in sustained use.         |             | preserved in place.     |
        +-------------------+------------------+             +-------------------------+
                            | proven
                            v
        +--------------------------------------+
        | 8. SUPPORTED                         |
        |    Officially or Conditionally.      |
        |    Documented and verified.          |
        +-------------------+------------------+
                            |
                 proven in sustained use,
                 owner approval  TG-032
                            |
                            v
        +--------------------------------------+
        | 9. PREFERRED                         |
        |    Default for AEOS work not yet     |
        |    started.                          |
        +--------------------------------------+
```

### B.3 Notes on the Flow

| Step | Note |
| :--- | :--- |
| 2 | The most valuable step and the most often skipped. Most technology proposals are answered by a technology already in the set, and answering them that way is a success rather than a rejection. |
| 3 | Gates are not weighed against one another. A candidate that fails one gate and excels at everything else still fails. |
| 4 | Migration cost imposed on existing Projects is counted as a cost of adoption, not as a cost borne by whoever raises it later. |
| 5 | The owner decides. A Contributor, human or AI Runtime, explains, proposes, and waits, exactly as AEOS itself does before any consequential action. |
| 7 | Withdrawal at this step is a normal outcome and is never treated as a failure of the proposal or the proposer. |
| 9 | Promotion to Preferred is never automatic and never a formality. Most Supported technologies will remain Supported permanently, which is the intended outcome. |

---

## Appendix C — Governance Statement Index

This appendix is non-normative. It indexes the governance statements and categories stated in the
document body.

| Range | Subject | Count | Section |
| :--- | :--- | :--- | :--- |
| `TG-001` to `TG-005` | Technology scopes | 5 | [4](#4-technology-scopes) |
| `TG-010` to `TG-012` | Rules of application for the principles | 3 | [5.14](#514-rules-of-application) |
| `TG-020` to `TG-024` | Support tiers | 5 | [6](#6-support-tiers) |
| `TG-030` to `TG-036` | Technology lifecycle | 7 | [10.3](#103-lifecycle-rules) |
| `TG-040` to `TG-046` | Gating criteria | 7 | [11.1](#111-gating-criteria) |
| `TG-050` to `TG-052` | Rules of evaluation | 3 | [11.3](#113-rules-of-evaluation) |
| `TG-060` to `TG-065` | Versions of supported technologies | 6 | [12.1](#121-versions-of-supported-technologies) |
| `TG-070` to `TG-072` | Breaking changes and compatibility | 3 | [12.2](#122-breaking-changes-and-compatibility) |
| `TG-080` to `TG-087` | Source of truth rules | 8 | [13](#13-source-of-truth-rules) |
| `TC-01` to `TC-20` | Technology categories | 20 | [7](#7-technology-categories), [8](#8-officially-supported-technologies-by-category) |

---

**End of Supported Technologies**

AEOS-TECH · Version 1.0.1 · Technology Source of Truth
