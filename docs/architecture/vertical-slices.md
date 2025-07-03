---
title: "Vertical Slice Organization"
status: "Draft"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# **Vertical Slice Organization**

This document defines how the application's codebase is organized around features to ensure related logic is cohesive and decoupled from other features.

## 1. Core Principle
The codebase is organized around features, often called "vertical slices," rather than being grouped by technical layers (e.g., a single folder for all controllers). The goal is to keep all the logic for a single high-level feature—from the API controller down to the data access logic—physically co-located. This makes features easier to develop, maintain, and understand in isolation.

---
## 2. Primary Feature Verticals (Logical Modules)
The application is broken down into the following primary feature verticals, which are implemented as distinct NestJS modules.

* **MC Module:** This vertical slice contains all features related to the "Mission Control" habit-tracking functionality. It requires an `organization_id` context to operate.
* **IDP Module:** This vertical slice contains all features related to the "Individual Development Plan," including goal and plan management. It is responsible for initiating AI analysis tasks and contains the core Role-Based Access Control (RBAC) logic. It also requires an `organization_id` context.

---
## 3. Shared Components (The "Core" Module)
Not all code fits neatly into a feature slice. The `Core Module` serves as a shared library for functionality that is used across multiple verticals. This includes authentication integration, user and organization management, shared utilities, and core abstractions for interacting with AI services (`AiCoreModule`).

---
## 4. Asynchronous Workers
Asynchronous tasks, particularly those for AI processing, are considered part of the feature vertical that initiates them. However, they are implemented as separate **BullMQ Processors** (Workers), logically grouped in a dedicated part of the codebase (e.g., `src/workers/`). This separates long-running background tasks from the immediate request-response cycle of the feature's API.
