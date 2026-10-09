# Cahier des Charges Général --- Real Estate Operations ERP

**Projet:** Cloud-Native Multi-Tenant Real Estate Operations ERP\
**Document:** 00 --- Cahier des Charges Général\
**Phase:** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version:** 0.7\
**Status:** Draft / Baseline\
**Date:** 2026-10-08

**Historique**

-   0.7 (2026-10-09) --- Rien n'est partagé entre organisations : un compte par organisation (Cahier des Charges 01, v0.5).
-   0.6 (2026-10-09) --- « Tenant » (le locataire) renommé « Renter » ; « tenant » désigne l'organisation, comme en multi-tenancy (glossaire N-01).
-   0.5 (2026-10-09) --- Le client est une société immobilière qui
    possède des biens ; les agences sont exclues (D-012).
-   0.4 (2026-10-08) --- Départements et rôles définis par chaque
    organisation (D-011).
-   0.3 (2026-10-08) --- MVP réservé aux utilisateurs internes (D-010).
-   0.2 (2026-10-08) --- alignement avec la Phase 0 mise à jour :
    acteur interne Commercial, deux modèles de vendeur, vente de base
    dans le MVP (D-008, D-009), matrice d'acteurs complétée.
-   0.1 (2026-10-08) --- version initiale.

------------------------------------------------------------------------

## 1. Objet du document

Ce document définit le cadre fonctionnel global du produit. Il sert de
référence commune avant la rédaction des cahiers des charges spécialisés
et du modèle de domaine.

Il précise :

-   la vision et les objectifs du produit ;
-   le périmètre fonctionnel ;
-   les acteurs ;
-   les grands workflows ;
-   les principes métier généraux ;
-   les limites du périmètre ;
-   les décisions structurantes héritées de la Phase 0 ;
-   les sujets devant être détaillés dans les cahiers suivants.

**Important :** ce document décrit ce que le système doit faire au
niveau métier. Les choix de PostgreSQL, REST, microservices, Kubernetes,
Terraform, CI/CD, etc. seront traités principalement en Phase 2.

------------------------------------------------------------------------

## 2. Vision du produit

Le produit est un **ERP SaaS B2B cloud-native et multi-tenant** destiné
aux **sociétés immobilières** qui **possèdent** des biens : elles en
louent la plupart et en vendent certains (promoteurs immobiliers
compris). Les agences, intermédiaires qui ne possèdent aucun bien, ne
sont pas des clients (D-012).

Il couvre principalement deux activités :

1.  **gestion locative** ;
2.  **vente de biens immobiliers**.

Ces activités utilisent une même base opérationnelle : biens, unités,
propriétaires, clients, documents, transactions, maintenance,
fournisseurs, paiements et reporting.

Le produit doit devenir la source de vérité opérationnelle de
l'entreprise au lieu de disperser les informations entre tableurs,
emails, messageries, documents et outils isolés.

------------------------------------------------------------------------

## 3. Périmètre général

### 3.1 Domaines

``` text
01  Organisation & Accès
02  Biens & Propriétés
03  Propriétaires
04  Location & Gestion Locative
05  Vente Immobilière
06  Prospects / Acheteurs
07  Finance & Transactions
08  Maintenance & Techniciens
09  Fournisseurs Externes
10  Documents
11  Notifications
12  Reporting
13  Audit
```

### 3.2 Forme du produit

-   SaaS B2B
-   multi-tenant
-   web application
-   API-first
-   mobile applications prévues
-   sécurité et audit intégrés
-   cloud-native
-   DevSecOps

### 3.3 Localisation initiale

Marché initial : Tunisie.

Langues cibles : français, arabe, anglais.\
Devise initiale : TND.\
L'architecture fonctionnelle doit rester compatible avec l'extension à
plusieurs devises et marchés.

------------------------------------------------------------------------

## 4. Acteurs

### 4.1 Acteurs internes à l'organisation cliente

-   Administrateur d'organisation
-   Property Manager
-   Commercial (Sales Agent)
-   Finance / Administration
-   Technicien interne

### 4.2 Acteurs externes

-   Propriétaire
-   Locataire
-   Prospect / Acheteur
-   Fournisseur / Vendor

### 4.3 Séparation importante

