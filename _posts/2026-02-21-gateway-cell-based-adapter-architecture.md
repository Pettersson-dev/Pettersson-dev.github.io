---
title: "Gateway, Cell-Based and Adapter Architecture"
excerpt: "From edge control to protected domain logic in modern enterprise platforms."
categories:
  - Enterprise Architecture
  - Platform Architecture
  - Distributed Systems
tags:
  - Architecture Patterns
  - Cell-Based Architecture
  - API Gateway
  - Adapters
  - Ports and Adapters
  - Cloud Architecture
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/nate-grant-QQ9LainS6tI-unsplash.jpg
  caption: "Photo credit: [**Photo by Nate Grant on Unsplash**](https://unsplash.com)"
author_profile: true
toc: true
toc_sticky: true
---

## Why this architecture exists

Many enterprise platforms struggle not because of missing tools or frameworks, but because:

- Failures propagate across the entire system  
- Shared platforms become bottlenecks  
- Integrations leak into core logic  
- Teams cannot change independently  

This architecture combines **client/server**, **gateway**, **cell-based decomposition**, and **adapter-based design** to explicitly address those problems.

---

## High-level structure

At a system level, the architecture is structured as:


```

Clients
   ↓
Gateway (API Gateway / BFF)
   ↓
Cells (independent runtime units)
   └─ Layered + adapter-based internal design

```

Each architectural pattern addresses a different concern:

- **Client/Server** – interaction model  
- **Gateway** – edge control and protection  
- **Cells** – fault isolation and scalability  
- **Adapters** – decoupling from dependencies  

Together, they form a coherent and evolvable platform architecture.

---

## Client / server interaction

Clients (web, mobile, partner systems, internal tools) interact with the platform using **stable, versioned APIs**.

Key principles:

- Clients never access internal services or data directly  
- Internal topology is hidden  
- Contracts evolve independently of implementation  

This keeps client change cheap and predictable.

---

## Gateway architecture

The gateway acts as the **single controlled entry point** into the platform.

Typical responsibilities include:

- Authentication and authorization  
- Rate limiting and throttling  
- Request routing  
- API versioning  
- Protocol translation  
- Observability (logging, metrics, tracing)

The gateway enforces **policy**, not **business behavior**.

> No business logic belongs in the gateway.

In larger systems this is often implemented as:
- A shared API Gateway  
- One or more BFFs (Backend for Frontend)

---

## Cell-based architecture

The platform is decomposed into **cells** — autonomous runtime units that include:

- Application services  
- Domain logic  
- Data storage  
- Integration adapters  
- Capacity and failure boundaries  

Each cell:

- Is deployed independently  
- Scales independently  
- Fails independently  

Traffic is routed to cells based on criteria such as tenant, geography, customer segment, or partition key.

This design turns system-wide failures into **contained, local incidents**.

---

## Internal structure of a cell

Inside each cell, a **layered architecture** is combined with **ports and adapters**.

```
Inbound Adapters
↓
Application Layer
↓
Domain Layer
↓
Outbound Adapters

```

This structure is intentional and enforced.

---
## When layered architecture turns into a mess

I’ve seen layered architecture go wrong more times than I can count.

On paper, the boundaries look clean.  
In reality, they often blur fast:

- Controllers start calling repositories directly  
- Application layers accumulate business rules  
- Domain logic leaks into adapters “just for convenience”  
- Infrastructure concerns creep upward, one shortcut at a time  

What started as a layered architecture quietly turns into a **layered illusion**.

Everyone still talks about “the domain layer”, but no one can really point to where it begins or ends.

### Common architectural smells — and the rules they violate

When layered architecture starts to decay, the symptoms are rarely dramatic.  
What I’ve learned is that every “small” shortcut almost always breaks a very specific architectural rule.

Making those rules explicit is what turns architecture from intention into constraint.

---

**Smell: Controllers calling repositories directly**  
**Violated rule:** *All business interactions must go through application use cases.*

This bypasses orchestration, authorization, and consistency boundaries.  
It turns the UI or API layer into an accidental application layer.

---

**Smell: “Just this once” logic in adapters**  
**Violated rule:** *Adapters translate — they do not decide.*

Inbound and outbound adapters exist to isolate the core from the outside world.  
Once business rules appear here, the direction of dependency is already broken.

---

**Smell: An application layer that keeps growing**  
**Violated rule:** *The application layer orchestrates behavior; it does not contain it.*

When business rules accumulate here, the domain is being hollowed out and the model loses meaning.

---

**Smell: Domain objects depending on frameworks or SDKs**  
**Violated rule:** *The domain must be technology-agnostic.*

Framework annotations, persistence concerns, or vendor SDKs in the domain are a direct breach of this rule — and they make change expensive later.

---

**Smell: Shared utility packages used everywhere**  
**Violated rule:** *Reuse must not create hidden coupling.*

Utilities that “everyone depends on” quickly become informal shared infrastructure with no clear ownership or lifecycle.

---

**Smell: Developers unsure where new logic belongs**  
**Violated rule:** *Every change must have an obvious home.*

When placement becomes a discussion rather than a decision, boundaries are no longer doing their job.

---

**Smell: Architecture diagrams that look right but don’t match the code**  
**Violated rule:** *Architecture must be enforced, not just documented.*

When diagrams and reality diverge, the architecture has already lost authority — even if no one says it out loud.

---

None of these violations are dramatic on their own.  
Together, they are a reliable signal that the system is no longer protecting its core.
---

## The problem isn’t layers — it’s missing boundaries

The issue isn’t that layered architecture is flawed.  
The issue is that **layers without ownership and enforcement are just naming conventions**.

