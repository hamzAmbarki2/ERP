# UML Diagrams

**Document:** UML Diagrams\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.25\
**Status:** In progress --- built step by step, one question at a time\
**Date:** 2026-10-08

Diagrams in this document:

1.  Use case diagram --- *done for the MVP*
2.  Class diagram, MVP --- *in progress*
3.  Class diagram, V1
4.  Activity diagrams
5.  Sequence diagrams
6.  State diagrams

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
    administrator ticks boxes for each role. Example: at Médina Immobilier,
    the "Gestionnaire" can press "Create a lease"; at Carthage Immobilier,
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
        P17(["Record the shared parts of a building<br/><i>box: Properties - Create</i>"])
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
    EM --- P17
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
| 17 | Record the shared parts of a building (stairs, elevator, entrance, parking), so jobs can be done on them *(syndic)* | Properties: Create |

**Delete or archive (rule agreed in question 10):**

-   **Delete** works only when the property or unit has **no leases and
    no payments**, for example one created by mistake. Then everything
    inside it is deleted too: units, photos, documents.
-   **Archive** is used when the property has history. It disappears
    from the lists, but its history is kept. Money records are never
    erased (Phase 0 §0.18).

### Step 6 --- Employee's buttons: Owners

The Owners area keeps the people and companies who own the properties
bought from the company or whose properties the company manages or
sells, which properties each one owns (and what share, when several
people own one property), and the agreement between the owner and the
company.

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
        O28(["Record each owner's share of the building's costs<br/><i>box: Owners - Update</i>"])
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
    EM --- O28
