# UML Diagrams

**Document:** UML Diagrams\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.12\
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
        P9(["Archive a property<br/><i>box: Properties - Delete</i>"])
        P10(["Search and filter properties<br/><i>box: Properties - View</i>"])
        P11(["View a property's history<br/><i>box: Properties - View</i>"])
        P12(["Add many units at once<br/><i>box: Properties - Create</i>"])
        P13(["Copy a unit<br/><i>box: Properties - Create</i>"])
        P14(["View properties on a map<br/><i>box: Properties - View</i>"])
        P15(["Print a property sheet<br/><i>box: Properties - View</i>"])
        P16(["Export properties to Excel<br/><i>box: Properties - View</i>"])
    end

    EM --- P1
    EM --- P2
    EM --- P3
    EM --- P4
    EM --- P5
    EM --- P6
    EM --- P7
    EM --- P8
    EM --- P9
    EM --- P10
    EM --- P11
    EM --- P12
    EM --- P13
    EM --- P14
    EM --- P15
    EM --- P16
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
| 9 | Archive a property: hide it from the lists, keep its history | Properties: Delete |
| 10 | Search and filter properties (city, type, status, branch) | Properties: View |
| 11 | View a property's history (changes, past tenants, past repairs) | Properties: View |
| 12 | Add many units at once (example: 5 floors × 4 apartments) | Properties: Create |
| 13 | Copy a unit: new unit pre-filled with the description of an existing one (never tenants, leases, payments or history) | Properties: Create |
| 14 | View properties on a map | Properties: View |
| 15 | Print a property sheet (details and photos on one page) | Properties: View |
| 16 | Export the list of properties to Excel | Properties: View |

**Delete or archive (rule agreed in question 10):**

-   **Delete** works only when the property or unit has **no leases and
    no payments**, for example one created by mistake. Then everything
    inside it is deleted too: units, photos, documents.
-   **Archive** is used when the property has history. It disappears
    from the lists, but its history is kept. Money records are never
    erased (Phase 0 §0.18).

### Step 6 --- Employee's buttons: Owners

The Owners area keeps the people and companies who own the properties
the agency manages or sells, which properties each one owns (and what
share, when several people own one property), and the agreement between
the owner and the agency.

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        O1(["View owners<br/><i>box: Owners - View</i>"])
        O2(["Add an owner<br/><i>box: Owners - Create</i>"])
        O3(["Edit an owner<br/><i>box: Owners - Update</i>"])
        O4(["Link an owner to a property or unit, with their share<br/><i>box: Owners - Update</i>"])
        O5(["Record a change of owner with its date<br/><i>box: Owners - Update</i>"])
        O6(["Add a management agreement<br/><i>box: Owners - Create</i>"])
        O7(["Attach documents<br/><i>box: Owners - Update</i>"])
        O8(["View an owner's properties<br/><i>box: Owners - View</i>"])
        O9(["Delete or archive an owner<br/><i>box: Owners - Delete</i>"])
        O10(["Search and filter owners<br/><i>box: Owners - View</i>"])
        O11(["View an owner's history<br/><i>box: Owners - View</i>"])
        O12(["Renew or end a management agreement<br/><i>box: Owners - Update</i>"])
        O13(["View agreements ending soon<br/><i>box: Owners - View</i>"])
        O14(["Add a note<br/><i>box: Owners - Update</i>"])
        O15(["Send an email to an owner<br/><i>box: Owners - View</i>"])
        O16(["Merge two owners<br/><i>box: Owners - Update</i>"])
        O17(["Print an owner sheet<br/><i>box: Owners - View</i>"])
        O18(["Export owners to Excel<br/><i>box: Owners - View</i>"])
        O19(["View an owner's summary<br/><i>box: Owners - View</i>"])
        O20(["Manage a group of co-owners<br/><i>box: Owners - Update</i>"])
        O21(["See who owned a unit on a given date<br/><i>box: Owners - View</i>"])
        O22(["Set an owner's repair approval limit<br/><i>box: Owners - Update</i>"])
        O23(["Record an owner's approval<br/><i>box: Owners - Update</i>"])
        O24(["Set the management fee per property<br/><i>box: Owners - Update</i>"])
        O25(["Transfer all of an owner's properties in one action<br/><i>box: Owners - Update</i>"])
        O26(["List owners with missing information<br/><i>box: Owners - View</i>"])
        O27(["Send a message to several owners at once<br/><i>box: Owners - View</i>"])
    end

    EM --- O1
    EM --- O2
    EM --- O3
    EM --- O4
    EM --- O5
    EM --- O6
    EM --- O7
    EM --- O8
    EM --- O9
    EM --- O10
    EM --- O11
    EM --- O12
    EM --- O13
    EM --- O14
    EM --- O15
    EM --- O16
    EM --- O17
    EM --- O18
    EM --- O19
    EM --- O20
    EM --- O21
    EM --- O22
    EM --- O23
    EM --- O24
    EM --- O25
    EM --- O26
    EM --- O27
