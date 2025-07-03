---
title: "Authentication & Authorization Strategy"
status: "Draft"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# **Authentication & Authorization (AuthN & AuthZ) Strategy**

This document outlines the architecture for verifying user identity (Authentication) and enforcing permissions (Authorization).

## 1. High-Level Strategy

The system uses **Supabase Auth** for Authentication (AuthN) and a custom, **application-centric** model for Authorization (AuthZ).

---
## 2. Authentication (AuthN) Flow

User identity is established via JSON Web Tokens (JWTs) issued by Supabase Auth. The backend is responsible for strictly validating every JWT on protected endpoints, checking its signature, expiration, issuer, and audience to ensure the authenticity of the user's identity claim.

---
## 3. Authorization (AuthZ) Model: RBAC

The system employs a Role-Based Access Control (RBAC) model that is contextual to a specific organization.

* **Definitions:** Roles (e.g., 'Player', 'Coach') and permissions (e.g., 'create_goal') are centrally defined in the database.
* **Assignment:** A user is assigned a role for a specific organization via the `memberships` and `membership_roles` tables. This allows a user to have different roles in different organizations.
* **Enforcement:** Permission checks are performed primarily within the backend application logic (e.g., in NestJS Guards or services) based on the user's validated identity, their active organization, and the roles they possess within that organization.

---
## 4. Implementation: The Authorization & Context Flow

The following sequence is executed on protected API requests to authorize the user and establish a trusted, request-scoped tenant context.

1.  **JWT Validation & User Context**
    A NestJS Guard is the first entry point. It robustly validates the JWT and, upon success, extracts the `user_id` and stores it securely in the `nestjs-cls` (CLS) store.

2.  **Tenant & Membership Validation**
    A subsequent NestJS Guard reads the `user_id` from the CLS store and retrieves the `X-Organization-ID` from the request header. It then performs the critical validation by querying the database to ensure the user has an active membership for the requested organization. If the membership is not valid, the request is rejected with a `ForbiddenException`.

3.  **Tenant Context Population**
    Upon successful membership validation, the Guard populates the CLS store with the validated `organizationId` and the user's roles within that organization.

4.  **Downstream Consumption**
    All downstream backend services and repositories can now inject the `ClsService` to reliably retrieve the trusted `userId` and `organizationId`. This context is then used to perform explicit application-level filtering (`WHERE organizationId = ?`) and any further fine-grained permission checks.

---
## 5. Security Best Practices

The following best practices for handling JWTs are recommended for this system:

* Use **HttpOnly Cookies** with **CSRF Protection** for token transport.
* Implement a **secure refresh token flow**.
* Enforce **short lifetimes** for access tokens.
