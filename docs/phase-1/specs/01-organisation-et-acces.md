# Cahier des Charges 01 --- Organisation & Accès

**Projet :** Cloud-Native Multi-Tenant Real Estate Operations ERP\
**Document :** Cahier des Charges 01 --- Organisation & Accès\
**Phase :** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version :** 0.2\
**Statut :** Brouillon --- les points marqués *(proposition)* sont à
valider\
**Date :** 2026-10-08\
**Dépend de :** [Cahier des Charges Général](00-cahier-des-charges-general.md),
[Phase 0](../../phase-0/01-product-definition.md),
[Glossaire](../../reference/01-glossary.md),
[Matrice de périmètre](../../reference/05-scope-matrix.md)

------------------------------------------------------------------------

## En bref

Ce cahier répond à trois questions :

1.  **Qui peut se connecter au logiciel ?**
2.  **Que peut voir et faire chaque personne ?**
3.  **Comment garantir qu'une entreprise cliente ne voit jamais les
    données d'une autre ?**

Il est la fondation de tous les autres cahiers : chaque fonction décrite
ailleurs (bail, paiement, intervention, vente) devra dire *qui* a le
droit de l'utiliser, en s'appuyant sur les règles définies ici.

**Rappel de la décision D-010 :** dans le MVP (la première version),
seul le personnel de l'entreprise cliente se connecte. Locataires,
propriétaires, fournisseurs et acheteurs n'ont pas d'accès avant la V1.

------------------------------------------------------------------------

## 1. Un exemple pour comprendre

> L'**Agence Médina** (exemple fictif) gère 300 logements et bureaux à
> Tunis et vend aussi des appartements. Elle s'abonne au logiciel. Dans
> le logiciel, l'Agence Médina est une **Organisation**.
>
> Elle a cinq employés :
>
> -   **Sonia**, la directrice → rôle **Administrateur**
> -   **Karim**, qui gère les locations → rôle **Gestionnaire**
> -   **Amira**, qui s'occupe des ventes → rôle **Commercial**
> -   **Hédi**, le comptable → rôle **Finance**
> -   **Ali**, le technicien → rôle **Technicien interne**
>
> Sonia crée l'organisation (avec l'aide de l'opérateur de la
> plateforme), puis invite les quatre autres par email. Chacun reçoit un
> lien, choisit son mot de passe et se connecte.
>
> Chaque employé arrive alors dans **sa propre interface**, conçue pour
> son métier : menus, tableau de bord et écrans ne sont pas les mêmes
> pour Karim, Amira, Hédi ou Ali.
>
> -   Karim voit tous les biens, locataires et baux, mais ne peut pas
>     gérer les utilisateurs.
> -   Amira voit les biens à vendre, les prospects et les ventes, mais
>     pas les loyers des locataires.
> -   Hédi voit et gère toute la finance.
> -   Ali ne voit que les interventions qui lui sont affectées, avec
>     l'adresse et l'unité concernées. Il ne voit aucun montant.
>
> Une autre agence, l'**Agence Carthage**, utilise le même logiciel :
> c'est une deuxième **Organisation**, avec ses propres employés et
> leurs propres interfaces.
> Personne à l'Agence Médina ne peut voir quoi que ce soit de l'Agence
> Carthage, et inversement --- même en tapant une adresse web ou un
> numéro de dossier au hasard.

------------------------------------------------------------------------

## 2. Périmètre

### 2.1 Ce que couvre ce cahier

-   la création et la vie d'une organisation ;
-   les utilisateurs, leurs comptes et leur connexion ;
-   l'invitation de nouveaux membres ;
-   les rôles, les permissions et les périmètres ;
-   la séparation stricte entre organisations ;
-   les événements d'audit liés aux accès ;
-   la préparation des accès externes de la V1.

### 2.2 Ce qu'il ne couvre pas

| Sujet | Où il est traité |
|---|---|
| Écrans d'administration de l'opérateur, abonnements, plans | Cahier des Charges 12 --- Administration Plateforme & Abonnements |
| Contenu détaillé des journaux d'audit et leur consultation | Cahier des Charges 10 --- Documents / Notifications / Reporting / Audit |
| Solution technique d'authentification et d'isolation | Phase 2 --- Security Architecture, Multi-Tenancy & Isolation Design |

### 2.3 Découpage MVP / V1 / plus tard

