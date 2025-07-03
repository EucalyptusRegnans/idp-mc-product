---
title: "Product Documentation System Usage Guide"
status: "In Force"
owner: "Ryan"
last_updated: "2025-07-03"
---

# **Product Documentation System Usage Guide**

> **Note: Where to Start**
> If you are ever lost, unsure of which procedure to follow, or onboarding a new AI assistant, the best place to begin is the `GETTING-STARTED.md` file in the root of this repository. It acts as a master guide to direct you to the correct workflow.

## **Table of Contents**
1.  [How to Propose a New Architectural Decision (ADR)](#1-how-to-propose-a-new-architectural-decision-adr)
2.  [How to Start a New PBE Project](#2-how-to-start-a-new-pbe-project)
3.  [How to Add or Update a Coding Standard](#3-how-to-add-or-update-a-coding-standard)
4.  [How to Update an Architecture Document](#4-how-to-update-an-architecture-document)
5.  [How to Document a New Guardrail](#5-how-to-document-a-new-guardrail)
6.  [How to Maintain the Two-Hub System](#6-how-to-maintain-the-two-hub-system)
7.  [Choosing a Workflow: The Two Modes of Operation](#7-choosing-a-workflow-the-two-modes-of-operation)
8.  [How to Use an AI to Edit Documentation](#8-how-to-use-an-ai-to-edit-documentation)

---

## **1. How to Propose a New Architectural Decision (ADR)**
*Use this process when a significant architectural choice needs to be made and recorded.*

1.  Navigate to the `/docs/adr/` directory.
2.  Find the highest existing ADR number (e.g., `adr-012-...`).
3.  Create a new file using the next sequential number and a descriptive kebab-case name.
    * **Example:** `adr-013-adopt-signal-store.md`
4.  Copy the content from an existing ADR to use as a template.
5.  Fill out the YAML front-matter, setting the `status` to `"Proposed"`.
6.  Complete the `Context` and `Decision` sections of the document.
7.  Commit this new file as part of a pull request for review. Once the decision is approved and merged, update the `status` to `"Accepted"` and fill out the `Consequences` section.

## **2. How to Start a New PBE Project**
*Use this process when beginning a new, discrete block of work, such as implementing a major feature or conducting a proof-of-concept.*

1.  Navigate to the `/docs/pbe/` directory.
2.  Create a new project folder using the `YYYY-MM-DD-[project-slug]` naming convention.
    * **Example:** `2025-07-15_user-profile-feature/`
3.  Inside the new folder, create an initial, empty `MPD.md` file.
4.  Begin the PBE process with Planner-AI, using this `MPD.md` as the chronological log for the project.

## **3. How to Add or Update a Coding Standard**
*Use this process when a new rule or best practice for writing code is established.*

1.  Navigate to the `/coding-standards/` directory.
2.  Select the appropriate file for the standard (e.g., `ts.md` for TypeScript, `naming.md` for naming conventions).
3.  Add a new section using a clear Markdown heading.
4.  Clearly state the rule.
5.  Provide simple code examples for both "Good" and "Bad" patterns where applicable.

## **4. How to Update an Architecture Document**
*Use this process when a technical decision requires an update to our agreed-upon architecture.*

1.  Identify the relevant document in the `/docs/architecture/` directory (e.g., `data-access.md`).
2.  Update the content to reflect the new decision or pattern.
3.  Update the `last_updated` field in the YAML front-matter.
4.  If the change was driven by a new ADR, ensure you add a reference to it in the `related_adrs` field in the front-matter.

## **5. How to Document a New Guardrail**
*Use this process when a decision results in a mandatory rule that the project must follow.*

1.  Open the `docs/engineering-handbook.md` file.
2.  Navigate to the "Guardrails" table.
3.  Add a new row to the table, assigning the next sequential ID (e.g., `GR-012`).
4.  In the "Rule" column, concisely state the mandatory rule.
5.  In the "Source" column, provide a relative Markdown link to the ADR or architecture document that established the rule.

## **6. How to Maintain the Two-Hub System**
*Use this guide to understand the workflow for our separated Product and Business documentation hubs.*

The project's entire knowledge base is separated into two distinct Git repositories to keep technical and business concerns separate.

* **`product/` Repository:** This contains all technical assets, including source code, this documentation, and the architecture specifications. **Commit and push from this directory for all technical work.**
* **`business/` Repository:** This contains all strategic and commercial documents, like go-to-market plans. **Commit and push from this directory for all business-related work.**

### **Standard Workflow**

1.  Navigate to the correct repository for your task (`product/` or `business/`).
2.  Make your changes to the files within that repository.
3.  Use `git add` and `git commit` to save your changes to the correct local repository.
4.  Use `git push` to sync your changes to the appropriate remote repository on GitHub.

## **7. Choosing a Workflow: The Two Modes of Operation**
To work efficiently, it's important to choose the right workflow for the task at hand. There are two primary modes for interacting with AI assistants in this project.

### **Mode 1: Focused Task Mode**
* **When to Use:** Use this for most day-to-day tasks, such as exploratory coding, debugging, or any work where you **do not** expect to change the core system architecture.
* **Process:**
    1.  Start a fresh AI chat session with a narrow context.
    2.  If you generate something valuable that needs to be documented, **only then** do you provide the `onboarding/product-documentation.md` document to begin the "documentation phase" at the end of the session.

### **Mode 2: Architectural Work Mode**
* **When to Use:** Use this when you are **knowingly** working on something that will impact the core system.
* **Process:**
    1.  Start the AI chat session by providing the relevant **upfront context** (e.g., a technical onboarding document).
    2.  Work on the technical implementation.
    3.  At the end of the session, provide the `onboarding/product-documentation.md` document to transition the AI to a "Documentation Specialist" to help you capture the work correctly.

## **8. How to Use an AI to Edit Documentation**
*Use this process when you want an AI assistant to modify a documentation file on your behalf.*

This workflow ensures the AI has the correct context to perform edits accurately and according to our established standards. The core pattern is **Plan -> Provide Context -> Give Command -> Receive Full File Content -> Replace**.

### **The Standard Playbook**

1.  **State Your Goal & Get a Plan (New Step):** Tell the AI what you want to document. Based on your goal, the AI should first provide a high-level plan, including which documentation procedure to follow and a list of files that will likely need to be created or updated. Review this plan before proceeding.

2.  **Start with an Onboarded AI:** Ensure your AI assistant has been properly onboarded. For general documentation tasks, this means you have started the session using the `GETTING-STARTED.md` or `onboarding/product-documentation.md` guides.
    * *Crucially, this ensures the AI has the content of this `USAGE-GUIDE-product.md` in its context, which allows it to understand our procedures.*

3.  **Provide the File(s) to be Edited:** Tell the AI which file you want to change and provide its current contents. If a file is blank, state that explicitly.
    * **Example Prompt:** "I need to update a file. The file is `docs/engineering-handbook.md`. Its current content is: (paste content or write 'The file is currently blank')."

4.  **Give an Explicit Command:** Tell the AI exactly what information to add or change.
    * **Example Prompt:** "Please add this rule as a new Guardrail to the `engineering-handbook.md` file. The rule is: '...'. The source is `docs/architecture/data-access.md`."

5.  **Request the Full File Content:** Always end your command by asking the AI to generate the complete, final version of the file.
    * **Example Prompt (continued):** "...Please generate the **complete, updated content** for the `engineering-handbook.md` file."

6.  **Replace Your Local File:** Copy the entire Markdown block provided by the AI and use it to replace the contents of your local file.
