# Architecture Diagrams

**Document:** Architecture Diagrams\
**Phase:** Phase 2 --- Architecture\
**Version:** 0.4\
**Status:** In progress --- built step by step, one question at a time\
**Date:** 2026-10-09\
**Depends on:** [Architecture Vision & Principles](01-architecture-vision.md),
[Cahier des Charges 01 --- Organisation & Accès](../../phase-1/specs/01-organisation-et-acces.md),
[Scope & Roadmap Matrix](../../reference/05-scope-matrix.md)

Diagrams in this document:

1.  Big picture --- *step 1 done*
2.  Building blocks inside the platform --- *step 2 done*
3.  How organizations are kept apart --- *step 3 done*
4.  What is shared and what belongs to one organization
5.  Where it runs (no cloud provider named)
6.  Keeping it running: releases, backups, monitoring

Diagrams are written in Mermaid so GitHub displays them, in the same way
as the [UML Diagrams](../../phase-1/model/01-uml-diagrams.md).

The first version is kept simple: **one database for all
organizations**, as chosen in Architecture Vision §5. The way
organizations are isolated is reviewed again before real-life testing.
No cloud provider is named: the hosting choice waits for the legal check
on where personal data may be stored (Architecture Vision §2.3 and §8).

Solid arrows are the first version (MVP); dotted arrows come later.

------------------------------------------------------------------------

## 1. Big picture

### Step 1 --- Who uses the system and what surrounds it

``` mermaid
flowchart LR
    ST["👤 Staff of each organization<br/>(one interface per role)"]
    OP["👤 Platform operator"]
    EX["👤 Renters, owners, vendors, buyers"]

    SYS(["Real Estate Operations ERP<br/>one platform, many organizations,<br/>kept strictly apart"])

    EM["Email service"]
    SM["SMS provider"]
    BK["Online payment and bank services"]

    ST -- "work in their organization" --> SYS
    OP -- "creates and suspends organizations" --> SYS
    SYS -- "invitations, reminders, receipts, statements" --> EM
    EX -. "portals (V1)" .-> SYS
    SYS -. "text messages (V1)" .-> SM
    SYS -. "rent payments and bank links (V1 / V2)" .-> BK
```

| Element | What it is | Version | Source |
|---|---|---|---|
| Staff of each organization | Employees who log in and work in their own organization. Each person gets an interface for their role. | MVP | Cahier des Charges 01, section 6.6 |
| Platform operator | The company that runs the platform. It creates and suspends organizations and cannot see an organization's business data. | MVP | Cahier des Charges 01, rule 12 |
| Email service | An outside provider that sends invitations, reminders, receipts and owner statements. It is the only channel to renters and owners in the first version. | MVP | D-010 |
| Renters, owners, vendors, buyers | Outside people who will get their own portals. They have no login in the first version: staff record their requests and send them documents. | V1 | D-010 |
| SMS provider | Text messages to users. | V1 | Phase 0 §0.14 |
| Online payment and bank services | Rent paid online, and links with banks. | V1 / V2 | Scope matrix, section 4.7 |

Notes:

-   The platform is drawn as **one box**. Its inside is step 2.
-   No cloud provider is drawn.

------------------------------------------------------------------------

## 2. Building blocks

### Step 2 --- What the platform is made of

``` mermaid
flowchart LR
    WEB["Web application<br/>staff interface, one per role"]
    OPS["Operator back office<br/>(platform operator only)"]
    LATER["Portals and mobile apps (V1)"]

    subgraph APP["Application server: one application, split into business modules"]
        M1["People and access<br/>Identity and Access · Platform Administration"]
        M2["Real estate<br/>Property · Owner · Leasing · Sales · Prospects"]
        M3["Money<br/>Finance"]
        M4["Operations<br/>Maintenance · Vendor"]
        M5["Shared services<br/>Documents · Notifications · Reporting · Audit"]
    end

    WK["Background worker<br/>same code, started in another mode:<br/>rent generation, reminders,<br/>expiries, emails, PDF generation"]
    DB[("Database<br/>all business data,<br/>kept apart by organization")]
    FS["File storage<br/>documents, photos, PDFs"]
    EM["Email service<br/>(outside provider)"]

    WEB -- "REST API over HTTPS" --> APP
    OPS -- "REST API over HTTPS" --> APP
    LATER -. "same REST API" .-> APP
    APP --> DB
    APP --> FS
    APP --> EM
    WK --> DB
    WK --> FS
    WK --> EM
```