```

| # | Button | Box that must be ticked |
|---|---|---|
| 1 | View owners | Owners: View |
| 2 | Add an owner (person or company) | Owners: Create |
| 3 | Edit an owner (contact details, bank account) | Owners: Update |
| 4 | Link an owner to a property or unit, with their share (example: 50%) | Owners: Update |
| 5 | Record a change of owner with its date (sold, inherited) | Owners: Update |
| 6 | Add a management agreement (what the company manages for the owner, dates, fee) | Owners: Create |
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
| 28 | Record each owner's share of the building's costs (example: A1 = 8%, A2 = 5%; a big apartment pays more) *(syndic)* | Owners: Update |

Owner statements (what the company owes each owner) belong to the
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
        L26(["Record items left by a tenant<br/><i>box: Tenants and leases - Update</i>"])
        L27(["Record a rent request<br/><i>box: Tenants and leases - Create</i>"])
        L28(["Record what they are looking for<br/><i>box: Tenants and leases - Update</i>"])
        L29(["Plan or record a viewing for a rent request<br/><i>box: Tenants and leases - Create</i>"])
        L30(["Find rent requests that match a free unit<br/><i>box: Tenants and leases - View</i>"])
        L31(["Turn a rent request into a tenant and a lease<br/><i>box: Tenants and leases - Create</i>"])
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
    EM --- L26
    EM --- L27
    EM --- L28
    EM --- L29
    EM --- L30
    EM --- L31
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
| 26 | Record items left by a tenant: what was left (with photos), where it is kept, when the tenant was contacted and the deadline to collect it, and the outcome (returned with date and signature, or given away or thrown out after the deadline). How long items must be kept may depend on the law (to check with the accountant or lawyer) | Tenants and leases: Update |
| 27 | Record a rent request: Someone asks to rent (example: Ines calls: "2-room apartment near Lac 2, maximum 900 TND a month"); the request is recorded with their contact details | Tenants and leases: Create |
| 28 | Record what they are looking for: Budget, number of rooms, area, move-in date | Tenants and leases: Update |
| 29 | Plan or record a viewing for a rent request: Example: "Ines visits A3 on Wednesday at 17:00" | Tenants and leases: Create |
| 30 | Find rent requests that match a free unit: When a unit becomes free, the ERP lists the requests that fit it (simple rules, no AI) | Tenants and leases: View |
| 31 | Turn a rent request into a tenant and a lease: When the lease is signed, the person becomes a tenant; nothing is retyped | Tenants and leases: Create |

**Rent requests** (buttons 27 to 31): a rent request is a person or
company who wants to rent but has no lease yet. The staff record it, so
nobody interested is forgotten and free units are filled faster. The
person has no interface (decision D-010).

Rent invoices and payments belong to the Finance area.

### Step 8 --- Employee's buttons: Finance

The Finance area follows all the money: rent the tenants must pay,
payments they make (cash, cheque, transfer), who still owes money, and
costs paid for properties. It also calculates what the company owes each
owner, after taking its fee, and records when the owner is paid.

Finance uses **three boxes**, so that money access can be given
precisely (example: a technician gets none of them; an accountant gets
all three): **Invoices and payments**, **Expenses**, **Owner
statements**.

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        F1(["View invoices, payments and balances<br/><i>box: Invoices and payments - View</i>"])
        F2(["Generate the monthly rent invoices<br/><i>box: Invoices and payments - Create</i>"])
        F3(["Create an invoice by hand<br/><i>box: Invoices and payments - Create</i>"])
        F4(["Record a payment<br/><i>box: Invoices and payments - Create</i>"])
        F5(["Link a payment to invoices<br/><i>box: Invoices and payments - Update</i>"])
        F6(["Print or send a receipt<br/><i>box: Invoices and payments - View</i>"])
        F7(["Cancel an invoice with a credit note<br/><i>box: Invoices and payments - Delete</i>"])
        F8(["View unpaid rent<br/><i>box: Invoices and payments - View</i>"])
        F9(["Record an expense for a property<br/><i>box: Expenses - Create</i>"])
        F10(["View expenses<br/><i>box: Expenses - View</i>"])
        F11(["Prepare an owner statement<br/><i>box: Owner statements - Create</i>"])
        F12(["Send an owner statement<br/><i>box: Owner statements - View</i>"])
        F13(["Record a payment to an owner<br/><i>box: Owner statements - Update</i>"])
        F14(["Follow a cheque<br/><i>box: Invoices and payments - Update</i>"])
        F15(["Record a bounced cheque<br/><i>box: Invoices and payments - Update</i>"])
        F16(["Send a payment reminder by hand<br/><i>box: Invoices and payments - View</i>"])
        F17(["Set up a payment plan<br/><i>box: Invoices and payments - Create</i>"])
        F18(["Refund a tenant<br/><i>box: Invoices and payments - Create</i>"])
        F19(["View deposits held<br/><i>box: Invoices and payments - View</i>"])
        F20(["Choose who pays an expense<br/><i>box: Expenses - Update</i>"])
        F21(["Attach a bill to an expense<br/><i>box: Expenses - Update</i>"])
        F22(["View money in and out per property<br/><i>box: Invoices and payments - View</i>"])
        F23(["Close a month<br/><i>box: Invoices and payments - Update</i>"])
        F24(["Check payments against the bank statement<br/><i>box: Invoices and payments - Update</i>"])
        F25(["View the company's own income<br/><i>box: Invoices and payments - View</i>"])
        F26(["Export for the accountant<br/><i>box: Invoices and payments - View</i>"])
        F27(["Ask owners for the building fees<br/><i>box: Invoices and payments - Create</i>"])
        F28(["View the building's money<br/><i>box: Invoices and payments - View</i>"])
    end

    EM --- F1
    EM --- F2
    EM --- F3
    EM --- F4
    EM --- F5
    EM --- F6
    EM --- F7
    EM --- F8
    EM --- F9
    EM --- F10
    EM --- F11
    EM --- F12
    EM --- F13
    EM --- F14
    EM --- F15
    EM --- F16
    EM --- F17
    EM --- F18
    EM --- F19
    EM --- F20
    EM --- F21
    EM --- F22
    EM --- F23
    EM --- F24
    EM --- F25
    EM --- F26
    EM --- F27
    EM --- F28
```

