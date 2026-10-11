# Phase 0 --- Définition du produit et validation du marché

**Projet :** ERP SaaS multi-tenant cloud-native pour les opérations immobilières\
**Phase :** 0 --- Recherche, définition du produit, périmètre et validation\
**Statut :** Spécification vivante pendant la phase de découverte\
**Dernière mise à jour :** 2026-10-08\
**Traduction française :** 2026-10-09\
**Version anglaise :** [Phase 0 --- Product Definition](01-product-definition.md)

**Journal des modifications**

-   2026-10-08 --- Ajout de la vente immobilière au périmètre du
    produit : nouveaux personas, workflows, éléments MVP / V1 / V2,
    règles métier et décision (§0.29). Produit rebaptisé « Property
    Management ERP » en « Real Estate Operations ERP ».
-   2026-10-08 --- MVP limité aux utilisateurs internes : locataires,
    propriétaires, fournisseurs et acheteurs n'ont aucune interface
    avant la V1 (§0.29).
-   2026-10-08 --- Départements et rôles définis par chaque organisation
    dès le MVP (Cahier des Charges Général, D-011).
-   2026-10-09 --- « Tenant » (la personne ou la société qui loue)
    renommé **Renter** ; « tenant » désigne désormais une organisation,
    comme en multi-tenancy (glossaire N-01).

------------------------------------------------------------------------

## 0.1 Définition du produit

### Produit

Un **ERP SaaS cloud-native et multi-tenant pour les opérations
immobilières**, proposé comme plateforme SaaS aux petites et moyennes
**sociétés immobilières** qui **possèdent** des biens : elles louent
surtout leurs biens et en vendent aussi certains. Cela inclut les
promoteurs immobiliers. Les agences --- intermédiaires qui ne possèdent
aucun bien --- ne sont pas des clients (§0.29).

Il couvre deux activités sur une même base de biens :

1.  la **location** --- louer les biens de la société (l'activité
    principale) ;
2.  la **vente immobilière** --- vendre des biens et des unités. Un
    acheteur (personne ou société) devient **propriétaire** de ce qu'il
    a acheté.

La plateforme centralise :

-   les biens, bâtiments, étages et unités
-   les propriétaires et les relations de propriété
-   les locataires (Renters) et leurs fiches
-   les baux et leur cycle de vie
-   les loyers récurrents et la facturation
-   les mandats de vente et les unités à vendre
-   les annonces de vente
-   les prospects et les acheteurs
-   les visites, offres, réservations et ventes
-   les paiements, imputations, soldes et reçus
-   les demandes d'intervention et les ordres de travail
-   **les techniciens internes et les fournisseurs externes de
    maintenance**
-   les dépenses et les coûts d'exploitation
-   les documents
-   les notifications et les communications
-   le reporting et les relevés propriétaires
-   les pistes d'audit et les événements de sécurité

Le projet est avant tout un **système de gestion opérationnelle**, pas
une place de marché immobilière.

### Forme du produit

-   SaaS B2B
-   Multi-tenant
-   Cloud-native
-   Application web
-   Applications mobiles prévues
-   Backend orienté API
-   Stockage sécurisé des documents
-   Autorisation et isolation entre tenants strictes
-   Livraison orientée DevSecOps
-   Observable et orienté production

### Localisation initiale

Le premier marché visé est la **Tunisie**, avec une architecture capable
de s'étendre plus tard au Maghreb et au marché francophone au sens
large.

Langues attendues au départ :

-   français
-   arabe
-   anglais

Devise initiale :

-   TND

Le modèle financier doit être conçu pour que d'autres devises puissent
être ajoutées plus tard sans refonte du domaine.

### Types de biens

Les types de biens pris en charge au départ peuvent inclure :

-   appartements
-   maisons
-   bureaux
-   commerces
-   entrepôts
-   places de parking
-   petits immeubles à usage mixte

------------------------------------------------------------------------

## 0.2 Client cible

### Clients principaux

Les petites et moyennes **sociétés immobilières** qui **possèdent** des
biens :

-   elles **louent surtout** leurs biens (appartements, maisons,
    bureaux, commerces, places de parking…) ;
-   elles **vendent aussi** certains biens ou unités --- les
    promoteurs immobiliers qui vendent les unités de leurs propres
    projets sont inclus.

Quand la société vend une unité, l'**acheteur devient propriétaire**.
Exemples : M. X achète un appartement à la société Y ; une société de
logiciels achète un bien de cinq bureaux à la société A. M. X et la
société de logiciels sont alors des **propriétaires** dans l'ERP.

**Ne sont pas des clients :** les agences --- intermédiaires qui ne
possèdent aucun bien (§0.29, décision D-012).

Profil cible initial :

-   environ 20 à 2 000 unités gérées ou en vente
-   environ 1 à 50 employés
-   portefeuilles résidentiels, commerciaux ou mixtes
-   activité locative, activité de vente, ou les deux
-   plusieurs propriétaires
-   plusieurs biens
-   encaissement récurrent des loyers
-   opérations de maintenance récurrentes
-   suivi permanent des prospects et des acheteurs
-   besoin de documentation et de reporting centralisés

### Profil du problème client

Les organisations typiques peuvent encore dépendre de :

-   tableurs
-   emails
-   appels téléphoniques
-   WhatsApp
-   documents papier
-   dossiers partagés
-   outils de comptabilité ou de facturation déconnectés

Le produit vise à remplacer cette coordination opérationnelle
fragmentée par un seul système maîtrisé.

------------------------------------------------------------------------

## 0.3 Personas

### 1. Property Manager

Responsable des opérations quotidiennes du portefeuille.

Besoins :

-   visibilité sur le portefeuille
-   gestion des locataires et des baux
-   suivi des loyers
-   coordination de la maintenance
-   affectation des techniciens
-   coordination des fournisseurs
-   accès aux documents
-   reporting vers les propriétaires
-   tableaux de bord opérationnels

### 2. Utilisateur Finance / Administration

Responsable des opérations financières et administratives.

Besoins :

-   factures
-   loyers récurrents
-   enregistrement des paiements
-   imputation des paiements
-   reçus
-   soldes
-   dépenses
-   relevés propriétaires
-   reporting financier
-   ajustements contrôlés

### 3. Administrateur de la société

Responsable de l'administration au niveau de l'organisation.

Besoins :

-   configuration de l'organisation
-   utilisateurs
-   rôles
-   permissions
-   paramétrage
-   paramètres de sécurité
-   visibilité sur l'audit

