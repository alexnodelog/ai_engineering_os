# OPENAI_PROMPTS.md

**Status:** Active
**Layer:** 8 — Prompt Library
**Framework Version:** v10
**Tier:** 2 (Required)
**Purpose:** Serve as the second of the minimum two tool-specific Prompt Library documents required for Tier 2 completion of Layer 8 (`FRAMEWORK_BLUEPRINT.md`, Sections 2.8, 11.3), providing complete, directly-usable **OpenAI**-targeted invocation wrappers for all three `Active` Tier 1 Skills (`create-feature`, `generate-tests`, `review-code`) — translating each tool-agnostic Skill (Layer 7) into a concrete, copyable prompt artifact for use with OpenAI's API (Responses API / Chat Completions API), without duplicating any Skill's own logic.
**Primary Authority (highest priority, in order):** `FRAMEWORK_BLUEPRINT.md` → `SKILLS.md` → `global_rules_revisionfinal_v10.md` → `global_technology_stack_v10.md`. This document introduces no architectural decision of its own and MUST NOT be read as amending any of the four. Where any statement below appears to conflict with one of them, the higher-priority document wins and this document MUST be corrected (Section 15).
**Reference materials (informational inspiration only, no authority):** the OpenAI Prompt Engineering Guide, OpenAI API Best Practices, and the OpenAI Responses API Documentation. These are consulted in Section 2 below solely for general patterns in prompt structuring, instruction-following reliability, and structured-output/tool-calling conventions. None of OpenAI's specific runtime mechanisms (the Assistants/Responses API runtime itself, function-calling execution, file-search or code-interpreter built-in tools, or any OpenAI-hosted orchestration feature) is adopted as a component of Framework v10. Where a reference-material pattern and the Primary Authority would ever conflict, the Primary Authority prevails without exception.
**Inherits from:** `SKILLS.md` (Layer 7 — the Skill Manifest and the three Skills this document wraps), `skill-create-feature.md`, `skill-generate-tests.md`, `skill-review-code.md` (Layer 7 — the full seven-field specifications this document invokes but does not restate), `global_rules_revisionfinal_v10.md` (Layer 2 — the rules each wrapped Skill's execution must follow), `global_technology_stack_v10.md` (Layer 3 — the technologies each wrapped Skill's `steps` may introduce), and, at the point a wrapped Skill actually executes, whichever Layer 4 Project Rule document matches the current project's archetype.
**Governs:** Nothing below it in the governing sense (KA-003, `FRAMEWORK_BLUEPRINT.md` Section 6 — operational layers are peers). It does, however, supply the Category 4 (mandatory Prompt Library references) artifact a concrete Layer 9 template targeting OpenAI-based developer tooling MUST reference, per `TEMPLATE_SPEC.md`, Section 8.
**Supersedes:** None. This is the first version of this document.
**Read order:** Read after `FRAMEWORK_README.md` (session initialization), the Layer 4 Project Rule document matching the current project's archetype, and `SKILLS.md` (Skill discovery) — at the specific point a developer or agent has selected a Skill via `SKILLS.md`'s two-part discovery filter and has chosen OpenAI's API as the invoking tool. This document MUST NOT be read as a substitute for any of those three prerequisites, nor as a substitute for the full Skill document it wraps.
**RFC 2119:** The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in RFC 2119. They apply to every rule below without exception unless the rule itself states otherwise.

---

## 0. Scope and Position in the Knowledge Architecture

```
Layer 1 — Constitution                         AI_DEVELOPMENT_PHILOSOPHY_v2.0.md
        ↓
Layer 2 — Framework Rules                       global_rules_revisionfinal_v10.md
        ↓
Layer 3 — Technology Standards                  global_technology_stack_v10.md
        ↓
Layer 4 — Project Rules                         project-pc-app_v04.md               (Active)
                                                 project-personal-full-stack_v01.md  (Active)
                                                 project-monolithic_v04.md           (Active)
                                                 project-mobile_v01.md               (Tier 3 / v10.1, pending)
        ↓
Layer 5 — Developer Manuals                     FRAMEWORK_README.md, PROJECT_BOOTSTRAP_GUIDE.md,
                                                 CONTRIBUTING.md, COMMANDS.md, PROJECT_STRUCTURE.md (all Active)
Layer 6 — Reference Implementations
Layer 7 — AI Skills           SKILLS.md, skill-create-feature.md,
                               skill-generate-tests.md, skill-review-code.md (all Active — Layer 7 Active as a whole)
        ↓ (this document inherits from Layer 7, by reference, without restating any Skill's logic)
Layer 8 — Prompt Library       CLAUDE_CODE_PROMPTS.md   (Active — first tool)
                                OPENAI_PROMPTS.md        ← this document (second tool)
        ↓
Layer 9 — Project Templates     TEMPLATE_SPEC.md (Active); template-fastapi-sqlite/ (pending)
```