| # | Button | What it does | Box that must be ticked |
|---|---|---|---|
| 1 | View invoices, payments and balances | See what each tenant was asked to pay and what they paid | Invoices and payments: View |
| 2 | Generate the monthly rent invoices | Create all rent invoices of the month in one go, from the active leases | Invoices and payments: Create |
| 3 | Create an invoice by hand | Example: charge a tenant 150 TND for a lost key | Invoices and payments: Create |
| 4 | Record a payment | Cash, cheque or bank transfer, with the date | Invoices and payments: Create |
| 5 | Link a payment to invoices | Example: one payment of 2,000 TND pays January and February | Invoices and payments: Update |
| 6 | Print or send a receipt | Proof of payment for the tenant | Invoices and payments: View |
| 7 | Cancel an invoice with a credit note | Mistakes are corrected with a new record, never erased | Invoices and payments: Delete |
| 8 | View unpaid rent | List of tenants who are late, and by how much | Invoices and payments: View |
| 9 | Record an expense for a property | Example: 300 TND plumber repair in apartment A1 | Expenses: Create |
| 10 | View expenses | All costs per property, owner or period | Expenses: View |
| 11 | Prepare an owner statement | Rent collected, minus expenses, minus the company's management fee, gives the amount due to the owner | Owner statements: Create |
| 12 | Send an owner statement | By email, as a PDF, to the owner (each co-owner gets a copy) | Owner statements: View |
| 13 | Record a payment to an owner | The company transfers the amount due to the owner | Owner statements: Update |
| 14 | Follow a cheque | A cheque goes through steps: received, deposited at the bank, then cleared or bounced | Invoices and payments: Update |
| 15 | Record a bounced cheque | The payment is cancelled, the tenant owes the rent again, and the bank fee can be charged to them | Invoices and payments: Update |
| 16 | Send a payment reminder by hand | Besides the automatic reminders (example: "Your March rent is 10 days late") | Invoices and payments: View |
| 17 | Set up a payment plan | Example: a tenant owes 3,000 TND and pays 500 TND extra each month for 6 months | Invoices and payments: Create |
| 18 | Refund a tenant | Example: the tenant paid twice by mistake | Invoices and payments: Create |
| 19 | View deposits held | All deposits kept by the company, per tenant and property | Invoices and payments: View |
| 20 | Choose who pays an expense | Owner, tenant (if they broke it) or the company | Expenses: Update |
| 21 | Attach a bill to an expense | Photo or PDF of the supplier's bill, kept as proof | Expenses: Update |
| 22 | View money in and out per property | Example: apartment A1 this year, 12,000 TND rent in, 900 TND repairs out | Invoices and payments: View |
| 23 | Close a month | Lock a checked month so nobody can change its figures afterwards | Invoices and payments: Update |
| 24 | Check payments against the bank statement | Compare the bank statement with recorded payments and find what is missing | Invoices and payments: Update |
| 25 | View the company's own income | Management fees and sales commissions earned by the company | Invoices and payments: View |
| 26 | Export for the accountant | Download invoices, payments and expenses of a period to Excel | Invoices and payments: View |
| 27 | Ask owners for the building fees | Every month or quarter, each owner receives their part of the building's costs; their payments are followed like rent *(syndic)* | Invoices and payments: Create |
| 28 | View the building's money | Fees collected minus costs paid (cleaner, guard, elevator) = what is left for the building *(syndic)* | Invoices and payments: View |

Taxes (VAT, withholding tax, electronic invoicing) are left for the
accountant's review (see the legal points discussed on 2026-10-09).

### Step 9 --- Employee's buttons: Maintenance

The Maintenance area handles all work done on the properties, from the
moment it is reported or planned until it is checked. This is **not
only repairs** (water leak, broken door): it also covers **cleaning**
and **detailing** (preparing a property so it looks its best). Each job
is done by an internal technician or an outside vendor.

Maintenance uses **two boxes**: **Maintenance requests** (the problem
or need reported) and **Work orders** (the job). A technician can get
only *Work orders*, limited to the jobs given to them.

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        M1(["View maintenance requests<br/><i>box: Maintenance requests - View</i>"])
        M2(["Record a maintenance request<br/><i>box: Maintenance requests - Create</i>"])
        M3(["Edit a request<br/><i>box: Maintenance requests - Update</i>"])
        M4(["Turn a request into a work order<br/><i>box: Work orders - Create</i>"])
        M5(["Assign a work order<br/><i>box: Work orders - Update</i>"])
        M6(["Plan the date of the work<br/><i>box: Work orders - Update</i>"])
        M7(["View work orders<br/><i>box: Work orders - View</i>"])
        M8(["Update the progress<br/><i>box: Work orders - Update</i>"])
        M9(["Add notes and photos<br/><i>box: Work orders - Update</i>"])
        M10(["Record materials and hours<br/><i>box: Work orders - Update</i>"])
        M11(["Verify the work<br/><i>box: Work orders - Update</i>"])
        M12(["Cancel a request or work order<br/><i>box: Work orders - Delete</i>"])
        M13(["Create the expense from a finished work order<br/><i>box: Expenses - Create</i>"])
        M14(["Choose the type of job<br/><i>box: Work orders - Update</i>"])
        M15(["Plan recurring jobs<br/><i>box: Work orders - Create</i>"])
        M16(["Prepare a unit for a new tenant<br/><i>box: Work orders - Create</i>"])
        M17(["Prepare a unit for sale or a viewing<br/><i>box: Work orders - Create</i>"])
        M18(["Use a checklist during a job<br/><i>box: Work orders - Update</i>"])
        M19(["Create checklist templates<br/><i>box: Work orders - Create</i>"])
        M20(["View the maintenance calendar<br/><i>box: Work orders - View</i>"])
        M21(["Reopen a job not well done<br/><i>box: Work orders - Update</i>"])
        M22(["View a property's maintenance history<br/><i>box: Maintenance requests - View</i>"])
        M23(["Export maintenance to Excel<br/><i>box: Work orders - View</i>"])
        M24(["Report a found item<br/><i>box: Work orders - Update</i>"])
    end

    EM --- M1
    EM --- M2
    EM --- M3
    EM --- M4
    EM --- M5
    EM --- M6
    EM --- M7
    EM --- M8
    EM --- M9
    EM --- M10
    EM --- M11
    EM --- M12
    EM --- M13
    EM --- M14
    EM --- M15
    EM --- M16
    EM --- M17
    EM --- M18
    EM --- M19
    EM --- M20
    EM --- M21
    EM --- M22
    EM --- M23
    EM --- M24
    M24 -. "added to the list of" .-> L26REF(["Record items left by a tenant<br/><i>(Tenants and leases)</i>"])
