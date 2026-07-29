<div align="center">

# AI Engineering Operating System

**AEOS — Supported Technologies**

*The permanent technology source of truth for AEOS.*

</div>

<table>
<thead>
<tr><th align="left">Field</th><th align="left">Value</th></tr>
</thead>
<tbody>
<tr><td><strong>Document</strong></td><td>Supported Technologies</td></tr>
<tr><td><strong>Product</strong></td><td>AI Engineering Operating System (AEOS)</td></tr>
<tr><td><strong>Document ID</strong></td><td>AEOS-TECH</td></tr>
<tr><td><strong>Version</strong></td><td>1.0.0</td></tr>
<tr><td><strong>Status</strong></td><td>Freeze candidate — awaiting owner approval</td></tr>
<tr><td><strong>Owner</strong></td><td>Product Owner, AEOS</td></tr>
<tr><td><strong>Author</strong></td><td>Chief Technology Officer and Technology Governance Authority, AEOS</td></tr>
<tr><td><strong>Audience</strong></td><td>Developers, architects, maintainers, contributors, and AI systems consuming this repository</td></tr>
<tr><td><strong>Suggested path</strong></td><td><code>docs/product/SUPPORTED_TECHNOLOGIES.md</code></td></tr>
<tr><td><strong>Companion documents</strong></td><td><code>AEOS_PRODUCT_REQUIREMENTS.md</code> (AEOS-PRD) · <code>AEOS_VISION.md</code> (AEOS-VISION) · <code>AEOS_GLOSSARY.md</code> (AEOS-GLOSSARY) · <code>AEOS_DOCUMENT_STANDARD.md</code> (AEOS-DOCSTD)</td></tr>
<tr><td><strong>Supersedes</strong></td><td>None</td></tr>
</tbody>
</table>

> **Authority of this document.**
> This document defines *which technologies AEOS officially supports* and *how that set is
> governed*. It defines no architecture, no implementation, no project template, no runtime
> behaviour, and no installation procedure. Where a downstream document names a technology, it
> references this document rather than restating or extending it. Where this document and the
> AEOS-PRD both speak to a subject, the PRD governs product behaviour and this document governs
> only the technology choice made within the behaviour the PRD already defines.

