---
title: "Engineering Handbook"
status: "In Force"
owner: "Ryan"
last_updated: "2025-07-03"
related_adrs: []
---

# Engineering Handbook

This document contains mandatory rules (Guardrails) and established best practices for the project.

## Guardrails

Guardrails are mandatory rules that must be followed to maintain system integrity, security, and architectural consistency. Any deviation requires an explicit, documented exception.

| ID | Rule | Source |
| :--- | :--- | :--- |
| GR-001 | The `RlsPrismaService` extension does not intercept raw SQL queries (`$queryRaw`, `$executeRaw`). Use of raw SQL is forbidden by project convention unless explicitly approved. | [./architecture/data-access.md](./architecture/data-access.md) |
