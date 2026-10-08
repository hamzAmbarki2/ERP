# Architecture Vision & Principles

**Project:** Cloud-Native Multi-Tenant Real Estate Operations ERP\
**Document:** Architecture Vision & Principles\
**Phase:** Phase 2 --- Architecture\
**Version:** 0.3\
**Status:** Draft --- items marked *(working assumption)* are to be
confirmed by Cahier des Charges 11\
**Date:** 2026-10-08\
**Depends on:** [Phase 0](../../phase-0/01-product-definition.md),
[Cahier des Charges Général](../../phase-1/specs/00-cahier-des-charges-general.md),
[Cahier des Charges 01 --- Organisation & Accès](../../phase-1/specs/01-organisation-et-acces.md),
[Scope & Roadmap Matrix](../../reference/05-scope-matrix.md)

------------------------------------------------------------------------

## In short

This is the most general architecture document. It does not choose a
programming language or a cloud provider. It fixes **the shape of the
system and the rules every later technical choice must respect**.

-   **Shape:** one application, split into clearly separated business
    modules (a *modular monolith*), one PostgreSQL database shared by
    all organizations with strict separation, a background worker, and
    file storage.
-   **Top priority:** an organization must never see another
    organization's data.
-   **Second priority:** money must always be right and traceable.
-   **Rule of thumb:** keep it simple. Add infrastructure only when a
    real need appears, never to look impressive.

Later Phase 2 documents go from this general level to the details (see
section 10).

------------------------------------------------------------------------

## 1. What the system is

A web application used by the staff of real-estate companies
(organizations) to run rentals, sales, maintenance, vendors and
finance. Many organizations use the same running system; each sees only
its own data.

### 1.1 Organizations and their users (multi-tenancy)

The system is **multi-tenant**: one running system shared by many
customer companies, each completely separated from the others. In this
project, each customer company is called an **Organization** (see the
[Glossary](../../reference/01-glossary.md), naming rule N-01: the word
"tenant" is reserved for people and companies who rent a unit).

``` text
Platform (one running system)
│
├── Organization: Agence Médina
│   ├── Sonia    — Administrator
│   ├── Karim    — Property Manager
│   ├── Amira    — Sales Agent
│   ├── Hédi     — Finance
│   └── Ali      — Internal Technician
│        (each user has an interface dedicated to their role)
│
├── Organization: Agence Carthage
│   └── its own users, its own data, its own interfaces
│
└── … more organizations
```

-   An organization has **many users**; each user sees only the data of
    the organization they are working in.
-   Nothing is shared between organizations except the platform itself.

### 1.2 Who uses the system, first version and V1

``` text
                     ┌─────────────────────────────────────┐
  FIRST VERSION      │      Real Estate Operations ERP     │
  (MVP)              │                                     │
  Staff of           │   one system, many organizations,   │ ───► Email service
  Agence Médina ───► │   each strictly separated           │      (invitations, reminders,
  Staff of           │                                     │       receipts, statements)
  Agence Carthage ─► │                                     │
  Platform operator ►│                                     │ ───► File storage
                     │                                     │      (documents, photos)
  VERSION 1 (V1)     │                                     │
  Tenants ─ ─ ─ ─ ─► │                                     │ ─ ─► SMS provider (V1)
  Owners ─ ─ ─ ─ ─ ► │                                     │
  Vendors ─ ─ ─ ─ ─► │                                     │ ─ ─► Online payment (V1/V2)
  Buyers ─ ─ ─ ─ ─ ► │                                     │
                     └─────────────────────────────────────┘
  ───►  first version          ─ ─►  version 1 and later
```

| Who | First version (MVP) | Version 1 |
|---|---|---|
| Staff of each organization | Logs in, one interface per role | Same |
| Platform operator | Logs in to the operator back office | Same, plus support access |
| **Tenants** (people and companies renting) | **No access.** Staff record their requests; they receive emails and documents. | **Tenant portal** |
| Owners | No access; statements by email | Owner portal |
| Vendors | No access; staff record their work | Vendor portal |
| Buyers | No access; the sales agent manages the record | Buyer portal |

