# AI Engineering Operating System

## AEOS — Code of Conduct

*Community standards for participation in the AEOS repository and its associated community spaces.*

| Field | Value |
| :--- | :--- |
| **Document** | Code of Conduct |
| **Product** | AI Engineering Operating System (AEOS) |
| **Document ID** | AEOS-COC |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Owner** | Product Owner, AEOS |
| **Author** | Documentation Governance Board, AEOS |
| **Audience** | Contributors, community participants, reviewers, and the AI systems that assist them |
| **Suggested path** | `docs/CODE_OF_CONDUCT.md` |
| **Companion documents** | `AEOS_VISION.md` (AEOS-VISION) · `AEOS_GLOSSARY.md` (AEOS-GLOSSARY) · `AEOS_DOCUMENT_STANDARD.md` (AEOS-DOCSTD) |
| **Supersedes** | None |

> **Authority of this document.**
> This document governs the standard of conduct expected of everyone participating in the AEOS
> repository and its community spaces, and the process by which a violation of that standard is
> reported and addressed.
> It defines no product requirement, no architecture, no runtime behavior, and no term; it does not
> restate AEOS-VISION's guiding principles for contributors, which remain the governing statement for
> how a technical contribution is proposed and reviewed. Where this document and a document of higher
> authority both speak to a subject, the higher-authority document governs and any conflict here is a
> defect to be reported.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Community Standards](#3-community-standards)
4. [Expected Behavior](#4-expected-behavior)
5. [Unacceptable Behavior](#5-unacceptable-behavior)
6. [Reporting](#6-reporting)
7. [Enforcement](#7-enforcement)
8. [Document Governance](#8-document-governance)
9. [Appendix A — Attribution](#appendix-a--attribution)

---

## 1. Executive Summary

AEOS is an open-source, collaborative engineering project, and its repository is read and changed by
people of differing backgrounds, experience levels, and expectations. AEOS-VISION's guiding
principles for contributors already state how a technical contribution is made — clearly,
incrementally, under test-first discipline, and with the architecture freeze respected. Those
principles say nothing about how participants treat one another while doing that work, and no AEOS
document before this one has stated a standard for that.

This document states that standard: the conduct expected of anyone taking part in the AEOS repository
or its community spaces, the conduct that is not tolerated, how a violation is reported, and how it is
addressed. It adapts the Contributor Covenant, version 2.1 — the code of conduct most widely adopted
across open-source projects for this purpose — into AEOS's own documentation form and terminology; see
[Appendix A](#appendix-a--attribution).

## 2. Scope and Applicability

### 2.1 Applicability

In plain terms: this document covers anyone taking part in AEOS, in the AEOS repository or while
officially representing it elsewhere, and it covers a human's responsibility for what an AI runtime
produces on their behalf — not the AI runtime itself.

| ID | Rule | Traces to |
| :--- | :--- | :--- |
| `COC-001` | This document applies to every Contributor, as AEOS-GLOSSARY defines the term, and to every other individual who opens an issue, comments, submits a pull request, participates in a review, or otherwise takes part in an AEOS community space — regardless of whether that participation meets AEOS-GLOSSARY's narrower test for a Contributor. | AEOS-VISION §9 |
| `COC-002` | An AEOS community space is the AEOS repository itself, including its issues, pull requests, discussions, and code review comments, together with any other space the project owner designates in writing as an AEOS community space. | — |
| `COC-003` | This document also applies to an individual's conduct outside an AEOS community space where that individual is officially representing AEOS — for example, using an official AEOS email address, posting from an official AEOS social media account, or acting as an appointed representative at an event. | — |
| `COC-004` | Where a change or communication a Contributor submits is produced by an AI runtime, the human who submitted it remains responsible for that content's compliance with this document. | AEOS-VISION `V2` · `G6` |
| `COC-005` | An AI runtime is not itself a subject of this document's enforcement provisions. Only a human participant — including the human operating an AI runtime — is subject to [Section 7](#7-enforcement); an AI runtime's output is addressed through `COC-004`, by way of the human responsible for it. | AEOS-VISION `V2` |

### 2.2 What This Document Does Not Cover

| Not governed here | Owned by |
| :--- | :--- |
| The mechanics of proposing, reviewing, or approving a change to AEOS itself | AEOS-VISION §9 (Guiding Principles for Contributors), and any Developer Guide the project owner authors for that purpose |
| Product requirements, architecture, runtime behavior, or specified behavior | AEOS-PRD, and the Architecture, Blueprint, and Specification documents |
| The meaning of an AEOS term | AEOS-GLOSSARY |
| Reporting or handling a security vulnerability | Any Developer Guide the project owner authors for that purpose; none exists in this repository yet |

A statement in this document that grants a capability, imposes a product requirement, defines a term,
or reaches into a subject the table above assigns elsewhere is a defect in this document. It MUST be
reported rather than acted upon.

### 2.3 Recorded Deviation: Normative Language

AEOS-DOCSTD Section 7.3 states that the Developer Guide layer SHOULD NOT use normative keywords,
because a guide instructs rather than obliges. This document uses normative keywords in this Scope
section and in [Section 5](#5-unacceptable-behavior), [Section 6](#6-reporting), and
[Section 7](#7-enforcement), because a standard of conduct and its enforcement process are meaningless
unless the obligations they state are independently checkable — the same reason AEOS-DOCSTD Section
7.3 gives for requiring normative language in the Specification layer. [Section 3](#3-community-standards)
and [Section 4](#4-expected-behavior) remain descriptive, consistent with the Developer Guide layer's
ordinary convention, because the conduct they describe is not independently checkable in the sense
AEOS-DOCSTD Section 7.4 requires of a normative statement. The deviation is deliberate and is recorded
here as AEOS-DOCSTD Section 7.2 requires of a deliberate deviation from a SHOULD, following the
precedent AEOS-GLOSSARY Section 2.4 established for the same deviation.

## 3. Community Standards

AEOS is built as a public, collaborative engineering artifact, open to contributors of every
background and level of experience. Participation in the AEOS repository and its community spaces is
expected to remain harassment-free and welcoming regardless of a participant's age, body size, visible
or invisible disability, ethnicity, sex characteristics, gender identity and expression, level of
experience, education, socio-economic status, nationality, personal appearance, race, religion, or
sexual identity and orientation.

A healthy community is sustained by shared standards, not by an absence of disagreement. Disagreement
about an engineering decision is expected, and is addressed through the review and escalation paths
AEOS-VISION's guiding principles for contributors already establish. This document addresses
interpersonal conduct, not engineering judgment, and does not weigh in on which side of a technical
disagreement is correct.

## 4. Expected Behavior

The behaviors below describe the standard AEOS asks of every participant. They are stated
descriptively rather than as checkable obligations, consistent with [Section 2.3](#23-recorded-deviation-normative-language).

| Behavior | In practice |
| :--- | :--- |
| Empathy and kindness | Approaching disagreement with curiosity about the other participant's perspective before assuming poor intent. |
| Respect for differing viewpoints | Treating a difference of opinion, technology preference, or experience level as ordinary, not as a defect in the other participant. |
| Graceful feedback | Offering feedback aimed at the work rather than the person, and receiving feedback as information rather than as an attack. |
| Accountability | Acknowledging a mistake, apologizing to those affected, and learning from the experience without demanding it be relitigated. |
| Community focus | Making decisions and comments in view of what serves the AEOS community as a whole, not only the individual participant's immediate preference. |

## 5. Unacceptable Behavior

In plain terms: harassment, sexualized conduct, personal attacks, and doxxing are not tolerated in any
AEOS community space, alongside anything else that would be plainly out of place in a professional
setting.

| ID | Rule |
| :--- | :--- |
| `COC-006` | A participant MUST NOT use sexualized language or imagery, or make a sexual advance or give unwelcome sexual attention of any kind, in an AEOS community space. |
| `COC-007` | A participant MUST NOT engage in trolling, an insulting or derogatory comment, or a personal or political attack directed at another participant. |
| `COC-008` | A participant MUST NOT engage in public or private harassment of another participant. |
| `COC-009` | A participant MUST NOT publish another participant's private information — including a physical address, an email address, or other identifying information — without that person's explicit permission. |
| `COC-010` | A participant MUST NOT engage in conduct that could reasonably be considered inappropriate in a professional setting, beyond the conduct `COC-006` through `COC-009` already name. |

## 6. Reporting

In plain terms: if you experience or witness a violation, report it; what to expect below is that a
report is reviewed, kept as confidential as the circumstances allow, and not held against you for
asking for accommodation in how you make it.

| ID | Rule |
| :--- | :--- |
| `COC-011` | A participant who experiences or witnesses conduct that may violate this document SHOULD report it as soon as reasonably possible, through the channel this section states. |
| `COC-012` | The specific contact channel through which a report is submitted is reserved to the project owner's designation and is not yet established in this repository's governing documents. Until a channel is designated, a report MUST be directed to the project owner through a channel the project owner has made available outside the public AEOS repository, and MUST NOT be filed as a public issue or comment where the report itself would disclose identifying or sensitive information about anyone involved. Where the report concerns the conduct of the project owner or of the person the project owner has designated to receive reports, `COC-018` states the recusal rule that applies in place of this rule. |
| `COC-013` | A report SHOULD include a description of the conduct, when and where it occurred, and any other participant involved, to the extent the reporter can safely provide it. |
| `COC-014` | A report, and the identity of the reporter, MUST be handled with as much confidentiality as the circumstances allow. Confidentiality cannot be guaranteed in every circumstance — for example, where the nature of the incident, the number of people involved, or a legal obligation requires some disclosure — and it MUST be disclosed further only to the extent needed to investigate or address the report. |
| `COC-015` | A reporter MAY request a reasonable accommodation in how a report is made, discussed, or followed up on — for example, written rather than live communication — and the person handling the report SHOULD accommodate the request where practicable. |

This document records `COC-012`'s open item as a finished statement about an undecided detail, per
AEOS-DOCSTD `DS-P-10`: what is undecided is the specific reporting channel; who decides it is the
project owner; what this document does in the meantime is stated in `COC-012` itself.

## 7. Enforcement

### 7.1 Responsibility

In plain terms: the project owner enforces this document and can delegate that role, steps aside from
any report naming them personally, and explains moderation decisions when the reason isn't obvious.

| ID | Rule |
| :--- | :--- |
| `COC-016` | The project owner is responsible for clarifying and enforcing this document's standards, and MAY designate another person in writing to assist with enforcement. |
| `COC-017` | The project owner, or a person designated under `COC-016`, MUST review a report received under [Section 6](#6-reporting) and respond in a manner appropriate to the circumstances. |
| `COC-018` | The project owner, or a person designated under `COC-016`, MUST recuse from reviewing, responding to, or enforcing a specific report where that person is the subject of the report or a named party to the conduct it describes. |
| `COC-019` | Where `COC-018` requires recusal and no other person is designated to act in the recused person's place, the report is held pending a person able to act impartially, rather than being reviewed by the recused person. Who is designated for that role, and when, is the project owner's decision — recorded here using the same `DS-P-10` pattern `COC-012` already uses for an open detail, rather than left unaddressed. |
| `COC-020` | The project owner, or a person designated under `COC-016`, MAY reject a pull request or code contribution that does not align with this document, and MAY remove or edit an issue, comment, wiki edit, or other repository content that does not align with this document, and MUST state the reason for doing so when it is not self-evident from the action itself. |

### 7.2 Consequences

| ID | Rule |
| :--- | :--- |
| `COC-021` | Enforcement action is proportionate to the community impact the conduct caused, following the tiers below, and is not limited to a single tier where the conduct or its impact warrants otherwise. |

| Tier | Community impact | Consequence |
| :--- | :--- | :--- |
| 1. Correction | A single instance of unclear, unprofessional, or unwelcome conduct. | A private, written explanation of why the conduct was inappropriate, and, where appropriate, a request for a public or private apology. |
| 2. Warning | A single significant violation, or a pattern of conduct across multiple instances. | A written warning stating the consequence of continued conduct, together with a stated period during which the participant is expected to have no unrequested interaction with those involved, in the AEOS community spaces `COC-002` names and the public-representation contexts `COC-003` names. Violating these terms MAY lead to a temporary or permanent ban. |
| 3. Temporary ban | A serious violation, including a sustained pattern of inappropriate conduct. | A temporary ban from any interaction or public communication with the AEOS community, for a period the project owner states. No public or private interaction with those involved is permitted during that period. Violating these terms MAY lead to a permanent ban. |
| 4. Permanent ban | A pattern of violation, sustained inappropriate conduct, harassment of an individual, or aggression toward or disparagement of a class of individuals. | A permanent ban from any public interaction within the AEOS community. |

## 8. Document Governance

### 8.1 Status

This document is a **Draft**. It is not authoritative until the project owner reviews and approves it,
per AEOS-DOCSTD Section 12. Freeze is additionally blocked while `COC-012` and `COC-019` each record an
undesignated contact, per this document's own Critical-finding threshold in
[Section 8.3](#83-review-policy).

### 8.2 Change Control

| Change type | Requires | Version impact |
| :--- | :--- | :--- |
| Editorial correction with no change of meaning | Contributor change, project owner acceptance | Patch |
| Clarification of an existing standard or process | Project owner approval | Minor |
| Designation or change of the reporting channel (`COC-012`) or the recusal fallback (`COC-019`) | Project owner decision, recorded in a revision history entry | Minor |
| Addition of a standard, process, or enforcement tier | Explicit project owner revision request | Major |
| Removal of a standard or enforcement tier | Explicit project owner decision, recorded, with the reasoning preserved in place | Major |

### 8.3 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**, per
AEOS-DOCSTD Section 12.3, and recommend freezing the document only when no Critical or Major finding
remains open. `COC-012`'s undesignated reporting channel and `COC-019`'s undesignated recusal fallback
are each a Critical finding under that classification until the project owner resolves them, because an
unreachable reporting or enforcement path would cause a reporter to act on incorrect information if this
document were treated as complete.

### 8.4 Precedence

| Situation | Resolution |
| :--- | :--- |
| This document conflicts with AEOS-VISION on a guiding principle for contributors | AEOS-VISION governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-GLOSSARY on the meaning of a term | AEOS-GLOSSARY governs. The conflict is a defect in this document and is reported. |
| This document conflicts with AEOS-DOCSTD on documentation form | AEOS-DOCSTD governs. The conflict is a defect in this document and is reported. |
| A reported violation also constitutes a legal matter | This document does not limit or substitute for legal process. Whether and how to involve one is a project-owner decision outside this document's scope. |

### 8.5 Revision History

| Version | Status | Summary |
| :--- | :--- | :--- |
| 1.0.0 | Draft | Establishes community standards, expected and unacceptable behavior, a reporting path, and a four-tier enforcement ladder, adapted from the Contributor Covenant, version 2.1. Records a deliberate deviation from AEOS-DOCSTD Section 7.3's Developer Guide default against normative language, limited to the sections where an obligation must be checkable to be enforceable. Adds `COC-005`, clarifying that an AI runtime is not itself subject to enforcement. Adds `COC-018` and `COC-019`, a recusal rule for a report naming the project owner or a designated enforcer. Separates rejecting a pull request or code contribution (`COC-020`) from removing or editing content that can actually be edited or removed. Aligns Tier 2's external-conduct language with the scope `COC-002` and `COC-003` already state. Adds `COC-014`'s confidentiality caveat and `COC-015`'s accommodation rule. Sets the suggested path to `docs/CODE_OF_CONDUCT.md`, matching REPOSITORY_LAYOUT.md's precedent of a repository-structural file sitting directly under `docs/` and GitHub's documented community-health-file discovery locations (root, `.github/`, or top-level `docs/`) — a placement recommendation only, pending the project owner's confirmation once REPOSITORY_LAYOUT.md and PROJECT_BOOTSTRAP.md are available for direct re-verification. Records `COC-012`'s reporting channel and `COC-019`'s recusal fallback as Critical findings blocking freeze, per Section 8.3. Consolidated into a single Draft revision-history entry rather than one row per review pass, consistent with `AEOS-DOCSTD` §12.1's treatment of Draft as open to any author change; per-version entries resume from Review onward. Introduces no product requirement, no architectural decision, and no redefinition of any AEOS-GLOSSARY term. |

---

## Appendix A — Attribution

**This appendix is non-normative.**

This document adapts the [Contributor Covenant, version 2.1](https://www.contributor-covenant.org/version/2/1/code_of_conduct.html),
reworded throughout for the AEOS repository's documentation form and for the terminology AEOS-GLOSSARY
defines. The Contributor Covenant is authored by Coraline Ada Ehmke and is made available under the
[Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
The four-tier enforcement structure in [Section 7.2](#72-consequences) follows the general shape of the
Contributor Covenant's own enforcement guidelines, which the Contributor Covenant in turn credits to
[Mozilla's code of conduct enforcement ladder](https://github.com/mozilla/diversity).

Answers to common questions about the source document are available in the
[Contributor Covenant FAQ](https://www.contributor-covenant.org/faq), and translations of the source
document are available on the [Contributor Covenant translations page](https://www.contributor-covenant.org/translations).
Neither the FAQ nor the translations have been independently adapted for AEOS; this document, in its
English form, is the sole authoritative Code of Conduct for the AEOS repository unless the project
owner states otherwise.

---

**End of Code of Conduct**

AEOS-COC · Version 1.0.0 · Community Standards Source of Truth
