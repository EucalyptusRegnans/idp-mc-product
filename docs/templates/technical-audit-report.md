---
title: "Technical Project Audit Report"
audit_date: "YYYY-MM-DD"
auditor_ai_model: "[Name of AI Model]"
status: "Completed"
project_scope: "[Name or ID of PBE Project Audited]"
---

# Technical Project Audit Report

## 1. Executive Summary
*(A high-level overview of the audit's findings for the specified project scope. e.g., "The audit of the 'Foundation Hardening' PBE project revealed 2 stale documents and identified 1 architectural pattern as a candidate for archival. Overall consistency is high.")*

---

## 2. Layer 1 Findings: Integrity & Staleness
*(This section covers objective, deterministic issues found within the project's documentation.)*

### 2.1. Placeholder Text Found (`TODO`, `FIXME`, etc.)
* **Finding 1:**
* **Finding 2:**

### 2.2. Staleness Warnings (Based on PBE Timeline)
* **Warning 1:** The document `docs/architecture/data-access.md` was not updated after the 'Foundation Hardening' PBE was completed, which made significant changes to the data access layer.
* **Warning 2:**

### 2.3. Missing Document References
* **Issue 1:** The `MPD.md` for this project references a deep research report titled "Example Report," but this file was not found.
* **Issue 2:**

---

## 3. Layer 2 Findings: Consistency Issues
*(This section details potential mismatches between the content of different documents.)*

### 3.1. ADR-to-Architecture Mismatches
* **Finding 1:**
* **Finding 2:**

### 3.2. Guardrail-to-Source Mismatches
* **Finding 1:** The summary for `GR-002` in `engineering-handbook.md` does not fully capture the nuance of the decision described in its source, `ADR-007`.
* **Finding 2:**

---

## 4. Layer 3 Findings: Relevancy & Archival Candidates (For Human Review)
*(This section contains subjective candidates for review. A final decision is required by the Human User.)*

### 4.1. Superseded Architectural Patterns
* **Candidate 1:** The pattern described in `docs/architecture/old-auth-v1.md` appears to be fully superseded by the 'JWT Freshness Check' pattern established in the 'Foundation Hardening' project.
    * **Recommendation:** Mark `old-auth-v1.md` as 'Deprecated'.
* **Candidate 2:**

### 4.2. Other Candidates for Archival or Deprecation
* **Candidate 1:**
* **Candidate 2:**

---

## 5. Overall Recommendations
*(A summary of the recommended next steps based on the findings above.)*
* **Recommendation 1:** Address the 'Staleness Warnings' by updating the identified documents.
* **Recommendation 2:** Review and confirm the 'Archival Candidates' and take appropriate action.
