**I. In your `IDP-MC System - Technical Specification (v2.2).md`:**

You should have a dedicated top-level **Security Section** (which you already do \- Section 4.7). I recommend enhancing it to include:

* **A new subsection: `4.7.x Threat Model Summary & Attacker Profile`**  
    
  * **Attacker Profile:** Briefly define the primary attacker profiles we are defending against. You can use the language we just discussed:  
    * **Primary Threats:** Script Kiddies (opportunistic, low-skill), Malicious Insiders (legitimate users attempting to cross tenant boundaries).  
    * **Secondary/Mitigated Threats:** Financially Motivated Cybercriminals (skilled, but deterred by high effort vs. low reward).  
    * **Out of Scope Threats:** State-Sponsored Actors.  
  * **Core Assets to Protect:** Explicitly state the most critical assets: Children's Personal Identifiable Information (PII), habit/goal data, and service availability/reputation.  
  * **Link to Full Document:** Add a clear reference: *"For a full, detailed breakdown of the threat model, attack vectors, and security architecture, see the **'IDP-MC System \- Security Architecture & Threat Model'** document."*


* **A new subsection: `4.7.y Defense-in-Depth Strategy`**  
    
  * Briefly list the layers of defense we've designed. This is a powerful summary that shows intentional security architecture.  
    1. **Authentication Layer:** Supabase JWT validation.  
    2. **Input Validation Layer:** `TenantHeaderGuard` performing header presence and format validation.  
    3. **Application Authorization Layer:** `MembershipService` performing active membership checks.  
    4. **Transactional Integrity Layer:** Use of `Serializable` transactions to mitigate race conditions (TOCTOU).  
    5. **Database Isolation Layer:** PostgreSQL Row-Level Security (RLS) as a final safety net.

# IDP-MC System \- Security Architecture & Threat Model

**Document Version:** 1.0 **Status:** In Force

## 1\. Introduction

### 1.1. Document Purpose

This document provides a comprehensive overview of the security architecture, design principles, and threat model for the IDP-MC System. It serves as the single source of truth for security-related thinking, guiding development, testing, and operational practices. Its purpose is to ensure that security is an intentional and integral part of the system's design, focused on defending against likely threats and protecting sensitive user data, particularly that of children.

### 1.2. Target Audience

This document is intended for project stakeholders, including developers, system architects, AI assistants (Planner-AI, Builder-AI, Deep Research AI), and any future security reviewers or partners.

## 2\. Security Guiding Principles

The following core principles guide all security-related decisions for the IDP-MC System:

* **Defense-in-Depth:** Security is not reliant on a single control. We implement multiple, overlapping layers of defense, from the network edge to the database, ensuring that the failure of one layer does not lead to a total system compromise.  
* **Principle of Least Privilege:** Users and system components are only granted the minimum level of access and permissions required to perform their intended functions.  
* **Fail-Secure by Default:** In the event of an error or unexpected state (e.g., missing context, failed authorization check), the system defaults to denying access rather than allowing it.  
* **Principle of Appropriate Controls:** Security measures are designed to be appropriate for the identified threats and the value of the assets being protected, avoiding both negligence and unnecessary complexity that could hinder development velocity.  
* **Security by Design:** Security is considered a foundational requirement throughout the entire development lifecycle, from initial architecture to implementation, testing, and deployment.

## 3\. Asset Identification

The following assets are identified as critical and require protection:

* **Asset 1: User Personal Identifiable Information (PII):**  
  * **Description:** Includes user emails, names, and any other profile data that can identify an individual. This is considered highly sensitive, especially as the user base includes minors.  
  * **Impact of Compromise:** High (Significant reputational damage, complete loss of user trust, potential legal/regulatory consequences under COPPA/GDPR).  
* **Asset 2: User-Generated Content (UGC):**  
  * **Description:** Includes user goals (IDPs), habit tracking data, and personal reflections. While potentially anonymized, this data is private to the user and their organization and could be sensitive.  
  * **Impact of Compromise:** High (Loss of user trust, violation of user privacy).  
