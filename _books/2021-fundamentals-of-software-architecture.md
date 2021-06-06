---
layout: post
year: 2021
title: "Fundamentals of Software Architecture - Neal Ford, Mark Richards"
---

# 1. Introduction

Expectations of an architect: Make architecture decisions, Continually analyze the architecture, keep current with latest trends, ensure compliance with decisions, diverse exposure and experience, have business domain knowledge, possess interpersonal skills, understand and navigate politics.

> Everything in software architecture is a trade-off

> Why is more important than how

# 2. Architectural thinking

Architect is responsible for the architecture (Characteristics, Style, Component Structure), the developer is responsible for design (Classes, User Interfaces, Creating and testing source code)

> "Unlike a developer, who must have a significant amount of technical depth to perform their job, a software architect must have a significant amount of technical breadth to think like an architect and see things with an architecture point of view."

# 6. Measuring and Governing Architecture Characteristics

Cyclomatic Complexity
Tools: Crap4J

Fitness functions - mechanism that provides an objective integrity assessment of some architecture characteristic. E.g. cyclomatic dependencies, persistence layer being accessed from the presentation layer.
Tools: ArchUnit

# Chapter 10: Layered Architecture Style

A client request goes through multiple layers to reach the Database, or another service. The presentation does not directly interact with the persistence layer.

Client <-> Presentation Layer <-> Business Layer <-> Persistence Layer <-> Database Layer

> The layered architecture is a technically partitioned architecture (as opposed to a domain-partitioned architecture). Groups of components, rather than being grouped by domain (such as customer), are grouped by their technical role in the architecture (such as presentation or business).

> Overall cost and simplicity are the primary strengths of the layered architecture style. Being monolithic in nature, layered architectures don’t have the complexities associated with distributed architecture styles, are simple and easy to understand, and are relatively low cost to build and maintain

# Chapter 11: Pipeline Architecture Style

> Most developers know this architecture as this underlying principle behind Unix terminal shell languages, such as Bash and Zsh.

Simple to understand, low cost to build and to maintain.

# Chapter 12. Microkernel Architecture Style

A core system and plug-in components. Runtime plugins can be added or removed at runtime. Meaning one does not need to redeploy the core or other plug-ins. Uses Frameworks such as OSGi for Java. The plugins share functionality of the core.

Examples would be Eclipse IDE, Jenkins, Chrome or Firefox.

> simplicity and overall cost are the main strengths of the microkernel architecture style, and scalability, fault tolerance, and extensibility its main weaknesses.

# Chapter 13. Service Based Architecture Style

> separately deployed user interface, separately deployed remote coarse-grained services, and a monolithic database.

Each service encompasses a particular domain -> Well suited for domain-driven design.

> service-based architecture is a good choice for achieving a good level of architectural modularity without having to get tangled up in the complexities and pitfalls of granularity

# Chapter 14. Event-Driven Architecture Style

> made up of decoupled event processing components that asynchronously receive and process events

> Overall simplicity and testability rate relatively low with event-driven architecture, mostly due to the nondeterministic and dynamic event flows typically found within this architecture style.

# Chapter 15. Space based architecture style

> space-based architecture style is specifically designed to address problems involving high scalability, elasticity, and high concurrency issues. It is also a useful architecture style for applications that have variable and unpredictable concurrent user volumes.

# Chapter 17. Microservices Architecture

Goal is decoupling. The services model their bounded context, meaning their specific domain or or workflow. Each service contains the necessary parts to operate independently, e.g. databases.

Drawbacks are the distributed nature, which include network latency, resilience requirements and security checks between the services to name some.

API-Layer for service discovery and as an entry point for clients.

# Chapter 19. Architecture Decisions

Helps to guide development teams in making the right technical choices.

## 

# Self Assessment Questions

## Chapter 24

The 20 minute rule states how an architect should spend at least 20 minutes a day learning about his field. What are up and coming technologies or tools? Even with a quick dive into a specific topic.

The ThoughtWorks technology radar presents technologies ThoughtWorks, a technology consultancy, suggests to adopt or to stop using. The radar is divided into four sections. Techniques, such as Design Systems, Remote Mob Programming or GitOps. Tools such as Sentry, Terratest or AWS CodePipeline. Platforms such as AWS CDK, Pulumi or Azure Machine Learning. Language and Frameworks such as Combine, Angular Testing Library or Flutter for Web.

Architects should maximize breath of knowledge, as is their scope of impact in an organization. Depth of knowledge is required of specialists that spend time on design instead of architecture. Meaning developers that write and test code in their specific areas of expertise.