**What this document contains.** Three complete, tool-specific invocation wrappers — one per `Active` Tier 1 Skill — expressed as OpenAI Responses API request artifacts, together with the session-initialization mapping (Section 8), the HITL Gate handling note (Section 9), and the explicit statement of this document's effect on Layer 8's Tier 2 completeness (Section 11).

**What this document does not contain, by design.** Per the Layer 8 prohibited responsibilities (`FRAMEWORK_BLUEPRINT.md`, Section 2.8):

- It **MUST NOT** define a new engineering capability independent of a Layer 7 Skill. Every wrapper below invokes an already-`Active` Skill; it does not introduce a fourth capability.
- It **MUST NOT** restate any Skill's `input`, `output`, `steps`, or `framework-alignment` content. Each wrapper points to its Skill document for that content.
- It **MUST NOT** be the only place a capability is defined. If a capability existed only as a prompt with no corresponding Skill, it would not yet be a framework-complete artifact (`FRAMEWORK_BLUEPRINT.md`, Section 11.2) — this is not the case for any wrapper below, since all three Skills already exist and are `Active`.
- It **MUST NOT** introduce a new HITL Gate, a new agent role, or a new Skill Metadata field. Every `gate` and `primary-agent-role` value below is reproduced, not invented (Section 4).

**How a developer or agent uses this document.** A human developer who has selected OpenAI's API as their invoking tool for a given Skill consults this document for that Skill's ready-to-use OpenAI request artifact (Sections 5–7), submits it via the OpenAI Responses API, and treats the OpenAI-hosted model as the Level 3 "AI Agent" of the tri-level model (`FRAMEWORK_BLUEPRINT.md`, Section 1.2) for the duration of that invocation.

---

## 1. Relationship to the Skill Manifest and Individual Skill Documents

Per `SKILLS.md`, Section 15, "Every Prompt document MUST reference a Skill indexed here rather than duplicate its logic." Per `FRAMEWORK_BLUEPRINT.md`, Section 11.2, "A Prompt document MUST reference the Skill it invokes rather than re-specifying the Skill's logic." This document satisfies both requirements identically for all three wrappers below: each wrapper's `instructions` text directs the invoked model to treat the named Skill document as the authoritative source of its `input`, `output`, `steps`, preconditions, completion criteria, and failure-condition/escalation rules, and instructs the model to reproduce none of that content from memory or from general knowledge of software engineering practice.

Each wrapper's `gate` and `primary-agent-role` values are reproduced from the Skill's own authoritative declaration (`skill-create-feature.md`, Section 3; `skill-generate-tests.md`, Section 3; `skill-review-code.md`, Section 3) for lookup convenience only. This document does not independently declare or re-derive these values; where a future amendment to a Skill document changes either value, the corresponding row in Section 4 below MUST be updated in the same change (Section 16).

---

## 2. Cross-Tool Design Inspiration (Informational Only)

**Purpose of this section.** Following the same auditable pattern already established for Layer 7 (`SKILLS.md`, Section 2; each Skill document's own Section 2) and for `CLAUDE_CODE_PROMPTS.md`, general *ideas* from OpenAI's own published guidance are reflected in this document's prompt structuring and invocation conventions — never in its architecture, mechanisms, or runtime behavior. Each requested pattern is named, its general source noted, and mapped to the specific Blueprint- or `global_rules`-defined mechanism that already governs it in Framework v10.

| Requested pattern | Illustrated (informationally) by | Realized in this document exclusively through |
|---|---|---|
| **Separate the model's standing instructions from the per-call request payload** | The OpenAI Responses API's `instructions` parameter, documented as taking precedence over conversation-history content for a given call | Each wrapper's `instructions` field (Section 4), which carries the framework-context and Skill-selection instructions, distinct from the `input` array carrying the specific invocation request |
| **Prefer structured, schema-constrained output over free-text parsing when a result must be machine-consumed** | The Responses API's `text.format` (`json_schema`) structured-output feature, and general Prompt Engineering Guide advice to specify output format explicitly | The optional structured-output schema in Section 4's Gate Check / Completion Criteria mapping, used only where a wrapper's output is consumed programmatically (e.g., `review-code`'s recommendation field, Section 7) — never used to encode or alter a Skill's own binding rules |
| **State the model's role and scope narrowly before the task itself** | General Prompt Engineering Guide advice to open with role/context framing before task instructions | The fixed `<framework-context>` block every wrapper opens with (Section 4), stating the tri-level model, the Skill being invoked, and the precondition-check requirement, before any task-specific instruction |
| **Explicit tool/function definitions for actions the model may take, rather than inferred capability** | The Responses API's `tools` (function-calling) parameter | Not adopted as a mechanism this document defines generically — where a specific wrapper's Skill has a genuine external action to perform (none of the three Tier 1 Skills do; all three produce artifacts for human or downstream-Skill consumption), a `tools` entry would be scoped narrowly to that action and documented in that wrapper's own section, never introduced framework-wide by this document |

