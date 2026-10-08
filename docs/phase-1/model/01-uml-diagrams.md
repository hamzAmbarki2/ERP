# UML Diagrams

**Document:** UML Diagrams\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.2\
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

Actors are fixed in the system, not job titles. Each organization names
and configures its own roles (decision D-011), so job titles such as
"Property Manager" are role templates, not actors.

``` mermaid
flowchart LR
    AD["👤 Organization Administrator"]
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        X(( ))
    end

    AD --- ERP
    EM --- ERP
    AD -->|"generalization: is a kind of"| EM
```

The Organization Administrator is a special kind of Employee: they can
do everything any employee can do (including a technician's work), plus
the administration tasks.

| Actor | Definition |
|---|---|
| Organization Administrator | Head of a real-estate company. Every organization has one; the role cannot be modified. |
| Employee | Any other member of an organization. The use cases they can perform depend on the privileges of their role. |

There is **no generalization** between the two actors: the Organization
Administrator is not a kind of Employee and does not inherit employee
work. Example: the administrator does not perform a technician's
maintenance work.

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-08 | Document created. Use case diagram, step 1: actors (Organization Administrator, Employee). |
| 0.2 | 2026-10-08 | Use case diagram, step 2: Organization Administrator generalizes Employee. |
