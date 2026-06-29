---
adr: ADR-002
title: Adopt Next.js App Router
status: Accepted
date: 2026-06-30
deciders: [Principal Software Architect]
project: ai-first-engineering-portfolio-platform
stack: [Next.js App Router, React, TypeScript, Tailwind CSS]
tags: [architecture, routing, rendering, framework]
supersedes: null
superseded_by: null
related: [ADR-001]
---

# ADR-002: Adopt Next.js App Router

## Status

**Accepted** — the App Router is the routing and rendering foundation for this platform. All
routes, layouts, and data-fetching boundaries are implemented against it. Any deviation requires
a new ADR.

## Context

This platform is built on Next.js, React, TypeScript, and Tailwind CSS, under a feature-first
folder structure (ADR-001), documentation-first engineering, and AI-assisted development
practices. The project is currently maintained by a single engineer but is expected to scale to a
small team, and is expected to remain in active extension for years rather than be treated as a
short-lived portfolio artifact.

Next.js currently ships two routing paradigms: the legacy **Pages Router** and the **App
Router**, introduced as the framework's forward path and now its primary point of investment.
The choice of router is foundational: it determines the rendering model (Server Components vs.
client-only components), the data-fetching model, the layout composition model, and how cleanly
routing can sit on top of the feature-first structure already adopted. Because this decision
touches every route in the application, it is the second-most foundational structural choice in
the system after ADR-001, and reversing it later would be a near-total rewrite of the
presentation layer.

The App Router also aligns well with AI-assisted development by providing predictable routing conventions and clear server/client boundaries, reducing ambiguity for both human engineers and AI coding agents.

## Problem Statement

The platform needs a routing and rendering foundation that:

- Composes cleanly with feature-first folders rather than forcing route structure to dictate
  feature structure.
- Supports a rendering model capable of serving content efficiently (a portfolio platform is
  read-heavy and benefits from server rendering and static generation) without locking every
  page into client-side-only execution.
- Remains the framework's actively maintained, long-term-supported routing model, since adopting
  a path the framework vendor is deprecating would directly contradict the project's long-term
  extensibility principle.
- Is tractable for AI-assisted development: predictable conventions for where data fetching,
  layouts, and route-level logic live reduce ambiguity for both human and AI contributors
  reasoning about a given route.

The question this ADR answers: **which Next.js routing paradigm should the application standardize
on, and why, given the project's structural and long-term-evolution constraints?**

## Decision

We will build the application exclusively on the **Next.js App Router** (`app/` directory),
using React Server Components as the default component type, with Client Components used
deliberately and only where interactivity requires them.

Route segments under `app/` are treated as a thin composition layer: they import from
feature-first modules (ADR-001) to assemble pages and layouts, and they do not contain
feature business logic themselves. Data fetching is colocated with the route or feature that
needs it, using the App Router's native server-side data-fetching model rather than introducing
a separate client-side fetching layer as the default pattern.

## Alternatives Considered

**1. Pages Router**
Next.js's original routing model, using `pages/` and `getServerSideProps`/`getStaticProps`.
Rejected because it is in maintenance mode rather than active investment: new framework
capabilities (Server Components, streaming, nested layouts, parallel and intercepting routes)
are built for the App Router first, and in several cases exclusively. Standardizing on the Pages
Router would mean deliberately building on a path with a declining trajectory, directly
conflicting with the project's long-term extensibility principle. It also lacks native nested
layouts, which would push more structural logic into ad hoc component composition rather than
the framework's own primitives.

**2. Client-side-only React (e.g., Vite + React Router)**
Rejected for a content-oriented portfolio platform: it would default every page to client-side
rendering, increasing time-to-first-byte-to-meaningful-content and SEO cost for what is
substantially a read-heavy, content-driven application. It would also forfeit the App Router's
server/client component split, which is directly useful for this platform's shape: most content
is static or server-derived, and only specific islands (contact forms, interactive widgets)
need client interactivity.