``` text
Entreprise cliente
├── Admin
├── Property Managers
├── Sales Agents
├── Finance Staff
└── Internal Technicians

Entreprises externes
└── Vendors
```

Un fournisseur externe n'est pas un utilisateur interne de
l'organisation cliente.

------------------------------------------------------------------------

## 5. Objectifs fonctionnels

Le système doit permettre de :

-   gérer les organisations et leurs utilisateurs ;
-   structurer propriétés, bâtiments, étages et unités ;
-   gérer les propriétaires et leurs droits économiques ;
-   gérer les locataires et les baux ;
-   générer et suivre les loyers ;
-   gérer les factures, paiements et soldes ;
-   traiter les demandes de maintenance ;
-   affecter un work order à un technicien interne ou à un fournisseur
    externe ;
-   gérer les fournisseurs comme de vraies entités métier ;
-   gérer les biens et unités disponibles à la vente ;
-   gérer prospects, visites, offres, réservations et ventes ;
-   suivre dépenses et flux financiers associés ;
-   gérer les documents ;
-   envoyer des notifications ;
-   produire des rapports ;
-   assurer la traçabilité des actions importantes.

------------------------------------------------------------------------

## 6. Modèle immobilier général

Le bien immobilier est une donnée de référence partagée par plusieurs
domaines.

``` text
Property
  └── Building (si nécessaire)
       └── Floor (si nécessaire)
            └── Unit
```

Une maison individuelle ne doit pas être artificiellement forcée dans
une hiérarchie complexe.

Une **Unit** doit pouvoir être utilisée par plusieurs processus :

``` text
Unit
├── Rental lifecycle
├── Sales lifecycle
├── Maintenance
└── Reporting
```

Le principe est de conserver une identité centrale du bien plutôt que de
créer des copies indépendantes pour la location et la vente.

------------------------------------------------------------------------

## 7. Gestion locative

### 7.1 Flux de référence

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
  ↓
Balance
```

### 7.2 Fonctionnalités générales

-   fiches locataires ;
-   baux ;
-   dates et échéances ;
-   loyer ;
-   dépôt de garantie ;
-   génération de charges ;
-   facturation ;
-   paiements ;
-   allocation des paiements ;
-   soldes ;
-   renouvellement ;
-   résiliation ;
-   documents contractuels ;
-   historique.

------------------------------------------------------------------------

## 8. Vente immobilière

La vente est un **domaine fonctionnel de premier niveau**, au même titre
que la location.

Deux modèles de vendeur sont couverts par un même workflow (D-008) :

| Vendeur | Exemple | Rôle de l'organisation | Revenu de l'organisation |
|---|---|---|---|
| Propriétaire tiers | un propriétaire (par exemple quelqu'un qui a acheté à la société) lui confie un mandat de vente | intermédiaire sous mandat | commission |
| L'organisation elle-même | la société vend des unités qu'elle possède (par exemple celles de son propre projet) | vendeur | prix de vente |

Chaque vente a exactement un vendeur. L'organisation peut donc être
propriétaire d'unités.

### 8.1 Flux de référence

``` text
Property / Unit
  ↓
Sales Mandate (vendeur tiers) ou Own Stock (organisation vendeuse)
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
Sales Transaction / Contract
  ↓
Payment / Settlement
  ↓
Closing
```

### 8.2 Capacités générales

-   désigner un bien / unité comme disponible à la vente ;
-   créer et gérer un listing ;
-   enregistrer des prospects ;
-   suivre les visites ;
-   recevoir et enregistrer des offres ;
-   gérer les réservations ;
-   suivre la transaction ;
-   enregistrer les paiements de vente ;
-   enregistrer la clôture ;
-   produire des indicateurs commerciaux.

### 8.3 Limite

Le système n'est pas une marketplace immobilière publique. Il sert
d'abord à l'entreprise pour piloter ses propres opérations commerciales.

### 8.4 Découpage MVP / V1 / V2 (D-009)

-   **MVP** : unités à vendre, mandat de vente simple, listings,
    prospects / acheteurs, visites, offres, réservation (acompte,
    expiration), vente, paiements acheteur, clôture et transfert de
    propriété. Pas d'accès acheteur.
-   **V1** : commissions (calcul, facturation, encaissement),
    échéanciers de paiement, portail acheteur, modèles de documents de
    vente, suivi de la vente par le propriétaire, rapprochement
    critères acheteur / unités (par règles).
-   **V2** : vente sur plan avec échéances liées à l'avancement des
    travaux, commissions avancées (plusieurs commerciaux),
    publication vers des portails d'annonces externes.

------------------------------------------------------------------------

## 9. Maintenance et interventions

### 9.1 Flux de référence

``` text
Maintenance Request
       ↓
