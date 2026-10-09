# Cahier des Charges 01 --- Organisation & Accès

**Projet :** Cloud-Native Multi-Tenant Real Estate Operations ERP\
**Document :** Cahier des Charges 01 --- Organisation & Accès\
**Phase :** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version :** 0.5\
**Statut :** Brouillon --- les points marqués *(proposition)* sont à
valider\
**Date :** 2026-10-09\
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

> **Médina Immobilier** (exemple fictif) est une société immobilière. Elle
> possède 300 logements et bureaux à Tunis : elle en loue la plupart et
> en vend certains. Elle s'abonne au logiciel. Dans
> le logiciel, Médina Immobilier est une **Organisation**.
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
> Avant d'inviter, Sonia décrit l'organisation de sa société : un
> département Location, un département Vente, etc. Elle ajuste les
> rôles avec des cases à cocher (Voir, Créer, Modifier, Supprimer).
>
> -   Karim voit tous les biens, locataires et baux, mais ne peut pas
>     gérer les utilisateurs.
> -   Amira voit les biens à vendre, les prospects et les ventes, mais
>     pas les loyers des locataires.
> -   Hédi voit et gère toute la finance.
> -   Ali ne voit que les interventions qui lui sont affectées, avec
>     l'adresse et l'unité concernées. Il ne voit aucun montant.
>
> Une autre société immobilière, **Carthage Immobilier**, utilise le même logiciel :
> c'est une deuxième **Organisation**, avec ses propres employés et
> leurs propres interfaces.
> Personne à Médina Immobilier ne peut voir quoi que ce soit de Carthage
> Immobilier, et inversement --- même en tapant une adresse web ou un
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
| Cinq rôles modèles : Administrateur, Gestionnaire, Commercial, Finance, Technicien interne | MVP |
| Rôles créés et modifiés par l'Administrateur (grille de privilèges à cocher) | MVP (D-011) |
| Départements et hiérarchie définis par l'Administrateur | MVP (D-011) |
| Plusieurs rôles pour une même personne | MVP |
| Périmètre par département | MVP (D-011) |
| Un compte par organisation : rien n'est partagé entre organisations | MVP |
| Double authentification (code en plus du mot de passe) | MVP *(proposition)* |
| Journal d'audit des accès | MVP |
| Comptes pour locataires, propriétaires, fournisseurs, acheteurs | V1 (D-010) |
| Accès du support avec accord du client | V1 |
| Connexion avec le compte d'entreprise du client (authentification unique) | Plus tard |

------------------------------------------------------------------------

## 3. Les notions de base

Les termes ci-dessous suivent le [Glossaire](../../reference/01-glossary.md).
Une image aide : le logiciel est un **immeuble de bureaux** partagé par
plusieurs entreprises.

| Notion | Définition simple | Dans l'image de l'immeuble |
|---|---|---|
| **Organisation** | Une entreprise cliente qui utilise le logiciel (société immobilière qui possède des biens, promoteur compris). | Une entreprise locataire d'un étage. |
| **Utilisateur** | Une personne qui peut se connecter. Un compte appartient à une seule organisation et est identifié par son email. | Une personne avec une carte d'accès. |
| **Appartenance** | Le lien entre un utilisateur et une organisation. C'est elle qui porte les rôles. | La carte d'accès donne accès à un étage précis. |
| **Département** | Une équipe de l'organisation (Location, Vente, Tunis Nord…), définie par l'Administrateur. Les départements forment un arbre. | Un service de l'entreprise, avec ses bureaux. |
| **Rôle** | Un ensemble de privilèges défini par l'Administrateur (cinq modèles fournis). | Le type de carte : employé, comptable, technicien. |
| **Privilège** | Voir, Créer, Modifier ou Supprimer sur un type de fiche. Aucune case cochée = aucun accès. | Le droit d'ouvrir une porte précise. |
| **Périmètre** | La partie des données sur laquelle un privilège s'applique (toute l'organisation, son département, les interventions affectées). | Les pièces de l'étage où la carte fonctionne. |
| **Opérateur de la plateforme** | La société qui exploite le logiciel (vous). | Le gestionnaire de l'immeuble : il ouvre et ferme les étages, mais n'entre pas dans les bureaux. |

