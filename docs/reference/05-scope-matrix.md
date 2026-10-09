# Scope & Roadmap Matrix

**Document:** Scope & Roadmap Matrix\
**Version:** 0.10\
**Status:** Draft — items marked *Proposed* to be confirmed\
**Date:** 2026-10-08\
**Depends on:** [Phase 0](../phase-0/01-product-definition.md) §0.13–0.17
and §0.29, [Cahier des Charges 00](../phase-1/specs/00-cahier-des-charges-general.md)
§8.4 and §20, [Glossary](01-glossary.md)

------------------------------------------------------------------------

## 1. How to read this document

This is the single place that says **when** each capability is built.
Other documents describe *what* a capability does; this one only places
it in time. If a cahier and this matrix disagree, the conflict is
resolved by a decision and both are updated.

**Tiers**

| Tier | Meaning |
|---|---|
| **MVP** | First usable version: the golden workflows work end to end. |
| **V1** | Next release after the MVP. |
| **V2** | Later release, once V1 is used. |
| **Later** | Wanted, not planned yet. The design must not block it. |
| **Out** | Never built (non-goal). |
| **Open** | Not decided yet; listed in §7. |

**Source column**

- `Phase 0 §x` — already decided in Phase 0.
- `D-0xx` — decision in Cahier des Charges 00 §22.
- `Cahier des Charges 00 §x` — stated in the general cahier.
- **Proposed** — a proposal, decided later in the cahier that owns the
  feature (see §9).

------------------------------------------------------------------------

## 2. Who can log in

This resolves Problem 5 of the documentation map.

**Decision D-010 (2026-10-08): the MVP is the ERP used by the
organization's own staff. No external actor logs in before V1.**

| Actor | MVP | V1 | Source |
|---|---|---|---|
| Organization Administrator, Property Manager, Sales Agent, Finance Staff | **Yes** — web application | — | Phase 0 §0.13 |
| Internal Technician | **Yes** — responsive web, limited to assigned work orders | Mobile app | Phase 0 §0.13, §0.14; responsive web *Proposed* |
| Renter | **No** — staff record the requests renters report by phone, message or in person; renters receive emails and documents | Renter portal, then mobile app | D-010 (replaces Phase 0 §0.13 "portal foundation") |
| Owner | **No** — owners receive their statements and documents by email | Owner portal, including sale progress | D-010 |
| Vendor | **No** — the property manager records the vendor's work | Vendor portal | Phase 0 §0.29 |
| Prospect / Buyer | **No** — the sales agent manages the record | Buyer portal | D-009 |
| Platform Operator | **Yes** — minimal back office: create and suspend organizations | Support access, plans | **Proposed** (Problem 7) |

**Consequence for Problem 6** (external actors working with several
organizations): since no external actor logs in during the MVP,
renters, owners, vendors and buyers are simply records inside each
organization. How a person or vendor working with several
organizations logs in is decided in Cahier des Charges 01 before V1. The identity
model must still be designed so these logins can be added in V1
without rework.

------------------------------------------------------------------------

## 3. The MVP in one paragraph

An organization can set up its users and roles; record its properties,
units and owners (including itself for its own stock); lease units to
renters, generate rent, invoice it, record payments by cash, cheque or
transfer and allocate them; handle maintenance requests through work
orders assigned to internal technicians or vendors, through to verified
completion and the resulting expense; sell units from listing to
closing, with offers, reservations and buyer payments; and produce
occupancy, arrears, maintenance, sales and owner reports. Only the
organization's staff log in; renters and owners receive emails and
documents. Everything is audited, isolated per organization, and available in French, Arabic
and English.

------------------------------------------------------------------------

## 4. Capabilities by domain

### 4.1 Organization and access

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Organization creation | MVP | Phase 0 §0.13 | |
| Users, authentication, memberships | MVP | Phase 0 §0.13 | |
| Five role templates (Admin, Property Manager, Sales Agent, Finance, Technician) | MVP | Phase 0 §0.13, D-008 | |
| Roles created and edited by the organization (View / Create / Update / Delete grid) | MVP | D-011 | |
| Departments and hierarchy defined by the organization, limiting visibility | MVP | D-011 | |
| Roles for external users (renter, owner, vendor, buyer) | V1 | D-010 | |
| Inviting users by email | MVP | Proposed | Needed to add users. |
| One user in several organizations | MVP | Proposed | Built into the data model from the start. |
| Multi-factor authentication for internal users | MVP | Proposed | Security objective, Phase 0 §0.12. |
| Single sign-on with the customer's identity provider | Later | Proposed | |