Review
       ↓
Work Order
       ↓
Assignment
   ┌───┴──────────┐
   ↓              ↓
Internal       External
Technician      Vendor
   └──────┬───────┘
          ↓
      Completion
          ↓
      Verification
          ↓
      Cost / Expense
```

### 9.2 Technicien interne

Le technicien interne est un salarié de l'organisation. Son accès est
limité aux interventions et aux informations nécessaires à leur
exécution.

### 9.3 Fournisseur externe

Le fournisseur est une société ou un professionnel externe. Il doit être
représenté par une vraie entité métier et non seulement par un champ
texte dans un work order.

### 9.4 Évolution du domaine fournisseur

``` text
MVP:
Vendor → Work Order → Completion → Expense / Invoice Reference

Future:
Vendor → Quote → Approval → Purchase Order → Work Order
       → Vendor Invoice → Payment
```

------------------------------------------------------------------------

## 10. Finance et transactions

Les flux financiers sont transversaux aux domaines.

### 10.1 Location

``` text
Lease → Charge → Invoice → Payment → Allocation → Balance
```

### 10.2 Maintenance / fournisseurs

``` text
Work Order → Expense

Vendor → Invoice → Approval → Payment
```

### 10.3 Vente

``` text
Sale → Payment Schedule → Payments → Settlement → Closing
```

Les opérations financières importantes ne doivent pas être corrigées par
suppression silencieuse. Le système privilégiera ajustements,
annulations contrôlées, remboursements et écritures correctives selon
les règles détaillées du cahier Finance.

------------------------------------------------------------------------

## 11. Documents

Les documents doivent être rattachés au contexte métier.

Exemples :

``` text
Property → titres / documents immobiliers
Owner → documents d'identité / propriété
Renter → identification
Lease → contrat
Vendor → certificats / contrats
Work Order → photos / justificatifs
Sale → documents de transaction
```

Chaque document devra avoir un type, une relation métier et des règles
d'accès appropriées.

------------------------------------------------------------------------

## 12. Notifications

Le système peut notifier les acteurs lors d'événements importants :

-   loyer dû ;
-   loyer en retard ;
-   bail arrivant à expiration ;
-   intervention affectée ;
-   intervention terminée ;
-   facture fournisseur reçue ;
-   offre reçue ;
-   offre acceptée ;
-   réservation expirant ;
-   vente clôturée.

Canaux initiaux : in-app et email.

Dans le MVP, les acteurs externes (locataires, propriétaires) n'ont pas
d'accès : ils sont notifiés par email uniquement (D-010).

------------------------------------------------------------------------

## 13. Reporting

### Location

-   occupation ;
-   loyers dus ;
-   loyers encaissés ;
-   impayés ;
-   échéances.

### Maintenance

-   demandes ouvertes ;
-   work orders en cours ;
-   coûts ;
-   interventions internes / externes ;
-   activité fournisseur.

### Vente

-   biens en vente ;
-   listings actifs ;
-   prospects ;
-   offres ;
-   réservations ;
-   ventes clôturées ;
-   montants.

### Propriétaire

-   revenus ;
-   dépenses ;
-   maintenance ;
-   location ;
-   ventes ;
-   relevés.

------------------------------------------------------------------------

## 14. Principes métier généraux

### 14.1 Appartenance organisationnelle

Toute donnée métier appartient à une organisation ou à une relation
contrôlée appartenant à une organisation.

### 14.2 Isolation

Une organisation ne doit jamais accéder aux données d'une autre
organisation.

### 14.3 Moindre privilège

Chaque acteur reçoit uniquement les permissions nécessaires à son
activité.

### 14.4 Contexte de ressource

La permission doit dépendre du rôle **et** du périmètre de la ressource.

### 14.5 Historisation

Les changements importants affectant la finance, les contrats, les
droits ou les rapports historiques doivent rester reconstituables.

### 14.6 Source de vérité

Une information de référence, par exemple l'identité d'une Unit, doit
avoir une source principale réutilisée par plusieurs domaines.

------------------------------------------------------------------------

## 15. États métier de haut niveau

Les états ci-dessous sont des candidats destinés à cadrer le travail de
Phase 1. Les transitions définitives seront spécifiées dans les cahiers
spécialisés.

### Lease

``` text
DRAFT → PENDING → ACTIVE → EXPIRED
                         ↘ TERMINATED
