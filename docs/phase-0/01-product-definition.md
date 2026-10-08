# Phase 0 --- Product Definition & Market Validation

**Project:** Cloud-Native Multi-Tenant Real Estate Operations ERP\
**Phase:** 0 --- Research, Product Definition, Scope & Validation\
**Status:** Living specification during discovery\
**Last updated:** 2026-10-08

**Change log**

-   2026-10-08 --- Property sales added to the product scope: new
    personas, workflows, MVP / V1 / V2 items, business rules and
    decision record (§0.29). Product renamed from "Property Management
    ERP" to "Real Estate Operations ERP".

------------------------------------------------------------------------

## 0.1 Product Definition

### Product

A **cloud-native, multi-tenant Real Estate Operations ERP** delivered as
a SaaS platform for small and mid-sized companies that manage and sell
real estate: property-management companies, real-estate agencies and
property developers (promoteurs immobiliers).

It covers two business activities on one shared property base:

1.  **rental management** --- managing rented properties on behalf of
    owners;
2.  **property sales** --- selling properties and units, either on
    behalf of owners or from the company's own stock.

The platform centralizes:

-   properties, buildings, floors and units
-   owners and ownership relationships
-   tenants and tenant records
-   leases and lease lifecycle
-   recurring rent and billing
-   sales mandates and units for sale
-   sales listings
-   prospects and buyers
-   viewings, offers, reservations and sales
-   payments, allocations, balances and receipts
-   maintenance requests and work orders
-   **internal technicians and external maintenance vendors**
-   expenses and operational costs
-   documents
-   notifications and communications
-   reporting and owner statements
-   audit trails and security events

The project is primarily a **business operations system**, not a
property marketplace.

### Product shape

-   B2B SaaS
-   Multi-tenant
-   Cloud-native
-   Web application
-   Mobile applications planned
-   API-first backend
-   Secure document storage
-   Strong authorization and tenant isolation
-   DevSecOps-oriented delivery
-   Observable and production-oriented architecture

### Initial localization

The first target geography is **Tunisia**, with architecture capable of
later expansion into the broader Maghreb / Francophone market.

Initial language expectations:

-   French
-   Arabic
-   English

Initial currency:

-   TND

The financial model should be designed so additional currencies can be
introduced later without redesigning the domain.

### Property types

Initial supported property types may include:

-   apartments
-   houses
-   offices
-   shops
-   warehouses
-   parking spaces
-   small mixed-use buildings

------------------------------------------------------------------------

## 0.2 Target Customer

### Primary customers

Small and mid-sized companies running real-estate operations:

-   **property-management companies** managing rental assets on behalf
    of property owners;
-   **real-estate agencies** renting and / or selling properties on
    behalf of owners (individuals or companies) under a mandate;
-   **property developers (promoteurs immobiliers)** selling the units
    of their own projects, and sometimes renting the unsold ones.

Many companies combine these activities: an agency may manage rentals
for some owners and sell properties for others, and may also buy units
to resell them.

Initial target profile:

-   approximately 20--2,000 managed or listed units
-   approximately 1--50 employees
-   residential, commercial or mixed portfolios
-   rental activity, sales activity, or both
-   multiple owners
-   multiple properties
-   recurring rent collection
-   recurring maintenance operations
-   ongoing sales follow-up with prospects and buyers
-   need for centralized documentation and reporting

### Customer problem profile

Typical organizations may still depend on:

-   spreadsheets
-   email
-   phone calls
-   WhatsApp
-   paper documents
-   shared folders
-   disconnected accounting or invoicing tools

The product aims to replace fragmented operational coordination with one
controlled system.

------------------------------------------------------------------------

## 0.3 Personas

### 1. Property Manager

Responsible for daily portfolio operations.

Needs:

-   portfolio visibility
-   tenant and lease management
-   rent tracking
-   maintenance coordination
-   technician assignment
-   vendor coordination
-   document access
-   owner reporting
-   operational dashboards

### 2. Finance / Administrative User

Responsible for financial and administrative operations.

Needs:

-   invoices
-   recurring rent
-   payment recording
-   payment allocation
-   receipts
-   balances
-   expenses
-   owner statements
-   financial reporting
-   controlled adjustments

### 3. Company Administrator

Responsible for organization-level administration.

Needs:

-   organization setup
-   users
-   roles
-   permissions
-   configuration
-   security settings
-   audit visibility

### 4. Property Owner

Receives transparency over assets managed by the company.

Needs:

-   own properties
-   occupancy
-   leases
-   rent collection
-   expenses
-   maintenance history
-   statements
-   documents
-   when selling through the company: listing status, viewings, offers
    and sale progress

Owners must not gain visibility into unrelated owners or organizational
data.

### 5. Tenant

Occupies a unit under a lease.

Needs:

-   own lease information
-   invoices / rent balance
-   payment history
-   receipts
-   maintenance requests
-   notifications
-   relevant documents
-   communication with the management company

### 6. Internal Technician

An employee of the property-management company.

Needs:

-   assigned work orders
-   property / unit context required to perform the job
-   priority
-   instructions
-   scheduled date
-   status updates
-   notes
-   photographs
-   completion details
-   labor / material information when applicable

A technician should **not** automatically receive access to unrelated
financial or tenant data.

### 7. External Vendor

An independent company or professional performing services for the
property-management company.

Examples:

-   plumber
-   electrician
-   HVAC company
-   cleaning company
-   elevator maintenance company
-   security company
-   pest-control company
-   renovation / construction contractor

A vendor is **not an employee** and belongs to a separate organization.

The platform must therefore distinguish:

``` text
Our organization
├── Admin
├── Property Managers
├── Sales Agents
├── Finance Staff
└── Internal Technicians

External organizations
└── Vendors
```

This distinction is important for authorization, contracts, work orders,
quotes, invoices, and auditability.

### 8. Sales Agent

An employee of the company responsible for selling properties and
units.

Needs:

-   units for sale and their availability
-   sales mandates
-   sales listings
-   prospects and buyers
-   viewing schedule
-   offers
-   reservations and their expiry
-   follow-up of each sale until closing
-   buyer payments received
-   sales pipeline dashboard

A sales agent should **not** automatically receive access to rental
finances, tenant data or unrelated owner information.

### 9. Prospect / Buyer

A person or company interested in buying a property or unit. A prospect
becomes a buyer once an offer or reservation is made.

Needs:

-   information on the properties they are interested in
-   viewing appointments
-   status of their offer and reservation
-   amounts paid and remaining
-   documents of their transaction
-   communication with the sales agent

In the MVP, buyers have no login: the sales agent manages their record.
A buyer portal is planned for V1.

### Seller

Every sale has one selling party. This is a role, not a new persona:

-   a **third-party owner** (individual or company) who gives the
    company a sales mandate --- the company acts as intermediary and
    earns a commission;
-   the **organization itself**, when it owns the unit --- a developer
    selling its own project, or an agency reselling a unit it bought.

------------------------------------------------------------------------

## 0.4 Core Business Problem

The product exists to solve operational fragmentation in property
management.

### Problem 1 --- Spreadsheet dependency

Rental portfolios often become difficult to manage when data is split
across:

-   spreadsheets
-   emails
-   PDFs
-   folders
-   chat conversations

This creates duplication and stale information.

### Problem 2 --- Weak rent visibility

Managers need to answer quickly:

-   Who has paid?
-   Who is late?
-   What is outstanding?
-   Which lease generated the charge?
-   Which payment was allocated to which invoice?
-   How much is due to the owner?

### Problem 3 --- Maintenance fragmentation

Maintenance requests can originate from many channels and then become
disconnected from:

-   the unit
-   tenant
-   property
-   lease
-   work order
-   assigned employee
-   external vendor
-   expense
-   final resolution

### Problem 4 --- Poor vendor coordination

When external companies are involved, property managers need to manage
more than a simple contact record.

They may need:

-   vendor profile
-   service category
-   contacts
-   documents
-   contracts
-   quotes
-   work orders
-   approvals
-   invoices
-   payment status
-   history of services performed

This becomes a meaningful ERP capability.

### Problem 5 --- Weak owner transparency

Owners need structured answers about:

-   occupancy
-   rent collection
-   expenses
-   maintenance
-   income
-   statements
-   property activity
-   sale progress

### Problem 6 --- Document sprawl

Leases, identification documents, receipts, maintenance evidence,
invoices and vendor documents are often scattered.

### Problem 7 --- Weak auditability

Important financial and operational actions should be traceable:

-   who created a lease
-   who changed rent
-   who recorded a payment
-   who approved an expense
-   who assigned work
-   who changed status
-   who accessed or changed sensitive data

### Problem 8 --- Access-control complexity

A realistic system needs different access rules for:

-   organization administrators
-   property managers
-   finance staff
-   owners
-   tenants
-   internal technicians
-   external vendors
-   sales agents
-   prospects and buyers

Multi-tenant isolation makes these permissions even more important.

### Problem 9 --- Disconnected sales follow-up

Sales are often followed in notebooks, spreadsheets, phone calls and
WhatsApp. Companies then struggle to answer:

-   Which units are still available?
-   Who visited which unit, and when?
-   Which offers were made, and which one was accepted?
-   Which unit is reserved, for whom, and until when?
-   How much has the buyer paid, and what remains?
-   Which owner is selling, and under which mandate?

Typical consequences: the same unit promised to two buyers, lost
prospects, unclear deposits, and property information re-entered
separately for rental and for sale.

------------------------------------------------------------------------

## 0.5 Fundamental Product Hypothesis

> A centralized, secure, multi-tenant platform that connects properties,
> leases, rent, sales, buyers, payments, maintenance, internal
> technicians, external vendors, expenses, documents and reporting can
> reduce administrative fragmentation while improving financial
> visibility, sales follow-up, maintenance coordination, owner
> transparency and auditability.

The hypothesis should be validated against:

-   customer interviews
-   workflows
-   competitor analysis
-   implementation complexity
-   usability
-   realistic data and operational scenarios

