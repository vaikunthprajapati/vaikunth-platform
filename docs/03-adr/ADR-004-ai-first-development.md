---
adr: ADR-004
title: Adopt AI-First Development Workflow
status: Accepted
date: 2026-06-30
deciders: [Principal Software Architect]
project: ai-first-engineering-portfolio-platform
stack: [Next.js App Router, React, TypeScript, Tailwind CSS, pnpm]
tags: [workflow, ai-assisted-development, governance, documentation]
supersedes: null
superseded_by: null
related: [ADR-001, ADR-002, ADR-003]
---

# ADR-004: Adopt AI-First Development Workflow

## Status

**Accepted** — AI-assisted development, governed by explicit agent personas and
documentation-first artifacts, is the standard working method for this repository. Any
deviation (e.g., ungoverned AI usage, or a return to purely manual development as the default)
requires a new ADR.

## Context

This platform is built on Next.js (App Router, ADR-002), React, TypeScript, and Tailwind CSS,
under a feature-first folder structure (ADR-001) and a pnpm-standardized toolchain (ADR-003).
Development is currently performed by a single engineer working heavily with AI coding
assistants, with an explicit long-term goal of team scalability.

AI-assisted coding tools are now capable of producing substantial portions of an application's
code, configuration, and documentation directly from natural-language instructions. Used without
structure, this capability tends to produce code that is locally plausible but globally
inconsistent: implementations that satisfy the immediate request while drifting from
architectural conventions (ADR-001), framework conventions (ADR-002), or tooling conventions
(ADR-003) established in prior decisions, simply because the AI agent had no durable record of
those decisions to reason against.

This repository has already begun formalizing AI involvement through a persona-based approach —
an Architect agent definition (`prompts/agents/architect.md`) and a sequence of ADRs intended to
serve as durable architectural memory. This ADR makes that approach explicit and binding,
rather than leaving it as an informal practice that could erode under time pressure.

## Problem Statement

The platform needs an AI-assisted development model that:

- Preserves architectural consistency across changes, regardless of whether a given change is
  authored primarily by a human or by an AI agent acting on human instruction.
- Gives AI agents sufficient, durable context to reason about prior decisions, rather than
  re-deriving (and potentially contradicting) them on every task.
- Keeps reasoning and trade-offs visible and reviewable, so AI-generated changes are auditable
  in the same way human-authored changes are expected to be under documentation-first
  engineering.
- Scales from a single engineer working with AI tools today to a small team in which multiple
  humans and AI agents contribute concurrently, without requiring a different process at that
  point.
- Avoids treating AI assistance as an unstructured productivity shortcut that bypasses the
  reasoning-before-implementation standard already required of human contributors.

The question this ADR answers: **how should AI assistance be integrated into this project's
development process so that it strengthens, rather than erodes, architectural consistency and
long-term maintainability?**

## Decision

We will adopt **AI-first development** as the standard working model: AI agents are treated as
first-class contributors operating under the same standards as a senior human engineer, governed
by explicit, versioned artifacts rather than ad hoc prompting.

Concretely:

- AI agents performing architectural or implementation work operate under defined personas
  (e.g., the Architect persona) that encode the project's engineering standards, including
  reasoning-before-implementation, explicit trade-off disclosure, and rejection of unnecessary
  complexity.
- Architecturally significant decisions — whether proposed by a human or surfaced by an AI agent
  — are captured as ADRs before or alongside implementation, so that both humans and future AI
  sessions have a durable, queryable record of *why* the system is built the way it is.
- AI agents are expected to consult existing ADRs and persona definitions as authoritative
  context before proposing changes, rather than treating each task as if no prior decisions
  exist.
- AI-generated code is held to the same review standard as human-generated code: it must be
  explainable, consistent with prior decisions, and accompanied by reasoning for any non-trivial
  choice.

  Prompt libraries, AI personas, and workspace context documents are treated as version-controlled engineering artifacts. They evolve alongside the codebase and form part of the project's institutional knowledge, ensuring that future AI sessions inherit architectural context rather than recreating it from scratch.

AI-first does not mean AI-unsupervised: a human remains accountable for accepting, rejecting, or
amending AI-proposed decisions and code.

## Alternatives Considered

**1. Ad hoc AI usage (no governance)**
Using AI coding assistants informally, on a per-task basis, with no persona definitions or
ADR discipline. Rejected because it produces exactly the inconsistency this ADR exists to
prevent: each AI session reasons from a blank context, with no enforced memory of prior
architectural decisions, and is therefore prone to silently reintroducing alternatives already
rejected in ADR-001 through ADR-003 simply because it was never told otherwise.

