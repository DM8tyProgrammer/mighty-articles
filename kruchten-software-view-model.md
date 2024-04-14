---
title: Kruchten’s 4 + 1 views of Software Design
subtitle: Software Design Knowledge Organization Framework
description: Philippe Kruchten defined a model named 4+1 views model; it is a set of views, to express the software design efficiently.
keywords: information framework, knowledge organization, 
datePublished: '2018-03-26'
lastModified: '2024-04-14'
---

We design software, we try to describe its design and related information; The collective knowledge can be described in terms of words (document), visual (diagram), or demonstration.

In the internal description of an application, a single representation of an architectural view cannot meet the eyes of all associated various skilled stakeholders _(Management, Infrastructure Operators, Product Managers, Developers, Business Executors)_.


> Each stakeholder is only interested in a portion of the software information.

[Philippe Kruchten](https://en.wikipedia.org/wiki/Philippe_Kruchten) defined **4+1 Views Model** to capture the description of Software Implementation or Architecture into_multiple concurrent views_.
A ViewModel is a _subpart of the overall software implementation description knowledge_ and targets a subset of the audience.

## 4+1 Views Model

4+1 views model is an _information organization framework_; it includes logical, process, development, physical layout and end-user perspective information of an application.


> A view is an aspect (subpart) of information.
> A notion is a way of representing information.

A single type of notation is usually used in multiple views. Views do not enforce any notion.

![4+1 views model](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*QmXrPGG7uZCNIQohIaJCYA.png)


### Logical View
__Logical View__ captures the _functional requirements_ of the application as decomposition of structural elements or abstractions.   
_For Developers, Engineer Managers,..._



* __Object Decomposition__ is capturing of application behavior into classes and packages. It is the base of functional analysis in case of Object-Oriented Paradigm.


> Classes and packages may be represented using UML Class Diagrams and UML Package Diagrams, respectively.


* __Data Modelling__ is the analysis of data gathering and organizing data into logical entities.

> ER diagram is a well-known notation to represent business entities and relationships.


* __System and subsystems view__ is breaking down of application into modules and arrangement of their responsibilities and relationships.

> UML Component diagram can be used.


### Process View

* __States Transition__ can be used to understand state and transition in case of a workflow-based system.

> UML State Diagram is used to represent state and state transitions.

* __Information Flow__ represents information routing from one entity to another entity.

> Data Flow Diagram (DFD), Application Prototype (UX), UML Activity, and UML Sequence diagram represent the various flow information levels.


* __Process Decomposition__ represents runtime partitioned application decomposition. It can be called Service Decomposition for network partitioned processes.

* __Non-functional aspects__ like Throughput, Availability, and Concurrency. These are easier to put in words than in diagrams.


### Development View
Development View focuses on the management of an application.   
_For Management & Developers_

* __Teams Organization__ — Roles and Responsibilities of team members


* __Development Methodology__ is a way of development workflow implementation. Agile Methodologies are popular these days.

> Tip: It is crucial to have agility than Agile Framework.

> Popular Agile Methodologies Framework: Scrum, XP, Kanban.

* __Development Standards__ — Set of guidelines & coding standards, automation tools.
> e.g. VCS System and their branching system, CI, Deployment Automation, Code Linting, Code Style
* __Test Planning__ of functionalities.
> How do you test? Automation or Manual or hybrid? What do you test? Scope of test? How do you record test step sequences?


* _Work logs and Tracker system_ to manage and track tasks.
> JIRA, Asana, Harvest are popular commercial tools.

* __Road-maps__ give ideas about deliverables.

### Physical View

Physical View represents the _deployment layout or infrastructure_ of an application. In essence, it captures hardware mapping of application components or processes.   
_For DevOps, EMs, Software Engineers_


* __Topology Architecture__ | Deployment Plan
The cluster of application instances and their places in the geography of physical or virtual machines available.
> UML deployment diagram, Network diagram are standard options.

* __System Capacity__ of the application

* __Configuration Management__

> Tip: It is essential to keep configuration out of the application and build a workflow to change the configuration to have a higher degree of observability and flexibility.


### Use Cases
Use Cases captures an _End-User Perspective_ into a systematic, logical information structure. It complements all other views and is used to validate them.    
_For Every Stakeholder_

> Stories are used for elaboration; It may be a combination of textual documents with or without UML Sequences, Actor diagrams, Prototypes.


## Closing Notes
4+1 Model Views covers much of the software if done right. It is not rigid; you can skip any view or subview that does not add any value in expressing your software design.

![4+1 view models](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*U1galJVGm4Db5J1nG-HNWw.png)

4+1 Views Model is too result-oriented. Views only represent the results of decisions that may not be evident over time or to the people who were not part of the original design discussion. This problem can be solved, including an additional view called Architecture Decision View (ADR).


## Architecture Decision View 
Architecture Decision View captures contextual knowledge behind any architectural decision. It focuses on causation and circumstances that lead to decisions.

__Architecture Decision Record (ADR)__ is one of the implementations which can be found at:
https://github.com/joelparkerhenderson/architecture_decision_record

## The Original Paper
You can also read the original paper of 4+1 Views Model at:
https://www.ics.uci.edu/~andre/ics223w2006/kruchten3.pdf

---

There are other documentation frameworks too:
* Arc 42 — https://arc42.org/overview/