This follows decision D-010 (Cahier des Charges Général §22).

------------------------------------------------------------------------

## 2. What drives the architecture

### 2.1 Quality priorities

When two goals conflict, the higher one wins.

| Rank | Quality | What it means here | Source |
|---|---|---|---|
| 1 | **Isolation between organizations** | No data ever crosses from one organization to another, in screens, search, files, emails, reports or background jobs. | Cahier des Charges 01, section 11 |
| 2 | **Security and least privilege** | Each person can do only what their role and scope allow, checked by the server on every action. | Cahier des Charges 01, rules 9 and 14 |
| 3 | **Financial correctness** | Amounts are exact; financial records are never silently changed or deleted; every balance can be explained. | Phase 0 §0.18 |
| 4 | **Traceability** | Important actions leave an audit record that cannot be altered. | Phase 0 §0.18, Cahier des Charges 01, rule 18 |
| 5 | **Maintainability** | A small team can understand, test and change any module without breaking others. | Phase 0 §0.20 |
| 6 | **Localization** | French, Arabic (right-to-left) and English from the first version; TND with 3 decimals; other currencies possible later. | Phase 0 §0.12 |
| 7 | **Reliability and recovery** | Data is backed up; the system can be restored; automatic jobs can safely run twice. | Phase 0 §0.12 |
| 8 | **Performance** | Fast enough for daily office work; no special optimization before measurements. | Phase 0 §0.12 |
| 9 | **Cost** | Affordable to run for a small number of customers at the start. | Phase 0 §0.16 |

### 2.2 Working sizes and targets

These numbers are *(working assumptions)* until Cahier des Charges 11
fixes them. They are deliberately modest.

| Item | First year | Design limit without redesign |
|---|---|---|
| Organizations | 10--50 | 1,000 |
| Units per organization | 20--2,000 | 10,000 |
| Staff users per organization | 1--50 | 200 |
| Users connected at the same time (whole platform) | 50 | 2,000 |
| Documents and photos | a few GB | a few TB |
| Availability | 99.5% per month (about 3.5 hours of downtime) | 99.9% |
| Screen response time | 95% of requests under 0.5 second | same |
| Maximum data loss after an incident | 1 hour | 15 minutes |
| Maximum time to restore service | 4 hours | 1 hour |

### 2.3 Constraints

-   **Team:** small (possibly one developer). Every choice must be
    learnable and operable by a small team.
-   **Market:** Tunisia first. The **Tunisian personal-data protection
    law (Loi organique 2004-63) and its authority (INPDP)** may limit
    hosting personal data outside Tunisia. This affects the choice of
    cloud provider and region and must be checked before choosing
    (Regulatory & Legal Register).
-   **Required by Phase 0 (§0.26):** PostgreSQL, a REST API, Docker,
    Terraform, continuous integration and delivery with security
    scanning, cloud deployment, observability, backup and restore.
-   **Excluded by Phase 0:** artificial intelligence and machine
    learning; microservices, Kubernetes or message brokers without a
    real need (§0.11, §0.16, §0.20).

------------------------------------------------------------------------

## 3. Architecture style: a modular monolith

### 3.1 The choice

**One application, deployed as one unit, divided inside into business
modules with strict boundaries.**

Comparison of the options:

| Option | Advantages | Disadvantages | Verdict |
|---|---|---|---|
| Classic monolith (no internal boundaries) | Fastest start | Becomes tangled; hard to change one area without breaking another | Rejected |
| **Modular monolith** | Simple to build, test, deploy and debug; one database transaction can cover several steps (important for money); modules can be extracted later if needed | Requires discipline to keep boundaries | **Chosen** |
| Microservices | Independent scaling and deployment per service | Much more infrastructure, distributed transactions, harder debugging and security; no real need at this size | Rejected for now (Phase 0 §0.20) |