```

| # | Button | Box that must be ticked |
|---|---|---|
| 1 | View owners | Owners: View |
| 2 | Add an owner (person or company) | Owners: Create |
| 3 | Edit an owner (contact details, bank account) | Owners: Update |
| 4 | Link an owner to a property or unit, with their share (example: 50%) | Owners: Update |
| 5 | Record a change of owner with its date (sold, inherited) | Owners: Update |
| 6 | Add a management agreement (what the agency manages, dates, fee) | Owners: Create |
| 7 | Attach documents (ID card, ownership title, signed agreement) | Owners: Update |
| 8 | View an owner's properties | Owners: View |
| 9 | Delete or archive an owner (same rule as properties) | Owners: Delete |
| 10 | Search and filter owners (name, city, person or company, branch) | Owners: View |
| 11 | View an owner's history (past properties, past agreements, changes) | Owners: View |
| 12 | Renew or end a management agreement | Owners: Update |
| 13 | View agreements ending soon, to renew them in time | Owners: View |
| 14 | Add a note (call, meeting, request from the owner) | Owners: Update |
| 15 | Send an email to an owner from the ERP, keeping a copy | Owners: View |
| 16 | Merge two owners: the same person entered twice is joined into one owner, with all properties, documents, notes and history | Owners: Update |
| 17 | Print an owner sheet (details and properties on one page) | Owners: View |
| 18 | Export owners to Excel | Owners: View |
| 19 | View an owner's summary: properties, rented or empty units, repairs in progress, money due to them | Owners: View (money figures also need Finance: View) |
| 20 | Manage a group of co-owners (example: 3 brothers who inherit an apartment): shares must add up to 100%, and **each co-owner receives their own copy** of every letter and document | Owners: Update |
| 21 | See who owned a unit on a given date (example: sold on 15 March, so March rent is split between the old and the new owner) | Owners: View |
| 22 | Set an owner's repair approval limit (example: above 500 TND, the owner must agree before work starts) | Owners: Update |
| 23 | Record an owner's approval given by phone or in person, with the date, as proof | Owners: Update |
| 24 | Set the management fee per property (example: 8% on apartments, 10% on a shop) | Owners: Update |
| 25 | Transfer all of an owner's properties in one action (example: heirs take over 6 apartments at once) | Owners: Update |
| 26 | List owners with missing information (no bank account, ID card or signed agreement) | Owners: View |
| 27 | Send a message to several owners at once (example: new office address) | Owners: View |

Owner statements (what the agency owes each owner) belong to the
Finance area.

### Step 7 --- Employee's buttons: Tenants and leases

The Tenants and leases area keeps the people and companies who rent
units, and their lease contracts: who rents which unit, from when to
when, for how much rent, and with what deposit. It also follows each
lease through its life: signed, renewed, ended.

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        L1(["View tenants and leases<br/><i>box: Tenants and leases - View</i>"])
        L2(["Add a tenant<br/><i>box: Tenants and leases - Create</i>"])
        L3(["Edit a tenant<br/><i>box: Tenants and leases - Update</i>"])
        L4(["Create a lease<br/><i>box: Tenants and leases - Create</i>"])
        L5(["Add a guarantor to a lease<br/><i>box: Tenants and leases - Update</i>"])
        L6(["Activate a lease<br/><i>box: Tenants and leases - Update</i>"])
        L7(["Change the rent<br/><i>box: Tenants and leases - Update</i>"])
        L8(["Renew a lease<br/><i>box: Tenants and leases - Update</i>"])
        L9(["End a lease<br/><i>box: Tenants and leases - Update</i>"])
        L10(["Attach documents<br/><i>box: Tenants and leases - Update</i>"])
        L11(["Delete or archive a tenant or lease<br/><i>box: Tenants and leases - Delete</i>"])
        L12(["Search and filter tenants and leases<br/><i>box: Tenants and leases - View</i>"])
        L13(["View leases ending soon<br/><i>box: Tenants and leases - View</i>"])
        L14(["Record the move-in check<br/><i>box: Tenants and leases - Update</i>"])
        L15(["Record the move-out check<br/><i>box: Tenants and leases - Update</i>"])
        L16(["Return the deposit<br/><i>box: Tenants and leases - Update</i>"])
        L17(["Put several tenants on one lease<br/><i>box: Tenants and leases - Update</i>"])
        L18(["One lease for several units<br/><i>box: Tenants and leases - Create</i>"])
        L19(["Track the lease registration<br/><i>box: Tenants and leases - Update</i>"])
        L20(["Add a note<br/><i>box: Tenants and leases - Update</i>"])
        L21(["Send an email to a tenant<br/><i>box: Tenants and leases - View</i>"])
        L22(["Print a lease<br/><i>box: Tenants and leases - View</i>"])
        L23(["View a tenant's history<br/><i>box: Tenants and leases - View</i>"])
        L24(["Merge two tenants<br/><i>box: Tenants and leases - Update</i>"])
        L25(["Export tenants and leases to Excel<br/><i>box: Tenants and leases - View</i>"])
    end

    EM --- L1
    EM --- L2
    EM --- L3
    EM --- L4
    EM --- L5
    EM --- L6
    EM --- L7
    EM --- L8
    EM --- L9
    EM --- L10
    EM --- L11
    EM --- L12
    EM --- L13
    EM --- L14
    EM --- L15
    EM --- L16
    EM --- L17
    EM --- L18
    EM --- L19
    EM --- L20
    EM --- L21
    EM --- L22
    EM --- L23
    EM --- L24
    EM --- L25
```