| Block | Role | Source |
|---|---|---|
| Web application | The staff interface. It shows each employee the menus of their roles and uses only the public API. | Cahier des Charges 01, section 6.6; Architecture Vision §4 |
| Operator back office | A small separate interface for the platform operator. It has no access to organizations' business data. | Cahier des Charges 01, rule 12; Architecture Vision §4 |
| Application server | All business logic and all permission checks. It exposes the REST API. It is one application, deployed as one unit, divided into business modules. | Architecture Vision §3 and §4 |
| Background worker | The same code as the application server, started in another mode. It runs scheduled and long tasks. | Architecture Vision §4 |
| Database | The single source of truth for all business data. One database serves all organizations in the first version. | Architecture Vision §4 and §5 |
| File storage | Stores files. Files are never public: they are downloaded through short-lived links issued after a permission check. | Architecture Vision §4 |
| Email service | An outside provider that sends the emails. | Architecture Vision §4 |
| Portals and mobile apps (V1) | More clients of the **same API**. There is no second back end. | Architecture Vision §4 |

Notes:

-   The 14 modules of Architecture Vision §3.2 are **grouped in five
    boxes for readability only**. The grouping is not a design rule.
-   Modules talk to each other through public interfaces or business
    events, never by reading each other's tables (Architecture Vision
    §3.3). This is not drawn here.
-   No cloud provider, server size or network is drawn. That comes in
    step 5.

------------------------------------------------------------------------

## 3. How organizations are kept apart

### Step 3 --- Different organization IDs

Every organization gets its own ID when it is created. Every record
carries the ID of its organization. The application server uses the ID
of the logged-in user's organization for every request.

``` mermaid
flowchart LR
    U1["👤 Karim<br/>Médina Immobilier"]
    U2["👤 Leila<br/>Carthage Immobilier"]

    SRV["Application server<br/>takes the organization ID from the login<br/>and uses it for every request"]

    subgraph DB["Database: one table of leases (example)"]
        R1["Lease A1<br/>organization ID 1"]
        R2["Lease A2<br/>organization ID 1"]
        R3["Lease B7<br/>organization ID 2"]
    end

    U1 -- "logs in" --> SRV
    U2 -- "logs in" --> SRV
    SRV -- "for Karim: ID 1 only" --> R1
    SRV -- "for Karim: ID 1 only" --> R2
    SRV -- "for Leila: ID 2 only" --> R3
```

| Element | What it is | Source |
|---|---|---|
| Organization ID | A unique number given to each organization when the operator creates it. | Cahier des Charges 01, UC-001 |
| ID on every record | Every business record carries its organization's ID, set when the record is created and never changed. | Cahier des Charges 01, rule 1 |
| ID from the login | The server takes the ID from the user's login, never from anything the user types. One organization at a time. | Cahier des Charges 01, rule 3 |

Notes:

-   If Karim asks for Lease B7 by its number, the server answers "not
    found", not "access denied" (Cahier des Charges 01, section 11.1).
-   The Architecture Vision (§5.2) also adds a second check inside the
    database itself (row-level security). It is not drawn here, to keep
    the first version simple.

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-09 | Document created with step 1 (the big picture) and an isolation decision: own database per organization. |
| 0.2 | 2026-10-09 | Isolation decision withdrawn: the first version uses one database for all organizations (Architecture Vision §5). New customers and payment provider removed from step 1; self-service sign-up stays "Later" in the scope matrix. |
| 0.3 | 2026-10-09 | Step 2: the building blocks inside the platform. |
| 0.4 | 2026-10-09 | Step 3: how organizations are kept apart (organization IDs). |