**Explicit non-adoptions.** No OpenAI-hosted orchestration feature (Assistants API threads/runs as a persistent execution engine, the built-in file-search or code-interpreter tools, OpenAI's own memory or fine-tuning mechanisms) is a component of this document or of Framework v10. This document uses only the Responses API's request/response shape as an *invocation wrapper format*; it does not depend on any OpenAI-side persistence, retrieval, or execution mechanism. Adopting any such mechanism would be a vendor-specific architectural decision requiring its own Gate 2 (Architecture Approval) proposal (`FRAMEWORK_BLUEPRINT.md`, Section 18.2), and is explicitly out of scope for this document. This is the identical non-adoption discipline `SKILLS.md`, Section 2, and `CLAUDE_CODE_PROMPTS.md` already apply to their own respective reference materials.

---

## 3. Tool Independence and This Document's Position

Per `FRAMEWORK_BLUEPRINT.md`, Section 1.4, and correction C-01: no specific AI tool or runtime is a component of the framework's architecture. OpenAI is named in this document only because Layer 8 — and only Layer 8 — is the layer at which naming a specific tool is permitted, since tool selection is a developer/project decision, not a framework architecture decision (`FRAMEWORK_BLUEPRINT.md`, Section 1.4; Section 2.8).

1. This document's existence **MUST NOT** be read as a framework preference for OpenAI over any other AI tool. A developer selecting a different tool for the same Skill consults that tool's own Prompt Library document instead (e.g., `CLAUDE_CODE_PROMPTS.md`).
2. Per `FRAMEWORK_BLUEPRINT.md`, Section 11.3, Tier 2 completion of Layer 8 requires a minimum of two distinct tool-specific Prompt sets, without specifying which two. This document is the second such set, following `CLAUDE_CODE_PROMPTS.md` (Claude Code, the first). Together, per correction C-07's canonical naming of "Prompt Library" and Section 11.3's minimum-two requirement, these two documents satisfy the Tier 2 minimum (Section 11 below).
3. No selection made within this document (e.g., which OpenAI model an invocation targets) **MUST** be treated as a Layer 3 technology-stack decision. `global_technology_stack_v10.md` governs the *application's* technology choices (FastAPI, SQLite, pnpm, Electron, and so forth); the OpenAI model used to execute a Skill wrapper is a separate, developer-side tooling choice this document deliberately does not fix, for the same reason `project-pc-app_v04.md`, Section 8, and `COMMANDS.md`, Section 7, deliberately do not fix an application's test runner or linter ahead of Layer 3 confirmation.

---

## 4. Prompt Document Structure — Common Template

Every wrapper in Sections 5–7 follows the same structure, so that a developer moving between wrapped Skills does not need to learn a new format each time. No field below is a new Skill Metadata field (`FRAMEWORK_BLUEPRINT.md`, Section 10.3); each is either a restatement of an existing Skill field (for lookup convenience) or an OpenAI Responses API request field (a tool-specific invocation detail, not framework architecture).

| Field | Source | Purpose |
|---|---|---|
| **Skill wrapped** | This document | Names the Layer 7 Skill document this wrapper invokes, by filename. |
| **`primary-agent-role`** | Reproduced from the Skill's own Section 3 | Lookup convenience only; not independently declared here. |
| **`gate`** | Reproduced from the Skill's own Section 3 | Lookup convenience only; not independently declared here. |
| **OpenAI API surface** | This document | States that the wrapper targets the OpenAI Responses API (`POST /v1/responses`), the currently documented general-purpose API surface for both single-turn and multi-turn model invocation. |
| **`instructions`** | This document (OpenAI Responses API request field) | The framework-context and Skill-invocation instructions (Section 4.1 below). Carries no engineering logic of its own. |
| **`input`** | This document (OpenAI Responses API request field) | The per-invocation request: the specific feature/test/review target and any Section-7-of-the-Skill input artifacts the developer supplies. |
| **`model`** | Left as a developer-configured placeholder | Per Section 3, item 3, this document does not fix a specific OpenAI model; the developer supplies the model identifier appropriate to their OpenAI account/project. |

### 4.1 The Common `<framework-context>` Instruction Block

Every wrapper's `instructions` field opens with the following fixed block, reproduced identically across all three wrappers so that the model's framework orientation does not vary by Skill:

```
<framework-context>
You are operating as an AI Agent (Level 3 of a three-level engineering model) inside
"Framework v10," an Engineering Operating System for Agentic Software Development.
A human "Engineering CEO" (Level 1) directs the work; the framework itself (Level 2)
supplies the rules and structures you must follow; you execute within them.

You are about to execute exactly one named, reusable engineering capability called a
"Skill." The full specification of this Skill — its preconditions, required input,
declared output, ordered steps, completion criteria, and failure-condition/escalation
rules — is authoritative and is named explicitly below. You MUST treat that document,
not your own general knowledge of software engineering practice, as the source of
truth for how this Skill is performed. You MUST NOT invent, skip, reorder, or
reinterpret any step, precondition, or completion criterion stated there.

If a precondition stated in that Skill's own document is not satisfied, you MUST
report which precondition is unmet and MUST NOT proceed. If executing this Skill
would require modifying a Layer 1–4 governing document, performing a cross-project
operation, or introducing a new architectural pattern not already authorized by
Layers 1–4, you MUST stop and report this as an escalation requiring a Gate 2
(Architecture Approval) decision, per the Skill's own Failure Conditions section —
you MUST NOT attempt to resolve it yourself.
</framework-context>
```

This block is identical in substance to the framework-orientation content `CLAUDE_CODE_PROMPTS.md` establishes for its own wrappers, restated here in OpenAI Responses API `instructions`-field form rather than in that document's tool-specific format, consistent with each tool's own idiom (Section 2 above).

---

## 5. Wrapper — `create-feature` (OpenAI)

**Skill wrapped:** `skill-create-feature.md`
**`primary-agent-role`:** Backend Agent **or** Frontend Agent, context-dependent (reproduced from `skill-create-feature.md`, Section 3 — the two-role framing is inherited verbatim; this document introduces no third option).
**`gate`:** `none` (reproduced from `skill-create-feature.md`, Section 3 — confirmed, not provisional).

### 5.1 Preconditions Reminder

Before submitting this request, the developer or orchestrating process MUST confirm the five preconditions of `skill-create-feature.md`, Section 6, are satisfied (session initialization complete; project bootstrapped; a scoped feature definition exists from Gate 3; the matching Layer 4 document is `Active`; this Skill was selected via `SKILLS.md`'s two-part discovery filter). This document does not restate those preconditions' content; it only reminds the invoker to check them, exactly as `skill-create-feature.md`, Section 6, itself requires.

### 5.2 OpenAI Responses API Request

```json
{
  "model": "<developer-configured-openai-model>",
  "instructions": "<framework-context block from Section 4.1, followed by:>\n\nYou are invoking the Skill named `create-feature`, fully specified in `skill-create-feature.md` (Framework v10, Layer 7). Your `primary-agent-role` for this invocation is either Backend Agent or Frontend Agent, determined by which side of the Clean Architecture boundary the feature's work primarily sits on, per that Skill's own Section 4 — state explicitly which role applies before beginning Stage 1. This Skill's `gate` field is `none`: your output feeds forward to `generate-tests` and then to `review-code` within the same Development Workflow Phase, and does not itself pause for human approval, but you MUST still perform every Self Review and Gate Check step that Skill's own workflow requires. Execute the Skill's seven-stage workflow (Requirement Analysis, Specification, Implementation Planning, Implementation, Self Review, Documentation Update, Gate Check) exactly as that document states it, citing the specific section of `skill-create-feature.md`, `global_rules_revisionfinal_v10.md`, `global_technology_stack_v10.md`, or the matching Layer 4 Project Rule document that governs each non-trivial choice you make. Self-check your output against that Skill's Completion Criteria table (its Section 10) before presenting it as complete.",
  "input": [
    {
      "role": "user",
      "content": "<the scoped feature definition produced at Gate 3, the matching Layer 4 Project Rule document's identity, and any relevant current codebase context and prior DECISIONS.md entries, per skill-create-feature.md, Section 7>"
    }
  ]
}
```

### 5.3 Handoff

Per `SKILLS.md`, Section 8, this wrapper's output is one of the two inputs `generate-tests` consumes. The developer or orchestrating process MUST pass this wrapper's full output — not a summary of it — as part of the `input` array of the `generate-tests` wrapper (Section 6 below).

---

## 6. Wrapper — `generate-tests` (OpenAI)

**Skill wrapped:** `skill-generate-tests.md`
**`primary-agent-role`:** Test Agent (reproduced from `skill-generate-tests.md`, Section 3 — a single, non-context-dependent role).
**`gate`:** `none` (reproduced from `skill-generate-tests.md`, Section 3 — confirmed, not provisional).

### 6.1 Preconditions Reminder

Before submitting this request, the developer or orchestrating process MUST confirm the six preconditions of `skill-generate-tests.md`, Section 6, are satisfied — in particular, that either the typical ordering (`create-feature`'s output already exists) or the reversed, test-first ordering (`create-feature`'s Specification/Implementation Plan artifacts exist ahead of its own Implementation stage) applies, per that Skill's own Section 1 and Section 6, item 4. This document does not restate that ordering logic; it only reminds the invoker that one of the two MUST be determined before this Skill begins (Stage 1 of that Skill's own workflow).

### 6.2 OpenAI Responses API Request

```json
{
  "model": "<developer-configured-openai-model>",
  "instructions": "<framework-context block from Section 4.1, followed by:>\n\nYou are invoking the Skill named `generate-tests`, fully specified in `skill-generate-tests.md` (Framework v10, Layer 7). Your `primary-agent-role` for this invocation is Test Agent. This Skill's `gate` field is `none`. Determine at Stage 1 whether the typical ordering or the reversed, test-first ordering applies to this invocation, per that Skill's Section 1 and Section 6, item 4 — do not assume one. Execute the Skill's nine-stage workflow (Intake and Test Scope Analysis, Test Strategy and TDD Boundary Mapping, Unit Test Generation, Integration Test Generation, End-to-End Test Plan Generation, Regression Test Generation, Edge Case and Negative Test Case Generation, Self Review and Documentation Update, Gate Check and Handoff) exactly as that document states it. Organize your output by the six required test categories (Unit, Integration, End-to-End, Regression, Edge Case, Negative), per that Skill's Section 10 Test Category Taxonomy, stating explicitly wherever a category does not apply (most commonly Regression Tests where no defect history exists) rather than omitting it silently. Self-check your output against that Skill's Completion Criteria table (its Section 11) before presenting it as complete.",
  "input": [
    {
      "role": "user",
      "content": "<create-feature's declared output (typical ordering) or Specification/Implementation Plan artifacts (reversed ordering); the scoped feature definition from Gate 3; and any prior DECISIONS.md entries or defect history relevant to the feature, per skill-generate-tests.md, Section 7>"
    }
  ]
}
```

### 6.3 Handoff

Per `SKILLS.md`, Section 8, this wrapper's output, jointly with `create-feature`'s output, is the declared input to `review-code`. Both outputs in full MUST be passed into the `input` array of the `review-code` wrapper (Section 7 below).

---

## 7. Wrapper — `review-code` (OpenAI)

**Skill wrapped:** `skill-review-code.md`
**`primary-agent-role`:** Code Review Agent (reproduced from `skill-review-code.md`, Section 3 — a single, non-context-dependent role).
**`gate`:** `Gate 4 — Implementation Approval` (reproduced from `skill-review-code.md`, Section 3 — confirmed, not provisional, per `global_rules_revisionfinal_v10.md`, Section 7.4, and `FRAMEWORK_BLUEPRINT.md`, Section 9.2).

### 7.1 Preconditions Reminder

Before submitting this request, the developer or orchestrating process MUST confirm the six preconditions of `skill-review-code.md`, Section 6, are satisfied — in particular, that `create-feature`'s output exists and that test coverage consistent with the Layer 2 TDD Boundary exists for the feature under review, whether from a separate `generate-tests` invocation or embedded in `create-feature`'s own test-first implementation.

### 7.2 OpenAI Responses API Request

Because this wrapper's output is what the human Engineering CEO evaluates at Gate 4, this wrapper additionally uses the Responses API's structured-output feature (`text.format`) to guarantee the recommendation field is machine-parseable, consistent with the pattern named in Section 2 above — this is a formatting convenience only and alters no rule `skill-review-code.md` itself states.

```json
{
  "model": "<developer-configured-openai-model>",
  "instructions": "<framework-context block from Section 4.1, followed by:>\n\nYou are invoking the Skill named `review-code`, fully specified in `skill-review-code.md` (Framework v10, Layer 7). Your `primary-agent-role` for this invocation is Code Review Agent. This Skill's `gate` field is `Gate 4 — Implementation Approval`: your output is what the human Engineering CEO evaluates before approving or rejecting merge; you MUST NOT render that approval decision yourself, and you MUST NOT describe your output as if Gate 4 had already occurred. Execute the Skill's nine-stage workflow (Intake and Review Scope Confirmation, Blueprint and Framework Rule Compliance, Architecture and Technology Compliance, Code Quality, Security, Test Coverage Verification, Documentation Impact, RFC 2119 Compliance where applicable, Severity Classification with Gate Handoff) exactly as that document states it. Classify every finding as Critical, Major, or Minor per that Skill's Section 10, citing the specific governing-layer rule each finding traces to, and determine your overall recommendation mechanically from whether any Critical finding remains open, per that Skill's Section 9.9. You MUST NOT implement, rewrite, or remediate any code or test you review; report findings only. Self-check your output against that Skill's Completion Criteria table (its Section 11) before presenting it as complete.",
  "input": [
    {
      "role": "user",
      "content": "<create-feature's declared output; generate-tests's declared output (or the test artifacts embedded in create-feature's own test-first implementation); the original Gate 3 scope definition; the matching Layer 4 Project Rule document; and any prior DECISIONS.md entries relevant to the feature, per skill-review-code.md, Section 7>"
    }
  ],
  "text": {
    "format": {
      "type": "json_schema",
      "name": "review_code_report",
      "schema": {
        "type": "object",
        "properties": {
          "review_scope": { "type": "string" },
          "findings": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "dimension": { "type": "string" },
                "severity": { "type": "string", "enum": ["Critical", "Major", "Minor"] },
                "description": { "type": "string" },
                "governing_rule_citation": { "type": "string" }
              },
              "required": ["dimension", "severity", "description", "governing_rule_citation"]
            }
          },
          "definition_of_done_cross_check": { "type": "string" },
          "test_coverage_summary": { "type": "string" },
          "flagged_decisions_md_candidates": { "type": "string" },
          "recommendation": { "type": "string", "enum": ["ready for Gate 4 approval", "not ready for Gate 4 approval"] }
        },
        "required": ["review_scope", "findings", "definition_of_done_cross_check", "test_coverage_summary", "recommendation"],
        "additionalProperties": false
      }
    }
  }
}
```

The `text.format` schema above encodes only the output *shape* `skill-review-code.md`, Section 8, already declares (a structured findings report, an explicit recommendation, a Definition of Done cross-check, a Test Coverage summary, and flagged decision candidates). It introduces no new content requirement beyond that Skill's own declared output, per Section 2 above.

---

## 8. Session Initialization Mapping onto the OpenAI API

The AI Session Initialization Sequence (`FRAMEWORK_README.md`, Section 7; `FRAMEWORK_BLUEPRINT.md`, Section 14) is defined once, at Layer 5, and is not restated here. This section states only how that already-frozen sequence maps onto OpenAI's request model, since the OpenAI Responses API is stateless per call by default (each `POST /v1/responses` request is independent unless the developer explicitly threads prior response IDs via the `previous_response_id` parameter or otherwise supplies conversation history).

1. The developer or orchestrating process **MUST** ensure the AI Session Initialization Sequence has been completed — `FRAMEWORK_README.md` read, the Constitution read for value calibration, `global_technology_stack_v10.md` read, `DECISIONS.md` and `FRAMEWORK_HANDOVER.md` read — before submitting any wrapper request in Sections 5–7, exactly as `PROJECT_BOOTSTRAP_GUIDE.md`, Section 2, already requires independent of which AI tool performs the work.
2. Because the Responses API does not persist framework context across calls on the developer's behalf, the developer or orchestrating process **MUST** supply the relevant outcome of that sequence (e.g., the current archetype's Layer 4 document identity, relevant `DECISIONS.md` entries, and `FRAMEWORK_HANDOVER.md`'s current-state summary) as part of the `input` array of each wrapper request, rather than assume the model retains it from an earlier, unrelated call.
3. This is a tool-specific consequence of OpenAI's request model, not a new framework requirement. `CLAUDE_CODE_PROMPTS.md` maps the identical sequence onto Claude Code's own project-memory mechanism; this document maps it onto explicit per-call context inclusion instead, because the two tools' underlying persistence models differ — the underlying sequence being mapped is unchanged.

---

## 9. HITL Gate Handling Within the OpenAI Interaction Model

Per `FRAMEWORK_BLUEPRINT.md`, Section 9.3, HE-003, the technical mechanism by which an agent pauses and resumes at a named HITL Gate is explicitly deferred and is not specified by the framework at v10. This document does not resolve that deferral; it states only how a `gate` value already declared by a wrapped Skill (Section 4) presents within the OpenAI interaction model:

1. Where a wrapper's `gate` field is `none` (`create-feature`, `generate-tests`), the model's response is passed directly to the next wrapper in the Development Workflow Phase (`SKILLS.md`, Section 8) without an intervening pause introduced by this document.
2. Where a wrapper's `gate` field names a canonical Gate (`review-code` → Gate 4), the model's response **MUST** be presented to the human Engineering CEO for approval or rejection before any subsequent action (e.g., a merge command per `COMMANDS.md`, Section 5) is taken. This document does not itself implement that pause — it is the developer's or orchestrating process's responsibility to withhold downstream action until human approval is recorded, exactly as `PROJECT_BOOTSTRAP_GUIDE.md`, Section 4.6, already requires for Gate 1 independent of tooling.
3. No feature of the OpenAI API (e.g., a tool-call requiring approval, or a background/async response) is treated by this document as satisfying the HE-003 pause/resume mechanism. Using such a feature for that purpose would be a runtime-mechanism adoption requiring its own Gate 2 proposal, per Section 2 above, and is out of scope for this document.

---

## 10. Relationship to Other Layer 8 Documents

| Document | Relationship to this document |
|---|---|
| `CLAUDE_CODE_PROMPTS.md` | The first of the minimum two required tool-specific Prompt documents (Claude Code). Wraps the identical three Skills this document wraps, using that tool's own idiom. Neither document has authority over the other — both are peer Layer 8 artifacts (`FRAMEWORK_BLUEPRINT.md`, Section 6, "Operational-layer authority note"). |
| `SKILLS.md` | The Layer 7 Manifest every wrapper in this document ultimately points to for Skill discovery and selection (Section 6 of that document). |
| `skill-create-feature.md`, `skill-generate-tests.md`, `skill-review-code.md` | The three full Skill specifications this document's wrappers invoke without restating. |
| `TEMPLATE_SPEC.md` | Category 4 (mandatory Prompt Library references, its own Section 8) requires a concrete Layer 9 template to reference at least two distinct tools' Prompt documents; a template targeting OpenAI-based developer tooling MAY now reference this document, alongside `CLAUDE_CODE_PROMPTS.md` for Claude Code. |

---

## 11. Effect of This Document on Layer 8 and Layer 9 Completeness

1. Per `FRAMEWORK_BLUEPRINT.md`, Section 2.8 ("`Active` once at least two tool-specific Prompt sets exist") and Section 11.3 ("minimum of two distinct AI tools"), **Layer 8 (Prompt Library) transitions to `Active` as a whole upon this document's creation**, since `CLAUDE_CODE_PROMPTS.md` (Claude Code) was already `Active` and this document supplies the second, distinct tool (OpenAI). This is a status transition mechanically determined by an already-frozen completeness criterion, not a new architectural decision — identical in kind to Layer 7's transition to `Active` upon `skill-generate-tests.md`'s creation.
2. Per `TEMPLATE_SPEC.md`, Section 8, item 1, and Section 14.1: Category 4 (mandatory Prompt Library references) previously could not be satisfied by any concrete template because Layer 8 supplied only one tool. With this document's creation, the two-tool minimum that category requires is now met in full (Claude Code + OpenAI). `TEMPLATE_SPEC.md`, Section 14.1, **MUST** be updated in the same change as this document's adoption to state that Category 4 is now fully — not merely partially — satisfiable, per `TEMPLATE_SPEC.md`, Section 17, Rule 2.
3. This document's creation does **not** resolve the Layer 9 KA-004 minimum-artifact gap (`template-fastapi-sqlite/` remains `Pending`) or the standing archetype-disambiguation question between `project-personal-full-stack_v01.md` and `project-monolithic_v04.md` (`FRAMEWORK_STATUS.md`, Flag 10). Neither is in scope for a Layer 8 document to resolve.

---

## 12. Prohibited Responsibilities — Explicit Boundary

Consistent with `FRAMEWORK_BLUEPRINT.md`, Section 2.8, this document explicitly does **not** do the following, and any future edit that adds one of these is out of scope for Layer 8 and MUST be redirected to the correct layer instead:

| Not defined here | Correct layer |
|---|---|
| The Skill's own `input`, `output`, `steps`, or `framework-alignment` content | Layer 7 (the individual Skill document) |
| A new engineering capability not already backed by an `Active` Layer 7 Skill | Layer 7 (a new Skill would be required first) |
| The canonical technology table, or any application-level technology selection | Layer 3 (`global_technology_stack_v10.md`) |
| Project directory layout or naming conventions | Layer 4 (the matching Project Rule document) |
| The AI Session Initialization Sequence itself | Layer 5 (`FRAMEWORK_README.md`, Section 7) |
| A concrete, clonable project scaffold | Layer 9 (`TEMPLATE_SPEC.md` and concrete templates) |
| The technical mechanism for pausing/resuming at a HITL Gate (HE-003) | Deferred at the framework level; not defined at any layer in v10 |

---

## 13. Authority and Conflict Resolution

1. Per the Primary Authority ordering stated in the header, and per the downward authority rule (KA-002), this document **MUST NOT** contradict `FRAMEWORK_BLUEPRINT.md`, `SKILLS.md`, `global_rules_revisionfinal_v10.md`, or `global_technology_stack_v10.md`, in that order of precedence. Where this document appears to conflict with any of the four, the higher-priority document wins and this document **MUST** be corrected.
2. Where a wrapper in this document appears to conflict with the Skill document it wraps (e.g., a stale `gate` value after that Skill is amended), the Skill document is authoritative and this document **MUST** be corrected in the same change (Section 16).
3. None of the reference materials named in the header (Section 2) carries any authority over this document. A pattern drawn from them **MUST NOT** be cited as justification for a rule that conflicts with any Primary Authority document.
4. `CLAUDE_CODE_PROMPTS.md` and this document are peer Layer 8 artifacts; neither outranks the other. A conflict between the two (e.g., over how a shared concept like Gate handling is described) **MUST** be resolved by correcting whichever document has drifted from the shared, authoritative Layer 7 source, not by preferring one tool's document over the other.
5. The full conflict-resolution procedure, including same-layer conflicts, is defined in `FRAMEWORK_BLUEPRINT.md`, Section 6, and is not restated here.

---

## 14. Change Control

1. This document **MUST NOT** be edited silently to wrap a Skill not already `Active` in `SKILLS.md`, or to introduce a new HITL Gate, agent role, or Skill Metadata field. Any such change is an architectural change and **MUST** follow the amendment procedure defined in `FRAMEWORK_BLUEPRINT.md`, Section 18.2.
2. Whenever a wrapped Skill document is amended (its `input`, `output`, `steps`, `gate`, or `framework-alignment` changed), the corresponding wrapper section of this document **MUST** be updated in the same change to reflect the amendment, per the mirroring obligation this document places on itself (Section 13, Rule 2).
3. Upon this document's creation, `FRAMEWORK_README.md`, Sections 4–6, `TEMPLATE_SPEC.md`, Section 14.1, and `FRAMEWORK_STATUS.md` **MUST** be updated in the same change to reflect: (a) this document's new `Active` status; (b) that Layer 8 as a whole is now `Active` (Section 11, item 1); and (c) that `TEMPLATE_SPEC.md`'s Category 4 gap is now fully — not partially — resolved (Section 11, item 2).
4. A future third tool's Prompt Library document (e.g., for Cursor, GitHub Copilot, or Codex, per `FRAMEWORK_BLUEPRINT.md`, Section 11.3's Tier 3/v10.1 target) **MAY** be added without amending this document, since Layer 8's Tier 2 minimum is already satisfied by this document together with `CLAUDE_CODE_PROMPTS.md`.

