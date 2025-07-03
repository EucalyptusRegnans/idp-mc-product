---
title: "Frontend & Native Application Architecture"
status: "Draft"
owner: "Ryan"
last_updated: "2025-07-02"
related_adrs: []
---

# **Frontend & Native Application Architecture**

This document outlines the architecture, technology stack, and strategies for the client-side single-page application (SPA), including its integration with native device capabilities.

## 1. High-Level Architecture
The frontend will be an Angular-based Single-Page Application (SPA) with Progressive Web App (PWA) support, which enables installability and basic offline functionality. The UI must be designed to handle users who belong to multiple organizations, allowing them to switch between different contexts.

---
## 2. Framework & Language
* **Language:** The application will be written exclusively in **TypeScript**.
* **Framework:** The core framework will be **Angular**, utilizing **Standalone Components** and **Signals** as the primary architectural patterns.

---
## 3. State Management
The application will employ a two-tiered state management strategy:

* **Local UI State:** Raw **Signals** will be used for managing state that is local to individual UI components.
* **Shared/Feature State:** **NgRx SignalStore** is the recommended solution for managing shared application state or complex feature state, valued for its structure and maintainability.

---
## 4. Styling
The styling strategy will be a combination of three technologies:

* **Scoped SCSS:** For component-specific styles.
* **Tailwind CSS:** For layout and utility-first styling.
* **Angular Material:** For pre-built, accessible UI components.

A key part of this strategy involves synchronizing Material Design theme tokens with the Tailwind configuration to ensure a consistent look and feel.

---
## 5. Offline & Native Strategy (Capacitor)
When targeting native platforms, **Capacitor** will be used to wrap the web application.

* **PWA Support:** The `@angular/pwa` package will be used to configure the service worker and caching strategies.
* **Offline Data Storage:** For reliable offline data persistence, the application will use **SQLite** via a Capacitor plugin.
* **Data Security:** The offline SQLite database **must be encrypted** using a technology like SQLCipher. Encryption keys must be securely managed via the native device's **Keychain (iOS) or Keystore (Android)**.
* **Synchronization:** Due to the high complexity of data synchronization logic, it is strongly recommended to use a dedicated library like **RxDB** rather than building a custom solution. The scope of offline sync will be simplified for the MVP.
* **Known Constraints:** There are known limitations with this approach, most critically the **unreliable nature of background execution and alarms on iOS devices**.