### 4.2 Platform administration

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Operator creates and suspends organizations | MVP | Proposed | Problem 7. |
| Support access to an organization's data, with the customer's consent and audit | V1 | Proposed | |
| Plans, limits and subscription billing | V1 | Proposed | Subscriptions invoiced manually until then. |
| Data export and deletion when an organization leaves | V1 | Proposed | |
| Self-service sign-up | Later | Proposed | |

### 4.3 Properties

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Properties, buildings, floors, units | MVP | Phase 0 §0.13 | |
| Optional levels (a house has no building or floor) | MVP | Cahier des Charges 00 §6 | |
| Property and unit types | MVP | Phase 0 §0.1 | |
| Unit status and status history | MVP | Phase 0 §0.13, Cahier des Charges 00 §14.5 | |
| Common areas | V1 | Proposed | |
| Meters | V2 | Phase 0 §0.15 | |
| Asset / equipment management | V2 | Phase 0 §0.15 | |

### 4.4 Owners

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Owner profiles (person or company) | MVP | Phase 0 §0.13 | |
| Ownership relationships and owner portfolio | MVP | Phase 0 §0.13 | |
| Ownership shares and joint ownership (indivision) | MVP | Proposed | Co-owners and heirs are common. |
| Effective-dated ownership (transfer at a date) | MVP | Phase 0 §0.18 | Required to sell a leased unit. |
| Organization as owner of its own stock | MVP | D-008 | |
| Basic management mandate (dates, fee terms) | MVP | Proposed | Needed for the management fee. |
| Owner portal | V1 | D-010, Phase 0 §0.14 | MVP: statements sent by email (§8). |

### 4.5 Renters and leasing

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Renter profiles and documents | MVP | Phase 0 §0.13 | |
| Renter portal | V1 | D-010 | MVP: staff record requests; documents sent by email. |
| Leases: parties, dates, rent, deposit, status | MVP | Phase 0 §0.13 | |
| Lease lifecycle including renewal and termination | MVP | Phase 0 §0.13, Cahier des Charges 00 §7.2 | |
| Guarantors | MVP | Proposed | As a lease party. |
| Fixed recoverable charges billed with the rent | MVP | Proposed | An extra line on the lease. |
| Recoverable charges reconciled against actual costs | V2 | Proposed | |
| Manual rent change, with history | MVP | Proposed | |
| Automatic rent revision (e.g. yearly increase) | V1 | Proposed | |
| Lease-expiration reminders | V1 | Phase 0 §0.14 | |
| Late fees | V1 | Phase 0 §0.14 | |
| Structured move-in / move-out inspection | V1 | Proposed | MVP: attach the signed document. |
| Renter mobile app | V1 | Phase 0 §0.14 | |
| Seasonal / short-term rentals | Open | Problem 16 | See §7. |
| Rent requests: record people who want to rent, their needs and viewings, match them to free units, turn them into renters | MVP | Decision 2026-10-09 (UML diagrams, Renters and leases 27–31) | No interface for the person (D-010). |