**3. A different meta-framework (e.g., Remix)**
Rejected primarily on ecosystem and tooling fit rather than capability: the project's AI-assisted
workflow, deployment target, and existing conventions are oriented around the Next.js ecosystem,
and introducing a second framework's mental model would add migration cost without a
corresponding capability gain over the App Router for this platform's requirements. This is a
defensible choice in other contexts; it is not the better fit here.

**4. Hybrid Pages Router + selective App Router adoption**
Running both routers simultaneously during a gradual migration. Rejected as a standing
architecture: this is a legitimate transitional pattern in large existing codebases with
significant Pages Router investment, but this platform has no such legacy investment to
preserve. Adopting a hybrid model here would introduce two routing conventions, two data-fetching
models, and ongoing decision overhead ("which router does this route belong to") with no
corresponding benefit.

## Trade-offs

Standardizing on the App Router accepts the following costs:

- **Steeper initial conceptual model.** Server Components, the server/client boundary, and
  streaming are less immediately familiar than a single client-rendered model. This is a
  one-time learning cost rather than a recurring one.
- **Less ecosystem maturity than the Pages Router** in some third-party libraries, which
  occasionally still assume a client-only rendering context. This requires periodic verification
  of library compatibility when adding dependencies.
- **More architectural discipline required at the server/client boundary.** Misplacing
  interactivity in a Server Component, or unnecessarily marking a component as a Client
  Component, are easy mistakes without an explicit convention (addressed below).

What this decision optimizes for: alignment with the framework's long-term direction, an
appropriate rendering model for a content-heavy platform, and a routing layer that composes
cleanly with feature-first folders rather than competing with them.

## Consequences

**Maintainability.** Route files stay thin and declarative; business logic remains inside
features, so changes to a feature rarely require touching routing code and vice versa.

**Scalability (rendering).** Server Components let most of the application render on the server
by default, with client-side JavaScript reserved for components that genuinely need it — keeping
the client bundle proportional to actual interactivity, not to total page count.

**Scalability (team).** Nested layouts and route groups let multiple contributors work on
different sections of the route tree with minimal structural collision.

**AI-assisted development.** A single, consistent routing and data-fetching convention reduces
the decision space an AI agent must consider when adding or modifying a route, and the
server/client split gives a clear, checkable rule ("does this need interactivity?") for where new
code should live.

**Onboarding.** New contributors learn one routing model and one data-fetching pattern, rather
than reconciling two coexisting paradigms.

**Long-term evolution.** Building on the framework's actively developed router means the
platform inherits future App Router capabilities (e.g., improved streaming, partial
prerendering) without a migration; the inverse would not be true on the Pages Router.

**Accepted risk.** Server/client boundary mistakes are possible without enforcement. This is
mitigated by the implementation guidelines below and by code review attention at feature
boundaries.

Treating the App Router as a thin delivery layer further reinforces the Feature-First Architecture established in ADR-001 by preventing business logic from becoming coupled to routing concerns.

## Implementation Guidelines

- Components default to Server Components; `"use client"` is added only when a component
  requires browser-only APIs, state, or event handlers, and the directive is placed at the
  smallest reasonable boundary, not at the top of a large subtree.
- Route segments (`app/**`) compose feature exports; they do not implement feature business
  logic directly.
- Data fetching happens as close to where the data is used as possible, using the App Router's
  server-side fetching model, rather than centralizing all fetching behind a generic client-side
  data layer.
- Shared layouts are expressed using nested `layout.tsx` files rather than re-implemented
  per-page wrapper components.
- Any use of a Client Component for something achievable on the server is treated as a review
  finding, not a stylistic preference.

## Future Considerations

As the App Router's streaming and partial prerendering capabilities mature, route-level
rendering strategy (static, dynamic, streamed) should be revisited per route rather than
decided once globally. If the platform later requires a rendering model the App Router cannot
support, that would warrant a new ADR rather than a silent reversal of this one.

## References

- ADR-001: Adopt Feature-First Architecture
- Architect persona definition: `prompts/agents/architect.md`