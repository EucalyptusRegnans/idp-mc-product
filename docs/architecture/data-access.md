---
title: "Data Access & Persistence Strategy"
status: "In Force"
owner: "Ryan"
last_updated: "2025-07-03"
related_adrs: []
---

# **Data Access & Persistence Strategy**

This document details the architecture for data storage, access, and tenant isolation in the IDP-MC System.

## **1. Core Data Storage Pattern**

The system will use a **Shared Database, Shared Schema** model on PostgreSQL. This approach was chosen for its initial operational simplicity and cost-effectiveness.

---
## **2. Tenant Data Isolation Strategy**

A hybrid strategy is used to ensure strict data isolation between tenants.

* **Primary Mechanism: Application-Level Filtering**
    The backend services **must** consistently apply explicit `WHERE organization_id = ?` filters to all database queries involving tenant-specific tables. This filtering will use the validated tenant context retrieved via the implicit context mechanism.

* **Secondary Mechanism (Defense-in-Depth): Row-Level Security (RLS)**
    PostgreSQL RLS will be enabled on tenant-specific tables as a critical security backstop. RLS policies will enforce tenant isolation based on validated context provided via a `SET LOCAL` command, which is triggered automatically before queries by a Prisma Client Extension.

---
## **3. Database Schema and Key Tables**

* **Tenant ID Column**
    Every tenant-scoped table must include a non-nullable, indexed `organization_id` foreign key to the `organizations` table.

* **Core Tables (Conceptual)**
    * **organizations:** Defines tenant entities. It includes a predefined system record representing the 'Individual Tenant' space for unaffiliated users.
    * **users:** Represents individual users, linked to `auth.users`. This table is central to the player-centric model.
    * **memberships:** Links users to organizations.
    * **roles:** Defines available roles (e.g., 'Player', 'Coach', 'OrgAdmin').
    * **permissions:** Defines granular permissions (e.g., 'create_goal').
    * **role_permissions:** Join table linking roles to permissions.
    * **membership_roles:** Join table linking memberships to roles, allowing a user to have multiple roles within one organization.

* **AI Data Storage**
    AI-generated results will be stored in separate, dedicated tables (e.g., `ai_idp_analyses`) and must include an indexed `organization_id` column. Large text inputs or outputs for AI will be stored in Supabase Storage, with references in the database.

* **Unaffiliated User Handling**
    Individual users not part of a club or other organization will be associated with the system-defined 'Individual Tenant' organization via their `memberships` record. This ensures all data is consistently scoped and that filtering and RLS mechanisms apply universally.

---
## **4. ORM and Database Interaction**

* **ORM**
    The project will use **Prisma** as its Object-Relational Mapper (ORM).

* **RLS Implementation Details**
    * **Policy Design:** RLS policies will focus strictly on basic tenant isolation using context from the `SET LOCAL` command (e.g., `current_setting('rls.organization_id', true)::uuid`). The `FORCE ROW LEVEL SECURITY` option will be used.
    * **Performance Optimization:** RLS performance optimization is mandatory. This includes indexing the `organization_id` column and keeping policies simple.
    * **Connection Security:** It is critical that the Prisma database user connects as a non-superuser that does not have the `BYPASSRLS` privilege.
    * **Testing:** Dedicated RLS tests are required.

* **Migration Strategy**
    RLS policies will be managed via separate, idempotent raw SQL scripts located within the Prisma migration folders. These scripts will be applied by a CI/CD wrapper script.

* **Raw SQL Usage**
    The use of raw SQL queries (e.g., via Prisma's `$queryRaw` or `$executeRaw` methods) is forbidden by project convention unless explicitly approved as a documented exception. This is a critical security and architectural convention.
    
    **Reasoning:** The Prisma Client Extension that automatically applies tenant context for Row-Level Security (RLS) **does not intercept raw SQL queries**. Using raw SQL directly bypasses this fundamental defense-in-depth security mechanism, creating a significant risk of data leakage between tenants.
