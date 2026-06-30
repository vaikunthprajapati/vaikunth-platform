# Information Architecture

## AI-First Engineering Portfolio Platform

**Version:** 1.0  
**Status:** Approved  
**Owner:** Product & Architecture

---

# 1. Purpose

This document defines the Information Architecture (IA) of the AI-First Engineering Portfolio Platform.

Its purpose is to establish how information is organized, owned, connected, and discovered throughout the platform.

The architecture is designed for long-term evolution and serves two primary consumers:

- Human users (recruiters, hiring managers, engineers, collaborators)
- AI systems (LLMs, autonomous agents, semantic crawlers)

This document defines **what information exists, where it belongs, how it relates to other information, and how users discover it.**

It intentionally excludes implementation details, visual design, component structure, and frontend technology decisions.

---

# 2. Information Architecture Principles

The platform follows the following architectural principles.

## User-Centered Organization

Information is organized around user goals rather than technical implementation.

---

## Single Source of Truth

Every piece of information has one authoritative location.

Other pages reference information rather than duplicate it.

---

## Progressive Disclosure

Users should understand the platform at increasing levels of detail.

High-level summaries should naturally lead to architectural evidence, implementation details, and supporting documentation.

---

## Information Ownership

Every page owns one clearly defined responsibility.

Responsibilities should never overlap.

---

## Connected Knowledge

Information domains remain independent while being interconnected through meaningful references.

Projects reference technologies.

Technologies reference projects.

Architecture references ADRs.

Documentation references implementation.

---

## AI-First Discoverability

The platform treats AI systems as first-class consumers of information.

Content should remain semantically structured, predictable, and easily discoverable by machine-readable systems without compromising the human experience.

---

## Long-Term Scalability

The information structure should support years of growth without requiring major reorganization.

New capabilities should extend existing domains rather than introduce unnecessary top-level sections.

---

# 3. Primary Information Domains

The platform is organized into the following primary information domains.

```
Home

About

Projects

Architecture

Engineering Ledger

Writing

Skills

Experience

Achievements

Resume

Contact
```

Each domain represents a stable business responsibility rather than a visual page.

---

# 4. Information Hierarchy

```
Home
│
├── About
│
├── Projects
│   ├── Project Overview
│   ├── Case Study
│   ├── Architecture
│   ├── Technical Decisions
│   ├── Repository
│   └── Live Deployment
│
├── Architecture
│   ├── Product Vision
│   ├── Product Goals
│   ├── Information Architecture
│   ├── User Personas
│   ├── ADRs
│   └── Engineering Documentation
│
├── Engineering Ledger
│   ├── Feature Capsules
│   ├── Decision History
│   ├── System Evolution
│   └── Architecture Timeline
│
├── Writing
│   ├── Engineering Articles
│   ├── Deep Dives
│   ├── System Design
│   └── Learning Notes
│
├── Skills
│
├── Experience
│
├── Achievements
│
├── Resume
│
└── Contact
```

The hierarchy remains intentionally shallow while allowing unlimited expansion within each information domain.

---

# 5. Information Ownership

## Home

Introduces the engineer and provides navigation into the platform.

It summarizes rather than duplicates information.

---

## About

Owns personal background, engineering philosophy, values, and career direction.

---

## Projects

Owns portfolio projects and their supporting evidence.

Projects reference architecture, repositories, demonstrations, and documentation without duplicating them.

---

## Architecture

Owns product strategy, architectural decisions, engineering standards, governance, ADRs, and technical documentation.

This section represents the engineering thinking behind the platform.

---

## Engineering Ledger

Owns the historical evolution of the platform.

It documents architectural decisions, feature evolution, design trade-offs, and engineering reasoning over time.

The Engineering Ledger transforms the portfolio from a static showcase into a living engineering system.

---

## Writing

Owns technical knowledge independent of individual projects.

Articles may reference projects but remain valuable on their own.

---

## Skills

Owns technology classifications.

Skills function as metadata connecting projects, articles, and experience rather than existing as isolated lists.

