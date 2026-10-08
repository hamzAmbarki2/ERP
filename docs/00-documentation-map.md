# Documentation Map — Real Estate Operations ERP

**Document:** DOC-MAP — Documentation Map & Plan\
**Version:** 0.1\
**Status:** Draft\
**Date:** 2026-10-08\
**Inputs:** [Phase 0 — Product Definition](phase-0/01-product-definition.md),
[CdC 00 — Cahier des Charges Général](phase-1/specs/00-cahier-des-charges-general.md)

------------------------------------------------------------------------

## 1. Purpose

This document lists every document the project needs, from discovery to
production: what each one must contain, which tier it belongs to, and in
which order to write them. It is the index of the documentation set.

- If a document is not listed here, it does not exist yet.
- If a topic is not owned by a document listed here, nobody is
  responsible for it.

The goal is that **at the end of Phase 1, a reader can understand the
whole product from documents alone**: who uses it, what it does, which
rules it enforces, how every amount is computed, and what "done" means.
Phase 2+ documents then explain how it is built, secured and operated.

IDs below are proposals. Renumber if you prefer, but keep them stable
once other documents reference them.

------------------------------------------------------------------------

## 2. Review of the existing documents — fix before continuing

Reading Phase 0 and CdC 00 together surfaced issues that affect several
downstream documents. The **blocking** ones should be resolved before
writing CdC 01, because roles and permissions depend on them.