### 4. Propriétaire

Reçoit de la transparence sur les biens gérés par la société.

Besoins :

-   ses biens
-   l'occupation
-   les baux
-   l'encaissement des loyers
-   les dépenses
-   l'historique de maintenance
-   les relevés
-   les documents
-   quand il vend par l'intermédiaire de la société : l'état de
    l'annonce, les visites, les offres et l'avancement de la vente

Les propriétaires ne doivent avoir aucune visibilité sur les autres
propriétaires ni sur les données de l'organisation qui ne les
concernent pas.

### 5. Locataire (Renter)

Occupe une unité dans le cadre d'un bail.

Besoins :

-   les informations de son propre bail
-   ses factures / son solde de loyer
-   l'historique de ses paiements
-   ses reçus
-   ses demandes d'intervention
-   ses notifications
-   les documents qui le concernent
-   la communication avec la société de gestion

### 6. Technicien interne

Un employé de la société de gestion immobilière.

Besoins :

-   les ordres de travail qui lui sont affectés
-   le contexte du bien / de l'unité nécessaire pour réaliser le
    travail
-   la priorité
-   les instructions
-   la date prévue
-   les mises à jour de statut
-   les notes
-   les photographies
-   les détails de fin d'intervention
-   les informations de main-d'œuvre / matériel le cas échéant

Un technicien ne doit **pas** recevoir automatiquement d'accès aux
données financières ou aux données de locataires sans rapport avec son
travail.

### 7. Fournisseur externe

Une société indépendante ou un professionnel indépendant qui réalise des
prestations pour la société de gestion.

Exemples :

-   plombier
-   électricien
-   société de climatisation / chauffage (HVAC)
-   société de nettoyage
-   société de maintenance d'ascenseurs
-   société de sécurité
-   société de désinsectisation
-   entrepreneur de rénovation / construction

Un fournisseur **n'est pas un employé** et appartient à une organisation
distincte.

La plateforme doit donc distinguer :

``` text
Notre organisation
├── Admin
├── Property Managers
├── Sales Agents
├── Finance Staff
└── Internal Technicians

Organisations externes
└── Vendors
```

Cette distinction est importante pour l'autorisation, les contrats, les
ordres de travail, les devis, les factures et l'auditabilité.

### 8. Commercial (Sales Agent)

Un employé de la société chargé de vendre les biens et les unités.

Besoins :

-   les unités à vendre et leur disponibilité
-   les mandats de vente
-   les annonces de vente
-   les prospects et les acheteurs
-   le planning des visites
-   les offres
-   les réservations et leur expiration
-   le suivi de chaque vente jusqu'à sa conclusion
-   les paiements reçus des acheteurs
-   un tableau de bord du pipeline de ventes

Un commercial ne doit **pas** recevoir automatiquement d'accès aux
finances locatives, aux données des locataires ni aux informations de
propriétaires sans rapport avec ses ventes.

### 9. Prospect / Acheteur

Une personne ou une société intéressée par l'achat d'un bien ou d'une
unité. Un prospect devient acheteur dès qu'une offre ou une réservation
est faite.

Besoins :

-   les informations sur les biens qui l'intéressent
-   les rendez-vous de visite
-   l'état de son offre et de sa réservation
-   les montants payés et restants
-   les documents de sa transaction
-   la communication avec le commercial

Dans le MVP, les acheteurs n'ont pas de connexion : le commercial gère
leur fiche. Un portail acheteur est prévu en V1.

### Vendeur

Chaque vente a une seule partie vendeuse. C'est un rôle, pas un nouveau
persona :

-   un **propriétaire tiers** (personne ou société) qui donne un mandat
    de vente à la société --- la société agit comme intermédiaire et
    perçoit une commission ;
-   l'**organisation elle-même**, quand elle possède l'unité --- un
    promoteur qui vend son propre projet, ou une société immobilière qui
    vend une unité qu'elle possède.

------------------------------------------------------------------------

## 0.4 Problème métier principal

Le produit existe pour résoudre la fragmentation opérationnelle de la
gestion immobilière.

### Problème 1 --- Dépendance aux tableurs

Les portefeuilles locatifs deviennent difficiles à gérer quand les
données sont dispersées entre :

-   tableurs
-   emails
-   PDF
-   dossiers
-   conversations de messagerie

Cela crée des doublons et des informations périmées.

### Problème 2 --- Faible visibilité sur les loyers

Les gestionnaires doivent pouvoir répondre rapidement à :

-   Qui a payé ?
-   Qui est en retard ?
-   Quel est le montant restant dû ?
-   Quel bail a généré la charge ?
-   Quel paiement a été imputé à quelle facture ?
-   Combien est dû au propriétaire ?

### Problème 3 --- Fragmentation de la maintenance

Les demandes de maintenance peuvent venir de nombreux canaux, puis se
retrouver déconnectées de :

-   l'unité
-   le locataire
-   le bien
-   le bail
-   l'ordre de travail
-   l'employé affecté
-   le fournisseur externe
-   la dépense
-   la résolution finale

### Problème 4 --- Mauvaise coordination des fournisseurs

Quand des sociétés externes interviennent, les property managers ont
besoin de gérer bien plus qu'une simple fiche de contact.

Ils peuvent avoir besoin de :

-   profil du fournisseur
-   catégorie de service
-   contacts
-   documents
-   contrats
-   devis
-   ordres de travail
-   approbations
-   factures
-   état des paiements
-   historique des prestations réalisées

Cela devient une véritable capacité d'ERP.

### Problème 5 --- Faible transparence pour les propriétaires

Les propriétaires ont besoin de réponses structurées sur :

-   l'occupation
-   l'encaissement des loyers
-   les dépenses
-   la maintenance
-   les revenus
-   les relevés
-   l'activité des biens
-   l'avancement des ventes

### Problème 6 --- Dispersion des documents

Les baux, pièces d'identité, reçus, preuves de maintenance, factures et
documents de fournisseurs sont souvent dispersés.

### Problème 7 --- Faible auditabilité

Les actions financières et opérationnelles importantes doivent être
traçables :

-   qui a créé un bail
-   qui a modifié un loyer
-   qui a enregistré un paiement
-   qui a approuvé une dépense
-   qui a affecté un travail
-   qui a changé un statut
-   qui a consulté ou modifié des données sensibles

### Problème 8 --- Complexité du contrôle d'accès

