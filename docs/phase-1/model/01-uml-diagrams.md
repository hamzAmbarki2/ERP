# UML Diagrams

**Document:** UML Diagrams\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.6\
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
```

| Actor | Definition |
|---|---|
| Organization Administrator | Head of a real-estate company. Every organization has one; the role cannot be modified. |
| Employee | Any other member of an organization. The use cases they can perform depend on the privileges of their role. |

There is **no generalization** between the two actors: the Organization
Administrator is not a kind of Employee and does not inherit employee
work. Example: the administrator does not perform a technician's
maintenance work.

### Step 2 --- Organization Administrator's use cases

``` mermaid
flowchart LR
    AD["👤 Organization Administrator"]
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        A1([Set up the organization])
        A2([Invite a member])
        A8([Create or edit a branch])
        A3([Create or edit a department])
        A4([Create or edit a role])
        A5([Change a member's roles])
        A6([Deactivate a member])
        A7([View the audit log])
    end

    AD --- A1
    AD --- A2
    AD --- A8
    AD --- A3
    AD --- A4
    AD --- A5
    AD --- A6
    AD --- A7
```

| Use case | Source |
|---|---|
| Set up the organization | Cahier des Charges 01, section 7.1 |
| Invite a member | Cahier des Charges 01, UC-019 |
| Create or edit a branch | Answer to question 5: the administrator names the branches on a dedicated page |
| Create or edit a department | Cahier des Charges 01, UC-027; departments named on a dedicated page |
| Create or edit a role | Cahier des Charges 01, UC-028 |
| Change a member's roles | Cahier des Charges 01, UC-022 |
| Deactivate a member | Cahier des Charges 01, UC-023 |
| View the audit log | Cahier des Charges 01, UC-018 |

### Step 3 --- More Organization Administrator use cases

``` mermaid
flowchart LR
    AD["👤 Organization Administrator"]
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        B1([Reactivate a member])
        R0([Request a move to another department or branch])
        B2([Approve or refuse a move request])
        B3([Resend or cancel an invitation])
        B4([Reset a member's double authentication])
        B5([Configure company information])
        B6([Choose the activities: rental, sales, or both])
        B7([Assign properties to a branch])
        B8([Set up reminders])
        B9([Customize document headers])
        B10([View the company-wide dashboard])
        B11([View reports across branches and departments])
        B12([Approve large expenses])
        B13([Import data from Excel])
        B14([Export the company's data])
        B15([View the subscription])
    end

    EM --- R0
    R0 -. "approved through" .-> B2
    AD --- B1
    AD --- B2
    AD --- B3
    AD --- B4
    AD --- B5
    AD --- B6
    AD --- B7
    AD --- B8
    AD --- B9
    AD --- B10
    AD --- B11
    AD --- B12
    AD --- B13
    AD --- B14
    AD --- B15
```

| Use case | Meaning |
|---|---|
| Reactivate a member | Bring back an employee who left and came back, with their history. |
| Request a move to another department or branch | *(Employee)* The employee asks to change department or branch. |
| Approve or refuse a move request | The administrator accepts or refuses the employee's request; if accepted, the member is moved. |
| Resend or cancel an invitation | The email was lost, or sent to the wrong address. |
| Reset a member's double authentication | An employee lost their phone; the administrator unlocks their login. |
| Configure company information | Logo, legal name, tax number, default language. |
| Choose the activities | Rental, sales, or both; hides the unused menus. |
| Assign properties to a branch | Decide which branch handles which properties, and so who sees them. |
| Set up reminders | For example, rent reminder 5 days before the due date and 3 days after. |
| Customize document headers | Logo and company details on receipts, invoices, owner statements. |
| View the company-wide dashboard | Key numbers for the whole company on one screen. |
| View reports across branches and departments | Compare and total results for the whole company. |
| Approve large expenses | Expenses above an amount set by the company wait for approval. |
| Import data from Excel | Load existing properties, owners, tenants and leases. |
| Export the company's data | Download everything, for backup or when leaving. |
| View the subscription | Current plan and the platform's invoices. |

### Step 4 --- Employee's use cases: the principle

Every company has its own hierarchy, and real hierarchies are very
different from one company to another. So the software does **not**
contain any company's hierarchy. Instead:

1.  **We build the buttons.** The ERP has a fixed list of actions
    ("Create a lease", "Record a payment", "Assign a repair", "Close a
    sale"…). Every company gets the same buttons.
2.  **Each company decides who presses which button.** The
    administrator ticks boxes for each role. Example: at Agence Médina,
    the "Gestionnaire" can press "Create a lease"; at Agence Carthage,
    the "Chargé de location" can press the same button.
3.  **The hierarchy only decides who sees what.** The administrator
    draws their own branches and departments. The software then checks
    two things: did your role get this button, and is this property in
    your branch or department?

For the diagram: the **Employee** is linked to **all the buttons**. Next
to each button, the diagram shows which box must be ticked to use it.
The diagram is the same for every company.

*The buttons are added area by area in the next steps.*

### Step 5 --- Employee's buttons: Properties

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        P1(["View properties and units<br/><i>box: Properties - View</i>"])
        P2(["Add a property<br/><i>box: Properties - Create</i>"])
        P3(["Add a building or floor<br/><i>box: Properties - Create</i>"])
        P4(["Add a unit<br/><i>box: Properties - Create</i>"])
        P5(["Edit a property or unit<br/><i>box: Properties - Update</i>"])
        P6(["Change a unit's status<br/><i>box: Properties - Update</i>"])
        P7(["Attach photos or documents<br/><i>box: Properties - Update</i>"])
        P8(["Delete a property or unit<br/><i>box: Properties - Delete</i>"])
    end

    EM --- P1
    EM --- P2
    EM --- P3
    EM --- P4
    EM --- P5
    EM --- P6
    EM --- P7
    EM --- P8
```

| # | Button | Box that must be ticked |
|---|---|---|
| 1 | View properties and units | Properties: View |
| 2 | Add a property (building, house, shop…) | Properties: Create |
| 3 | Add a building or floor inside a property (only when needed) | Properties: Create |
| 4 | Add a unit (apartment, office, parking space…) | Properties: Create |
| 5 | Edit a property or unit | Properties: Update |
| 6 | Change a unit's status (available, rented, for sale, under repair) | Properties: Update |
| 7 | Attach photos or documents to a property | Properties: Update |
| 8 | Delete a property or unit | Properties: Delete |

------------------------------------------------------------------------

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-08 | Document created. Use case diagram, step 1: actors (Organization Administrator, Employee). |
| 0.2 | 2026-10-08 | Use case diagram, step 2: Organization Administrator's use cases. |
| 0.3 | 2026-10-08 | Use case diagram, step 3: fifteen more administrator use cases; a member requests a move, the administrator approves it. |
| 0.4 | 2026-10-08 | Question 6: Administrator and Employee are separate (generalization removed). Question 5: organizations have branches and departments; "Create or edit a branch" added. |
| 0.5 | 2026-10-08 | Question 7: the Employee's use cases are the full list of buttons; each company decides who can press them. |
| 0.6 | 2026-10-08 | Step 5: eight Properties buttons for the Employee. |