* **Asset 3: Service Availability & Integrity:**  
  * **Description:** The ability of the application to function correctly and be available to legitimate users.  
  * **Impact of Compromise:** Medium-to-High (Reputational damage, user churn, loss of confidence in the platform).

## 4\. Threat Model

### 4.1. Attacker Profiles

We have defined the following attacker profiles to focus our defensive efforts. The architecture is primarily designed to be highly resilient against the first two profiles.

* **Profile 1: The Script Kiddie (Opportunistic Attacker)**  
    
  * **Motivation:** Mischief, curiosity, low-effort disruption.  
  * **Skill Level:** Low. Relies on publicly available, automated scanning tools and exploits for common, well-known vulnerabilities.  
  * **Typical Attacks:** SQL Injection attempts, Cross-Site Scripting (XSS), probing for unpatched dependencies (e.g., CVEs), directory traversal, credential stuffing.  
  * **Relevance to IDP-MC:** High probability of being targeted by automated scans. This is our baseline threat that must be comprehensively mitigated.


* **Profile 2: The Malicious Insider (Authorized User)**  
    
  * **Motivation:** Curiosity, attempting to gain an advantage, or interpersonal conflict within or between organizations (e.g., a coach from Club A trying to view players from Club B).  
  * **Skill Level:** Low-to-Medium. Has legitimate credentials and a deep understanding of the application's intended functionality.  
  * **Typical Attacks:** Attempting to manipulate API requests by changing identifiers (e.g., organization IDs, user IDs) to access data outside of their authorized scope (Insecure Direct Object Reference \- IDOR), privilege escalation.  
  * **Relevance to IDP-MC:** High probability. This is the primary threat that the multi-tenancy architecture is designed to defeat.


* **Profile 3: The Financially Motivated Cybercriminal (Rational Attacker)**  
    
  * **Motivation:** Financial gain (e.g., stealing PII for identity theft, ransomware).  
  * **Skill Level:** Medium-to-High. Capable of finding and exploiting more complex vulnerabilities.  
  * **Typical Attacks:** Targeting data exfiltration, looking for misconfigurations that allow database access, exploiting complex application logic flaws.  
  * **Relevance to IDP-MC:** Lower probability. While the PII has value, the application is not a direct financial target (e.g., it does not store credit card information). A skilled attacker of this type is likely to deem the effort required to defeat the layered security as too high for the potential reward, preferring easier, more profitable targets. Our defenses should still be sufficient to deter this profile.


* **Profile 4: The State-Sponsored Actor (Out of Scope)**  
    
  * **Motivation:** Espionage, large-scale disruption.  
  * **Skill Level:** Very High. Access to zero-day exploits and extensive resources.  
  * **Relevance to IDP-MC:** Extremely low probability. Defending against this level of threat is explicitly out of scope for this project.

### 4.2. Key Attack Surfaces

* **Public API Endpoints:** The primary interface for all application interactions.  
* **Authentication System:** The user login and session management flow (Supabase Auth).  
* **Database:** The PostgreSQL database containing all user and application data.  
* **Third-Party Dependencies:** Vulnerabilities in `npm` packages or other external libraries.  
* **Infrastructure:** The Docker containers and hosting environment.

## 5\. Security Architecture & Controls (Defense-in-Depth)

The IDP-MC system's security relies on a multi-layered defense strategy, where each layer mitigates specific threats.

### Layer 1: Authentication (Supabase Auth)

* **Description:** User identity is verified by Supabase Auth, which issues industry-standard JSON Web Tokens (JWTs). The backend uses a `JwtStrategy` (via Passport.js) to validate the signature and expiration of every JWT on protected endpoints, ensuring the authenticity of the user's identity claim.  
* **Threats Mitigated:** Unauthorized access by unauthenticated parties, token forgery, session hijacking (mitigated by short token lifespans and secure transport).

### Layer 2: Input & Context Validation (Guard Layer)

* **Description:** A chain of NestJS Guards runs after successful authentication. The `TenantHeaderGuard` is responsible for intercepting the `X-Organization-ID` HTTP header, validating that it is present and conforms to a strict UUID format. This prevents malformed or malicious input from propagating into the application.  
* **Threats Mitigated:** Malformed requests, basic injection attempts via the header, denial-of-service from processing invalid inputs.