------------------------------------------------------------------------

## 0.6 What "ERP" Means in This Project

This project is not an attempt to reproduce every module of SAP, Oracle
or NetSuite.

Here, ERP means an integrated operational system connecting several
business processes around the same underlying records.

Core ERP domains:

1.  Property management
2.  Leasing
3.  Tenant management
4.  **Sales**
5.  **Prospects / buyers**
6.  Billing
7.  Payments
8.  Maintenance
9.  **Vendor management**
10. Expenses
11. Owner management
12. Documents
13. Notifications
14. Reporting
15. Audit

The important property is integration.

For example:

``` text
Property
  ↓
Unit
  ↓
Tenant
  ↓
Lease
  ↓
Rent Charge
  ↓
Invoice
  ↓
Payment
```

And for maintenance:

``` text
Unit
  ↓
Maintenance Request
  ↓
Work Order
  ↓
Internal Technician OR External Vendor
  ↓
Completion
  ↓
Expense / Invoice
  ↓
Owner Statement
```

And for sales:

``` text
Unit
  ↓
Sales Listing
  ↓
Prospect / Buyer
  ↓
Offer
  ↓
Reservation
  ↓
Sale
  ↓
Buyer Payments
  ↓
Closing
  ↓
New Owner
```

The value comes from keeping these relationships connected: the unit
that is rented, maintained and sold is the same record.

------------------------------------------------------------------------

## 0.7 Core Business Workflows

### Workflow A --- Property onboarding

``` text
Organization
   ↓
Property
   ↓
Building / Floor
   ↓
Unit
   ↓
Owner Relationship
```

### Workflow B --- Tenant + lease

``` text
Tenant
   ↓
Lease
   ↓
Unit
   ↓
Lease Terms
   ↓
Rent Schedule
   ↓
Active Lease
```

### Workflow C --- Monthly rent

``` text
Active Lease
   ↓
Recurring Rent Charge
   ↓
Invoice
   ↓
Payment
   ↓
Payment Allocation
   ↓
Remaining Balance
```

### Workflow D --- Maintenance request

``` text
Tenant
   ↓
Maintenance Request
   ↓
Property Manager Review
   ↓
Work Order
   ↓
Internal Technician OR External Vendor
   ↓
Work
   ↓
Completion
   ↓
Manager Verification
   ↓
Expense
```

### Workflow E --- Vendor service

``` text
Maintenance Request
   ↓
Work Order
   ↓
External Vendor
   ↓
Quote / Approval (when required)
   ↓
Work
   ↓
Completion
   ↓
Vendor Invoice
   ↓
Expense / Payable
   ↓
Payment
```

Vendor procurement and accounting should be designed as **bounded
extensions** rather than an attempt to build a complete
procurement/accounting suite in the MVP.

### Workflow F --- Owner reporting

``` text
Property
   ↓
Unit
   ↓
Lease
   ↓
Rent
   ↓
Payments
   ↓
Expenses
   ↓
Maintenance
   ↓
Owner Statement
```

For owners selling through the company, owner reporting also covers
listing activity, viewings, offers and sale progress.

### Workflow G --- Property sale

``` text
Seller
(owner under a sales mandate, or the organization itself)
   ↓
Unit for Sale
   ↓
Sales Listing
   ↓
Prospect / Buyer
   ↓
Viewing
   ↓
Offer
   ↓
Reservation (deposit, expiry date)
   ↓
Sale Agreement
   ↓
Buyer Payments
   ↓
Closing
   ↓
Ownership Transfer to the Buyer
```

The same workflow serves both seller models. What changes is who sells
and how the company earns money:

| Seller | Example | Company's role | Company's revenue |
|---|---|---|---|
| Third-party owner | an owner gives an agency a mandate to sell their apartment | intermediary under a sales mandate | commission |
| The organization itself | a developer sells units of its own project; an agency resells a unit it bought | seller | sale price |

How the sale price is paid (through the company, through a notary, or
directly between the parties) must be clarified in Phase 1, together
with the legal steps of a sale in Tunisia.

------------------------------------------------------------------------

## 0.8 Golden Workflows

### Golden workflow 1 --- Rental and maintenance

The end-to-end rental demonstration workflow remains:

``` text
Owner
  ↓
Property
  ↓
Unit
  ↓
Tenant
  ↓
Lease
  ↓
Rent
  ↓
Payment
  ↓
Maintenance Request
  ↓
Work Order
  ↓
Internal Technician OR External Vendor
  ↓
Completion
  ↓
Expense / Vendor Invoice
  ↓
Owner Statement
```

### Golden workflow 2 --- Sale

``` text
Unit
  ↓
Sales Mandate (owner seller) or Own Stock (organization seller)
  ↓
Sales Listing
  ↓
Prospect / Buyer
  ↓
Viewing
  ↓
Offer
  ↓
Reservation
  ↓
Sale
  ↓
Buyer Payment
  ↓
Closing
  ↓
New Owner Recorded
```

These two workflows should become the main acceptance scenarios for the
project. A combined scenario --- selling a unit that is currently
leased --- should also be covered (see §0.18).