| Capacité | Version |
|---|---|
| Création d'une organisation | MVP |
| Comptes utilisateurs, connexion, mot de passe | MVP |
| Invitation par email | MVP |
| Rôles internes prédéfinis : Administrateur, Gestionnaire, Commercial, Finance, Technicien interne | MVP |
| Plusieurs rôles pour une même personne | MVP |
| Périmètre par portefeuille de biens | MVP *(proposition)* |
| Une même personne dans plusieurs organisations | MVP |
| Double authentification (code en plus du mot de passe) | MVP *(proposition)* |
| Journal d'audit des accès | MVP |
| Comptes pour locataires, propriétaires, fournisseurs, acheteurs | V1 (D-010) |
| Accès du support avec accord du client | V1 |
| Rôles personnalisés créés par l'organisation | V2 |
| Connexion avec le compte d'entreprise du client (authentification unique) | Plus tard |

------------------------------------------------------------------------

## 3. Les notions de base

Les termes ci-dessous suivent le [Glossaire](../../reference/01-glossary.md).
Une image aide : le logiciel est un **immeuble de bureaux** partagé par
plusieurs entreprises.

| Notion | Définition simple | Dans l'image de l'immeuble |
|---|---|---|
| **Organisation** | Une entreprise cliente qui utilise le logiciel (agence, gestionnaire, promoteur). | Une entreprise locataire d'un étage. |
| **Utilisateur** | Une personne qui peut se connecter. Une personne = un compte, identifié par son email. | Une personne avec une carte d'accès. |
| **Appartenance** | Le lien entre un utilisateur et une organisation. C'est elle qui porte les rôles. | La carte d'accès donne accès à un étage précis. |
| **Rôle** | Un ensemble de droits correspondant à un métier (Gestionnaire, Finance…). | Le type de carte : employé, comptable, technicien. |
| **Permission** | Le droit de faire une action précise sur un type de fiche (exemple : « enregistrer un paiement »). | Le droit d'ouvrir une porte précise. |
| **Périmètre** | La partie des données sur laquelle une permission s'applique (toute l'organisation, certains biens, les interventions affectées). | Les pièces de l'étage où la carte fonctionne. |
| **Opérateur de la plateforme** | La société qui exploite le logiciel (vous). | Le gestionnaire de l'immeuble : il ouvre et ferme les étages, mais n'entre pas dans les bureaux. |

**Règle d'or :** une personne ne fait une action que si elle a une
appartenance active à l'organisation, un rôle qui contient la
permission, **et** que la fiche concernée est dans son périmètre.

------------------------------------------------------------------------

## 4. Les acteurs

### 4.1 Dans le MVP

| Acteur | Qui c'est | Ce qu'il fait principalement |
|---|---|---|
| Administrateur d'organisation | Le responsable de l'entreprise cliente | Paramètre l'organisation, invite et gère les membres, a accès à tout |
| Gestionnaire | Employé chargé des locations et de la maintenance | Biens, propriétaires, locataires, baux, interventions, fournisseurs |
| Commercial | Employé chargé des ventes | Mandats de vente, annonces, prospects, visites, offres, réservations, ventes |
| Finance | Comptable ou assistant administratif | Factures, paiements, soldes, dépenses, relevés propriétaires |
| Technicien interne | Employé qui réalise les interventions | Ses interventions affectées uniquement |
| Opérateur de la plateforme | Vous, la société qui exploite le logiciel | Crée et suspend les organisations ; n'accède pas aux données métier |

### 4.2 À partir de la V1

Locataire, Propriétaire, Utilisateur fournisseur, Acheteur. Voir la
section 11.

------------------------------------------------------------------------

## 5. Les fiches et leurs informations

Les informations listées sont des informations **métier**. Le modèle
technique sera défini en Phase 2.

### 5.1 Organisation

| Information | Exemple | Obligatoire |
|---|---|---|
| Nom commercial | Agence Médina | Oui |
| Raison sociale | Médina Immobilier SARL | Oui |
| Matricule fiscal | 1234567/A/M/000 | Non *(à valider : obligatoire pour la facturation ?)* |
| Adresse | 12 rue de Marseille, Tunis | Oui |
| Téléphone, email de contact | | Oui |
| Logo | | Non |
| Langue par défaut | Français | Oui |
| Devise | TND | Oui (TND seule dans le MVP) |
| Activités | Location, Vente, ou les deux | Oui |
| Statut | `ACTIVE` | Oui (voir section 9) |
| Date de création | | Automatique |

L'activité choisie sert seulement à adapter les menus : une
organisation qui ne fait que de la location ne voit pas le menu Vente.
Elle peut l'activer plus tard.

### 5.2 Utilisateur

| Information | Exemple | Obligatoire |
|---|---|---|
| Prénom, nom | Karim Ben Salah | Oui |
| Email | karim@agence-medina.tn | Oui, unique sur toute la plateforme |
| Téléphone mobile | | Non |
| Langue de l'interface | Arabe | Oui (par défaut : celle de l'organisation) |
| Double authentification activée | Oui / Non | Oui |
| Statut | `ACTIVE` | Oui |

