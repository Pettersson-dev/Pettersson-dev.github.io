---
title: "Information and integration strategy"
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/nate-grant-QQ9LainS6tI-unsplash.jpg
  caption: "Photo credit: [**Photo by Nate Grant on Unsplash**](https://unsplash.com)"
excerpt: "Integration between different machines over a communication medium"
---

# Background
Today more and more of the world becomes digital and as the systems grow in numbers so does the need for integration.

# API based integration
API based integration decouples the information from the database implementation. There are many styles of APIs from action based implementations such as gRPC and SOAP to resource based GraphQL and Restful APIs.
API based integration can either be centralized with a dedicated integration team or decentralised with an integration platform or framework.

## Principles
- Smart endpoints and dumb pipes
- Decouple information from implementation (eg. databases)
- Clients needs to handle server unavailabillity

## Rationale
Low threshold to get started.
Integration decentralisation allows integrations to scale.
Decouple information from databases.

## Risks
API based integration tends to be point to point. The risk with the approach is that the number of integrations become to many to keep track of and visualize. Moreover api integrations require temporal coupling and must have the abillity to handle load.

# Event based integration
Event based integration addresses the concerns of API based integration when it comes to temporal coupling. Event based systems also tend to have lower coupling between systems as topics can be made generic.

## Principles
- Smart endpoints and dumb pipes.
- Reusable topics

## Rationale
When the domain wants to decouple it self from the sourrounding systems.
- Low temporal coupling
- No point to point if topics is used.
- Transactional boundaries can be enforced.
- Near realtime updates possible.

## Risks
Event driven architectures are more complex than point to point.
Data consistency could be an issue.

# Information hub based integration
With the information hub the integration changes focus from system to information. The hub acts as master for the data and all information flows as rivers into the data lake. The raw data is enriched and processed making it available for consumption. 

## Principles
- All information is processed in the information hub
- The information hub contains masterdata
- Data is ingested from system of records to the information hub
- No processing is done outside the datahub

## Rationale
When the domain is information and report driven and the system of records don't have the abillity to integrate.
- There is no or litle point to point integrations.
- Information is freed for reports and integrations. 
- The information is freed from system of records meaning they don't need to evovle their capabilitites to integrate. 
- Information is made available for use without impacting systems of records.
- Loose coupling of systems.
- Information and data becomes system agnostic.
- Data can be preserved over time.

## Risks
Centralised integration can be a bottleneck.
Missing data governance could make data unusable.
Data processing and storage comes with a cost.