| # | Finding | Why it matters | Resolved in | Priority |
|---|---|---|---|---|
| F-01 | **Sales was added without a Phase 0 decision.** CdC 00 makes real-estate sales and prospects/buyers first-level domains (D-005) and renames the product from "Property Management ERP" to "Real Estate Operations ERP". Phase 0 is rental-only: its target customer, personas, golden workflow, MVP/V1/V2, success criteria and competitor set contain no sales, and its non-goals exclude "a real-estate agency CRM" (CdC 00 §20 quietly rewords this to "un CRM généraliste"). | Phase 0 itself sets the rule (§0.29): a scope change gets a decision record. Without one, Phase 0 and Phase 1 describe two different products. | REF-02 (new decision D-008) + P0-01 update | Blocking |
| F-02 | **The sales business model is undefined.** CdC 00 mixes two different businesses: an *agency* selling on mandate for owners ("selon mandat", commissions) and a *developer* (promoteur) selling its own stock with payment schedules, often off-plan (*vente sur plan*), which also brushes against the "not a construction ERP" non-goal. There is also no internal sales actor: §4.1 still lists only Admin, Manager, Finance and Technician. | The two models have different actors, contracts, money flows and documents. CdC 05/06 cannot be written without picking one (or both, staged). | D-008 + P0-01 (new persona) | Blocking |
| F-03 | **Sales has no MVP staging.** Vendors received a precise MVP/V1/V2 split (§0.29); sales did not, and UC-011 to UC-016 sit at the same level as rental. | Scope explosion is Phase 0's first listed risk. | REF-05 | Blocking |
| F-04 | **"Tenant" means two things.** *Multi-tenant* (a customer organization of the SaaS) vs. *Tenant* (locataire). Same collision for "Owner" (property owner vs. organization owner) and "Organization" (client company vs. vendor company in §0.23). | The collision will leak into the data model (`tenant_id` on a lease?), permissions and UI. | REF-01 — e.g. reserve "Tenant" for the locataire and call the SaaS customer "Organization" | Blocking |
| F-05 | **Which external actors log in at MVP?** The §16 matrix gives owners, tenants, vendors and buyers access, but Phase 0 puts the vendor portal and the advanced owner portal in V1 and the tenant portal as a "foundation". | Each external login changes the identity model, the attack surface and the test scope. | CdC 01 + REF-05 | Blocking |
| F-06 | **Can an external actor span several organizations?** A vendor, owner or tenant may deal with two customer organizations on the platform. Is a vendor a record inside each organization (simple, isolated), or a platform-level account linked to several (one login, harder isolation)? | This is the hardest multi-tenancy question in the product and shapes the identity model. | CdC 01, DM-03 | Blocking |
| F-07 | **Missing actor: the platform operator.** Nobody provisions organizations, suspends them, handles support access to customer data, manages plans/subscriptions, or exports and deletes an organization's data on offboarding. | Every multi-tenant SaaS needs this, and support access is a classic cross-tenant risk. | New CdC 12 | High |
| F-08 | **The core money flow of property management is not specified:** management fees (honoraires de gestion), owner remittances (reversements), owner balances, security deposits held on behalf of tenants, income split between co-owners. | Phase 0 asks "how much is due to the owner?" but no document owns the answer, and owner statements depend on it. | CdC 07, CdC 03, DM-06 | High |
| F-09 | **The accounting boundary is undefined.** "Not a full SAP accounting suite" says what it isn't. Is it a sub-ledger (balances computed from transactions), an internal double-entry ledger, or a system that exports to the customer's accountant? | Shapes the whole finance data model; very expensive to change later. | Decision at the start of CdC 07 | High |
| F-10 | **Local legal and tax rules are not inventoried.** Phase 0 says not to hard-code legal conclusions, but the rules to check are not listed: VAT on commercial rents, withholding tax (retenue à la source) on rent paid by companies, timbre fiscal and invoice numbering rules, possible e-invoicing obligations (TTN), lease registration, deposit rules, personal-data law (Loi organique 2004-63, INPDP) including transfers of personal data abroad, e-signature validity, legal retention periods. | Finance depends on the tax rules; the cross-border transfer rules constrain the choice of cloud region. | P0-06, validated with an accountant / lawyer | High |
| F-11 | **Local payment reality is missing.** Cheques (including post-dated and bounced), cash, transfers and bills of exchange (traites) behave differently: a cheque is received, deposited, then cleared or bounced. Also, TND has **3 decimal places** (millimes). | A payment model built only for bank transfers, or money stored with 2 decimals, will be wrong. | CdC 07, CdC 14 | Medium |
| F-12 | **State machines already disagree.** Work order: Phase 0 has `WAITING` and `CANCELLED` but no `CLOSED`; CdC 00 has `CLOSED` but no `WAITING`/`CANCELLED`. Invoice: Phase 0's statuses (`RECEIVED`, `APPROVED`, `REJECTED`) describe a vendor bill (payable); CdC 00's (`ISSUED`, `PARTIALLY_PAID`, `VOID`) describe a customer invoice (receivable). Two different objects share the name "Invoice". | Inconsistent lifecycles become inconsistent code and reports. | REF-01 (e.g. *Invoice* vs. *Vendor Bill*) + DM-04 | Medium |
| F-13 | **Phase 0 is not done by its own definition of done:** competitor research, vendor capability comparison, evidence-backed differentiation and customer validation have no documents, and there is no Phase 0 freeze. | Phase 1 is being built on unvalidated hypotheses. | P0-02 to P0-04, P0-07 — or record them as deferred with the risk accepted | Medium |
| F-14 | **Non-functional requirements are qualitative only:** no availability target, RPO/RTO, response time, data volume or retention duration. | Architecture choices and tests need numbers. | CdC 11 | Medium |
| F-15 | **No data onboarding.** Target customers live in spreadsheets. Without importing properties, owners, tenants, leases and opening balances, a new customer cannot start. | Adoption blocker; also the fastest way to load demo data. | New CdC 13 | Medium |
| F-16 | **Scope edges never decided:** condominium management (*syndic de copropriété*; Phase 0 even lists "Syndic Digital" as a competitor), seasonal/short-term rentals (common locally, distinct from the Airbnb non-goal), recoverable charges and utility re-billing (charges locatives, STEG/SONEDE), and tenant ↔ company messaging vs. notifications only (Phase 0 lists "communication" as a tenant need; WhatsApp is today's channel). | Each one silently changes the size of a domain. | REF-02 + REF-05 | Medium |
| F-17 | **A unit can be sold while leased.** Ownership must be effective-dated (who owned it on which date), so rent, expenses and owner statements split correctly at the transfer date. One person can also be owner, tenant and buyer at once. | Easy to design in from day one, painful to retrofit. | CdC 02, 03, 05; DM-03 | Medium |

------------------------------------------------------------------------

## 3. Documentation principles

- **One owner per topic.** Each topic is defined in exactly one
  document; others reference it. Example: a lifecycle is defined in the
  owning cahier and collected in DM-04, never re-defined elsewhere.
- **Stable IDs.** `D-xxx` decisions, `Q-xxx` open questions, `R-xxx`
  risks, `UC-xxx` use cases, `BR-<DOMAIN>-xxx` business rules,
  `NFR-<CATEGORY>-xxx` non-functional requirements, `AC-xxx` acceptance
  criteria, `ADR-xxxx` architecture decisions. Referencing by ID is what
  makes traceability (REF-06) possible.
- **Standard header** on every document: ID, title, version, status
  (`Draft → In review → Baseline → Superseded`), date, depends on,
  changelog.
- **Decisions are superseded, never silently edited** — the pattern
  Phase 0 already used in §0.29 for vendors.
- **Language rule (decide once).** Proposal: cahiers may be written in
  French; entity names, states, permissions and IDs stay in English
  because they appear in code and APIs; the glossary gives the FR / EN /
  AR equivalents. CdC 00 already works this way — make it explicit.
- **Living vs. frozen.** Reference documents (REF) live through all
  phases. Phase documents are frozen at the phase gate and then change
  only through a decision.
- **Docs as code.** Markdown in this repository, diagrams as code
  (Mermaid / PlantUML), reviewed like code.
- **Keep them short.** Most documents are tables or one-pagers. ADRs
  are one page each; several Phase 2 artifacts (OpenAPI, physical ERD)
  are generated from or maintained with the code. The heavy writing is
  in CdC 01 to CdC 14.

------------------------------------------------------------------------

## 4. The document set

**Tiers**

- **A — Understanding baseline.** Needed to understand the product and
  take structural decisions. Complete before writing production code.
- **B — Build.** Written before or while building the part it covers.
- **C — Run & launch.** Needed before production use or publishing the
  portfolio.

### 4.1 Reference documents — living, all phases

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| REF-01 | Glossary / Lexique | Every business term: definition, FR / EN / AR names, synonyms to avoid, owning domain. Resolves F-04 and F-12. Also the source for UI translations of business terms. | A |
| REF-02 | Decision Log | All product and scope decisions (`D-xxx`): context, decision, consequences, superseded-by. Migrates Phase 0 §0.29 and CdC 00 D-001 to D-007. Technical decisions go to ADRs (ARC-03). | A |
| REF-03 | Open Questions Register | `Q-xxx`: question, why it matters, which document it blocks, owner, due date, answer → decision link. Takes over CdC 00 §23. | A |
| REF-04 | Risk Register | Phase 0 §0.25 risks with likelihood, impact, mitigation, owner, status. Reviewed at each phase gate. | A |
| REF-05 | Scope & Roadmap Matrix | One table: every capability × `MVP / V1 / V2 / Out`. Scope is currently scattered over Phase 0 §0.13–0.15, §0.29 and CdC 00 §20. | A |
| REF-06 | Requirements Traceability Matrix | `BR / NFR → UC → entity → API endpoint → test → release`. Started in Phase 1, filled during build. Proves the golden workflows are fully covered. | B |

### 4.2 Phase 0 — completion

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| P0-01 | Product Definition *(exists)* | Update for F-01/F-02: target customers for sales, new personas (internal sales agent, prospect/buyer, platform operator), golden workflow including a sale, success criteria, reworded non-goals, product name. | A |
| P0-02 | Competitor Analysis | International (Yardi, AppFolio, Buildium, …), regional and open-source products: features, pricing, positioning, localization. Now also sales-side competitors (real-estate CRMs, developer sales tools). | A |
| P0-03 | Vendor Capability Comparison | Evidence-based answer to the §0.9 question, per competitor, across the levels: contact record → work-order assignment → portal → quotes → POs → bills → payments → compliance documents. Confirms or kills the vendor differentiation. | A |
| P0-04 | Customer Discovery & Validation | Interview guide, interview notes (even 5–10 interviews), synthesis: confirmed / refuted hypotheses, real workflows, current tools, pain ranking, willingness to pay. | A |
| P0-05 | Business Model & Pricing Hypothesis | Who pays, for what (per unit, per user, per module), plans and limits, trial. Feeds CdC 12. Can stay light if the project is primarily a portfolio. | B |
| P0-06 | Regulatory & Legal Register (Tunisia) | One row per legal/tax assumption (F-10): rule, source, product impact, validated by / when, status. Nothing tax- or law-sensitive is built on an unvalidated row. | A |
| P0-07 | Phase 0 Freeze Note | One page: what is validated, what is deferred with accepted risk, frozen decisions, Phase 1 inputs. | A |

### 4.3 Phase 1 — Functional specifications (cahiers des charges)

The 00–11 split from CdC 00 §24 is kept, with three additions (12–14).

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| CdC 00 | Général *(exists)* | Update after D-008 (actors, sales scope, product name); link to REF documents; add CdC 12–14 to §24. | A |
| CdC 01 | Organisation & Accès | Organization lifecycle; membership; users in several organizations; invitations; roles, permissions and scopes (portfolio / property / assignment); external identities (owner, tenant, vendor user, buyer) and which exist at MVP (F-05); cross-organization external actors (F-06); MFA policy; delegation; support access rules. Produces DM-05. | A |
| CdC 02 | Biens & Propriétés | Property / Building / Floor / Unit with optional levels; property types and attributes; unit statuses (rental availability vs. sale availability); status history; common areas; one unit identity shared by rental, sale and maintenance (D-007). | A |
| CdC 03 | Propriétaires | Owner types (person, company, *indivision*); ownership shares; effective-dated ownership and transfers (F-17); management mandate (scope, fee terms, duration); owner bank details; owner portal scope. | A |
| CdC 04 | Location & Gestion Locative | Lease types (residential / commercial / seasonal?); parties and guarantors; terms; rent schedule; revisions and increases; deposits; recoverable charges and utilities; renewals; termination; move-in / move-out inspections (*état des lieux*); arrears and reminders. | A |
| CdC 05 | Vente Immobilière | Depends on D-008. Listing; mandate (agency) or stock (developer); multiple offers and counter-offers; reservation (deposit, expiry); pre-contract / contract stages; payment schedule; closing; commission; effect on ownership and on active leases. Depth follows the staging in REF-05. | A |
| CdC 06 | Prospects / Acheteurs | Prospect vs. buyer; sources; qualification; search criteria; rule-based matching (no ML); viewings; interaction history; contact consent; deduplication. | A |
| CdC 07 | Finance & Transactions | Accounting boundary decision (F-09); money and currency model (TND, 3 decimals); charges, invoices, credit notes; payments per instrument (F-11); allocation rules; deposits; owner ledger, management fees and remittances (F-08); vendor bills and expenses; sale payments and commissions; reversals and corrections; numbering; taxes per P0-06; export to the accountant. Owns the owner-statement *calculation*. Produces DM-06. | A |
| CdC 08 | Maintenance & Techniciens | Intake channels; categories; priority; triage; work order; scheduling; technician assignment and capacity; materials and labor; photos; completion and verification; cost approval thresholds; who pays (owner / tenant / company); preventive maintenance (stage?); SLAs. | A |
| CdC 09 | Fournisseurs / Vendors | Profile; company / individual; categories; contacts; documents with expiry (insurance, licences); status; assignment; service history; rule-based rating; quotes and approval (V1); vendor bills; portal (V1); staging per §0.29. | A |
| CdC 10 | Documents / Notifications / Reporting / Audit | Document types, attachment rules, access, versions, retention; generated documents (lease, receipt, invoice, owner statement templates, bilingual). Notification catalog (event → recipients → channel → template → timing) and preferences. Report catalog (definition, filters, permissions for each report). Business audit events. Owner statements: presentation and delivery only (calculation is in CdC 07). | A |
| CdC 11 | Exigences Transversales & Critères d'Acceptation | Quantified NFRs (F-14): availability, RPO / RTO, response times, volumes per organization, retention, supported browsers and devices, accessibility. End-to-end acceptance criteria for the golden workflows. | A |
| CdC 12 *(new)* | Administration Plateforme & Abonnements | Platform operator role; organization provisioning; plans and limit enforcement; subscription and SaaS invoicing (manual at first); suspension; support access with customer consent and audit; offboarding with data export and deletion (F-07). | A |
| CdC 13 *(new)* | Reprise de Données & Onboarding | Import templates (Excel / CSV) for properties, units, owners, tenants, leases, vendors and opening balances; validation and error reports; dry run; idempotent re-import; go-live checklist for a new organization (F-15). | A |
| CdC 14 *(new)* | Localisation & Internationalisation | FR / AR / EN for UI, generated documents and notifications; RTL; language per user vs. per organization; date, number and currency formats; TND millimes; bilingual legal documents; translation workflow; rules for adding currencies later. Separate from CdC 11 because it touches every domain. | A |

### 4.4 Phase 1 — Domain modeling (cross-domain consolidation)

These are built incrementally — each cahier contributes its part — and
consolidated at the end of Phase 1. They are the deliverables listed in
CdC 00 §25.

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| DM-01 | Use Case Catalog | Index of every `UC-xxx` with actor, domain, scope tier and status. Detailed use cases live in each cahier. | A |
| DM-02 | Context Map | Modules from Phase 0 §0.20, updated with Sales, Prospects and Platform Administration; what each owns; upstream / downstream relations; shared references (Unit, Party). Becomes the module structure of the monolith. | A |
| DM-03 | Conceptual Domain Model | Aggregates, entities, value objects, key attributes, cardinalities. Settles the *Party* question: one person can be owner, tenant and buyer at once — one identity or several? | A |
| DM-04 | State Machine Catalog | For every lifecycle: states, transitions, guards, who may trigger, side effects. Organization, Membership, Unit, Ownership, Mandate, Lease, Charge, Invoice, Credit Note, Payment (per instrument), Deposit, Remittance, Maintenance Request, Work Order, Vendor, Vendor Bill, Expense, Listing, Offer, Reservation, Sale, Document. | A |
| DM-05 | Authorization Matrix | Role × action × scope, exhaustively, for every use case, including external actors and the platform operator. Becomes the source of the authorization tests. | A |
| DM-06 | Conceptual Financial Model | Money-flow diagrams (rent, deposits, owner remittance, maintenance recharge, vendor bills, sale payments); definition of every balance (tenant, owner, vendor, deposit); invariants (e.g. sum of allocations ≤ payment amount). | A |
| DM-07 | Business Events Catalog | Domain events (`LeaseActivated`, `PaymentAllocated`, `WorkOrderAssigned`, …): producer, business payload, consumers (audit, notifications, reporting). Connects CdC 10 to every other domain. | A |
| DM-08 | Conceptual ERD | Entities and relations across contexts, ready to become the logical model in Phase 2. | A |
| DM-09 | Acceptance Scenarios & Reference Dataset | The golden workflows (rental, internal and vendor maintenance, owner statement, sale) as end-to-end Given / When / Then scenarios, on one realistic Tunisian dataset (organization, properties, owners, leases, amounts). The same dataset serves tests, demo and screenshots. | A |

### 4.5 UX — end of Phase 1 / Phase 2

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| UX-01 | Information Architecture | The applications and portals (back office, owner, tenant, technician, vendor, buyer?), navigation, screen inventory per persona. | B |
| UX-02 | User Journeys & Wireframes | Low-fidelity wireframes of the golden-workflow screens, reviewed with users from P0-04. | B |
| UX-03 | Design System & UI Guidelines | Components, RTL mirroring rules, accessibility target (e.g. WCAG 2.1 AA), form and table patterns, empty and error states. | B |

### 4.6 Phase 2 — Architecture & technical design

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| ARC-01 | Architecture Vision & Principles | Drivers (from CdC 11), constraints, quality-attribute priorities, modular-monolith rationale, module boundary rules. | A |
| ARC-02 | Architecture Description (C4) | Context, container and component views; deployment view; key runtime scenarios (recurring billing run, payment allocation, document download). | B |
| ARC-03 | ADR Log | One ADR per technical decision. Initial backlog: language and framework; tenancy model (shared schema + organization column + PostgreSQL RLS vs. schema per organization); identity provider (build vs. Keycloak / managed OIDC); API style; job scheduler; object storage; frontend stack; mobile stack; cloud provider and region (constrained by P0-06); repository layout. | A (initial ADRs) |
| ARC-04 | Multi-Tenancy & Isolation Design | How isolation is enforced at every layer (request context, queries, storage paths, caches, jobs, logs, exports) and how it is tested. | B |
| ARC-05 | Logical & Physical Data Model | From DM-08: tables, keys, constraints, indexes, money types, effective dating, audit columns, soft-delete policy, migration strategy. | B |
| ARC-06 | API Guidelines & Specification | Conventions (resources, versioning, pagination, filtering, error format, idempotency keys, concurrency control, localization) and the OpenAPI specification maintained with the code. | B |
| ARC-07 | Background Processing & Integrations | Scheduled jobs (rent generation, reminders, expiries), retries, idempotency, outbox for domain events, email / SMS providers, future payment and bank integrations. | B |
| ARC-08 | Reporting & Read Models | How reports are computed; immutable snapshots of issued owner statements; performance of aggregates. | B |

### 4.7 Phase 2 — Security & privacy

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| SEC-01 | Security Architecture | Authentication (OIDC, MFA, sessions); authorization enforcement implementing DM-05; secrets; encryption in transit and at rest; document access (signed URLs, expiry); rate limiting. | A |
| SEC-02 | Threat Model | STRIDE per container plus product-specific abuse cases: cross-organization access (IDOR), vendor and technician over-reach, leaked document links, payment / allocation tampering, support-access abuse, malicious import files. | A |
| SEC-03 | Data Protection & Privacy | Personal-data inventory and classification; legal basis; retention and deletion per data type; data-subject rights; INPDP obligations; cross-border transfer position. | A |
| SEC-04 | Secure SDLC / DevSecOps Policy | SAST, dependency, container, IaC and secret scanning; SBOM; branch protection; review rules; vulnerability triage deadlines. | B |
| SEC-05 | Audit Logging Specification | Technical side of audit: event schema, integrity / tamper evidence, retention, who can read audit logs, separation from application logs. | B |

### 4.8 Phase 2 — Infrastructure

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| INF-01 | Cloud Infrastructure Architecture | Provider and region (data residency), network, compute, database, storage, Terraform module structure. | B |
| INF-02 | Environments & Configuration | Local / dev / staging / prod; configuration and secrets per environment; seed and demo data; feature flags. | B |
| INF-03 | CI/CD Pipeline Design | Stages, quality gates, security gates, artifact signing, deployment strategy, rollback. | B |
| INF-04 | Observability Design | Log structure (organization ID, no PII), metrics, traces, dashboards, alert rules mapped to SLOs, business KPIs. | B |
| INF-05 | Backup, Restore & DR Plan | RPO / RTO from CdC 11; backup scope (database and documents); restore procedure; DR test schedule. | B |
| INF-06 | Cost Model | Estimated monthly cost per environment and per customer organization; cost guardrails. | C |

### 4.9 Phase 3 — Delivery *(proposed phase)*

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| DEL-01 | Test Strategy | Test levels; financial calculation tests; authorization tests generated from DM-05; tenant-isolation tests; end-to-end tests from DM-09; performance and security tests; test data. | A |
| DEL-02 | Release Plan & Backlog | Epics and stories derived from use cases, mapped to REF-05 tiers; milestones; MVP definition of done. | B |
| DEL-03 | Engineering Handbook | Repository layout, conventions, branching and commits, Definition of Ready / Done, review checklist, automated module-boundary checks. | B |
| DEL-04 | README / CONTRIBUTING | Local setup, running tests, project structure. | B |
| DEL-05 | Changelog & Release Notes | Per release. | C |

### 4.10 Phase 4 — Production readiness & operations *(proposed phase)*

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| OPS-01 | Production Readiness Checklist | Security, tested backups, alerts, runbooks, load test, legal documents. | C |
| OPS-02 | Runbooks | Deploy / rollback; restore; rotate secrets; re-run a failed billing job; onboard / offboard an organization; answer a personal-data access request. | C |
| OPS-03 | Incident Response & Postmortem Template | Severity levels, roles, communication, customer notification including data-breach obligations from SEC-03. | C |
| OPS-04 | Evidence Reports | DR test results, load test results, security scan summaries, SLO reports. | C |

### 4.11 Phase 5 — User-facing, legal & portfolio *(proposed phase)*

| ID | Document | What it must contain | Tier |
|---|---|---|---|
| USR-01 | User Guides per Persona | Admin, manager, finance, technician, owner, tenant, vendor, sales. | C |
| USR-02 | Customer Onboarding Guide | How a new organization goes live (uses CdC 13). | C |
| USR-03 | Public API Documentation | Rendered from the OpenAPI specification, with guides. | C |
| USR-04 | Legal Documents | Terms of service, privacy policy, data processing agreement, SLA — if offered commercially. | C |
| PF-01 | Demo Script & Video | Golden-workflow walkthrough on the DM-09 dataset. | C |
| PF-02 | Portfolio Case Study | Problem, architecture, security, trade-offs, links to the evidence listed in Phase 0 §0.26. | C |

### 4.12 Totals

| Tier | Count | Notes |
|---|---|---|
| A | 41 | Of which 15 are cahiers (the heavy writing); REF and DM documents are mostly tables consolidated from the cahiers. |
| B | 21 | Written alongside the code they describe. |
| C | 12 | Written before launch / publication. |

------------------------------------------------------------------------

## 5. Recommended writing order

CdC 00 §26 names CdC 01 as the next document. A short **Step 0** should
come first: CdC 01 defines roles and permissions, and those depend on
F-01 to F-06.

``` mermaid
flowchart TD
  S0["Step 0 — Foundations<br/>D-008 sales decision · Glossary · Scope matrix · registers"]
  P0["Step 1 — Finish Phase 0<br/>competitors · vendor comparison · interviews · legal register · freeze"]
  S2["Step 2 — CdC 01 Organisation & Accès<br/>+ CdC 12 Plateforme"]
  S3["Step 3 — CdC 02 Biens · CdC 03 Propriétaires"]
  S4["Step 4 — CdC 07 Finance, part 1<br/>accounting boundary · money · payment instruments"]
  S5["Step 5 — CdC 04 Location<br/>+ CdC 07 part 2"]
  S6["Step 6 — CdC 08 Maintenance · CdC 09 Vendors<br/>+ CdC 07 part 3"]
  S7["Step 7 — CdC 05 Vente · CdC 06 Prospects<br/>+ CdC 07 part 4"]
  S8["Step 8 — CdC 10 · CdC 13 · CdC 14 · CdC 11"]
  S9["Step 9 — DM-01 to DM-09 consolidation<br/>Phase 1 freeze"]
  S10["Step 10 — Phase 2<br/>ARC · ADR · SEC · INF · UX"]
  S0 --> S2 --> S3 --> S4 --> S5 --> S6 --> S7 --> S8 --> S9 --> S10
  S0 --> P0
  P0 -. "legal register before finance" .-> S4
  P0 -. "interviews before freezing specs" .-> S9
```

| Step | Documents | Why at this point |
|---|---|---|
| 0 | D-008 in REF-02; REF-01 v0.1; REF-05 v0.1; REF-03 and REF-04 migrated from existing text; P0-01 and CdC 00 updated | Resolves the blocking findings F-01 to F-06. Small documents, a few days of work. |
| 1 | P0-02, P0-03, P0-04, P0-06, then P0-07 | Runs in parallel with steps 2–8. P0-06 must exist before CdC 07; interviews should happen before the rental, sales and maintenance cahiers are frozen. |
| 2 | CdC 01 + CdC 12 → DM-05 v1 | Identity is the foundation of every other domain; the platform operator is one more role in the same model. |
| 3 | CdC 02, CdC 03 → DM-03 v1 | The real-estate reference data every workflow points to, including effective-dated ownership. |
| 4 | CdC 07 part 1 → DM-06 v1 | Lease, maintenance and sale all create financial records. Fixing the accounting boundary, money model and payment instruments first avoids redesigning each domain later. |
| 5 | CdC 04, then CdC 07 part 2 (rent, deposits, owner remittance) | The primary business loop. |
| 6 | CdC 08 + CdC 09 together, then CdC 07 part 3 (expenses, vendor bills) | Work orders and vendors are one workflow with two performer types. |
| 7 | CdC 05 + CdC 06, then CdC 07 part 4 (sale payments, commissions) | Depth according to the D-008 staging. Even if sales is V1, the conceptual part must exist so the Unit and ownership model supports it. |
| 8 | CdC 10, CdC 13, CdC 14, CdC 11 | Transversal: they consume what the domain cahiers produce (events, documents, reports, NFR needs). |
| 9 | DM-01 to DM-09 consolidated, REF-06 started, Phase 1 freeze | Check consistency across cahiers; answer the exit questions in §7. |
| 10 | ARC-01, initial ADRs, SEC-01 to SEC-03, DEL-01, then Tier B | Phase 2. UX can start in parallel from step 9. |

------------------------------------------------------------------------

## 6. Standard structure of a detailed cahier

Every cahier (CdC 01 to CdC 14) uses the same skeleton so the DM
documents can be consolidated mechanically. Sections that don't apply
are kept and marked "N/A".

``` text
1.  Objet et périmètre            in / out, MVP / V1 / V2 (→ REF-05)
2.  Acteurs et permissions        role × action × scope (→ DM-05)
3.  Glossaire du domaine          new terms (→ REF-01)
4.  Concepts et entités           business attributes, cardinalities (→ DM-03)
5.  Workflows                     diagrams + narrative
6.  Cas d'utilisation détaillés   UC-xxx (→ DM-01)
7.  Règles métier                 BR-<DOM>-xxx, numbered and testable
8.  Cycles de vie                 states, transitions, guards, actor (→ DM-04)
9.  Événements métier             emitted / consumed (→ DM-07)
10. Impacts financiers            records created, amounts, balances (→ CdC 07, DM-06)
11. Documents                     attached / generated (→ CdC 10)
12. Notifications                 event → recipient → channel (→ CdC 10)
13. Audit                         which actions are audited (→ CdC 10)
14. Reporting / KPI               reports and indicators (→ CdC 10)
15. Interactions inter-domaines   references to other cahiers
16. Exigences non fonctionnelles  domain-specific (→ CdC 11)
17. Critères d'acceptation        AC-xxx, Given / When / Then
18. Questions ouvertes            Q-xxx (→ REF-03)
19. Décisions                     D-xxx (→ REF-02)
```

**Use case template**

``` text
UC-004 — Créer un locataire et un bail
Acteur principal     : Property Manager
Acteurs secondaires  : Tenant (notifié), Owner (visibilité)
Portée               : MVP
Préconditions        : Unit existe, statut disponible à la location
Déclencheur          : …
Scénario nominal     : 1. … 2. … 3. …
Alternatives         : 3a. …
Exceptions           : E1. …
Postconditions       : Lease en DRAFT, …
Règles               : BR-LEA-001, BR-LEA-007
Permissions          : DM-05 § Lease
Événements émis      : LeaseCreated
Critères             : AC-004-01, AC-004-02
```

**Business rule format** — one rule, one ID, testable:

``` text
BR-LEA-007 — A lease cannot be activated if another ACTIVE lease
covers the same unit for an overlapping period.
```

------------------------------------------------------------------------

## 7. Phase 1 exit test — "global understanding"

Phase 1 is complete when someone who has never seen the project can
answer each question **from the documents alone**:

| Question | Answered by |
|---|---|
| Who are all the actors, and what can each one see and do? | CdC 01, DM-05 |
| Step by step, what happens in each golden workflow, and which records are created? | DM-09, DM-07 |
| How is every amount on an owner statement computed? | CdC 07, DM-06 |
| What are all the states of each record, and who can change them? | DM-04 |
| What is in MVP, what comes later, what is never built? | REF-05 |
| Which legal and tax assumptions are validated, and which are not? | P0-06 |
| What does the system promise in numbers (availability, recovery, performance)? | CdC 11 |
| Does every term have exactly one meaning? | REF-01 |
| Why was each structural choice made? | REF-02 |

If one question cannot be answered, Phase 1 is not done.

------------------------------------------------------------------------

## 8. Repository layout

``` text
docs/
├── 00-documentation-map.md        this document
├── reference/                     REF-01 … REF-06
├── phase-0/                       P0-01 … P0-07
├── phase-1/
│   ├── specs/                     CdC 00 … CdC 14
│   └── model/                     DM-01 … DM-09
├── ux/                            UX-01 … UX-03
├── phase-2/
│   ├── architecture/              ARC-01 … ARC-08
│   ├── adr/                       ADR-0001 …
│   ├── security/                  SEC-01 … SEC-05
│   └── infrastructure/            INF-01 … INF-06
├── delivery/                      DEL-01 … DEL-05
├── operations/                    OPS-01 … OPS-04
└── user/                          USR-01 … USR-04, PF-01, PF-02
```

Folders are created when their first document is written.
