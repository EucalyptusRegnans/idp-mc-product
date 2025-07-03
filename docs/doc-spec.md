# **Documentation Specification v1.8**

*For Mini Habit Echo — AI-First Solo-Dev Project*
*Last-updated: 2025-07-03*

## **1 | Purpose, Scope & System Architecture**

### **1.1 Purpose**

This document provides the authoritative specification for the **Product & Engineering Hub**. Its goal is to create a single, reliable map of all technical project knowledge, structured for efficient use by both the primary human developer and collaborating AI assistants.

### **1.2 System Architecture: The Two-Hub Model**

This project's entire knowledge base is organized into two distinct, physically separate hubs (Git repositories) to ensure a clean separation of concerns:

  * **1. The `product/` Hub (This Repository):** Contains all technical assets. This includes source code (`src/`), this documentation specification, technical architecture documents, coding standards, PBE project logs, and ADRs. Its audience is the "builders."
  * **2. The `business/` Hub (Separate Repository):** Contains all strategic and commercial documents. This includes business plans, go-to-market strategies, etc. Its audience is the "strategists."

### **1.3 AI Collaboration & Context Management**

A primary goal of this two-hub architecture is to enable effective and "clean" collaboration with AI assistants.

  * **Controlled Context:** AI models operate on the context they are explicitly given for a specific task. They do not have access to the entire file system.
  * **Avoiding Contamination:** To ensure AI assistants have relevant, low-noise context, they should typically only be provided with files from the `product/` hub for technical tasks. For example, an IDE-based coding AI should have its project root set to the `product/` directory.

### **1.4 The Onboarding Document Pattern**

A key workflow for this documentation system is the use of dedicated **Onboarding Documents**.

  * **Purpose:** An onboarding document acts as a "bootloader" or **primary interface** for an AI assistant for a specific domain (e.g., documentation, development, marketing). Its purpose is to rapidly provide a new AI chat session with the role, context, and instructions it needs to become a specialized expert.
  * **Core Function:** A well-designed onboarding document instructs the AI on what other key documents and context it needs, and then tells the AI to request those documents from the user. This turns the AI into an active participant in its own onboarding.

### **1.5 Scope**

  * **In scope:** This document governs the structure of the **`product/` hub only**. This includes its directory structures, file naming conventions, required document schemas, and cross-linking rules.
  * **Out of scope:** The structure of the `business/` hub and the content of any documents.

-----

## **2 | Guiding Principles**

| Code | Principle | Rationale & Implementation |
| :--- | :--- | :--- |
| **P-1**|**Docs-as-Code**| **Why:** Treating documentation like code—storing it in Git—gives us versioning, history, and reviewability for free. |
| **P-2**|**Single Point of Truth**| **Why:** To prevent confusion and conflicting information, any single piece of knowledge must have one—and only one—canonical home. |
| **P-3**|**Lean-First → Split-Later**| **Why:** To avoid creating an overly complex structure before it's needed. We start with the simplest possible structure and only create new documents when justified by complexity. |
| **P-4**|**AI-First Format**| **Why:** A primary audience for this documentation is AI assistants. The format must be easy for them to parse and understand reliably. |
| **P-5**|**Automation \> Manual Vigilance**| **Why:** Humans forget to update indexes or check for broken links. We rely on automation to maintain the system's integrity. |
| **P-6**|**Evolutionary Design**| **Why:** This documentation system is a new and evolving framework. It is expected to have flaws and areas for improvement. We must treat the system itself as a product that improves over time through use. **Implementation:** All users (human and AI) are encouraged to be critical of the process. When a rule or procedure is found to be inefficient or flawed, the final step of the task should be to propose an update to this specification or its related guides. |

-----

## **3 | Repository Layout (`product/` hub)**

```text
/product/
├─ .git/
├─ GETTING-STARTED.md
├─ README.md
├─ project-config.yaml 
├─ roadmap.md
├─ src/
├─ issues/
│  └─ ...
├─ coding-standards/
│  ├─ index.md
│  └─ ...
└─ docs/
   ├─ USAGE-GUIDE-product.md
   ├─ adr/
   ├─ pbe/
   ├─ architecture/
   │  ├─ index.md
   │  └─ ...
   ├─ onboarding/
   │  └─ product-documentation.md
   ├─ engineering-handbook.md
   └─ security-spec.md
```