```

| # | Button | What it does | Box that must be ticked |
|---|---|---|---|
| 1 | View maintenance requests | All reported problems and needs, with their status | Maintenance requests: View |
| 2 | Record a maintenance request | Example: a tenant calls, "water leak in the kitchen of A1" | Maintenance requests: Create |
| 3 | Edit a request | Change the description or the priority (urgent, normal, low) | Maintenance requests: Update |
| 4 | Turn a request into a work order | The request becomes a job | Work orders: Create |
| 5 | Assign a work order | Give the job to an internal technician or a vendor | Work orders: Update |
| 6 | Plan the date of the work | Example: Thursday 10:00 | Work orders: Update |
| 7 | View work orders | A technician sees only the jobs given to them | Work orders: View |
| 8 | Update the progress | Started, waiting for parts, finished | Work orders: Update |
| 9 | Add notes and photos | Before and after photos, comments | Work orders: Update |
| 10 | Record materials and hours | Example: 1 pipe and 2 hours of work | Work orders: Update |
| 11 | Verify the work | A manager checks the job is well done before closing it | Work orders: Update |
| 12 | Cancel a request or work order | Duplicate, or no longer needed | Work orders: Delete |
| 13 | Create the expense from a finished work order | The job cost goes to Finance in one click | Expenses: Create |
| 14 | Choose the type of job | Repair, cleaning, deep cleaning or detailing, painting, gardening, pest control, inspection | Work orders: Update |
| 15 | Plan recurring jobs | Example: clean the building stairs every Monday, garden every 2 weeks, elevator check every month; the ERP creates the jobs automatically | Work orders: Create |
| 16 | Prepare a unit for a new tenant | When a tenant leaves, one click creates the usual jobs (deep cleaning, painting, small repairs, final check); when all are done, the unit becomes "ready to rent" | Work orders: Create |
| 17 | Prepare a unit for sale or a viewing | Detailing before photos or visits: deep cleaning, windows, small touch-ups | Work orders: Create |
| 18 | Use a checklist during a job | The cleaner ticks each item on their phone: kitchen, bathroom, windows, floors | Work orders: Update |
| 19 | Create checklist templates | Write a checklist once and reuse it (example: "standard cleaning", "deep cleaning before a new tenant") | Work orders: Create |
| 20 | View the maintenance calendar | All planned jobs by day or week, and who does what | Work orders: View |
| 21 | Reopen a job not well done | The job goes back to the person who did it, with a comment | Work orders: Update |
| 22 | View a property's maintenance history | Everything done on a unit: repairs, cleanings, dates, costs | Maintenance requests: View |
| 23 | Export maintenance to Excel | Download the list of jobs | Work orders: View |
| 24 | Report a found item | During a job (example: cleaning), the worker reports something the former tenant left; it is added to the same list as "Record items left by a tenant" | Work orders: Update |

### Step 10 --- Employee's buttons: Building security

Building security covers incidents, visitors, guards and keys. A guard
is either an **employee** of the company (with a role that gives only
the Security buttons) or comes from a **security company** (a vendor).

Security uses **one box**: **Security**.

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        S1(["Record a security incident<br/><i>box: Security - Create</i>"])
        S2(["View incidents<br/><i>box: Security - View</i>"])
        S3(["Record a visitor<br/><i>box: Security - Create</i>"])
        S4(["Plan the guards' shifts<br/><i>box: Security - Update</i>"])
        S5(["Record who holds the keys<br/><i>box: Security - Update</i>"])
    end

    EM --- S1
    EM --- S2
    EM --- S3
    EM --- S4
    EM --- S5
```

| # | Button | What it does | Box that must be ticked |
|---|---|---|---|
| 1 | Record a security incident | Example: "broken entrance lock, 2:00 am", with photos | Security: Create |
| 2 | View incidents | All incidents per building and date | Security: View |
| 3 | Record a visitor | Name, which unit they visit, time in and out | Security: Create |
| 4 | Plan the guards' shifts | Who guards which building, day or night | Security: Update |
| 5 | Record who holds the keys | Example: "key to A1 given to the plumber on 10 March, returned on 11 March" | Security: Update |

