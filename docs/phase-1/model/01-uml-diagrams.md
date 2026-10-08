# UML Diagrams

**Document:** UML Diagrams\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.1\
**Status:** In progress --- built step by step, one question at a time\
**Date:** 2026-10-08

Diagrams in this document:

1.  Use case diagram --- *in progress*
2.  Class diagram
3.  Activity diagrams
4.  Sequence diagrams
5.  State diagrams

Diagrams are written in Mermaid so GitHub displays them. Mermaid has no
dedicated use case notation, so the use case diagram uses the usual
convention: actors on the sides, use cases as rounded shapes inside the
system boundary.

------------------------------------------------------------------------

## 1. Use case diagram

### Step 1 --- Actors

Actors are the people who **log in** to the ERP. Owners, tenants,
vendors and buyers are records managed by the staff; they do not log
in, so they are not actors (decision D-010).

``` mermaid
flowchart LR
    OP["👤 Platform Operator"]
    AD["👤 Administrator"]
    PM["👤 Property Manager"]
    SA["👤 Sales Agent"]
    FI["👤 Finance"]
    TE["👤 Internal Technician"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        X(( ))
    end

    OP --- ERP
    AD --- ERP
    PM --- ERP
    SA --- ERP
    FI --- ERP
    TE --- ERP
```

| Actor | Who | Source |
|---|---|---|
| Platform Operator | The company running the platform | Cahier des Charges 01, section 4.1 |
| Administrator | Head of a real-estate company (organization) | Cahier des Charges 01, section 4.1 |
| Property Manager | Employee managing rentals and maintenance | Cahier des Charges 01, section 4.1 |
| Sales Agent | Employee managing sales | Cahier des Charges 01, section 4.1 |
| Finance | Accountant or administrative assistant | Cahier des Charges 01, section 4.1 |
| Internal Technician | Employee performing maintenance work | Cahier des Charges 01, section 4.1 |

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-08 | Use case diagram, step 1: actors. |
