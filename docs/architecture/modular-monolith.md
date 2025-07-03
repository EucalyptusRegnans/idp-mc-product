---
title: "Modular Monolith Architecture"
status: "Draft"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# **Modular Monolith Architecture**

This document details the project's foundational architectural pattern: the Modular Monolith. It outlines the high-level structure, the logical modules within the monolith, and the backend framework that governs its implementation.

## **1. Overall Architectural Pattern**

The IDP-MC System will be implemented initially as a **Modular Monolith** operating within a multi-tenant model.

* **Reasoning:** This approach was chosen to reduce complexity and operational overhead for a solo developer during the initial MVP phases. It allows for faster iteration and a more direct development workflow. The clear internal boundaries established by the modular design will facilitate a smoother evolution to a more distributed architecture in the future, if required.
* **Alternatives Considered:** A **Microservices** architecture was rejected due to excessive complexity for the project's initial scale. A model using **separate instances** for each tenant was rejected due to prohibitive cost and management overhead.

The system operates as a single deployable backend application where internal code is organized into distinct modules with strictly enforced boundaries.

## **2. Logical Modules**

The monolith's codebase is organized into the following distinct logical modules, each with clear responsibilities.

* **MC Module:** Contains all features related to habit tracking. This module requires an `organization_id` context to function correctly.
* **IDP Module:** Contains all features related to goal and plan management. This module also requires an `organization_id` context and is responsible for initiating AI analysis tasks and handling core RBAC logic.
* **Core Module:** Contains shared functionality used by other modules, including authentication integration, user and organization management, and shared utilities. It also provides core abstractions for interacting with AI services (`AiCoreModule`).
* **AI Workers:** This is a logical grouping for the asynchronous processors that handle the execution of AI tasks. These are typically implemented as BullMQ Processors located within a `src/workers/` directory.

## **3. Backend Framework and Structure**

The backend monolith is built using the NestJS framework on a Node.js and TypeScript stack.

* **Internal Structure:** The application follows a pragmatic internal structure, generally adhering to a **Controller-Service-Prisma/Repository** pattern for organizing logic.
* **Module Boundaries:** A critical principle is the enforcement of strict module boundaries. This will be supported by automated linting rules to prevent improper cross-module dependencies.
* **Inter-Module Communication:** Initially, communication between modules should favor **direct service injection**. The use of `@nestjs/event-emitter` for asynchronous decoupling should be applied sparingly to maintain simplicity in the early stages.
