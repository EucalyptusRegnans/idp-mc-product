---
title: "Hexagonal Architecture (Ports & Adapters)"
status: "Draft"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# **Hexagonal Architecture (Ports & Adapters)**

This document defines the principles of Hexagonal Architecture (also known as Ports and Adapters) as they apply to the IDP-MC System. These principles govern the internal structure of modules and the flow of dependencies within the backend application.

## 1. Core Principle
The primary goal of this architecture is to protect the application's core business logic from external dependencies and concerns. The core application should not be dependent on the database, the web framework, or any other external service. This ensures the application is easier to test in isolation, maintain, and evolve over time, which aligns with the guiding principle to "Maintain Architectural Integrity & Manage Technical Debt".

---
## 2. Application Layers
Within any given feature or domain, the code is organized into distinct layers, following a pragmatic **Controller-Service-Repository** pattern.

* **Controllers (Primary Adapters):** This is the outermost layer and the entry point for external interactions, primarily REST API requests. Controllers are responsible for translating incoming HTTP requests into application service calls, handling request validation, and formatting the final HTTP response. They contain no business logic.
* **Services (Application Core):** This layer contains the core business logic and orchestrates application use cases. A service is called by a Controller and uses Repositories to interact with data. This layer is the heart of the application.
* **Repositories (Secondary Adapters):** This layer is responsible for all data persistence logic. It acts as an abstraction layer over the database (Prisma), exposing simple methods for data retrieval and storage. The rest of the application interacts with the Repository, not directly with the ORM.

---
## 3. The Dependency Rule
The single most important rule of this architecture is that **dependencies must only point inwards.**

* **Controllers** can depend on **Services**.
* **Services** can depend on **Repository interfaces**.
* **Services must not depend on or know about Controllers.**
* The **Application Core must not depend on** concrete external infrastructure like the NestJS framework or the Prisma client directly.

This rule ensures that the core business logic can be tested without needing a running web server or a database, which dramatically improves testability and development speed.

---
## 4. Inter-Module Boundaries
At a higher level, the codebase is organized into logical modules (`MC`, `IDP`, `Core`). The dependency rule also applies here.

It is a mandatory requirement to **enforce strict module boundaries**. For example, a service within the `IDP Module` must not directly import and use a service from the `MC Module`. If functionality needs to be shared, it must be placed in the `Core Module` and exposed via a clean interface. This prevents the system from degrading into a "big ball of mud" and will be enforced via automated linting rules where possible.