Le mot de passe n'est jamais visible, ni par l'organisation, ni par
l'opérateur.

### 5.3 Appartenance

| Information | Exemple |
|---|---|
| Utilisateur | Karim Ben Salah |
| Organisation | Agence Médina |
| Rôles | Gestionnaire (une personne peut en avoir plusieurs) |
| Périmètre | Toute l'organisation, ou une liste de biens |
| Statut | `ACTIVE` |
| Date d'entrée, date de sortie | |
| Invité par | Sonia (Administrateur) |

### 5.4 Invitation

| Information | Exemple |
|---|---|
| Email invité | karim@agence-medina.tn |
| Organisation | Agence Médina |
| Rôles proposés | Gestionnaire |
| Périmètre proposé | Toute l'organisation |
| Envoyée par, date d'envoi | Sonia, 2026-10-08 |
| Date d'expiration | 7 jours après l'envoi *(proposition)* |
| Statut | `PENDING` |

### 5.5 Combien de quoi ? (cardinalités)

-   Une organisation a **un ou plusieurs** membres, dont au moins un
    Administrateur actif.
-   Un utilisateur a **une ou plusieurs** appartenances (exemple : un
    comptable indépendant qui travaille pour deux agences).
-   Une appartenance a **un ou plusieurs** rôles.
-   Une fiche métier (bien, bail, paiement…) appartient à **une seule**
    organisation, pour toujours.

------------------------------------------------------------------------

## 6. Les rôles et ce qu'ils permettent

### 6.1 Principe

-   Les rôles sont **prédéfinis** dans le MVP : l'organisation ne peut
    pas en créer de nouveaux (V2).
-   Les permissions s'**additionnent** : une personne Gestionnaire **et**
    Commercial a les droits des deux rôles.
-   Il n'existe pas de « droit d'interdire » : on ne retire pas une
    permission, on choisit les bons rôles.
-   L'**Administrateur a tous les droits** de l'organisation
    *(proposition)* --- dans une petite agence, c'est souvent le
    directeur, qui doit pouvoir tout faire.

### 6.2 Tableau des droits (première version)

Légende : **Gérer** = créer, modifier, annuler · **Voir** = consulter
seulement · **Non** = aucun accès · **Affecté** = seulement les
interventions qui lui sont affectées.

| Domaine | Action | Administrateur | Gestionnaire | Commercial | Finance | Technicien |
|---|---|---|---|---|---|---|
| Organisation | Paramètres de l'organisation | Gérer | Non | Non | Non | Non |
| Organisation | Membres, invitations, rôles | Gérer | Non | Non | Non | Non |
| Organisation | Journal d'audit | Voir | Non | Non | Non | Non |
| Biens | Biens, bâtiments, étages, unités | Gérer | Gérer | Voir | Voir | Affecté (adresse et unité) |
| Propriétaires | Fiches propriétaires, propriété, mandats de gestion | Gérer | Gérer | Voir | Voir | Non |
| Location | Locataires et baux | Gérer | Gérer | Non | Voir | Non *(voir règle 13)* |
| Finance | Factures, paiements, imputations, reçus | Gérer | Voir | Non | Gérer | Non |
| Finance | Soldes et impayés | Voir | Voir | Non | Voir | Non |
| Finance | Dépenses | Gérer | Gérer | Non | Gérer | Non |
| Finance | Relevés propriétaires | Gérer | Voir | Non | Gérer | Non |
| Maintenance | Demandes d'intervention | Gérer | Gérer | Non | Voir | Affecté |
| Maintenance | Ordres de travail : créer, affecter | Gérer | Gérer | Non | Voir | Non |
| Maintenance | Ordres de travail : avancement, notes, photos, fin des travaux | Gérer | Gérer | Non | Non | Affecté |
| Maintenance | Validation des travaux | Gérer | Gérer | Non | Non | Non |
| Fournisseurs | Fiches fournisseurs | Gérer | Gérer | Non | Voir | Non |
| Vente | Mandats de vente, annonces, prospects, visites, offres, réservations | Gérer | Voir | Gérer | Voir | Non |
| Vente | Paiements des acheteurs | Gérer | Non | Voir | Gérer | Non |
| Vente | Clôture d'une vente | Gérer | Non | Gérer | Voir | Non |
| Rapports | Rapports de location et de maintenance | Voir | Voir | Non | Voir | Non |
| Rapports | Rapports de vente | Voir | Non | Voir | Voir | Non |
| Rapports | Rapports financiers | Voir | Voir | Non | Voir | Non |

