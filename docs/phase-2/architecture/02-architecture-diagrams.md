# Architecture Diagrams

**Document:** Architecture Diagrams\
**Phase:** Phase 2 --- Architecture\
**Version:** 0.2\
**Status:** In progress --- built step by step, one question at a time\
**Date:** 2026-10-09\
**Depends on:** [Architecture Vision & Principles](01-architecture-vision.md),
[Cahier des Charges 01 --- Organisation & Accès](../../phase-1/specs/01-organisation-et-acces.md),
[Scope & Roadmap Matrix](../../reference/05-scope-matrix.md)

Diagrams in this document:

1.  Big picture --- *step 1 done*
2.  Building blocks inside the platform
3.  How organizations are kept apart (the two walls)
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

-   The platform is drawn as **one box**. Its inside comes in step 2.
-   No cloud provider is drawn.

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-09 | Document created with step 1 (the big picture) and an isolation decision: own database per organization. |
| 0.2 | 2026-10-09 | Isolation decision withdrawn: the first version uses one database for all organizations (Architecture Vision §5). New customers and payment provider removed from step 1; self-service sign-up stays "Later" in the scope matrix. |
