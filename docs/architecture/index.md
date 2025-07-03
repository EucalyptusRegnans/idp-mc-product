---
title: "Architecture Overview"
status: "In Force"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# Architecture Overview

This document provides a high-level summary of the IDP-MC System's technical architecture and the guiding principles that inform its design.

## 1. Project Overview

The IDP-MC System platform integrates two core concepts:

* **MC App (Mission Control):** A habit-building tool using a checkbox-grid system, providing daily/weekly scores and tracking trends over time.
* **IDP System (Individual Development Plan):** A system for goal setting, analysis, and progress tracking, initially conceptualized for youth soccer development but with broader commercial potential.

The goal is to merge these concepts into a single, cohesive, **multi-tenant**, **player-centric** platform targeting youth development. This documentation specifies the technical approach, leveraging modern technologies and significant AI development assistance.

## 2. Guiding Principles

The technical decisions outlined in the architecture documents adhere to these core principles:

* **Modular Monolith Architecture:** Prioritize a well-structured monolith initially.
* **Hexagonal Principles (Ports & Adapters):** Enforce clear boundaries and dependency inversion between internal modules to ensure loose coupling and high testability.
* **Vertical Slice Organization:** Organize features into vertical slices that span from the API controller down to the data access layer, keeping related logic cohesive.
* **Multi-Tenancy:** Support multiple organizations securely (Shared DB/Schema + App Filtering + RLS).
* **Player-Centric Data Model:** Allow users core identity ownership and multiple org associations.
* **Reliable Asynchronous Processing:** Utilize a robust task queue system for all long-running or non-blocking operations, especially AI-driven tasks.
* **TypeScript Focus & Unified Frontend Stack:** Utilize TypeScript across the stack. Unified Angular frontend prioritized.
* **Validated Technology Choices:** Prioritize patterns proven viable during MHE prototyping.
* **Scalability & Performance by Design:** Proactive approach to performance/optimization along a defined scaling path.
* **Maintain Architectural Integrity & Manage Technical Debt:** Prioritize clean code, adherence to patterns, and robust testing.
* **Leverage PaaS/Managed Services:** Offload infrastructure management where practical.
* **Cost-Consciousness:** Prioritize low-cost options initially.
* **Automation & Testing:** Implement automated testing (including dedicated RLS tests) and CI/CD early.
* **Security by Design:** Embed security considerations into the core architecture.
* **AI Collaboration:** Leverage AI to enforce best practices and improve quality, not introduce architectural deviations.

## 3. Core Architectural Pattern

The system will be implemented initially as a **Modular Monolith** operating within a **multi-tenant** model.

---

## 4. Table of Contents

* [Modular Monolith](./modular-monolith.md)
* [Hexagonal Architecture](./hexagonal.md)
* [Vertical Slices](./vertical-slices.md)
* [Data Access & Persistence](./data-access.md)
* [Authentication & Authorization](./authentication-and-authorization.md)
* [Asynchronous Processing](./asynchronous-processing.md)
* [Frontend & Native Strategy](./frontend-and-native.md)
* [Scaling Strategy](./scaling-strategy.md)