**Documents :** un document suit les droits de la fiche à laquelle il
est attaché. Exemple : une photo d'intervention est visible par le
technicien affecté ; une copie de carte d'identité d'un locataire ne
l'est pas.

Ce tableau est la première version de l'**Authorization Matrix**
(matrice d'autorisation complète). Chaque cahier suivant la complétera
action par action.

### 6.3 Périmètre

| Périmètre | Qui | Exemple |
|---|---|---|
| Toute l'organisation | Par défaut pour tous les rôles sauf Technicien | Karim voit les 300 biens |
| Un portefeuille de biens *(proposition)* | Gestionnaire ou Commercial | Une agence avec deux gestionnaires : chacun ne voit que ses 150 biens |
| Les interventions affectées | Technicien | Ali voit 4 interventions cette semaine |

### 6.4 Une interface dédiée à chaque employé

Le logiciel a trois niveaux :

``` text
Plateforme (le logiciel)
└── Organisations (les entreprises clientes : Agence Médina, Agence Carthage…)
    └── Employés de chaque organisation
        └── Interface dédiée à chaque employé, selon son ou ses rôles
```

Dans une organisation, chaque employé a **sa propre interface** :

| Rôle | Ce que contient son interface |
|---|---|
| Administrateur | Tableau de bord global, paramètres, membres et rôles, journal d'audit, tous les menus |
| Gestionnaire | Tableau de bord locatif (occupation, impayés, interventions ouvertes), biens, propriétaires, locataires, baux, maintenance, fournisseurs |
| Commercial | Tableau de bord des ventes (biens à vendre, visites prévues, offres, réservations qui expirent), prospects, ventes |
| Finance | Tableau de bord financier (encaissements, impayés, dépenses), factures, paiements, relevés propriétaires |
| Technicien interne | Ses interventions du jour et à venir, pensée pour le téléphone |

Règles :

-   L'interface ne montre **que** ce que le rôle permet : un menu sans
    droit n'apparaît pas.
-   Une personne avec plusieurs rôles a une interface qui réunit les
    menus de ces rôles.
-   Masquer un menu ne remplace jamais le contrôle par le serveur
    (règle 14).
-   Le contenu exact de chaque écran est défini dans les cahiers
    métier et dans l'Information Architecture (Phase 2).

------------------------------------------------------------------------

## 7. Comment ça se passe (workflows)

### 7.1 Démarrer une nouvelle organisation

``` text
L'opérateur crée l'organisation
        ↓
Il invite le premier Administrateur (email)
        ↓
L'Administrateur accepte l'invitation, choisit son mot de passe
        ↓
Il complète les paramètres de l'organisation
        ↓
Il invite les autres membres
```

### 7.2 Faire entrer un nouvel employé

``` text
L'Administrateur saisit l'email, le rôle et le périmètre
        ↓
Le logiciel envoie une invitation par email (valable 7 jours)
        ↓
L'employé clique sur le lien
   ├── il n'a pas encore de compte → il crée son mot de passe
   └── il a déjà un compte (autre organisation) → il se connecte
        ↓
L'appartenance devient active
        ↓
L'employé voit l'organisation dans son espace
```

### 7.3 Se connecter

``` text
Email + mot de passe
        ↓
Code de double authentification (si activée ou obligatoire)
        ↓
Une seule organisation ? → elle s'ouvre directement
Plusieurs organisations ? → la personne choisit
        ↓
Toutes les données affichées appartiennent à l'organisation choisie
```

### 7.4 Faire sortir un employé

``` text
L'Administrateur désactive l'appartenance
        ↓
Accès coupé immédiatement, sessions ouvertes fermées
        ↓
Tout ce que l'employé a fait reste dans l'historique, à son nom
```

------------------------------------------------------------------------

## 8. Cas d'utilisation

La numérotation suit celle du Cahier des Charges Général (UC-001 à
UC-018). Les nouveaux cas commencent à UC-019.

### UC-001 --- Créer une organisation

| | |
|---|---|
| Acteur | Opérateur de la plateforme |
| Version | MVP |
| Avant | Le client a signé ; son nom et l'email de son responsable sont connus. |
| Déroulement | 1. L'opérateur saisit le nom, la raison sociale, l'adresse, la langue, les activités. 2. Il saisit l'email du premier Administrateur. 3. Le logiciel crée l'organisation au statut `ACTIVE` et envoie l'invitation. |
| Cas particuliers | L'email appartient déjà à un utilisateur : l'invitation lui est envoyée normalement ; il se connecte avec son compte existant. |
| Après | L'organisation existe, vide, avec une invitation Administrateur en attente. |
| Règles | 1, 5 |
| Événement | `OrganizationCreated` |

### UC-019 --- Inviter un membre

| | |
|---|---|
| Acteur | Administrateur |
| Version | MVP |
| Avant | L'Administrateur est connecté à son organisation. |
| Déroulement | 1. Il saisit l'email, choisit un ou plusieurs rôles et le périmètre. 2. Le logiciel envoie l'invitation. |
| Cas particuliers | La personne est déjà membre active : le logiciel le signale et propose de modifier ses rôles. Une invitation est déjà en attente pour cet email : le logiciel propose de la renvoyer. |
| Après | Invitation au statut `PENDING`. |
| Règles | 6, 7 |
| Événement | `MemberInvited` |

### UC-020 --- Accepter une invitation

| | |
|---|---|
| Acteur | Personne invitée |
| Version | MVP |
| Avant | Elle a reçu l'email d'invitation. |
| Déroulement | 1. Elle clique sur le lien. 2. Sans compte : elle saisit nom, prénom, mot de passe. Avec compte : elle se connecte. 3. L'appartenance devient active. |
| Cas particuliers | Lien expiré ou déjà utilisé : message clair, la personne doit demander une nouvelle invitation. |
| Après | Appartenance `ACTIVE`. |
| Règles | 4, 7 |
| Événement | `InvitationAccepted` |

### UC-021 --- Se connecter

| | |
|---|---|
| Acteur | Tout utilisateur |
| Version | MVP |
| Déroulement | 1. Email et mot de passe. 2. Code de double authentification si nécessaire. 3. Choix de l'organisation si plusieurs. |
| Cas particuliers | 5 erreurs de mot de passe de suite : compte bloqué 15 minutes *(proposition)*. Organisation suspendue : message « Accès suspendu, contactez votre administrateur ». |
| Règles | 3, 14, 15, 16 |
| Événements | `UserLoggedIn`, `LoginFailed`, `UserLocked` |

### UC-022 --- Modifier les rôles ou le périmètre d'un membre

| | |
|---|---|
| Acteur | Administrateur |
| Version | MVP |
| Déroulement | 1. Il ouvre la fiche du membre. 2. Il ajoute ou retire des rôles, change le périmètre. 3. Le changement s'applique immédiatement. |
| Cas particuliers | Retirer le rôle Administrateur au dernier Administrateur : refusé. |
| Règles | 5, 8 |
| Événement | `MemberRolesChanged` |

### UC-023 --- Désactiver un membre

| | |
|---|---|
| Acteur | Administrateur |
| Version | MVP |
| Déroulement | 1. Il désactive l'appartenance. 2. L'accès est coupé immédiatement. |
| Cas particuliers | Dernier Administrateur : refusé. Le membre a des interventions affectées en cours : le logiciel les liste pour qu'elles soient réaffectées. |
| Après | Appartenance `DEACTIVATED`. L'historique est conservé. |
| Règles | 5, 10 |
| Événement | `MemberDeactivated` |

### UC-024 --- Suspendre ou réactiver une organisation

| | |
|---|---|
| Acteur | Opérateur de la plateforme |
| Version | MVP |
| Déroulement | 1. L'opérateur suspend l'organisation en indiquant un motif (exemple : abonnement impayé). 2. Plus personne de l'organisation ne peut se connecter. 3. Il peut la réactiver plus tard. |
| Après | Statut `SUSPENDED`. Aucune donnée n'est supprimée. |
| Règles | 11 |
| Événements | `OrganizationSuspended`, `OrganizationReactivated` |

### UC-025 --- Changer d'organisation

| | |
|---|---|
| Acteur | Utilisateur membre de plusieurs organisations |
| Version | MVP |
| Déroulement | 1. Il choisit une autre organisation dans un menu. 2. L'écran se recharge entièrement avec les données de la nouvelle organisation. |
| Règles | 3 |
| Événement | `ActiveOrganizationSwitched` |

### UC-026 --- Mot de passe oublié

| | |
|---|---|
| Acteur | Tout utilisateur |
| Version | MVP |
| Déroulement | 1. Il saisit son email. 2. Il reçoit un lien valable 1 heure *(proposition)*. 3. Il choisit un nouveau mot de passe. |
| Cas particuliers | Email inconnu : le logiciel affiche le même message que pour un email connu, pour ne pas révéler qui a un compte. |
| Événement | `PasswordReset` |

### UC-018 --- Consulter les événements d'audit (partie accès)

| | |
|---|---|
| Acteur | Administrateur |
| Version | MVP |
| Déroulement | Il consulte la liste des connexions, invitations, changements de rôles et désactivations de son organisation, avec filtres par personne et par date. |
| Règles | 18 |

------------------------------------------------------------------------

## 9. Cycles de vie

Les noms des statuts sont en anglais (règle du glossaire), avec leur
traduction pour l'interface.

### 9.1 Organisation

``` text
ACTIVE  ⇄  SUSPENDED
   │            │
   └──→ CLOSED ←┘
```

| Statut | Interface | Signification |
|---|---|---|
| `ACTIVE` | Active | Fonctionne normalement. |
| `SUSPENDED` | Suspendue | Personne ne peut se connecter. Données conservées. |
| `CLOSED` | Fermée | Le client est parti. Données conservées puis exportées et supprimées selon le Cahier des Charges 12 (V1). |

Seul l'opérateur change le statut d'une organisation.

### 9.2 Appartenance

``` text
ACTIVE  →  DEACTIVATED  →  ACTIVE (réintégration)
```

| Statut | Interface | Qui le change |
|---|---|---|
| `ACTIVE` | Actif | Automatique à l'acceptation de l'invitation |
| `DEACTIVATED` | Désactivé | Administrateur |

### 9.3 Invitation

``` text
PENDING  →  ACCEPTED
   ├────→  EXPIRED   (après 7 jours)
   └────→  REVOKED   (annulée par l'Administrateur)
```

### 9.4 Utilisateur

| Statut | Interface | Signification |
|---|---|---|
| `ACTIVE` | Actif | Peut se connecter. |
| `LOCKED` | Bloqué temporairement | Trop d'erreurs de mot de passe ; se débloque seul après 15 minutes *(proposition)*. |
| `DISABLED` | Désactivé | Désactivé par l'opérateur (exemple : compte compromis). Bloque la personne dans **toutes** ses organisations. |

------------------------------------------------------------------------

## 10. Règles métier

Chaque règle porte un numéro pour pouvoir être citée : « Cahier des
Charges 01, règle 3 ».

**Organisations et séparation**

1.  Toute fiche métier appartient à **une seule organisation**, et ne
    change jamais d'organisation.
2.  Un utilisateur ne voit les données d'une organisation que s'il en
    est membre actif.
3.  À un instant donné, un utilisateur travaille dans **une seule
    organisation**. Aucun écran, rapport, recherche ou export ne mélange
    les données de deux organisations.
4.  Une personne a **un seul compte** sur toute la plateforme,
    identifié par son email, même si elle travaille pour plusieurs
    organisations.
5.  Une organisation a **toujours au moins un Administrateur actif**.
    Le logiciel refuse de retirer ou désactiver le dernier.

**Invitations et rôles**

6.  Seul un Administrateur invite des membres et modifie les rôles.
7.  Une invitation n'est utilisable **qu'une seule fois** et expire
    après 7 jours *(proposition)*.
8.  Un changement de rôle ou de périmètre s'applique **immédiatement**,
    y compris pour une personne déjà connectée.
9.  Une permission ne s'applique que **dans le périmètre** de la
    personne. Exemple : un gestionnaire limité à 150 biens ne voit pas
    les baux des 150 autres.

**Départs et suspensions**

10. Désactiver un membre coupe son accès immédiatement. Rien de ce qu'il
    a fait n'est supprimé : l'historique garde son nom.
11. Une organisation suspendue n'est accessible à aucun de ses membres.
    Ses données sont conservées intactes.
12. L'opérateur de la plateforme **n'a pas accès** aux données métier
    des organisations (biens, locataires, montants). L'accès du support
    viendra en V1, avec l'accord du client et une trace d'audit.

**Technicien**

13. Un technicien voit uniquement ses interventions affectées, avec le
    minimum pour travailler : adresse, unité, description du problème,
    photos, et le nom et le téléphone de la personne à contacter sur
    place *(proposition)*. Il ne voit ni montants, ni baux, ni pièces
    d'identité.

**Sécurité**

14. Le contrôle des droits est fait **par le serveur** à chaque action.
    Cacher un bouton dans l'interface ne suffit jamais.
15. Après 5 erreurs de mot de passe consécutives, le compte est bloqué
    15 minutes *(proposition)*.
16. La double authentification est **obligatoire** pour les rôles
    Administrateur et Finance, et facultative pour les autres
    *(proposition)*.
17. Une session se ferme après 30 minutes sans activité et dure au
    maximum 12 heures *(proposition)*.
18. Chaque connexion, échec de connexion, invitation, changement de
    rôle, désactivation et suspension produit un **événement d'audit**
    qui ne peut être ni modifié ni supprimé.

------------------------------------------------------------------------

## 11. Séparation entre organisations

C'est l'exigence la plus importante du produit. Si elle échoue, une
agence voit les locataires et les montants d'une agence concurrente.

### 11.1 Ce qui doit être garanti

Pour un membre de l'Agence Médina, **rien** de l'Agence Carthage ne doit
être visible ou modifiable, nulle part :

-   écrans et listes ;
-   recherche ;
-   rapports et exports (Excel, PDF) ;
-   documents et photos, y compris par lien direct ;
-   notifications et emails ;
-   traitements automatiques (génération des loyers, relances) ;
-   numéros de dossier : demander la fiche n°45 de l'Agence Carthage
    répond « introuvable », pas « accès refusé », pour ne pas révéler
    qu'elle existe.

### 11.2 Ce qui est partagé

Seuls sont partagés entre organisations :

-   le compte d'une personne (son email, son mot de passe, sa langue) ;
-   les listes de référence identiques pour tous (pays, devises, types
    de biens).

### 11.3 Comment c'est vérifié

Par des tests automatiques obligatoires (voir critères d'acceptation
1 à 4) exécutés à chaque modification du logiciel. La solution
technique est décrite en Phase 2 (Multi-Tenancy & Isolation Design).

------------------------------------------------------------------------

## 12. Préparer la V1 : les accès externes

Rien de cette section n'est construit dans le MVP, mais le MVP ne doit
rien faire qui l'empêche.

| Acteur externe (V1) | Ce qu'il verra | Périmètre |
|---|---|---|
| Locataire | Son bail, ses factures, ses paiements et reçus, ses demandes d'intervention | Ses propres fiches |
| Propriétaire | Ses biens, ses relevés, ses documents, l'avancement de ses ventes | Ses propres biens |
| Utilisateur fournisseur | Ses interventions affectées | Interventions affectées à son entreprise |
| Acheteur | Ses offres, sa réservation, ses paiements | Sa propre transaction |

Principes déjà fixés :

-   ils utiliseront le **même mécanisme** (utilisateur, appartenance,
    rôle, périmètre) que le personnel ;
-   leur périmètre sera toujours « leurs propres fiches » ;
-   une personne pourra être à la fois employée d'une agence et
    propriétaire chez une autre, avec un seul compte.

------------------------------------------------------------------------

## 13. Événements, notifications et audit

| Événement | Notification envoyée | Audit |
|---|---|---|
| `OrganizationCreated` | Invitation au premier Administrateur | Oui |
| `MemberInvited` | Email d'invitation | Oui |
| `InvitationAccepted` | Email à l'Administrateur qui a invité | Oui |
| `MemberRolesChanged` | Email au membre concerné | Oui |
| `MemberDeactivated` | --- | Oui |
| `OrganizationSuspended` | Email aux Administrateurs de l'organisation | Oui |
| `OrganizationReactivated` | Email aux Administrateurs de l'organisation | Oui |
| `UserLoggedIn` | --- | Oui |
| `LoginFailed` | --- | Oui |
| `UserLocked` | Email à l'utilisateur | Oui |
| `PasswordReset` | Email de confirmation à l'utilisateur | Oui |
| `ActiveOrganizationSwitched` | --- | Oui |

Chaque événement d'audit enregistre : qui, quoi, sur quelle fiche,
quand, depuis quelle organisation, et l'adresse réseau de connexion.

------------------------------------------------------------------------

## 14. Liens avec les autres cahiers

| Cahier | Lien |
|---|---|
| Tous | Chaque cahier complète le tableau des droits (section 6.2) pour ses propres actions. |
| Cahier des Charges 02 --- Biens & Propriétés | Le périmètre « portefeuille » s'appuie sur la liste des biens. |
| Cahier des Charges 08 --- Maintenance | Définit ce qu'est une intervention « affectée » au technicien. |
| Cahier des Charges 10 --- Audit | Définit l'écran de consultation et la conservation des événements d'audit. |
| Cahier des Charges 12 --- Administration Plateforme | Écrans de l'opérateur, abonnements, fermeture d'une organisation. |
| Cahier des Charges 14 --- Localisation | Langue par utilisateur et par organisation. |

------------------------------------------------------------------------

## 15. Exigences non fonctionnelles propres à ce cahier

-   Interface de connexion disponible en français, arabe et anglais.
-   Une désactivation ou un changement de rôle prend effet en moins de
    1 minute sur toutes les sessions ouvertes *(proposition)*.
-   Les mots de passe sont stockés de façon irréversible (jamais en
    clair).
-   Mot de passe : 10 caractères minimum *(proposition)* ; mots de
    passe trop courants refusés.

------------------------------------------------------------------------

## 16. Critères d'acceptation

Format : **Étant donné** (situation) · **Quand** (action) · **Alors**
(résultat attendu).

**Séparation entre organisations**

-   **Critère 1** --- Étant donné Karim, membre de l'Agence Médina
    seulement · Quand il demande la fiche d'un bail de l'Agence
    Carthage (par son numéro ou par un lien direct) · Alors il obtient
    « introuvable ».
-   **Critère 2** --- Étant donné Karim · Quand il fait une recherche
    « Ben Ali » · Alors seuls les résultats de l'Agence Médina
    apparaissent, même si l'Agence Carthage a un locataire de ce nom.
-   **Critère 3** --- Étant donné un lien de téléchargement d'un document
    de l'Agence Carthage · Quand Karim l'ouvre · Alors le document
    n'est pas téléchargé.
-   **Critère 4** --- Étant donné la génération automatique des loyers du
    mois · Quand elle s'exécute · Alors chaque facture est créée dans
    l'organisation de son bail, et aucune notification n'est envoyée à
    une autre organisation.

**Rôles et périmètres**

-   **Critère 5** --- Étant donné Ali, technicien · Quand il ouvre la liste
    des interventions · Alors il ne voit que les siennes, et aucun
    montant.
-   **Critère 6** --- Étant donné Amira, commerciale · Quand elle essaie
    d'ouvrir la liste des paiements de loyers (même par lien direct) ·
    Alors l'accès est refusé.
-   **Critère 7** --- Étant donné Karim, Gestionnaire **et** Commercial ·
    Quand il se connecte · Alors il voit les menus Location et Vente.

**Comptes et invitations**

-   **Critère 8** --- Étant donné une invitation envoyée il y a 8 jours ·
    Quand la personne clique sur le lien · Alors un message indique que
    le lien a expiré.
-   **Critère 9** --- Étant donné Sonia, seule Administratrice · Quand elle
    essaie de retirer son propre rôle Administrateur · Alors le
    logiciel refuse et explique pourquoi.
-   **Critère 10** --- Étant donné Ali connecté sur son téléphone · Quand
    Sonia désactive son appartenance · Alors Ali perd l'accès en moins
    de 1 minute.
-   **Critère 11** --- Étant donné un comptable membre de l'Agence Médina et
    de l'Agence Carthage · Quand il passe de l'une à l'autre · Alors
    aucune donnée de la première ne reste affichée.
-   **Critère 12** --- Étant donné l'Agence Médina suspendue · Quand Karim
    essaie de se connecter · Alors l'accès est refusé avec le message
    prévu.

**Audit**

-   **Critère 13** --- Étant donné Sonia qui change le rôle de Karim · Quand
    elle consulte le journal d'audit · Alors elle voit qui a fait le
    changement, quand, l'ancien et le nouveau rôle.

------------------------------------------------------------------------

## 17. Questions ouvertes

| N° | Question | Pourquoi c'est important |
|---|---|---|
| Question 1 | Le périmètre « portefeuille de biens » est-il nécessaire dès le MVP, ou toutes les agences visées sont-elles assez petites pour que chacun voie tout ? | Simplifie beaucoup le MVP si on peut l'enlever. À vérifier lors des entretiens clients. |
| Question 2 | Le Gestionnaire doit-il pouvoir enregistrer un paiement (exemple : un locataire paie en espèces à l'agence) ou seulement le voir ? | Dans les petites agences, le gestionnaire encaisse souvent lui-même. |
| Question 3 | Le technicien doit-il voir le nom et le téléphone de l'occupant ? | Nécessaire pour accéder au logement, mais c'est une donnée personnelle. |
| Question 4 | Le matricule fiscal de l'organisation est-il obligatoire ? | Dépend des obligations de facturation (Regulatory & Legal Register). |
| Question 5 | Faut-il un rôle « lecture seule » (exemple : un associé qui consulte sans modifier) ? | Demande fréquente, facile à ajouter si confirmée. |

------------------------------------------------------------------------

## 18. Propositions à valider

| N° | Proposition | Section |
|---|---|---|
| 1 | L'Administrateur a tous les droits de l'organisation. | 6.1 |
| 2 | Périmètre « portefeuille de biens » dans le MVP. | 6.3 |
| 3 | Double authentification obligatoire pour Administrateur et Finance. | Règle 16 |
| 4 | Blocage 15 minutes après 5 erreurs de mot de passe. | Règle 15 |
| 5 | Session : 30 minutes d'inactivité, 12 heures maximum. | Règle 17 |
| 6 | Invitation valable 7 jours ; lien de mot de passe oublié valable 1 heure. | Règle 7, UC-026 |
| 7 | Mot de passe de 10 caractères minimum. | Section 15 |

------------------------------------------------------------------------

## 19. Historique

| Version | Date | Changement |
|---|---|---|
| 0.1 | 2026-10-08 | Première version. |
| 0.2 | 2026-10-08 | Section 6.4 : une interface dédiée à chaque employé ; exemple précisé (chaque agence est une organisation avec ses propres employés). |
