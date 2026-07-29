<div align="center">

# AI Engineering Operating System

**AEOS — Document Standard**

*The permanent statement of how AEOS documentation is written, structured, reviewed, and frozen.*

</div>

<table>
<thead>
<tr><th align="left">Field</th><th align="left">Value</th></tr>
</thead>
<tbody>
<tr><td><strong>Document</strong></td><td>Document Standard</td></tr>
<tr><td><strong>Product</strong></td><td>AI Engineering Operating System (AEOS)</td></tr>
<tr><td><strong>Document ID</strong></td><td>AEOS-DOCSTD</td></tr>
<tr><td><strong>Version</strong></td><td>1.0.0</td></tr>
<tr><td><strong>Status</strong></td><td>Freeze candidate — awaiting owner approval</td></tr>
<tr><td><strong>Owner</strong></td><td>Product Owner, AEOS</td></tr>
<tr><td><strong>Author</strong></td><td>Documentation Architect and Documentation Standards Authority, AEOS</td></tr>
<tr><td><strong>Audience</strong></td><td>AI systems, contributors, maintainers, reviewers, and documentation authors</td></tr>
<tr><td><strong>Suggested path</strong></td><td><code>docs/product/DOCUMENT_STANDARD.md</code></td></tr>
<tr><td><strong>Companion documents</strong></td><td><code>AEOS_VISION.md</code> (AEOS-VISION) · <code>AEOS_PRODUCT_REQUIREMENTS.md</code> (AEOS-PRD) · <code>AEOS_GLOSSARY.md</code> (AEOS-GLOSSARY)</td></tr>
<tr><td><strong>Supersedes</strong></td><td>None</td></tr>
</tbody>
</table>

