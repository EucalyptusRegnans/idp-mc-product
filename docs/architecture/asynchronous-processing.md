---
title: "Asynchronous Processing & AI Integration Architecture"
status: "Draft"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# **Asynchronous Processing & AI Integration Architecture**

This document details the architecture for handling long-running background tasks, which are essential for the system's AI features.

## **1. Core Requirement**
It is a **mandatory** requirement that all long-running or non-blocking operations, especially calls to external AI services, are handled reliably and outside of the main request-response cycle. This ensures the application remains responsive and can manage complex background jobs effectively.

***

## **2. Technology Stack**
* **Technology**: The system will use **BullMQ** as its asynchronous task queue, backed by **Redis** for message broking and storage.
* **Integration**: The `@nestjs/bull` library will be used for seamless integration with the NestJS framework. The system will be configured with job queues, producers (to add jobs), and consumers (workers to process jobs), with policies for retries and backoff.

***

## **3. Architectural Pattern**
The architecture follows a standard, scalable pattern for integrating AI tasks:

1.  A standard API request is received by a feature module (e.g., the `IDP Module`).
2.  The service places a job onto a BullMQ queue.
3.  An asynchronous **Worker** (a BullMQ Processor, likely located in `src/workers/`) picks up the job from the queue.
4.  The Worker executes the main AI logic, interacting with external AI services.
5.  The Worker stores the results of the AI task in the database.

A central `AiCoreModule` will provide thin abstractions for these interactions.

***

## **4. Implementation Details**

* **Context & Prompt Management**: The assembly of prompts for AI services will occur within the Workers. This process will use prompt builders or templates to structure the context, and must securely inject tenant-scoped data to prevent data leakage between tenants.
* **External AI Service Interaction**: Workers will use official vendor SDKs (e.g., from OpenAI or Google AI) to interact with external AI services. This requires secure API key management and robust error handling with retries to manage network issues or timeouts.
* **AI Data Storage**: The results from AI tasks will be stored in dedicated, tenant-scoped database tables that include an `organization_id` column. Any large data blobs (e.g., text for analysis) will be stored in Supabase Storage, with a reference stored in the database.