Un système réaliste a besoin de règles d'accès différentes pour :

-   les administrateurs d'organisation
-   les property managers
-   le personnel finance
-   les propriétaires
-   les locataires
-   les techniciens internes
-   les fournisseurs externes
-   les commerciaux
-   les prospects et les acheteurs

L'isolation multi-tenant rend ces permissions encore plus importantes.

### Problème 9 --- Suivi des ventes déconnecté

Les ventes sont souvent suivies dans des carnets, des tableurs, des
appels et WhatsApp. Les sociétés peinent alors à répondre à :

-   Quelles unités sont encore disponibles ?
-   Qui a visité quelle unité, et quand ?
-   Quelles offres ont été faites, et laquelle a été acceptée ?
-   Quelle unité est réservée, pour qui, et jusqu'à quand ?
-   Combien l'acheteur a-t-il payé, et que reste-t-il ?
-   Quel propriétaire vend, et sous quel mandat ?

Conséquences typiques : la même unité promise à deux acheteurs, des
prospects perdus, des acomptes peu clairs, et des informations sur le
bien ressaisies séparément pour la location et pour la vente.

------------------------------------------------------------------------

## 0.5 Hypothèse fondamentale du produit

> Une plateforme centralisée, sécurisée et multi-tenant, qui relie les
> biens, les baux, les loyers, les ventes, les acheteurs, les
> paiements, la maintenance, les techniciens internes, les fournisseurs
> externes, les dépenses, les documents et le reporting, peut réduire la
> fragmentation administrative tout en améliorant la visibilité
> financière, le suivi des ventes, la coordination de la maintenance, la
> transparence envers les propriétaires et l'auditabilité.

L'hypothèse doit être validée au regard de :

-   entretiens avec des clients
-   workflows
-   analyse de la concurrence
-   complexité de mise en œuvre
-   utilisabilité
-   données et scénarios opérationnels réalistes

------------------------------------------------------------------------

## 0.6 Ce que signifie « ERP » dans ce projet

Ce projet ne cherche pas à reproduire tous les modules de SAP, Oracle ou
NetSuite.

Ici, ERP désigne un système opérationnel intégré qui relie plusieurs
processus métier autour des mêmes enregistrements.

Domaines ERP principaux :

1.  Gestion des biens
2.  Location
3.  Gestion des locataires
4.  **Ventes**
5.  **Prospects / acheteurs**
6.  Facturation
7.  Paiements
8.  Maintenance
9.  **Gestion des fournisseurs**
10. Dépenses
11. Gestion des propriétaires
12. Documents
13. Notifications
14. Reporting
15. Audit

L'important est l'intégration.

Par exemple :

``` text
Property
  ↓
Unit
  ↓
Renter
  ↓
Lease
  ↓
Rent Charge
  ↓
Invoice
  ↓
Payment
```

Et pour la maintenance :

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

Et pour la vente :

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

La valeur vient du fait de garder ces relations connectées : l'unité
qui est louée, entretenue et vendue est le même enregistrement.

------------------------------------------------------------------------

## 0.7 Workflows métier principaux

### Workflow A --- Intégration d'un bien

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

### Workflow B --- Locataire + bail

``` text
Renter
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

### Workflow C --- Loyer mensuel

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

### Workflow D --- Demande de maintenance

``` text
Renter
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

### Workflow E --- Prestation d'un fournisseur

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

L'approvisionnement et la comptabilité fournisseurs doivent être conçus
comme des **extensions bornées**, et non comme une tentative de
construire une suite complète d'achats / comptabilité dans le MVP.

### Workflow F --- Reporting propriétaire

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

Pour les propriétaires qui vendent par l'intermédiaire de la société, le
reporting propriétaire couvre aussi l'activité des annonces, les
visites, les offres et l'avancement de la vente.

### Workflow G --- Vente d'un bien

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

Le même workflow sert les deux modèles de vendeur. Ce qui change, c'est
qui vend et comment la société gagne de l'argent :