------------------------------------------------------------------------

## 0.9 Competitive Reality Check

Competitor research must cover established property-management platforms
and relevant regional/local products.

Initial competitor set:

-   Yardi
-   AppFolio
-   Buildium
-   Propertyware
-   MRI Software
-   Entrata

Regional / smaller-market candidates to research may include:

-   Immoflow
-   Immotech
-   The Landlord
-   Nexa
-   Syndic Digital

Open-source property-management projects on GitHub should also be
reviewed.

Since sales is in scope, the research must also cover sales-side tools:

-   real-estate agency software and CRMs used in the French-speaking
    and Maghreb markets (candidates to verify, e.g. Apimo, Hektor)
-   tools used by property developers to manage unit stock,
    reservations and buyer payments
-   general CRMs that agencies adapt for real-estate sales

### Rental + sales research question

> **Which competing systems handle rental management and sales on the
> same property records, and how well?**

Many products specialize in one activity. Whether combining both is a
genuine differentiator must be verified, not assumed.

### Vendor-management research question

A specific competitive-validation question is now part of Phase 0:

> **Which competing systems support external service vendors as an
> integrated operational entity, and how deeply do they support vendor
> workflows?**

Do not claim that competitors lack this capability until verified.

The research should distinguish between:

-   simple vendor/contact records
-   vendor assignment to work orders
-   vendor portals
-   vendor contracts
-   quotes
-   purchase orders
-   invoices
-   approvals
-   payment tracking
-   service history
-   compliance documents

The goal is to determine whether the project's vendor workflow can
become a genuine differentiator, not to assume it before evidence
exists.

------------------------------------------------------------------------

## 0.10 Product Differentiation

Potential differentiation areas:

### SME focus

Avoid enterprise complexity where it does not create value for smaller
property-management companies.

### Tunisia / Maghreb localization

Potential local-market advantage through:

-   French / Arabic support
-   RTL support
-   local financial conventions
-   local workflow assumptions
-   TND-first design
-   regional terminology

### Unified maintenance ecosystem

Connect:

``` text
Tenant
   ↓
Maintenance Request
   ↓
Manager
   ↓
Internal Technician OR External Vendor
   ↓
Work Order
   ↓
Completion
   ↓
Expense
   ↓
Owner Reporting
```

### Strong external vendor domain

A potentially differentiating capability is treating vendors as
structured business entities instead of storing them as simple contact
names.

This can evolve into:

``` text
Vendor
├── Identity
├── Contacts
├── Service Categories
├── Properties Served
├── Documents
├── Contracts
├── Quotes
├── Work Orders
├── Vendor Invoices
├── Payments
└── Service History
```

**Important:** the strength of this differentiation must be validated
through competitor research before it is presented as a proven market
gap.

### Rental and sales on one property base

One unit record is shared by rental, sales, maintenance and reporting:

-   no re-entering a property to sell it after renting it
-   owners see rental and sale activity in one place
-   a unit can be sold while leased without losing its history
-   one tool for companies that do both activities

As with vendors, this must be validated through competitor research.

### Security and authorization

Make multi-tenant isolation and least-privilege access first-class
engineering concerns.

### Mobile-first operations

Especially useful for:

-   internal technicians
-   tenants
-   eventually vendor interactions

### Cloud-native delivery

Demonstrate:

-   containerization
-   infrastructure as code
-   CI/CD
-   security scanning
-   monitoring
-   observability
-   backup / recovery

### Auditability

Important business and security actions should produce useful audit
records.

------------------------------------------------------------------------

## 0.11 Explicitly Exclude AI / ML

Machine learning and AI are intentionally **out of scope**.

Do not introduce:

-   predictive maintenance
-   rent prediction
-   tenant scoring
-   AI chatbot
-   computer vision
-   generative AI
-   predictive analytics based on ML

Deterministic automation is allowed:

-   scheduled jobs
-   recurring billing
-   reminders
-   notification rules
-   state machines
-   rule-based validation
-   rule-based escalation

The objective is to demonstrate strong software engineering, cloud
architecture and DevSecOps without depending on AI.

------------------------------------------------------------------------

## 0.12 Non-Functional Objectives

### Security

-   authentication
-   authorization
-   organization isolation
-   role-based access control
-   least privilege
-   secure document access
-   audit logs
-   rate limiting
-   input validation
-   secure secrets management
-   dependency scanning
-   container scanning
-   SAST
-   secret scanning
-   secure CI/CD

### Reliability

-   database backups
-   recovery procedures
-   retryable background jobs
-   idempotency for important operations
-   error handling
-   health checks
-   monitoring
-   alerting

### Scalability

-   multi-tenant design
-   horizontal scaling where appropriate
-   background jobs
-   database indexing
-   pagination
-   efficient queries

### Observability

-   structured logs
-   metrics
-   traces where useful
-   application health
-   infrastructure health
-   alerts
-   business KPIs

### Localization

-   French
-   Arabic
-   English
-   RTL support
-   TND
-   future multi-currency support