### 3.2 The modules

Based on Phase 0 §0.20, updated for sales and platform administration.

| Module | Owns | Main Cahier des Charges |
|---|---|---|
| Identity & Access | Users, memberships, departments, roles, privileges, scopes, sessions | 01 |
| Platform Administration | Organizations, their status, later subscriptions | 12 |
| Property | Properties, buildings, floors, units, unit status | 02 |
| Owner | Owners, ownership shares and dates, mandates | 03 |
| Leasing | Tenants, leases, rent schedules, deposits | 04 |
| Sales | Sales mandates, listings, offers, reservations, sales | 05 |
| Prospects | Prospects and buyers, viewings | 06 |
| Finance | Charges, invoices, payments, allocations, balances, expenses, owner statements | 07 |
| Maintenance | Maintenance requests, work orders, technician assignments | 08 |
| Vendor | Vendors, contacts, categories, service history | 09 |
| Documents | Files and their metadata, access to files | 10 |
| Notifications | Emails and in-app messages, templates, reminders | 10 |
| Reporting | Reports built from other modules' data | 10 |
| Audit | Audit events | 10 |

### 3.3 Module rules

1.  **Each module owns its data.** Only the module that owns a table
    reads or writes it directly.
2.  **Modules talk through public interfaces or business events.** Example:
    Leasing asks Property "is this unit available?" through Property's
    interface, never by reading Property's tables.
3.  **No circular dependencies** between modules.
4.  **Rules are checked automatically** by tests that fail the build if
    a module reaches into another module's internals.
5.  **A module could become a separate service later** without
    rewriting its business logic. This is a possibility, not a plan.

------------------------------------------------------------------------

## 4. The main building blocks

``` text
┌──────────────────────┐     ┌──────────────────────┐
│  Web application     │     │  Operator back office │
│  (staff interface,   │     │  (platform operator)  │
│  one per role)       │     │                       │
└──────────┬───────────┘     └──────────┬────────────┘
           │   HTTPS, REST API          │
           ▼                            ▼
┌──────────────────────────────────────────────────────┐
│  Application server (modular monolith)               │
│  Identity & Access · Property · Owner · Leasing ·    │
│  Sales · Prospects · Finance · Maintenance · Vendor ·│
│  Documents · Notifications · Reporting · Audit ·     │
│  Platform Administration                             │
└───────┬──────────────────┬───────────────────┬───────┘
        │                  │                   │
        ▼                  ▼                   ▼
┌──────────────┐   ┌───────────────┐   ┌────────────────┐
│ PostgreSQL   │   │ File storage  │   │ Email service  │
│ (all data,   │   │ (documents,   │   │ (external      │
│ separated by │   │ photos, PDFs) │   │ provider)      │
│ organization)│   └───────────────┘   └────────────────┘
└──────▲───────┘
       │
┌──────┴───────────────────────────┐
│ Background worker (same code)    │
│ rent generation, reminders,      │
│ expiries, emails, PDF generation │
└──────────────────────────────────┘
```

| Block | Role |
|---|---|
| Web application | The staff interface. Shows each employee the menus of their role(s) (Cahier des Charges 01, section 6.6). Uses only the public API. |
| Operator back office | Small separate interface for the platform operator. No access to organizations' business data (Cahier des Charges 01, rule 12). |
| Application server | All business logic and all permission checks. Exposes the REST API. |
| Background worker | Same code base as the application server, started in a different mode. Runs scheduled and long tasks. |
| PostgreSQL | The single source of truth for all business data. |
| File storage | Stores files. Files are never public; they are downloaded through short-lived links issued after a permission check. |
| Email service | External provider for sending emails. |

The V1 portals (tenants, owners, vendors, buyers) and mobile apps will
be additional clients of the **same API**. No second back end.