> **Requirement terminology.**
> The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**,
> **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted
> as described in RFC 2119 and RFC 8174, and only when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Purpose and Boundary](#2-purpose-and-boundary)
3. [Relationship to Frozen Documents](#3-relationship-to-frozen-documents)
4. [Scope of Technology Support](#4-scope-of-technology-support)
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

<section>

## 1. Executive Summary

A product that intends to remain maintainable for a decade cannot leave its technology choices to
the moment each choice is needed. Technology decided per task is technology decided by whoever
happened to be present, for reasons nobody recorded, in a form nobody can review. The result is not
a stack; it is an accumulation. It is discovered years later, by people who did not choose it, in
the form of a migration nobody budgeted for.

Technology governance is how AEOS avoids that outcome. It states, in one place, which technologies
AEOS depends on, which it is tested and documented to work alongside, which it advises against, and
by what process any of those answers may change. A contributor — human or AI — should never have to
guess, and should never have to negotiate the answer twice.

**AEOS remains technology-neutral while officially supporting a curated technology set.** Those two
statements are compatible, and the distinction between them is the most important thing in this
document:

<table>
<thead>
<tr><th align="left">Neutrality means</th><th align="left">Official support means</th></tr>
</thead>
<tbody>
<tr><td>No vendor, runtime, model, platform, or distribution is privileged or required.</td><td>AEOS documents, verifies, and maintains its work against a stated set of technologies.</td></tr>
<tr><td>A technology's absence never disables AEOS; it reduces the options available to the user.</td><td>A supported technology is one a user can rely on working, and whose breakage is a defect AEOS owns.</td></tr>
<tr><td>Being named here confers no privilege. Being unnamed implies no exclusion.</td><td>Support is a maintenance commitment, not an endorsement and not a recommendation to prefer one vendor over another.</td></tr>
<tr><td>Users MAY use any technology they choose with AEOS.</td><td>Unlisted technologies are permitted and unsupported — never prohibited.</td></tr>
</tbody>
</table>

The curated set exists because a commitment that covers everything covers nothing. Verification,
documentation, and cross-platform testing are finite. AEOS states where it has spent them so that
users know exactly what they are relying on, and so that the boundary of that reliance is a
published fact rather than an assumption.

</section>

---

<section>

## 2. Purpose and Boundary

### What this document defines

<table>
<thead>
<tr><th align="left">Defined here</th><th align="left">Section</th></tr>
</thead>
<tbody>
<tr><td>Officially supported technologies, by category and support tier</td><td><a href="#8-officially-supported-technologies-by-category">8</a>, <a href="#9-the-initial-official-technology-set">9</a></td></tr>
<tr><td>Technology selection principles</td><td><a href="#5-technology-principles">5</a></td></tr>
<tr><td>Technology governance — who decides, and by what process</td><td><a href="#13-source-of-truth-rules">13</a>, <a href="#15-document-governance">15</a></td></tr>
<tr><td>Technology lifecycle — how a technology enters, matures, and leaves the set</td><td><a href="#10-technology-lifecycle">10</a></td></tr>
<tr><td>Technology categories and their selection criteria</td><td><a href="#7-technology-categories">7</a></td></tr>
<tr><td>Evaluation criteria for technologies not yet in the set</td><td><a href="#11-technology-evaluation-criteria">11</a></td></tr>
<tr><td>Versioning policy for supported technologies</td><td><a href="#12-versioning-policy">12</a></td></tr>
</tbody>
</table>

### What this document does not define

<table>
<thead>
<tr><th align="left">Not defined here</th><th align="left">Because</th></tr>
</thead>
<tbody>
<tr><td>Architecture — structure, layers, components, boundaries</td><td>Architecture documents own it and reference this document for technology.</td></tr>
<tr><td>Implementation — code, algorithms, file layout, dependency pins</td><td>The codebase owns it and MUST remain within the set stated here.</td></tr>
<tr><td>Project templates and scaffolds</td><td>Template assets own it and reference this document for technology.</td></tr>
<tr><td>Runtime behaviour — execution, lifecycle, orchestration mechanics</td><td>Runtime documents own it.</td></tr>
<tr><td>Installation and distribution procedures</td><td>Distribution documents own them; support tiers here are independent of installation method.</td></tr>
<tr><td>Product requirements, capabilities, and scope</td><td>AEOS-PRD owns them and is not restated here.</td></tr>
</tbody>
</table>

> **The reading rule.**
> If a statement in this document tells a reader *how to build something*, it is a defect in this
> document. Report it rather than acting on it. This document answers only *what may be built with*.

</section>

---

<section>

## 3. Relationship to Frozen Documents

Four documents are frozen and govern this one. This document introduces nothing that contradicts
them and restates nothing they already define.

<table>
<thead>
<tr><th align="left">Document</th><th align="left">Governs</th><th align="left">This document's obligation</th></tr>
</thead>
<tbody>
<tr>
<td><strong>AEOS-PRD</strong></td>
<td>Product behaviour, capabilities, requirements, scope.</td>
<td>Every technology decision here MUST be compatible with the PRD's requirements and thirteen product principles. Where a technology choice would require weakening a requirement, the technology loses.</td>
</tr>
<tr>
<td><strong>AEOS-VISION</strong></td>
<td>Purpose, philosophy, invariants.</td>
<td>No technology decision may trade away an invariant. A technology that would make AEOS perform inference, privilege a vendor, or require a fork is excluded regardless of its merits.</td>
</tr>
<tr>
<td><strong>AEOS-GLOSSARY</strong></td>
<td>Terminology.</td>
<td>Terms defined there are used here with their defined meaning and are not redefined. Terms introduced here — support tier, lifecycle state, technology scope — are technology-governance vocabulary and are defined in place.</td>
</tr>
<tr>
<td><strong>AEOS-DOCSTD</strong></td>
<td>Document structure, formatting, and quality.</td>
<td>This document's structure, Markdown-with-semantic-HTML style, and review classification follow it.</td>
</tr>
</tbody>
</table>

### Precedence

<table>
<thead>
<tr><th align="left">Situation</th><th align="left">Resolution</th></tr>
</thead>
<tbody>
<tr><td>A technology in this document conflicts with an AEOS-VISION invariant</td><td>The invariant governs. The entry is removed and the conflict is recorded as a defect in this document.</td></tr>
<tr><td>A technology in this document conflicts with an AEOS-PRD requirement</td><td>The PRD governs. The entry is corrected; the requirement is not reinterpreted.</td></tr>
<tr><td>An architecture, specification, or template document names a technology not listed here</td><td>This document governs. The downstream document is corrected, or the technology is proposed for evaluation under <a href="#10-technology-lifecycle">Section 10</a>.</td></tr>
<tr><td>Two entries in this document conflict with one another</td><td>Escalate to the owner. A contributor reports the inconsistency and does not resolve it by preference.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 4. Scope of Technology Support

"Supported" does not mean the same thing for a technology AEOS is built on, a technology AEOS talks
to, and a technology a user's project happens to use. Three scopes are distinguished so that a
single tier label cannot be misread.

<table>
<thead>
<tr><th align="left">Scope</th><th align="left">Meaning</th><th align="left">What support obliges</th></tr>
</thead>
<tbody>
<tr>
<td><strong>P — Product</strong></td>
<td>Technologies AEOS itself is built on, or requires in order to run.</td>
<td>AEOS takes a real dependency. The technology MUST be available on all three supported platforms, and a break in it is a release-blocking defect.</td>
</tr>
<tr>
<td><strong>I — Integration</strong></td>
<td>External systems AEOS orchestrates and never contains — AI runtimes, version control, delivery pipelines, interoperability standards.</td>
<td>AEOS is documented and verified to orchestrate the technology. AEOS MUST remain whole and functional in its absence, and MUST NOT require any single entry.</td>
</tr>
<tr>
<td><strong>G — Governed project</strong></td>
<td>Technologies AEOS is documented and tested to govern inside a user's project.</td>
<td>AEOS provides documentation and verification for governing work in that technology. Support is descriptive, never prescriptive: AEOS MUST NOT require a project to adopt a listed technology, nor refuse to govern a project that uses an unlisted one.</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Governance statement</th></tr>
</thead>
<tbody>
<tr><td><code>TG-001</code></td><td>Every entry in this document MUST carry at least one scope. An entry without a scope is incomplete.</td></tr>
<tr><td><code>TG-002</code></td><td>Scope <strong>P</strong> entries MUST be available and functionally equivalent on Windows, macOS, and Linux. A technology that is not MUST NOT be a Scope <strong>P</strong> dependency.</td></tr>
<tr><td><code>TG-003</code></td><td>No AEOS capability may depend on a Scope <strong>I</strong> entry. Unavailability of an integration reduces the user's options and MUST NOT corrupt project state or disable non-inference capability.</td></tr>
<tr><td><code>TG-004</code></td><td>Scope <strong>G</strong> support is descriptive. AEOS MUST NOT require, prefer, or penalise a user project's technology choice on the basis of this document.</td></tr>
<tr><td><code>TG-005</code></td><td>Listing a commercial vendor and listing an open-source alternative in the same tier constitutes equal support. Order within a tier is alphabetical and confers no ranking.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 5. Technology Principles

These principles decide technology questions the way the PRD's product principles decide behaviour
questions. They are mandatory for technology selection and are subordinate to — never a
reinterpretation of — the product principles and invariants they serve.

<details>
<summary><strong>5.1 Vendor Neutral</strong></summary>

<br>

No vendor is privileged and no vendor is required. Support for a vendor's technology is a
maintenance commitment, not a partnership, a preference, or a default.

**In practice.** Where a category contains commercial vendors, the set MUST contain more than one,
and MUST include at least one option the user can operate without a commercial relationship. A
category that can only be satisfied by a single vendor MUST be recorded as a concentration risk in
that category's *Future Evolution*.

</details>

<details>
<summary><strong>5.2 Platform Neutral</strong></summary>

<br>

Windows, macOS, and Linux are equal citizens. Technology selection is the most common way that
equality is lost, because cross-platform gaps are usually inherited rather than chosen.

**In practice.** A technology available on only one or two platforms MAY be Conditionally Supported
for the platforms where it exists. It MUST NOT be Preferred, and no AEOS capability may depend on
it.

</details>

<details>
<summary><strong>5.3 Runtime Neutral</strong></summary>

<br>

AEOS performs no inference and contains no model. Every AI runtime is an integration, chosen by the
user and replaceable by the user.

**In practice.** Switching between listed AI providers, models, or inference runtimes MUST NOT
require changes to a project's rules, skills, prompts, workflows, or repository structure. A
technology whose adoption would make that untrue is excluded, however capable it is.

</details>

<details>
<summary><strong>5.4 Open Standards First</strong></summary>

<br>

Where an open, independently implementable specification exists and is adequate, it is chosen over a
proprietary equivalent — even a better one.

**In practice.** A standard qualifies as open when its specification is public, its evolution is not
controlled by a single vendor's commercial interest, and at least two independent implementations
exist. Proprietary protocols with no published specification are Not Recommended by default.

</details>

<details>
<summary><strong>5.5 Long-term Stability</strong></summary>

<br>

The correct horizon for a technology decision is the lifetime of the projects built on it, not the
excitement of the quarter in which it was made.

**In practice.** Preference goes to technologies with a public release history, a stated
compatibility policy, and a record of handling their own breaking changes responsibly. A technology
that has broken its users without a migration path has told AEOS what to expect.

</details>

<details>
<summary><strong>5.6 Maintainability over Novelty</strong></summary>

<br>

A technology is adopted for what it costs to keep, not for what it costs to start.

**In practice.** Where two technologies are otherwise equivalent, the one that is plainer, more
widely understood, and easier for a stranger to maintain wins. Novelty MUST earn its place by
solving a problem the familiar option cannot.

</details>

<details>
<summary><strong>5.7 Mature Ecosystems Preferred</strong></summary>

<br>

Maturity is measured in years of production use, breadth of independent tooling, and the existence
of answers to ordinary questions that were not written by the vendor.

**In practice.** A technology with fewer than two years of stable public release history is
Experimental at best, regardless of adoption velocity.

</details>

<details>
<summary><strong>5.8 Community Support</strong></summary>

<br>

A technology maintained by one person, one company, or one funding round is a dependency on that
party's continued attention.

**In practice.** Preference goes to technologies with plural maintainership, public governance, and
a contribution process that a stranger has successfully used. Single-maintainer technologies MAY be
supported, and their concentration risk MUST be stated.

</details>

<details>
<summary><strong>5.9 Education Friendly</strong></summary>

<br>

AEOS is used by students, educators, and self-taught developers, and a technology that is
inaccessible to them narrows who can participate in engineering.

**In practice.** Preference goes to technologies that are free to learn, installable on ordinary
hardware, documented in a form a beginner can follow, and observable enough that a learner can see
what happened rather than only that it worked.

</details>

<details>
<summary><strong>5.10 Offline Friendly</strong></summary>

<br>

Engineering happens on aeroplanes, in secure facilities, in classrooms with hostile networks, and in
countries where connectivity is expensive.

**In practice.** Non-inference capability MUST remain usable without network access. A technology
that requires a network round trip to perform a local operation, or that cannot be installed once
and reused, is Not Recommended for Scope **P**.

</details>

<details>
<summary><strong>5.11 Local-first Where Practical</strong></summary>

<br>

Where a capability can be delivered from the user's machine at acceptable cost, it should be.
Local-first protects privacy, cost, latency, and the user's ability to work when a service is
unavailable — none of which are recoverable once the dependency is embedded.

**In practice.** Where a hosted service and a local equivalent both satisfy a need, the local option
is Preferred and the hosted option is Supported. "Practical" is judged on capability and cost, not
on convenience of integration.

**Its limit.** This principle does not apply across the AI categories (`TC-09`, `TC-10`, `TC-11`),
where Runtime Neutral and equal treatment of commercial and open-source AI govern instead. Local
inference is not ranked above hosted inference by AEOS; the choice between them is the user's, made
on privacy, cost, capability, and locality.

</details>

<details>
<summary><strong>5.12 Commercial AI and Open-source AI Treated Equally</strong></summary>

<br>

Commercial AI services and open-source models are the same kind of thing to AEOS: external
inference, chosen by the user, replaceable by the user.

**In practice.** Both appear in the same categories, at the same tiers, evaluated by the same
criteria. Documentation MUST NOT present either as the normal case and the other as the fallback,
and MUST NOT assume a commercial provider is more capable or an open-source model less serious.

</details>

<details>
<summary><strong>5.13 Technology May Evolve; Philosophy Does Not</strong></summary>

<br>

Every entry in this document is expected to change. Nothing in AEOS-VISION is.

**In practice.** A proposal that adds, promotes, demotes, or removes a technology is ordinary
governance. A proposal that requires an invariant to bend in order to accommodate a technology is
not a technology proposal at all, and is refused at this document's boundary rather than escalated.

</details>

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Governance statement</th></tr>
</thead>
<tbody>
<tr><td><code>TG-010</code></td><td>Every technology entry MUST be justifiable against the principles in this section, and its rationale MUST be recorded alongside it.</td></tr>
<tr><td><code>TG-011</code></td><td>Where two principles conflict, resolve in this order: Runtime Neutral, Platform Neutral, Vendor Neutral, Offline Friendly, Long-term Stability, then the remainder on the merits of the case.</td></tr>
<tr><td><code>TG-012</code></td><td>A technology proposal that requires an AEOS-VISION invariant to change MUST be refused, not escalated.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 6. Support Tiers

A tier states how strong a commitment AEOS has made, and therefore what a user may rely on.

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Definition</th><th align="left">What the user may rely on</th><th align="left">What AEOS owes</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Preferred</strong></td>
<td>The default choice within its category for new AEOS work. A strict subset of Officially Supported. A category MAY have no Preferred entry.</td>
<td>This is what AEOS itself uses and documents first; choosing it requires no justification.</td>
<td>Documentation, cross-platform verification, worked examples, and a migration path if it is ever demoted.</td>
</tr>
<tr>
<td><strong>Officially Supported</strong></td>
<td>Verified on all three platforms within its scope, documented, and maintained.</td>
<td>It works, and a break is a defect AEOS owns.</td>
<td>Documentation, verification, and release-blocking treatment of regressions.</td>
</tr>
<tr>
<td><strong>Conditionally Supported</strong></td>
<td>Supported subject to a stated condition — platform availability, project applicability, licensing, or external maintenance.</td>
<td>It works where the stated condition holds, and the condition is published rather than discovered.</td>
<td>Documentation including the condition. Regressions are tracked but are not release-blocking.</td>
</tr>
<tr>
<td><strong>Experimental</strong></td>
<td>Under evaluation. May be used, and may change or be withdrawn without a deprecation cycle.</td>
<td>Nothing beyond the ability to try it. It MUST NOT be a dependency of any <code>P0</code> capability.</td>
<td>An honest statement that it is provisional, and a recorded evaluation outcome.</td>
</tr>
<tr>
<td><strong>Not Recommended</strong></td>
<td>AEOS advises against the technology for AEOS-related purposes and provides no assets, documentation, or verification for it.</td>
<td>Clarity. Use is permitted and unsupported.</td>
<td>A stated reason. Never a prohibition, and never a judgement of the technology's quality in general.</td>
</tr>
</tbody>
</table>

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Governance statement</th></tr>
</thead>
<tbody>
<tr><td><code>TG-020</code></td><td>Every entry MUST carry exactly one tier. Preferred entries are additionally Officially Supported by definition.</td></tr>
<tr><td><code>TG-021</code></td><td>A technology MUST satisfy every gating criterion in <a href="#111-gating-criteria">Section 11.1</a> before it may be assigned any tier above Experimental.</td></tr>
<tr><td><code>TG-022</code></td><td>A Conditionally Supported entry MUST state its condition explicitly. A condition that cannot be stated is not a condition; the entry is Experimental.</td></tr>
<tr><td><code>TG-023</code></td><td>Not Recommended is a statement about fit with AEOS, never about a technology's quality or a vendor's conduct. Rationale MUST be written accordingly.</td></tr>
<tr><td><code>TG-024</code></td><td>A technology absent from this document is unsupported, not prohibited. AEOS MUST NOT refuse to operate because a project uses an unlisted technology.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 7. Technology Categories

AEOS organises technology into twenty categories. Each category states its **Purpose**, its
**Selection Criteria** beyond the universal criteria of [Section 11](#11-technology-evaluation-criteria),
and its expected **Future Evolution**. Tier assignments for each category are in
[Section 8](#8-officially-supported-technologies-by-category).

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Category</th><th align="left">Scope</th></tr>
</thead>
<tbody>
<tr><td><code>TC-01</code></td><td>Operating Systems</td><td>P · G</td></tr>
<tr><td><code>TC-02</code></td><td>Development Environments</td><td>I · G</td></tr>
<tr><td><code>TC-03</code></td><td>Programming Languages</td><td>P · G</td></tr>
<tr><td><code>TC-04</code></td><td>Frameworks</td><td>G</td></tr>
<tr><td><code>TC-05</code></td><td>Databases</td><td>G</td></tr>
<tr><td><code>TC-06</code></td><td>ORM and Data Access</td><td>G</td></tr>
<tr><td><code>TC-07</code></td><td>Package Managers</td><td>P · G</td></tr>
<tr><td><code>TC-08</code></td><td>Containers</td><td>I · G</td></tr>
<tr><td><code>TC-09</code></td><td>AI Providers</td><td>I</td></tr>
<tr><td><code>TC-10</code></td><td>Open-source Models</td><td>I</td></tr>
<tr><td><code>TC-11</code></td><td>Inference Runtimes</td><td>I</td></tr>
<tr><td><code>TC-12</code></td><td>Vector Databases</td><td>G</td></tr>
<tr><td><code>TC-13</code></td><td>MCP and Interoperability Standards</td><td>I</td></tr>
<tr><td><code>TC-14</code></td><td>Build Systems</td><td>P · G</td></tr>
<tr><td><code>TC-15</code></td><td>Testing Frameworks</td><td>P · G</td></tr>
<tr><td><code>TC-16</code></td><td>Documentation</td><td>P · G</td></tr>
<tr><td><code>TC-17</code></td><td>CI/CD</td><td>I · G</td></tr>
<tr><td><code>TC-18</code></td><td>Version Control</td><td>I · G</td></tr>
<tr><td><code>TC-19</code></td><td>Monitoring</td><td>G</td></tr>
<tr><td><code>TC-20</code></td><td>Security</td><td>P · G</td></tr>
</tbody>
</table>

<details>
<summary><strong>TC-01 — Operating Systems</strong></summary>

<br>

**Purpose.** The host environments on which AEOS runs and on which governed projects are built.

**Selection criteria.** An operating system is supported when AEOS can deliver identical product
capability on it, when its current releases receive vendor security support, and when the tooling in
every other category is available on it. Support is stated per release family rather than per
distribution build, because distributions vary faster than the guarantees that matter.

**Future evolution.** The three supported families are stable and no change is anticipated. Growth
will occur inside them — new Windows and macOS majors, new Linux LTS lines — and is handled by the
versioning policy rather than by adding categories. Container-hosted and remote development hosts
are treated as instances of these families, not as a fourth platform.

</details>

<details>
<summary><strong>TC-02 — Development Environments</strong></summary>

<br>

**Purpose.** The editing surfaces developers work in alongside AEOS.

**Selection criteria.** AEOS is IDE-agnostic, so an environment is supported when AEOS can be used
from it without the environment holding product state. The decisive criterion is negative: an
environment that requires rules, workflows, prompts, or project state to live in its own
configuration rather than in the repository cannot be Officially Supported, because that arrangement
contradicts Repository as Product regardless of how convenient it is.

**Future evolution.** This category churns faster than any other, as AI-assisted editors appear,
merge, and are absorbed. The list will change often and the criterion will not. AEOS MUST remain
fully usable from a terminal with no editor at all, which is what keeps this category from becoming
a dependency.

</details>

<details>
<summary><strong>TC-03 — Programming Languages</strong></summary>

<br>

**Purpose.** The languages AEOS itself is written in (Scope **P**) and the languages AEOS is
documented and tested to govern in user projects (Scope **G**).

**Selection criteria.** For Scope **P**: cross-platform toolchain, mature test runner, stable
packaging story, and a large pool of maintainers — including maintainers who did not write the
original code. For Scope **G**: a mainstream test-first workflow must exist, since TDD is the
default development workflow and a language without a credible test runner cannot be governed
test-first.

**Future evolution.** The Scope **P** set is deliberately small and is expected to stay that way;
adding an implementation language multiplies the maintenance surface permanently. The Scope **G**
set is expected to broaden steadily, driven by demand, and broadening it costs documentation and
verification rather than architecture.

</details>

<details>
<summary><strong>TC-04 — Frameworks</strong></summary>

<br>

**Purpose.** Application frameworks used inside governed projects.

**Selection criteria.** A framework is listed when AEOS can demonstrate that its workflows, rules,
and TDD cycle apply cleanly to projects built with it. Testability is the dominant criterion:
frameworks that resist test-first development are poor fits for AEOS regardless of popularity. AEOS
produces no application framework of its own and never will.

**Future evolution.** Expected to broaden as demand appears. The risk to watch is drift toward
opinionation: a long framework list can begin to read as a recommended stack, which is not what it
is. Each entry MUST remain clearly Scope **G**.

</details>

<details>
<summary><strong>TC-05 — Databases</strong></summary>

<br>

**Purpose.** Persistent data stores used inside governed projects.

**Selection criteria.** Local operability without a hosted account, a text-expressible schema and
migration story, and cross-platform availability. A database that cannot be run on a developer's
machine offline fails Offline Friendly and cannot be Preferred.

**Future evolution.** Stable. Embedded and single-file databases are becoming more capable and are
expected to cover a growing share of real projects, which suits Local-first Where Practical.

</details>

<details>
<summary><strong>TC-06 — ORM and Data Access</strong></summary>

<br>

**Purpose.** The layer through which governed projects express and evolve their data access.

**Selection criteria.** Migrations MUST be expressible as reviewable, diffable, version-controlled
artifacts — this is the criterion that matters most, because migrations are where data loss lives.
Beyond that: the ability to drop to raw queries, and behaviour that a maintainer can predict without
reading the library's source.

**Future evolution.** Expected to broaden cautiously. An ORM is a deep commitment for a project and
a shallow one for AEOS, so the list stays short and the rationale stays explicit. Using no ORM is
always a valid choice and is never treated as a gap.

</details>

<details>
<summary><strong>TC-07 — Package Managers</strong></summary>

<br>

**Purpose.** Dependency resolution, installation, and reproducibility for AEOS itself and for
governed projects.

**Selection criteria.** Deterministic lockfiles, an offline or cached installation mode,
cross-platform behavioural parity, and a lockfile format that reviews sensibly in a pull request.
Reproducibility is the whole purpose of this category; a package manager that resolves differently
on two machines has failed its only job.

**Future evolution.** Active. This category has changed substantially in recent years in both the
Python and JavaScript ecosystems, and further consolidation is likely. AEOS expects to revisit it
more often than most categories, and the versioning policy exists partly to absorb that.

</details>

<details>
<summary><strong>TC-08 — Containers</strong></summary>

<br>

**Purpose.** Isolated, reproducible environments for development, verification, and delivery.

**Selection criteria.** Conformance to open container specifications, cross-platform availability,
and the ability to be used without a commercial subscription for individual and educational use. The
governing constraint: containers MUST remain optional. Locked-down machines, air-gapped
environments, and shared classroom systems frequently cannot run them.

**Future evolution.** Stable and standard-driven. Alternative runtimes conforming to the same
specifications are expected to gain support as they mature, precisely because the specifications
make substitution cheap.

</details>

<details>
<summary><strong>TC-09 — AI Providers</strong></summary>

<br>

**Purpose.** Commercial services that perform inference on behalf of the user. They are integrations.
AEOS performs no inference of its own under any circumstance.

**Selection criteria.** A documented and versioned API, a stated deprecation policy, availability to
individual developers without enterprise negotiation, and — decisively — substitutability: the
provider MUST be replaceable by another listed provider without changing a project's rules, skills,
prompts, workflows, or repository structure. A provider whose integration would require special-case
handling in a project's assets fails this criterion no matter how capable it is.

**Future evolution.** The most volatile category in this document. Providers will consolidate,
disappear, change pricing, and be replaced. AEOS expects to absorb that churn rather than track it,
and the value of this category is measured by how little a change here disturbs a project.

</details>

<details>
<summary><strong>TC-10 — Open-source Models</strong></summary>

<br>

**Purpose.** Openly licensed model weights the user may run themselves. First-class runtimes, not
fallbacks.

**Selection criteria.** Openly published weights under a license permitting commercial and
educational use, availability across a range of sizes so that ordinary hardware is viable, an active
release lineage, and support by more than one listed inference runtime.

**Future evolution.** Fast-moving and improving. Model families listed here will be superseded by
their own successors, which is expected and is handled by the versioning policy rather than by
relisting. AEOS is not tuned to any model family and MUST NOT become so.

</details>

<details>
<summary><strong>TC-11 — Inference Runtimes</strong></summary>

<br>

**Purpose.** The local and self-hosted systems that execute open-source models.

**Selection criteria.** Support for multiple model families, an installation path an ordinary
developer can complete, and offline operation once models are present. Cross-platform availability
determines the tier ceiling: a runtime that exists on one platform may be useful and MUST NOT be
Preferred.

**Future evolution.** Active and consolidating around a small number of serving stacks. Hardware
acceleration remains the main source of platform asymmetry, and is the reason several entries in
this category are conditional rather than official.

</details>

<details>
<summary><strong>TC-12 — Vector Databases</strong></summary>

<br>

**Purpose.** Similarity search over embeddings inside governed projects.

**Selection criteria.** The strongest preference in this category is for capability added to an
already-supported database rather than for a new system: a project that gains vector search without
gaining a database gains less operational cost. Beyond that: local operation without a hosted
account, and persistence in an inspectable form.

**Future evolution.** Consolidating. The trend of general-purpose databases absorbing vector search
is expected to continue, which will likely shrink rather than grow this category — a good outcome
for maintainability.

</details>

<details>
<summary><strong>TC-13 — MCP and Interoperability Standards</strong></summary>

<br>

**Purpose.** Open specifications through which AEOS and AI runtimes interoperate without
vendor-specific coupling.

**Selection criteria.** A public specification, evolution not controlled by a single vendor's
commercial interest, at least two independent implementations, and a versioning policy for the
specification itself. This category exists to serve Open Standards First directly, and its entries
are held to that principle more strictly than anywhere else.

**Future evolution.** Standards in this space are young. AEOS expects the current generation to be
extended and eventually superseded, and expects to support more than one standard at a time during
transitions. Supporting a standard MUST NOT make it required: no product capability may be exclusive
to any distribution method or interoperability standard.

</details>

<details>
<summary><strong>TC-14 — Build Systems</strong></summary>

<br>

**Purpose.** Turning source into runnable and distributable artifacts, and running repeatable
development tasks.

**Selection criteria.** Identical invocation and identical results on all three platforms. This
criterion eliminates more candidates than any other in this category, because a great many build and
task tools assume a POSIX shell. Beyond that: language-native tooling is preferred over a general
build system, on the grounds that fewer layers are easier to maintain.

**Future evolution.** Stable for single-language projects. Monorepo and multi-language orchestration
is the area most likely to change, and is deliberately kept at a lower tier until a cross-platform
option proves itself over several years.

</details>

<details>
<summary><strong>TC-15 — Testing Frameworks</strong></summary>

<br>

**Purpose.** Executing the tests that make test-first development real. This is the most
consequential category in the document: TDD is the default development workflow, and a project's
test runner is the mechanism through which that default is enforced.

**Selection criteria.** A machine-readable result format, a non-zero exit code on failure, failure
output detailed enough to act on, deterministic ordering options, and cross-platform parity. AEOS
orchestrates the project's own test tooling and provides no test framework of its own.

**Future evolution.** Stable. Runners are long-lived and their result formats are converging on
common standards, which is what allows AEOS to orchestrate them without special-casing each one.

</details>

<details>
<summary><strong>TC-16 — Documentation</strong></summary>

<br>

**Purpose.** The formats in which AEOS documentation and governed-project documentation are
authored, reviewed, and consumed by humans and AI runtimes from the same artifact.

**Selection criteria.** Plain-text source, meaningful diffs, correct rendering on GitHub, and
readability by an AI runtime without a separate machine-only version existing. A format that
requires a build step to be readable, or that only reviews as a binary blob, cannot be a source of
truth here.

**Future evolution.** Stable. Documentation *sites* are a presentation concern and may change
freely; the source format is expected to remain plain text indefinitely, because the two-audience
requirement admits nothing else.

</details>

<details>
<summary><strong>TC-17 — CI/CD</strong></summary>

<br>

**Purpose.** Continuous verification and delivery systems that AEOS orchestrates and never replaces.

**Selection criteria.** Configuration expressed as version-controlled text in the project's own
repository, hosted runners for all three platforms, and the ability to run the same pipeline locally
or on self-hosted infrastructure. A pipeline whose definition lives in a web console rather than the
repository cannot be Officially Supported, because the project's own truth would then live outside
the project.

**Future evolution.** Stable in shape, competitive in market. Broadening this list costs
documentation and verification, and AEOS's neutrality obligation makes broadening it a matter of
demand rather than preference.

</details>

<details>
<summary><strong>TC-18 — Version Control</strong></summary>

<br>

**Purpose.** The system of record for the repository, which is the product.

**Selection criteria.** Distributed operation with complete local history, content-addressed
integrity, offline operation, and universal tooling support. Hosting platforms are evaluated
separately from the version control system itself, and a project with no remote at all MUST remain
fully supported.

**Future evolution.** The most stable category in this document. Hosting platforms may change; the
underlying version control system is not expected to.

</details>

<details>
<summary><strong>TC-19 — Monitoring</strong></summary>

<br>

**Purpose.** Observability of governed projects in the environments their owners choose.

**Selection criteria.** Open instrumentation standards, a self-hosted option, and the ability to
operate without transmitting project content to a third party. This category is governed by a
constraint rather than a preference: AEOS ships no default telemetry, and monitoring is opt-in and
user-owned. Telemetry is Runtime State — it describes usage, never the product — and MUST NOT become
a Repository Asset.

**Future evolution.** Converging on open instrumentation standards, which is the outcome AEOS wants:
vendor-neutral instrumentation makes the backend an ordinary, replaceable choice.

</details>

<details>
<summary><strong>TC-20 — Security</strong></summary>

<br>

**Purpose.** Credential handling, dependency and secret scanning, provenance, and supply-chain
integrity for AEOS and for governed projects.

**Selection criteria.** Local execution without uploading source to a third party by default, open
and standard output formats, cross-platform availability, and free availability for individual and
educational use. The absolute constraint: credentials and secrets are never written into prompts,
logs, reports, documentation, or Repository Assets, and no technology may be listed that would
require otherwise.

**Future evolution.** Active, standards-driven, and increasingly required by regulation. Software
bills of materials and artifact signing are expected to move from good practice to expectation, and
this category will grow accordingly.

</details>

</section>

---

<section>

## 8. Officially Supported Technologies by Category

Tier assignments as of this version. Rationale accompanies each assignment; the extended reasoning
for the headline choices is in [Section 9](#9-the-initial-official-technology-set).

> **How to read these tables.** Order within a tier is alphabetical and confers no ranking.
> A blank cell means the category has no entry at that tier today, which is a statement of fact
> rather than a gap awaiting a filler.

<details>
<summary><strong>TC-01 — Operating Systems</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>Naming a preferred platform would contradict the equal-citizen commitment. This blank is deliberate and permanent.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Linux (current LTS and stable releases of mainstream distributions) · macOS (current and previous major) · Windows (current supported client releases)</td><td>All three host serious engineering work, receive vendor security support, and carry the tooling in every other category.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>WSL2 · Rolling-release Linux distributions</td><td>WSL2 is supported <em>as a Linux environment</em> and never as a substitute for native Windows support. Rolling releases move faster than verification can follow; supported where the user accepts that.</td></tr>
<tr><td><strong>Experimental</strong></td><td>BSD family</td><td>Technically plausible, not currently verified. Usable; unverified.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Operating system releases past vendor end-of-life</td><td>Unpatched hosts cannot be secured, and verification against them would misrepresent what AEOS can promise.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-02 — Development Environments</strong> · Scope I · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>Editors are a matter of long-held personal preference. Preferring one would make AEOS an adoption obstacle for everyone using another.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Cursor · Terminal / shell (no editor) · VS Code · Windsurf</td><td>All three named editors are cross-platform, keep project state in the repository, and are widely used for AI-assisted work. The terminal entry is listed first-class deliberately: it is the guarantee that this category never becomes a dependency.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Emacs · GitHub Copilot (as an assistant within a supported editor) · JetBrains IDEs · Neovim · Zed</td><td>All are capable and cross-platform; none is currently covered by AEOS verification. Condition: the user accepts documentation-only support.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Browser-hosted and remote development environments</td><td>Promising for education and locked-down machines; local tooling assumptions are not yet verified against them.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Environments that require rules, workflows, prompts, or project state to live in editor-owned configuration</td><td>Product state outside the repository contradicts Repository as Product. This is a fit judgement about state ownership, not about editor quality.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-03 — Programming Languages</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>Python (Scope P · G) · TypeScript (Scope P · G)</td><td>Between them they cover the tooling, service, and interface work AEOS performs and governs, with mature test-first ecosystems on all three platforms.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Python · TypeScript</td><td>Deliberately equal to the Preferred set at 1.0. A small Scope <strong>P</strong> language set is a permanent maintenance decision, not a starting position.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>C# (G) · Go (G) · Java (G) · JavaScript (G) · Rust (G)</td><td>All have strong test-first ecosystems and are common in real projects AEOS will be asked to adopt. Condition: Scope <strong>G</strong> only — AEOS governs projects in these languages and is not written in them.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Other languages with a mainstream test runner</td><td>Governance is plausible; documentation and verification do not yet exist.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Languages with no maintained cross-platform toolchain, or no credible automated test runner</td><td>TDD is the default workflow. A language that cannot be driven test-first cannot be governed by AEOS as designed.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-04 — Frameworks</strong> · Scope G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>Framework choice belongs to the project. A preferred framework would read as a recommended stack, which this document is not.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>FastAPI · React · React Native</td><td>One service framework and one interface family, each testable without a running browser or server, each with large independent ecosystems. React Native extends the same interface model to mobile without introducing a second paradigm.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Django · Express · Flask · Next.js · Svelte · Vue</td><td>All are mature and testable. Condition: documentation-only support, and framework-specific build conventions are the project's responsibility.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Emerging frameworks with fewer than two years of stable release history</td><td>Adoption velocity is not maturity. Evaluated on the standard cycle.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Frameworks whose primary workflow is a hosted visual builder, or that cannot be tested without a proprietary service</td><td>Untestable-without-a-service contradicts TDD-first and Offline Friendly. Not a comment on their suitability elsewhere.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-05 — Databases</strong> · Scope G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>PostgreSQL · SQLite</td><td>SQLite for embedded, zero-administration, fully offline work; PostgreSQL for everything that outgrows it. The pair covers most of the range with one dialect family and one migration story.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>PostgreSQL · SQLite</td><td>Both are open source, cross-platform, decades old, exhaustively documented, and runnable on a laptop without an account.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>DuckDB · MariaDB · MySQL</td><td>Mature and locally operable. Condition: documentation-only support; DuckDB is analytical rather than transactional and should be chosen accordingly.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Distributed SQL and embedded alternatives with wire compatibility to a supported database</td><td>Compatibility makes evaluation cheap; verification has not yet been performed.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Databases with no local development mode, or whose schema and migrations cannot be expressed as version-controlled text</td><td>A project whose schema lives in a console cannot be reproduced from its repository.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-06 — ORM and Data Access</strong> · Scope G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None</em></td><td>Data access is among the most consequential decisions a project makes, and the least suitable for a default supplied by a governance document. Using no ORM is a valid choice.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Alembic (migrations) · SQLAlchemy</td><td>Long-lived, extensively documented, works with both Preferred databases, and expresses migrations as reviewable version-controlled files — the criterion that matters most in this category.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Drizzle ORM · Prisma · SQLModel</td><td>All express migrations as reviewable artifacts. Condition: documentation-only support; Prisma's generated client and engine add a build step projects must account for.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Query builders and lightweight mappers in supported languages</td><td>Frequently a better fit than a full ORM; not yet documented or verified.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Tools that apply schema changes without producing a reviewable migration artifact</td><td>Unreviewable schema change is where irrecoverable data loss originates. The strongest recommendation against anything in this document.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-07 — Package Managers</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>pnpm (JavaScript / TypeScript) · uv (Python)</td><td>Both produce deterministic lockfiles, install from cache offline, and behave identically on all three platforms — the three properties this category exists to guarantee.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>pnpm · uv</td><td>Reproducibility is not a preference in AEOS; it is a quality attribute. These are the tools that deliver it most reliably today.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>npm · pip with virtual environments · Poetry · Yarn</td><td>All are widespread and will be encountered in adopted projects, which AEOS must handle gracefully. Condition: a committed lockfile MUST be present.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Emerging resolvers in supported language ecosystems</td><td>This category has moved substantially in recent years and is expected to continue.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Workflows without a committed lockfile, and global installation as a project's dependency strategy</td><td>Without a lockfile, no two machines are running the same project, and reproducibility claims become untrue without anyone noticing.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-08 — Containers</strong> · Scope I · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>Containers MUST remain optional. A Preferred container technology would imply a default that many supported environments cannot run.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Docker · Docker Compose</td><td>The most widely available implementation of the open container specifications on all three platforms, and the one contributors are most likely to already have.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>containerd · Podman</td><td>Specification-conformant and often preferable on Linux and in restricted environments. Condition: cross-platform behavioural parity is the user's responsibility.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Development-container specifications for environment definition</td><td>Attractive for teaching and onboarding; not yet verified across all three platforms.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Making containers a prerequisite for any AEOS capability</td><td>Air-gapped, locked-down, and shared classroom machines frequently cannot run them. Requiring containers would exclude exactly the users Offline Friendly protects.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-09 — AI Providers</strong> · Scope I</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy — permanently</em></td><td>A Preferred AI provider would convert vendor independence from a structural fact into a marketing position. This blank is the load-bearing one in this document.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Anthropic · Google · OpenAI</td><td>Each publishes a documented, versioned API with a stated deprecation policy, is available to individual developers without enterprise negotiation, and is substitutable for the others without changing a project's assets. Listed alphabetically; the ordering means nothing.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Cloud-hosted model gateways (for example, managed enterprise endpoints for the above) · Providers offering an API compatible with a supported provider</td><td>Condition: availability, quotas, regional restrictions, and model catalogue are the operator's, not AEOS's. Compatibility layers are supported to the extent they are genuinely compatible.</td></tr>
<tr><td><strong>Experimental</strong></td><td>New commercial providers meeting the gating criteria</td><td>Evaluated on the standard cycle. Entry is expected to be routine, not exceptional.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Providers with no published API stability or deprecation policy, or whose terms prohibit substitution</td><td>An integration that cannot be replaced is a dependency acquired by accident — the exact failure mode independence exists to prevent.</td></tr>
</tbody>
</table>

<blockquote>
AEOS performs no inference. Every entry in this category is external, user-selected, user-approved
before invocation, and replaceable. The absence of every provider listed here reduces the user's
options and disables nothing.
</blockquote>

</details>

<details>
<summary><strong>TC-10 — Open-source Models</strong> · Scope I</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>Model selection belongs to the user and depends on hardware, privacy, cost, and task. AEOS is not tuned to any model family and MUST NOT become so.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Gemma · Llama · Mistral · Qwen</td><td>Each publishes openly licensed weights across several sizes, has an active release lineage, and is runnable by more than one supported inference runtime. The size range matters: it is what makes these usable on a student's laptop and on a server.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Other openly licensed families with permissive commercial and educational terms</td><td>Condition: the user verifies license terms for their use, which vary meaningfully between families and releases.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Newly released families and specialised code models</td><td>Frequently excellent and frequently short-lived. Evaluated on the standard cycle.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Weights published without clear license terms, or under terms prohibiting commercial or educational use</td><td>Unclear licensing transfers legal risk to the user silently. A statement about terms, not about quality.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-11 — Inference Runtimes</strong> · Scope I</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>Hardware determines the right answer here more than governance can. Preferring one would penalise users whose machines suit another.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>LM Studio · Ollama · Transformers</td><td>All three run on all three platforms and support all four supported model families. Ollama offers the simplest offline path, LM Studio the most approachable one for learners, and Transformers the reference implementation everything else is measured against.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>llama.cpp · MLX · vLLM</td><td>Condition: platform and hardware limits. vLLM targets server-class Linux with accelerators; MLX is macOS-only on Apple silicon; llama.cpp is broadly portable but requires more assembly. Each is excellent within its condition, and none may be Preferred while the condition holds.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Emerging serving stacks with OpenAI-compatible interfaces</td><td>Interface compatibility makes substitution cheap and evaluation inexpensive.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Runtimes requiring a network round trip to serve a locally stored model</td><td>Defeats the reason to run locally: privacy, offline capability, and cost.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-12 — Vector Databases</strong> · Scope G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>pgvector · sqlite-vec</td><td>Both add vector search to a database the project already supports, runs, backs up, and understands. Gaining a capability without gaining a system is the best available outcome in this category.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>pgvector · sqlite-vec</td><td>Open source, local-first, cross-platform, and persisted in an inspectable form alongside the project's other data.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Chroma · Qdrant</td><td>Both offer genuine local modes and are appropriate at scales the embedded options do not reach. Condition: the project accepts operating an additional system.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Other locally operable vector stores</td><td>A fast-moving field that is consolidating; evaluation is deferred rather than declined.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Hosted-only vector services with no local development mode</td><td>Cannot be developed against offline, and makes a project's data layer unreproducible from its repository.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-13 — MCP and Interoperability Standards</strong> · Scope I</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>MCP</td><td>The only open, independently implemented, vendor-neutral standard currently addressing runtime interoperability at the level AEOS needs. Preferred here because standards, unlike vendors, are the thing this document is supposed to prefer.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>MCP · OpenAPI-described interfaces</td><td>Both are public specifications with multiple independent implementations and their own versioning policies. OpenAPI covers integration surfaces that predate and sit outside MCP.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Language Server Protocol · Vendor extensions to a supported standard</td><td>Condition: an extension is supported only where the base standard remains sufficient without it. LSP is relevant to editor integration rather than to inference.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Emerging agent and tool interoperability specifications</td><td>The field is young. AEOS expects to support more than one standard concurrently during transitions.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Proprietary protocols with no published specification or no independent implementation</td><td>A protocol only one party can implement is that party's product, not a standard, and adopting it would import the coupling AEOS exists to avoid.</td></tr>
</tbody>
</table>

<blockquote>
Support for a standard never makes it required. No product capability is exclusive to MCP, to any
other standard, or to any distribution method.
</blockquote>

</details>

<details>
<summary><strong>TC-14 — Build Systems</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>Language-native build tooling (for example, the build backend invoked by the language's package manager)</td><td>Fewer layers, no shell assumptions, and identical behaviour on all three platforms. Preferring the native path is preferring the plainer solution.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Language-native build tooling · Vite</td><td>Vite is the front-end build path used with the supported interface frameworks; it is cross-platform, configuration-light, and fast enough that verification stays cheap.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>just · Make</td><td>Condition: cross-platform parity is the project's responsibility. Make in particular assumes a POSIX shell, which is exactly the assumption this category is most alert to.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Monorepo orchestration systems</td><td>Real value at scale, significant complexity, and cross-platform behaviour that has not been verified over enough years.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Build definitions requiring a platform-specific shell or non-portable path handling</td><td>The most common way platform equality is lost in practice, and the hardest to notice until a contributor on another platform cannot build.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-15 — Testing Frameworks</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>Playwright (end-to-end) · pytest (Python) · Vitest (TypeScript unit and integration)</td><td>One preferred runner per layer, each with machine-readable results, actionable failure output, and cross-platform parity. These are the runners AEOS's own test-first development uses.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Jest · Playwright · pytest · Vitest</td><td>Jest is Officially Supported rather than Preferred: it is present in an enormous number of existing projects AEOS will be asked to adopt, and adoption safety matters more than tidiness. New TypeScript work should prefer Vitest.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Cypress · Testing Library · unittest</td><td>Condition: documentation-only support. Testing Library complements rather than replaces a runner and is listed for clarity about that relationship.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Emerging runners in supported languages · Property-based testing libraries</td><td>Property-based testing is a strong fit for AEOS's philosophy and is not yet documented in AEOS assets.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Runners without machine-readable output, or that do not return a non-zero exit code on failure</td><td>AEOS gates workflow progression on test results. A runner that cannot report failure unambiguously cannot be gated on, which makes it unusable for the product's central discipline.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-16 — Documentation</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>GitHub-Flavored Markdown with semantic HTML, per AEOS-DOCSTD</td><td>Plain text, meaningful diffs, correct GitHub rendering, and directly consumable by AI runtimes — one artifact serving both audiences, which is the requirement no other format meets as well.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>GitHub-Flavored Markdown with semantic HTML · Mermaid (diagrams)</td><td>Mermaid keeps diagrams in text: reviewable in a pull request, readable by an AI runtime, and rendered by GitHub without a build step.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Docusaurus · MkDocs · Sphinx</td><td>Condition: presentation only. A documentation site MAY be generated from repository sources; it MUST NOT become the source of truth.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Structured documentation formats with stronger machine semantics</td><td>Potentially valuable for AI consumption; must not cost human readability, which no candidate has yet demonstrated.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Binary or proprietary document formats as a source of truth · Externally hosted wikis as a source of truth</td><td>Neither diffs, neither reviews, and neither survives the tool that produced it. Both contradict Repository as Product.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-17 — CI/CD</strong> · Scope I · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td><em>None by policy</em></td><td>A project's pipeline is deeply embedded in how its organisation operates. Preferring one would make AEOS adoption a migration.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>GitHub Actions</td><td>Configuration lives as text in the repository, hosted runners exist for all three platforms — which is what allows AEOS to verify platform equality rather than assert it — and it is available to open-source projects at no cost.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Azure Pipelines · CircleCI · GitLab CI · Jenkins</td><td>All express pipelines as version-controlled text. Condition: documentation-only support; platform runner availability is the operator's responsibility.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Local pipeline emulation for offline verification</td><td>Valuable for Offline Friendly; fidelity to hosted execution is not yet established.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Pipelines defined only in a web console rather than in the repository</td><td>The project's own build truth would live outside the project, and could not be reviewed, diffed, or reproduced.</td></tr>
</tbody>
</table>

<blockquote>
AEOS orchestrates the project's delivery systems and never replaces them. Deployment is a
destructive action requiring explicit, specific confirmation every time, regardless of which system
in this category performs it.
</blockquote>

</details>

<details>
<summary><strong>TC-18 — Version Control</strong> · Scope I · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>Git</td><td>Distributed, complete local history, content-addressed integrity, fully offline, universally tooled, and effectively without an alternative in current practice. The repository is the product, and Git is how it endures.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Git · GitHub (hosting)</td><td>Git is the system of record. GitHub is supported as a hosting platform because AEOS documentation targets its rendering and its automation surfaces — a documentation and verification commitment, not a requirement.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Azure Repos · Bitbucket · Gitea · GitLab</td><td>All host Git faithfully. Condition: platform-specific automation and rendering are the project's responsibility.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Git-compatible version control front-ends</td><td>Interesting where they preserve full Git interoperability, which is the only property that matters for support.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Centralised version control without complete local history</td><td>Offline work, local verification, and repository-as-product all depend on the full history being present on the machine.</td></tr>
</tbody>
</table>

<blockquote>
A repository with no remote is fully supported. Hosting is a convenience, never a requirement, and
no AEOS capability may depend on the presence of one.
</blockquote>

</details>

<details>
<summary><strong>TC-19 — Monitoring</strong> · Scope G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>Structured local logging under the project's control</td><td>The least that suffices, owned entirely by the user, requiring no network and no third party. Observability should start here and grow only when a project genuinely needs more.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>OpenTelemetry (instrumentation) · Structured local logging</td><td>OpenTelemetry is vendor-neutral instrumentation with a self-hosted path, which makes the backend an ordinary replaceable choice rather than a lock-in point.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>Grafana · Prometheus · Sentry</td><td>Condition: opt-in, project-owned, and configured so that project content and credentials are not transmitted without explicit approval.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Local-first observability backends</td><td>Aligned with Local-first Where Practical; not yet verified across all three platforms.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Any monitoring that transmits project content off-machine by default · Default-on telemetry in AEOS itself</td><td>AEOS ships no default telemetry. Anything crossing the machine boundary is disclosed and approved first, without exception.</td></tr>
</tbody>
</table>

</details>

<details>
<summary><strong>TC-20 — Security</strong> · Scope P · G</summary>

<br>

<table>
<thead>
<tr><th align="left">Tier</th><th align="left">Technologies</th><th align="left">Rationale</th></tr>
</thead>
<tbody>
<tr><td><strong>Preferred</strong></td><td>Platform-native credential stores (Linux Secret Service · macOS Keychain · Windows Credential Manager)</td><td>Credentials belong to a person or an organisation, never to a repository. The platform's own store is the mechanism the user already trusts and already controls.</td></tr>
<tr><td><strong>Officially Supported</strong></td><td>Dependency auditing built into supported package managers · gitleaks · Platform-native credential stores · Trivy</td><td>All run locally without uploading source, produce machine-readable output, and are available on all three platforms at no cost to individuals and educators.</td></tr>
<tr><td><strong>Conditionally Supported</strong></td><td>CycloneDX / SPDX software bills of materials · Dependabot · Renovate · Sigstore artifact signing</td><td>Condition: several depend on a hosting platform or a public transparency service, so availability follows the project's hosting choices rather than AEOS's.</td></tr>
<tr><td><strong>Experimental</strong></td><td>Commercial SAST and supply-chain platforms</td><td>Often valuable; evaluated case by case against the no-upload-by-default criterion.</td></tr>
<tr><td><strong>Not Recommended</strong></td><td>Storing credentials or secrets in Repository Assets, environment files committed to version control, or documentation · Scanners that transmit source without explicit approval</td><td>Absolute. Secrets are never written into prompts, logs, reports, documentation, or Repository Assets, and no technology may be adopted that would require otherwise.</td></tr>
</tbody>
</table>

</details>

</section>

---

<section>

## 9. The Initial Official Technology Set

The complete tier assignments are in [Section 8](#8-officially-supported-technologies-by-category)
and summarised in [Appendix A](#appendix-a--technology-matrix). This section explains *why* the
headline choices were made, because a list without reasoning cannot be argued with, corrected, or
inherited — and a governance document that cannot be argued with is not governance.

Read together, the set expresses one preference above all others: **the smallest number of
technologies that can honestly cover the work, each chosen because it will still be maintainable
when nobody involved in choosing it is still here.**

<details>
<summary><strong>Why Windows, macOS, and Linux — with no preferred platform</strong></summary>

<br>

All three host serious engineering work, and each hosts developers accustomed to being second-class
somewhere. Naming a preferred platform would make that experience official.

The practical consequence is felt in every other category: it eliminates otherwise excellent tools
that assume a POSIX shell, and it is the reason several inference runtimes and build tools sit at
Conditionally Supported despite being the best available option on the platform where they run. The
cost is real and is paid deliberately.

</details>

<details>
<summary><strong>Why VS Code, Cursor, and Windsurf — and why the terminal is listed alongside them</strong></summary>

<br>

The three named editors are cross-platform, widely used for AI-assisted work, and — decisively — do
not require project state to live in editor configuration.

The terminal entry matters more than the editors. It is the guarantee that this category never
becomes a dependency: a contributor with none of these installed, on a machine where none can be
installed, can still use AEOS completely. Every editor in this category is a convenience, and listing
the terminal at the same tier is how that stays true as the category churns.

</details>

<details>
<summary><strong>Why Python and TypeScript</strong></summary>

<br>

Two languages, chosen to cover the range with the smallest permanent maintenance surface.

Python brings the strongest ecosystem for tooling, automation, and AI-adjacent work, a test-first
culture that predates the current wave, and a low barrier for learners and researchers. TypeScript
brings type safety to the interface and service layers, is the default of the web and mobile
ecosystems AEOS governs, and its type system makes generated code more reviewable — which matters
disproportionately when a substantial share of code is produced by an AI runtime.

Both are cross-platform, both have first-class test runners, and both are read fluently by every
current AI runtime, which is a real criterion when AI runtimes are first-class readers of the
repository. A third implementation language would multiply the maintenance surface permanently in
exchange for a benefit that has not yet been demonstrated.

</details>

<details>
<summary><strong>Why FastAPI, React, and React Native</strong></summary>

<br>

One service framework and one interface family, both testable without a running server or browser —
the property that determines whether a framework can be governed test-first at all.

FastAPI's explicitness suits AI-assisted work: its type-driven interfaces make generated code easier
to review, and its documentation generation follows from the code rather than being maintained
beside it. React's component model tests cleanly at the unit level and is the largest independent
interface ecosystem. React Native extends the same model to mobile without asking a project to learn
a second paradigm — which is the reason it is here rather than a native mobile toolkit, since neither
of those can be developed on all three platforms.

These are Scope **G** entries. A project using none of them is fully supported, and this list is not
a recommended stack.

</details>

<details>
<summary><strong>Why SQLite and PostgreSQL</strong></summary>

<br>

A pair that covers the range from a single file to production scale within one dialect family and one
migration story.

SQLite is the most widely deployed database in existence, requires no server, no account, and no
network, and makes fully offline development ordinary rather than special. PostgreSQL is where a
project goes when it outgrows that: open source, decades old, exhaustively documented, and available
from every hosting provider without vendor coupling. It also carries the Preferred vector extension,
which lets a project add similarity search without adding a system.

Both are open source, both run on all three platforms, and both can be operated by a student on a
laptop and by an organisation in production without changing the project's data access.

</details>

<details>
<summary><strong>Why uv and pnpm</strong></summary>

<br>

Reproducibility is a stated quality attribute of AEOS, and this category is where reproducibility is
won or lost.

Both produce deterministic lockfiles, install from a local cache without a network, and behave
identically on all three platforms. Both are substantially faster than their predecessors, which
sounds like a convenience and is not: a slow install is a slow verification cycle, and a slow
verification cycle is the most common reason a team quietly stops running the full suite.

Their predecessors remain Conditionally Supported because adopted projects will use them, and
adoption safety outranks tidiness in every judgement this document makes.

</details>

<details>
<summary><strong>Why Docker — and why containers are never required</strong></summary>

<br>

Docker is the most widely available implementation of the open container specifications on all three
platforms and the one a contributor is most likely to already have.

The more important half of the decision is the constraint. Containers MUST remain optional, because
locked-down corporate machines, air-gapped environments, and shared classroom systems frequently
cannot run them — and those are precisely the users Offline Friendly and Education Friendly exist to
protect. Docker's conformance to open specifications is also what makes Podman and containerd
inexpensive to support, so this choice buys optionality rather than spending it.

</details>

<details>
<summary><strong>Why Anthropic, Google, and OpenAI — listed alphabetically, ranked never</strong></summary>

<br>

Three providers, not one, because a single-provider category would be a preference wearing the
costume of a technology decision.

Each publishes a documented and versioned API with a stated deprecation policy, is available to an
individual developer without enterprise negotiation, and — the criterion that decided it — is
substitutable for the others without changing a project's rules, skills, prompts, workflows, or
repository structure. That substitutability is the entire point. If switching between them required
touching a project's assets, runtime independence would be aspirational and AEOS would know it.

The permanently empty Preferred row in this category is the most load-bearing blank in this document.

</details>

<details>
<summary><strong>Why Gemma, Llama, Mistral, and Qwen</strong></summary>

<br>

Four families, listed at the same tier as the commercial providers above, because commercial AI and
open-source AI are the same kind of thing to AEOS: external inference, chosen by the user.

Each publishes openly licensed weights, has an active release lineage rather than a single drop, and
is runnable by more than one supported inference runtime. Each is also available across a range of
sizes, which is the criterion that decides whether local inference is a real option or a
demonstration: a family that only exists at sizes requiring server-class hardware cannot serve a
student on a laptop, a developer on a train, or an organisation that cannot send its code anywhere.

</details>

<details>
<summary><strong>Why Ollama, LM Studio, vLLM, and Transformers</strong></summary>

<br>

Four runtimes covering four genuinely different situations, deliberately spanning the tier boundary.

Ollama is the simplest path to a working local model on any of the three platforms, with a single
install and offline operation thereafter. LM Studio adds a graphical interface, which matters more
than it sounds: it is often the difference between a learner running a model and a learner reading
about one. Transformers is the reference implementation, the widest model support, and the runtime a
researcher will reach for when reproducibility matters more than throughput.

vLLM is Conditionally Supported rather than Officially Supported, and the reason is instructive. It
is the strongest option for serving at throughput, and it targets server-class Linux with
accelerators. Platform neutrality is not a claim AEOS can make selectively, so a technology that
cannot be Preferred on all three platforms is not Preferred on any — the condition is published
rather than quietly absorbed. MLX sits at the same tier for the same reason on the opposite platform.

</details>

<details>
<summary><strong>Why Git and GitHub</strong></summary>

<br>

Git is Preferred and effectively without an alternative: distributed, complete local history,
content-addressed integrity, fully offline, and universally tooled. The repository is the product,
and Git is the mechanism by which the repository outlives everything around it.

GitHub is supported as a hosting platform, at a lower level of commitment and for a narrower reason:
AEOS documentation targets its Markdown rendering and its automation surfaces, and that targeting is
a verification commitment rather than a requirement. Four alternative hosts are Conditionally
Supported, and a repository with no remote at all is fully supported. Nothing AEOS does may depend on
where a repository is hosted, or on whether it is hosted.

</details>

<details>
<summary><strong>Why pytest, Vitest, Playwright, and Jest</strong></summary>

<br>

This is the most consequential set in the document. TDD is the default development workflow, AEOS
gates workflow progression on test results, and these are the tools through which that gate operates.

pytest is the Python standard by a wide margin: minimal ceremony to write a first test, powerful
fixtures when a project needs them, and machine-readable output AEOS can act on. Vitest is preferred
for new TypeScript work — fast, aligned with the supported build tooling, and largely
API-compatible with Jest, which makes migration cheap. Playwright covers end-to-end testing across
browsers on all three platforms, which no alternative currently matches.

Jest is Officially Supported but not Preferred, and the distinction is deliberate rather than
diplomatic. An enormous number of existing projects use it, and adoption is the harder and more
common case AEOS must handle safely. A project on Jest is fully supported and under no obligation to
move; a new project should choose Vitest. Saying both plainly is more useful than pretending the
answer is uniform.

</details>

</section>

---

<section>

## 10. Technology Lifecycle

Every technology in this document occupies a lifecycle state. The state describes where a technology
stands in its relationship with AEOS; the support tier describes how strong AEOS's commitment is
today. The two are related but not identical, and the mapping is stated below so that neither
vocabulary can be mistaken for the other.

### 10.1 Lifecycle States

<table>
<thead>
<tr><th align="left">State</th><th align="left">Meaning</th><th align="left">Obligations while in this state</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Candidate</strong></td>
<td>Proposed and under evaluation. Not yet a commitment.</td>
<td>Evaluation against the criteria in <a href="#11-technology-evaluation-criteria">Section 11</a> MUST be recorded, including the outcome if it is declined.</td>
</tr>
<tr>
<td><strong>Supported</strong></td>
<td>Documented, verified within its scope, and maintained.</td>
<td>Documentation MUST exist. Regressions are defects AEOS owns within the limits of the assigned tier.</td>
</tr>
<tr>
<td><strong>Preferred</strong></td>
<td>The default choice within its category for new AEOS work.</td>
<td>Everything Supported requires, plus worked examples and first-position documentation. At most a small number per category.</td>
</tr>
<tr>
<td><strong>Deprecated</strong></td>
<td>Still supported, scheduled for removal, and no longer chosen for new work.</td>
<td>A stated reason, a stated replacement or the honest absence of one, a migration path, and a minimum notice period before removal.</td>
</tr>
<tr>
<td><strong>Removed</strong></td>
<td>No longer supported. Retained in the revision history rather than deleted.</td>
<td>The entry and its removal rationale MUST be preserved so that a future reader can tell a considered removal from an oversight.</td>
</tr>
</tbody>
</table>

### 10.2 Movement Between States

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
    proven in use,      |  |  demoted: better option, or                | notice period
    owner approved      |  |  criteria no longer met                    | elapsed
                        v  |                                            v
                 +-------------+                                 +-------------+
                 |  PREFERRED  |                                 |   REMOVED   |
                 +-------------+                                 +-------------+
```

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Governance statement</th></tr>
</thead>
<tbody>
<tr><td><code>TG-030</code></td><td>Every promotion, demotion, deprecation, and removal MUST be approved by the owner and recorded in this document's revision history.</td></tr>
<tr><td><code>TG-031</code></td><td>A technology MUST spend a stated evaluation period as a Candidate before promotion to Supported. The period MUST be recorded with the evaluation.</td></tr>
<tr><td><code>TG-032</code></td><td>A technology MUST be Supported before it may become Preferred. Direct promotion from Candidate to Preferred is not permitted.</td></tr>
<tr><td><code>TG-033</code></td><td>Removal MUST be preceded by Deprecation for at least one AEOS minor release. Emergency removal for a security or licensing reason is the sole exception and MUST be recorded as such with its rationale.</td></tr>
<tr><td><code>TG-034</code></td><td>Deprecation MUST state a replacement, or MUST state plainly that none exists. Silent deprecation is a defect.</td></tr>
<tr><td><code>TG-035</code></td><td>Removed entries MUST NOT be deleted from the document's history. A future reader MUST be able to distinguish a considered removal from an omission.</td></tr>
<tr><td><code>TG-036</code></td><td>A technology MAY re-enter as a Candidate after removal. Re-entry MUST be evaluated afresh and MUST NOT inherit its former tier.</td></tr>
</tbody>
</table>

### 10.3 Relationship to Support Tiers

<table>
<thead>
<tr><th align="left">Lifecycle state</th><th align="left">Compatible support tiers</th></tr>
</thead>
<tbody>
<tr><td>Candidate</td><td>Experimental</td></tr>
<tr><td>Supported</td><td>Officially Supported · Conditionally Supported</td></tr>
<tr><td>Preferred</td><td>Officially Supported (with the Preferred marker)</td></tr>
<tr><td>Deprecated</td><td>Conditionally Supported (condition: scheduled for removal)</td></tr>
<tr><td>Removed</td><td>Not Recommended, or absent from the document entirely</td></tr>
</tbody>
</table>

</section>

---

<section>

## 11. Technology Evaluation Criteria

Evaluation has two parts. Gating criteria are pass or fail. Weighted criteria distinguish between
candidates that have already passed the gates.

### 11.1 Gating Criteria

Every criterion below MUST hold before a technology may be assigned any tier above Experimental.
Failing one is disqualifying regardless of how strongly the others are satisfied.

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Gate</th><th align="left">Why it is absolute</th></tr>
</thead>
<tbody>
<tr><td><code>TG-040</code></td><td><strong>Replaceable.</strong> No AEOS capability may depend on the technology exclusively; an alternative or an honest degradation path MUST exist.</td><td>Independence is structural or it is marketing. A capability with one possible provider is a dependency acquired by accident.</td></tr>
<tr><td><code>TG-041</code></td><td><strong>Cross-platform.</strong> Available and functionally equivalent on Windows, macOS, and Linux — or restricted to Conditionally Supported with the platform limit stated.</td><td>Platform equality is a release gate in AEOS, not a preference.</td></tr>
<tr><td><code>TG-042</code></td><td><strong>Text-based artifacts.</strong> Its configuration and outputs MUST be human-readable, diffable, and consumable by an AI runtime without a separate machine-only format.</td><td>Two audiences from one artifact. A format only one of them can read has failed half its readers.</td></tr>
<tr><td><code>TG-043</code></td><td><strong>Locally operable.</strong> Usable without transmitting project content to a third party — except where the technology <em>is</em> an external AI provider or inference service, which is governed by the product's approval gates and disclosure obligations.</td><td>What leaves the machine is disclosed and approved beforehand, without exception.</td></tr>
<tr><td><code>TG-044</code></td><td><strong>Actively maintained.</strong> A public release history and evidence of ongoing maintenance.</td><td>An unmaintained dependency is a future incident with a known cause.</td></tr>
<tr><td><code>TG-045</code></td><td><strong>Usable license.</strong> The license MUST permit use by individuals, organisations, and educators without per-seat negotiation for the technology itself. Paid access to an external <em>service</em> is a separate matter and does not fail this gate.</td><td>Education Friendly and Community Support both fail if a technology cannot be picked up by someone without a purchasing department.</td></tr>
<tr><td><code>TG-046</code></td><td><strong>No secret exposure.</strong> The technology MUST NOT require credentials or secrets to be written into prompts, logs, reports, documentation, or Repository Assets.</td><td>Absolute across the entire product, without exception or negotiation.</td></tr>
</tbody>
</table>

### 11.2 Weighted Criteria

Applied after the gates, to choose between candidates and to assign a tier.

<table>
<thead>
<tr><th align="left">Criterion</th><th align="left">What is assessed</th><th align="left">Evidence that satisfies it</th></tr>
</thead>
<tbody>
<tr><td><strong>Community</strong></td><td>Plural maintainership and independent participation.</td><td>Multiple active maintainers, a public contribution process a stranger has used, and independent forums and tooling.</td></tr>
<tr><td><strong>Documentation</strong></td><td>Whether a competent stranger can succeed without the vendor's help.</td><td>Complete reference documentation, worked examples, and substantial material not written by the maintainers.</td></tr>
<tr><td><strong>Longevity</strong></td><td>Evidence it will still be here in five years.</td><td>Years of stable releases, a stated compatibility policy, and a record of handling its own breaking changes responsibly.</td></tr>
<tr><td><strong>Security</strong></td><td>How the technology handles its own vulnerabilities.</td><td>A published disclosure process, timely advisories, and a maintained supported-version policy.</td></tr>
<tr><td><strong>License</strong></td><td>Terms beyond the gating minimum.</td><td>An OSI-approved license, stable licensing history, and no history of relicensing against existing users.</td></tr>
<tr><td><strong>Maintainability</strong></td><td>What it costs to keep, not to start.</td><td>Predictable behaviour, comprehensible failure modes, and a small operational surface.</td></tr>
<tr><td><strong>Cross-platform support</strong></td><td>Quality of parity beyond mere availability.</td><td>Identical behaviour, path handling, and results across all three platforms — not merely successful installation.</td></tr>
<tr><td><strong>Offline capability</strong></td><td>Behaviour without a network.</td><td>Installable once and reusable; local operations require no network round trip.</td></tr>
<tr><td><strong>AI compatibility</strong></td><td>How well an AI runtime can work with it.</td><td>Text-based configuration, structured output, stable interfaces, and documentation an AI runtime can consume directly.</td></tr>
<tr><td><strong>Educational value</strong></td><td>Whether a learner can use it and learn from it.</td><td>Free to learn, runs on ordinary hardware, observable enough that a learner can see what happened rather than only that it worked.</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Governance statement</th></tr>
</thead>
<tbody>
<tr><td><code>TG-050</code></td><td>Every evaluation MUST record its outcome against both the gating and the weighted criteria, including for declined candidates.</td></tr>
<tr><td><code>TG-051</code></td><td>Weighted criteria are not scored numerically. A written judgement that a future reader can disagree with is required; a total that hides the reasoning is not.</td></tr>
<tr><td><code>TG-052</code></td><td>Where a candidate would displace an incumbent, the evaluation MUST state the migration cost imposed on existing projects and MUST weigh it as a cost of adoption.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 12. Versioning Policy

### 12.1 Versions of Supported Technologies

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Policy</th></tr>
</thead>
<tbody>
<tr><td><code>TG-060</code></td><td>This document states <strong>minimum supported versions and supported version ranges</strong>. It MUST NOT state exact pins; pinning is an implementation concern realised in lockfiles and manifests.</td></tr>
<tr><td><code>TG-061</code></td><td>Where a technology offers a long-term-support or stable channel, that channel is Preferred. Where it does not, the most recent release with a stable-release history is Preferred.</td></tr>
<tr><td><code>TG-062</code></td><td>AEOS SHOULD support every version of a technology still receiving upstream security support, and MUST NOT claim support for a version that has reached upstream end-of-life.</td></tr>
<tr><td><code>TG-063</code></td><td>A version that reaches upstream end-of-life becomes Deprecated at the next AEOS minor release without requiring a separate proposal.</td></tr>
<tr><td><code>TG-064</code></td><td>A new major version of a supported technology enters as a <strong>Candidate</strong> and MUST be evaluated before the supported range is extended. A major version is never inherited automatically.</td></tr>
<tr><td><code>TG-065</code></td><td>Where a technology's majors are not compatible with one another, this document MUST state which majors are supported and MUST NOT imply that any major is interchangeable with another.</td></tr>
</tbody>
</table>

### 12.2 Breaking Changes and Compatibility

A change to this document is **breaking** when it invalidates a technology choice a project has
already made in reliance on it.

<table>
<thead>
<tr><th align="left">Change</th><th align="left">Breaking?</th><th align="left">Required handling</th><th align="left">Document version impact</th></tr>
</thead>
<tbody>
<tr><td>Adding a technology at any tier</td><td>No</td><td>Owner approval and recorded rationale.</td><td>Minor</td></tr>
<tr><td>Promoting a technology to a higher tier</td><td>No</td><td>Owner approval and recorded evaluation.</td><td>Minor</td></tr>
<tr><td>Extending a supported version range</td><td>No</td><td>Owner approval and recorded evaluation.</td><td>Minor</td></tr>
<tr><td>Demoting a technology to a lower tier</td><td>Yes</td><td>Stated reason, and Deprecation before any removal.</td><td>Major</td></tr>
<tr><td>Narrowing a supported version range</td><td>Yes</td><td>Deprecation notice and a migration path.</td><td>Major</td></tr>
<tr><td>Removing a technology</td><td>Yes</td><td>One AEOS minor release of Deprecation, a migration path, and preserved rationale.</td><td>Major</td></tr>
<tr><td>Editorial correction with no change of meaning</td><td>No</td><td>Contributor change, owner acceptance.</td><td>Patch</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Policy</th></tr>
</thead>
<tbody>
<tr><td><code>TG-070</code></td><td>Existing projects are commitments already made. A breaking change MUST state the benefit that exceeds the cost imposed on people who are not present to object, and MUST provide a migration path.</td></tr>
<tr><td><code>TG-071</code></td><td>A security or licensing emergency MAY compress the deprecation period. The compression and its reason MUST be recorded; the requirement to record is never compressed.</td></tr>
<tr><td><code>TG-072</code></td><td>This document's own version follows the AEOS document change control in <a href="#15-document-governance">Section 15</a>, which is consistent with the change control defined by AEOS-PRD and AEOS-DOCSTD.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 13. Source of Truth Rules

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td><code>TG-080</code></td><td><strong>Technology decisions originate here.</strong> A technology becomes supported by being added to this document, and by no other means.</td></tr>
<tr><td><code>TG-081</code></td><td><strong>Architecture references this document.</strong> Architecture documents MUST cite categories and entries rather than restating, extending, or narrowing them.</td></tr>
<tr><td><code>TG-082</code></td><td><strong>Templates reference this document.</strong> Template assets MUST NOT introduce a technology absent from this document.</td></tr>
<tr><td><code>TG-083</code></td><td><strong>Implementation follows this document.</strong> Manifests, lockfiles, and pins realise the set stated here. They MUST NOT extend it.</td></tr>
<tr><td><code>TG-084</code></td><td><strong>Specifications reference this document.</strong> A specification MUST NOT name a technology as a requirement unless it is Officially Supported or Preferred here.</td></tr>
<tr><td><code>TG-085</code></td><td><strong>A conflict is a defect in the downstream document.</strong> Where a downstream document names a technology this document does not, the downstream document is corrected or the technology is proposed as a Candidate. It is never resolved by reinterpreting this document.</td></tr>
<tr><td><code>TG-086</code></td><td><strong>Absence is not prohibition.</strong> A technology absent from this document is unsupported. AEOS MUST NOT refuse to operate on a project because of it.</td></tr>
<tr><td><code>TG-087</code></td><td><strong>Product documents outrank this one.</strong> Where a technology decision would conflict with AEOS-PRD or an AEOS-VISION invariant, the technology decision is withdrawn.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 14. Future Evolution

**Technology may evolve. The product philosophy does not.**

Every entry in this document is expected to change. Providers will consolidate and disappear. Model
families will be superseded by their own successors. Package managers will be replaced by faster
ones with better lockfiles. Editors will merge. Standards will be extended and eventually
superseded. None of that is an engineering event for a project governed by AEOS, and the measure of
this document's success is how little disturbance each of those changes causes.

What does not change is the reasoning by which entries are chosen: no vendor privileged, no platform
second-class, no runtime required, open standards preferred, offline work protected, commercial and
open-source AI treated as the same kind of thing, and the whole set kept as small as honesty allows.

<blockquote>
A proposal that adds, promotes, demotes, or removes a technology is ordinary governance and is
welcome. A proposal that requires an AEOS-VISION invariant to bend in order to accommodate a
technology is not a technology proposal, and is refused at this document's boundary rather than
escalated for a decision it is not entitled to request.
</blockquote>

The direction of travel for the set as a whole is toward fewer systems doing more, not more systems
doing less: vector search absorbed into supported databases rather than added beside them,
build tooling native to the language rather than layered above it, observability instrumented
through open standards rather than through a vendor's agent. Each of those reduces what a maintainer
must understand five years from now, which is the only measure this document ultimately answers to.

</section>

---

<section>

## 15. Document Governance

### Status

This document is the **Technology Source of Truth** for the AEOS repository. Every architecture,
blueprint, specification, and template document references it instead of defining supported
technologies of its own.

### Change Control

<table>
<thead>
<tr><th align="left">Change type</th><th align="left">Requires</th><th align="left">Version impact</th></tr>
</thead>
<tbody>
<tr><td>Editorial correction with no change of meaning</td><td>Contributor change, owner acceptance</td><td>Patch</td></tr>
<tr><td>Adding a technology, or promoting one to a higher tier</td><td>Owner approval with a recorded evaluation</td><td>Minor</td></tr>
<tr><td>Extending a supported version range</td><td>Owner approval with a recorded evaluation</td><td>Minor</td></tr>
<tr><td>Adding a technology category</td><td>Explicit owner revision request</td><td>Minor</td></tr>
<tr><td>Demoting, deprecating, or removing a technology</td><td>Explicit owner approval with recorded rationale and a migration path</td><td>Major</td></tr>
<tr><td>Changing a technology principle, support tier definition, or lifecycle state</td><td>Explicit owner revision request</td><td>Major</td></tr>
</tbody>
</table>

### Identifier Policy

`TC-` category identifiers and `TG-` governance statement identifiers are permanent. They are never
reused, never renumbered, and never repurposed. A retired statement is marked retired in place,
retaining its identifier and its rationale.

### Relationship to the Architecture Freeze

This document introduces no architecture and grants no capability. Technology proposals arising from
architecture work are recorded as Candidates under [Section 10](#10-technology-lifecycle) and applied
only after explicit owner approval.

### Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical or
Major findings remain.

### Traceability

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Obligation</th></tr>
</thead>
<tbody>
<tr><td>Architecture documents</td><td>Every technology named traces to a <code>TC-</code> category and an entry in <a href="#8-officially-supported-technologies-by-category">Section 8</a>.</td></tr>
<tr><td>Specifications</td><td>Every required technology is Officially Supported or Preferred here.</td></tr>
<tr><td>Templates</td><td>Every technology used traces to an entry here.</td></tr>
<tr><td>Implementation</td><td>Manifests and lockfiles realise entries here and introduce none.</td></tr>
<tr><td>Issues and pull requests</td><td>Reference the <code>TC-</code> or <code>TG-</code> identifiers they affect.</td></tr>
</tbody>
</table>

### Revision History

<table>
<thead>
<tr><th align="left">Version</th><th align="left">Status</th><th align="left">Summary</th></tr>
</thead>
<tbody>
<tr><td>1.0.0</td><td>Freeze candidate</td><td>Initial technology governance definition. Establishes three technology scopes, thirteen technology principles, five support tiers, twenty technology categories, the initial official technology set with rationale, five lifecycle states, gating and weighted evaluation criteria, the versioning policy, and the source of truth rules. Introduces no architecture, no requirement, and no capability.</td></tr>
</tbody>
</table>

</section>

---

## Appendix A — Technology Matrix

Summary view. [Section 8](#8-officially-supported-technologies-by-category) is authoritative; where
this table and Section 8 differ, Section 8 governs. Preferred entries are a subset of Officially
Supported and are not repeated in the Supported column. Order within a cell is alphabetical.

<table>
<thead>
<tr><th align="left">Category</th><th align="left">Preferred</th><th align="left">Supported</th><th align="left">Conditional</th><th align="left">Experimental</th></tr>
</thead>
<tbody>
<tr>
<td><code>TC-01</code> Operating Systems</td>
<td><em>None by policy</em></td>
<td>Linux · macOS · Windows</td>
<td>Rolling-release Linux · WSL2</td>
<td>BSD family</td>
</tr>
<tr>
<td><code>TC-02</code> Development Environments</td>
<td><em>None by policy</em></td>
<td>Cursor · Terminal · VS Code · Windsurf</td>
<td>Emacs · GitHub Copilot · JetBrains · Neovim · Zed</td>
<td>Browser-hosted and remote environments</td>
</tr>
<tr>
<td><code>TC-03</code> Programming Languages</td>
<td>Python · TypeScript</td>
<td><em>Preferred set only</em></td>
<td>C# · Go · Java · JavaScript · Rust</td>
<td>Other languages with a mainstream test runner</td>
</tr>
<tr>
<td><code>TC-04</code> Frameworks</td>
<td><em>None by policy</em></td>
<td>FastAPI · React · React Native</td>
<td>Django · Express · Flask · Next.js · Svelte · Vue</td>
<td>Frameworks under two years of stable release history</td>
</tr>
<tr>
<td><code>TC-05</code> Databases</td>
<td>PostgreSQL · SQLite</td>
<td><em>Preferred set only</em></td>
<td>DuckDB · MariaDB · MySQL</td>
<td>Wire-compatible distributed and embedded alternatives</td>
</tr>
<tr>
<td><code>TC-06</code> ORM and Data Access</td>
<td><em>None</em></td>
<td>Alembic · SQLAlchemy</td>
<td>Drizzle ORM · Prisma · SQLModel</td>
<td>Query builders and lightweight mappers</td>
</tr>
<tr>
<td><code>TC-07</code> Package Managers</td>
<td>pnpm · uv</td>
<td><em>Preferred set only</em></td>
<td>npm · pip with virtual environments · Poetry · Yarn</td>
<td>Emerging resolvers</td>
</tr>
<tr>
<td><code>TC-08</code> Containers</td>
<td><em>None by policy</em></td>
<td>Docker · Docker Compose</td>
<td>containerd · Podman</td>
<td>Development-container specifications</td>
</tr>
<tr>
<td><code>TC-09</code> AI Providers</td>
<td><em>None by policy — permanently</em></td>
<td>Anthropic · Google · OpenAI</td>
<td>Compatible-API providers · Managed enterprise endpoints</td>
<td>New commercial providers meeting the gates</td>
</tr>
<tr>
<td><code>TC-10</code> Open-source Models</td>
<td><em>None by policy</em></td>
<td>Gemma · Llama · Mistral · Qwen</td>
<td>Other openly licensed families</td>
<td>New families · Specialised code models</td>
</tr>
<tr>
<td><code>TC-11</code> Inference Runtimes</td>
<td><em>None by policy</em></td>
<td>LM Studio · Ollama · Transformers</td>
<td>llama.cpp · MLX · vLLM</td>
<td>Compatible-interface serving stacks</td>
</tr>
<tr>
<td><code>TC-12</code> Vector Databases</td>
<td>pgvector · sqlite-vec</td>
<td><em>Preferred set only</em></td>
<td>Chroma · Qdrant</td>
<td>Other locally operable vector stores</td>
</tr>
<tr>
<td><code>TC-13</code> MCP and Interoperability</td>
<td>MCP</td>
<td>OpenAPI-described interfaces</td>
<td>LSP · Vendor extensions to a supported standard</td>
<td>Emerging agent and tool specifications</td>
</tr>
<tr>
<td><code>TC-14</code> Build Systems</td>
<td>Language-native build tooling</td>
<td>Vite</td>
<td>just · Make</td>
<td>Monorepo orchestration systems</td>
</tr>
<tr>
<td><code>TC-15</code> Testing Frameworks</td>
<td>Playwright · pytest · Vitest</td>
<td>Jest</td>
<td>Cypress · Testing Library · unittest</td>
<td>Emerging runners · Property-based testing</td>
</tr>
<tr>
<td><code>TC-16</code> Documentation</td>
<td>GitHub-Flavored Markdown with semantic HTML</td>
<td>Mermaid</td>
<td>Docusaurus · MkDocs · Sphinx</td>
<td>Structured formats with stronger machine semantics</td>
</tr>
<tr>
<td><code>TC-17</code> CI/CD</td>
<td><em>None by policy</em></td>
<td>GitHub Actions</td>
<td>Azure Pipelines · CircleCI · GitLab CI · Jenkins</td>
<td>Local pipeline emulation</td>
</tr>
<tr>
<td><code>TC-18</code> Version Control</td>
<td>Git</td>
<td>GitHub (hosting)</td>
<td>Azure Repos · Bitbucket · Gitea · GitLab</td>
<td>Git-compatible front-ends</td>
</tr>
<tr>
<td><code>TC-19</code> Monitoring</td>
<td>Structured local logging</td>
<td>OpenTelemetry</td>
<td>Grafana · Prometheus · Sentry</td>
<td>Local-first observability backends</td>
</tr>
<tr>
<td><code>TC-20</code> Security</td>
<td>Platform-native credential stores</td>
<td>Package manager dependency auditing · gitleaks · Trivy</td>
<td>CycloneDX / SPDX SBOM · Dependabot · Renovate · Sigstore</td>
<td>Commercial SAST and supply-chain platforms</td>
</tr>
</tbody>
</table>

---

## Appendix B — Technology Selection Decision Flow

The process by which a technology is proposed, evaluated, and admitted. Each step produces a
recorded outcome, including when the answer is no.

```text
        +--------------------------------------+
        | 1. NEED IDENTIFIED                   |
        |    A capability or project requires  |
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
        | 3. GATING CRITERIA  (TG-040..TG-046) +--------->| NOT RECOMMENDED.          |
        |    Pass or fail. No exceptions.      |          | Record which gate failed. |
        +-------------------+------------------+          +---------------------------+
                            | passes
                            v
        +--------------------------------------+
        | 4. WEIGHTED EVALUATION  (Sec. 11.2)  |
        |    Written judgement, not a score.   |
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
        |    Did it hold up in real use?       |             | preserved in place.     |
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
                 owner approval  (TG-032)
                            |
                            v
        +--------------------------------------+
        | 9. PREFERRED                         |
        |    Default for new AEOS work.        |
        +--------------------------------------+
```

### Notes on the Flow

<table>
<thead>
<tr><th align="left">Step</th><th align="left">Note</th></tr>
</thead>
<tbody>
<tr><td>2</td><td>The most valuable step and the most often skipped. Most technology proposals are answered by a technology already in the set, and answering them that way is a success rather than a rejection.</td></tr>
<tr><td>3</td><td>Gates are not weighed against one another. A candidate that fails one gate and excels at everything else still fails.</td></tr>
<tr><td>4</td><td>Migration cost imposed on existing projects is counted as a cost of adoption, not as a cost borne by whoever raises it later.</td></tr>
<tr><td>5</td><td>The owner decides. A contributor — human or AI — explains, proposes, and waits, exactly as AEOS itself does before any consequential action.</td></tr>
<tr><td>7</td><td>Withdrawal at this step is a normal outcome and is never treated as a failure of the proposal or the proposer.</td></tr>
<tr><td>9</td><td>Promotion to Preferred is never automatic and never a formality. Most Supported technologies will remain Supported permanently, which is the intended outcome.</td></tr>
</tbody>
</table>

---

## Appendix C — Governance Statement Index

<table>
<thead>
<tr><th align="left">Range</th><th align="left">Subject</th><th align="left">Count</th><th align="left">Section</th></tr>
</thead>
<tbody>
<tr><td><code>TG-001</code>–<code>TG-005</code></td><td>Technology scopes</td><td>5</td><td><a href="#4-scope-of-technology-support">4</a></td></tr>
<tr><td><code>TG-010</code>–<code>TG-012</code></td><td>Technology principles</td><td>3</td><td><a href="#5-technology-principles">5</a></td></tr>
<tr><td><code>TG-020</code>–<code>TG-024</code></td><td>Support tiers</td><td>5</td><td><a href="#6-support-tiers">6</a></td></tr>
<tr><td><code>TG-030</code>–<code>TG-036</code></td><td>Technology lifecycle</td><td>7</td><td><a href="#10-technology-lifecycle">10</a></td></tr>
<tr><td><code>TG-040</code>–<code>TG-046</code></td><td>Gating criteria</td><td>7</td><td><a href="#111-gating-criteria">11.1</a></td></tr>
<tr><td><code>TG-050</code>–<code>TG-052</code></td><td>Evaluation conduct</td><td>3</td><td><a href="#112-weighted-criteria">11.2</a></td></tr>
<tr><td><code>TG-060</code>–<code>TG-065</code></td><td>Versioning of supported technologies</td><td>6</td><td><a href="#121-versions-of-supported-technologies">12.1</a></td></tr>
<tr><td><code>TG-070</code>–<code>TG-072</code></td><td>Breaking changes and compatibility</td><td>3</td><td><a href="#122-breaking-changes-and-compatibility">12.2</a></td></tr>
<tr><td><code>TG-080</code>–<code>TG-087</code></td><td>Source of truth rules</td><td>8</td><td><a href="#13-source-of-truth-rules">13</a></td></tr>
<tr><td colspan="2"><strong>Total</strong></td><td><strong>47</strong></td><td>—</td></tr>
</tbody>
</table>

<table>
<thead>
<tr><th align="left">Range</th><th align="left">Subject</th><th align="left">Count</th><th align="left">Section</th></tr>
</thead>
<tbody>
<tr><td><code>TC-01</code>–<code>TC-20</code></td><td>Technology categories</td><td>20</td><td><a href="#7-technology-categories">7</a>, <a href="#8-officially-supported-technologies-by-category">8</a></td></tr>
</tbody>
</table>

---

<div align="center">

**End of Supported Technologies Document**

AEOS-TECH · Version 1.0.0 · Technology Source of Truth

</div>