---

## Closing Statement

This document is the second of the minimum two tool-specific Prompt Library documents required for Tier 2 completion of Layer 8 (`FRAMEWORK_BLUEPRINT.md`, Sections 2.8, 11.3), providing complete OpenAI Responses API invocation wrappers for all three `Active` Tier 1 Skills — `create-feature`, `generate-tests`, and `review-code` — each pointing to its Skill document for engineering logic rather than duplicating it, each reproducing its Skill's already-authoritative `primary-agent-role` and `gate` values rather than re-declaring them, and each expressed as a complete, copyable OpenAI Responses API request artifact. No new architectural decision has been introduced: no new Skill, Gate, agent role, or schema field appears anywhere in this document, and the OpenAI-specific structuring choices in Section 2 map directly to mechanisms `FRAMEWORK_BLUEPRINT.md` already defines or to OpenAI's own documented, unmodified API surface. With this document's creation, **Layer 8 (Prompt Library) transitions to `Active` as a whole**, and `TEMPLATE_SPEC.md`'s previously partial Category 4 gap is now fully resolved.

---

**Reminder:** Per this document's own Section 14, `SKILLS.md`'s general mirroring discipline, and `FRAMEWORK_README.md`'s Section 9 obligation, please update `FRAMEWORK_STATUS.md` to: move `OPENAI_PROMPTS.md` from "Current Work" / "Next Targets" into "Completed Milestones" and "Active Documents"; record that **Layer 8 (Prompt Library) is now `Active` as a whole**, superseding the "not yet `Active`" qualification carried since `CLAUDE_CODE_PROMPTS.md`'s own entry; update Flag 13 to reflect that `TEMPLATE_SPEC.md`, Section 14.1's Category 4 gap is now fully resolved (not merely narrowed) and that `TEMPLATE_SPEC.md` itself has been updated accordingly; remove "Prompt Library — second tool" from "Next Targets" and "Documents To Generate"; advance "Current Target" to `template-fastapi-sqlite/` as the sole remaining blocker for Layer 9's KA-004 minimum-artifact requirement, noting that its standing archetype-disambiguation question (Flag 10, Full-Stack vs. Monolithic) remains open and untouched by this generation; and add a changelog entry recording this document's generation and Layer 8's resulting completion — in the same change, per PR-001. Please also flag `FRAMEWORK_README.md`, Sections 4.2 and 5, for the Owner's attention, since the Prompt Library row of that document's domain table currently marks this item `Pending (Tier 2)`.