------------------------------------------------------------------------

## 5. Separation between organizations (multi-tenancy)

### 5.1 The choice

**One shared database, with an organization identifier on every
business record, enforced twice: by the application and by the
database itself.**

| Option | Isolation | Cost and effort | Verdict |
|---|---|---|---|
| One database per organization | Strongest | High: hundreds of databases to migrate, back up and monitor | Rejected for now |
| One database schema per organization | Strong | Medium: schema changes repeated per organization | Rejected for now |
| **Shared tables with an organization identifier, plus database row-level security** | Strong if both layers are applied | Low | **Chosen** |

### 5.2 The two walls

1.  **Application wall:** every request runs inside the context of one
    organization (the one the user chose at login). Every query is
    filtered by that organization automatically, not by each developer
    remembering to add a filter.
2.  **Database wall:** PostgreSQL row-level security refuses to return
    or change rows of another organization, even if the application
    has a bug.

The same organization context applies to files (storage paths per
organization), background jobs (each job runs for one organization),
caches, logs and exports. Details: Multi-Tenancy & Isolation Design.

### 5.3 Inside an organization: departments and roles

Isolation **between** organizations (section 5.2) is fixed by the
platform. Access **inside** an organization is configured by each
organization (decision D-011):

``` text
Platform
└── Organization                      ← isolation fixed by the platform
    ├── Departments (tree)            ← defined by the organization
    ├── Roles = grids of privileges   ← defined by the organization
    └── Members
          └── access = privileges of their roles
                       applied to records of their scope
                       (their department, or the whole organization)
```

Consequences for the design:

-   The platform defines a **fixed catalog of domains** and four
    privileges per domain: View, Create, Update, Delete. Organizations
    combine them into roles; they cannot invent new kinds of actions.
-   Roles, departments and memberships are **data**, not code. A change
    takes effect immediately, without a new deployment.
-   Every permission check answers three questions: is the member
    active in this organization? Does one of their roles hold this
    privilege on this domain? Is the record inside their scope
    (department subtree or whole organization)?
-   Records that carry visibility (properties, and the leases, work
    orders and sales attached to them) store their department, so the
    scope filter is applied in the database query, not after loading.
-   These checks are tested automatically like isolation tests.

------------------------------------------------------------------------

## 6. Architecture principles

Every later document and every piece of code must respect these.

| # | Principle | In practice |
|---|---|---|
| 1 | **Organization context everywhere** | No business data is read or written without a known organization. |
| 2 | **The server decides** | Permissions are checked by the server on every action. The interface only hides what is not allowed. |
| 3 | **Deny by default** | An action not explicitly allowed is refused. |
| 4 | **Money is exact** | Amounts are stored as exact numbers with their currency (TND has 3 decimals), never as floating-point numbers. |
| 5 | **Financial history is never rewritten** | Corrections are new records (credit note, reversal, adjustment), never edits or deletions of issued records. |
| 6 | **Audit by design** | Each module records its important actions as audit events; audit records cannot be changed. |
| 7 | **Business events inside the application** | Modules announce what happened (`LeaseActivated`, `PaymentAllocated`) through an internal event mechanism stored in the database. No external message broker until a real need exists. |
| 8 | **Jobs can safely run twice** | Automatic tasks (rent generation, reminders) produce the same result if repeated; no double invoices. |
| 9 | **API first** | Every screen uses the same documented REST API that future portals and mobile apps will use. |
| 10 | **Localization from day one** | No text written directly in the code; every screen works in right-to-left; dates, numbers and amounts formatted per language. |
| 11 | **Everything as code** | Infrastructure (Terraform), database changes (migration scripts), pipelines and configuration are versioned in the repository. |
| 12 | **Security in the pipeline** | Every change is scanned for vulnerable dependencies, secrets, code weaknesses and container issues before it can be deployed. |
| 13 | **Observable** | Structured logs (with the organization identifier, without personal data), metrics, health checks and alerts from the first deployment. |
| 14 | **Simple first** | Prefer managed services and boring, proven technology. Kubernetes, message brokers, microservices or several regions only with a documented need. |