### Step 11 --- Employee's buttons: Vendors

The Vendors area keeps the outside companies and independent workers who
do jobs for the company (plumbers, electricians, cleaning companies,
elevator maintenance, security companies…): who they are, what they do,
their documents and contracts, and every job they did.

Vendors uses **one box**: **Vendors** (bills and payments use the
Finance box **Expenses**).

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        V1(["View vendors<br/><i>box: Vendors - View</i>"])
        V2(["Add a vendor<br/><i>box: Vendors - Create</i>"])
        V3(["Edit a vendor<br/><i>box: Vendors - Update</i>"])
        V4(["Add contact people<br/><i>box: Vendors - Update</i>"])
        V5(["Choose service types<br/><i>box: Vendors - Update</i>"])
        V6(["Activate or deactivate a vendor<br/><i>box: Vendors - Update</i>"])
        V7A(["Attach a document with its type and dates<br/><i>box: Vendors - Update</i>"])
        V7B(["Get warned before a document expires<br/><i>box: Vendors - View</i>"])
        V7C(["See each vendor's document status<br/><i>box: Vendors - View</i>"])
        V7E(["Ask the vendor for a new document<br/><i>box: Vendors - View</i>"])
        V7F(["Record the contract terms<br/><i>box: Vendors - Update</i>"])
        V8(["View a vendor's job history<br/><i>box: Vendors - View</i>"])
        V9(["Add a note<br/><i>box: Vendors - Update</i>"])
        V10(["Record a vendor's bill<br/><i>box: Expenses - Create</i>"])
        V11(["Delete or archive a vendor<br/><i>box: Vendors - Delete</i>"])
        V12(["Search and filter vendors<br/><i>box: Vendors - View</i>"])
        V13(["Rate a vendor after a job<br/><i>box: Vendors - Update</i>"])
        V14(["Compare vendors for one service type<br/><i>box: Vendors - View</i>"])
        V15(["Ask vendors for quotes and choose one<br/><i>box: Vendors - Create</i>"])
        V16(["View what is owed to each vendor<br/><i>box: Expenses - View</i>"])
        V17(["Record a payment to a vendor<br/><i>box: Expenses - Update</i>"])
        V18(["Mark a preferred vendor<br/><i>box: Vendors - Update</i>"])
        V19(["Merge two vendors<br/><i>box: Vendors - Update</i>"])
        V20(["Export vendors to Excel<br/><i>box: Vendors - View</i>"])
    end

    EM --- V1
    EM --- V2
    EM --- V3
    EM --- V4
    EM --- V5
    EM --- V6
    EM --- V7A
    EM --- V7B
    EM --- V7C
    EM --- V7E
    EM --- V7F
    EM --- V8
    EM --- V9
    EM --- V10
    EM --- V11
    EM --- V12
    EM --- V13
    EM --- V14
    EM --- V15
    EM --- V16
    EM --- V17
    EM --- V18
    EM --- V19
    EM --- V20