| Vendeur | Exemple | Rôle de la société | Revenu de la société |
|---|---|---|---|
| Propriétaire tiers | un propriétaire (par exemple quelqu'un qui a acheté à la société) donne à la société un mandat pour vendre son appartement | intermédiaire sous mandat | commission |
| L'organisation elle-même | la société vend des unités qu'elle possède (par exemple celles de son propre projet) | vendeur | prix de vente |

La manière dont le prix de vente est payé (par la société, par un
notaire, ou directement entre les parties) doit être clarifiée en
Phase 1, avec les étapes juridiques d'une vente en Tunisie.

------------------------------------------------------------------------

## 0.8 Workflows de référence (golden workflows)

### Workflow de référence 1 --- Location et maintenance

Le workflow de démonstration de bout en bout de la location reste :

``` text
Owner
  ↓
Property
  ↓
Unit
  ↓
Renter
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

### Workflow de référence 2 --- Vente

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

Ces deux workflows doivent devenir les principaux scénarios
d'acceptation du projet. Un scénario combiné --- la vente d'une unité
actuellement louée --- doit aussi être couvert (voir §0.18).

------------------------------------------------------------------------

## 0.9 Vérification de la réalité concurrentielle

La recherche concurrentielle doit couvrir les plateformes établies de
gestion immobilière ainsi que les produits régionaux / locaux
pertinents.

Ensemble initial de concurrents :

-   Yardi
-   AppFolio
-   Buildium
-   Propertyware
-   MRI Software
-   Entrata

Candidats régionaux / de plus petits marchés à étudier :

-   Immoflow
-   Immotech
-   The Landlord
-   Nexa
-   Syndic Digital

Les projets open source de gestion immobilière sur GitHub doivent aussi
être examinés.

Comme la vente est dans le périmètre, la recherche doit aussi couvrir
les outils côté vente :

-   les logiciels et CRM de vente immobilière utilisés sur les marchés
    francophones et maghrébins (candidats à vérifier, par exemple
    Apimo, Hektor)
-   les outils utilisés par les promoteurs immobiliers pour gérer le
    stock d'unités, les réservations et les paiements des acheteurs
-   les CRM généralistes adaptés à la vente immobilière

### Question de recherche location + vente

> **Quels systèmes concurrents gèrent la location et la vente sur les
> mêmes enregistrements de biens, et avec quelle qualité ?**

Beaucoup de produits se spécialisent dans une seule activité. Savoir si
combiner les deux est un véritable différenciateur doit être vérifié, et
non supposé.

### Question de recherche sur la gestion des fournisseurs

Une question de validation concurrentielle spécifique fait désormais
partie de la Phase 0 :

> **Quels systèmes concurrents prennent en charge les fournisseurs de
> services externes comme une entité opérationnelle intégrée, et avec
> quelle profondeur prennent-ils en charge les workflows
> fournisseurs ?**

Ne pas affirmer que les concurrents n'ont pas cette capacité avant de
l'avoir vérifié.

La recherche doit distinguer :

-   les simples fiches fournisseur / contact
-   l'affectation d'un fournisseur à un ordre de travail
-   les portails fournisseurs
-   les contrats fournisseurs
-   les devis
-   les bons de commande
-   les factures
-   les approbations
-   le suivi des paiements
-   l'historique des prestations
-   les documents de conformité

L'objectif est de déterminer si le workflow fournisseurs du projet peut
devenir un véritable différenciateur, et non de le supposer avant que
des preuves n'existent.

------------------------------------------------------------------------

## 0.10 Différenciation du produit

Axes de différenciation possibles :

### Focalisation PME

Éviter la complexité d'entreprise là où elle ne crée pas de valeur pour
les petites sociétés de gestion immobilière.

### Localisation Tunisie / Maghreb

Avantage potentiel sur le marché local grâce à :

-   le support français / arabe
-   le support RTL (écriture de droite à gauche)
-   les conventions financières locales
-   les hypothèses de workflow locales
-   une conception centrée sur le TND
-   la terminologie régionale

### Écosystème de maintenance unifié

Relier :

``` text
Renter
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

### Domaine fournisseurs externes solide

Une capacité potentiellement différenciante consiste à traiter les
fournisseurs comme des entités métier structurées plutôt que de les
stocker comme de simples noms de contacts.

Cela peut évoluer vers :

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

**Important :** la force de cette différenciation doit être validée par
la recherche concurrentielle avant d'être présentée comme une lacune de
marché avérée.

### Location et vente sur une même base de biens

Un seul enregistrement d'unité est partagé par la location, la vente, la
maintenance et le reporting :

-   pas besoin de ressaisir un bien pour le vendre après l'avoir loué
-   les propriétaires voient l'activité de location et de vente au même
    endroit
-   une unité peut être vendue pendant qu'elle est louée sans perdre son
    historique
-   un seul outil pour les sociétés qui font les deux activités

Comme pour les fournisseurs, cela doit être validé par la recherche
concurrentielle.

### Sécurité et autorisation

Faire de l'isolation multi-tenant et de l'accès au moindre privilège des
préoccupations d'ingénierie de premier ordre.

### Opérations mobile-first

Particulièrement utile pour :

-   les techniciens internes
-   les locataires
-   à terme, les interactions avec les fournisseurs

### Livraison cloud-native

Démontrer :

-   la conteneurisation
-   l'infrastructure as code
-   le CI/CD
-   les scans de sécurité
-   la supervision
-   l'observabilité
-   la sauvegarde / restauration

### Auditabilité

Les actions métier et de sécurité importantes doivent produire des
enregistrements d'audit utiles.

------------------------------------------------------------------------

## 0.11 Exclusion explicite de l'IA / du ML

L'apprentissage automatique et l'IA sont volontairement **hors
périmètre**.

Ne pas introduire :

-   la maintenance prédictive
-   la prédiction des loyers
-   le scoring des locataires
-   un chatbot IA
-   la vision par ordinateur
-   l'IA générative
-   l'analytique prédictive fondée sur le ML

L'automatisation déterministe est autorisée :

-   tâches planifiées
-   facturation récurrente
-   rappels
-   règles de notification
-   machines à états
-   validation par règles
-   escalade par règles

L'objectif est de démontrer une solide ingénierie logicielle, une
architecture cloud et du DevSecOps sans dépendre de l'IA.

------------------------------------------------------------------------

## 0.12 Objectifs non fonctionnels

### Sécurité

-   authentification
-   autorisation
-   isolation entre organisations
-   contrôle d'accès par rôles
-   moindre privilège
-   accès sécurisé aux documents
-   journaux d'audit
-   limitation de débit
-   validation des entrées
-   gestion sécurisée des secrets
-   analyse des dépendances
-   analyse des conteneurs
-   SAST
-   détection de secrets
-   CI/CD sécurisé

### Fiabilité

-   sauvegardes de base de données
-   procédures de restauration
-   tâches de fond avec nouvelle tentative
-   idempotence des opérations importantes
-   gestion des erreurs
-   vérifications d'état (health checks)
-   supervision
-   alertes

### Scalabilité

-   conception multi-tenant
-   mise à l'échelle horizontale quand c'est approprié
-   tâches de fond
-   indexation de la base de données
-   pagination
-   requêtes efficaces

### Observabilité

-   logs structurés
-   métriques
-   traces quand elles sont utiles
-   santé de l'application
-   santé de l'infrastructure
-   alertes
-   indicateurs métier

### Localisation

-   français
-   arabe
-   anglais
-   support RTL
-   TND
-   support multi-devises futur

------------------------------------------------------------------------

## 0.13 MVP initial

Le MVP doit démontrer la boucle opérationnelle complète de la gestion
immobilière sans chercher à implémenter toutes les fonctions d'une
entreprise.

### Identité / organisation

-   création d'une organisation
-   utilisateurs
-   authentification
-   appartenance à l'organisation
-   départements et rôles définis par chaque organisation (§0.29)
-   privilèges (voir, créer, modifier, supprimer) par rôle

### Gestion des biens

-   biens
-   bâtiments
-   étages
-   unités
-   statut des unités

### Propriétaires

-   fiches propriétaires
-   relations de propriété
-   portefeuille du propriétaire

### Locataires

-   fiches locataires
-   documents pertinents

Les locataires n'ont pas de connexion dans le MVP (§0.29) : le personnel
enregistre leurs demandes et leur envoie les documents par email. Le
portail locataire est prévu en V1.

### Location

-   baux
-   parties au bail
-   cycle de vie du bail
-   loyer
-   dépôt de garantie
-   dates
-   statut

### Facturation

-   loyers récurrents
-   factures
-   lignes de facture
-   paiements
-   imputation des paiements
-   reçus
-   soldes

### Maintenance

-   demandes de maintenance signalées par les locataires et enregistrées
    par le personnel
-   ordres de travail
-   affectation
-   prise en charge par un technicien interne
-   affectation à un fournisseur
-   statut de l'ordre de travail
-   notes
-   photographies
-   documents
-   fin d'intervention
-   vérification par le manager
-   dépenses

### Gestion des fournisseurs --- socle du MVP

Les fournisseurs externes doivent être inclus comme un **véritable
domaine de premier niveau**, mais avec un MVP borné.

Capacités fournisseurs du MVP :

-   profil du fournisseur
-   type société / individu
-   coordonnées
-   catégories de service
-   statut actif / inactif
-   documents associés
-   affectation à un ordre de travail
-   historique des prestations du fournisseur
-   notes de base
-   référence de facture de base / lien avec une dépense le cas échéant

### Ventes --- socle du MVP

Les ventes sont incluses dans le MVP à un niveau de base, pour les deux
modèles de vendeur.

Capacités de vente du MVP :

-   marquer une unité comme à vendre (stock propre ou mandat d'un
    propriétaire)
-   mandat de vente de base : propriétaire vendeur, unité, prix
    demandé, durée, commission convenue (enregistrée à titre
    d'information)
-   annonces de vente
-   fiches prospect / acheteur
-   visites
-   offres (plusieurs par annonce ; accepter / refuser)
-   réservation avec acompte et date d'expiration
-   fiche de vente : vendeur, acheteur, prix convenu
-   enregistrement des paiements de l'acheteur
-   conclusion et transfert de propriété à l'acheteur
-   documents liés à la vente

Les acheteurs n'ont pas de connexion dans le MVP.

### Documents

-   métadonnées
-   stockage objet sécurisé
-   URL signées
-   contrôle d'accès

### Notifications

-   notifications dans l'application
-   email
-   rappels

### Audit

-   événements métier importants
-   événements de sécurité importants

### Reporting

-   occupation
-   encaissements
-   soldes impayés
-   maintenance
-   historique des prestations des fournisseurs
-   relevés propriétaires
-   unités à vendre et pipeline de ventes
-   réservations et ventes conclues

------------------------------------------------------------------------

## 0.14 Après le MVP : V1

Capacités possibles de la V1 :

-   portail locataire et application mobile locataire
-   application mobile technicien
-   interaction plus riche avec les fournisseurs
-   portail fournisseur
-   SMS
-   intégrations de paiement
-   portail propriétaire
-   dépenses récurrentes
-   pénalités de retard
-   rappels d'expiration de bail
-   reporting avancé
-   modèles de documents
-   génération de PDF
-   workflows de maintenance plus riches
-   contrats fournisseurs
-   documents de conformité des fournisseurs
-   devis
-   approbation des devis
-   bons de commande
-   workflow des factures fournisseurs
-   commissions de vente : calcul, facturation et encaissement
-   échéanciers de paiement des acheteurs (par tranches)
-   portail acheteur
-   modèles de documents de vente (réservation, promesse de vente)
-   vue du propriétaire sur l'avancement de la vente
-   rapprochement par règles entre les critères de l'acheteur et les
    unités disponibles

La capacité fournisseurs peut ainsi évoluer de :

``` text
MVP:
Vendor → Work Order → Service History → Expense
```

à :

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

De même, la vente peut évoluer de :

``` text
MVP:
Listing → Offer → Reservation → Sale → Buyer Payments → Closing
```

à :

``` text
V1:
Listing → Matching → Offer → Reservation → Sale
        → Payment Schedule → Buyer Payments → Commission → Closing
```

------------------------------------------------------------------------

## 0.15 Après le MVP : V2

Fonctionnalités possibles de la V2 :

-   intégrations avec des prestataires de paiement
-   intégrations bancaires
-   finance avancée
-   budgets
-   bons de commande
-   achats avancés
-   contrats fournisseurs
-   suivi de la performance des fournisseurs
-   gestion des actifs
-   compteurs
-   workflows avancés
-   reporting propriétaire plus riche
-   localisation régionale plus poussée
-   intégrations plus nombreuses
-   vente sur plan (VEFA) avec échéances liées à l'avancement des
    travaux
-   règles de commission avancées (plusieurs commerciaux)
-   publication des annonces sur des portails d'annonces externes

------------------------------------------------------------------------

## 0.16 Capacités cloud / plateforme futures

Seulement après que le produit de base a fait ses preuves :

-   Kubernetes / Kubernetes managé
-   GitOps
-   observabilité avancée
-   déploiement multi-région
-   automatisation de plateforme plus avancée
-   écosystème d'intégrations plus large
-   capacités comptables plus complètes

Éviter d'ajouter de l'infrastructure uniquement pour la complexité
visuelle.

------------------------------------------------------------------------

## 0.17 Non-objectifs explicites du produit

Le projet **n'est pas** :

-   une place de marché immobilière
-   un clone d'Airbnb
-   une plateforme de réservation
-   un PMS hôtelier
-   une plateforme de voyage
-   un ERP de construction (la vente sur plan n'inclut pas la gestion de
    chantier)
-   un CRM généraliste ou un outil d'automatisation marketing (le suivi
    commercial se limite aux biens, aux acheteurs et aux transactions)
-   une plateforme comptable complète de type SAP
-   une plateforme d'IA / ML
-   un immense écosystème de microservices

Les fournisseurs externes **ne sont pas** un non-objectif.

La décision de périmètre est :

> **Les fournisseurs externes font partie du domaine du produit.
> L'approvisionnement avancé, la gestion des contrats et les workflows
> complets de comptabilité fournisseurs sont mis en place
> progressivement.**

La vente immobilière n'est **pas** non plus un non-objectif :

> **La vente immobilière fait partie du domaine du produit, pour les
> propriétaires vendeurs et pour les organisations vendeuses. Les
> commissions, les échéanciers de paiement, le portail acheteur et la
> vente sur plan sont mis en place progressivement.**

Les annonces de vente sont des enregistrements de travail internes, pas
une place de marché publique.

------------------------------------------------------------------------

## 0.18 Règles métier initiales

### Appartenance à une organisation

Chaque enregistrement métier doit appartenir à une organisation,
directement ou par une relation organisationnelle contrôlée.

### Isolation entre tenants

Les utilisateurs ne doivent accéder qu'aux données autorisées pour leur
organisation et leur rôle.

### Accès du propriétaire

Les propriétaires ne peuvent accéder qu'à leurs biens, unités, relevés
et documents autorisés.

### Accès du locataire

Les locataires ne peuvent accéder qu'à leurs propres informations
pertinentes.

### Accès du technicien

Les techniciens internes peuvent accéder aux ordres de travail qui leur
sont affectés et au contexte minimal de bien / d'unité nécessaire pour
réaliser le travail.

Ils ne doivent pas recevoir d'accès illimité aux informations
financières.

### Accès du fournisseur

Les fournisseurs externes, quand l'accès par portail est introduit, ne
doivent voir que :

-   le profil de leur organisation
-   leurs travaux affectés
-   les informations nécessaires pour réaliser ces travaux
-   la communication pertinente
-   les preuves d'achèvement autorisées

Ils ne doivent jamais voir les locataires, propriétaires, fournisseurs,
biens ou finances de la société qui ne les concernent pas.

### Accès du commercial

Les commerciaux peuvent accéder aux unités à vendre, mandats, annonces,
prospects, acheteurs et ventes dans leur périmètre. Ils ne doivent pas
recevoir d'accès illimité aux finances locatives ni aux données des
locataires.

### Accès de l'acheteur

Les acheteurs, quand l'accès par portail est introduit, ne voient que
leurs propres offres, réservations, vente, paiements et documents.

### Vendeur

Chaque vente a exactement une partie vendeuse : un propriétaire tiers
sous mandat de vente, ou l'organisation elle-même. L'organisation peut
donc être propriétaire d'unités.

### Disponibilité d'une unité à la vente

Une unité ne peut pas être réservée ou vendue à deux acheteurs en même
temps.

### Vente d'une unité louée

Une unité peut être vendue pendant qu'elle est louée. La propriété doit
donc être datée, afin que les loyers, dépenses et relevés propriétaires
soient répartis à la date du transfert. L'effet juridique d'une vente
sur un bail existant doit être validé (§0.25).

### Intégrité financière

Les enregistrements financiers importants ne doivent pas être modifiés
de façon destructive une fois qu'ils ont une portée métier.

Privilégier :

-   les ajustements
-   les annulations (reversals)
-   les écritures correctives
-   les changements de statut explicites

### Auditabilité

Les actions métier et de sécurité importantes doivent être auditables.

### Exécutant d'un ordre de travail

Un ordre de travail peut être affecté à :

-   un technicien interne
-   un fournisseur externe

Le modèle d'autorisation et le modèle de données doivent préserver cette
distinction.

### Frontière de l'organisation du fournisseur

Un fournisseur ne fait pas partie des effectifs internes de la société
de gestion immobilière.

------------------------------------------------------------------------

## 0.19 Orientation initiale du modèle de domaine

Un modèle conceptuel utile est :

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
│       ├── Renter
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

Le fournisseur externe devrait à terme être modélisé comme un véritable
contexte borné / agrégat, et non simplement comme un champ texte libre
dans un ordre de travail.

Relations possibles :

``` text
Vendor 1 ──────── N Work Orders
Vendor 1 ──────── N Documents
Vendor 1 ──────── N Invoices
Vendor 1 ──────── N Contracts      [later]
Vendor 1 ──────── N Quotes         [later]
Vendor 1 ──────── N Properties     [through service history / assignment]
```

------------------------------------------------------------------------

## 0.20 Conséquence sur l'architecture

L'ajout des fournisseurs externes et de la vente immobilière ne justifie
**pas** de découper prématurément l'application en microservices.

Recommandation initiale :

> **Un monolithe modulaire avec des frontières de domaine explicites.**

Modules suggérés :

``` text
Identity / Access
Property
Owner
Renter
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

Le module Vendor doit être clairement propriétaire des règles métier et
des identifiants propres aux fournisseurs.

L'extraction ultérieure en service ne doit se faire que s'il existe une
raison opérationnelle ou de montée en charge concrète.

À éviter :

-   des microservices pour chaque entité
-   Kafka sans véritable besoin événementiel
-   Kubernetes uniquement pour paraître complexe

------------------------------------------------------------------------

## 0.21 Modèle opérationnel technicien + fournisseur

La plateforme prend en charge deux catégories d'exécutants de
maintenance.

### Interne

``` text
Property Manager
      ↓
Internal Technician
      ↓
Work Order
      ↓
Completion
```

### Externe

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

Le même processus de maintenance peut ainsi servir le travail interne
comme le travail externalisé tout en préservant des règles de sécurité
et des règles financières distinctes.

------------------------------------------------------------------------

## 0.22 Exemple de cycle de vie d'un fournisseur

Un cycle de vie réaliste d'un fournisseur peut être :

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

Le cycle de vie exact devra être affiné en Phase 1.

Un modèle de statut du fournisseur distinct ne doit pas être confondu
avec le statut de l'ordre de travail.

Par exemple :

**Statut du fournisseur**

``` text
ACTIVE
INACTIVE
SUSPENDED
```

**Statut de l'ordre de travail**

``` text
OPEN
ASSIGNED
IN_PROGRESS
WAITING
COMPLETED
VERIFIED
CANCELLED
```

**Statut de la facture**

``` text
DRAFT
RECEIVED
APPROVED
PAID
REJECTED
```

Les machines à états finales relèvent de la Phase 1.

------------------------------------------------------------------------

## 0.23 Implications de sécurité des fournisseurs externes

Les fournisseurs externes rendent l'autorisation plus intéressante et
améliorent donc la profondeur d'ingénierie du projet.

Le système doit empêcher un fournisseur de :

-   voir des ordres de travail qui ne le concernent pas
-   voir des biens qui ne le concernent pas
-   accéder aux informations financières des propriétaires
-   accéder à des locataires qui ne le concernent pas
-   lire les notes internes de la société
-   modifier des enregistrements de gestion en dehors de son périmètre
    affecté

Le futur portail fournisseur doit suivre un schéma strict :

``` text
Vendor User
   ↓
Vendor Organization
   ↓
Authorized Work Orders
   ↓
Minimal Required Property / Unit Context
```

Cela doit être appliqué côté serveur, et pas seulement en masquant des
éléments dans l'interface.

------------------------------------------------------------------------

## 0.24 Hypothèse de différenciation par les fournisseurs

Le projet dispose désormais d'une hypothèse spécifique à tester :

> Un ERP de gestion immobilière peut créer une différenciation
> significative en intégrant les prestataires de services externes dans
> le workflow opérationnel plutôt qu'en les traitant comme des contacts
> non structurés.

Valeur potentielle :

-   moins de conversations déconnectées
-   historique de maintenance traçable
-   responsabilités plus claires
-   approbations structurées
-   meilleure traçabilité des dépenses
-   meilleur reporting vers les propriétaires
-   comparaison plus facile des fournisseurs dans le temps
-   auditabilité renforcée

Cela reste une **hypothèse tant que la recherche concurrentielle et la
validation auprès des clients ne l'ont pas confirmée**.

------------------------------------------------------------------------

## 0.25 Risques et contraintes

### Explosion du périmètre

La gestion immobilière peut s'étendre vers :

-   la comptabilité
-   les achats
-   la gestion documentaire
-   le CRM
-   la construction
-   la gestion des installations (facility management)
-   les workflows juridiques

Atténuation : frontières MVP / V1 / V2 explicites.

### Architecture prématurée

Ne pas construire un système distribué simplement parce que le projet
est « cloud-native ».

### Complexité du cloud

L'infrastructure cloud doit démontrer des concepts d'ingénierie utiles
sans devenir le produit lui-même.

### Exactitude financière

La facturation, les paiements, les soldes et les dépenses exigent des
règles métier et des tests rigoureux.

### Sécurité multi-tenant

L'isolation entre tenants est un risque critique.

### Complexité des fournisseurs

Les workflows fournisseurs peuvent rapidement s'étendre vers :

-   les achats
-   les contrats
-   les devis
-   les bons de commande
-   les factures
-   les paiements
-   la conformité
-   la gestion de la performance

Atténuation : inclure le domaine fournisseurs dès le début, mais
mettre en place progressivement les workflows avancés.

### Complexité de la vente

La vente peut s'étendre vers :

-   le CRM et l'automatisation marketing
-   les règles de commission
-   les échéanciers par tranches
-   la vente sur plan liée à la construction
-   les procédures notariales et juridiques

Prendre en charge deux modèles de vendeur (propriétaire vendeur et
organisation vendeuse) pourrait aussi doubler le travail.

Atténuation : un seul workflow de vente où le type de vendeur est un
paramètre ; vente de base dans le MVP ; fonctions avancées étalées en
V1 / V2.

### Localisation

Les exigences français / arabe / anglais et RTL peuvent affecter :

-   l'interface
-   la validation
-   les documents
-   les dates
-   les nombres
-   la devise
-   le reporting

### Hypothèses juridiques / réglementaires

Ne pas coder en dur de conclusions juridiques sans valider les exigences
tunisiennes pertinentes lorsque le projet arrive à l'implémentation de
comportements juridiques ou fiscaux sensibles.

Cela inclut le processus de vente : acomptes de réservation, promesses
de vente, rôle des notaires, transfert de propriété, et effet d'une
vente sur un bail existant.

------------------------------------------------------------------------

## 0.26 Critères de succès

### Succès métier

Le produit peut démontrer :

1.  l'intégration d'un bien
2.  la création d'unités
3.  le rattachement d'un propriétaire
4.  la création d'un locataire
5.  la création d'un bail
6.  la génération du loyer
7.  l'enregistrement d'un paiement
8.  la création d'une demande de maintenance
9.  l'affectation du travail à un technicien interne **ou** à un
    fournisseur externe
10. la fin et la vérification du travail
11. l'enregistrement de la dépense résultante / de la référence de
    facture fournisseur
12. l'effet sur le reporting propriétaire
13. la mise en vente d'une unité (mandat d'un propriétaire ou stock
    propre)
14. l'enregistrement d'un prospect / acheteur
15. l'enregistrement d'une visite et d'une offre
16. la réservation de l'unité
17. l'enregistrement des paiements de l'acheteur et la conclusion de la
    vente
18. l'affichage du nouveau propriétaire et de la vente dans le
    reporting

### Succès d'ingénierie

Le projet doit démontrer :

-   un SaaS multi-tenant
-   une authentification sécurisée
-   une autorisation solide
-   PostgreSQL
-   des frontières de domaine propres
-   des machines à états
-   l'intégrité financière
-   une architecture REST / API
-   des tests
-   Docker
-   Terraform / IaC
-   le CI/CD
-   un déploiement cloud
-   l'observabilité
-   la sauvegarde et la reprise
-   l'automatisation de la sécurité

### Preuves pour le portfolio

Le portfolio final doit contenir des preuves telles que :

-   un schéma d'architecture
-   un modèle de domaine
-   un ERD
-   la documentation de l'API
-   un modèle de menaces
-   l'architecture de sécurité
-   le pipeline CI/CD
-   le code d'infrastructure
-   des tests automatisés
-   les résultats des scans de sécurité
-   des tableaux de bord d'observabilité
-   un déploiement proche de la production
-   une procédure de reprise après sinistre
-   la documentation
-   une vidéo de démonstration

------------------------------------------------------------------------

## 0.27 Livrables de la Phase 0

La Phase 0 doit produire :

-   la définition du produit
-   la définition du client cible
-   les personas
-   l'énoncé du problème métier
-   les workflows principaux
-   le workflow de référence (golden workflow)
-   la recherche concurrentielle
-   la comparaison des capacités fournisseurs
-   la recherche concurrentielle côté vente
-   les hypothèses de différenciation
-   l'exclusion explicite de l'IA / du ML
-   le périmètre MVP / V1 / V2
-   les non-objectifs
-   les objectifs non fonctionnels
-   les règles métier
-   les risques et contraintes
-   les critères de succès
-   les entrées de la Phase 1

------------------------------------------------------------------------

## 0.28 Séquence de la Phase 0

Séquence recommandée :

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

## 0.29 Registre actuel des décisions de périmètre

### Décision : les fournisseurs externes sont inclus

**Date :** 2026-10-08

**Décision**

Les sociétés externes / professionnels indépendants qui réalisent des
services de maintenance font officiellement partie du domaine du
produit.

**Raison**

La gestion immobilière implique fréquemment des services externalisés,
et l'intégration de ces acteurs crée une valeur opérationnelle
supplémentaire ainsi qu'une complexité d'autorisation / de workflow
significative.

**Conséquences**

Le système doit prendre en charge la distinction entre :

-   les employés / techniciens internes
-   les organisations fournisseurs externes

Le modèle de domaine doit donc tenir compte des identités liées aux
fournisseurs, des affectations de travail et de la traçabilité
financière.

**Traitement du périmètre**

-   Domaine fournisseurs : **inclus**
-   Profil du fournisseur : **MVP**
-   Affectation du fournisseur aux ordres de travail : **MVP**
-   Historique des prestations du fournisseur : **MVP**
-   Portail fournisseur : **V1**
-   Contrats fournisseurs : **V1/V2**
-   Devis / approbations : **V1**
-   Bons de commande : **V2**
-   Achats avancés : **V2**
-   Comptabilité fournisseurs complète : **V2 / plus tard**

**Important**

Cette décision remplace la décision temporaire précédente d'exclure les
fournisseurs externes.

### Décision : la vente immobilière est incluse

**Date :** 2026-10-08

**Décision**

Le produit couvre la vente immobilière en plus de la gestion locative.
Les ventes et les prospects / acheteurs sont des domaines de premier
niveau, et la vente de base fait partie du MVP. Le produit est rebaptisé
« Real Estate Operations ERP ».

**Raison**

Les sociétés cibles --- sociétés immobilières et promoteurs --- louent
et vendent souvent à la fois. Les mêmes biens, propriétaires et
documents sont concernés ; une base de biens commune évite la
duplication des données et donne aux propriétaires une vue unique.

**Modèles de vendeur**

Les deux sont pris en charge avec un seul workflow :

-   **propriétaire vendeur** --- un propriétaire tiers donne à la
    société un mandat de vente ; la société est l'intermédiaire et
    perçoit une commission ;
-   **organisation vendeuse** --- la société possède l'unité (par
    exemple le projet propre d'un promoteur) et la vend directement.

**Conséquences**

-   nouveau persona interne : Commercial (Sales Agent)
-   nouveau persona externe : Prospect / Acheteur
-   l'organisation elle-même peut être propriétaire d'unités
-   la propriété doit être datée (une unité peut être vendue pendant
    qu'elle est louée)
-   nouveaux modules : Sales, Prospects / Buyers
-   nouveau workflow de référence : la vente
-   recherche concurrentielle étendue aux outils de vente

**Traitement du périmètre**

-   Domaine des ventes : **inclus**
-   Unités à vendre, annonces, prospects / acheteurs, visites, offres,
    réservations : **MVP**
-   Mandat de vente de base : **MVP**
-   Fiche de vente, paiements des acheteurs, conclusion, transfert de
    propriété : **MVP**
-   Calcul et facturation des commissions : **V1**
-   Échéanciers de paiement (par tranches) : **V1**
-   Portail acheteur : **V1**
-   Vue du propriétaire sur l'avancement de la vente : **V1**
-   Vente sur plan liée à l'avancement des travaux : **V2**
-   Publication sur des portails d'annonces externes : **V2 / plus
    tard**
-   Place de marché publique : **jamais** (non-objectif)

**Ouvert pour la Phase 1**

-   la manière dont le prix de vente est payé : par la société, par un
    notaire, ou directement entre les parties
-   les règles de l'acompte de réservation (montant, remboursement,
    expiration)
-   les étapes et documents juridiques d'une vente en Tunisie
-   les règles de commission

**Important**

Cette décision remplace l'ancien non-objectif « un CRM d'agence
immobilière » (§0.17), qui est reformulé en conséquence.

### Décision : le client est une société immobilière qui possède des biens

**Date :** 2026-10-09

**Décision**

Le client de l'ERP (l'Organisation) est une **société immobilière**
(société immobilière) qui **possède** des biens. Elle loue surtout ses
biens et en vend aussi certains. Les **agences** --- intermédiaires qui
ne possèdent aucun bien --- ne sont **pas** des clients.

Quand la société vend un bien ou une unité, l'**acheteur** (personne ou
société) en devient **propriétaire** et est enregistré dans la zone
Propriétaires.

**Conséquences**

-   le client cible (§0.2) et la définition du produit (§0.1) sont
    reformulés ;
-   les exemples utilisent des noms tels que « Médina Immobilier »,
    jamais « Agence » ;
-   le mot « agence » n'est employé pour le client dans aucun document.

### Décision : seuls les utilisateurs internes se connectent au MVP

**Date :** 2026-10-08

**Décision**

Le MVP est l'ERP utilisé par le personnel de l'organisation :
administrateurs, property managers, commerciaux, personnel finance et
techniciens internes. Les locataires, les prospects à la location, les
propriétaires, les fournisseurs et les acheteurs n'ont aucune interface
dans le MVP. Ils existent comme fiches gérées par le personnel et
reçoivent des emails et des documents.

**Raison**

Concentrer la première version sur l'ERP de base pour les sociétés qui
louent et vendent des biens.

**Conséquences**

-   le personnel enregistre les demandes de maintenance que les
    locataires signalent par téléphone, par message ou en personne
-   les reçus et les relevés propriétaires parviennent aux locataires et
    aux propriétaires sous forme de documents envoyés par email
-   l'autorisation dans le MVP couvre les rôles internes et l'isolation
    entre organisations
-   le modèle d'identité doit tout de même permettre d'ajouter des
    utilisateurs externes en V1 sans refonte

**Traitement du périmètre**

-   Portail locataire : **V1**
-   Portail propriétaire : **V1**
-   Portail fournisseur : **V1** (inchangé)
-   Portail acheteur : **V1** (inchangé)

------------------------------------------------------------------------

## 0.30 Définition de « terminé » de la Phase 0

La Phase 0 est terminée quand :

-   l'objectif du produit est clairement défini
-   le client cible est défini
-   tous les personas initiaux sont documentés
-   les techniciens internes et les fournisseurs externes sont
    explicitement distingués
-   la location et la vente sont toutes deux couvertes, y compris les
    deux modèles de vendeur
-   les problèmes métier principaux sont documentés
-   les workflows principaux sont documentés
-   le workflow de référence est accepté
-   la recherche concurrentielle est terminée
-   la recherche concurrentielle sur la gestion des fournisseurs est
    terminée
-   la recherche concurrentielle côté vente est terminée
-   les hypothèses de différenciation sont étayées par des preuves
-   l'IA / le ML est explicitement exclu
-   le MVP / la V1 / la V2 sont définis
-   le périmètre fournisseurs est explicitement étalé dans le temps
-   le périmètre des ventes est explicitement étalé dans le temps
-   les non-objectifs sont documentés
-   les objectifs non fonctionnels sont documentés
-   les règles métier initiales sont documentées
-   les risques majeurs sont identifiés
-   les critères de succès sont définis
-   les entrées de la Phase 1 sont prêtes
-   les principales décisions de périmètre de la Phase 0 sont figées

À ce moment-là, le projet peut passer à :

> **Phase 1 --- Cahier des Charges + Modélisation du domaine**
