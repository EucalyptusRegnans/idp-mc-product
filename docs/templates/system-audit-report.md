---
title: "Documentation System Audit Report"
audit_date: "YYYY-MM-DD"
auditor_ai_model: "[Name of AI Model]"
status: "Completed"
---

# Documentation System Audit Report

## 1. Executive Summary
*(A high-level overview of the audit's findings and the overall health of the documentation system. e.g., "The system is largely consistent, with 2 critical contradictions found and several areas of minor duplication that should be addressed.")*

---

## 2. Critical Findings (Action Required)
*(This section is for major issues that break the system's logic or integrity and require immediate attention.)*

### 2.1. Contradictions
*(Lists direct contradictions between documents. e.g., "Contradiction found between `USAGE-GUIDE-product.md` and `doc-spec.md` regarding the ADR process.")*
* **Finding 1:**
* **Finding 2:**

### 2.2. Major Inconsistencies
*(Lists cases where implementation significantly deviates from the specification. e.g., "`doc-spec.md` states that Guardrails must have a source link, but 3 Guardrails in `engineering-handbook.md` are missing them.")*
* **Finding 1:**
* **Finding 2:**

---

## 3. Duplication & Redundancy Report
*(This section details instances of duplicated or redundant information that violate the "Single Point of Truth" principle.)*
* **Finding 1:** The rule for "Raw SQL Usage" is defined in both `docs/architecture/data-access.md` and `coding-standards/ts.md`.
    * **File A:** `docs/architecture/data-access.md`
    * **File B:** `coding-standards/ts.md`
* **Finding 2:**

---

## 4. Conformance & Integrity Issues
*(This section covers deterministic, structural, and linking issues.)*

### 4.1. Schema Conformance
*(e.g., "The ADR file `adr-014-new-feature.md` is missing the mandatory `status` field in its front-matter.")*
* **Issue 1:**
* **Issue 2:**

### 4.2. Cross-Reference & Link Integrity
*(e.g., "The file `engineering-handbook.md` links to a non-existent ADR: `./../adr/adr-099-...`")*
* **Issue 1:**
* **Issue 2:**

### 4.3. Orphaned Documents
*(e.g., "The file `docs/architecture/old-pattern.md` is not linked to from the main architecture index or any other system document.")*
* **Issue 1:**
* **Issue 2:**

---

## 5. Maintenance & Currency Review
*(This section identifies documents that may be out of date.)*
* **Stale Document Warning 1:** The file `docs/security-spec.md` has not been updated since `2024-01-15` (over 6 months) and may require a review for currency.
* **Stale Document Warning 2:**

---

## 6. Clarity & Best Practice Suggestions (For Human Review)
*(This section contains heuristic suggestions about potential ambiguity, lack of clarity, or deviation from best practices. These require human judgment to validate.)*
* **Suggestion 1:** The process for 'PBE Project Folder' creation in `USAGE-GUIDE-product.md` seems less detailed than other procedures. Consider expanding it for clarity.
* **Suggestion 2:**

---

## 7. Overall Recommendations
*(A summary of the recommended next steps.)*
* **Recommendation:** It is recommended that this audit report be used as the input for the "System Evolution" workflow to address the critical findings and duplication issues.