Without clear rules:

- Layers become suggestions instead of constraints  
- Violations feel harmless in the moment  
- Short-term delivery pressure overrides structure  
- The architecture erodes silently  

By the time the pain is visible, the boundaries are already gone.

 ## Architecture principles I rely on

When I say “layers” or “adapters”, I’m not talking about boxes in a diagram.  
I’m talking about constraints that make the *right thing* easy and the *wrong thing* uncomfortable.

These are the principles I use to keep boundaries real.

### 1. Use cases are the only entry point to business behavior
All business interactions must be expressed as **application use cases** (commands/queries).  
No controller, UI, scheduler, or consumer should reach into repositories or domain objects directly.

**Implication:** request handlers should be thin; orchestration lives in the application layer.

---

### 2. Adapters translate — they do not decide
Adapters exist to **isolate** the core from protocols, vendors, and transport concerns.  
They can validate, map, normalize, and enrich context — but they must not contain business rules.

**Implication:** if business logic appears in an adapter, the boundary is leaking.

---

### 3. The application layer orchestrates; the domain contains behavior
The application layer coordinates work: transactions, authorization, consistency, workflows.  
Business rules and invariants live in the **domain model**.

**Implication:** if the application layer keeps growing, it’s usually absorbing domain behavior that should be modeled explicitly.

---

### 4. The domain is pure and technology-agnostic
The domain must not depend on frameworks, persistence, SDKs, or vendor APIs.  
If the domain can’t be unit tested without infrastructure, it’s not isolated enough.

**Implication:** keep annotations, mappers, repositories, and clients outside the domain.

---

### 5. Reuse must not create hidden coupling
Shared libraries are allowed, but only when they do not become a “global dependency magnet”.  
If everyone depends on a utility package, it needs ownership, versioning, and a lifecycle — just like a product.

**Implication:** prefer duplication over accidental coupling when ownership is unclear.

---

### 6. Every change must have an obvious home
A healthy architecture makes it obvious where new logic belongs.  
If engineers debate placement, boundaries are unclear or principles aren’t enforced.

**Implication:** clarify responsibilities until “where does this go?” becomes a non-question.

---

### 7. Architecture is enforced, not just documented
Diagrams are useful, but they don’t create architecture — constraints do.  
If the code and the diagram diverge, the code wins, and the architecture has already lost authority.

**Implication:** enforce boundaries via reviews, tests, and automation (where possible).

---

### 8. Own boundaries through cells
Cells exist to make ownership and failure boundaries explicit.  
A cell owns its runtime, dependencies, and data. Crossing boundaries must be intentional and visible.

**Implication:** if teams can’t evolve independently, the cell boundaries are wrong (or not real).

---

---

## Why I combine layers with adapters

This is why I don’t rely on layers alone.

Adapters make boundaries **explicit and enforceable**:

- Inbound adapters define how the system may be called  
- Outbound adapters define how the system may depend on others  
- Everything in between is forced to stay honest  

It becomes obvious when something is in the wrong place — and just as obvious when someone tries to bypass a layer.

---

## Cells make boundary violations visible

Cell-based architecture adds another level of pressure in the right direction.

When each cell owns its runtime, data, and dependencies:

- Boundary violations hurt faster  
- Coupling becomes visible immediately  
- “Quick fixes” stop being quick  

You can’t casually reach across layers or cells without feeling the cost.

---

## The real lesson

Layered architecture isn’t enough by itself.

Without explicit boundaries, enforcement, and ownership, layers decay into a mess — not because people are careless, but because the system doesn’t push back.

Good architecture should make the *right thing* the easy thing, and the *wrong thing* uncomfortable.

That’s the difference between a diagram and a design.

## Inbound adapters

Inbound adapters translate external interaction models into internal use cases.

Examples include:

- REST controllers  
- GraphQL resolvers  
- Event consumers  
- Batch job handlers  

They handle protocols, validation, mapping, and context propagation — but contain **no business logic**.

---

## Application layer

The application layer orchestrates use cases.

Its responsibilities include:

- Coordinating domain operations  
- Managing transactions  
- Enforcing authorization rules  
- Handling consistency boundaries  

The application layer is procedural and explicit, but deliberately thin.

---

## Domain layer

The domain layer contains the **core business logic**:

- Entities and aggregates  
- Value objects  
- Domain services  
- Business rules and invariants  

The domain layer has **no dependency on infrastructure, frameworks, or vendors**.

This is the layer the architecture is designed to protect.

---

## Outbound adapters

Outbound adapters connect the application and domain layers to external systems such as:

- Databases  
- Legacy platforms  
- External APIs  
- Message brokers  

They handle protocol translation, data mapping, retries, and resilience patterns.  
The domain remains unaware of how integration and persistence are implemented.

---

## Data ownership and integration

Each cell owns its data.

- No shared databases between cells  
- No implicit coupling  
- Explicit contracts for communication  

Cross-cell interaction is:
- Event-driven where possible  
- API-based where necessary  

This enforces ownership, autonomy, and clarity.

---

## Governance model

The architecture supports **guardrail-based governance**:

- Central architecture defines principles, standards, and boundaries  
- Teams own cells, internal design decisions, and deployment cadence  

Architecture becomes an enabler for change rather than a constraint.

---

## Summary

This architecture combines client/server interaction with gateway-based edge control, cell-based system decomposition, and adapter-driven internal design. Gateways protect and route traffic, cells limit blast radius and enable independent evolution, and adapters isolate core domain logic from external dependencies. The result is a scalable, resilient platform architecture that supports autonomy without sacrificing architectural integrity.