> **Authority of this document.**
> This document defines *how* AEOS documentation is written. It is normative for the **form,
> structure, language, ownership, and lifecycle** of every AEOS document.
> It defines no product requirement, no terminology, no architecture, no runtime behavior, and no
> implementation. Where this document and a document of higher authority both speak to a subject,
> the higher-authority document governs and any conflict here is a defect to be reported.

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Applicability](#2-scope-and-applicability)
3. [Documentation Principles](#3-documentation-principles)
4. [Documentation Hierarchy](#4-documentation-hierarchy)
5. [Document Responsibilities](#5-document-responsibilities)
6. [Writing Style](#6-writing-style)
7. [Normative Language](#7-normative-language)
8. [HTML-in-Markdown Standard](#8-html-in-markdown-standard)
9. [AI Readability](#9-ai-readability)
10. [Human Readability](#10-human-readability)
11. [Source of Truth Rules](#11-source-of-truth-rules)
12. [Review and Freeze](#12-review-and-freeze)
13. [Future Evolution](#13-future-evolution)
14. [Document Governance](#14-document-governance)
15. [Appendix A — Recommended Document Template](#appendix-a--recommended-document-template)
16. [Appendix B — Recommended Section Ordering](#appendix-b--recommended-section-ordering)

---

<section>

## 1. Executive Summary

AEOS states that the repository is the product. A repository is the product only if what it contains
can be read, trusted, and acted upon by whoever arrives next — a contributor who was not present for
the reasoning, a maintainer years later, or an AI runtime that has no memory of the conversation in
which a decision was made. Documentation is the medium through which that happens. It is therefore
not a description of the engineering work. It is part of it.

Consistency is what makes documentation usable at that scale. A reader who must re-learn how a
project expresses itself in every new document cannot read quickly, cannot compare reliably, and
cannot tell a deliberate difference from an accidental one. For an AI runtime the cost is sharper:
an unstated convention is not a convention, and inconsistent structure becomes an invitation to
guess. Inconsistency does not merely look untidy — it silently reduces the accuracy of every
participant that depends on the repository.

Four properties make documentation consistency a first-class engineering concern in AEOS.

<table>
<thead>
<tr><th align="left">Property</th><th align="left">Why it is engineering, not presentation</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Documentation is an interface</strong></td>
<td>Humans and AI runtimes both act on it. An interface with unpredictable shape is a defective interface, regardless of how well written each instance is.</td>
</tr>
<tr>
<td><strong>Documentation carries authority</strong></td>
<td>AEOS documents govern what may be built. If a reader cannot tell which document governs a subject, authority is ambiguous and decisions are made without a basis.</td>
</tr>
<tr>
<td><strong>Documentation is traced against</strong></td>
<td>Downstream documents, tests, issues, and releases reference identifiers written here and elsewhere. Traceability survives only when identifiers and structure are stable by rule rather than by habit.</td>
</tr>
<tr>
<td><strong>Documentation outlives its authors</strong></td>
<td>The correct time horizon is the life of the product. A convention that exists only in the current contributors' practice disappears with them; a convention written down does not.</td>
</tr>
</tbody>
</table>

This document exists so that none of those properties depends on who happened to write a given
document. It defines the hierarchy that assigns authority, the responsibility boundaries that keep
documents from overlapping, the language rules that make statements unambiguous, the format rules
that keep documents readable by both audiences from one artifact, and the lifecycle that determines
when a document becomes binding.

It defines nothing else. A documentation standard that also decided product behavior, terminology,
or architecture would be committing the exact error it exists to prevent.

</section>

---

<section>

## 2. Scope and Applicability

### 2.1 What This Document Governs

This Standard governs the form of AEOS documentation:

- the hierarchy of documents and the authority each layer carries;
- the responsibility boundary of each document;
- the language, tone, and normative vocabulary used in documents;
- the permitted document format, including HTML used inside Markdown;
- the conventions that make documents readable by humans and consumable by AI runtimes;
- the rules by which documents reference one another;
- the lifecycle from draft to freeze, and what a freeze means.

### 2.2 What This Document Does Not Govern

<table>
<thead>
<tr><th align="left">Not governed here</th><th align="left">Owned by</th></tr>
</thead>
<tbody>
<tr><td>Why AEOS exists, and what must never change about it</td><td>AEOS-VISION</td></tr>
<tr><td>What AEOS is and what it must do</td><td>AEOS-PRD</td></tr>
<tr><td>What AEOS terms mean</td><td>AEOS-GLOSSARY</td></tr>
<tr><td>How AEOS is structured</td><td>Architecture documents</td></tr>
<tr><td>How defined behavior works precisely</td><td>Specification documents</td></tr>
<tr><td>How AEOS executes and in what environment</td><td>Runtime documents</td></tr>
<tr><td>What code realizes the product</td><td>The codebase and its tests</td></tr>
</tbody>
</table>

A statement in this document that grants a capability, imposes a product requirement, defines a
term, or implies a structure is a **defect in this document**. It MUST be reported rather than acted
upon.

### 2.3 Applicability in Time

This Standard applies to every AEOS document created or revised after this Standard is frozen.

Documents frozen before this Standard was frozen — including AEOS-VISION, AEOS-PRD, and
AEOS-GLOSSARY — remain valid and authoritative exactly as written. This Standard does not
retroactively invalidate them, does not require their reformatting, and does not constitute a
revision request against them. Where such a document diverges from this Standard, the divergence is
recorded as a Minor finding and is reconciled only through that document's own change control, at
the owner's discretion.

### 2.4 Applicability to Authors

This Standard applies identically to human and AI authors. Where a rule is stated for "the author",
it binds whichever kind of participant produced the text. An AI runtime generating an AEOS document
MUST comply with this Standard; a contributor accepting AI-generated documentation into the
repository is responsible for that compliance.

</section>

---

<section>

## 3. Documentation Principles

These principles constrain every AEOS document. A document that violates one is defective, not
merely unconventional. Each principle carries a stable identifier so that reviews, issues, and
future documents can reference it precisely.

<table>
<thead>
<tr><th align="left">ID</th><th align="left">Principle</th></tr>
</thead>
<tbody>
<tr><td><code>DS-P-01</code></td><td>Documentation is a Repository Asset.</td></tr>
<tr><td><code>DS-P-02</code></td><td>Documentation is version-controlled and reviewed like code.</td></tr>
<tr><td><code>DS-P-03</code></td><td>Documentation is engineering, not decoration.</td></tr>
<tr><td><code>DS-P-04</code></td><td>Documentation is reviewed before freeze.</td></tr>
<tr><td><code>DS-P-05</code></td><td>Documentation remains implementation-independent unless its layer requires otherwise.</td></tr>
<tr><td><code>DS-P-06</code></td><td>One document owns one responsibility.</td></tr>
<tr><td><code>DS-P-07</code></td><td>Definitions are never duplicated.</td></tr>
<tr><td><code>DS-P-08</code></td><td>Sources of truth never conflict.</td></tr>
<tr><td><code>DS-P-09</code></td><td>One artifact serves two audiences.</td></tr>
<tr><td><code>DS-P-10</code></td><td>An incomplete document is not published.</td></tr>
<tr><td><code>DS-P-11</code></td><td>Every document states its own authority.</td></tr>
<tr><td><code>DS-P-12</code></td><td>Documentation is written to age well.</td></tr>
</tbody>
</table>

<details>
<summary><strong>DS-P-01 — Documentation is a Repository Asset</strong></summary>

<br>

Documentation lives in the repository, beside the material it describes. It is durable, versioned,
inspectable, consumable, portable, and extensible in the same sense as every other Repository Asset.

Documentation that exists only in a chat session, an issue thread, a wiki outside the repository, a
vendor account, or an individual's notes is not AEOS documentation. It MAY be a useful working
artifact; it carries no authority and MUST NOT be referenced as though it did.

</details>

<details>
<summary><strong>DS-P-02 — Documentation is version-controlled and reviewed like code</strong></summary>

<br>

Every documentation change enters the repository through the project's ordinary history and review.
Documents MUST be diffable: authors write in a form where a meaningful change produces a meaningful
diff.

Practically, this means one sentence per line is permitted but not required; what is required is
that reflowing, reformatting, and renumbering are avoided in changes whose purpose is substantive,
so that reviewers can see what actually changed. Formatting-only changes SHOULD be submitted
separately from substantive ones.

</details>

<details>
<summary><strong>DS-P-03 — Documentation is engineering, not decoration</strong></summary>

<br>

A document is an engineering artifact subject to the same standard of truth as code: it states what
is so, distinguishes what is decided from what is proposed, and is wrong when it does not match
reality.

Consequences: documentation is not written to persuade, not written to impress, and not written to
fill a section that a template implied should exist. Structure serves comprehension. A section with
nothing to say is removed, not padded.

</details>

<details>
<summary><strong>DS-P-04 — Documentation is reviewed before freeze</strong></summary>

<br>

No document becomes authoritative without review. Review classifies findings, identifies
inconsistencies, and does not redesign the document under review.

A document MUST NOT be frozen while any Critical or Major finding remains open. See
[Section 12](#12-review-and-freeze).

</details>

<details>
<summary><strong>DS-P-05 — Documentation remains implementation-independent unless its layer requires otherwise</strong></summary>

<br>

Each layer of the hierarchy has a permitted depth. A document MUST NOT reach below its own layer.

Higher-layer documents (Vision, Product Requirements, Glossary, this Standard) state outcomes,
intent, meaning, and form — never mechanism. Lower-layer documents (Specification, Implementation
Guides, Developer Guides) are expected to be concrete, and their concreteness is not a violation.

The test for a higher-layer document: if a statement can be satisfied in only one way, it is too
specific for its layer.

</details>

<details>
<summary><strong>DS-P-06 — One document owns one responsibility</strong></summary>

<br>

Every document answers exactly one question about the product. A document that answers two questions
is two documents that have not yet been separated.

When a document is tempted to explain something outside its responsibility, it references the owning
document instead. Brief orienting context — a sentence establishing why the referenced concept
matters here — is permitted; restating the concept is not.

</details>

<details>
<summary><strong>DS-P-07 — Definitions are never duplicated</strong></summary>

<br>

A term is defined in exactly one place: the Glossary. Other documents use the term and, where the
reader benefits, link to it. They MUST NOT restate the definition, extend it, narrow it, or offer an
alternative phrasing "for convenience".

Duplicated definitions do not stay identical. The second copy is not redundancy; it is a future
contradiction with a delayed fuse.

</details>

<details>
<summary><strong>DS-P-08 — Sources of truth never conflict</strong></summary>

<br>

At any moment, exactly one document is authoritative for any given subject. Two documents making
different statements about the same subject is a defect in at least one of them, and it is resolved
at the owning document — never by a local reinterpretation in the document that discovered it.

A contributor who finds such a conflict reports it. A contributor who silently resolves it has
created a third version of the truth.

</details>

<details>
<summary><strong>DS-P-09 — One artifact serves two audiences</strong></summary>

<br>

Every AEOS document is written to be intelligible to a human maintainer and consumable by an AI
runtime, from the same file. A separate machine-readable version of a document MUST NOT be created.

Two artifacts describing one subject will diverge, and the one that diverges will be the one nobody
is reading that week. Conventions serving each audience are stated in
[Section 9](#9-ai-readability) and [Section 10](#10-human-readability); they are complementary, and
where they appear to compete, the resolution is stated in [Section 10](#10-human-readability).

</details>

<details>
<summary><strong>DS-P-10 — An incomplete document is not published</strong></summary>

<br>

A published AEOS document MUST NOT contain placeholders, `TODO` markers, empty sections, "to be
determined" values, lorem text, or statements that depend on information the author did not have.

Where something is genuinely undecided, the document says so as a finished statement: what is
undecided, who decides it, and what the document does in the meantime. That is a complete sentence
about an incomplete decision, which is acceptable. An unfinished section is not.

</details>

<details>
<summary><strong>DS-P-11 — Every document states its own authority</strong></summary>

<br>

Every AEOS document opens with a metadata block and an authority statement declaring what it
governs, what it does not govern, and which document wins in a conflict.

A reader MUST be able to determine a document's authority without reading the document, and without
consulting a person.

</details>

<details>
<summary><strong>DS-P-12 — Documentation is written to age well</strong></summary>

<br>

Documents are written for readers who will arrive years later with no context. Wording is timeless:
no "currently", "recently", "new", "modern", "for now", no references to what is fashionable, and no
dates embedded in prose where a governance record would serve.

Where a statement is true only of a moment, it belongs in a revision history, a decision record, or
a lower layer that is expected to change — not in a document intended to be frozen.

</details>

</section>

---

<section>

## 4. Documentation Hierarchy

### 4.1 The Hierarchy

The hierarchy assigns **authority**. A document may constrain documents below it and MUST NOT
contradict documents above it.

```text
        AEOS-VISION            Why the product exists
              |
              v
        AEOS-PRD               What the product is and must do
              |
              v
        AEOS-GLOSSARY          What the terms mean
              |
              v
        AEOS-DOCSTD            How documentation is written
              |
              v
        TECHNOLOGY CATALOG     What technologies the project recognizes
              |
              v
        ARCHITECTURE           How the product is structured
              |
              v
        BLUEPRINT              How the structure is arranged to be built
              |
              v
        SPECIFICATION          How each behavior must work, precisely
              |
              v
        IMPLEMENTATION GUIDES  How the specified behavior is realized
              |
              v
        DEVELOPER GUIDES       How a person works within the result
```

The same relationships, stated without the diagram: Vision governs Product Requirements; Product
Requirements governs Glossary, Document Standard, Technology Catalog, Architecture, Blueprint,
Specification, Implementation Guides, and Developer Guides; each remaining layer governs every layer
below it in the order listed.

### 4.2 Two Kinds of Layer

Three layers are **foundational**: they do not derive behavior from the layer above and are not
derived from by the layer below. They serve every layer at once.

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Kind</th><th align="left">Relationship to the rest</th></tr>
</thead>
<tbody>
<tr><td>AEOS-VISION</td><td>Foundational</td><td>Constrains what may be proposed anywhere. Derives from nothing.</td></tr>
<tr><td>AEOS-PRD</td><td>Derivative and governing</td><td>Derives intent from the Vision; governs behavior for everything below.</td></tr>
<tr><td>AEOS-GLOSSARY</td><td>Foundational</td><td>Supplies meaning to every layer, including the layers above it in change-control weight.</td></tr>
<tr><td>AEOS-DOCSTD</td><td>Foundational</td><td>Supplies form to every layer. Carries no authority over content at any layer.</td></tr>
<tr><td>All remaining layers</td><td>Derivative</td><td>Each derives from the layer above and traces to <code>PR-</code> requirement identifiers.</td></tr>
</tbody>
</table>

> **On position in the diagram.**
> The Glossary and this Standard appear beneath the PRD because they carry the same
> change-control weight as the documents above them and are governed by the same formal process —
> not because terminology or documentation form is derived from product requirements. Their service
> is horizontal: every layer, above and below, depends on them.

### 4.3 Purpose of Each Layer

<details>
<summary><strong>Vision — AEOS-VISION</strong></summary>

<br>

**Answers:** Why does AEOS exist, and what must it always remain?

**Contains:** vision, mission, philosophy, design values, non-goals, contributor principles,
invariants.

**Stability:** highest. A change here is a change of purpose.

**Does not contain:** requirements, capabilities, terminology definitions, structure, mechanism.

</details>

<details>
<summary><strong>Product Requirements — AEOS-PRD</strong></summary>

<br>

**Answers:** What is AEOS, who is it for, and what must it do for them?

**Contains:** capabilities, numbered requirements with stable identifiers, scope, quality
attributes, success metrics, release phases.

**Stability:** high. Requirements are added, refined, and retired under change control; identifiers
are permanent.

**Does not contain:** reasoning that belongs to the Vision, term definitions that belong to the
Glossary, or any statement of structure or mechanism.

</details>

<details>
<summary><strong>Glossary — AEOS-GLOSSARY</strong></summary>

<br>

**Answers:** What does each AEOS term mean?

**Contains:** the single authoritative definition of every term used across AEOS documentation.

**Stability:** high. Terminology drift is a defect; a redefinition is a governance event.

**Does not contain:** requirements, rationale beyond what the definition needs, structure, or
behavior.

</details>

<details>
<summary><strong>Document Standard — AEOS-DOCSTD (this document)</strong></summary>

<br>

**Answers:** How is AEOS documentation written, structured, reviewed, and frozen?

**Contains:** documentation principles, hierarchy, responsibility boundaries, writing style,
normative vocabulary, format rules, readability conventions, source-of-truth rules, lifecycle.

**Stability:** high. It is the shape every other document takes.

**Does not contain:** anything about the product itself.

</details>

<details>
<summary><strong>Technology Catalog</strong></summary>

<br>

**Answers:** Which technologies, runtimes, and tooling does the project recognize, and what is the
recorded status of each?

**Contains:** an inventory with the recorded status of each entry — recognized, adopted, evaluated,
or declined — and the reasoning attached to that status.

**Stability:** moderate. The technology landscape changes; the catalog is expected to record that.

**Does not contain:** structure, behavior, or requirements. Presence in the catalog records
recognition only. Consistent with the product's independence commitments, inclusion confers no
privilege and absence implies no exclusion, and the catalog MUST NOT be written in a way that ranks
or endorses a vendor.

</details>

<details>
<summary><strong>Architecture</strong></summary>

<br>

**Answers:** How is AEOS structured so that it can deliver the product?

**Contains:** structural decisions, boundaries, responsibilities of parts, and the reasoning behind
them, each traced to `PR-` identifiers.

**Stability:** moderate. Architecture may evolve under its own governance without touching the
layers above it.

**Does not contain:** product requirements, terminology, precise behavioral rules, or code.

</details>

<details>
<summary><strong>Blueprint</strong></summary>

<br>

**Answers:** How is the architecture arranged so that it can actually be built?

**Contains:** the buildable arrangement of what the Architecture established — decomposition,
relationships, and boundaries at the level of detail a specification can be written against.

**Stability:** moderate.

**Does not contain:** the structural decisions themselves, which belong to Architecture, nor the
precise behavioral rules, which belong to Specification.

</details>

<details>
<summary><strong>Specification</strong></summary>

<br>

**Answers:** How must each defined behavior work, precisely and testably?

**Contains:** normative behavioral rules, inputs and outputs, states, error conditions, and
acceptance criteria, each traced to `PR-` identifiers.

**Stability:** moderate. Specifications evolve as behavior is refined.

**Does not contain:** rationale that belongs above it, or code.

</details>

<details>
<summary><strong>Implementation Guides</strong></summary>

<br>

**Answers:** How is the specified behavior realized?

**Contains:** concrete guidance for building to specification, including technology-specific detail.

**Stability:** low by design. This layer is expected to change most often.

**Does not contain:** anything that changes what must be built, which belongs to Specification or
above.

</details>

<details>
<summary><strong>Developer Guides</strong></summary>

<br>

**Answers:** How does a person work within the result?

**Contains:** task-oriented instruction for contributors and users — setup, workflow, conventions in
practice, troubleshooting.

**Stability:** low by design.

**Does not contain:** authority. A guide describes; it never decides. Where a guide and a governing
document disagree, the governing document is correct and the guide is defective.

</details>

### 4.4 Hierarchy Rules

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>H1</td><td>A document MUST NOT contradict any document above it in the hierarchy.</td></tr>
<tr><td>H2</td><td>A document MUST NOT weaken, reinterpret, or quietly widen a statement made above it.</td></tr>
<tr><td>H3</td><td>A document MUST NOT assume the responsibility of another document at any layer.</td></tr>
<tr><td>H4</td><td>Every derivative document MUST trace its content to the layer above, and ultimately to <code>PR-</code> requirement identifiers.</td></tr>
<tr><td>H5</td><td>Introducing a new document layer is a change to the hierarchy and requires the governance described in <a href="#14-document-governance">Section 14</a>.</td></tr>
<tr><td>H6</td><td>A document that belongs to no layer MUST NOT be published as AEOS documentation.</td></tr>
<tr><td>H7</td><td>The hierarchy describes authority, not reading order. A reader MAY enter at any layer; a document MUST make its position explicit so that entry point does not matter.</td></tr>
</tbody>
</table>

### 4.5 Unassigned Layer

> **On Runtime documents.**
> AEOS-PRD names a Runtime layer among the layers to which it defers. This Standard does not assign
> that layer a position in the documentation hierarchy, because assigning it is a hierarchy decision
> reserved to the owner under rule H5.
> Until the owner decides, a Runtime document is written to the responsibility boundary AEOS-PRD
> states for it, and complies with every rule in this Standard that does not depend on hierarchy
> position. Placing the layer in [Section 4.1](#41-the-hierarchy) is a Major change under
> [Section 14.2](#142-change-control).

</section>

---

<section>

## 5. Document Responsibilities

### 5.1 The Responsibility Table

<table>
<thead>
<tr><th align="left">Document</th><th align="left">Owns</th><th align="left">Answers</th><th align="left">MUST NOT contain</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Vision</strong></td>
<td>Purpose and enduring intent</td>
<td>WHY</td>
<td>Requirements, definitions, structure, rules, mechanism</td>
</tr>
<tr>
<td><strong>Product Requirements</strong></td>
<td>Product definition and obligations</td>
<td>WHAT</td>
<td>Rationale owned by Vision, definitions owned by Glossary, structure, mechanism</td>
</tr>
<tr>
<td><strong>Glossary</strong></td>
<td>Terminology</td>
<td>TERMS</td>
<td>Requirements, structure, rules, guidance</td>
</tr>
<tr>
<td><strong>Document Standard</strong></td>
<td>Documentation form and lifecycle</td>
<td>HOW DOCUMENTS ARE WRITTEN</td>
<td>Anything about the product itself</td>
</tr>
<tr>
<td><strong>Technology Catalog</strong></td>
<td>Recognized technologies and their status</td>
<td>WHAT IS RECOGNIZED</td>
<td>Structure, behavior, requirements, endorsement</td>
</tr>
<tr>
<td><strong>Architecture</strong></td>
<td>Structure and structural reasoning</td>
<td>STRUCTURE</td>
<td>Requirements, definitions, precise behavioral rules, code</td>
</tr>
<tr>
<td><strong>Blueprint</strong></td>
<td>Buildable arrangement of the structure</td>
<td>ARRANGEMENT</td>
<td>Structural decisions, behavioral rules, code</td>
</tr>
<tr>
<td><strong>Specification</strong></td>
<td>Precise, testable behavioral rules</td>
<td>RULES</td>
<td>Rationale owned above, structural decisions, code</td>
</tr>
<tr>
<td><strong>Implementation Guides</strong></td>
<td>Realization guidance</td>
<td>HOW</td>
<td>Anything that changes what must be built</td>
</tr>
<tr>
<td><strong>Developer Guides</strong></td>
<td>Task-oriented instruction</td>
<td>HOW TO WORK</td>
<td>Authority over any governed subject</td>
</tr>
</tbody>
</table>

### 5.2 The Ownership Rule

> **No document may take responsibility belonging to another document.**

This rule has no exceptions and no "for readability" allowance. The two common violations are worth
naming because both feel helpful at the moment they are committed:

<table>
<thead>
<tr><th align="left">Violation</th><th align="left">What it looks like</th><th align="left">Correct form</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Convenience restatement</strong></td>
<td>A document repeats a definition, requirement, or decision owned elsewhere "so the reader does not have to look it up".</td>
<td>Reference the owning document. If the reference is hard to follow, improve the reference — not by copying the content.</td>
</tr>
<tr>
<td><strong>Anticipatory detail</strong></td>
<td>A higher-layer document specifies mechanism because the author already knows how it will be built.</td>
<td>State the outcome at this layer. Record the mechanism in the layer that owns it, once that layer exists.</td>
</tr>
</tbody>
</table>

### 5.3 The Responsibility Test

Before a paragraph is added to a document, its author applies one test:

> **Which question does this paragraph answer?**
> If the answer is not the question the document owns, the paragraph belongs in a different
> document — however true, useful, or well written it is.

A paragraph that fails this test and cannot be relocated because the owning document does not exist
yet is recorded as a pending item in that document's future, not smuggled into this one.

</section>

---

<section>

## 6. Writing Style

### 6.1 Language

<table>
<thead>
<tr><th align="left">#</th><th align="left">Convention</th><th align="left">In practice</th></tr>
</thead>
<tbody>
<tr><td>S1</td><td><strong>Use precise engineering language.</strong></td><td>Say what is so, in the fewest words that remain unambiguous. Prefer the concrete noun to the abstract one.</td></tr>
<tr><td>S2</td><td><strong>Avoid marketing language.</strong></td><td>No superlatives, no "powerful", "seamless", "revolutionary", "world-class", "effortless". A claim that cannot be checked MUST NOT be made.</td></tr>
<tr><td>S3</td><td><strong>Avoid implementation assumptions.</strong></td><td>Above the Specification layer, do not name a mechanism, format, library, or file layout unless the document's layer owns it.</td></tr>
<tr><td>S4</td><td><strong>Prefer timeless wording.</strong></td><td>No "currently", "now", "recently", "new", "in the near future". Write as though the reader arrives in an unknown year.</td></tr>
<tr><td>S5</td><td><strong>Prefer explicit statements.</strong></td><td>State the subject, the obligation, and the object. Do not rely on the reader inferring an actor from context.</td></tr>
<tr><td>S6</td><td><strong>Avoid ambiguous terminology.</strong></td><td>No "simply", "just", "obviously", "should be fine", "etc." with an unbounded list, "and so on" in a normative sentence.</td></tr>
<tr><td>S7</td><td><strong>Use one term per concept.</strong></td><td>Once a term is chosen, it is used everywhere. Synonyms introduced for variety are terminology drift.</td></tr>
<tr><td>S8</td><td><strong>Prefer the active voice for obligations.</strong></td><td>"The author MUST state the scope" — not "the scope must be stated". Passive obligations hide who is bound.</td></tr>
<tr><td>S9</td><td><strong>Prefer positive statements.</strong></td><td>State what is required. Use prohibitions where the prohibition is the point, not as a substitute for stating the rule.</td></tr>
<tr><td>S10</td><td><strong>Expand abbreviations at first use.</strong></td><td>Within each document, on first appearance, unless the term is defined in the Glossary and used as a defined term.</td></tr>
<tr><td>S11</td><td><strong>Write in the third person.</strong></td><td>No "we", no "I". Second person is permitted only in Developer Guides, where instruction is the purpose.</td></tr>
<tr><td>S12</td><td><strong>Mark examples as non-normative.</strong></td><td>An example illustrates a rule; it never extends one. Where confusion is possible, label the example explicitly.</td></tr>
</tbody>
</table>

### 6.2 Construction

- Paragraphs SHOULD carry one idea and SHOULD NOT exceed roughly six lines of rendered text.
- Sentences SHOULD state one obligation. A sentence containing two normative keywords SHOULD be split.
- Headings MUST describe content, not tone. "Constraints on approval" — not "The hard part".
- Lists are used for enumerable items. Prose is used for reasoning. A list of full paragraphs is prose that has been formatted as a list.
- Tables are used where items share a shape and the reader compares across them. A table with one populated column is a list.
- Numbers, ranges, and counts stated in a document MUST match the material they describe. Where a count would drift, the document states the rule instead of the count.

### 6.3 Tone

AEOS documents are written plainly and without persuasion. The reader is assumed to be competent,
short of time, and possibly hostile to the idea being described — which is the correct audience to
write for, because a document that survives that reader survives everyone.

Humor, informality, rhetorical questions, and direct address are absent from every layer above
Developer Guides.

</section>

---

<section>

## 7. Normative Language

### 7.1 Adoption

AEOS adopts the key words of RFC 2119, interpreted as described in RFC 8174: the keywords carry
their normative meaning **only when they appear in all capitals**. The same words in lower case
carry their ordinary English meaning and impose nothing.

### 7.2 The AEOS Keyword Set

<table>
<thead>
<tr><th align="left">Keyword</th><th align="left">Meaning</th><th align="left">Consequence of non-compliance</th></tr>
</thead>
<tbody>
<tr>
<td><strong>MUST</strong></td>
<td>An absolute requirement. There is no permitted circumstance in which the behavior is omitted.</td>
<td>Non-compliance is a defect. The artifact is not conformant.</td>
</tr>
<tr>
<td><strong>MUST NOT</strong></td>
<td>An absolute prohibition.</td>
<td>Non-compliance is a defect. The artifact is not conformant.</td>
</tr>
<tr>
<td><strong>SHOULD</strong></td>
<td>A strong recommendation. Valid reasons to deviate may exist and MUST be understood and weighed before deviating.</td>
<td>Deviation is permitted, MUST be deliberate, and SHOULD be recorded where a reader would otherwise assume an error.</td>
</tr>
<tr>
<td><strong>SHOULD NOT</strong></td>
<td>A strong recommendation against. Valid reasons to proceed may exist and MUST be weighed.</td>
<td>As above.</td>
</tr>
<tr>
<td><strong>MAY</strong></td>
<td>An optional item. The choice is genuinely free and carries no preferred direction.</td>
<td>None. An implementation that omits it and one that includes it are equally conformant.</td>
</tr>
</tbody>
</table>

The remaining RFC 2119 keywords — `REQUIRED`, `SHALL`, `SHALL NOT`, `RECOMMENDED`,
`NOT RECOMMENDED`, `OPTIONAL` — are recognized as synonyms of the five above. AEOS documents SHOULD
use only the five, so that readers and AI runtimes match against a small, fixed vocabulary.

### 7.3 Where Normative Language Is Used

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Normative language</th><th align="left">Reason</th></tr>
</thead>
<tbody>
<tr><td>Vision</td><td>MUST NOT be used</td><td>The Vision imposes no requirement. Normative keywords there would create obligations the document has no authority to create.</td></tr>
<tr><td>Product Requirements</td><td>MAY be used</td><td>Requirements are already binding by virtue of being requirements with identifiers.</td></tr>
<tr><td>Glossary</td><td>SHOULD NOT be used</td><td>A definition states meaning, not obligation.</td></tr>
<tr><td>Document Standard</td><td>MUST be used for its rules</td><td>This document binds the form of every other document, and the strength of each rule must be unambiguous.</td></tr>
<tr><td>Technology Catalog</td><td>SHOULD NOT be used</td><td>The catalog records status; it does not oblige.</td></tr>
<tr><td>Architecture</td><td>MAY be used</td><td>Structural constraints are sometimes binding on lower layers; where they are, keywords make it explicit.</td></tr>
<tr><td>Blueprint</td><td>MAY be used</td><td>As above.</td></tr>
<tr><td>Specification</td><td>MUST be used</td><td>A specification exists to state testable obligations. A specification sentence without a keyword states nothing enforceable.</td></tr>
<tr><td>Implementation Guides</td><td>MAY be used</td><td>Guidance is mostly descriptive; obligations inherited from Specification are referenced, not restated.</td></tr>
<tr><td>Developer Guides</td><td>SHOULD NOT be used</td><td>Guides instruct. Obligations belong to the documents that own them.</td></tr>
</tbody>
</table>

### 7.4 Rules of Use

1. A normative statement MUST identify the party bound by it. "The author MUST", "A conformant document MUST" — never an unattached "It must be ensured".
2. A normative statement MUST be independently checkable. If no reviewer could determine compliance, the statement is not normative; it is an aspiration and MUST be reworded.
3. A normative statement MUST NOT contain more than one obligation. Compound obligations are split.
4. A normative statement MUST NOT be placed inside an example, a note, or a parenthetical.
5. Keywords MUST NOT be emphasized with bold or italics in running text; capitalization is the marker.
6. A document that uses normative keywords MUST include the conformance notice in [Section 7.5](#75-required-conformance-notice).

### 7.5 Required Conformance Notice

Every document that uses normative keywords MUST include the following notice, verbatim, in its
opening material:

> The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are
> to be interpreted as described in RFC 2119 and RFC 8174, and carry their normative meaning only
> when they appear in all capitals.

</section>

---

<section>

## 8. HTML-in-Markdown Standard

### 8.1 The Format

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>F1</td><td>Every AEOS document MUST be a GitHub Markdown file with the <code>.md</code> extension.</td></tr>
<tr><td>F2</td><td>Human-oriented documents SHOULD use GitHub-compatible semantic HTML inside Markdown where it improves readability.</td></tr>
<tr><td>F3</td><td>A document MUST render correctly on GitHub without any external asset, stylesheet, or script.</td></tr>
<tr><td>F4</td><td>A document MUST remain meaningful as plain text when no renderer is present.</td></tr>
<tr><td>F5</td><td>Markdown syntax MUST be preferred wherever it expresses the intent adequately. HTML is used where Markdown cannot express the structure.</td></tr>
</tbody>
</table>

### 8.2 Permitted HTML Elements

<table>
<thead>
<tr><th align="left">Element</th><th align="left">Use</th></tr>
</thead>
<tbody>
<tr><td><code>&lt;section&gt;</code></td><td>Grouping a major section, making its boundary explicit to both audiences.</td></tr>
<tr><td><code>&lt;details&gt;</code></td><td>Progressive disclosure of supporting detail beneath a summary.</td></tr>
<tr><td><code>&lt;summary&gt;</code></td><td>The visible label of a <code>&lt;details&gt;</code> block. Required as its first child.</td></tr>
<tr><td><code>&lt;table&gt;</code> and its child elements</td><td>Comparative or multi-column content, especially where a cell contains block content.</td></tr>
<tr><td><code>&lt;blockquote&gt;</code></td><td>Authority statements, reading rules, and other passages the reader must not skim past.</td></tr>
<tr><td><code>&lt;hr&gt;</code></td><td>Separating major sections.</td></tr>
<tr><td><code>&lt;div align="center"&gt;</code></td><td>Document title blocks and closing blocks only.</td></tr>
<tr><td><code>&lt;br&gt;</code></td><td>A line break inside a table cell or immediately after <code>&lt;/summary&gt;</code>.</td></tr>
<tr><td><code>&lt;strong&gt;</code>, <code>&lt;em&gt;</code>, <code>&lt;code&gt;</code>, <code>&lt;a&gt;</code></td><td>Inline emphasis, literals, and links inside HTML blocks, where Markdown inline syntax is not parsed.</td></tr>
</tbody>
</table>

Elements outside this list SHOULD NOT be used. Where an author believes one is necessary, the need is
raised as a proposed revision to this Standard rather than introduced in a document.

### 8.3 Prohibitions

<table>
<thead>
<tr><th align="left">#</th><th align="left">Prohibition</th><th align="left">Reason</th></tr>
</thead>
<tbody>
<tr><td>F6</td><td>Documents MUST NOT be standalone HTML pages.</td><td>A page is not a Repository Asset that diffs, reviews, and reads as text.</td></tr>
<tr><td>F7</td><td>Documents MUST NOT be wrapped in <code>&lt;html&gt;</code>, <code>&lt;head&gt;</code>, or <code>&lt;body&gt;</code>.</td><td>The document is a fragment inside a rendering context it does not own.</td></tr>
<tr><td>F8</td><td>Documents MUST NOT use CSS, including <code>&lt;style&gt;</code> elements and <code>style</code> attributes.</td><td>Presentation is not portable and is stripped by renderers. Meaning carried by styling is meaning lost.</td></tr>
<tr><td>F9</td><td>Documents MUST NOT use JavaScript or any <code>&lt;script&gt;</code> element.</td><td>Documentation does not execute. Executable documentation is an attack surface and an unreviewable one.</td></tr>
<tr><td>F10</td><td>Documents MUST NOT rely on element attributes beyond <code>align</code>, <code>colspan</code>, <code>rowspan</code>, and <code>href</code>.</td><td>Other attributes are sanitized inconsistently and their effect cannot be relied upon.</td></tr>
<tr><td>F11</td><td>Documents MUST NOT convey meaning through visual formatting alone.</td><td>A reader consuming the source, or an AI runtime, receives none of it.</td></tr>
</tbody>
</table>

### 8.4 Practical Conventions

- A blank line MUST follow an opening HTML block element and precede its closing tag where Markdown is intended to be parsed inside it. Markdown inside HTML without surrounding blank lines is not reliably rendered.
- A `<details>` block SHOULD contain `<summary>` as its first child, followed by a blank line, then a `<br>`, then the content.
- Table header cells SHOULD carry `align="left"` unless the column contains only short numeric values.
- Pipe tables SHOULD be used for simple tabular data; HTML tables are used where a cell contains block content, where header formatting matters, or where cells span.
- Code fences MUST declare a language, or `text` where none applies.
- Every diagram MUST be accompanied by the same information stated in prose or in a table, so that no meaning exists only in the drawing.
- Internal links SHOULD use GitHub anchor form derived from the heading text, and MUST be verified to resolve before freeze.

### 8.5 Objective

> The objective of the HTML-in-Markdown standard is to improve readability for humans while
> preserving full GitHub compatibility, plain-text legibility, diffability, and AI consumption from
> the same artifact. HTML is used to express structure that Markdown cannot. It is never used to
> style, to decorate, or to produce a page.

</section>

---

<section>

## 9. AI Readability

AI runtimes are first-class readers of AEOS documentation. These conventions exist so that a runtime
can determine what a document says, what it governs, and what it requires — without inference and
without a machine-only version of the document.

<table>
<thead>
<tr><th align="left">#</th><th align="left">Convention</th><th align="left">In practice</th></tr>
</thead>
<tbody>
<tr><td>A1</td><td><strong>Clear headings.</strong></td><td>Every heading describes the content beneath it, is unique within the document, and is stable across revisions. Heading levels descend by one; a level is never skipped.</td></tr>
<tr><td>A2</td><td><strong>Predictable hierarchy.</strong></td><td>Documents of the same layer share section order (<a href="#appendix-b--recommended-section-ordering">Appendix B</a>), so position carries information.</td></tr>
<tr><td>A3</td><td><strong>Stable terminology.</strong></td><td>One term per concept, matching the Glossary exactly, including capitalization.</td></tr>
<tr><td>A4</td><td><strong>Short paragraphs.</strong></td><td>One idea per paragraph, so that a passage can be extracted without carrying an unrelated claim with it.</td></tr>
<tr><td>A5</td><td><strong>Explicit ownership.</strong></td><td>Every document states what it governs and what it does not. Every normative statement names the bound party.</td></tr>
<tr><td>A6</td><td><strong>No hidden assumptions.</strong></td><td>Preconditions, scope limits, and exclusions are written, not implied by placement or by omission.</td></tr>
<tr><td>A7</td><td><strong>Self-contained statements.</strong></td><td>A normative statement does not depend on a pronoun resolved three sentences earlier, or on the heading above it, to be understood.</td></tr>
<tr><td>A8</td><td><strong>Stable identifiers.</strong></td><td>Identifiers are assigned once and never reused, renumbered, or repurposed. A retired item is marked retired in place.</td></tr>
<tr><td>A9</td><td><strong>Consistent structural markers.</strong></td><td>The same construct is always expressed the same way: rules in tables with identifiers, detail in <code>&lt;details&gt;</code>, authority in <code>&lt;blockquote&gt;</code>.</td></tr>
<tr><td>A10</td><td><strong>No meaning in presentation.</strong></td><td>Colour, alignment, ordering by visual weight, and typographic emphasis carry no information that is not also stated.</td></tr>
<tr><td>A11</td><td><strong>Explicit references.</strong></td><td>A reference names the document by identifier and the section or item by its identifier — never "as described above" or "see the other document".</td></tr>
<tr><td>A12</td><td><strong>Bounded enumerations.</strong></td><td>A list that is complete says so. A list that is illustrative says so. An unmarked list is read as complete and MUST therefore be complete.</td></tr>
</tbody>
</table>

> **On collapsed content.** Content inside `<details>` is present in the document source and is fully
> available to an AI runtime. Progressive disclosure is a rendering behavior, not a reduction in
> content, and it MUST NOT be used to hide material a reader is required to see.

</section>

---

<section>

## 10. Human Readability

Human readers arrive with a specific question, limited time, and no obligation to read from the
beginning. These conventions serve that reader without costing the other audience anything.

<table>
<thead>
<tr><th align="left">#</th><th align="left">Convention</th><th align="left">In practice</th></tr>
</thead>
<tbody>
<tr><td>R1</td><td><strong>Summaries before detail.</strong></td><td>Every document opens with material that lets a reader decide whether to continue. Every long section opens with its conclusion, not its derivation.</td></tr>
<tr><td>R2</td><td><strong>Progressive disclosure.</strong></td><td>Supporting depth is placed in <code>&lt;details&gt;</code> beneath a summary line that states what is inside, so the page can be scanned in full before any block is opened.</td></tr>
<tr><td>R3</td><td><strong>Consistent section order.</strong></td><td>A reader who knows one AEOS document knows where to look in the next one.</td></tr>
<tr><td>R4</td><td><strong>Tables where appropriate.</strong></td><td>Comparable items go in tables. The reader should not have to hold four paragraphs in memory to compare four things.</td></tr>
<tr><td>R5</td><td><strong>A table of contents.</strong></td><td>Required for any document with more than five top-level sections, with working internal links.</td></tr>
<tr><td>R6</td><td><strong>Scannable structure.</strong></td><td>Meaningful headings, short paragraphs, and no section so long that a reader loses the thread before its point arrives.</td></tr>
<tr><td>R7</td><td><strong>Stated authority up front.</strong></td><td>The reader learns what the document governs before learning what it says.</td></tr>
<tr><td>R8</td><td><strong>Reasons alongside rules.</strong></td><td>Where a rule is likely to be argued with, its reason is given once, briefly. A rule whose reason nobody recorded is a rule someone will eventually remove.</td></tr>
<tr><td>R9</td><td><strong>Examples over abstraction.</strong></td><td>Where a rule is easy to misapply, one concrete example is given and marked non-normative.</td></tr>
<tr><td>R10</td><td><strong>No dead ends.</strong></td><td>Where a document declines to cover something, it names the document that does.</td></tr>
</tbody>
</table>

### Resolving Apparent Conflicts Between the Two Audiences

The two audiences rarely conflict, because both are served by explicitness. Where they appear to,
the resolution is fixed:

> **Serve comprehension first, and never at the cost of completeness.**
> A convention that makes a document pleasant to read while removing information — collapsing
> something a reader must see, replacing a statement with a diagram, implying scope through layout —
> is not a readability improvement. It is a defect that happens to look tidy.

</section>

---

<section>

## 11. Source of Truth Rules

### 11.1 Ownership

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>T1</td><td>Every concept has exactly one owning document.</td></tr>
<tr><td>T2</td><td>A document that does not own a concept MUST reference it rather than define, restate, extend, or narrow it.</td></tr>
<tr><td>T3</td><td>Terminology is owned by the Glossary without exception. A document MUST NOT define a term locally, including in a "terms used here" section.</td></tr>
<tr><td>T4</td><td>Ownership is stated by the owning document, in its authority statement.</td></tr>
<tr><td>T5</td><td>Where ownership of a concept is unclear, the ambiguity is a defect and MUST be resolved by the owner before any document relies on that concept.</td></tr>
</tbody>
</table>

### 11.2 Referencing

A reference MUST identify the owning document and the referenced item precisely enough that a reader
can find it without searching.

<table>
<thead>
<tr><th align="left">Reference to</th><th align="left">Form</th></tr>
</thead>
<tbody>
<tr><td>A document</td><td>Document ID, optionally with the file name — for example, AEOS-PRD.</td></tr>
<tr><td>A requirement</td><td>Document ID and requirement identifier — for example, AEOS-PRD <code>PR-SAF-003</code>.</td></tr>
<tr><td>A section</td><td>Document ID and section number and title.</td></tr>
<tr><td>A term</td><td>The term itself, spelled as the Glossary spells it, linked to the Glossary where the link is useful.</td></tr>
<tr><td>A section within the same document</td><td>An internal link to the section heading.</td></tr>
</tbody>
</table>

References MUST NOT be made by page position, by relative phrasing such as "the document above", or
by quoting a passage in place of citing it. A summary of referenced material is permitted where it
orients the reader; it MUST be brief, MUST be marked as a summary, and MUST NOT be relied upon as
the statement of record.

### 11.3 Conflict Resolution

When a contributor — human or AI — finds two documents making different statements about one
subject:

<table>
<thead>
<tr><th align="left">Step</th><th align="left">Action</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>Determine which document owns the subject, using <a href="#5-document-responsibilities">Section 5</a>.</td></tr>
<tr><td>2</td><td>Treat the owning document's statement as correct for the purpose of proceeding.</td></tr>
<tr><td>3</td><td>Report the conflict as a defect against the non-owning document.</td></tr>
<tr><td>4</td><td>Resolve the conflict at the owning document, under that document's change control.</td></tr>
<tr><td>5</td><td>Where both documents are frozen, or where ownership itself is contested, escalate to the owner. A contributor MUST NOT resolve it.</td></tr>
</tbody>
</table>

A contributor MUST NOT resolve a conflict by editing the document they happen to be working in, by
adding a clarifying note that reconciles the two, or by choosing the statement that suits the task
in hand. Each of those produces a third version of the truth while appearing to remove one.

### 11.4 Precedence

Where documents disagree, precedence follows the hierarchy in
[Section 4](#4-documentation-hierarchy), with these fixed rules:

<table>
<thead>
<tr><th align="left">Situation</th><th align="left">Resolution</th></tr>
</thead>
<tbody>
<tr><td>Any document conflicts with AEOS-PRD on product behavior</td><td>AEOS-PRD governs.</td></tr>
<tr><td>Any document conflicts with AEOS-GLOSSARY on the meaning of a term</td><td>AEOS-GLOSSARY governs.</td></tr>
<tr><td>Any document conflicts with this Standard on documentation form</td><td>This Standard governs.</td></tr>
<tr><td>This Standard appears to speak to product behavior, terminology, architecture, or runtime</td><td>The owning document governs. The statement here is a defect in this document and is reported.</td></tr>
<tr><td>A conflict involves AEOS-VISION</td><td>Resolved as AEOS-VISION and AEOS-PRD themselves prescribe. This Standard does not adjudicate between them.</td></tr>
<tr><td>Two documents at the same layer conflict</td><td>Escalate to the owner. Same-layer conflicts indicate that a responsibility boundary is wrong, not that one document is.</td></tr>
</tbody>
</table>

</section>

---

<section>

## 12. Review and Freeze

### 12.1 Lifecycle States

<table>
<thead>
<tr><th align="left">State</th><th align="left">Meaning</th><th align="left">Permitted changes</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Draft</strong></td>
<td>Under authorship. Not authoritative and MUST NOT be referenced as a source of truth.</td>
<td>Any, by the author.</td>
</tr>
<tr>
<td><strong>In Review</strong></td>
<td>Complete and submitted for review. Content is stable while findings are gathered.</td>
<td>None during a review pass; findings are recorded, not applied.</td>
</tr>
<tr>
<td><strong>In Revision</strong></td>
<td>Findings are being addressed by the author.</td>
<td>Only changes that address recorded findings.</td>
</tr>
<tr>
<td><strong>Approved</strong></td>
<td>The owner has accepted the document. No Critical or Major findings remain open.</td>
<td>Editorial correction only.</td>
</tr>
<tr>
<td><strong>Frozen</strong></td>
<td>Authoritative. Downstream documents may trace to it and depend on it.</td>
<td>Only through the document's change control.</td>
</tr>
<tr>
<td><strong>Superseded</strong></td>
<td>Replaced by a later document, which names it in its <em>Supersedes</em> field.</td>
<td>None. Retained for history.</td>
</tr>
</tbody>
</table>

A document's current state MUST appear in its metadata block. A state transition MUST be recorded in
the document's revision history.

### 12.2 The Lifecycle

<table>
<thead>
<tr><th align="left">Stage</th><th align="left">What happens</th><th align="left">Exit condition</th></tr>
</thead>
<tbody>
<tr>
<td><strong>1. Draft</strong></td>
<td>The author writes the document complete: no placeholders, no unfinished sections, authority stated, responsibilities respected.</td>
<td>The document is complete and self-reviewed against this Standard.</td>
</tr>
<tr>
<td><strong>2. Review</strong></td>
<td>Reviewers examine the document and classify every finding. Reviewers identify inconsistencies; they MUST NOT redesign the document.</td>
<td>All findings recorded with classifications.</td>
</tr>
<tr>
<td><strong>3. Revision</strong></td>
<td>The author addresses findings. Each finding is resolved, declined with a recorded reason, or escalated.</td>
<td>No Critical or Major finding remains open.</td>
</tr>
<tr>
<td><strong>4. Approval</strong></td>
<td>The owner accepts the document. Approval is explicit; silence is not approval.</td>
<td>Owner approval recorded.</td>
</tr>
<tr>
<td><strong>5. Freeze</strong></td>
<td>The document is marked Frozen and becomes authoritative for its subject.</td>
<td>Status and revision history updated.</td>
</tr>
</tbody>
</table>

Stages 2 and 3 repeat until stage 3's exit condition holds. A review pass that produces no Critical
or Major findings SHOULD conclude with a recommendation to freeze.

### 12.3 Finding Classification

<table>
<thead>
<tr><th align="left">Class</th><th align="left">Definition</th><th align="left">Effect on freeze</th></tr>
</thead>
<tbody>
<tr>
<td><strong>Critical</strong></td>
<td>The document contradicts a higher-authority document, takes responsibility it does not own, or states something that would cause incorrect work if acted upon.</td>
<td>Blocks freeze.</td>
</tr>
<tr>
<td><strong>Major</strong></td>
<td>The document is internally inconsistent, incomplete in a way that leaves a reader unable to act, or ambiguous on a point that matters.</td>
<td>Blocks freeze.</td>
</tr>
<tr>
<td><strong>Minor</strong></td>
<td>The document deviates from this Standard in form, structure, or wording without affecting correctness.</td>
<td>Does not block freeze; recorded and normally addressed.</td>
</tr>
<tr>
<td><strong>Nitpick</strong></td>
<td>A matter of preference with no effect on correctness, consistency, or comprehension.</td>
<td>Does not block freeze; addressed at the author's discretion.</td>
</tr>
</tbody>
</table>

### 12.4 Review Conduct

- A reviewer MUST classify every finding.
- A reviewer MUST identify inconsistencies and MUST NOT rewrite or redesign the document under review.
- A reviewer MUST cite the specific location and, where the finding is a Standard violation, the specific rule identifier.
- A reviewer SHOULD state the smallest change that would resolve each finding, without imposing it.
- A reviewer who believes the document's premise is wrong records that as a single Critical finding and escalates, rather than raising it repeatedly throughout.

### 12.5 What a Freeze Means

> A frozen document is **authoritative**. Downstream documents, contributors, reviewers, and AI
> runtimes MUST treat it as correct and MUST NOT reinterpret it locally. A frozen document changes
> only through its own change control. A better idea is recorded as a recommendation for a future
> release; it is never applied silently.

A frozen document remains frozen when a downstream document discovers a problem with it. The
discovery is reported; the freeze holds until the owner acts.

</section>

---

<section>

## 13. Future Evolution

Documentation must be able to change without the hierarchy losing meaning. The arrangement that
allows this is simple: lower layers absorb change, higher layers absorb almost none, and every layer
changes through a process proportionate to what depends on it.

### 13.1 Rate of Change by Layer

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Expected rate of change</th><th align="left">Governance</th></tr>
</thead>
<tbody>
<tr><td>Vision</td><td>Rare — a change of purpose</td><td>Formal governance; owner decision with recorded rationale</td></tr>
<tr><td>Product Requirements</td><td>Controlled — requirements added, refined, retired</td><td>Formal governance; identifiers immutable</td></tr>
<tr><td>Glossary</td><td>Rare — terminology drift is a defect</td><td>Formal governance</td></tr>
<tr><td>Document Standard</td><td>Rare — it is the shape of everything else</td><td>Formal governance</td></tr>
<tr><td>Technology Catalog</td><td>Ongoing — the landscape changes</td><td>Ordinary review</td></tr>
<tr><td>Architecture</td><td>Periodic — under the architecture freeze and owner approval</td><td>Its own change control</td></tr>
<tr><td>Blueprint</td><td>Periodic</td><td>Its own change control</td></tr>
<tr><td>Specification</td><td>Ongoing as behavior is refined</td><td>Its own change control; traces to <code>PR-</code> identifiers</td></tr>
<tr><td>Implementation Guides</td><td>Frequent</td><td>Ordinary review</td></tr>
<tr><td>Developer Guides</td><td>Frequent</td><td>Ordinary review</td></tr>
</tbody>
</table>

### 13.2 Rules of Evolution

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>E1</td><td>A lower layer MUST NOT be changed in a way that contradicts a higher layer. Where the lower layer is right, the higher layer is revised first, through its own governance.</td></tr>
<tr><td>E2</td><td>Evolution SHOULD be additive. New material is added; existing identifiers, sections, and headings are preserved.</td></tr>
<tr><td>E3</td><td>Identifiers MUST NOT be reused, renumbered, or repurposed. A retired item is marked retired in place, with its reason.</td></tr>
<tr><td>E4</td><td>A heading that other documents link to SHOULD NOT be renamed. Where renaming is unavoidable, the change is recorded and dependent links are updated in the same change.</td></tr>
<tr><td>E5</td><td>A new kind of document MUST be assigned to an existing layer. Where no layer fits, adding a layer is a change to this Standard and requires formal governance.</td></tr>
<tr><td>E6</td><td>A change that alters what a document owns is a change to this Standard, not to the document.</td></tr>
<tr><td>E7</td><td>An improvement that would alter a frozen document's concepts is recorded as a recommendation for a future release and applied only after explicit owner approval.</td></tr>
<tr><td>E8</td><td>Every change to a frozen document MUST update that document's version and revision history.</td></tr>
</tbody>
</table>

### 13.3 Versioning

AEOS documents are versioned `MAJOR.MINOR.PATCH`:

<table>
<thead>
<tr><th align="left">Increment</th><th align="left">When</th></tr>
</thead>
<tbody>
<tr><td><strong>Patch</strong></td><td>Editorial correction with no change of meaning.</td></tr>
<tr><td><strong>Minor</strong></td><td>Addition or clarification that does not invalidate anything already stated or depended upon.</td></tr>
<tr><td><strong>Major</strong></td><td>A change of meaning, a removal, or any change that invalidates a statement other documents depend on.</td></tr>
</tbody>
</table>

Where a document defines its own change control, that document's table governs the mapping between
change type and version increment. This section states the default for documents that do not.

</section>

---

<section>

## 14. Document Governance

### 14.1 Status

This document is the **Documentation Source of Truth** for the AEOS repository. It is intended to be
frozen as part of AEOS 1.0 and to remain stable across the life of the product.

### 14.2 Change Control

<table>
<thead>
<tr><th align="left">Change type</th><th align="left">Requires</th><th align="left">Version impact</th></tr>
</thead>
<tbody>
<tr><td>Editorial correction with no change of meaning</td><td>Contributor change, owner acceptance</td><td>Patch</td></tr>
<tr><td>Clarification of an existing rule, convention, or principle</td><td>Owner approval</td><td>Minor</td></tr>
<tr><td>Addition of a rule, convention, principle, or permitted HTML element</td><td>Explicit owner revision request</td><td>Major</td></tr>
<tr><td>Change to the documentation hierarchy or to a document's responsibility</td><td>Explicit owner revision request with recorded rationale</td><td>Major</td></tr>
<tr><td>Removal of a rule, principle, or layer</td><td>Explicit owner decision, recorded, with the reasoning preserved in place</td><td>Major</td></tr>
</tbody>
</table>

### 14.3 Relationship to the Architecture Freeze

This document introduces no architecture and grants no capability. Ideas arising from it that would
change the product's concepts, capability set, or principles are recorded as recommendations for a
future release under the AEOS-PRD governance, and are applied only after explicit owner approval.

### 14.4 Review Policy

Reviews of this document classify findings as **Critical**, **Major**, **Minor**, or **Nitpick**,
identify inconsistencies without redesigning, and recommend freezing the document when no Critical
or Major findings remain.

### 14.5 Precedence

<table>
<thead>
<tr><th align="left">Situation</th><th align="left">Resolution</th></tr>
</thead>
<tbody>
<tr><td>This document conflicts with AEOS-PRD on product behavior</td><td>AEOS-PRD governs. The conflict is a defect in this document and is reported.</td></tr>
<tr><td>This document conflicts with AEOS-GLOSSARY on the meaning of a term</td><td>AEOS-GLOSSARY governs. The conflict is a defect in this document and is reported.</td></tr>
<tr><td>This document conflicts with AEOS-VISION on intent</td><td>Escalate to the owner. This document has no authority over intent.</td></tr>
<tr><td>A downstream document deviates from this document on documentation form</td><td>This document governs. The deviation is a finding against the downstream document.</td></tr>
<tr><td>A frozen document predating this Standard deviates from it</td><td>The frozen document stands. The deviation is a Minor finding, reconciled only under that document's own change control.</td></tr>
</tbody>
</table>

### 14.6 Revision History

<table>
<thead>
<tr><th align="left">Version</th><th align="left">Status</th><th align="left">Summary</th></tr>
</thead>
<tbody>
<tr>
<td>1.0.0</td>
<td>Freeze candidate</td>
<td>Initial documentation standard. Establishes twelve documentation principles, a ten-layer documentation hierarchy with responsibility boundaries, writing style conventions, adoption of RFC 2119 normative language with a five-keyword working set, the HTML-in-Markdown format standard with permitted elements and prohibitions, AI and human readability conventions, source-of-truth and referencing rules, the draft-to-freeze lifecycle with four finding classes, evolution rules, a document template, and a recommended section ordering. Introduces no requirement, terminology, capability, or architecture.</td>
</tr>
</tbody>
</table>

</section>

---

<section>

## Appendix A — Recommended Document Template

**A.1 is non-normative. A.2 is normative.**

### A.1 Template

The template below shows the structure a conformant AEOS document takes. Comments mark which parts
the rules in A.2 require; the remainder is adapted to the document's layer and subject.

```text
<div align="center">

# AI Engineering Operating System

**AEOS — <Document Name>**

*<One-line statement of what this document is.>*

</div>

<table>                                          <!-- required: metadata block -->
  Document | Product | Document ID | Version | Status | Owner | Author |
  Audience | Suggested path | Companion documents | Supersedes
</table>

> **Authority of this document.**                <!-- required: authority statement -->
> What this document governs.
> What it does not govern.
> Which document wins in a conflict.

> The key words MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY ...
                                                 <!-- required if normative keywords are used -->

---

## Table of Contents                             <!-- required if more than five sections -->

---

## 1. Executive Summary                          <!-- required -->
Why this document exists and what a reader gains from it.

## 2. Scope and Applicability                    <!-- required -->
What is governed, what is not, and to whom and when it applies.

## 3..N. Subject Sections                        <!-- required: the document's own content -->
The document's responsibility, expressed in its layer's permitted depth.
Rules in tables with stable identifiers.
Supporting depth in <details> blocks.

## N+1. Document Governance                      <!-- required -->
Status · Change control · Review policy · Precedence · Revision history

## Appendices                                    <!-- optional -->
Non-normative supporting material.

---

<div align="center">

**End of <Document Name>**

<DOCUMENT-ID> · Version <x.y.z> · <Source of Truth statement>

</div>
```

### A.2 Template Rules

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>TP1</td><td>Every document MUST include the metadata block, the authority statement, an executive summary, a scope section, and a governance section.</td></tr>
<tr><td>TP2</td><td>Every document that uses normative keywords MUST include the conformance notice.</td></tr>
<tr><td>TP3</td><td>Sections MUST be numbered; appendices MUST be lettered.</td></tr>
<tr><td>TP4</td><td>A section that would be empty MUST be omitted rather than filled.</td></tr>
<tr><td>TP5</td><td>An appendix SHOULD be non-normative. An appendix that contains normative statements MUST be marked normative at its head, and MUST NOT introduce an obligation absent from the document body.</td></tr>
</tbody>
</table>

</section>

---

<section>

## Appendix B — Recommended Section Ordering

**B.1 to B.3 are non-normative. B.4 is normative.**

Ordering exists so that position carries information: a reader who knows one AEOS document can find
their way through the next one without reading it.

### B.1 Universal Ordering

<table>
<thead>
<tr><th align="left">#</th><th align="left">Section</th><th align="left">Presence</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>Title block</td><td>Required</td></tr>
<tr><td>2</td><td>Metadata block</td><td>Required</td></tr>
<tr><td>3</td><td>Authority statement</td><td>Required</td></tr>
<tr><td>4</td><td>Conformance notice</td><td>Required where normative keywords are used</td></tr>
<tr><td>5</td><td>Table of contents</td><td>Required above five top-level sections</td></tr>
<tr><td>6</td><td>Executive summary</td><td>Required</td></tr>
<tr><td>7</td><td>Scope and applicability</td><td>Required</td></tr>
<tr><td>8</td><td>Principles or foundational statements</td><td>Where the document has them</td></tr>
<tr><td>9</td><td>Subject sections, general before specific</td><td>Required</td></tr>
<tr><td>10</td><td>Constraints, prohibitions, and boundaries</td><td>Where the document has them</td></tr>
<tr><td>11</td><td>Lifecycle, process, or evolution</td><td>Where the document has them</td></tr>
<tr><td>12</td><td>Document governance</td><td>Required</td></tr>
<tr><td>13</td><td>Appendices</td><td>Optional</td></tr>
<tr><td>14</td><td>Closing block</td><td>Required</td></tr>
</tbody>
</table>

### B.2 Ordering Within a Section

1. State the conclusion or the rule.
2. State its scope and any exclusions.
3. State the detail, in a table where items are comparable.
4. State the reasoning, briefly, where the rule is likely to be argued with.
5. Give one example where misapplication is likely, marked non-normative.

### B.3 Layer Variations

<table>
<thead>
<tr><th align="left">Layer</th><th align="left">Ordering emphasis</th></tr>
</thead>
<tbody>
<tr><td>Vision</td><td>Purpose, then long-horizon intent, then convictions, then non-goals, then invariants.</td></tr>
<tr><td>Product Requirements</td><td>Boundary, then problem, then capabilities, then numbered requirements, then quality attributes, then phases.</td></tr>
<tr><td>Glossary</td><td>Terms in a single stable order, with no narrative sections between them.</td></tr>
<tr><td>Technology Catalog</td><td>Categories, then entries within each category, each with a recorded status.</td></tr>
<tr><td>Architecture and Blueprint</td><td>Context, then structure, then boundaries, then decisions with their reasoning and traces.</td></tr>
<tr><td>Specification</td><td>Scope, then definitions referenced, then normative rules, then error conditions, then acceptance criteria.</td></tr>
<tr><td>Implementation and Developer Guides</td><td>Prerequisites, then task order, then verification, then troubleshooting.</td></tr>
</tbody>
</table>

### B.4 Ordering Rules

<table>
<thead>
<tr><th align="left">#</th><th align="left">Rule</th></tr>
</thead>
<tbody>
<tr><td>O1</td><td>Documents of the same layer SHOULD share section order.</td></tr>
<tr><td>O2</td><td>General content MUST precede specific content within a section.</td></tr>
<tr><td>O3</td><td>Normative content MUST precede non-normative supporting material.</td></tr>
<tr><td>O4</td><td>Governance MUST be the final numbered section, before any appendix.</td></tr>
<tr><td>O5</td><td>A document that departs from this ordering SHOULD state why in its scope section.</td></tr>
</tbody>
</table>

</section>

---

<div align="center">

**End of Document Standard**

AEOS-DOCSTD · Version 1.0.0 · Documentation Source of Truth

</div>