### 4.6 Sales

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Unit for sale (own stock or owner's sales mandate) | MVP | D-009 | |
| Basic sales mandate | MVP | D-009 | Commission recorded for information. |
| Sales listings | MVP | D-009 | Internal records, not a marketplace. |
| Prospects and buyers | MVP | D-009 | |
| Viewings | MVP | D-009 | |
| Offers (several per listing; accept / reject) | MVP | D-009 | |
| Reservation with deposit and expiry | MVP | D-009 | |
| Sale record, buyer payments, closing, ownership transfer | MVP | D-009 | |
| Selling a leased unit | MVP | Phase 0 §0.18 | |
| Commission on mandate sales | MVP | Decision 2026-10-09 (UML diagrams, Sales 22) | Was V1 in D-009. |
| Buyer payment schedules (installments) | MVP | Decision 2026-10-09 (UML diagrams, Sales 20) | Was V1 in D-009. |
| Buyer portal | V1 | D-009 | |
| Sale document templates | V1 | D-009 | |
| Owner view of sale progress | V1 | D-009 | |
| Rule-based matching of buyer criteria to units | MVP | Decision 2026-10-09 (UML diagrams, Sales 16) | Was V1 in Phase 0. |
| Off-plan sales tied to construction milestones | V2 | D-009 | |
| Advanced commission rules (several sales agents) | V2 | Phase 0 §0.15 | |
| Publishing listings to external portals | V2 | D-009 | |

### 4.7 Billing, payments and finance

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Recurring rent generation | MVP | Phase 0 §0.13 | |
| Invoices and invoice lines | MVP | Phase 0 §0.13 | |
| Payments and allocation | MVP | Phase 0 §0.13 | |
| Receipts | MVP | Phase 0 §0.13 | PDF: see §8. |
| Renter, owner and buyer balances | MVP | Phase 0 §0.13 | |
| Credit notes, reversals, adjustments (no deletion) | MVP | Phase 0 §0.18 | |
| Payment instruments: cash, cheque, bank transfer | MVP | Proposed | Problem 11. |
| Cheque follow-up: deposited, cleared, bounced | MVP | Proposed | Problem 11. |
| Bills of exchange (traites) | V1 | Proposed | Mostly for installments. |
| Security deposits held and returned | MVP | Phase 0 §0.13 | |
| Management fee (percentage of collected rent) | MVP | Proposed | Problem 8; needed for a correct owner statement. |
| Owner remittance recorded (manual payout) | MVP | Proposed | Problem 8. |
| Expenses | MVP | Phase 0 §0.13 | |
| Recurring expenses | V1 | Phase 0 §0.14 | |
| Export for the accountant (CSV / Excel) | V1 | Proposed | |
| Online payment integration | V1 / V2 | Phase 0 §0.14, §0.15 | To settle — see §8. |
| Bank integrations | V2 | Phase 0 §0.15 | |
| Budgets | V2 | Phase 0 §0.15 | |
| Taxes: VAT, withholding tax, timbre fiscal, e-invoicing | Open | Problem 10 | Depends on the legal register (Regulatory & Legal Register). |
| Additional currencies | Later | Phase 0 §0.1 | The money model supports it from the MVP. |

### 4.8 Maintenance

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Maintenance requests recorded by staff (reported by renters by phone, message or in person) | MVP | Phase 0 §0.13, D-010 | The channel is recorded on the request. |
| Renters submit maintenance requests themselves | V1 | D-010 | Renter portal. |
| Work orders, assignment, status | MVP | Phase 0 §0.13 | |
| Internal technician handling | MVP | Phase 0 §0.13 | |
| Vendor assignment | MVP | Phase 0 §0.13 | |
| Notes, photos, documents | MVP | Phase 0 §0.13 | |
| Completion and manager verification | MVP | Phase 0 §0.13 | |
| Labor and material cost entry on completion | MVP | Phase 0 §0.3 | Technician persona needs. |
| Expense created from a work order | MVP | Phase 0 §0.13 | |
| Cost approval thresholds | V1 | Proposed | |
| Richer maintenance workflows | V1 | Phase 0 §0.14 | |
| Preventive / recurring maintenance | V1 | Proposed | |
| Technician mobile app | V1 | Phase 0 §0.14 | MVP: responsive web. |
| Service-level targets per priority | V2 | Proposed | |

### 4.9 Vendors

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Vendor profile: company / individual, contacts, service categories, status | MVP | Phase 0 §0.13, §0.29 | |
| Vendor documents | MVP | Phase 0 §0.13 | |
| Work-order assignment and service history | MVP | Phase 0 §0.29 | |
| Notes; vendor bill reference linked to an expense | MVP | Phase 0 §0.13 | |
| Vendor portal | V1 | Phase 0 §0.29 | |
| Quotes and choosing a quote | MVP | Decision 2026-10-09 (UML diagrams, Vendors 15) | Was V1 in Phase 0. |
| Vendor bill workflow (received → approved → paid) | V1 | Phase 0 §0.14 | |
| Documents with validity dates, expiry alerts and status | MVP | Decision 2026-10-09 (UML diagrams, Vendors 7a–7e) | Was V1 in Phase 0. |
| Vendor contract terms | MVP | Decision 2026-10-09 (UML diagrams, Vendors 7f) | Was V1 / V2 in Phase 0. |
| Vendor performance tracking | V2 | Phase 0 §0.15 | |
| Purchase orders | V2 | Phase 0 §0.29 | See §8. |
| Advanced procurement | V2 | Phase 0 §0.29 | |
| Full accounts payable | V2 / Later | Phase 0 §0.29 | |

### 4.10 Documents

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Document metadata and secure storage | MVP | Phase 0 §0.13 | |
| Time-limited download links (signed URLs) | MVP | Phase 0 §0.13 | |
| Access control per document | MVP | Phase 0 §0.13 | |
| PDF receipt and PDF owner statement | MVP | Proposed | See §8. |
| PDF generation for other documents; editable templates | V1 | Phase 0 §0.14 | |
| Generated documents in French and Arabic | V1 | Proposed | With the templates. |
| Electronic signature | Open | Problem 10 | Legal validity to check. |
| Retention and deletion rules | Open | Problem 10 | Legal register (Regulatory & Legal Register). |

### 4.11 Notifications and communication

| Capability | Tier | Source | Notes |
|---|---|---|---|
| In-app notifications | MVP | Phase 0 §0.13 | |
| Email notifications, including to renters and owners (reminders, receipts, statements) | MVP | Phase 0 §0.13, Cahier des Charges 00 §12 | The only channel to external actors in the MVP (D-010). |
| Reminders (rent due, rent late, reservation expiring) | MVP | Phase 0 §0.13, Cahier des Charges 00 §12 | |
| Notification preferences per user | V1 | Proposed | |
| SMS | V1 | Phase 0 §0.14 | |
| WhatsApp | Open | Problem 16 | See §7. |
| Messaging between renters and the organization | Open | Problem 16 | See §7. |

### 4.12 Reporting

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Occupancy, collection, outstanding balances | MVP | Phase 0 §0.13 | |
| Maintenance and vendor service history | MVP | Phase 0 §0.13 | |
| Owner statements | MVP | Phase 0 §0.13 | |
| Units for sale, sales pipeline, reservations, closed sales | MVP | Phase 0 §0.13 | |
| Export of reports to Excel / CSV | V1 | Proposed | |
| Advanced reporting | V1 | Phase 0 §0.14 | |
| Employees' reports and dashboards | V1 | Decision 2026-10-09 | After the administrator's dashboard. |
| Richer owner reporting | V2 | Phase 0 §0.15 | |

### 4.13 Audit

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Business audit events | MVP | Phase 0 §0.13 | |
| Security audit events | MVP | Phase 0 §0.13 | |
| Audit log viewer for administrators | MVP | Cahier des Charges 00 UC-018 | |

### 4.13b Syndic (shared parts of a building)

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Shared parts of a building, owner shares, building fees, building money | MVP | Decision 2026-10-09 | Done through the existing areas (UML diagrams, syndic note). |
| Dedicated syndic interface | V1 | Decision 2026-10-09 | After the MVP. |

### 4.14 Data import and onboarding

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Import of properties, units, owners, renters and leases from Excel / CSV, with an error report | MVP | Proposed | Problem 15. |
| Manual entry of opening balances | MVP | Proposed | |
| Import of opening balances and vendors | V1 | Proposed | |
| Sample data for a new organization | MVP | Proposed | Same dataset as the acceptance scenarios (Acceptance Scenarios & Reference Dataset). |

### 4.15 Languages, currency and devices

| Capability | Tier | Source | Notes |
|---|---|---|---|
| Interface in French, Arabic (right-to-left) and English | MVP | Phase 0 §0.1, §0.12 | |
| TND with 3 decimals | MVP | Phase 0 §0.1 | |
| Responsive web usable on phones | MVP | Proposed | |
| Native mobile apps (renter, technician) | V1 | Phase 0 §0.14 | |
| Additional currencies | Later | Phase 0 §0.1 | |

------------------------------------------------------------------------

## 5. Engineering baseline

| Capability | Tier | Source |
|---|---|---|
| Organization isolation, authentication, resource-scoped authorization | MVP | Phase 0 §0.26 |
| PostgreSQL, REST API, modular monolith with clear module boundaries | MVP | Phase 0 §0.20, §0.26 |
| Automated tests | MVP | Phase 0 §0.26 |
| Docker, Terraform, CI/CD with security scanning | MVP | Phase 0 §0.26 |
| Cloud deployment, observability, backup and restore | MVP | Phase 0 §0.26 |
| Kubernetes, GitOps, multi-region, advanced observability | Later | Phase 0 §0.16 |

------------------------------------------------------------------------

## 6. Out of scope

From Phase 0 §0.11, §0.17 and Cahier des Charges 00 §20: public real-estate marketplace;
Airbnb clone or booking platform; hotel PMS; travel platform;
construction ERP; general-purpose CRM or marketing automation; full
SAP-style accounting suite; banking platform; any AI / ML feature;
microservices created only for complexity.

------------------------------------------------------------------------

## 7. Open — needs a decision

These move to the Open Questions Register (Open Questions Register) when it is created.

| Topic | Why it matters | Decide in |
|---|---|---|
| Seasonal / short-term rentals | Common locally; changes lease durations, billing and availability. Distinct from the "Airbnb clone" non-goal. | Cahier des Charges 04 |
| Messaging with renters; WhatsApp as a channel | Phase 0 lists communication as a renter need, and WhatsApp is today's channel. | Cahier des Charges 10 |
| Taxes and e-invoicing | Affects invoices, receipts, owner statements and vendor bills. | Regulatory & Legal Register, Cahier des Charges 07 |
| Electronic signature; retention periods | Legal validity and how long documents must be kept. | Regulatory & Legal Register, Cahier des Charges 10 |
| How the sale price is paid (through the organization, a notary, or directly) | Decides whether sale money passes through the system. | Cahier des Charges 05, Cahier des Charges 07 |
| One vendor login across several organizations | Needed before the V1 vendor portal. | Cahier des Charges 01 |

------------------------------------------------------------------------

## 8. Inconsistencies found in Phase 0's scope lists

| Item | Problem | Treatment here |
|---|---|---|
| Purchase orders | Listed in both V1 (§0.14) and V2 (§0.15). | **V2**, as the decision record §0.29 says. |
| Vendor contracts | Listed in both V1 and V2; §0.29 says "V1 / V2". | Left as V1 / V2; to settle in Cahier des Charges 09. |
| Payment integrations | "Payment integrations" in V1 and "payment-provider integrations" in V2. | Left as V1 / V2; to settle in Cahier des Charges 07. |
| Receipts and owner statements vs. PDF | Receipts and owner statements are MVP, but PDF generation is V1, so there would be nothing to hand to a renter or send to an owner. | **Proposed:** PDF receipt and PDF owner statement in MVP; other PDFs and templates stay in V1. Without portals (D-010), these PDFs are how renters and owners get their documents. |
| Owner portal | V1 has an "advanced owner portal", but no basic one exists before it. | Resolved by D-010: the owner portal comes in V1. |

------------------------------------------------------------------------

## 9. Proposals to settle in the detailed cahiers

These are feature-level choices. They stay *Proposed* in this matrix
and are decided when the cahier that owns them is written; this matrix
is then updated.

| Proposal | Decided in |
|---|---|
| A minimal platform back office in the MVP: the operator creates and suspends organizations (§2, §4.2) | Cahier des Charges 12 |
| Management fee and owner remittance in the MVP, so owner statements show the net amount due (§4.7) | Cahier des Charges 07, Cahier des Charges 03 |
| Cash, cheque and transfer, with cheque follow-up, in the MVP (§4.7) | Cahier des Charges 07 |
| PDF receipt and owner statement in the MVP (§8) | Cahier des Charges 10 |
| Excel / CSV import of the main records in the MVP (§4.14) | Cahier des Charges 13 |

------------------------------------------------------------------------

## 10. Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-10-08 | First version: who logs in, capabilities by domain with sources, open topics, Phase 0 inconsistencies, proposals to confirm. |
| 0.2 | 2026-10-08 | Open topic added: rental prospects. |
| 0.3 | 2026-10-08 | D-010: only internal staff log in to the MVP; tenant and owner portals moved to V1. |
| 0.5 | 2026-10-08 | D-011: organization-defined roles and departments moved to MVP. |
| 0.6 | 2026-10-09 | Syndic included: basic syndic work in the MVP through existing areas; dedicated syndic interface in V1. |
| 0.7 | 2026-10-09 | Vendor quotes, documents with expiry and contract terms moved to the MVP. |
| 0.8 | 2026-10-09 | Sales commission, payment schedules and buyer matching moved to the MVP. |
| 0.9 | 2026-10-09 | Rent requests in the MVP (open topic closed); employees' reports and dashboards in V1. |
| 0.10 | 2026-10-09 | Tenant (the person or company who rents) renamed **Renter**; "tenant" now means an organization, as in multi-tenancy (glossary N-01). |
| 0.4 | 2026-10-08 | Feature-level proposals deferred to the cahiers that own them (§9). |