```

### Work Order

``` text
OPEN → ASSIGNED → IN_PROGRESS → COMPLETED → VERIFIED → CLOSED
```

### Sales Listing

``` text
DRAFT → ACTIVE → UNDER_OFFER → RESERVED → SOLD
                         └→ CANCELLED
```

### Invoice

``` text
DRAFT → ISSUED → PARTIALLY_PAID → PAID
                      └──────────→ VOID
```

------------------------------------------------------------------------

## 16. Matrice d'acteurs --- principe

| Domaine | Admin | Manager | Commercial | Finance | Tech. interne | Owner | Renter | Vendor | Prospect/Buyer |
|---|---|---|---|---|---|---|---|---|---|
| Organisation | Gérer | Limité | Non | Limité | Non | Non | Non | Non | Non |
| Propriétés | Gérer | Gérer | Biens en vente | Voir | Voir le nécessaire | Ses biens | Son unité | Contexte assigné | Bien concerné |
| Location | Gérer | Gérer | Non | Gérer | Non | Son périmètre | Son bail | Non | Non |
| Finance | Gérer | Selon rôle | Paiements de ses ventes | Gérer | Non | Son périmètre | Ses paiements | Ses factures | Sa transaction |
| Maintenance | Gérer | Gérer | Non | Voir coûts | Exécuter assigné | Voir selon droits | Créer / suivre | Exécuter assigné | Non |
| Vente | Gérer | Gérer | Gérer (son périmètre) | Selon rôle | Non | Selon mandat | Non | Non | Parcours lié à son intérêt |

Les rôles et départements sont définis par chaque organisation (D-011) ;
cette table donne les **rôles modèles** proposés par défaut.

Cette table constitue une orientation. Le cahier « Organisation & Accès
» produira la matrice d'autorisation exhaustive, action par action.

**MVP (D-010) :** seuls les utilisateurs internes (Admin, Manager,
Commercial, Finance, Technicien interne) se connectent. Les colonnes
Owner, Renter, Vendor et Prospect/Buyer décrivent les accès prévus à
partir de la V1.

------------------------------------------------------------------------

## 17. Cas d'utilisation majeurs

-   UC-001 Créer une organisation
-   UC-002 Ajouter un bien / une unité
-   UC-003 Enregistrer un propriétaire
-   UC-004 Créer un locataire et un bail
-   UC-005 Générer une charge locative
-   UC-006 Enregistrer et allouer un paiement
-   UC-007 Déclarer une maintenance
-   UC-008 Affecter une intervention à un technicien ou fournisseur
-   UC-009 Terminer et vérifier une intervention
-   UC-010 Enregistrer une dépense / facture fournisseur
-   UC-011 Mettre une unité en vente
-   UC-012 Enregistrer un prospect / acheteur
-   UC-013 Organiser / enregistrer une visite
-   UC-014 Enregistrer une offre
-   UC-015 Créer une réservation
-   UC-016 Clôturer une vente
-   UC-017 Produire un reporting propriétaire
-   UC-018 Consulter les événements d'audit

Chaque cas d'utilisation sera détaillé ensuite avec préconditions,
données, règles et critères d'acceptation.

------------------------------------------------------------------------

## 18. Exigence de traçabilité

Les opérations significatives doivent produire des événements d'audit,
notamment pour :

-   création / modification de contrats ;
-   modifications financières ;
-   changements d'état importants ;
-   approbations ;
-   affectations ;
-   opérations sensibles sur les documents ;
-   actions de sécurité.

Exemple :

``` text
User A
→ Invoice #123 created