------------------------------------------------------------------------

## 0.13 Initial MVP

The MVP should demonstrate the complete property-management operational
loop without trying to implement every enterprise feature.

### Identity / organization

-   organization creation
-   users
-   authentication
-   organization membership
-   roles
-   permissions

### Property management

-   properties
-   buildings
-   floors
-   units
-   unit status

### Owners

-   owner profiles
-   ownership relationships
-   owner portfolio

### Tenants

-   tenant profiles
-   relevant documents
-   portal foundation

### Leasing

-   leases
-   lease parties
-   lease lifecycle
-   rent
-   deposit
-   dates
-   status

### Billing

-   recurring rent
-   invoices
-   invoice lines
-   payments
-   payment allocation
-   receipts
-   balances

### Maintenance

-   tenant maintenance requests
-   work orders
-   assignment
-   internal technician handling
-   vendor assignment
-   work-order status
-   notes
-   photographs
-   documents
-   completion
-   manager verification
-   expenses

### Vendor management --- MVP baseline

External vendors should be included as a **real first-class domain**,
but with a bounded MVP.

MVP vendor capabilities:

-   vendor profile
-   company / individual type
-   contact information
-   service categories
-   active / inactive status
-   related documents
-   work-order assignment
-   vendor service history
-   basic notes
-   basic invoice reference / expense linkage where applicable

### Sales --- MVP baseline

Sales is included in the MVP at a basic level, for both seller models.

MVP sales capabilities:

-   marking a unit as for sale (own stock or owner mandate)
-   basic sales mandate: selling owner, unit, asking price, duration,
    agreed commission (recorded for information)
-   sales listings
-   prospect / buyer records
-   viewings
-   offers (several per listing; accept / reject)
-   reservation with deposit and expiry date
-   sale record: seller, buyer, agreed price
-   recording buyer payments
-   closing and transfer of ownership to the buyer
-   sale-related documents

Buyers have no login in the MVP.

### Documents

-   metadata
-   secure object storage
-   signed URLs
-   access control

### Notifications

-   in-app notifications
-   email
-   reminders

### Audit

-   important business events
-   important security events

### Reporting

-   occupancy
-   collection
-   outstanding balances
-   maintenance
-   vendor service history
-   owner statements
-   units for sale and sales pipeline
-   reservations and closed sales

------------------------------------------------------------------------

## 0.14 Post-MVP V1

Possible V1 capabilities:

-   tenant mobile application
-   technician mobile application
-   richer vendor interaction
-   vendor portal
-   SMS
-   payment integrations
-   advanced owner portal
-   recurring expenses
-   late fees
-   lease-expiration reminders
-   advanced reporting
-   document templates
-   PDF generation
-   richer maintenance workflows
-   vendor contracts
-   vendor compliance documents
-   quotes
-   quote approval
-   purchase orders
-   vendor invoice workflow
-   sales commissions: calculation, invoicing and collection
-   buyer payment schedules (installments)
-   buyer portal
-   sale document templates (reservation, sale agreement)
-   owner view of sale progress
-   rule-based matching of buyer criteria to available units

The vendor capability can therefore evolve from:

``` text
MVP:
Vendor → Work Order → Service History → Expense
```

to:

``` text
V1:
Vendor
  ↓
Quote
  ↓
Approval
  ↓
Work Order
  ↓
Completion
  ↓
Vendor Invoice
  ↓
Payment
```

Similarly, sales can evolve from:

``` text
MVP:
Listing → Offer → Reservation → Sale → Buyer Payments → Closing
```

to:

``` text
V1:
Listing → Matching → Offer → Reservation → Sale
        → Payment Schedule → Buyer Payments → Commission → Closing
```

------------------------------------------------------------------------

## 0.15 Post-MVP V2

Potential V2 features:

-   payment-provider integrations
-   bank integrations
-   advanced finance
-   budgets
-   purchase orders
-   advanced procurement
-   vendor contracts
-   vendor performance tracking
-   asset management
-   meters
-   advanced workflows
-   richer owner reporting
-   deeper regional localization
-   more extensive integrations
-   off-plan sales (vente sur plan) with installments tied to
    construction milestones
-   advanced commission rules (several agents, co-agency)
-   publishing listings to external listing portals

------------------------------------------------------------------------

## 0.16 Future Cloud / Platform Capabilities

Only after the core product proves itself:

-   Kubernetes / managed Kubernetes
-   GitOps
-   advanced observability
-   multi-region deployment
-   more advanced platform automation
-   broader integration ecosystem
-   more complete accounting capabilities

Avoid adding infrastructure only for visual complexity.

------------------------------------------------------------------------

## 0.17 Explicit Product Non-Goals

The project is **not**:

-   a real-estate marketplace
-   an Airbnb clone
-   a booking platform
-   a hotel PMS
-   a travel platform
-   a construction ERP (off-plan sales do not include construction
    management)
-   a general-purpose CRM or marketing-automation tool (sales follow-up
    is limited to properties, buyers and transactions)
-   a full SAP-style accounting platform
-   an AI / ML platform
-   a giant microservice ecosystem