-----

## **4 | Document Types & Schemas**

### **4.1 Summary matrix**

| Type | File Pattern | Required Front-Matter / Sections | Owner |
| :--- | :--- | :--- | :--- |
| **Getting Started** | `GETTING-STARTED.md` | Free Markdown | Ryan |
| **README** | `README.md` | Free Markdown | Ryan |
| **Roadmap** | `roadmap.md` | YAML front-matter + optional narrative | Ryan |
| **Issue Register** | `issues/issues.json` | JSON schema (§4.3) | AI Script |
| **Usage Guide** | `docs/USAGE-GUIDE-product.md` | Free Markdown | Ryan/AI |
| **Onboarding Doc** | `docs/onboarding/[name].md`| Free Markdown | Ryan/AI |
| **ADR** | `docs/adr/adr-###-slug.md` | Standard Front-Matter (§4.6) + Markdown body | Ryan |
| **PBE Project Folder**| `docs/pbe/[YYYY-MM-DD]-[slug]/` | See §4.5 for required contents | Ryan |
| **Coding Standard** | `coding-standards/[topic].md` | Standard Front-Matter (§4.6) + Free Markdown | Ryan/AI |
| **Architecture Doc** |`docs/architecture/[topic].md`| Standard Front-Matter (§4.6) + Free Markdown | Ryan/AI |
| **Security Spec** | `docs/security-spec.md` | Standard Front-Matter (§4.6) + Free Markdown | Ryan/AI |
| **Engineering Handbook**| `docs/engineering-handbook.md` | Standard Front-Matter (§4.6) + Sections | Ryan |
| **Project Config** | `project-config.yaml` | N/A (Pure YAML) | Ryan |

### **4.2 Roadmap `roadmap.md` — YAML schema**

```yaml
program: Mini Habit Echo
version: 0.3
last_updated: 2025-06-30

projects:
  - id: VERT1
    title: "Vertical Slice"
    objective: "End-to-end walking skeleton."
    status: in-progress # planned | in-progress | blocked | done | deferred
    owner: ryan
    start: 2025-06-15
    target_finish: 2025-07-10
    depends_on: []
    milestone: "MVP-0.1"
    issue_labels: ["project:vertical-slice"]
```

### **4.3 Issue Register `issues/issues.json` — authoritative JSON schema**

```jsonc
{
  "$schema": "https://example.com/solo-issue-schema/v1.1",
  "nextId": 201,
  "issues": [
    {
      "id": 200,
      "title": "Stage-3-Step-5: Handle malformed rows",
      "status": "open",
      "priority": "high",
      "project_id": "VERT1",
      "related_adrs": ["ADR-012"],
      "labels": ["tech-debt"],
      "context": { "codeRefs": ["src/import/parser.ts#L42-L88"] },
      "description": "Parser throws generic 400…",
      "owner": "ryan",
      "created": "2025-06-29",
      "updated": "2025-06-30"
    }
  ]
}
```

### **4.4 ADR template (Markdown)**

```markdown
---
title: "ADR Title in Plain Language"
status: "Proposed"  # Proposed | Accepted | Rejected | Deprecated
date: "2025-06-30"
creates_issues: []
related_guardrails: []
---

# ADR ###: Title Matches Front-Matter

## Context
<why the decision is needed>

## Decision
<what is being decided>

## Consequences
...
```

### **4.5 PBE Project Folder**

A PBE Project Folder is the canonical location for all artifacts related to a single, discrete PBE project.

  * **Folder Naming:** `docs/pbe/[YYYY-MM-DD]-[project-slug]/`
  * **Required Contents:** `MPD.md`, `Onboarding-Document.md`
  * **Stage Sub-folders:** May contain stage-specific sub-folders (e.g., `stage-1-foundational-setup/`) to hold AIDR reports.
  * **AIDR Report Naming Convention:** `[stage.step]-[type]-[model]-[doc-name].md`.

### **4.6 Standard Front-Matter**

