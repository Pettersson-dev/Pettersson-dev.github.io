---
title: "Information and Types of Integration"
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/nate-grant-QQ9LainS6tI-unsplash.jpg
  caption: "Photo credit: [**Nate Grant on Unsplash**](https://unsplash.com)"
excerpt: "An overview of common integration styles — APIs, events, and information hubs — with their principles, rationale, and risks."
classes: wide
tags:
  - Integration
  - Architecture
  - Security
---

# Background  
As the world becomes increasingly digital, the number of systems within organizations continues to grow. With this growth comes a stronger need for **integration** — the ability for systems to exchange information reliably, securely, and at scale.  

There are many integration styles. This post highlights three of the most common:  
1. **API-based integration**  
2. **Event-based integration**  
3. **Information hub-based integration**  

Each style comes with its own strengths, trade-offs, and risks. In practice, enterprises usually apply a **blend of all three**, depending on the business domain and use case.  

# API-based integration  
API-based integration decouples information from database implementations. APIs expose data and functionality through standardized interfaces, and can be designed in different styles — from **action-based** (SOAP, gRPC) to **resource-based** (REST, GraphQL).  

API integration can be delivered **centrally** (by a dedicated integration team) or **decentrally** (through a shared integration platform or framework).  

## Principles  
- Smart endpoints and dumb pipes  
- Decouple information from underlying implementations (e.g., databases)  
- Clients must handle server unavailability gracefully  

## Rationale  
- Low threshold to get started  
- Supports decentralization and scaling of integration work  
- Reduces dependency on database structures  

## Risks  
- Easily becomes **point-to-point sprawl** if unmanaged  
- Requires **temporal coupling** — both systems must be available at the same time  
- Load handling and retry logic are often pushed to clients  

## Example use cases  
- Customer-facing portals calling product, pricing, and order APIs  
- Mobile apps integrating with back-end services  
- Partner integration through exposed APIs  

# Event-based integration  
Event-based integration addresses the **temporal coupling problem** in APIs. Instead of synchronous requests, systems publish and subscribe to events on topics. This reduces direct dependencies between systems and promotes looser coupling.  

## Principles  
- Smart endpoints and dumb pipes  
- Use reusable, well-defined topics  

## Rationale  
- Decouples domains from surrounding systems  
- Low temporal coupling — systems don’t need to be online simultaneously  
- Removes point-to-point integration if topics are shared  
- Enables near real-time updates  
- Supports transactional boundaries  

## Risks  
- More complex than simple point-to-point APIs  
- Data consistency challenges (eventual consistency)  
- Requires strong monitoring and governance to avoid “event spaghetti”  

## Example use cases  
- E-commerce order events triggering warehouse and billing processes  
- Customer data change events (e.g., GDPR updates) published to multiple consuming systems  
- Real-time IoT telemetry streams  

# Information hub-based integration  
In an **information hub** model, integration is centered on **information** rather than system-to-system flows. The hub acts as a master for shared data — often as a **data lake or data warehouse** — ingesting, enriching, and distributing information for reporting and consumption.  

## Principles  
- All information flows through the hub  
- The hub contains master data  
- Data is ingested from systems of record into the hub  
- No processing is done outside the hub  

## Rationale  
- Reduces point-to-point integrations  
- Frees information for reporting and downstream integration  
- Allows systems of record to remain stable without adapting for integration needs  
- Preserves data over time  
- Loosely couples consuming systems from sources  

## Risks  
- Centralization may create bottlenecks  
- Without strong data governance, data quality can degrade  
- Processing and storage costs can become significant  
- Risk of turning into a “data swamp” if design and stewardship are weak  

## Example use cases  
- Enterprise data warehouse supporting financial reporting  
- Data lake aggregating telemetry from diverse platforms  
- Master data hub ensuring consistent customer or product information  

# Choosing the right integration style  
- **Use APIs** when you need **transactional, request/response** communication between systems.  
- **Use events** when you need **real-time, decoupled notifications** and want to scale to many subscribers.  
- **Use an information hub** when your focus is **analytics, reporting, and long-term data retention**.  

In reality, most organizations combine these: an API exposes product information, events notify downstream systems of changes, and a hub stores the full product history for analysis.  

# Governance considerations  
Integration is not just technical — it must be governed as an **enterprise capability**. Key practices include:  
- **API catalogues** to avoid duplication and shadow integrations  
- **Event schemas and versioning** to maintain contract clarity  
- **Data stewardship** for information hubs to ensure data quality and compliance  
- **Security controls** (authentication, encryption, monitoring) across all integration styles  

# Closing Thoughts  
There is no single “best” integration style. Most organizations use a **mix**:  
- APIs for **operational transactions**,  
- Events for **decoupling and real-time updates**, and  
- Information hubs for **analytics and reporting**.  

The right choice depends on the domain, the maturity of the teams, and the strategic importance of data. What matters most is to **apply the right style for the right problem — and govern integration as a capability, not just a technical detail**.  