---

## Experience

Owns professional progression.

It demonstrates growth rather than implementation details.

---

## Achievements

Owns external validation.

Examples include certifications, awards, rankings, and public recognition.

---

## Resume

Owns recruiter-facing professional documents.

This remains the authoritative source for downloadable resumes.

---

## Contact

Owns all communication channels.

No unrelated content belongs here.

---

# 6. Relationships Between Information Domains

The platform follows a connected information model rather than isolated pages.

```
Home
 │
 ├───────────────┐
 │               │
 ▼               ▼
About        Projects
 │               │
 │               ▼
 │         Architecture
 │               │
 ▼               ▼
Experience   Engineering Ledger
 │               │
 └───────┐   ┌───┘
         ▼   ▼
       Writing
          │
          ▼
      Achievements
          │
          ▼
        Resume
          │
          ▼
        Contact
```

Information should flow naturally without duplication.

---

# 7. User Journeys

## Technical Recruiter

Objective:

Quickly determine engineering capability.

Typical journey:

```
Home

↓

Projects

↓

Architecture

↓

Resume

↓

Contact
```

---

## Hiring Manager

Objective:

Evaluate engineering maturity.

Typical journey:

```
Home

↓

Projects

↓

Engineering Ledger

↓

Architecture Decisions

↓

Technical Writing
```

---

## Technical Peer

Objective:

Understand implementation quality.

Typical journey:

```
Projects

↓

Architecture

↓

ADRs

↓

Repositories

↓

Articles
```

---

## AI Agent

Objective:

Efficiently extract structured engineering knowledge.

Typical journey:

```
Semantic Metadata

↓

Structured Content

↓

Architecture

↓

Engineering Ledger

↓

Project Relationships

↓

Referenced Documentation
```

Human navigation and AI discovery share the same authoritative information rather than maintaining separate content.

---

# 8. Discoverability Principles

The platform is designed for both human navigation and machine interpretation.

Information should remain:

- logically organized
- semantically meaningful
- consistently named
- internally linked
- easily searchable
- structurally predictable

Machine-readable metadata, structured documents, and semantic relationships enhance discoverability without becoming separate sources of truth.

---

# 9. Information Governance

The architecture follows these governance rules.

- Every information domain has a single owner.
- Information exists only once.
- Cross-domain references replace duplication.
- New domains require clear business responsibility.
- Navigation reflects information relationships rather than implementation.
- Major structural changes should be supported through architectural documentation before implementation.

---

# 10. Scalability

The architecture supports future expansion without structural redesign.

Potential future additions include:

```
Projects
    Categories
    Open Source
    Live Metrics
    Engineering Analytics

Architecture
    System Topology
    Domain Models
    Deployment Architecture
    Security Architecture

Engineering Ledger
    Timeline
    Architecture Evolution
    Decision Maps
    Technical Debt Log

Writing
    Tutorials
    Research
    RFCs
    AI Engineering

Skills
    Technology Roadmaps
    Learning History
```

These additions strengthen existing domains instead of introducing unnecessary top-level sections.

---

# 11. Relationship with Product Vision and ADRs

This Information Architecture operationalizes the Product Vision by defining how information is structured and consumed.

It is supported by:

- Product Vision
- Product Goals
- User Personas
- ADR-001 — Feature-First Architecture
- ADR-002 — Next.js App Router
- ADR-003 — pnpm Package Manager
- ADR-004 — AI-First Development Workflow

If implementation decisions conflict with this Information Architecture, the implementation should be reconsidered rather than silently changing the information model.

---

# 12. Summary

The Information Architecture establishes the long-term organizational model for the AI-First Engineering Portfolio Platform.

Rather than functioning as a collection of isolated pages, the platform operates as a connected engineering knowledge system where projects, architecture, documentation, experience, and technical reasoning reinforce one another.

By combining clear ownership, progressive disclosure, semantic discoverability, and strong governance, the architecture enables efficient evaluation by both human reviewers and AI systems while remaining maintainable as the platform grows over time.