```

| # | Button | What it does | Box that must be ticked |
|---|---|---|---|
| 1 | View vendors | The list of all vendors | Vendors: View |
| 2 | Add a vendor | A company or an individual (example: "Plomberie Ben Salah") | Vendors: Create |
| 3 | Edit a vendor | Address, phone, bank account, tax number | Vendors: Update |
| 4 | Add contact people | The boss, the secretary, the worker who comes on site | Vendors: Update |
| 5 | Choose service types | Plumbing, electricity, cleaning, elevator, painting, security… | Vendors: Update |
| 6 | Activate or deactivate a vendor | Stop working with a vendor without losing their history | Vendors: Update |
| 7a | Attach a document with its type and dates | Contract, insurance certificate, professional license, tax certificate, social-security (CNSS) certificate, each with its validity dates; old versions are kept | Vendors: Update |
| 7b | Get warned before a document expires | Example: "Plomberie Ben Salah's insurance ends in 30 days" | Vendors: View |
| 7c | See each vendor's document status | Green: all valid; orange: one expires soon; red: expired or missing | Vendors: View |
| 7e | Ask the vendor for a new document | The ERP emails the vendor to send an updated document | Vendors: View |
| 7f | Record the contract terms | Services, price, duration, renewal (example: elevator check, 300 TND/month, 1 year, renews automatically) | Vendors: Update |
| 8 | View a vendor's job history | All jobs done: where, when, how much, how well | Vendors: View |
| 9 | Add a note | Example: "always late", "good price for big jobs" | Vendors: Update |
| 10 | Record a vendor's bill | The bill for a job, linked to that job and turned into an expense | Expenses: Create |
| 11 | Delete or archive a vendor | Same rule as properties | Vendors: Delete |
| 12 | Search and filter vendors | By service type, city, status (example: all active electricians in Tunis) | Vendors: View |
| 13 | Rate a vendor after a job | Quality, punctuality and price, from 1 to 5 stars | Vendors: Update |
| 14 | Compare vendors for one service type | Average rating, average price, number of jobs | Vendors: View |
| 15 | Ask vendors for quotes and choose one | Ask several vendors for a price, record their quotes, pick one | Vendors: Create |
| 16 | View what is owed to each vendor | Unpaid vendor bills | Expenses: View |
| 17 | Record a payment to a vendor | Pay a vendor's bill | Expenses: Update |
| 18 | Mark a preferred vendor | Example: "for elevators in Résidence Yasmine, always call X first" | Vendors: Update |
| 19 | Merge two vendors | The same vendor entered twice is joined into one | Vendors: Update |
| 20 | Export vendors to Excel | Download the list | Vendors: View |

**Rule 7d:** before a job is given to a vendor whose document status is
red (expired or missing), the ERP warns: "This vendor's insurance has
expired. Assign anyway?"

Assigning a job to a vendor is done in Maintenance (button 5).
Contracts, compliance documents and quotes were planned for V1 in
Phase 0; they are now part of the MVP.

### Step 12 --- Employee's buttons: Sales

The Sales area follows the selling of a property or unit, from putting it
up for sale to the day the **buyer becomes the owner**. Most of the time
the company sells its **own** units; sometimes an owner asks the company
to resell their unit (sales mandate).

Sales uses **three boxes**: **Sales** (listings, offers, reservations,
closing), **Prospects and buyers** (people interested, viewings) and
**Buyer payments** (money received from buyers).

``` mermaid
flowchart LR
    EM["👤 Employee"]

    subgraph ERP["Real Estate Operations ERP"]
        direction TB
        SA1(["View units for sale<br/><i>box: Sales - View</i>"])
        SA2(["Put a unit up for sale<br/><i>box: Sales - Create</i>"])
        SA3(["Record a sales mandate<br/><i>box: Sales - Create</i>"])
        SA4(["Edit a listing<br/><i>box: Sales - Update</i>"])
        SA5(["Add a prospect<br/><i>box: Prospects and buyers - Create</i>"])
        SA6(["Plan or record a viewing<br/><i>box: Prospects and buyers - Create</i>"])
        SA7(["Record an offer<br/><i>box: Sales - Create</i>"])
        SA8(["Accept or refuse an offer<br/><i>box: Sales - Update</i>"])
        SA9(["Reserve the unit for the buyer<br/><i>box: Sales - Update</i>"])
        SA10(["Record a buyer payment<br/><i>box: Buyer payments - Create</i>"])
        SA11(["Close the sale<br/><i>box: Sales - Update</i>"])
        SA12(["Cancel a reservation or sale<br/><i>box: Sales - Delete</i>"])
        SA13(["Attach documents<br/><i>box: Sales - Update</i>"])
        SA14(["Search and filter listings<br/><i>box: Sales - View</i>"])
        SA15(["Record a prospect's needs<br/><i>box: Prospects and buyers - Update</i>"])
        SA16(["Find units that match a prospect<br/><i>box: Prospects and buyers - View</i>"])
        SA17(["Send a listing to a prospect<br/><i>box: Sales - View</i>"])
        SA18(["Record a counter-offer<br/><i>box: Sales - Update</i>"])
        SA19(["Get warned before a reservation expires<br/><i>box: Sales - View</i>"])
        SA20(["Set up a payment schedule<br/><i>box: Buyer payments - Create</i>"])
        SA21(["Follow buyer payments<br/><i>box: Buyer payments - View</i>"])
        SA22(["Record the commission on a mandate sale<br/><i>box: Sales - Update</i>"])
        SA23(["View the sales pipeline<br/><i>box: Sales - View</i>"])
        SA24(["Print sale documents<br/><i>box: Sales - View</i>"])
        SA25(["Add a note on a prospect<br/><i>box: Prospects and buyers - Update</i>"])
        SA26(["Merge two prospects<br/><i>box: Prospects and buyers - Update</i>"])
        SA27(["Sell a unit that has a tenant<br/><i>box: Sales - Update</i>"])
        SA28(["Check the buyer's identity<br/><i>box: Prospects and buyers - Update</i>"])
        SA29(["Export sales to Excel<br/><i>box: Sales - View</i>"])
    end

    EM --- SA1
    EM --- SA2
    EM --- SA3
    EM --- SA4
    EM --- SA5
    EM --- SA6
    EM --- SA7
    EM --- SA8
    EM --- SA9
    EM --- SA10
    EM --- SA11
    EM --- SA12
    EM --- SA13
    EM --- SA14
    EM --- SA15
    EM --- SA16
    EM --- SA17
    EM --- SA18
    EM --- SA19
    EM --- SA20
    EM --- SA21
    EM --- SA22
    EM --- SA23
    EM --- SA24
    EM --- SA25
    EM --- SA26
    EM --- SA27
    EM --- SA28
    EM --- SA29
