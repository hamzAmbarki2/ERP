# Workflow Diagrams

**Document:** Workflow Diagrams\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.1\
**Status:** In progress --- built step by step, one question at a time\
**Date:** 2026-10-08

------------------------------------------------------------------------

## 1. The big picture

People who **log in** (users) versus people the staff **manage**
(records). Owners, tenants, vendors and buyers exist in the ERP from
the first version, as records managed by the staff. They do not log in
and have no interface of their own.

``` mermaid
flowchart TD
    P["Platform (the software)"]
    P --> O1["Organization: Agence Médina"]
    P --> O2["Organization: Agence Carthage"]

    O1 --> STAFF["Employees (users who log in)<br/>Administrator · Property Manager · Sales Agent<br/>Finance · Internal Technician<br/>— one interface per employee —"]
    STAFF -->|manage| REC["Records managed by the staff<br/>(no login, no interface)<br/>Owners · Tenants · Vendors · Buyers"]
```

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-08 | Step 1: platform, organizations, employees, managed records. |