**2. AI used only for low-stakes tasks (boilerplate, formatting) with architecture decided
exclusively by humans**
A more conservative middle ground, where AI assistance is scoped narrowly to mechanical work.
Rejected as the standing policy because it underuses a capability the project has already
committed to relying on, and because the boundary between "low-stakes" and "architecturally
significant" is not actually stable — a seemingly mechanical change (e.g., adding a dependency,
restructuring a folder) can have exactly the architectural consequences ADR-001 through ADR-003
address. Rather than drawing an unreliable line, this ADR applies the same governance to all
AI-assisted work and scales the depth of reasoning to the significance of the change, consistent
with the Architect persona's existing interaction pattern.

**3. Full AI autonomy (AI agents commit and merge without human review)**
Rejected unconditionally at this stage. Removing human accountability from the loop is
incompatible with production-grade engineering practices and with documentation-first
engineering's premise that decisions are deliberated, not merely generated. This is a question
of accountability and review process, not of AI capability, and is not revisited by this ADR.

**4. Bespoke, per-conversation prompting instead of versioned personas and ADRs**
Re-explaining project context and standards in each new AI conversation rather than maintaining
durable artifacts. Rejected because it is not reproducible or auditable: two different sessions
could receive subtly different instructions, and there would be no artifact a human or future AI
agent could inspect to understand what standard was actually applied. Versioned personas and ADRs
convert tacit prompting into explicit, reviewable, reusable project memory.

## Trade-offs

Adopting AI-first development with explicit governance accepts the following costs:

- **Upfront and ongoing documentation overhead.** Maintaining persona definitions and writing
  ADRs for architecturally significant decisions takes time that ad hoc AI usage would not
  require. This is accepted because the cost of inconsistency discovered later is materially
  higher than the cost of documenting a decision once, at the time it is made.
- **Discipline required to keep governing artifacts current.** A persona definition or ADR that
  drifts from actual practice is worse than none, because it creates false confidence. This
  requires the same maintenance attention as code, not a one-time setup.
- **Some friction against the fastest possible AI usage pattern.** Requiring AI agents to
  consult prior ADRs and reason before implementing is slower per task than unconstrained
  prompting. This is a deliberate trade of immediate speed for sustained consistency.

What this decision optimizes for: architectural consistency and auditability across an
indeterminate number of future AI-assisted changes, at the cost of process overhead that a fully
unstructured approach would avoid.

## Consequences

**Maintainability.** Every architecturally significant decision, regardless of whether a human or
an AI agent first proposed it, leaves a written record, so future changes are evaluated against
documented intent rather than against guesses about prior intent.

**Scalability (team).** As additional human contributors and AI sessions join the project, they
inherit the same governing context — personas and ADRs — rather than requiring direct,
person-to-person transfer of undocumented conventions.

**AI-assisted development.** This ADR is itself the formalization of the project's AI-assisted
development principle: AI agents are given durable, structured context (persona definitions,
prior ADRs) specifically so their output remains consistent with ADR-001 through ADR-003 rather
than independently re-deriving conventions on every task.

Because architectural context is stored inside the repository rather than remaining in individual conversations, AI tools can operate with smaller, more focused context windows while producing more consistent engineering decisions.

**Onboarding.** A new contributor — human or AI — can become aligned with the project's
standards by reading the persona definitions and the ADR log, rather than needing direct
explanation from the original engineer.

**Long-term evolution.** As the project's architecture evolves, new ADRs extend the same record
rather than requiring a parallel "tribal knowledge" channel that AI agents cannot access.

**Accepted risk.** Governance artifacts can become stale if not actively maintained. This is
mitigated by treating persona and ADR updates as part of the definition of done for any change
that alters the standards they describe.

## Implementation Guidelines

- Every architecturally significant change is preceded by, or accompanied by, an ADR, regardless
  of whether the change originated from a human proposal or an AI agent's suggestion.
- AI agents performing non-trivial work operate under a defined persona (e.g.,
  `prompts/agents/architect.md`) rather than unscoped, default prompting.
- Before proposing a design or implementation, an AI agent is expected to reference relevant
  existing ADRs explicitly rather than implicitly assuming no prior decisions exist.
- AI-proposed changes are reviewed against the same Definition of Done used for human-authored
  changes, including consistency with ADR-001 (feature-first structure), ADR-002 (App Router
  conventions), and ADR-003 (pnpm tooling).
- A human remains the accountable approver for any AI-proposed architectural decision; AI
  proposes and reasons, humans decide.

## Future Considerations

As the team grows beyond a single engineer, this ADR should be revisited to define how Future ADRs may define:

- standardized prompt libraries
- repository-wide AI context management
- automated architectural compliance checking
- specialized AI agents for architecture, implementation, testing, documentation and review
- repository-aware autonomous engineering workflows

## References

- ADR-001: Adopt Feature-First Architecture
- ADR-002: Adopt Next.js App Router
- ADR-003: Adopt pnpm Package Manager
- Architect persona definition: `prompts/agents/architect.md`