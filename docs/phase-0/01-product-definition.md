# Phase 0 --- Product Definition & Market Validation

**Project:** Cloud-Native Multi-Tenant Property Management ERP\
**Phase:** 0 --- Research, Product Definition, Scope & Validation\
**Status:** Living specification during discovery\
**Last updated:** 2026-10-08

------------------------------------------------------------------------

## 0.1 Product Definition

### Product

A **cloud-native, multi-tenant Property Management ERP** delivered as a
SaaS platform for small and mid-sized property-management companies.

The platform centralizes:

-   properties, buildings, floors and units
-   owners and ownership relationships
-   tenants and tenant records
-   leases and lease lifecycle
-   recurring rent and billing
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

### Primary customer

Small and mid-sized **property-management companies** managing rental
assets on behalf of property owners.

Initial target profile:

-   approximately 20--2,000 managed units
-   approximately 1--50 employees
-   residential, commercial or mixed portfolios
-   multiple owners
-   multiple properties
-   recurring rent collection
-   recurring maintenance operations
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
├── Finance Staff
└── Internal Technicians

External organizations
└── Vendors
```

This distinction is important for authorization, contracts, work orders,
quotes, invoices, and auditability.

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

Multi-tenant isolation makes these permissions even more important.

------------------------------------------------------------------------

## 0.5 Fundamental Product Hypothesis

> A centralized, secure, multi-tenant platform that connects properties,
> leases, rent, payments, maintenance, internal technicians, external
> vendors, expenses, documents and reporting can reduce administrative
> fragmentation while improving financial visibility, maintenance
> coordination, owner transparency and auditability.

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
4.  Billing
5.  Payments
6.  Maintenance
7.  **Vendor management**
8.  Expenses
9.  Owner management
10. Documents
11. Notifications
12. Reporting
13. Audit

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

The value comes from keeping these relationships connected.

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

------------------------------------------------------------------------

## 0.8 Golden Workflow

The end-to-end demonstration workflow remains:

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

This workflow should become one of the main acceptance scenarios for the
project.

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
-   a construction ERP
-   a real-estate agency CRM
-   a full SAP-style accounting platform
-   an AI / ML platform
-   a giant microservice ecosystem

External vendors are **not** a non-goal.

The scope decision is:

> **External vendors are part of the product domain. Advanced
> procurement, contract management and full accounts-payable workflows
> are staged progressively.**

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
│   ├── Finance Staff
│   └── Internal Technician
│
├── Property
│   └── Unit
│       ├── Owner Relationship
│       ├── Tenant
│       └── Lease
│
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

The addition of external vendors does **not** justify prematurely
splitting the application into microservices.

Initial recommendation:

> **Modular monolith with explicit domain boundaries.**

Suggested modules:

``` text
Identity / Access
Property
Owner
Tenant
Lease
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

------------------------------------------------------------------------

## 0.30 Phase 0 Definition of Done

Phase 0 is complete when:

-   the product purpose is clearly defined
-   the target customer is defined
-   all initial personas are documented
-   internal technicians and external vendors are explicitly
    distinguished
-   core business problems are documented
-   core workflows are documented
-   the golden workflow is accepted
-   competitor research is completed
-   vendor-management competitor research is completed
-   differentiation hypotheses are evidence-backed
-   AI / ML is explicitly excluded
-   MVP / V1 / V2 are defined
-   vendor scope is explicitly staged
-   non-goals are documented
-   non-functional objectives are documented
-   initial business rules are documented
-   major risks are identified
-   success criteria are defined
-   Phase 1 inputs are ready
-   major Phase 0 scope decisions are frozen

At that point, the project can move to:

> **Phase 1 --- Cahier des Charges + Domain Modeling**