| # | Button | Box that must be ticked |
|---|---|---|
| 1 | View tenants and leases | Tenants and leases: View |
| 2 | Add a tenant (person or company) | Tenants and leases: Create |
| 3 | Edit a tenant (contact details, ID) | Tenants and leases: Update |
| 4 | Create a lease (tenant, unit, dates, rent, deposit) | Tenants and leases: Create |
| 5 | Add a guarantor to a lease | Tenants and leases: Update |
| 6 | Activate a lease once signed: the unit becomes "rented" | Tenants and leases: Update |
| 7 | Change the rent, with the date it starts | Tenants and leases: Update |
| 8 | Renew a lease | Tenants and leases: Update |
| 9 | End a lease when the tenant leaves: the unit becomes "available" | Tenants and leases: Update |
| 10 | Attach documents (signed lease, ID card) | Tenants and leases: Update |
| 11 | Delete or archive a tenant or lease (same rule as properties) | Tenants and leases: Delete |
| 12 | Search and filter tenants and leases (example: all leases in Tunis Nord ending this year) | Tenants and leases: View |
| 13 | View leases ending soon (next 60 days), to renew them or find a new tenant in time | Tenants and leases: View |
| 14 | Record the move-in check (état des lieux d'entrée): condition of each room, with photos | Tenants and leases: Update |
| 15 | Record the move-out check and compare it with the move-in check | Tenants and leases: Update |
| 16 | Return the deposit fully or partly, with the reason (example: 2,000 TND deposit, 200 TND kept for a broken window). The deposit is money paid by the tenant at the start of the lease as a guarantee against damage or unpaid rent | Tenants and leases: Update |
| 17 | Put several tenants on one lease (couple, flatmates) | Tenants and leases: Update |
| 18 | One lease for several units (example: a company rents 3 offices and 2 parking spaces) | Tenants and leases: Create |
| 19 | Track the lease registration at the tax office (date, receipt), with a warning before the 60-day limit | Tenants and leases: Update |
| 20 | Add a note (call, complaint, request from the tenant) | Tenants and leases: Update |
| 21 | Send an email to a tenant from the ERP, keeping a copy | Tenants and leases: View |
| 22 | Print a lease, filled with the tenant's and unit's details, ready to sign | Tenants and leases: View |
| 23 | View a tenant's history (past leases and units, payment punctuality) | Tenants and leases: View |
| 24 | Merge two tenants: the same tenant entered twice is joined into one | Tenants and leases: Update |
| 25 | Export tenants and leases to Excel | Tenants and leases: View |

Rent invoices and payments belong to the Finance area.

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
| 0.7 | 2026-10-08 | Question 10: delete-or-archive rule; eight more Properties buttons. |
| 0.8 | 2026-10-09 | Step 6: nine Owners buttons for the Employee. |
| 0.9 | 2026-10-09 | Nine more Owners buttons (search, history, agreements, notes, email, merge, print, export). |
| 0.10 | 2026-10-09 | Nine advanced Owners buttons (summary, co-owner groups, ownership by date, repair approval limit and approvals, fee per property, bulk transfer, missing information, group message). |
| 0.11 | 2026-10-09 | Step 7: eleven Tenants and leases buttons for the Employee. |
| 0.12 | 2026-10-09 | Fourteen more Tenants and leases buttons (move-in and move-out checks, deposit return, several tenants or units per lease, lease registration, notes, email, print, history, merge, export). |