External vendors are **not** a non-goal.

The scope decision is:

> **External vendors are part of the product domain. Advanced
> procurement, contract management and full accounts-payable workflows
> are staged progressively.**

Property sales are **not** a non-goal either:

> **Property sales are part of the product domain, for owner sellers
> and organization sellers. Commissions, payment schedules, the buyer
> portal and off-plan sales are staged progressively.**

Sales listings are internal working records, not a public marketplace.

------------------------------------------------------------------------

## 0.18 Initial Business Rules

### Organization ownership

Every business record must belong to an organization, directly or
through a controlled organizational relationship.

### Tenant isolation

Users must only access data authorized for their organization and role.

### Owner access

Owners can access only their authorized properties, units, statements
and documents.

### Tenant access

Tenants can access only their own relevant information.

### Technician access

Internal technicians can access assigned work orders and the minimum
property / unit context needed to perform work.

They should not receive unrestricted access to financial information.

### Vendor access

External vendors, when portal access is introduced, must see only:

-   their organization profile
-   their assigned work
-   information necessary to perform that work
-   relevant communication
-   permitted completion evidence

They must never see unrelated tenants, owners, vendors, properties or
company finances.

### Sales agent access

Sales agents can access units for sale, mandates, listings, prospects,
buyers and sales within their scope. They should not receive
unrestricted access to rental finances or tenant data.

### Buyer access

Buyers, when portal access is introduced, see only their own offers,
reservations, sale, payments and documents.

### Seller

Every sale has exactly one selling party: a third-party owner under a
sales mandate, or the organization itself. The organization can
therefore be the owner of units.

### Unit availability for sale

A unit cannot be reserved for, or sold to, two buyers at the same time.

### Sale of a leased unit

A unit can be sold while it is leased. Ownership must therefore be
effective-dated, so that rent, expenses and owner statements are split
at the transfer date. The legal effect of a sale on an existing lease
must be validated (§0.25).

### Financial integrity

Important financial records should not be destructively edited after
becoming business-relevant.

Prefer:

-   adjustments
-   reversals
-   corrective entries
-   explicit status changes

### Auditability

Important business and security actions should be auditable.

### Work-order performer

A work order may be assigned to:

-   an internal technician
-   an external vendor

The authorization and data model must preserve this distinction.

### Vendor organization boundary

A vendor is not a member of the property-management company's internal
workforce.

------------------------------------------------------------------------

## 0.19 Initial Domain Model Direction

A useful conceptual model is:

``` text
Organization
│
├── User
│   ├── Admin
│   ├── Property Manager
│   ├── Sales Agent
│   ├── Finance Staff
│   └── Internal Technician
│
├── Property
│   └── Unit
│       ├── Owner Relationship
│       ├── Tenant
│       ├── Lease
│       └── Sales Listing
│           ├── Offer
│           ├── Reservation
│           └── Sale
│
├── Sales Mandate
├── Prospect / Buyer
├── Viewing
├── Invoice
├── Payment
├── Expense
├── Maintenance Request
├── Work Order
├── Vendor
├── Document
├── Notification
└── Audit Event
```

The external vendor should eventually be modeled as a proper bounded
context / aggregate rather than merely a free-text field on a work
order.

Possible relationships:

``` text
Vendor 1 ──────── N Work Orders
Vendor 1 ──────── N Documents
Vendor 1 ──────── N Invoices
Vendor 1 ──────── N Contracts      [later]
Vendor 1 ──────── N Quotes         [later]
Vendor 1 ──────── N Properties     [through service history / assignment]
```

------------------------------------------------------------------------

## 0.20 Architecture Consequence

The addition of external vendors and of property sales does **not**
justify prematurely splitting the application into microservices.

Initial recommendation:

> **Modular monolith with explicit domain boundaries.**

Suggested modules:

``` text
Identity / Access
Property
Owner
Tenant
Lease
Sales
Prospects / Buyers
Billing
Payments
Maintenance
Vendor
Expenses
Documents
Notifications
Reporting
Audit
```

The Vendor module should have clear ownership of vendor-specific
business rules and identifiers.

Later extraction into a service should happen only when there is a
concrete operational or scaling reason.

Avoid:

-   microservices for every entity
-   Kafka without a real event-driven requirement
-   Kubernetes solely to appear complex

------------------------------------------------------------------------

## 0.21 Technician + Vendor Operational Model

The platform supports two classes of maintenance performer.

### Internal

``` text
Property Manager
      ↓
Internal Technician
      ↓
Work Order
      ↓
Completion
```

### External

``` text
Property Manager
      ↓
External Vendor
      ↓
Work Order
      ↓
Completion
      ↓
Vendor Invoice / Expense
```

The same maintenance process can therefore serve both internal and
outsourced work while preserving separate security and financial rules.

------------------------------------------------------------------------

## 0.22 Example Vendor Lifecycle

A realistic vendor lifecycle may be:

``` text
Prospect
   ↓
Active
   ↓
Assigned to Work
   ↓
Completed Work
   ↓
Invoice Received
   ↓
Paid
```