User B
→ Payment #77 allocated to Invoice #123

User C
→ Work Order #45 assigned to Vendor #8

User D
→ Sale #22 changed to CLOSED
```

------------------------------------------------------------------------

## 19. Non-fonctionnel --- cadre général

Le produit vise notamment :

### Sécurité

-   authentification ;
-   RBAC / contrôle d'accès ;
-   isolation multi-tenant ;
-   validation des entrées ;
-   gestion sécurisée des secrets ;
-   audit ;
-   sécurité des documents ;
-   scans de sécurité en CI/CD.

### Fiabilité

-   health checks ;
-   gestion des erreurs ;
-   idempotence des opérations critiques ;
-   sauvegardes ;
-   restauration documentée.

### Performance

-   pagination ;
-   indexation ;
-   requêtes maîtrisées ;
-   traitement asynchrone lorsque pertinent.

### Observabilité

-   logs structurés ;
-   métriques ;
-   traces lorsque pertinentes ;
-   alertes ;
-   indicateurs métier.

### Internationalisation

-   français ;
-   arabe ;
-   anglais ;
-   RTL ;
-   TND ;
-   extensibilité multi-devise.

------------------------------------------------------------------------

## 20. Hors périmètre

Le produit n'est pas :

-   une marketplace immobilière publique ;
-   un clone d'Airbnb ;
-   un PMS hôtelier ;
-   un ERP de construction complet ;
-   un CRM généraliste ;
-   une suite comptable complète de type SAP ;
-   une plateforme bancaire ;
-   une plateforme IA / ML ;
-   un ensemble de microservices créés uniquement pour la complexité.

Les fournisseurs externes sont **dans le périmètre**.

Les fonctions de procurement avancé, contrats fournisseurs complexes,
comptes fournisseurs complets et commissions commerciales avancées sont
progressives et seront réparties entre MVP, V1 et V2.

La vente immobilière est **dans le périmètre** ; son découpage MVP / V1
/ V2 est fixé en §8.4.

------------------------------------------------------------------------

## 21. Critères fonctionnels de sortie

Le cahier général est considéré comme suffisamment défini lorsque :

-   les acteurs sont identifiés ;
-   les domaines fonctionnels sont délimités ;
-   location et vente sont toutes deux couvertes ;
-   technicien interne et fournisseur externe sont distingués ;
-   les flux métier majeurs sont connus ;
-   les interactions inter-domaines sont identifiées ;
-   les principales contraintes de sécurité et de traçabilité sont
    explicites ;
-   les limites du produit sont claires ;
-   les questions nécessitant une spécification spécialisée sont
    listées.

------------------------------------------------------------------------

## 22. Décisions structurantes

### D-001 --- SaaS multi-tenant

Plusieurs entreprises clientes utilisent la plateforme avec isolation
logique stricte.

### D-002 --- Pas d'IA / ML

Aucun module IA / ML n'est nécessaire pour le produit.

### D-003 --- Techniciens internes

Les techniciens peuvent être des employés de l'entreprise cliente.

### D-004 --- Fournisseurs externes

Les entreprises externes sont des acteurs métier à part entière.

### D-005 --- Vente immobilière

La vente de biens et d'unités est officiellement incluse dans le
produit.

### D-006 --- Pas de marketplace publique

La plateforme reste un ERP métier utilisé par les entreprises.

### D-007 --- Une même base métier pour location et vente

Property / Unit constitue une référence commune ; les workflows de
location et de vente restent distincts et historisés.

### D-008 --- Deux modèles de vendeur

Le vendeur d'une vente est soit un propriétaire tiers sous mandat de
vente (l'organisation est intermédiaire et perçoit une commission),
soit l'organisation elle-même lorsqu'elle possède l'unité (par exemple
un promoteur qui vend son propre projet). Un seul workflow de vente
couvre les deux cas.

### D-009 --- Vente de base dans le MVP

Le MVP inclut un périmètre de vente de base (voir §8.4). Commissions,
échéanciers, portail acheteur et vente sur plan sont progressifs.

### D-010 --- MVP réservé aux utilisateurs internes

Le MVP est l'ERP utilisé par le personnel de l'organisation. Locataires,
prospects, propriétaires, fournisseurs et acheteurs n'ont pas
d'interface avant la V1 : ils existent comme fiches gérées par le
personnel et reçoivent emails et documents.

### D-011 --- Départements et rôles définis par chaque organisation

Chaque organisation est organisée différemment. Dès le MVP,
l'Administrateur de chaque organisation définit ses départements
(arbre hiérarchique) et ses rôles, au moyen d'une grille de privilèges
à cocher (Voir, Créer, Modifier, Supprimer, ou aucun accès) par domaine.
Les départements limitent la visibilité des fiches. Cinq rôles modèles
sont fournis. Détail : Cahier des Charges 01, section 6.

### D-012 --- Le client est une société immobilière qui possède des biens

Le client de l'ERP (l'Organisation) est une **société immobilière** qui
**possède** des biens : elle en loue la plupart et en vend certains. Les
**agences**, intermédiaires qui ne possèdent aucun bien, ne sont **pas**
des clients. Quand la société vend un bien ou une unité, l'**acheteur**
(personne ou entreprise) en devient **propriétaire** et figure dans les
propriétaires de l'ERP.

------------------------------------------------------------------------

## 23. Questions à résoudre dans les cahiers détaillés

### Organisation & Accès

-   un compte par organisation (réglé : rien n'est partagé entre organisations) ;
-   rôles ;
-   permissions ;
-   scopes ;
-   accès externes ;
-   isolation tenant.

### Biens & Propriétés

-   granularité Property / Building / Floor / Unit ;
-   ownership ;
-   copropriété / multi-propriété ;
-   disponibilité ;
-   historique des statuts.

### Location

-   structure d'un bail ;
-   indexation / révision ;
-   dépôts ;
-   renouvellements ;
-   résiliation.

### Vente

-   structure du listing ;
-   prospect vs buyer ;
-   visites ;
-   offres multiples ;
-   réservation ;
-   contrat ;
-   paiements ;
-   circuit de paiement du prix (organisation, notaire, direct) ;
-   acompte de réservation (montant, restitution, expiration) ;
-   closing ;
-   commissions ;
-   effet d'une vente sur un bail en cours.

### Maintenance

-   catégories ;
-   priorité ;
-   planification ;
-   matériaux ;
-   coûts ;
-   approbations ;
-   SLA éventuels.

### Vendors

-   profil ;
-   contacts ;
-   catégories ;
-   documents ;
-   contrats ;
-   devis ;
-   factures ;
-   portail.

### Finance

-   modèle de transaction ;
-   allocation ;
-   remboursements ;
-   frais ;
-   taxes ;
-   devises ;
-   rapprochement éventuel.

------------------------------------------------------------------------

## 24. Découpage Phase 1

Le cahier général est complété par :

``` text
00 — Cahier des Charges Général
01 — Organisation & Accès
02 — Biens & Propriétés
03 — Propriétaires
04 — Location & Gestion Locative
05 — Vente Immobilière
06 — Prospects / Acheteurs
07 — Finance & Transactions
08 — Maintenance & Techniciens
09 — Fournisseurs / Vendors
10 — Documents / Notifications / Reporting / Audit
11 — Exigences Transversales & Critères d'Acceptation
```

Les références croisées entre ces documents sont nécessaires. Ils
constituent une seule spécification produit, pas onze applications
indépendantes.

------------------------------------------------------------------------

## 25. Livrables de sortie de Phase 1

La Phase 1 doit produire :

-   cahiers des charges détaillés ;
-   workflows métier ;
-   règles métier ;
-   use cases ;
-   critères d'acceptation ;
-   state machines ;
-   matrice d'autorisation ;
-   modèle conceptuel de domaine ;
-   premières cardinalités ;
-   modèle financier conceptuel ;
-   ERD initial ;
-   décisions structurantes documentées.

------------------------------------------------------------------------

## 26. Prochaine étape

Le prochain document à construire est :

> **Cahier des Charges 01 --- Organisation & Accès**

Il devra définir précisément :

``` text
Organization
  ↓
Membership
  ↓
User
  ↓
Role
  ↓
Permission
  ↓
Scope
  ↓
Tenant Isolation
```

Ce cahier sera la fondation de tous les autres domaines.