**Règle d'or :** une personne ne fait une action que si elle a une
appartenance active à l'organisation, un rôle qui contient le
privilège, **et** que la fiche concernée est dans son périmètre
(son département, ou toute l'organisation).

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
| Nom commercial | Médina Immobilier | Oui |
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
| Email | karim@medina-immobilier.tn | Oui, unique sur toute la plateforme |
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
| Organisation | Médina Immobilier |
| Rôles | Gestionnaire (une personne peut en avoir plusieurs) |
| Département | Location --- Tunis Nord |
| Périmètre | Son département, ou toute l'organisation |
| Statut | `ACTIVE` |
| Date d'entrée, date de sortie | |
| Invité par | Sonia (Administrateur) |

### 5.4 Invitation

| Information | Exemple |
|---|---|
| Email invité | karim@medina-immobilier.tn |
| Organisation | Médina Immobilier |
| Rôles proposés | Gestionnaire |
| Périmètre proposé | Toute l'organisation |
| Envoyée par, date d'envoi | Sonia, 2026-10-08 |
| Date d'expiration | 7 jours après l'envoi *(proposition)* |
| Statut | `PENDING` |

### 5.5 Département

| Information | Exemple |
|---|---|
| Nom | Tunis Nord |
| Département parent | Location |
| Responsable | Karim Ben Salah (facultatif) |
| Membres | Karim, … |
| Biens rattachés | 150 biens |
| Statut | `ACTIVE` ou `ARCHIVED` |

### 5.6 Rôle

| Information | Exemple |
|---|---|
| Nom | Assistante location |
| Description | Saisit les locataires et les demandes d'intervention |
| Créé à partir du modèle | Gestionnaire (facultatif) |
| Grille de privilèges | Voir / Créer / Modifier / Supprimer par domaine |
| Périmètre | Son département, ou toute l'organisation |
| Modifiable | Oui, sauf le rôle Administrateur |

### 5.7 Combien de quoi ? (cardinalités)

-   Une organisation a **un ou plusieurs** membres, dont au moins un
    Administrateur actif.
-   Un utilisateur a **une seule** appartenance : son compte appartient à une seule organisation. Une personne qui travaille pour deux sociétés immobilières a deux comptes séparés.
-   Une appartenance a **un ou plusieurs** rôles.
-   Une organisation a **zéro ou plusieurs** départements ; un
    département a **zéro ou plusieurs** sous-départements.
-   Un membre appartient à **un** département *(proposition)*.
-   Un bien est rattaché à **un** département.
-   Une fiche métier (bien, bail, paiement…) appartient à **une seule**
    organisation, pour toujours.

------------------------------------------------------------------------

## 6. Les rôles et ce qu'ils permettent

### 6.1 Principe

Chaque société immobilière est organisée différemment : une petite
société a trois
personnes polyvalentes, une grande a des départements et des chefs
d'équipe. Le logiciel ne peut donc pas imposer une seule organisation
interne (décision D-011).

-   L'**Administrateur** de chaque organisation **définit lui-même** ses
    départements (section 6.4) et ses rôles (sections 6.2 et 6.3).
-   Le logiciel fournit **cinq rôles modèles** prêts à l'emploi :
    Administrateur, Gestionnaire, Commercial, Finance, Technicien
    interne. L'Administrateur peut les garder tels quels, les modifier
    ou créer de nouveaux rôles (exemple : « Assistante », « Associé en
    lecture seule »).
-   Un rôle est une **grille de privilèges à cocher** (section 6.2).
-   Les privilèges s'**additionnent** : une personne avec deux rôles a
    les privilèges des deux.
-   Il n'existe pas de « droit d'interdire » : on ne retire pas un
    privilège, on choisit les bons rôles.
-   Le rôle **Administrateur** a toujours tous les privilèges et ne peut
    pas être modifié, pour qu'une organisation ne puisse jamais se
    bloquer elle-même.

### 6.2 Les privilèges : une grille à cocher

Pour chaque **domaine** (type de fiche), l'Administrateur coche les
privilèges du rôle :

| Privilège | Signification | Exemple |
|---|---|---|
| **Voir** | Consulter les fiches | Voir la liste des baux |
| **Créer** | Ajouter une fiche | Créer un bail |
| **Modifier** | Changer une fiche existante | Changer la date de fin d'un bail |
| **Supprimer** | Retirer une fiche | Supprimer un prospect enregistré en double |
| *Aucune case cochée* | **Aucun accès** : le domaine et son menu sont invisibles | Un technicien sans case sur « Finance » ne voit aucun revenu de l'organisation |

Exemple d'écran pour le rôle Technicien interne :

``` text
Rôle : Technicien interne
Domaine                         Voir   Créer   Modifier   Supprimer
Biens                            [x]    [ ]      [ ]        [ ]
Locataires et baux               [ ]    [ ]      [ ]        [ ]
Factures, paiements, reçus       [ ]    [ ]      [ ]        [ ]    ← aucun accès
Rapports financiers              [ ]    [ ]      [ ]        [ ]    ← aucun accès
Ordres de travail                [x]    [ ]      [x]        [ ]
Périmètre : interventions qui lui sont affectées
```

Règles de la grille :

-   Cocher Créer, Modifier ou Supprimer coche automatiquement **Voir** :
    on ne peut pas modifier ce qu'on ne voit pas *(proposition)*.
-   **Finance :** « Supprimer » sur une fiche financière déjà émise
    (facture, paiement, reçu) signifie **annuler par une écriture de
    correction** (avoir, contre-passation). Elle n'est jamais effacée
    (Phase 0 §0.18).
-   Les documents suivent les privilèges de la fiche à laquelle ils sont
    attachés.

Domaines de la grille (première liste, complétée par chaque cahier) :
Paramètres de l'organisation · Membres, départements et rôles · Journal
d'audit · Biens · Propriétaires · Locataires et baux · Factures,
paiements, reçus · Dépenses · Relevés propriétaires · Demandes
d'intervention · Ordres de travail · Fournisseurs · Mandats de vente,
annonces, offres, réservations · Prospects et visites · Paiements des
acheteurs · Rapports de location et de maintenance · Rapports de vente ·
Rapports financiers.

### 6.3 Les cinq rôles modèles

Point de départ proposé à chaque nouvelle organisation. L'Administrateur
peut tout ajuster, sauf le rôle Administrateur.

Légende : **Gérer** = Voir + Créer + Modifier + Supprimer · **Voir** = consulter
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

### 6.4 Départements

L'Administrateur décrit l'organisation interne de sa société sous forme
d'**arbre de départements**, avec autant de niveaux qu'il veut.

``` text
Médina Immobilier
├── Direction            (responsable : Sonia)
├── Location
│   ├── Tunis Nord       (responsable : Karim)
│   └── Tunis Sud
├── Vente                (responsable : Amira)
├── Finance              (responsable : Hédi)
└── Maintenance          (Ali)
```

-   Chaque département a un nom, un département parent (sauf le premier
    niveau), un responsable (facultatif) et des membres.
-   Chaque membre appartient à **un département** *(proposition)*.
-   Les **biens** sont rattachés à un département. Les fiches qui en
    dépendent (baux, interventions, ventes) suivent le département de
    leur bien.
-   **Visibilité :**
    -   un membre voit les fiches de **son département** ;
    -   le **responsable** d'un département voit aussi celles de **tous
        ses sous-départements** ;
    -   un rôle avec le périmètre « toute l'organisation » voit tout.

**Ce qu'une personne peut faire = ses privilèges (rôle) appliqués aux
fiches de son département (périmètre).** Exemple : Karim a « Modifier »
sur les baux et appartient à Tunis Nord. Il modifie les baux de Tunis
Nord, pas ceux de Tunis Sud. Sonia, responsable de la Direction avec le
périmètre « toute l'organisation », voit tout.

Les cas particuliers (fiche sans département, bien partagé entre Location
et Vente, changement de département) sont listés en questions ouvertes
(section 17) et seront détaillés plus tard.

### 6.5 Périmètre

| Périmètre | Qui | Exemple |
|---|---|---|
| Toute l'organisation | Choisi par l'Administrateur dans le rôle (par défaut : Administrateur, Finance) | Sonia voit les 300 biens |
| Son département (et ses sous-départements pour le responsable) | Tout rôle (section 6.4) | Karim voit les 150 biens de Tunis Nord |
| Les interventions affectées | Technicien | Ali voit 4 interventions cette semaine |

### 6.6 Une interface dédiée à chaque employé

Le logiciel a trois niveaux :

``` text
Plateforme (le logiciel)
└── Organisations (les entreprises clientes : Médina Immobilier, Carthage Immobilier…)
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
   └── il crée son mot de passe : le compte n'existe que dans cette organisation
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
L'organisation du compte s'ouvre directement
        ↓
Toutes les données affichées appartiennent à l'organisation du compte
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
| Cas particuliers | L'email appartient déjà à un compte d'une autre organisation : le logiciel le refuse ; la personne doit utiliser un autre email (question 10). |
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

### UC-025 --- Changer d'organisation (supprimé)

Supprimé le 2026-10-09 : un compte appartient à une seule organisation, il n'y a donc rien à changer. Le numéro UC-025 n'est pas réutilisé.

### UC-026 --- Mot de passe oublié

| | |
|---|---|
| Acteur | Tout utilisateur |
| Version | MVP |
| Déroulement | 1. Il saisit son email. 2. Il reçoit un lien valable 1 heure *(proposition)*. 3. Il choisit un nouveau mot de passe. |
| Cas particuliers | Email inconnu : le logiciel affiche le même message que pour un email connu, pour ne pas révéler qui a un compte. |
| Événement | `PasswordReset` |

### UC-027 --- Créer ou modifier un département

| | |
|---|---|
| Acteur | Administrateur (ou rôle ayant le privilège) |
| Version | MVP |
| Déroulement | 1. Il crée un département : nom, département parent, responsable. 2. Il y rattache des membres et des biens. |
| Cas particuliers | Supprimer un département qui a des membres ou des biens : refusé, l'archivage est proposé. |
| Règles | 19, 23, 24, 25 |
| Événements | `DepartmentCreated`, `DepartmentUpdated` |

### UC-028 --- Créer ou modifier un rôle

| | |
|---|---|
| Acteur | Administrateur (ou rôle ayant le privilège) |
| Version | MVP |
| Déroulement | 1. Il crée un rôle, vide ou à partir d'un modèle. 2. Il coche les privilèges domaine par domaine. 3. Il choisit le périmètre. 4. Il l'attribue à des membres. |
| Cas particuliers | Modifier le rôle Administrateur : refusé. Supprimer un rôle encore attribué : refusé. |
| Règles | 20, 21, 22, 25 |
| Événements | `RoleCreated`, `RoleUpdated`, `RoleDeleted` |

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
4.  Un compte appartient à **une seule organisation** et est identifié par son email, unique sur toute la plateforme *(proposition, voir question 10)*. Une personne qui travaille pour deux sociétés a deux comptes séparés, avec deux emails.
5.  Une organisation a **toujours au moins un Administrateur actif**.
    Le logiciel refuse de retirer ou désactiver le dernier.

**Invitations et rôles**

6.  Seul un rôle ayant les privilèges sur « Membres, départements et
    rôles » invite des membres, crée les départements et modifie les
    rôles. Par défaut, c'est l'Administrateur.
7.  Une invitation n'est utilisable **qu'une seule fois** et expire
    après 7 jours *(proposition)*.
8.  Un changement de rôle ou de périmètre s'applique **immédiatement**,
    y compris pour une personne déjà connectée.
9.  Un privilège ne s'applique que **dans le périmètre** de la
    personne. Exemple : un gestionnaire du département Tunis Nord ne
    voit pas les baux de Tunis Sud.

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

**Départements et rôles (D-011)**

19. Chaque organisation définit ses propres départements et rôles. Les
    départements et rôles d'une organisation n'existent pas pour les
    autres.
20. Un rôle est une grille de privilèges : **Voir, Créer, Modifier,
    Supprimer** par domaine. Aucune case cochée = aucun accès.
21. Le rôle Administrateur garde toujours tous les privilèges et ne peut
    être ni modifié ni supprimé.
22. Un rôle encore attribué à des membres ne peut pas être supprimé.
23. Un département qui a encore des membres ou des biens ne peut pas
    être supprimé ; il peut être archivé.
24. Le responsable d'un département voit les fiches de tous ses
    sous-départements, dans la limite des privilèges de ses rôles.
25. Modifier un rôle ou un département s'applique **immédiatement** à
    tous les membres concernés (comme la règle 8).

------------------------------------------------------------------------

## 11. Séparation entre organisations

C'est l'exigence la plus importante du produit. Si elle échoue, une
société voit les locataires et les montants d'une société concurrente.

### 11.1 Ce qui doit être garanti

Pour un membre de Médina Immobilier, **rien** de Carthage Immobilier ne doit
être visible ou modifiable, nulle part :

-   écrans et listes ;
-   recherche ;
-   rapports et exports (Excel, PDF) ;
-   documents et photos, y compris par lien direct ;
-   notifications et emails ;
-   traitements automatiques (génération des loyers, relances) ;
-   numéros de dossier : demander la fiche n°45 de Carthage Immobilier
    répond « introuvable », pas « accès refusé », pour ne pas révéler
    qu'elle existe.

### 11.2 Rien n'est partagé

Rien n'est partagé entre organisations : chaque organisation a ses propres comptes, ses propres fiches et ses propres listes.

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
-   un compte appartient toujours à une seule organisation : une personne employée d'une société immobilière et propriétaire chez une autre a deux comptes séparés.

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
| `DepartmentCreated`, `DepartmentUpdated` | --- | Oui |
| `RoleCreated`, `RoleUpdated`, `RoleDeleted` | --- | Oui |

Chaque événement d'audit enregistre : qui, quoi, sur quelle fiche,
quand, depuis quelle organisation, et l'adresse réseau de connexion.

------------------------------------------------------------------------

## 14. Liens avec les autres cahiers

| Cahier | Lien |
|---|---|
| Tous | Chaque cahier complète la liste des domaines (section 6.2) et les rôles modèles (section 6.3) pour ses propres fiches. |
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

-   **Critère 1** --- Étant donné Karim, membre de Médina Immobilier
    seulement · Quand il demande la fiche d'un bail de Carthage
    Immobilier (par son numéro ou par un lien direct) · Alors il obtient
    « introuvable ».
-   **Critère 2** --- Étant donné Karim · Quand il fait une recherche
    « Ben Ali » · Alors seuls les résultats de Médina Immobilier
    apparaissent, même si Carthage Immobilier a un locataire de ce nom.
-   **Critère 3** --- Étant donné un lien de téléchargement d'un document
    de Carthage Immobilier · Quand Karim l'ouvre · Alors le document
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
-   **Critère 11** --- Étant donné un comptable qui a un compte chez Médina Immobilier et un autre compte chez Carthage Immobilier · Quand il se connecte avec l'un des deux · Alors il ne voit que les données de cette organisation.
-   **Critère 12** --- Étant donné Médina Immobilier suspendue · Quand Karim
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
| Question 1 | ~~Le périmètre « portefeuille de biens » est-il nécessaire dès le MVP ?~~ | **Réglée par D-011 :** le périmètre se fait par département. |
| Question 2 | ~~Le Gestionnaire doit-il pouvoir enregistrer un paiement ?~~ | **Réglée par D-011 :** chaque organisation coche le privilège si elle le souhaite. |
| Question 3 | Le technicien doit-il voir le nom et le téléphone de l'occupant ? | Nécessaire pour accéder au logement, mais c'est une donnée personnelle. |
| Question 4 | Le matricule fiscal de l'organisation est-il obligatoire ? | Dépend des obligations de facturation (Regulatory & Legal Register). |
| Question 5 | ~~Faut-il un rôle « lecture seule » ?~~ | **Réglée par D-011 :** l'Administrateur crée un rôle avec seulement « Voir ». |
| Question 6 | Certaines actions ne sont ni créer, ni modifier, ni supprimer : valider des travaux, imputer un paiement, clôturer une vente, inviter un membre. Faut-il des privilèges supplémentaires (exemple : « Valider ») ? | Sinon, « Modifier » donne trop ou trop peu de droits. |
| Question 7 | Une fiche peut-elle n'appartenir à aucun département ? Si oui, qui la voit ? | Exemple : un fournisseur, un prospect sans bien précis. |
| Question 8 | Un bien géré par Location et mis en vente par Vente : à quel département appartient-il ? | Deux équipes doivent le voir. |
| Question 9 | Un membre peut-il appartenir à plusieurs départements ? | Exemple : un employé qui travaille pour Tunis Nord et Tunis Sud. |
| Question 10 | Comment la connexion trouve-t-elle l'organisation du compte ? Email unique sur toute la plateforme (une personne qui travaille pour deux sociétés utilise deux emails), ou code d'organisation à saisir à la connexion ? | Sans partage entre organisations, un compte n'existe que dans une organisation. |

------------------------------------------------------------------------

## 18. Propositions à valider

| N° | Proposition | Section |
|---|---|---|
| 1 | L'Administrateur a tous les droits de l'organisation. | 6.1 |
| 2 | Cocher Créer, Modifier ou Supprimer coche automatiquement Voir. | 6.2 |
| 3 | Double authentification obligatoire pour Administrateur et Finance. | Règle 16 |
| 4 | Blocage 15 minutes après 5 erreurs de mot de passe. | Règle 15 |
| 5 | Session : 30 minutes d'inactivité, 12 heures maximum. | Règle 17 |
| 6 | Invitation valable 7 jours ; lien de mot de passe oublié valable 1 heure. | Règle 7, UC-026 |
| 7 | Mot de passe de 10 caractères minimum. | Section 15 |
| 8 | Un membre appartient à un seul département. | 6.4 |

------------------------------------------------------------------------

## 19. Historique

| Version | Date | Changement |
|---|---|---|
| 0.1 | 2026-10-08 | Première version. |
| 0.2 | 2026-10-08 | Section 6.4 : une interface dédiée à chaque employé ; exemple précisé (chaque société immobilière est une organisation avec ses propres employés). |
| 0.3 | 2026-10-08 | D-011 : départements et rôles définis par chaque organisation dès le MVP ; grille de privilèges Voir / Créer / Modifier / Supprimer ; sections 6 renumérotées (6.4 Départements, 6.5 Périmètre, 6.6 Interface) ; règles 19 à 25 ; UC-027 et UC-028. |
| 0.4 | 2026-10-09 | D-012 : exemples renommés (Médina Immobilier, Carthage Immobilier) ; le client est une société immobilière qui possède des biens. |
| 0.5 | 2026-10-09 | Rien n'est partagé entre organisations : un compte appartient à une seule organisation (règle 4, section 11.2, UC-025 supprimé, critère 11, question 10). |