The exact lifecycle should be refined in Phase 1.

A separate vendor status model should not be confused with work-order
status.

For example:

**Vendor status**

``` text
ACTIVE
INACTIVE
SUSPENDED
```

**Work-order status**

``` text
OPEN
ASSIGNED
IN_PROGRESS
WAITING
COMPLETED
VERIFIED
CANCELLED
```

**Invoice status**

``` text
DRAFT
RECEIVED
APPROVED
PAID
REJECTED
```

The final state machines belong in Phase 1.

------------------------------------------------------------------------

## 0.23 Security Implications of External Vendors

External vendors make authorization more interesting and therefore
improve the engineering depth of the project.

The system must prevent a vendor from:

-   seeing unrelated work orders
-   seeing unrelated properties
-   accessing owner financial information
-   accessing unrelated tenants
-   reading internal company notes
-   modifying management records outside their assigned scope

The future vendor portal should follow a strict pattern:

``` text
Vendor User
   ↓
Vendor Organization
   ↓
Authorized Work Orders
   ↓
Minimal Required Property / Unit Context
```

This should be enforced server-side, not merely through frontend hiding.

------------------------------------------------------------------------

## 0.24 Vendor Differentiation Hypothesis

The project now has a specific hypothesis worth testing:

> A property-management ERP can create meaningful differentiation by
> making external service providers part of the operational workflow
> rather than treating them as unstructured contacts.

Potential value:

-   fewer disconnected conversations
-   traceable maintenance history
-   clearer responsibility
-   structured approvals
-   better expense traceability
-   better owner reporting
-   easier vendor comparison over time
-   stronger auditability

This remains a **hypothesis until competitor research and customer
validation confirm it**.

------------------------------------------------------------------------

## 0.25 Risks and Constraints

### Scope explosion

Property management can expand into:

-   accounting
-   procurement
-   document management
-   CRM
-   construction
-   facility management
-   legal workflows

Mitigation: explicit MVP / V1 / V2 boundaries.

### Premature architecture

Do not build a distributed system simply because the project is
"cloud-native."

### Cloud complexity

Cloud infrastructure should demonstrate useful engineering concepts
without becoming the product itself.

### Financial correctness

Billing, payments, balances and expenses require careful business rules
and testing.

### Multi-tenant security

Tenant isolation is a critical risk.

### Vendor complexity

Vendor workflows can grow rapidly into:

-   procurement
-   contracts
-   quotations
-   purchase orders
-   invoices
-   payments
-   compliance
-   performance management

Mitigation: include the vendor domain early, but stage advanced
workflows.

### Sales complexity

Sales can grow into:

-   CRM and marketing automation
-   commission rules
-   installment schedules
-   off-plan sales tied to construction
-   notary and legal procedures

Supporting two seller models (owner seller and organization seller)
could also double the work.

Mitigation: one sales workflow where the seller type is a parameter;
basic sales in the MVP; advanced features staged into V1 / V2.

### Localization

French / Arabic / English and RTL requirements can affect:

-   UI
-   validation
-   documents
-   dates
-   numbers
-   currency
-   reporting

### Legal / regulatory assumptions

Do not hard-code legal conclusions without validating the relevant
Tunisian requirements when the project reaches implementation of legal
or tax-sensitive behavior.

This includes the sale process: reservation deposits, sale agreements,
the role of notaries, transfer of ownership, and the effect of a sale
on an existing lease.

------------------------------------------------------------------------

## 0.26 Success Criteria

### Business success

The product can demonstrate:

1.  onboarding a property
2.  creating units
3.  attaching an owner
4.  creating a tenant
5.  creating a lease
6.  generating rent
7.  recording a payment
8.  creating a maintenance request
9.  assigning the job to an internal technician **or** external vendor
10. completing and verifying the work
11. recording the resulting expense / vendor invoice reference
12. showing the effect in owner reporting
13. putting a unit up for sale (owner mandate or own stock)
14. registering a prospect / buyer
15. recording a viewing and an offer
16. reserving the unit
17. recording buyer payments and closing the sale
18. showing the new owner and the sale in reporting

### Engineering success

The project should demonstrate:

-   multi-tenant SaaS
-   secure authentication
-   strong authorization
-   PostgreSQL
-   clean domain boundaries
-   state machines
-   financial integrity
-   REST/API architecture
-   testing
-   Docker
-   Terraform / IaC
-   CI/CD
-   cloud deployment
-   observability
-   backup and recovery
-   security automation

### Portfolio evidence

The final portfolio should contain evidence such as:

-   architecture diagram
-   domain model
-   ERD
-   API documentation
-   threat model
-   security architecture
-   CI/CD pipeline
-   infrastructure code
-   automated tests
-   security scan results
-   observability dashboards
-   production-like deployment
-   disaster-recovery procedure
-   documentation
-   demo video

------------------------------------------------------------------------

## 0.27 Phase 0 Deliverables

Phase 0 must produce:

-   product definition
-   target customer definition
-   personas
-   business problem statement
-   core workflows
-   golden workflow
-   competitor research
-   vendor capability comparison
-   sales-side competitor research
-   differentiation hypotheses
-   explicit AI/ML exclusion
-   MVP / V1 / V2 scope
-   non-goals
-   non-functional objectives
-   business rules
-   risks and constraints
-   success criteria
-   Phase 1 input

------------------------------------------------------------------------

## 0.28 Phase 0 Sequence

Recommended sequence:

``` text
1. Product definition
        ↓
2. Target customer
        ↓
3. Personas
        ↓
4. Business problems
        ↓
5. Core workflows
        ↓
6. Golden workflow
        ↓
7. Competitor research
        ↓
8. Vendor-management research
        ↓
9. Differentiation validation
        ↓
10. MVP scope
        ↓
11. V1 / V2 scope
        ↓
12. Non-goals
        ↓
13. Business rules
        ↓
14. Non-functional requirements
        ↓
15. Risks
        ↓
16. Success criteria
        ↓
17. Phase 0 freeze
        ↓
18. Phase 1 — Cahier des Charges + Domain Modeling
```

------------------------------------------------------------------------

## 0.29 Current Scope Decision Record

### Decision: External vendors are included

**Date:** 2026-10-08

**Decision**

External companies / independent professionals performing maintenance
services are officially part of the product domain.

**Reason**

Property management frequently involves outsourced services, and
integrating these actors creates additional operational value and
meaningful authorization / workflow complexity.

**Consequences**

The system must support the distinction between:

-   internal employees / technicians
-   external vendor organizations

The domain model must therefore account for vendor-related identities,
work assignments and financial traceability.

**Scope treatment**

-   Vendor domain: **included**
-   Vendor profile: **MVP**
-   Vendor assignment to work orders: **MVP**
-   Vendor service history: **MVP**
-   Vendor portal: **V1**
-   Vendor contracts: **V1/V2**
-   Quotes / approvals: **V1**
-   Purchase orders: **V2**
-   Advanced procurement: **V2**
-   Full accounts payable: **V2 / later**

**Important**

This decision supersedes the earlier temporary decision to exclude
external vendors.

### Decision: Property sales are included

**Date:** 2026-10-08

**Decision**

The product covers property sales in addition to rental management.
Sales and prospects / buyers are first-level domains, and basic sales
is part of the MVP. The product is renamed "Real Estate Operations
ERP".

**Reason**

Target companies --- agencies, developers and property managers ---
often both rent and sell. The same properties, owners and documents are
involved, so one shared property base avoids duplicated data and gives
owners a single view.

**Seller models**

Both are supported with one workflow:

-   **owner seller** --- a third-party owner gives the company a sales
    mandate; the company is the intermediary and earns a commission;
-   **organization seller** --- the company owns the unit (a
    developer's own project, or a unit an agency bought) and sells it
    directly.

**Consequences**

-   new internal persona: Sales Agent
-   new external persona: Prospect / Buyer
-   the organization itself can be the owner of units
-   ownership must be effective-dated (a unit can be sold while leased)
-   new modules: Sales, Prospects / Buyers
-   new golden workflow: sale
-   competitor research extended to sales tools

**Scope treatment**

-   Sales domain: **included**
-   Units for sale, listings, prospects / buyers, viewings, offers,
    reservations: **MVP**
-   Basic sales mandate: **MVP**
-   Sale record, buyer payments, closing, ownership transfer: **MVP**
-   Commission calculation and invoicing: **V1**
-   Payment schedules (installments): **V1**
-   Buyer portal: **V1**
-   Owner view of sale progress: **V1**
-   Off-plan sales tied to construction milestones: **V2**
-   Publishing to external listing portals: **V2 / later**
-   Public marketplace: **never** (non-goal)

**Open for Phase 1**

-   how the sale price is paid: through the company, a notary, or
    directly between the parties
-   reservation deposit rules (amount, refund, expiry)
-   legal steps and documents of a sale in Tunisia
-   commission rules

**Important**

This decision replaces the earlier non-goal "a real-estate agency CRM"
(§0.17), which is reworded accordingly.

------------------------------------------------------------------------

## 0.30 Phase 0 Definition of Done

Phase 0 is complete when:

-   the product purpose is clearly defined
-   the target customer is defined
-   all initial personas are documented
-   internal technicians and external vendors are explicitly
    distinguished
-   rental and sales are both covered, including the two seller models
-   core business problems are documented
-   core workflows are documented
-   the golden workflow is accepted
-   competitor research is completed
-   vendor-management competitor research is completed
-   sales-side competitor research is completed
-   differentiation hypotheses are evidence-backed
-   AI / ML is explicitly excluded
-   MVP / V1 / V2 are defined
-   vendor scope is explicitly staged
-   sales scope is explicitly staged
-   non-goals are documented
-   non-functional objectives are documented
-   initial business rules are documented
-   major risks are identified
-   success criteria are defined
-   Phase 1 inputs are ready
-   major Phase 0 scope decisions are frozen

At that point, the project can move to:

> **Phase 1 --- Cahier des Charges + Domain Modeling**