```

| # | Button | What it does | Box that must be ticked |
|---|---|---|---|
| 1 | View units for sale | All listings and their status | Sales: View |
| 2 | Put a unit up for sale | Example: apartment B4, asking price 320,000 TND | Sales: Create |
| 3 | Record a sales mandate | An owner (for example someone who bought from the company) asks the company to resell their unit | Sales: Create |
| 4 | Edit a listing | Example: lower the price to 305,000 TND | Sales: Update |
| 5 | Add a prospect | A person or company interested in buying | Prospects and buyers: Create |
| 6 | Plan or record a viewing | Example: "Mrs. Y visits B4 on Saturday at 10:00" | Prospects and buyers: Create |
| 7 | Record an offer | Example: Mrs. Y offers 300,000 TND | Sales: Create |
| 8 | Accept or refuse an offer | Accepting leads to the reservation | Sales: Update |
| 9 | Reserve the unit for the buyer | With a deposit (arbon) and an expiry date, so nobody else can buy it meanwhile | Sales: Update |
| 10 | Record a buyer payment | Deposit, installment or final payment | Buyer payments: Create |
| 11 | Close the sale | Final deed signed: the buyer becomes the owner in the Owners area | Sales: Update |
| 12 | Cancel a reservation or sale | Record the reason and what happens to the deposit | Sales: Delete |
| 13 | Attach documents | Sale agreement, buyer's ID, final deed | Sales: Update |
| 14 | Search and filter listings | Example: all 3-room apartments under 350,000 TND in La Marsa | Sales: View |
| 15 | Record a prospect's needs | Budget, type, area, number of rooms | Prospects and buyers: Update |
| 16 | Find units that match a prospect | The ERP lists the units that fit the prospect's needs (simple rules, no AI) | Prospects and buyers: View |
| 17 | Send a listing to a prospect | Email with photos, price and plan | Sales: View |
| 18 | Record a counter-offer | Example: the company answers 310,000 TND to an offer of 300,000 TND | Sales: Update |
| 19 | Get warned before a reservation expires | Example: "the reservation of B4 ends in 3 days, final payment not received" | Sales: View |
| 20 | Set up a payment schedule | Example: 30% at signing, 40% in 6 months, 30% at delivery | Buyer payments: Create |
| 21 | Follow buyer payments | What each buyer paid and what is left | Buyer payments: View |
| 22 | Record the commission on a mandate sale | The company's fee when it resells a unit for an owner | Sales: Update |
| 23 | View the sales pipeline | Example this month: 40 prospects, 25 viewings, 8 offers, 3 reservations, 2 sales | Sales: View |
| 24 | Print sale documents | Reservation form and sale agreement, filled with the buyer's and unit's details | Sales: View |
| 25 | Add a note on a prospect | Example: "called back, wants a second visit" | Prospects and buyers: Update |
| 26 | Merge two prospects | The same person entered twice is joined into one | Prospects and buyers: Update |
| 27 | Sell a unit that has a tenant | The lease continues; from the sale date, rent goes to the new owner | Sales: Update |
| 28 | Check the buyer's identity | Required by the January 2026 anti-money-laundering rule for property sales (to confirm with the accountant) | Prospects and buyers: Update |
| 29 | Export sales to Excel | Download listings, offers and sales | Sales: View |

Buttons 16 (matching), 20 (payment schedule) and 22 (commission) were
planned for V1 in Phase 0 (D-009); they are now part of the MVP.

### V1 --- Employees' reports and dashboards

Reports and dashboards for employees come **after the administrator's
dashboard**: they are planned for **V1** (decision 2026-10-09).

### Note --- Syndic (managing the shared parts of a building)

When the company sells several units of a building, the building has
many owners, and its shared parts (stairs, elevator, entrance, guard,
cleaning) must be managed and paid for by all owners together. This is
the **syndic** work.

The syndic is **not a separate activity**: the ERP already covers most
of it (Maintenance for cleaning and repairs, Vendors for the companies
involved, Finance for paying costs). Four buttons, marked *(syndic)*,
were added to the existing areas:

-   Properties 17 --- Record the shared parts of a building
-   Owners 28 --- Record each owner's share of the building's costs
-   Finance 27 --- Ask owners for the building fees
-   Finance 28 --- View the building's money

A **dedicated syndic interface** comes **after the MVP**.

------------------------------------------------------------------------

## 2. Class diagram --- MVP

Built group by group. Attributes are added in a later step; this step
fixes the classes and how they are linked.

### Step 1 --- The company and its people

| Class | What it is |
|---|---|
| Organization | The real estate company (example: Médina Immobilier) |
| Branch | A branch of the company (example: Tunis Nord, Sousse) |
| Department | A department inside a branch (example: Location, Vente) |
| User | A person who logs in (one account per person) |
| Membership | The link between a user and a company |
| Role | A job role defined by the company (example: "Gestionnaire") |
| Privilege | One ticked box: a domain and an action (example: Leases / Create) |

``` mermaid
classDiagram
    class Organization
    class Branch
    class Department
    class User
    class Membership
    class Role
    class Privilege

    Organization "1" --> "*" Branch : has
    Branch "1" --> "*" Department : has
    Organization "1" --> "*" Role : defines
    Role "1" --> "*" Privilege : grants
    User "1" --> "*" Membership : has
    Organization "1" --> "*" Membership : has
    Membership "*" --> "*" Role : holds
    Membership "*" --> "1" Department : works in