------------------------------------------------------------------------

## 7. Environments

| Environment | Purpose | Data |
|---|---|---|
| Local | Developer machine, started with Docker | Fictional sample data |
| Staging | Test of each version before production, same setup as production | Fictional sample data, never real customer data |
| Production | Real customers | Real data, backed up |

------------------------------------------------------------------------

## 8. Decisions still to take

These are the next documents: one **Architecture Decision Record** per
decision, each comparing options and recording the choice.

| # | Decision | Options to compare | Depends on |
|---|---|---|---|
| 1 | Back-end language and framework | Java with Spring Boot · Kotlin with Spring Boot · C# with .NET · TypeScript with NestJS · Python with Django | Team skills; module-boundary tooling; library maturity for finance and PDF |
| 2 | Front-end framework | React · Angular · Vue | Team skills; right-to-left and translation support |
| 3 | Authentication | Build on the framework's security library · Keycloak (self-hosted) · a managed identity service | Double authentication, invitations, future external users |
| 4 | Cloud provider and region | A major international cloud · a Tunisian or regional host | Data protection law (section 2.3), cost, managed PostgreSQL availability |
| 5 | Hosting model | Managed container service · virtual machines with Docker | Cost, simplicity |
| 6 | Background jobs and internal events | A job library using PostgreSQL · a separate queue service | Principle 14 |
| 7 | PDF generation | Server-side HTML-to-PDF · a document templating library | Arabic and right-to-left support |
| 8 | Repository layout | One repository for everything · separate repositories | Team size |

------------------------------------------------------------------------

## 9. Risks for the architecture

| Risk | Effect | Mitigation |
|---|---|---|
| Cahiers des Charges 02 to 14 are not written yet | Some needs may change the design | Keep this vision general; review it when each cahier is finished |
| An isolation bug leaks data between organizations | Severe: loss of trust, legal exposure | Two walls (section 5.2); mandatory automatic isolation tests (Cahier des Charges 01, criteria 1 to 4) |
| Module boundaries erode over time | Back to a tangled monolith | Automatic boundary checks in the build (section 3.3, rule 4) |
| Hosting abroad not allowed for personal data | Change of provider late | Check the law before decision 4 (section 8) |
| Over-engineering to look "cloud-native" | Time lost, harder to operate | Principle 14; every extra component needs a written reason |

------------------------------------------------------------------------

## 10. From general to detailed: the Phase 2 documents

Each document goes one level deeper than the previous one.

``` text
1. Architecture Vision & Principles            ← this document
        ↓
2. Architecture Decision Records               language, framework, cloud, authentication…
        ↓
3. Architecture Description                    detailed views: context, containers, components
        ↓
4. Multi-Tenancy & Isolation Design            how the two walls are built and tested
5. Security Architecture + Threat Model        authentication, permissions, files, attacks
6. Data Protection & Privacy                   personal data, retention, legal obligations
        ↓
7. Logical & Physical Data Model               tables, keys, money types, history
8. API Guidelines & Specification              conventions, errors, pagination, OpenAPI
9. Background Processing & Integrations        jobs, events, email
        ↓
10. Cloud Infrastructure Architecture          Terraform, network, database, storage
11. Environments & Configuration
12. CI/CD Pipeline Design
13. Observability Design
14. Backup, Restore & Disaster Recovery Plan
```

------------------------------------------------------------------------

## 11. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-08 | First version. |
| 0.3 | 2026-10-08 | Section 5.3: departments and roles defined by each organization (D-011). |
| 0.2 | 2026-10-08 | Section 1: organizations and their users (multi-tenancy); tenants, owners, vendors and buyers shown as V1. |
