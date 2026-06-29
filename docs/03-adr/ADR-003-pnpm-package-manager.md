---

adr: ADR-003
title: Adopt pnpm Package Manager
status: Accepted
date: 2026-06-30
deciders: [Principal Software Architect]
project: ai-first-engineering-portfolio-platform
stack: [Next.js App Router, React, TypeScript, Tailwind CSS, pnpm]
tags: [tooling, dependency-management, build, ci]
supersedes: null
superseded_by: null
related: [ADR-001, ADR-002]
---------------------------

# ADR-003: Adopt pnpm Package Manager

## Status

**Accepted** — pnpm is the sole package manager for this repository. All dependency installation, scripts, and CI pipelines are standardized on it. Any deviation requires a new ADR.

---

## Context

This platform is built on Next.js (App Router, ADR-002), React, TypeScript, and Tailwind CSS, under a Feature-First Architecture (ADR-001), with documentation-first engineering and AI-assisted software development as core engineering principles.

Although the project currently has a single maintainer, it is intended to evolve into a long-lived production-grade engineering platform that may eventually become a multi-package workspace with multiple contributors.

The package manager is foundational infrastructure. It determines how dependencies are installed, resolved, cached, versioned, and reproduced across developer machines and CI environments. It also influences build reliability, dependency correctness, developer experience, and long-term maintainability.

Changing package managers after a project has accumulated dependencies, lockfiles, and CI workflows is a significant migration rather than a simple tooling change. For that reason, this decision is made early, before additional technical debt accumulates.

---

## Problem Statement

The platform requires a package manager that:

* Produces deterministic and reproducible dependency installations across local development and CI environments.
* Scales efficiently as the dependency graph grows without excessive install times or unnecessary disk usage.
* Prevents accidental reliance on undeclared (phantom) dependencies, preserving package correctness and architectural boundaries.
* Supports future workspace or monorepo evolution without requiring a package manager migration.
* Integrates cleanly with the modern Next.js ecosystem and common CI/CD providers.
* Supports AI-assisted development by providing predictable dependency resolution and minimizing hidden package relationships.

The question this ADR answers is:

**Which package manager should this repository standardize on, and why?**

---

## Decision

This project will adopt **pnpm** as its exclusive package manager.

A single `pnpm-lock.yaml` file will serve as the authoritative source of dependency resolution. All dependency installation, development scripts, production builds, and CI pipelines will be executed using pnpm.

pnpm's strict dependency resolution model is considered a deliberate architectural safeguard rather than an inconvenience. If code fails because a dependency has not been explicitly declared, the dependency declaration—not the package manager—must be corrected.

Internally, pnpm uses a content-addressable package store combined with a symlink-based `node_modules` structure. This avoids unnecessary duplication of identical packages while enforcing explicit dependency relationships between modules.

---

## Alternatives Considered

### 1. npm

Rejected despite being the default package manager.

npm's flattened dependency structure allows **phantom dependencies**, where code can successfully import packages that were never explicitly declared because they happen to exist as transitive dependencies elsewhere in the dependency tree.

This weakens dependency correctness, makes builds less predictable, and increases the risk of hidden failures—particularly in AI-assisted workflows where automatically generated imports may appear to work without proper dependency declarations.

While npm remains mature and widely supported, these correctness limitations outweigh its familiarity.

---

### 2. Yarn (Classic and Berry)

Yarn Classic shares many of npm's dependency resolution characteristics and offers limited advantages over pnpm for this project's requirements.

Yarn Berry's Plug'n'Play model improves dependency correctness but introduces a non-standard module resolution mechanism that can require additional tooling support and increases conceptual complexity for contributors.

Although Yarn Berry is a capable solution, pnpm provides comparable correctness guarantees while remaining closer to standard Node.js semantics and requiring fewer ecosystem-specific adaptations.

---

### 3. Bun

Bun's package manager offers excellent performance and continues to mature rapidly.

However, its ecosystem, tooling integration, and long-term production experience remain less established than pnpm's within the Next.js ecosystem.

Choosing Bun today would prioritize early adoption over long-term stability, which conflicts with this project's production-grade engineering principles.

---

### 4. No Standardized Package Manager

Rejected outright.

Allowing contributors to choose different package managers inevitably produces inconsistent lockfiles, dependency resolution drift, and environment-specific build failures.

A production-grade engineering workflow requires one documented, enforceable source of truth.

---

## Trade-offs

Adopting pnpm introduces several costs.

* **Reduced familiarity.** Many developers are more accustomed to npm, requiring a small one-time learning investment.
* **Occasional compatibility issues.** Some third-party packages incorrectly rely on npm's loose dependency resolution and may expose undeclared dependency problems under pnpm's stricter model.
* **CI configuration.** Continuous integration environments must explicitly install or enable pnpm before executing builds.

These costs are accepted because they are small, predictable, and primarily one-time investments.

In return, the project gains significantly stronger dependency correctness, improved installation performance, lower disk usage, and a more deterministic development workflow.

---

## Consequences

### Maintainability

Every dependency must be explicitly declared, ensuring that `package.json` accurately represents the project's true dependency graph rather than relying on transitive package availability.

---

### Scalability

pnpm's content-addressable package store avoids storing identical package versions multiple times, improving installation speed and reducing disk consumption as the project grows.

This also benefits CI environments by reducing cache size and installation overhead over the lifetime of the project.

---

### Team Collaboration

A single package manager and a single committed lockfile eliminate ambiguity regarding dependency resolution across developer machines and automated build environments.

---

### AI-Assisted Development

Strict dependency resolution provides immediate feedback whenever an AI assistant—or a human developer—introduces code that depends on undeclared packages.

Instead of allowing hidden dependency relationships to accumulate silently, pnpm converts these mistakes into immediate build-time failures, improving the reliability of AI-generated code and reinforcing explicit architectural boundaries.

---

### Onboarding

New contributors follow one documented installation workflow without needing to determine which package manager or lockfile governs the repository.

---

### Long-Term Evolution

If the platform later expands into a larger workspace or monorepo, pnpm's mature workspace capabilities allow that evolution without requiring a package manager migration.

---

### Accepted Risk

A small number of legacy packages may require additional investigation when they assume npm's permissive dependency resolution behavior.

This project accepts that occasional friction in exchange for stronger dependency correctness across the entire codebase.

---

## Future Considerations

Future Architecture Decision Records may define:

* Workspace organization
* Internal package boundaries
* Shared package publishing
* Dependency governance policies
* Build pipeline optimization
* CI/CD dependency caching strategies
* Evaluation of emerging package managers such as Bun as the ecosystem matures

This ADR establishes only the project's package manager standard.

---

## References

* ADR-001: Adopt Feature-First Architecture
* ADR-002: Adopt Next.js App Router
* pnpm Documentation
* Next.js Documentation
* Michael Nygard — *Documenting Architecture Decisions*
* Robert C. Martin — *Clean Architecture*