```

How to read it: `"1" --> "*"` means "one … has many …". Example: one
organization has many branches; one user can have several memberships
(one per company they work for).

### Step 2 --- The properties

| Class | What it is |
|---|---|
| Property | A building, a house or a plot, at one address |
| Building | A building inside a property, only when needed |
| Floor | A floor of a building, only when needed |
| Unit | An apartment, office, shop or parking space: what is rented or sold |
| SharedPart | The stairs, elevator, entrance or parking of a building (syndic work) |

``` mermaid
classDiagram
    class Branch
    class Property
    class Building
    class Floor
    class Unit
    class SharedPart

    Branch "1" --> "*" Property : manages
    Property "1" --> "0..*" Building : contains
    Building "1" --> "0..*" Floor : contains
    Property "1" --> "1..*" Unit : contains
    Floor "0..1" --> "*" Unit : contains
    Property "1" --> "0..*" SharedPart : has
```

Notes:

-   Building and Floor are **optional**: a house is a property with one
    unit and no building or floor.
-   `"0..*"` means "zero or many"; `"0..1"` means "zero or one".

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
| 0.13 | 2026-10-09 | Step 8: Finance with three boxes and thirteen buttons. |
| 0.14 | 2026-10-09 | Thirteen more Finance buttons (cheques, reminders, payment plans, refunds, deposits, who pays, bills, money per property, month closing, bank check, company income, accountant export). |
| 0.15 | 2026-10-09 | Step 9: Maintenance with two boxes and thirteen buttons; maintenance includes repairs, cleaning and detailing. |
| 0.16 | 2026-10-09 | Ten more Maintenance buttons (job types, recurring jobs, unit preparation for a new tenant or a sale, checklists, calendar, reopen, history, export). |
| 0.17 | 2026-10-09 | Items left by a former tenant: "Record items left by a tenant" (Tenants and leases) and "Report a found item" (Maintenance). |
| 0.18 | 2026-10-09 | Syndic: four buttons in existing areas (shared parts, owner share, building fees, building money); dedicated syndic interface after the MVP. |
| 0.19 | 2026-10-09 | Step 10: Building security with one box and five buttons (incidents, visitors, guard shifts, keys). |
| 0.20 | 2026-10-09 | Step 11: Vendors with 24 buttons (documents with validity dates and alerts, contract terms, ratings, comparison, quotes, vendor bills and payments) and rule 7d. |
| 0.21 | 2026-10-09 | Step 12: Sales with three boxes and thirteen buttons. |
| 0.22 | 2026-10-09 | Sixteen more Sales buttons (search, needs and matching, counter-offers, reservation alerts, payment schedules, commission, pipeline, documents, notes, merge, sale of a rented unit, buyer identity check, export). |
| 0.23 | 2026-10-09 | Rent requests: five buttons in Tenants and leases (MVP). Employees' reports and dashboards planned for V1. |
| 0.24 | 2026-10-09 | Class diagram (MVP), step 1: the company and its people. |
| 0.25 | 2026-10-09 | Class diagram (MVP), step 2: the properties. |
