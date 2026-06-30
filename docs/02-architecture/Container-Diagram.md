---
id: C4-L2
title: Container Diagram (C4 Level 2)
status: Approved
version: 1.0
owner: Vaikunth Prajapati
reviewer: ChatGPT
---

# C4 Model — Level 2: Container Diagram

## Purpose

This document decomposes the AI-First Engineering Portfolio Platform into its major logical containers.

It defines the responsibilities, ownership, interfaces, and relationships between the platform's primary runtime containers while remaining independent of implementation technologies.

This document expands the System Context (C4 Level 1) without describing internal components.

---

# Scope

This document includes:

- logical containers
- responsibilities
- interfaces
- ownership
- interactions
- architectural boundaries

This document excludes:

- deployment
- infrastructure
- source code
- UI implementation
- folder structure
- component design

---

# Architectural Principles

- One container owns one primary responsibility.
- Every piece of information has a single source of truth.
- Containers communicate only through defined interfaces.
- AI interactions are mediated through dedicated services.
- Presentation remains independent from business logic.
- Documentation remains a first-class architectural capability.

---

# Containers

## 1. Presentation Application

### Responsibility

Provides the public interface for recruiters, engineers, and visitors.

### Owns

- navigation
- presentation
- user interactions

### Interfaces

- receives requests from users
- retrieves information from the Content Repository
- forwards AI requests to the AI Collaboration Service

---

## 2. Content & Governance Repository

### Responsibility

Maintains all authoritative platform information.

### Owns

- Product documentation
- Architecture documentation
- ADRs
- Projects
- Articles
- Resume
- Skills
- Experience
- Metadata

### Interfaces

- provides read access to Presentation
- provides structured content to AI Collaboration

---

## 3. AI Collaboration Service

### Responsibility

Acts as the controlled gateway between the platform and external AI providers.

### Owns

- prompt execution
- AI context preparation
- semantic retrieval
- AI response validation

### Interfaces

- communicates with external AI providers
- reads platform documentation
- returns validated responses

---

## 4. Engagement Service

### Responsibility

Processes communication requests.

### Owns

- contact requests
- communication routing

### Interfaces

- receives requests from Presentation
- forwards communication to external providers

---

# Container Relationships

| Source | Target | Purpose |
|---------|---------|---------|
| Presentation | Repository | Retrieve content |
| Presentation | AI Collaboration | AI interactions |
| Presentation | Engagement | Contact requests |
| AI Collaboration | Repository | Read platform knowledge |
| AI Collaboration | External AI | AI reasoning |
| Engagement | Communication Provider | Deliver messages |

---

# External Systems

The platform integrates with:

- GitHub
- LinkedIn
- Resume downloads
- Communication providers
- AI Providers (OpenAI, Anthropic, Google Gemini)

These systems remain outside the platform boundary.

---

# High-Level Interaction Flow

### Human User
Visitor

↓

Presentation Application

↓

Content Repository

↓

Response


### AI Workflow


User

↓

Presentation

↓

AI Collaboration

↓

External AI

↓

Validated Response

↓

Presentation


### Contact Flow


Visitor

↓

Presentation

↓

Engagement Service

↓

Communication Provider


---

# Container Diagram

```text
                 +---------------------------+
                 |      Presentation         |
                 +-------------+-------------+
                               |
          +--------------------+--------------------+
          |                                         |
          v                                         v
+-------------------------+             +-------------------------+
| Content Repository      |             | AI Collaboration        |
|                         |             |                         |
+------------+------------+             +------------+------------+
             |                                         |
             |                                         |
             |                              External AI Providers
             |
             v
+-------------------------+
| Engagement Service      |
+------------+------------+
             |
             v
 Communication Provider

 Architectural Boundaries

The platform owns:

platform content
documentation
engineering knowledge
governance
AI orchestration
user interactions

The platform does not own:

GitHub
LinkedIn
AI providers
communication infrastructure
Alignment

This document complements:

Product Vision
Product Goals
User Personas
Information Architecture
Software Architecture
C4 Level 1 – System Context
ADR-001 Feature-First Architecture
ADR-002 Next.js App Router
ADR-003 pnpm Package Manager
ADR-004 AI-First Development
Summary

The platform is composed of four primary logical containers:

Presentation Application
Content & Governance Repository
AI Collaboration Service
Engagement Service

Each container has a clearly defined responsibility, communicates through explicit interfaces, and preserves single ownership of information.

This separation improves maintainability, scalability, AI integration, and long-term architectural consistency while remaining independent of implementation technologies.