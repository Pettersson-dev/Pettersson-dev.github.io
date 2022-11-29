# Background
Today more and more of the world becomes digital and as the systems grow in numbers so does the need for integration.

# API based integration
API based integration
## Definition

## Principles

## Rationale
Low threshold to get started.
Integration decentralisation allows integrations to scale

## Risks
API based integration tends to be point to point. The risk with the approach is that the number of integrations become to many to keep track of and visualize. Moreover api integrations require temporal coupling and must have the abillity to handle load.

# Event based integration
Event based integration addresses the concerns of API based integration when it comes to temporal coupling. Event based systems also tend to have lower coupling between systems as topics can be made generic.

## Rationale
The information is freed from system of records meaning they don't need to evovle their capabilitites to integrate. 
Information is made available for use without impacting systems of records.
Loose coupling of systems.
Information and data becomes system agnostic.
Data can be preserved over time.


## Risks
Event driven architectures are more complex than point to point

# Information hub based integration
Information hub integration is based or rather information is the most important thing in the organisation. 
## Rationale
When the domain is information and report driven and the system of records don't have the abillity to integrate.
There is no or litle point to point integrations.

## Risks
Centralised integration t