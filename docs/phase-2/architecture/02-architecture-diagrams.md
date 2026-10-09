# Architecture Diagrams

**Document:** Architecture Diagrams\
**Phase:** Phase 2 --- Architecture\
**Version:** 0.1\
**Status:** In progress --- built step by step, one question at a time\
**Date:** 2026-10-09\
**Depends on:** [Architecture Vision & Principles](01-architecture-vision.md),
[Cahier des Charges 01 --- Organisation & Accès](../../phase-1/specs/01-organisation-et-acces.md),
[Scope & Roadmap Matrix](../../reference/05-scope-matrix.md)

Diagrams in this document:

1.  Big picture --- *step 1 done*
2.  Building blocks inside the platform
3.  How organizations are kept apart
4.  What is shared and what belongs to one organization
5.  Where it runs (no provider named)
6.  Keeping it running: releases, backups, monitoring
7.  One organization leaving

Diagrams are written in Mermaid so GitHub displays them, in the same way
as the [UML Diagrams](../../phase-1/model/01-uml-diagrams.md). No cloud
provider or product is named: the hosting choice waits for the legal
check on where personal data may be stored (Architecture Vision §2.3
and §8). Solid arrows are the first version (MVP); dotted arrows come
later.

------------------------------------------------------------------------

## Decision --- how organizations are kept apart

**Decision of 2026-10-09** (to be numbered in the Decision Log):

-   Every organization has **its own database**. The business data of
    two organizations is never in the same database.
-   **Small customers** have their own database, on database servers
    shared in groups. New servers are added as the platform grows.
-   **Larger customers** (premium plan) get **their own database
    server**.
-   An own copy of the application per organization, and an own cloud
    account per organization, are not offered at first. The design must
    not prevent them later.
-   A small shared **control plane** (the "reception desk") remains: the
    list of organizations, subscriptions, creation of new
    organizations, and (to be decided) user accounts.
-   This **supersedes** the choice in Architecture Vision §5: one shared
    database with an organization id on every record, plus row-level
    security.

To settle in the next steps:

-   Where user accounts live, since one person can belong to several
    organizations (Cahier des Charges 01, rule 4).
-   How each release updates many databases.
-   How one organization's backup, restore, export and deletion work.
-   The payment funnel: the scope matrix lists self-service sign-up as
    "Later" and subscription billing as V1. Selling the software with a
    payment funnel may move them earlier.

------------------------------------------------------------------------

## 1. Big picture

### Step 1 --- Who uses the system and what surrounds it

``` mermaid
flowchart LR
    ST["👤 Staff of each organization<br/>(one interface per role)"]
    OP["👤 Platform operator"]
    NC["👤 New customers"]
    EX["👤 Renters, owners, vendors, buyers"]

    SYS(["Real Estate Operations ERP<br/>one platform, many organizations,<br/>each with its own database"])

    EM["Email service"]
    SM["SMS provider"]
    PP["Payment provider"]
    BK["Online payment and bank services"]

    ST -- "work in their organization" --> SYS
    OP -- "creates and suspends organizations" --> SYS
    SYS -- "invitations, reminders, receipts, statements" --> EM
    NC -. "sign up and pay (later)" .-> SYS
    EX -. "portals (V1)" .-> SYS
    SYS -. "text messages (V1)" .-> SM
    SYS -. "subscription payments (later)" .-> PP
    SYS -. "rent payments and bank links (V1 / V2)" .-> BK
```

| Element | What it is | Version | Source |
|---|---|---|---|
| Staff of each organization | Employees who log in and work in their own organization. Each person gets an interface for their role. | MVP | Cahier des Charges 01, section 6.6 |
| Platform operator | The company that runs the platform. It creates and suspends organizations and cannot see an organization's business data. | MVP | Cahier des Charges 01, rule 12 |
| Email service | An outside provider that sends invitations, reminders, receipts and owner statements. It is the only channel to renters and owners in the first version. | MVP | D-010 |
| Renters, owners, vendors, buyers | Outside people who will get their own portals. They have no login in the first version: staff record their requests and send them documents. | V1 | D-010 |
| SMS provider | Text messages to users. | V1 | Phase 0 §0.14 |
| New customers and payment provider | A real estate company signs up and pays online, and its organization is then created. | Later *(proposed)* | Decision of 2026-10-09. The scope matrix lists self-service sign-up as Later and subscription billing as V1: to confirm. |
| Online payment and bank services | Rent paid online, and links with banks. | V1 / V2 | Scope matrix, section 4.7 |

Notes:

-   The platform is drawn as **one box**. Its inside comes in step 2.
-   "Each with its own database" comes from the decision above. How it
    works comes in step 3.
-   No cloud provider is drawn.

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-09 | Document created. Decision: every organization has its own database; larger customers get their own database server. Step 1: the big picture. |
