# AI Engineering Operating System

## 1. Overview

AEOS is an operating system for AI-assisted software engineering.

An operating system is what makes many independent capabilities usable together, consistently, so
that every program need not solve the same problems again. AEOS does that for engineering work
performed with AI. It holds the project's context, upholds the engineering discipline, sequences the
work, and keeps the person in control of what happens next.

> **On the metaphor.** *Operating system* describes what AEOS does for engineering activity, not how
> AEOS is built. AEOS is not a kernel. It does not schedule processes, abstract hardware, or replace
> the operating system a machine already runs. The metaphor names the orchestration of software
> engineering work, and nothing below that level.

AEOS does not perform language-model inference. It never has its own model, and it never becomes
one. Inference is delegated to external AI runtimes that the user chooses and controls. AEOS
supplies what those runtimes lack: durable project context, an enforced engineering process,
minimized and deliberate context delivery, a human approval gate in front of every consequential
action, and a repository that remains the single source of truth after the AI session ends.

The result is a product that turns ad-hoc AI assistance into a reproducible engineering practice.

| In one sentence |
| :--- |
| AEOS lets developers build software through AI-assisted, human-supervised engineering workflows, orchestrating any AI runtime across the full lifecycle without ever becoming a model, an IDE, or a framework. |

---

## 2. Executive Summary

**What AEOS is.** AEOS is an operating system for AI-assisted software engineering. It orchestrates
the complete engineering lifecycle — environment preparation, project initialization, requirement
analysis, architecture, TDD, agentic orchestration, code generation, review, refactoring, testing,
documentation, deployment, and maintenance — while performing no language-model inference of its
own. Inference belongs to external AI runtimes that the user chooses and controls. AEOS supplies
what those runtimes do not: durable project context, an enforced engineering discipline, deliberate
context minimization, a human approval gate in front of every consequential action, and a repository
that remains authoritative after the session ends.

**Who it is for.** Developers who build with AI and want the result to be verifiable rather than
merely fast. Engineering leads who need a team's practice to be explicit, versioned, and uniform
across people and tools. Platform engineers responsible for predictable behavior across Windows,
macOS, and Linux. Open-source maintainers who want a repository that teaches contributors — human
and AI — how the project is built. And the AI runtimes themselves, which read the repository to
learn how to act inside a project.

**Why it exists.** AI coding tools have become capable; AI engineering has not become disciplined.
Process improvised per session is not reproducible. Context thrown at a model raises cost and lowers
accuracy. Tools that act before they explain destroy trust. Tests written last leave generated code
unverified. A practice locked to one vendor must be rebuilt when the vendor changes. Environments
assumed rather than inspected break working machines. Knowledge left in a chat log dies with the
session. AEOS answers these as product behavior, not as advice.

**What it guarantees.** The human decides before AEOS acts. AEOS explains before it executes. Tests
come before implementation. The repository is the source of truth. Context is minimized on purpose.
The user's choice of vendor, runtime, model, platform, and distribution method is theirs alone, and
changing any of them does not change how the project is built.

| At a glance |  |
| :--- | :--- |
| **Product category** | Operating system for AI-assisted, human-supervised software engineering |
| **Performs inference** | Never — external AI runtimes do, under user control |
| **Core loop** | Inspect → Explain → Propose → Confirm → Execute → Report |
| **Capabilities** | Ten, spanning environment, project, workflow, runtime, TDD, documentation, rules, skills, prompts, and repository |
| **Platforms** | Windows, macOS, Linux — equal citizens, no primary |
| **Distribution** | GitHub Clone, Native Installer, MCP, Portable — with package managers, containers, and IDE marketplaces planned |
| **Source of truth** | The repository, as versioned Repository Assets |
| **Default posture** | Human-in-the-Loop, TDD-first, safe by default, fails closed |

---