To enhance machine-readability, all major Markdown documents (ADRs, Architecture, etc.) **must** begin with a YAML front-matter block.

**Example:**

```yaml
---
title: "Data Access & Persistence"
status: "In Force"  # Draft | In Force | Deprecated
owner: "Ryan"
last_updated: "2025-07-03"
related_adrs: ["ADR-003-UsePrisma", "ADR-005-EnableRLS"]
---
```

-----

## **5 | Cross-Linking Rules**

| From | To | Mechanism & Example |
| :--- | :--- | :--- |
| **Issue** | ADR | Structured ID link in JSON: `"related_adrs": ["ADR-012"]` |
| **ADR** | Issue | Structured ID link in Front-Matter: `creates_issues: [201]` |
| **Architecture Doc** | ADR | Markdown link: `See [ADR-005](./../adr/adr-005-enable-rls.md)` |
| **Roadmap Epic** | Business Document | Markdown link: `Rationale in [GTM Strategy](../../business/strategy/commercialisation.md)` |

-----

## **6 | Update & Review Workflow**

| Trigger | Steps | Artifacts Updated |
| :--- | :--- | :--- |
| **Add task** | `bin/issues new --title "..."` | `issues.json` updated |
| **Close task** | `bin/issues close <id>` | Issue moved to archive |
| **New decision** | Copy ADR template → fill → PR | `docs/adr/...` |
| **Guardrail change** | Edit Handbook table (+source) | `engineering-handbook.md` |
| **PBE `Completion Report` is added to `MPD.md`** | **End-of-PBE-Step Review:** \<br\>Review `deviationsFromPlan`, `issuesEncounteredAndResolutions`, `lessonsLearned_Overall`. \<br\>Determine if a significant architectural decision, new rule, or best practice has emerged. \<br\>Create or update the appropriate permanent document. | • New **ADR** \<br\>• **Engineering Handbook** \<br\>• Document in **`/docs/architecture/`** \<br\>• New issue in `issues.json` |

-----

## **7 | Automation & CI**

| Check | Tool/Cmd | Fails PR if… |
| :--- | :--- | :--- |
| JSON schema validation | `bin/issues lint` | bad syntax / dup IDs |
| Structured Link Integrity | `bin/docs link-check` | An ID in a `related_adrs` or `project_id` field does not exist. |
| ADR index | `bin/adr build-index` | index out-of-date |
| Dead links | `bin/docs link-check` | broken relative path |
| Guardrail source | `bin/docs guardrail-lint` | missing Source column |

-----

## **8 | AI Usage Guidelines**

### **8.1 Which docs to load**

| Task | Required Files |
| :--- | :--- |
| “What’s the next project?” | `roadmap.md` |
| “Create subtasks for X” | `issues/issues.json` (+ roadmap labels) |
| “Refactor auth layer” | Related ADR(s) + relevant `/docs/architecture/` files |
| "Bring me up to speed on Project X" | The relevant `MPD.md` from the `docs/pbe/` directory. |

### **8.2 AI as a Query Interface**

The `issues/issues.json` file is intentionally optimized for machine parsing, not direct human reading. **AI assistants are the designated primary interface for this file.** The developer will query the issue register by prompting an AI.

-----

## **9 | Versioning & Change Log**

  * This specification is a living document. Its version is tracked in the header. A separate `changelog.md` file should be maintained to summarize changes between versions.

-----

## **10 | Glossary**

| Term | Definition |
| :--- | :--- |
| **Program** | The entire software product initiative (Mini Habit Echo). |
| **Project** | A discrete chunk of work with its own objective. See also **PBE Project**. |
| **PBE Project** | A specific project executed using the PBE framework, whose artifacts are logged in a dedicated folder under `docs/pbe/`. |
| **Guardrail (GR-xxx)** | A mandatory rule recorded in Engineering Handbook. |
| **Technical Debt (TD-xxx)** | An intentional compromise captured in Handbook + linked Issue. |
| **ADR** | Architecture Decision Record, immutable once “Accepted.” |
| **MPD (Master Project Document)** | A temporary, chronological log of a single PBE project, curated by the Human User. |
