---
title: Security and trust
---
Know your trust anchors.
## Trust
In the end security relies on trust. A solution will most likely have several different
trust anchors such as:
* Identity provider
* CA in a PKI based solution.
* Peer in web of trust.
* Identity card issuer
* Co-worker at work
* Etc

Principles
* There should be a determined set of trust anchors 
* There should be a lowest level of acceptable trust
* There should be a chain of trust from a trust anchor
* Chain of trust should verifiable
* Broken chains or chains that don't meet criteria should not be trusted

### Segregation of duties
The concept of having more than one person required to complete a task.
In business the separation by sharing of more than one individual in one single task is an internal control intended to prevent fraud and error.

### Separation of concerns
Encapsulation can limit the blast radius and provide a more manage parts. 
Separation of concerns can among others be applied to areas (domains), tasks, systems, components and phases.

Principles:
* Apply separation between developmment, test and production systems.
* Apply separation between areas and teams.
* Don't share components between zones.
* Don't share components between teams.
* Have clear boundaries and controls between sepearated parts.