### Layer 3: Application-Layer Authorization (Service Layer)

* **Description:** The core authorization logic resides in a dedicated `MembershipService`. Before any tenant-specific business logic is executed, this service is called to perform a database query that explicitly verifies the authenticated user (`userId` from the JWT) has an active membership record associated with the target `organizationId` from the header.  
* **Threats Mitigated:** Horizontal privilege escalation (users accessing data from tenants they do not belong to), Insecure Direct Object Reference (IDOR) attacks where an attacker tries to use a known resource ID from another tenant.

### Layer 4: Transactional Integrity (Service Layer)

* **Description:** Critical business operations, which include the authorization check from Layer 3 and the subsequent data modification (e.g., creating a habit), are wrapped in a single, atomic `Serializable` database transaction using Prisma.  
* **Threats Mitigated:** Time-of-Check-to-Time-of-Use (TOCTOU) race condition vulnerabilities. This ensures that a user's permissions cannot be revoked between the authorization check and the action, preventing a narrow but critical attack vector.

### Layer 5: Database Isolation (PostgreSQL RLS)

* **Description:** As a final safety net, PostgreSQL's Row-Level Security (RLS) is enabled on all tenant-scoped tables. Application logic sets a session-local variable (e.g., `SET LOCAL rls.organization_id = ?`) within each transaction. RLS policies attached to the tables then ensure that any query, regardless of its application-level filters, can only see or modify rows matching the session's currently defined `organization_id`.  
* **Threats Mitigated:** Accidental data leakage due to developer error (e.g., a forgotten `WHERE` clause in a complex query) or unforeseen bugs in application-level data filtering logic.

## 6\. Key Security Decisions & Rationale

* **Decision: Shared Database / Shared Schema with RLS**  
  * **Rationale:** This model was chosen for its initial operational simplicity and cost-effectiveness, which is appropriate for an MVP and a solo developer context. The high inherent risk of data leakage in a shared schema is acknowledged and explicitly mitigated by the robust, multi-layered defense strategy outlined above, particularly the combination of mandatory application-level filtering and RLS as a backstop.  
* **Decision: Implicit Context Propagation (`nestjs-cls`)**  
  * **Rationale:** This pattern was chosen to improve developer experience and reduce boilerplate code associated with passing tenant context through every function call. The known reliability risks of this approach (e.g., context loss) are mitigated through specific best practices, such as robust "fail-secure" error handling for missing context and a comprehensive testing strategy.  
* **Decision: Service-Layer Transactional Authorization**  
  * **Rationale:** This design was a specific and deliberate choice to robustly mitigate TOCTOU vulnerabilities. While a membership check in the guard layer is simpler, it is fundamentally insecure against race conditions. By moving the check into the same transaction as the business operation, we prioritize data integrity and security, accepting the minor trade-off of performing the check later in the request lifecycle.

## 7\. Compliance Considerations

* **COPPA / GDPR:** The architecture supports compliance by design. The strong tenant isolation provided by the multi-layered defense ensures that data for one organization (e.g., a sports club in a specific region) is strictly separated from others. This is a prerequisite for managing data according to regional laws. Technical controls for Verifiable Parental Consent flows and Data Subject Access Request (DSAR) procedures (Access, Rectify, Erasure) will be built upon this secure, tenant-aware foundation.

## 8\. Future Security Roadmap

* **Caching Strategy:** Implement a secure, tenant-aware caching layer for membership checks to improve performance at scale. Cache keys must be namespaced by `organizationId` and `userId` to prevent data leakage.  
* **CI/CD Security Scanning:** Integrate automated dependency vulnerability scanning (e.g., `npm audit`) and Static Analysis Security Testing (SAST) tools into the CI/CD pipeline to catch issues early.  
* **Formal Penetration Testing:** Conduct formal third-party penetration testing before a major public launch or upon acquiring enterprise-level customers to validate the effectiveness of the security controls.  
* **Audit Logging:** Implement a comprehensive, immutable audit trail for sensitive administrative actions (e.g., membership changes, role grants) and security-related events (e.g., repeated failed authorization attempts).

