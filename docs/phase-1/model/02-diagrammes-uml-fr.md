# Diagrammes UML (version française)

**Document :** Diagrammes UML --- version française\
**Phase :** Phase 1 --- Cahier des Charges + Domain Modeling\
**Version :** 1.2\
**Statut :** Diagramme de cas d'utilisation et diagramme de classes du MVP\
**Date :** 2026-10-09\
**Version anglaise :** [UML Diagrams](01-uml-diagrams.md)

Ce document est la version française du diagramme de cas d'utilisation
et du diagramme de classes du MVP. Les diagrammes sont écrits en Mermaid
pour que GitHub les affiche.

------------------------------------------------------------------------

## 1. Diagramme de cas d'utilisation

### 1.1 Les acteurs

Les acteurs sont fixes dans le logiciel ; ce ne sont pas des intitulés
de poste. Chaque société nomme et configure ses propres rôles (décision
D-011) : « Gestionnaire », « Commercial »… sont des modèles de rôles,
pas des acteurs.

``` mermaid
flowchart LR
    AD["👤 Administrateur de l'organisation"]
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        X(( ))
    end

    AD --- ERP
    EM --- ERP
```

| Acteur | Définition |
|---|---|
| Administrateur de l'organisation | Le responsable de la société immobilière. Chaque organisation en a un ; ce rôle ne peut pas être modifié. |
| Employé | Tout autre membre de l'organisation. Les cas d'utilisation qu'il peut réaliser dépendent des privilèges de son rôle. |

Il n'y a **pas de généralisation** entre les deux acteurs :
l'administrateur n'est pas un employé et ne fait pas le travail des
employés (par exemple, il ne fait pas le travail d'un technicien).

Les propriétaires, locataires, fournisseurs et acheteurs **ne sont pas
des acteurs** : ils ne se connectent pas dans le MVP. Ce sont des fiches
gérées par le personnel (décision D-010).

### 1.2 Cas d'utilisation de l'administrateur

``` mermaid
flowchart LR
    AD["👤 Administrateur de l'organisation"]
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        A1(["Configurer l'organisation"])
        A2(["Inviter un membre"])
        A3(["Créer ou modifier une succursale"])
        A4(["Créer ou modifier un département"])
        A5(["Créer ou modifier un rôle"])
        A6(["Modifier les rôles d'un membre"])
        A7(["Désactiver un membre"])
        A8(["Consulter le journal d'audit"])
        A9(["Réactiver un membre"])
        A10(["Accepter ou refuser une demande de mutation"])
        A11(["Renvoyer ou annuler une invitation"])
        A12(["Réinitialiser la double authentification d'un membre"])
        A13(["Configurer les informations de la société"])
        A14(["Choisir les activités : location, vente ou les deux"])
        A15(["Rattacher des biens à une succursale"])
        A16(["Paramétrer les rappels"])
        A17(["Personnaliser l'en-tête des documents"])
        A18(["Consulter le tableau de bord de la société"])
        A19(["Consulter les rapports de toutes les succursales et départements"])
        A20(["Approuver les dépenses importantes"])
        A21(["Importer des données depuis Excel"])
        A22(["Exporter les données de la société"])
        A23(["Consulter l'abonnement"])
        R0(["Demander une mutation de département ou de succursale"])
    end

    EM --- R0
    R0 -. "validée par" .-> A10
    AD --- A1
    AD --- A2
    AD --- A3
    AD --- A4
    AD --- A5
    AD --- A6
    AD --- A7
    AD --- A8
    AD --- A9
    AD --- A10
    AD --- A11
    AD --- A12
    AD --- A13
    AD --- A14
    AD --- A15
    AD --- A16
    AD --- A17
    AD --- A18
    AD --- A19
    AD --- A20
    AD --- A21
    AD --- A22
    AD --- A23
```

| N° | Cas d'utilisation | Explication |
|---|---|---|
| 1 | Configurer l'organisation | — |
| 2 | Inviter un membre | — |
| 3 | Créer ou modifier une succursale | Page dédiée où l'administrateur nomme les succursales |
| 4 | Créer ou modifier un département | Page dédiée où l'administrateur nomme les départements |
| 5 | Créer ou modifier un rôle | Grille de privilèges à cocher |
| 6 | Modifier les rôles d'un membre | — |
| 7 | Désactiver un membre | — |
| 8 | Consulter le journal d'audit | — |
| 9 | Réactiver un membre | Un employé parti puis revenu retrouve son historique |
| 10 | Accepter ou refuser une demande de mutation | L'employé demande à changer de département ou de succursale ; l'administrateur décide |
| 11 | Renvoyer ou annuler une invitation | L'email a été perdu ou envoyé à la mauvaise adresse |
| 12 | Réinitialiser la double authentification d'un membre | Un employé a perdu son téléphone |
| 13 | Configurer les informations de la société | Logo, raison sociale, matricule fiscal, langue |
| 14 | Choisir les activités : location, vente ou les deux | Masque les menus inutiles |
| 15 | Rattacher des biens à une succursale | Décide quelle succursale gère quels biens, donc qui les voit |
| 16 | Paramétrer les rappels | Exemple : rappel de loyer 5 jours avant et 3 jours après l'échéance |
| 17 | Personnaliser l'en-tête des documents | Logo et coordonnées sur les reçus, factures, relevés |
| 18 | Consulter le tableau de bord de la société | Les chiffres clés de toute la société sur un écran |
| 19 | Consulter les rapports de toutes les succursales et départements | — |
| 20 | Approuver les dépenses importantes | Au-delà d'un montant fixé par la société |
| 21 | Importer des données depuis Excel | Biens, propriétaires, locataires, baux existants |
| 22 | Exporter les données de la société | Sauvegarde, ou départ de la société |
| 23 | Consulter l'abonnement | Formule en cours et factures de la plateforme |
| — | Demander une mutation de département ou de succursale *(Employé)* | L'employé fait la demande ; l'administrateur l'accepte ou la refuse (cas 10) |

### 1.3 Cas d'utilisation de l'employé : le principe

Chaque société a sa propre hiérarchie, et les hiérarchies réelles sont
très différentes d'une société à l'autre. Le logiciel ne contient donc
**aucune** hiérarchie de société. À la place :

1.  **Nous construisons les boutons.** L'ERP a une liste fixe d'actions
    (« Créer un bail », « Enregistrer un paiement », « Affecter des
    travaux », « Conclure une vente »…). Toutes les sociétés ont les
    mêmes boutons.
2.  **Chaque société décide qui appuie sur quel bouton.**
    L'administrateur coche des cases pour chaque rôle. Exemple : chez
    Médina Immobilier, le « Gestionnaire » peut appuyer sur « Créer un
    bail » ; chez Carthage Immobilier, le « Chargé de location » peut
    appuyer sur le même bouton.
3.  **La hiérarchie décide seulement qui voit quoi.** L'administrateur
    dessine ses succursales et ses départements. Le logiciel vérifie
    deux choses : votre rôle a-t-il ce bouton, et ce bien est-il dans
    votre succursale ou votre département ?

L'**Employé** est relié à **tous les boutons** ; à côté de chaque
bouton, le diagramme indique la case à cocher. Les cases sont : **Voir**,
**Créer**, **Modifier**, **Supprimer** ; aucune case cochée = aucun
accès.

### 1.4 Boutons de l'employé : Biens

Les immeubles, maisons et terrains de la société, et leurs unités (appartements, bureaux, commerces, parkings).

Case utilisée : **Biens**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        P1(["Consulter les biens et les unités<br/><i>case : Biens - Voir</i>"])
        P2(["Ajouter un bien<br/><i>case : Biens - Créer</i>"])
        P3(["Ajouter un bâtiment ou un étage<br/><i>case : Biens - Créer</i>"])
        P4(["Ajouter une unité<br/><i>case : Biens - Créer</i>"])
        P5(["Modifier un bien ou une unité<br/><i>case : Biens - Modifier</i>"])
        P6(["Changer le statut d'une unité<br/><i>case : Biens - Modifier</i>"])
        P7(["Joindre des photos ou des documents<br/><i>case : Biens - Modifier</i>"])
        P8(["Supprimer un bien ou une unité<br/><i>case : Biens - Supprimer</i>"])
        P9(["Archiver un bien<br/><i>case : Biens - Supprimer</i>"])
        P10(["Rechercher et filtrer les biens<br/><i>case : Biens - Voir</i>"])
        P11(["Consulter l'historique d'un bien<br/><i>case : Biens - Voir</i>"])
        P12(["Ajouter plusieurs unités en une fois<br/><i>case : Biens - Créer</i>"])
        P13(["Copier une unité<br/><i>case : Biens - Créer</i>"])
        P14(["Voir les biens sur une carte<br/><i>case : Biens - Voir</i>"])
        P15(["Imprimer la fiche d'un bien<br/><i>case : Biens - Voir</i>"])
        P16(["Exporter les biens vers Excel<br/><i>case : Biens - Voir</i>"])
        P17(["Enregistrer les parties communes d'un immeuble<br/><i>case : Biens - Créer</i>"])
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

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les biens et les unités | — | Biens : Voir |
| 2 | Ajouter un bien | Immeuble, maison, commerce… | Biens : Créer |
| 3 | Ajouter un bâtiment ou un étage | Seulement quand c'est utile | Biens : Créer |
| 4 | Ajouter une unité | Appartement, bureau, parking… | Biens : Créer |
| 5 | Modifier un bien ou une unité | — | Biens : Modifier |
| 6 | Changer le statut d'une unité | Libre, louée, en vente, en travaux | Biens : Modifier |
| 7 | Joindre des photos ou des documents | — | Biens : Modifier |
| 8 | Supprimer un bien ou une unité | Seulement sans bail ni paiement (voir la règle) | Biens : Supprimer |
| 9 | Archiver un bien | Le retirer des listes en gardant son historique | Biens : Supprimer |
| 10 | Rechercher et filtrer les biens | Par ville, type, statut, succursale | Biens : Voir |
| 11 | Consulter l'historique d'un bien | Modifications, anciens locataires, anciennes réparations | Biens : Voir |
| 12 | Ajouter plusieurs unités en une fois | Exemple : 5 étages × 4 appartements | Biens : Créer |
| 13 | Copier une unité | Nouvelle unité pré-remplie avec la description d'une autre (jamais les locataires, baux, paiements ni l'historique) | Biens : Créer |
| 14 | Voir les biens sur une carte | — | Biens : Voir |
| 15 | Imprimer la fiche d'un bien | Détails et photos sur une page | Biens : Voir |
| 16 | Exporter les biens vers Excel | — | Biens : Voir |
| 17 | Enregistrer les parties communes d'un immeuble | Escaliers, ascenseur, entrée, parking *(syndic)* | Biens : Créer |

**Supprimer ou archiver :** « Supprimer » ne fonctionne que si le bien ou
l'unité n'a **aucun bail et aucun paiement** (par exemple une fiche créée
par erreur) ; tout ce qu'il contient est alors supprimé aussi (unités,
photos, documents). Sinon, on **archive** : le bien disparaît des listes
mais son historique est conservé. Les données financières ne sont jamais
effacées.

### 1.5 Boutons de l'employé : Propriétaires

Les personnes et sociétés qui ont acheté des biens à la société, ou dont la société gère ou vend les biens ; leurs quotes-parts et leurs accords avec la société.

Case utilisée : **Propriétaires**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        O1(["Consulter les propriétaires<br/><i>case : Propriétaires - Voir</i>"])
        O2(["Ajouter un propriétaire<br/><i>case : Propriétaires - Créer</i>"])
        O3(["Modifier un propriétaire<br/><i>case : Propriétaires - Modifier</i>"])
        O4(["Lier un propriétaire à un bien ou une unité, avec sa quote-part<br/><i>case : Propriétaires - Modifier</i>"])
        O5(["Enregistrer un changement de propriétaire avec sa date<br/><i>case : Propriétaires - Modifier</i>"])
        O6(["Ajouter un mandat de gestion<br/><i>case : Propriétaires - Créer</i>"])
        O7(["Joindre des documents<br/><i>case : Propriétaires - Modifier</i>"])
        O8(["Consulter les biens d'un propriétaire<br/><i>case : Propriétaires - Voir</i>"])
        O9(["Supprimer ou archiver un propriétaire<br/><i>case : Propriétaires - Supprimer</i>"])
        O10(["Rechercher et filtrer les propriétaires<br/><i>case : Propriétaires - Voir</i>"])
        O11(["Consulter l'historique d'un propriétaire<br/><i>case : Propriétaires - Voir</i>"])
        O12(["Renouveler ou terminer un mandat de gestion<br/><i>case : Propriétaires - Modifier</i>"])
        O13(["Voir les mandats qui arrivent à échéance<br/><i>case : Propriétaires - Voir</i>"])
        O14(["Ajouter une note<br/><i>case : Propriétaires - Modifier</i>"])
        O15(["Envoyer un email à un propriétaire<br/><i>case : Propriétaires - Voir</i>"])
        O16(["Fusionner deux propriétaires<br/><i>case : Propriétaires - Modifier</i>"])
        O17(["Imprimer la fiche d'un propriétaire<br/><i>case : Propriétaires - Voir</i>"])
        O18(["Exporter les propriétaires vers Excel<br/><i>case : Propriétaires - Voir</i>"])
        O19(["Consulter le résumé d'un propriétaire<br/><i>case : Propriétaires - Voir</i>"])
        O20(["Gérer un groupe de copropriétaires<br/><i>case : Propriétaires - Modifier</i>"])
        O21(["Voir qui possédait une unité à une date donnée<br/><i>case : Propriétaires - Voir</i>"])
        O22(["Fixer le plafond de réparation d'un propriétaire<br/><i>case : Propriétaires - Modifier</i>"])
        O23(["Enregistrer l'accord d'un propriétaire<br/><i>case : Propriétaires - Modifier</i>"])
        O24(["Fixer les honoraires de gestion par bien<br/><i>case : Propriétaires - Modifier</i>"])
        O25(["Transférer tous les biens d'un propriétaire en une fois<br/><i>case : Propriétaires - Modifier</i>"])
        O26(["Lister les propriétaires aux informations incomplètes<br/><i>case : Propriétaires - Voir</i>"])
        O27(["Envoyer un message à plusieurs propriétaires<br/><i>case : Propriétaires - Voir</i>"])
        O28(["Enregistrer la quote-part de chaque propriétaire dans les charges de l'immeuble<br/><i>case : Propriétaires - Modifier</i>"])
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

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les propriétaires | — | Propriétaires : Voir |
| 2 | Ajouter un propriétaire | Personne ou société | Propriétaires : Créer |
| 3 | Modifier un propriétaire | Coordonnées, compte bancaire | Propriétaires : Modifier |
| 4 | Lier un propriétaire à un bien ou une unité, avec sa quote-part | Exemple : 50 % | Propriétaires : Modifier |
| 5 | Enregistrer un changement de propriétaire avec sa date | Vente, héritage | Propriétaires : Modifier |
| 6 | Ajouter un mandat de gestion | Ce que la société gère pour le propriétaire, dates, honoraires | Propriétaires : Créer |
| 7 | Joindre des documents | Pièce d'identité, titre de propriété, mandat signé | Propriétaires : Modifier |
| 8 | Consulter les biens d'un propriétaire | — | Propriétaires : Voir |
| 9 | Supprimer ou archiver un propriétaire | Même règle que les biens | Propriétaires : Supprimer |
| 10 | Rechercher et filtrer les propriétaires | — | Propriétaires : Voir |
| 11 | Consulter l'historique d'un propriétaire | Anciens biens, anciens mandats, modifications | Propriétaires : Voir |
| 12 | Renouveler ou terminer un mandat de gestion | — | Propriétaires : Modifier |
| 13 | Voir les mandats qui arrivent à échéance | Pour les renouveler à temps | Propriétaires : Voir |
| 14 | Ajouter une note | Appel, rendez-vous, demande du propriétaire | Propriétaires : Modifier |
| 15 | Envoyer un email à un propriétaire | Une copie est conservée | Propriétaires : Voir |
| 16 | Fusionner deux propriétaires | La même personne saisie deux fois devient une seule fiche | Propriétaires : Modifier |
| 17 | Imprimer la fiche d'un propriétaire | — | Propriétaires : Voir |
| 18 | Exporter les propriétaires vers Excel | — | Propriétaires : Voir |
| 19 | Consulter le résumé d'un propriétaire | Ses biens, unités louées ou vides, travaux en cours, sommes dues (les montants demandent aussi Factures et paiements : Voir) | Propriétaires : Voir |
| 20 | Gérer un groupe de copropriétaires | Exemple : 3 frères héritiers ; les quotes-parts font 100 % et **chacun reçoit sa propre copie** de chaque courrier | Propriétaires : Modifier |
| 21 | Voir qui possédait une unité à une date donnée | Exemple : vendue le 15 mars, le loyer de mars est partagé | Propriétaires : Voir |
| 22 | Fixer le plafond de réparation d'un propriétaire | Exemple : au-delà de 500 TND, son accord est nécessaire | Propriétaires : Modifier |
| 23 | Enregistrer l'accord d'un propriétaire | Accord donné par téléphone ou en personne, avec la date, comme preuve | Propriétaires : Modifier |
| 24 | Fixer les honoraires de gestion par bien | Exemple : 8 % sur les appartements, 10 % sur un commerce | Propriétaires : Modifier |
| 25 | Transférer tous les biens d'un propriétaire en une fois | Exemple : des héritiers reprennent 6 appartements | Propriétaires : Modifier |
| 26 | Lister les propriétaires aux informations incomplètes | Sans compte bancaire, pièce d'identité ou mandat signé | Propriétaires : Voir |
| 27 | Envoyer un message à plusieurs propriétaires | Exemple : nouvelle adresse des bureaux | Propriétaires : Voir |
| 28 | Enregistrer la quote-part de chaque propriétaire dans les charges de l'immeuble | Exemple : A1 = 8 %, A2 = 5 % *(syndic)* | Propriétaires : Modifier |

Les relevés propriétaires (ce que la société doit à chaque propriétaire) sont dans la partie Finance.

### 1.6 Boutons de l'employé : Locataires et baux

Les personnes et sociétés qui louent des unités, leurs baux (qui loue quoi, de quand à quand, pour quel loyer, avec quel dépôt de garantie) et les demandes de location.

Case utilisée : **Locataires et baux**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        L1(["Consulter les locataires et les baux<br/><i>case : Locataires et baux - Voir</i>"])
        L2(["Ajouter un locataire<br/><i>case : Locataires et baux - Créer</i>"])
        L3(["Modifier un locataire<br/><i>case : Locataires et baux - Modifier</i>"])
        L4(["Créer un bail<br/><i>case : Locataires et baux - Créer</i>"])
        L5(["Ajouter un garant à un bail<br/><i>case : Locataires et baux - Modifier</i>"])
        L6(["Activer un bail<br/><i>case : Locataires et baux - Modifier</i>"])
        L7(["Modifier le loyer<br/><i>case : Locataires et baux - Modifier</i>"])
        L8(["Renouveler un bail<br/><i>case : Locataires et baux - Modifier</i>"])
        L9(["Terminer un bail<br/><i>case : Locataires et baux - Modifier</i>"])
        L10(["Joindre des documents<br/><i>case : Locataires et baux - Modifier</i>"])
        L11(["Supprimer ou archiver un locataire ou un bail<br/><i>case : Locataires et baux - Supprimer</i>"])
        L12(["Rechercher et filtrer les locataires et les baux<br/><i>case : Locataires et baux - Voir</i>"])
        L13(["Voir les baux qui arrivent à échéance<br/><i>case : Locataires et baux - Voir</i>"])
        L14(["Enregistrer l'état des lieux d'entrée<br/><i>case : Locataires et baux - Modifier</i>"])
        L15(["Enregistrer l'état des lieux de sortie<br/><i>case : Locataires et baux - Modifier</i>"])
        L16(["Restituer le dépôt de garantie<br/><i>case : Locataires et baux - Modifier</i>"])
        L17(["Mettre plusieurs locataires sur un bail<br/><i>case : Locataires et baux - Modifier</i>"])
        L18(["Un bail pour plusieurs unités<br/><i>case : Locataires et baux - Créer</i>"])
        L19(["Suivre l'enregistrement du bail<br/><i>case : Locataires et baux - Modifier</i>"])
        L20(["Ajouter une note<br/><i>case : Locataires et baux - Modifier</i>"])
        L21(["Envoyer un email à un locataire<br/><i>case : Locataires et baux - Voir</i>"])
        L22(["Imprimer un bail<br/><i>case : Locataires et baux - Voir</i>"])
        L23(["Consulter l'historique d'un locataire<br/><i>case : Locataires et baux - Voir</i>"])
        L24(["Fusionner deux locataires<br/><i>case : Locataires et baux - Modifier</i>"])
        L25(["Exporter les locataires et les baux vers Excel<br/><i>case : Locataires et baux - Voir</i>"])
        L26(["Enregistrer les objets laissés par un locataire<br/><i>case : Locataires et baux - Modifier</i>"])
        L27(["Enregistrer une demande de location<br/><i>case : Locataires et baux - Créer</i>"])
        L28(["Enregistrer ce que la personne recherche<br/><i>case : Locataires et baux - Modifier</i>"])
        L29(["Planifier ou enregistrer une visite pour une demande de location<br/><i>case : Locataires et baux - Créer</i>"])
        L30(["Trouver les demandes de location qui correspondent à une unité libre<br/><i>case : Locataires et baux - Voir</i>"])
        L31(["Transformer une demande de location en locataire et en bail<br/><i>case : Locataires et baux - Créer</i>"])
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

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les locataires et les baux | — | Locataires et baux : Voir |
| 2 | Ajouter un locataire | Personne ou société | Locataires et baux : Créer |
| 3 | Modifier un locataire | Coordonnées, pièce d'identité | Locataires et baux : Modifier |
| 4 | Créer un bail | Locataire, unité, dates, loyer, dépôt | Locataires et baux : Créer |
| 5 | Ajouter un garant à un bail | — | Locataires et baux : Modifier |
| 6 | Activer un bail | Une fois signé, l'unité devient « louée » | Locataires et baux : Modifier |
| 7 | Modifier le loyer | Avec sa date de prise d'effet | Locataires et baux : Modifier |
| 8 | Renouveler un bail | — | Locataires et baux : Modifier |
| 9 | Terminer un bail | Le locataire part, l'unité devient « libre » | Locataires et baux : Modifier |
| 10 | Joindre des documents | Bail signé, pièce d'identité | Locataires et baux : Modifier |
| 11 | Supprimer ou archiver un locataire ou un bail | Même règle que les biens | Locataires et baux : Supprimer |
| 12 | Rechercher et filtrer les locataires et les baux | — | Locataires et baux : Voir |
| 13 | Voir les baux qui arrivent à échéance | Dans les 60 prochains jours | Locataires et baux : Voir |
| 14 | Enregistrer l'état des lieux d'entrée | État de chaque pièce, avec photos | Locataires et baux : Modifier |
| 15 | Enregistrer l'état des lieux de sortie | Comparé à l'état des lieux d'entrée | Locataires et baux : Modifier |
| 16 | Restituer le dépôt de garantie | Totalement ou en partie, avec le motif. Exemple : 2 000 TND de dépôt, 200 TND retenus pour une vitre cassée. Le dépôt est l'argent versé par le locataire au début du bail comme garantie contre les dégâts ou les impayés | Locataires et baux : Modifier |
| 17 | Mettre plusieurs locataires sur un bail | Couple, colocataires | Locataires et baux : Modifier |
| 18 | Un bail pour plusieurs unités | Exemple : une société loue 3 bureaux et 2 parkings | Locataires et baux : Créer |
| 19 | Suivre l'enregistrement du bail | Date et reçu de la recette des finances ; alerte avant la limite de 60 jours | Locataires et baux : Modifier |
| 20 | Ajouter une note | Appel, plainte, demande du locataire | Locataires et baux : Modifier |
| 21 | Envoyer un email à un locataire | Une copie est conservée | Locataires et baux : Voir |
| 22 | Imprimer un bail | Rempli avec les informations du locataire et de l'unité | Locataires et baux : Voir |
| 23 | Consulter l'historique d'un locataire | Anciens baux et unités, ponctualité des paiements | Locataires et baux : Voir |
| 24 | Fusionner deux locataires | — | Locataires et baux : Modifier |
| 25 | Exporter les locataires et les baux vers Excel | — | Locataires et baux : Voir |
| 26 | Enregistrer les objets laissés par un locataire | Quoi (avec photos), où c'est gardé, contact et délai pour les récupérer, et la suite (rendus avec date et signature, ou donnés ou jetés après le délai). La durée légale de conservation est à vérifier | Locataires et baux : Modifier |
| 27 | Enregistrer une demande de location | Exemple : Ines appelle, « 2 pièces près du Lac 2, 900 TND par mois maximum » | Locataires et baux : Créer |
| 28 | Enregistrer ce que la personne recherche | Budget, nombre de pièces, quartier, date d'entrée | Locataires et baux : Modifier |
| 29 | Planifier ou enregistrer une visite pour une demande de location | — | Locataires et baux : Créer |
| 30 | Trouver les demandes de location qui correspondent à une unité libre | Règles simples, sans IA | Locataires et baux : Voir |
| 31 | Transformer une demande de location en locataire et en bail | Rien n'est ressaisi | Locataires et baux : Créer |

**Demandes de location** (boutons 27 à 31) : une personne ou une société qui veut louer mais n'a pas encore de bail. Elle n'a pas d'interface (décision D-010) ; c'est le personnel qui l'enregistre.

### 1.7 Boutons de l'employé : Finance

Tout l'argent : loyers à payer, paiements reçus (espèces, chèque, virement), impayés, dépenses pour les biens, et ce que la société doit à chaque propriétaire.

Cases utilisées : **Factures et paiements**, **Dépenses**, **Relevés propriétaires**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        F1(["Consulter les factures, paiements et soldes<br/><i>case : Factures et paiements - Voir</i>"])
        F2(["Générer les factures de loyer du mois<br/><i>case : Factures et paiements - Créer</i>"])
        F3(["Créer une facture manuellement<br/><i>case : Factures et paiements - Créer</i>"])
        F4(["Enregistrer un paiement<br/><i>case : Factures et paiements - Créer</i>"])
        F5(["Affecter un paiement à des factures<br/><i>case : Factures et paiements - Modifier</i>"])
        F6(["Imprimer ou envoyer un reçu<br/><i>case : Factures et paiements - Voir</i>"])
        F7(["Annuler une facture par un avoir<br/><i>case : Factures et paiements - Supprimer</i>"])
        F8(["Consulter les loyers impayés<br/><i>case : Factures et paiements - Voir</i>"])
        F9(["Enregistrer une dépense pour un bien<br/><i>case : Dépenses - Créer</i>"])
        F10(["Consulter les dépenses<br/><i>case : Dépenses - Voir</i>"])
        F11(["Préparer un relevé propriétaire<br/><i>case : Relevés propriétaires - Créer</i>"])
        F12(["Envoyer un relevé propriétaire<br/><i>case : Relevés propriétaires - Voir</i>"])
        F13(["Enregistrer un versement au propriétaire<br/><i>case : Relevés propriétaires - Modifier</i>"])
        F14(["Suivre un chèque<br/><i>case : Factures et paiements - Modifier</i>"])
        F15(["Enregistrer un chèque impayé<br/><i>case : Factures et paiements - Modifier</i>"])
        F16(["Envoyer un rappel de paiement manuellement<br/><i>case : Factures et paiements - Voir</i>"])
        F17(["Mettre en place un échéancier de remboursement<br/><i>case : Factures et paiements - Créer</i>"])
        F18(["Rembourser un locataire<br/><i>case : Factures et paiements - Créer</i>"])
        F19(["Consulter les dépôts de garantie détenus<br/><i>case : Factures et paiements - Voir</i>"])
        F20(["Choisir qui paie une dépense<br/><i>case : Dépenses - Modifier</i>"])
        F21(["Joindre une facture à une dépense<br/><i>case : Dépenses - Modifier</i>"])
        F22(["Consulter les entrées et sorties d'argent par bien<br/><i>case : Factures et paiements - Voir</i>"])
        F23(["Clôturer un mois<br/><i>case : Factures et paiements - Modifier</i>"])
        F24(["Rapprocher les paiements du relevé bancaire<br/><i>case : Factures et paiements - Modifier</i>"])
        F25(["Consulter les revenus propres de la société<br/><i>case : Factures et paiements - Voir</i>"])
        F26(["Exporter pour le comptable<br/><i>case : Factures et paiements - Voir</i>"])
        F27(["Appeler les charges de l'immeuble auprès des propriétaires<br/><i>case : Factures et paiements - Créer</i>"])
        F28(["Consulter la trésorerie de l'immeuble<br/><i>case : Factures et paiements - Voir</i>"])
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

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les factures, paiements et soldes | — | Factures et paiements : Voir |
| 2 | Générer les factures de loyer du mois | Toutes en une fois, à partir des baux actifs | Factures et paiements : Créer |
| 3 | Créer une facture manuellement | Exemple : 150 TND pour une clé perdue | Factures et paiements : Créer |
| 4 | Enregistrer un paiement | Espèces, chèque ou virement, avec la date | Factures et paiements : Créer |
| 5 | Affecter un paiement à des factures | Exemple : un paiement de 2 000 TND règle janvier et février | Factures et paiements : Modifier |
| 6 | Imprimer ou envoyer un reçu | — | Factures et paiements : Voir |
| 7 | Annuler une facture par un avoir | On corrige par une nouvelle écriture, on n'efface jamais | Factures et paiements : Supprimer |
| 8 | Consulter les loyers impayés | — | Factures et paiements : Voir |
| 9 | Enregistrer une dépense pour un bien | Exemple : 300 TND de plombier dans l'appartement A1 | Dépenses : Créer |
| 10 | Consulter les dépenses | Par bien, propriétaire ou période | Dépenses : Voir |
| 11 | Préparer un relevé propriétaire | Loyers encaissés − dépenses − honoraires de la société = somme due au propriétaire | Relevés propriétaires : Créer |
| 12 | Envoyer un relevé propriétaire | En PDF par email (chaque copropriétaire reçoit une copie) | Relevés propriétaires : Voir |
| 13 | Enregistrer un versement au propriétaire | — | Relevés propriétaires : Modifier |
| 14 | Suivre un chèque | Reçu, remis en banque, puis encaissé ou impayé | Factures et paiements : Modifier |
| 15 | Enregistrer un chèque impayé | Le paiement est annulé, le loyer est de nouveau dû, les frais bancaires peuvent être refacturés | Factures et paiements : Modifier |
| 16 | Envoyer un rappel de paiement manuellement | En plus des rappels automatiques | Factures et paiements : Voir |
| 17 | Mettre en place un échéancier de remboursement | Exemple : 3 000 TND de dette, 500 TND de plus chaque mois | Factures et paiements : Créer |
| 18 | Rembourser un locataire | Exemple : il a payé deux fois par erreur | Factures et paiements : Créer |
| 19 | Consulter les dépôts de garantie détenus | — | Factures et paiements : Voir |
| 20 | Choisir qui paie une dépense | Propriétaire, locataire (s'il a cassé) ou société | Dépenses : Modifier |
| 21 | Joindre une facture à une dépense | Photo ou PDF de la facture du prestataire | Dépenses : Modifier |
| 22 | Consulter les entrées et sorties d'argent par bien | — | Factures et paiements : Voir |
| 23 | Clôturer un mois | Plus personne ne peut modifier ses chiffres | Factures et paiements : Modifier |
| 24 | Rapprocher les paiements du relevé bancaire | — | Factures et paiements : Modifier |
| 25 | Consulter les revenus propres de la société | Honoraires de gestion et commissions de vente | Factures et paiements : Voir |
| 26 | Exporter pour le comptable | Factures, paiements et dépenses d'une période vers Excel | Factures et paiements : Voir |
| 27 | Appeler les charges de l'immeuble auprès des propriétaires | Chaque mois ou trimestre, chacun reçoit sa part *(syndic)* | Factures et paiements : Créer |
| 28 | Consulter la trésorerie de l'immeuble | Charges encaissées − dépenses payées *(syndic)* | Factures et paiements : Voir |

La TVA, la retenue à la source et la facture électronique attendent l'avis du comptable.

### 1.8 Boutons de l'employé : Maintenance

Tous les travaux sur les biens : **réparations**, mais aussi **ménage** et **remise en état / détailing**, réalisés par un technicien interne ou un fournisseur.

Cases utilisées : **Demandes d'intervention**, **Ordres de travail**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        M1(["Consulter les demandes d'intervention<br/><i>case : Demandes d'intervention - Voir</i>"])
        M2(["Enregistrer une demande d'intervention<br/><i>case : Demandes d'intervention - Créer</i>"])
        M3(["Modifier une demande<br/><i>case : Demandes d'intervention - Modifier</i>"])
        M4(["Transformer une demande en ordre de travail<br/><i>case : Ordres de travail - Créer</i>"])
        M5(["Affecter un ordre de travail<br/><i>case : Ordres de travail - Modifier</i>"])
        M6(["Planifier la date des travaux<br/><i>case : Ordres de travail - Modifier</i>"])
        M7(["Consulter les ordres de travail<br/><i>case : Ordres de travail - Voir</i>"])
        M8(["Mettre à jour l'avancement<br/><i>case : Ordres de travail - Modifier</i>"])
        M9(["Ajouter des notes et des photos<br/><i>case : Ordres de travail - Modifier</i>"])
        M10(["Enregistrer le matériel et les heures<br/><i>case : Ordres de travail - Modifier</i>"])
        M11(["Valider les travaux<br/><i>case : Ordres de travail - Modifier</i>"])
        M12(["Annuler une demande ou un ordre de travail<br/><i>case : Ordres de travail - Supprimer</i>"])
        M13(["Créer la dépense à partir d'un ordre de travail terminé<br/><i>case : Dépenses - Créer</i>"])
        M14(["Choisir le type de travaux<br/><i>case : Ordres de travail - Modifier</i>"])
        M15(["Planifier des travaux récurrents<br/><i>case : Ordres de travail - Créer</i>"])
        M16(["Préparer une unité pour un nouveau locataire<br/><i>case : Ordres de travail - Créer</i>"])
        M17(["Préparer une unité pour une vente ou une visite<br/><i>case : Ordres de travail - Créer</i>"])
        M18(["Suivre une check-list pendant les travaux<br/><i>case : Ordres de travail - Modifier</i>"])
        M19(["Créer des modèles de check-list<br/><i>case : Ordres de travail - Créer</i>"])
        M20(["Consulter le calendrier de maintenance<br/><i>case : Ordres de travail - Voir</i>"])
        M21(["Rouvrir des travaux mal faits<br/><i>case : Ordres de travail - Modifier</i>"])
        M22(["Consulter l'historique de maintenance d'un bien<br/><i>case : Demandes d'intervention - Voir</i>"])
        M23(["Exporter la maintenance vers Excel<br/><i>case : Ordres de travail - Voir</i>"])
        M24(["Signaler un objet trouvé<br/><i>case : Ordres de travail - Modifier</i>"])
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
```

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les demandes d'intervention | — | Demandes d'intervention : Voir |
| 2 | Enregistrer une demande d'intervention | Exemple : le locataire appelle, « fuite d'eau dans la cuisine de A1 » | Demandes d'intervention : Créer |
| 3 | Modifier une demande | Description ou priorité | Demandes d'intervention : Modifier |
| 4 | Transformer une demande en ordre de travail | — | Ordres de travail : Créer |
| 5 | Affecter un ordre de travail | À un technicien interne **ou** à un fournisseur | Ordres de travail : Modifier |
| 6 | Planifier la date des travaux | — | Ordres de travail : Modifier |
| 7 | Consulter les ordres de travail | Un technicien ne voit que les siens | Ordres de travail : Voir |
| 8 | Mettre à jour l'avancement | Commencé, en attente de pièces, terminé | Ordres de travail : Modifier |
| 9 | Ajouter des notes et des photos | Photos avant et après | Ordres de travail : Modifier |
| 10 | Enregistrer le matériel et les heures | — | Ordres de travail : Modifier |
| 11 | Valider les travaux | Un responsable vérifie avant de clôturer | Ordres de travail : Modifier |
| 12 | Annuler une demande ou un ordre de travail | — | Ordres de travail : Supprimer |
| 13 | Créer la dépense à partir d'un ordre de travail terminé | — | Dépenses : Créer |
| 14 | Choisir le type de travaux | Réparation, ménage, nettoyage approfondi / détailing, peinture, jardinage, désinsectisation, inspection | Ordres de travail : Modifier |
| 15 | Planifier des travaux récurrents | Exemple : ménage des escaliers chaque lundi | Ordres de travail : Créer |
| 16 | Préparer une unité pour un nouveau locataire | Un clic crée les travaux habituels ; quand tout est fait, l'unité devient « prête à louer » | Ordres de travail : Créer |
| 17 | Préparer une unité pour une vente ou une visite | Détailing avant les photos ou les visites | Ordres de travail : Créer |
| 18 | Suivre une check-list pendant les travaux | Cuisine, salle de bain, fenêtres, sols… | Ordres de travail : Modifier |
| 19 | Créer des modèles de check-list | — | Ordres de travail : Créer |
| 20 | Consulter le calendrier de maintenance | — | Ordres de travail : Voir |
| 21 | Rouvrir des travaux mal faits | — | Ordres de travail : Modifier |
| 22 | Consulter l'historique de maintenance d'un bien | — | Demandes d'intervention : Voir |
| 23 | Exporter la maintenance vers Excel | — | Ordres de travail : Voir |
| 24 | Signaler un objet trouvé | Ajouté à la liste des objets laissés par l'ancien locataire | Ordres de travail : Modifier |

### 1.9 Boutons de l'employé : Sécurité des immeubles

Incidents, visiteurs, gardiens et clés. Un gardien est soit un **employé** de la société, soit une **société de sécurité** (fournisseur).

Case utilisée : **Sécurité**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        SEC1(["Enregistrer un incident de sécurité<br/><i>case : Sécurité - Créer</i>"])
        SEC2(["Consulter les incidents<br/><i>case : Sécurité - Voir</i>"])
        SEC3(["Enregistrer un visiteur<br/><i>case : Sécurité - Créer</i>"])
        SEC4(["Planifier les tours de garde<br/><i>case : Sécurité - Modifier</i>"])
        SEC5(["Enregistrer qui détient les clés<br/><i>case : Sécurité - Modifier</i>"])
    end

    EM --- SEC1
    EM --- SEC2
    EM --- SEC3
    EM --- SEC4
    EM --- SEC5
```

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Enregistrer un incident de sécurité | Exemple : « serrure de l'entrée cassée, 2 h du matin », avec photos | Sécurité : Créer |
| 2 | Consulter les incidents | — | Sécurité : Voir |
| 3 | Enregistrer un visiteur | Nom, unité visitée, heure d'arrivée et de départ | Sécurité : Créer |
| 4 | Planifier les tours de garde | Qui garde quel immeuble, de jour ou de nuit | Sécurité : Modifier |
| 5 | Enregistrer qui détient les clés | Exemple : clé de A1 remise au plombier le 10 mars, rendue le 11 | Sécurité : Modifier |

### 1.10 Boutons de l'employé : Fournisseurs

Les sociétés et travailleurs indépendants qui font des travaux pour la société : qui ils sont, ce qu'ils font, leurs documents et contrats, et tous leurs travaux.

Cases utilisées : **Fournisseurs**, **Dépenses**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        V1(["Consulter les fournisseurs<br/><i>case : Fournisseurs - Voir</i>"])
        V2(["Ajouter un fournisseur<br/><i>case : Fournisseurs - Créer</i>"])
        V3(["Modifier un fournisseur<br/><i>case : Fournisseurs - Modifier</i>"])
        V4(["Ajouter des contacts<br/><i>case : Fournisseurs - Modifier</i>"])
        V5(["Choisir les types de services<br/><i>case : Fournisseurs - Modifier</i>"])
        V6(["Activer ou désactiver un fournisseur<br/><i>case : Fournisseurs - Modifier</i>"])
        V7A(["Joindre un document avec son type et ses dates<br/><i>case : Fournisseurs - Modifier</i>"])
        V7B(["Être averti avant l'expiration d'un document<br/><i>case : Fournisseurs - Voir</i>"])
        V7C(["Voir l'état des documents de chaque fournisseur<br/><i>case : Fournisseurs - Voir</i>"])
        V7E(["Demander un nouveau document au fournisseur<br/><i>case : Fournisseurs - Voir</i>"])
        V7F(["Enregistrer les conditions du contrat<br/><i>case : Fournisseurs - Modifier</i>"])
        V8(["Consulter l'historique des travaux d'un fournisseur<br/><i>case : Fournisseurs - Voir</i>"])
        V9(["Ajouter une note<br/><i>case : Fournisseurs - Modifier</i>"])
        V10(["Enregistrer la facture d'un fournisseur<br/><i>case : Dépenses - Créer</i>"])
        V11(["Supprimer ou archiver un fournisseur<br/><i>case : Fournisseurs - Supprimer</i>"])
        V12(["Rechercher et filtrer les fournisseurs<br/><i>case : Fournisseurs - Voir</i>"])
        V13(["Noter un fournisseur après des travaux<br/><i>case : Fournisseurs - Modifier</i>"])
        V14(["Comparer les fournisseurs d'un même service<br/><i>case : Fournisseurs - Voir</i>"])
        V15(["Demander des devis et en choisir un<br/><i>case : Fournisseurs - Créer</i>"])
        V16(["Consulter ce qui est dû à chaque fournisseur<br/><i>case : Dépenses - Voir</i>"])
        V17(["Enregistrer un paiement à un fournisseur<br/><i>case : Dépenses - Modifier</i>"])
        V18(["Désigner un fournisseur préféré<br/><i>case : Fournisseurs - Modifier</i>"])
        V19(["Fusionner deux fournisseurs<br/><i>case : Fournisseurs - Modifier</i>"])
        V20(["Exporter les fournisseurs vers Excel<br/><i>case : Fournisseurs - Voir</i>"])
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

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les fournisseurs | — | Fournisseurs : Voir |
| 2 | Ajouter un fournisseur | Société ou indépendant | Fournisseurs : Créer |
| 3 | Modifier un fournisseur | Adresse, téléphone, compte bancaire, matricule fiscal | Fournisseurs : Modifier |
| 4 | Ajouter des contacts | Le patron, la secrétaire, l'ouvrier sur place | Fournisseurs : Modifier |
| 5 | Choisir les types de services | Plomberie, électricité, ménage, ascenseur, sécurité… | Fournisseurs : Modifier |
| 6 | Activer ou désactiver un fournisseur | Sans perdre son historique | Fournisseurs : Modifier |
| 7a | Joindre un document avec son type et ses dates | Contrat, attestation d'assurance, licence, attestation fiscale, attestation CNSS ; les anciennes versions sont gardées | Fournisseurs : Modifier |
| 7b | Être averti avant l'expiration d'un document | Exemple : « l'assurance de Plomberie Ben Salah expire dans 30 jours » | Fournisseurs : Voir |
| 7c | Voir l'état des documents de chaque fournisseur | Vert : tout est valide ; orange : un document expire bientôt ; rouge : expiré ou manquant | Fournisseurs : Voir |
| 7e | Demander un nouveau document au fournisseur | Par email depuis l'ERP | Fournisseurs : Voir |
| 7f | Enregistrer les conditions du contrat | Exemple : entretien ascenseur, 300 TND par mois, 1 an, renouvelable | Fournisseurs : Modifier |
| 8 | Consulter l'historique des travaux d'un fournisseur | — | Fournisseurs : Voir |
| 9 | Ajouter une note | Exemple : « toujours en retard » | Fournisseurs : Modifier |
| 10 | Enregistrer la facture d'un fournisseur | Liée aux travaux et transformée en dépense | Dépenses : Créer |
| 11 | Supprimer ou archiver un fournisseur | Même règle que les biens | Fournisseurs : Supprimer |
| 12 | Rechercher et filtrer les fournisseurs | — | Fournisseurs : Voir |
| 13 | Noter un fournisseur après des travaux | Qualité, ponctualité, prix, de 1 à 5 étoiles | Fournisseurs : Modifier |
| 14 | Comparer les fournisseurs d'un même service | Note moyenne, prix moyen, nombre de travaux | Fournisseurs : Voir |
| 15 | Demander des devis et en choisir un | — | Fournisseurs : Créer |
| 16 | Consulter ce qui est dû à chaque fournisseur | — | Dépenses : Voir |
| 17 | Enregistrer un paiement à un fournisseur | — | Dépenses : Modifier |
| 18 | Désigner un fournisseur préféré | Exemple : « pour les ascenseurs de la Résidence Yasmine, appeler X en premier » | Fournisseurs : Modifier |
| 19 | Fusionner deux fournisseurs | — | Fournisseurs : Modifier |
| 20 | Exporter les fournisseurs vers Excel | — | Fournisseurs : Voir |

**Règle 7d :** avant de confier des travaux à un fournisseur dont l'état
des documents est rouge, l'ERP avertit : « L'assurance de ce fournisseur
a expiré. Affecter quand même ? »

### 1.11 Boutons de l'employé : Ventes

La vente d'un bien ou d'une unité, de la mise en vente au jour où **l'acheteur devient propriétaire**. La société vend surtout ses propres unités ; parfois un propriétaire lui demande de revendre la sienne (mandat de vente).

Cases utilisées : **Ventes**, **Prospects et acheteurs**, **Paiements des acheteurs**.

``` mermaid
flowchart LR
    EM["👤 Employé"]

    subgraph ERP["ERP des opérations immobilières"]
        direction TB
        SA1(["Consulter les unités en vente<br/><i>case : Ventes - Voir</i>"])
        SA2(["Mettre une unité en vente<br/><i>case : Ventes - Créer</i>"])
        SA3(["Enregistrer un mandat de vente<br/><i>case : Ventes - Créer</i>"])
        SA4(["Modifier une annonce<br/><i>case : Ventes - Modifier</i>"])
        SA5(["Ajouter un prospect<br/><i>case : Prospects et acheteurs - Créer</i>"])
        SA6(["Planifier ou enregistrer une visite<br/><i>case : Prospects et acheteurs - Créer</i>"])
        SA7(["Enregistrer une offre<br/><i>case : Ventes - Créer</i>"])
        SA8(["Accepter ou refuser une offre<br/><i>case : Ventes - Modifier</i>"])
        SA9(["Réserver l'unité pour l'acheteur<br/><i>case : Ventes - Modifier</i>"])
        SA10(["Enregistrer un paiement de l'acheteur<br/><i>case : Paiements des acheteurs - Créer</i>"])
        SA11(["Conclure la vente<br/><i>case : Ventes - Modifier</i>"])
        SA12(["Annuler une réservation ou une vente<br/><i>case : Ventes - Supprimer</i>"])
        SA13(["Joindre des documents<br/><i>case : Ventes - Modifier</i>"])
        SA14(["Rechercher et filtrer les annonces<br/><i>case : Ventes - Voir</i>"])
        SA15(["Enregistrer les besoins d'un prospect<br/><i>case : Prospects et acheteurs - Modifier</i>"])
        SA16(["Trouver les unités qui correspondent à un prospect<br/><i>case : Prospects et acheteurs - Voir</i>"])
        SA17(["Envoyer une annonce à un prospect<br/><i>case : Ventes - Voir</i>"])
        SA18(["Enregistrer une contre-offre<br/><i>case : Ventes - Modifier</i>"])
        SA19(["Être averti avant l'expiration d'une réservation<br/><i>case : Ventes - Voir</i>"])
        SA20(["Mettre en place un échéancier de paiement<br/><i>case : Paiements des acheteurs - Créer</i>"])
        SA21(["Suivre les paiements des acheteurs<br/><i>case : Paiements des acheteurs - Voir</i>"])
        SA22(["Enregistrer la commission d'une vente sous mandat<br/><i>case : Ventes - Modifier</i>"])
        SA23(["Consulter le pipeline des ventes<br/><i>case : Ventes - Voir</i>"])
        SA24(["Imprimer les documents de vente<br/><i>case : Ventes - Voir</i>"])
        SA25(["Ajouter une note sur un prospect<br/><i>case : Prospects et acheteurs - Modifier</i>"])
        SA26(["Fusionner deux prospects<br/><i>case : Prospects et acheteurs - Modifier</i>"])
        SA27(["Vendre une unité occupée par un locataire<br/><i>case : Ventes - Modifier</i>"])
        SA28(["Vérifier l'identité de l'acheteur<br/><i>case : Prospects et acheteurs - Modifier</i>"])
        SA29(["Exporter les ventes vers Excel<br/><i>case : Ventes - Voir</i>"])
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

| N° | Bouton | Ce qu'il fait | Case à cocher |
|---|---|---|---|
| 1 | Consulter les unités en vente | — | Ventes : Voir |
| 2 | Mettre une unité en vente | Exemple : appartement B4, prix demandé 320 000 TND | Ventes : Créer |
| 3 | Enregistrer un mandat de vente | Un propriétaire demande à la société de revendre son unité | Ventes : Créer |
| 4 | Modifier une annonce | Exemple : baisser le prix | Ventes : Modifier |
| 5 | Ajouter un prospect | Personne ou société intéressée | Prospects et acheteurs : Créer |
| 6 | Planifier ou enregistrer une visite | — | Prospects et acheteurs : Créer |
| 7 | Enregistrer une offre | — | Ventes : Créer |
| 8 | Accepter ou refuser une offre | — | Ventes : Modifier |
| 9 | Réserver l'unité pour l'acheteur | Avec un acompte (arbon) et une date d'expiration | Ventes : Modifier |
| 10 | Enregistrer un paiement de l'acheteur | Acompte, échéance ou solde | Paiements des acheteurs : Créer |
| 11 | Conclure la vente | Acte définitif signé : l'acheteur devient propriétaire | Ventes : Modifier |
| 12 | Annuler une réservation ou une vente | Motif et sort de l'acompte | Ventes : Supprimer |
| 13 | Joindre des documents | Promesse de vente, pièce d'identité de l'acheteur, acte | Ventes : Modifier |
| 14 | Rechercher et filtrer les annonces | — | Ventes : Voir |
| 15 | Enregistrer les besoins d'un prospect | Budget, type, quartier, nombre de pièces | Prospects et acheteurs : Modifier |
| 16 | Trouver les unités qui correspondent à un prospect | Règles simples, sans IA | Prospects et acheteurs : Voir |
| 17 | Envoyer une annonce à un prospect | Email avec photos, prix et plan | Ventes : Voir |
| 18 | Enregistrer une contre-offre | — | Ventes : Modifier |
| 19 | Être averti avant l'expiration d'une réservation | — | Ventes : Voir |
| 20 | Mettre en place un échéancier de paiement | Exemple : 30 % à la signature, 40 % à 6 mois, 30 % à la livraison | Paiements des acheteurs : Créer |
| 21 | Suivre les paiements des acheteurs | — | Paiements des acheteurs : Voir |
| 22 | Enregistrer la commission d'une vente sous mandat | — | Ventes : Modifier |
| 23 | Consulter le pipeline des ventes | Prospects, visites, offres, réservations, ventes du mois | Ventes : Voir |
| 24 | Imprimer les documents de vente | Bon de réservation, promesse de vente | Ventes : Voir |
| 25 | Ajouter une note sur un prospect | — | Prospects et acheteurs : Modifier |
| 26 | Fusionner deux prospects | — | Prospects et acheteurs : Modifier |
| 27 | Vendre une unité occupée par un locataire | Le bail continue ; à partir de la date de vente, le loyer va au nouveau propriétaire | Ventes : Modifier |
| 28 | Vérifier l'identité de l'acheteur | Règle anti-blanchiment de janvier 2026 (à confirmer avec le comptable) | Prospects et acheteurs : Modifier |
| 29 | Exporter les ventes vers Excel | — | Ventes : Voir |

### 1.12 Syndic

Quand la société vend plusieurs unités d'un immeuble, l'immeuble a
plusieurs propriétaires ; ses parties communes (escaliers, ascenseur,
entrée, gardien, ménage) doivent être gérées et payées par tous les
propriétaires ensemble. C'est le travail du **syndic**. Ce n'est **pas
une activité séparée** : l'ERP en couvre déjà l'essentiel. Quatre
boutons, marqués *(syndic)*, ont été ajoutés aux parties existantes :
Biens 17, Propriétaires 28, Finance 27 et 28. Une **interface dédiée au
syndic** viendra **après le MVP**.

### 1.13 Prévu en V1

Les rapports et tableaux de bord des employés viendront après le tableau
de bord de l'administrateur, en **V1**.

------------------------------------------------------------------------

## 2. Diagramme de classes --- MVP

Les noms entre crochets dans les diagrammes sont les noms français ; le
nom anglais (utilisé dans le code) figure dans la table de
correspondance (section 2.12). Les attributs de chaque classe seront
ajoutés plus tard.

Lecture : `"1" --> "*"` signifie « un … a plusieurs … » ; `"0..1"`
signifie « zéro ou un » ; `"0..*"` signifie « zéro ou plusieurs ».

### 2.1 La société et ses membres

| Classe | Ce que c'est |
|---|---|
| Organisation | La société immobilière (exemple : Médina Immobilier) |
| Succursale | Une succursale de la société (exemple : Tunis Nord, Sousse) |
| Département | Un département d'une succursale (exemple : Location, Vente) |
| Utilisateur | Une personne qui se connecte (un compte par organisation) |
| Appartenance | Le lien entre un utilisateur et une société |
| Rôle | Un rôle défini par la société (exemple : « Gestionnaire ») |
| Privilège | Une case cochée : un domaine et une action (exemple : Baux / Créer) |

``` mermaid
classDiagram
    class Organization["Organisation"]
    class Branch["Succursale"]
    class Department["Département"]
    class Role["Rôle"]
    class Privilege["Privilège"]
    class User["Utilisateur"]
    class Membership["Appartenance"]

    Organization "1" --> "*" Branch : possède
    Branch "1" --> "*" Department : possède
    Organization "1" --> "*" Role : définit
    Role "1" --> "*" Privilege : accorde
    User "1" --> "1" Membership : possède
    Organization "1" --> "*" Membership : possède
    Membership "*" --> "*" Role : détient
    Membership "*" --> "1" Department : travaille dans
```

### 2.2 Les biens

| Classe | Ce que c'est |
|---|---|
| Bien | Un immeuble, une maison ou un terrain, à une adresse |
| Bâtiment | Un bâtiment d'un bien, seulement si utile |
| Étage | Un étage d'un bâtiment, seulement si utile |
| Unité | Appartement, bureau, commerce, parking : ce qui est loué ou vendu |
| PartieCommune | Escaliers, ascenseur, entrée ou parking d'un immeuble (syndic) |

``` mermaid
classDiagram
    class Branch["Succursale"]
    class Property["Bien"]
    class Building["Bâtiment"]
    class Floor["Étage"]
    class Unit["Unité"]
    class SharedPart["PartieCommune"]

    Branch "1" --> "*" Property : gère
    Property "1" --> "0..*" Building : contient
    Building "1" --> "0..*" Floor : contient
    Property "1" --> "1..*" Unit : contient
    Floor "0..1" --> "*" Unit : contient
    Property "1" --> "0..*" SharedPart : possède
```

### 2.3 Les propriétaires

| Classe | Ce que c'est |
|---|---|
| Propriétaire | Une personne ou société qui possède des unités : un acheteur, ou la société elle-même pour son stock |
| Détention | Quel propriétaire détient quelle unité, avec sa quote-part et les dates |
| MandatDeGestion | Ce que la société gère pour un propriétaire, avec les dates et les honoraires par bien |

``` mermaid
classDiagram
    class Owner["Propriétaire"]
    class Ownership["Détention"]
    class Unit["Unité"]
    class ManagementAgreement["MandatDeGestion"]
    class Organization["Organisation"]

    Owner "1" --> "*" Ownership : détient
    Ownership "*" --> "1" Unit : sur
    Owner "1" --> "*" ManagementAgreement : signe
    ManagementAgreement "*" --> "*" Unit : couvre
    Organization "0..1" --> "0..1" Owner : est propriétaire de son propre stock
```

### 2.4 Locataires et baux

| Classe | Ce que c'est |
|---|---|
| Locataire | Une personne ou société qui loue |
| Bail | Le contrat de location : dates, loyer, statut |
| Garant | Une personne qui garantit que le locataire paiera |
| ChangementDeLoyer | Chaque changement de loyer, avec sa date (historique conservé) |
| ÉtatDesLieux | L'état des lieux d'entrée ou de sortie, avec photos |
| DépôtDeGarantie | Le dépôt versé, conservé, puis restitué (totalement ou en partie, avec motifs) |
| ObjetLaissé | Les objets laissés par un locataire, et ce qu’ils sont devenus |
| DemandeDeLocation | Une personne qui veut louer, avec ce qu’elle recherche |
| Visite | Une visite d'une unité, pour une demande de location ou un prospect |

``` mermaid
classDiagram
    class Lease["Bail"]
    class Renter["Locataire"]
    class Unit["Unité"]
    class Guarantor["Garant"]
    class RentChange["ChangementDeLoyer"]
    class Inspection["ÉtatDesLieux"]
    class Deposit["DépôtDeGarantie"]
    class LeftItem["ObjetLaissé"]
    class RentRequest["DemandeDeLocation"]
    class Viewing["Visite"]

    Lease "*" --> "*" Renter : loué par
    Lease "*" --> "*" Unit : couvre
    Lease "1" --> "*" Guarantor : garanti par
    Lease "1" --> "*" RentChange : historique du loyer
    Lease "1" --> "0..2" Inspection : entrée et sortie
    Lease "1" --> "0..1" Deposit : possède
    Lease "1" --> "*" LeftItem : objets laissés
    RentRequest "1" --> "*" Viewing : possède
    Viewing "*" --> "1" Unit : de
    RentRequest "0..1" --> "0..1" Renter : devient
```

**Noms anglais « Renter » et « tenant » :** dans le code, le locataire
(la personne ou la société qui **loue** une unité) s'appelle **Renter**.
Le mot anglais **tenant** garde son sens de multi-tenancy : un tenant
est une **organisation** (la société immobilière qui utilise l'ERP).
Ainsi, les deux sens ne sont pas mélangés dans la base de données
(Glossaire, règle de nommage N-01).

### 2.5 Finance

| Classe | Ce que c'est |
|---|---|
| Facture | Ce qu’une personne doit payer : loyer d’un locataire ou charges d’immeuble d’un propriétaire |
| LigneDeFacture | Une ligne de facture (loyer, charges, clé perdue…) |
| Avoir | Annule tout ou partie d’une facture ; rien n’est jamais effacé |
| Paiement | Argent reçu : espèces, chèque ou virement ; un chèque garde son statut (reçu, remis, encaissé, impayé) |
| Imputation | Quelle partie d’un paiement règle quelle facture |
| ÉchéancierDeRemboursement | Un accord pour qu’un locataire rembourse sa dette en plusieurs fois |
| Remboursement | Argent rendu à un locataire |
| Dépense | Un coût payé pour un bien ou une unité, et qui le paie (propriétaire, locataire ou société) |
| RelevéPropriétaire | Pour un propriétaire et une période : loyers − dépenses − honoraires = somme due |
| VersementPropriétaire | L’argent versé au propriétaire |
| ClôtureDePériode | Un mois clôturé, qui ne peut plus être modifié |

``` mermaid
classDiagram
    class Lease["Bail"]
    class Invoice["Facture"]
    class Renter["Locataire"]
    class Owner["Propriétaire"]
    class InvoiceLine["LigneDeFacture"]
    class CreditNote["Avoir"]
    class Payment["Paiement"]
    class Allocation["Imputation"]
    class PaymentPlan["ÉchéancierDeRemboursement"]
    class Refund["Remboursement"]
    class Unit["Unité"]
    class Expense["Dépense"]
    class OwnerStatement["RelevéPropriétaire"]
    class OwnerPayout["VersementPropriétaire"]
    class Organization["Organisation"]
    class PeriodClosing["ClôtureDePériode"]

    Lease "1" --> "*" Invoice : génère
    Invoice "*" --> "0..1" Renter : facturée à
    Invoice "*" --> "0..1" Owner : facturée à
    Invoice "1" --> "*" InvoiceLine : contient
    Invoice "1" --> "*" CreditNote : annulée par
    Payment "1" --> "*" Allocation : réparti en
    Allocation "*" --> "1" Invoice : règle
    Renter "1" --> "*" PaymentPlan : accepte
    Renter "1" --> "*" Refund : reçoit
    Unit "1" --> "*" Expense : coûte
    Owner "1" --> "*" OwnerStatement : reçoit
    OwnerStatement "1" --> "0..1" OwnerPayout : payé par
    Organization "1" --> "*" PeriodClosing : clôture
```

### 2.6 Maintenance

| Classe | Ce que c'est |
|---|---|
| DemandeDIntervention | Un problème ou un besoin signalé, avec son canal et sa priorité |
| OrdreDeTravail | Les travaux : type (réparation, ménage, détailing…), date prévue, statut |
| JournalDeTravaux | Avancement, notes et photos avant/après |
| MatérielUtilisé | Matériel et heures utilisés |
| ModèleDeCheckList | Une check-list réutilisable, avec ses éléments |
| TravauxRécurrents | Des travaux qui reviennent régulièrement ; ils créent leurs ordres de travail |
| PlanDePréparation | Un ensemble de travaux pour préparer une unité (nouveau locataire ou vente) |
| AccordPropriétaire | L’accord d’un propriétaire pour des travaux coûteux, avec la date |

``` mermaid
classDiagram
    class MaintenanceRequest["DemandeDIntervention"]
    class Unit["Unité"]
    class SharedPart["PartieCommune"]
    class WorkOrder["OrdreDeTravail"]
    class Membership["Appartenance"]
    class Vendor["Fournisseur"]
    class WorkLog["JournalDeTravaux"]
    class MaterialUse["MatérielUtilisé"]
    class ChecklistTemplate["ModèleDeCheckList"]
    class RecurringJob["TravauxRécurrents"]
    class PreparationPlan["PlanDePréparation"]
    class OwnerApproval["AccordPropriétaire"]
    class Expense["Dépense"]
    class LeftItem["ObjetLaissé"]

    MaintenanceRequest "*" --> "0..1" Unit : concerne
    MaintenanceRequest "*" --> "0..1" SharedPart : concerne
    MaintenanceRequest "1" --> "*" WorkOrder : devient
    WorkOrder "*" --> "0..1" Unit : sur
    WorkOrder "*" --> "0..1" SharedPart : sur
    WorkOrder "*" --> "0..1" Membership : réalisé par un technicien interne
    WorkOrder "*" --> "0..1" Vendor : réalisé par un fournisseur
    WorkOrder "1" --> "*" WorkLog : avancement
    WorkOrder "1" --> "*" MaterialUse : utilise
    WorkOrder "*" --> "0..1" ChecklistTemplate : suit
    RecurringJob "1" --> "*" WorkOrder : crée
    PreparationPlan "1" --> "*" WorkOrder : regroupe
    WorkOrder "1" --> "0..1" OwnerApproval : approuvé par le propriétaire
    WorkOrder "1" --> "0..1" Expense : coûte
    WorkOrder "1" --> "*" LeftItem : objets trouvés
```

### 2.7 Fournisseurs

| Classe | Ce que c'est |
|---|---|
| Fournisseur | Une société ou un travailleur indépendant extérieur |
| ContactFournisseur | Une personne chez le fournisseur |
| TypeDeService | Un type de travaux : plomberie, électricité, ménage, ascenseur, sécurité… |
| DocumentFournisseur | Un document avec son type et ses dates de validité ; les anciennes versions sont gardées |
| ContratFournisseur | Les conditions du contrat : services, prix, durée, renouvellement |
| Devis | Le prix d’un fournisseur pour des travaux, choisi ou non |
| FactureFournisseur | La facture du fournisseur ; elle devient une dépense |
| PaiementFournisseur | Un paiement d’une facture fournisseur |
| ÉvaluationFournisseur | Les étoiles données après des travaux |
| FournisseurPréféré | « Pour ce service dans ce bien, appeler ce fournisseur en premier » |

``` mermaid
classDiagram
    class Vendor["Fournisseur"]
    class VendorContact["ContactFournisseur"]
    class ServiceType["TypeDeService"]
    class VendorDocument["DocumentFournisseur"]
    class VendorContract["ContratFournisseur"]
    class WorkOrder["OrdreDeTravail"]
    class Quote["Devis"]
    class VendorBill["FactureFournisseur"]
    class Expense["Dépense"]
    class VendorPayment["PaiementFournisseur"]
    class VendorRating["ÉvaluationFournisseur"]
    class PreferredVendor["FournisseurPréféré"]
    class Property["Bien"]

    Vendor "1" --> "*" VendorContact : possède
    Vendor "*" --> "*" ServiceType : propose
    Vendor "1" --> "*" VendorDocument : fournit
    Vendor "1" --> "*" VendorContract : lié par
    WorkOrder "1" --> "*" Quote : reçoit
    Quote "*" --> "1" Vendor : de
    Vendor "1" --> "*" VendorBill : envoie
    VendorBill "*" --> "0..1" WorkOrder : pour
    VendorBill "1" --> "1" Expense : devient
    VendorBill "1" --> "*" VendorPayment : payée par
    WorkOrder "1" --> "0..1" VendorRating : évalué
    VendorRating "*" --> "1" Vendor : concerne
    PreferredVendor "*" --> "1" Vendor : préfère
    PreferredVendor "*" --> "1" ServiceType : pour le service
    PreferredVendor "*" --> "1" Property : dans
```

### 2.8 Ventes

| Classe | Ce que c'est |
|---|---|
| Annonce | Une unité mise en vente, avec son prix demandé et son statut |
| MandatDeVente | Un propriétaire demande à la société de revendre son unité, avec les conditions de commission |
| Prospect | Une personne ou société intéressée par un achat ; après une offre, c’est l’**acheteur** |
| Offre | Un prix proposé ; une contre-offre répond à une autre offre |
| Réservation | L’unité bloquée pour un acheteur, avec un acompte et une date d’expiration |
| Vente | L’accord : prix convenu, signature, conclusion |
| ÉchéancierDePaiement | Le plan des échéances de l’acheteur |
| PaiementAcheteur | Argent reçu d’un acheteur |
| Commission | Les honoraires de la société sur une vente sous mandat |
| VérificationDIdentité | La vérification de l'identité de l'acheteur (règle anti-blanchiment) |

``` mermaid
classDiagram
    class Unit["Unité"]
    class Listing["Annonce"]
    class SalesMandate["MandatDeVente"]
    class Owner["Propriétaire"]
    class Prospect["Prospect"]
    class Viewing["Visite"]
    class Offer["Offre"]
    class Reservation["Réservation"]
    class Sale["Vente"]
    class PaymentSchedule["ÉchéancierDePaiement"]
    class BuyerPayment["PaiementAcheteur"]
    class Commission["Commission"]
    class IdentityCheck["VérificationDIdentité"]
    class Ownership["Détention"]

    Unit "1" --> "*" Listing : mise en vente
    Listing "*" --> "0..1" SalesMandate : sous
    SalesMandate "*" --> "1" Owner : donné par
    Prospect "1" --> "*" Viewing : effectue
    Viewing "*" --> "1" Unit : de
    Listing "1" --> "*" Offer : reçoit
    Offer "*" --> "1" Prospect : faite par
    Offer "0..1" --> "0..1" Offer : répond à
    Offer "1" --> "0..1" Reservation : mène à
    Reservation "1" --> "0..1" Sale : mène à
    Sale "1" --> "0..1" PaymentSchedule : payée selon
    Sale "1" --> "*" BuyerPayment : reçoit
    Sale "1" --> "0..1" Commission : rapporte
    Prospect "1" --> "*" IdentityCheck : vérifié par
    Sale "1" --> "1" Ownership : crée
```

### 2.9 Sécurité des immeubles

| Classe | Ce que c'est |
|---|---|
| IncidentDeSécurité | Un événement (serrure cassée, intrusion), avec date, lieu et photos |
| Visiteur | Un visiteur : nom, unité visitée, heures d’arrivée et de départ |
| TourDeGarde | Qui garde quel bien, et quand |
| RemiseDeClé | Une clé remise à quelqu’un, et sa date de retour |

``` mermaid
classDiagram
    class Property["Bien"]
    class SecurityIncident["IncidentDeSécurité"]
    class VisitorEntry["Visiteur"]
    class Unit["Unité"]
    class GuardShift["TourDeGarde"]
    class Membership["Appartenance"]
    class Vendor["Fournisseur"]
    class KeyHandover["RemiseDeClé"]

    Property "1" --> "*" SecurityIncident : enregistre
    Property "1" --> "*" VisitorEntry : enregistre
    VisitorEntry "*" --> "0..1" Unit : visite
    GuardShift "*" --> "1" Property : garde
    GuardShift "*" --> "0..1" Membership : gardien employé par la société
    GuardShift "*" --> "0..1" Vendor : gardien d’une société de sécurité
    KeyHandover "*" --> "1" Unit : clé de
```

### 2.10 Fiches partagées

| Classe | Ce que c'est |
|---|---|
| Document | Un fichier (PDF, photo) avec son type, joint à n’importe quelle fiche |
| Note | Un commentaire joint à n’importe quelle fiche |
| Notification | Un message à un employé dans l’ERP, ou un email à un locataire, propriétaire ou fournisseur |
| ÉvénementDAudit | Qui a fait quoi, sur quelle fiche, et quand ; jamais modifiable |
| Invitation | Une invitation à rejoindre la société dans l’ERP |
| DemandeDeMutation | La demande d’un employé de changer de département ou de succursale, et la réponse |
| QuotePartDeCharges | La quote-part de chaque propriétaire dans les charges d’un immeuble (syndic) |
| NImporteQuelleFiche | N’importe quelle fiche (bail, unité, fournisseur, travaux, vente…) — ce n'est pas une vraie classe |

``` mermaid
classDiagram
    class Document["Document"]
    class AnyRecord["NImporteQuelleFiche"]
    class Note["Note"]
    class AuditEvent["ÉvénementDAudit"]
    class Membership["Appartenance"]
    class Notification["Notification"]
    class Organization["Organisation"]
    class Invitation["Invitation"]
    class MoveRequest["DemandeDeMutation"]
    class Owner["Propriétaire"]
    class CostShare["QuotePartDeCharges"]
    class Property["Bien"]

    Document "*" --> "1" AnyRecord : joint à
    Note "*" --> "1" AnyRecord : jointe à
    AuditEvent "*" --> "1" AnyRecord : concerne
    AuditEvent "*" --> "1" Membership : fait par
    Notification "*" --> "0..1" Membership : envoyée à l’employé
    Organization "1" --> "*" Invitation : envoie
    Membership "1" --> "*" MoveRequest : demande
    Owner "1" --> "*" CostShare : paie
    CostShare "*" --> "1" Property : de l’immeuble
```

Remarques :

-   **Bâtiment** et **Étage** sont facultatifs : une maison est un bien
    avec une seule unité.
-   Des **copropriétaires** (exemple : 3 frères) sont plusieurs
    Détentions sur la même unité, qui totalisent 100 %.
-   Une **facture** est adressée **soit** à un locataire (loyer), **soit**
    à un propriétaire (charges d'immeuble).
-   Un **ordre de travail** est réalisé **soit** par un technicien interne,
    **soit** par un fournisseur ; il concerne **soit** une unité, **soit**
    une partie commune.
-   La **Visite** sert à la fois aux demandes de location et aux
    prospects.
-   À la **conclusion** d'une vente, une nouvelle Détention est créée :
    l'acheteur devient propriétaire.

### 2.11 Diagramme de classes complet du MVP

Toutes les classes et tous les liens dans une seule vue (74 classes).

``` mermaid
classDiagram
    class Organization["Organisation"]
    class Branch["Succursale"]
    class Department["Département"]
    class Role["Rôle"]
    class Privilege["Privilège"]
    class User["Utilisateur"]
    class Membership["Appartenance"]
    class Property["Bien"]
    class Building["Bâtiment"]
    class Floor["Étage"]
    class Unit["Unité"]
    class SharedPart["PartieCommune"]
    class Owner["Propriétaire"]
    class Ownership["Détention"]
    class ManagementAgreement["MandatDeGestion"]
    class Lease["Bail"]
    class Renter["Locataire"]
    class Guarantor["Garant"]
    class RentChange["ChangementDeLoyer"]
    class Inspection["ÉtatDesLieux"]
    class Deposit["DépôtDeGarantie"]
    class LeftItem["ObjetLaissé"]
    class RentRequest["DemandeDeLocation"]
    class Viewing["Visite"]
    class Invoice["Facture"]
    class InvoiceLine["LigneDeFacture"]
    class CreditNote["Avoir"]
    class Payment["Paiement"]
    class Allocation["Imputation"]
    class PaymentPlan["ÉchéancierDeRemboursement"]
    class Refund["Remboursement"]
    class Expense["Dépense"]
    class OwnerStatement["RelevéPropriétaire"]
    class OwnerPayout["VersementPropriétaire"]
    class PeriodClosing["ClôtureDePériode"]
    class MaintenanceRequest["DemandeDIntervention"]
    class WorkOrder["OrdreDeTravail"]
    class Vendor["Fournisseur"]
    class WorkLog["JournalDeTravaux"]
    class MaterialUse["MatérielUtilisé"]
    class ChecklistTemplate["ModèleDeCheckList"]
    class RecurringJob["TravauxRécurrents"]
    class PreparationPlan["PlanDePréparation"]
    class OwnerApproval["AccordPropriétaire"]
    class VendorContact["ContactFournisseur"]
    class ServiceType["TypeDeService"]
    class VendorDocument["DocumentFournisseur"]
    class VendorContract["ContratFournisseur"]
    class Quote["Devis"]
    class VendorBill["FactureFournisseur"]
    class VendorPayment["PaiementFournisseur"]
    class VendorRating["ÉvaluationFournisseur"]
    class PreferredVendor["FournisseurPréféré"]
    class Listing["Annonce"]
    class SalesMandate["MandatDeVente"]
    class Prospect["Prospect"]
    class Offer["Offre"]
    class Reservation["Réservation"]
    class Sale["Vente"]
    class PaymentSchedule["ÉchéancierDePaiement"]
    class BuyerPayment["PaiementAcheteur"]
    class Commission["Commission"]
    class IdentityCheck["VérificationDIdentité"]
    class SecurityIncident["IncidentDeSécurité"]
    class VisitorEntry["Visiteur"]
    class GuardShift["TourDeGarde"]
    class KeyHandover["RemiseDeClé"]
    class Document["Document"]
    class AnyRecord["NImporteQuelleFiche"]
    class Note["Note"]
    class AuditEvent["ÉvénementDAudit"]
    class Notification["Notification"]
    class Invitation["Invitation"]
    class MoveRequest["DemandeDeMutation"]
    class CostShare["QuotePartDeCharges"]

    Organization "1" --> "*" Branch : possède
    Branch "1" --> "*" Department : possède
    Organization "1" --> "*" Role : définit
    Role "1" --> "*" Privilege : accorde
    User "1" --> "1" Membership : possède
    Organization "1" --> "*" Membership : possède
    Membership "*" --> "*" Role : détient
    Membership "*" --> "1" Department : travaille dans
    Branch "1" --> "*" Property : gère
    Property "1" --> "0..*" Building : contient
    Building "1" --> "0..*" Floor : contient
    Property "1" --> "1..*" Unit : contient
    Floor "0..1" --> "*" Unit : contient
    Property "1" --> "0..*" SharedPart : possède
    Owner "1" --> "*" Ownership : détient
    Ownership "*" --> "1" Unit : sur
    Owner "1" --> "*" ManagementAgreement : signe
    ManagementAgreement "*" --> "*" Unit : couvre
    Organization "0..1" --> "0..1" Owner : est propriétaire de son propre stock
    Lease "*" --> "*" Renter : loué par
    Lease "*" --> "*" Unit : couvre
    Lease "1" --> "*" Guarantor : garanti par
    Lease "1" --> "*" RentChange : historique du loyer
    Lease "1" --> "0..2" Inspection : entrée et sortie
    Lease "1" --> "0..1" Deposit : possède
    Lease "1" --> "*" LeftItem : objets laissés
    RentRequest "1" --> "*" Viewing : possède
    Viewing "*" --> "1" Unit : de
    RentRequest "0..1" --> "0..1" Renter : devient
    Lease "1" --> "*" Invoice : génère
    Invoice "*" --> "0..1" Renter : facturée à
    Invoice "*" --> "0..1" Owner : facturée à
    Invoice "1" --> "*" InvoiceLine : contient
    Invoice "1" --> "*" CreditNote : annulée par
    Payment "1" --> "*" Allocation : réparti en
    Allocation "*" --> "1" Invoice : règle
    Renter "1" --> "*" PaymentPlan : accepte
    Renter "1" --> "*" Refund : reçoit
    Unit "1" --> "*" Expense : coûte
    Owner "1" --> "*" OwnerStatement : reçoit
    OwnerStatement "1" --> "0..1" OwnerPayout : payé par
    Organization "1" --> "*" PeriodClosing : clôture
    MaintenanceRequest "*" --> "0..1" Unit : concerne
    MaintenanceRequest "*" --> "0..1" SharedPart : concerne
    MaintenanceRequest "1" --> "*" WorkOrder : devient
    WorkOrder "*" --> "0..1" Unit : sur
    WorkOrder "*" --> "0..1" SharedPart : sur
    WorkOrder "*" --> "0..1" Membership : réalisé par un technicien interne
    WorkOrder "*" --> "0..1" Vendor : réalisé par un fournisseur
    WorkOrder "1" --> "*" WorkLog : avancement
    WorkOrder "1" --> "*" MaterialUse : utilise
    WorkOrder "*" --> "0..1" ChecklistTemplate : suit
    RecurringJob "1" --> "*" WorkOrder : crée
    PreparationPlan "1" --> "*" WorkOrder : regroupe
    WorkOrder "1" --> "0..1" OwnerApproval : approuvé par le propriétaire
    WorkOrder "1" --> "0..1" Expense : coûte
    WorkOrder "1" --> "*" LeftItem : objets trouvés
    Vendor "1" --> "*" VendorContact : possède
    Vendor "*" --> "*" ServiceType : propose
    Vendor "1" --> "*" VendorDocument : fournit
    Vendor "1" --> "*" VendorContract : lié par
    WorkOrder "1" --> "*" Quote : reçoit
    Quote "*" --> "1" Vendor : de
    Vendor "1" --> "*" VendorBill : envoie
    VendorBill "*" --> "0..1" WorkOrder : pour
    VendorBill "1" --> "1" Expense : devient
    VendorBill "1" --> "*" VendorPayment : payée par
    WorkOrder "1" --> "0..1" VendorRating : évalué
    VendorRating "*" --> "1" Vendor : concerne
    PreferredVendor "*" --> "1" Vendor : préfère
    PreferredVendor "*" --> "1" ServiceType : pour le service
    PreferredVendor "*" --> "1" Property : dans
    Unit "1" --> "*" Listing : mise en vente
    Listing "*" --> "0..1" SalesMandate : sous
    SalesMandate "*" --> "1" Owner : donné par
    Prospect "1" --> "*" Viewing : effectue
    Listing "1" --> "*" Offer : reçoit
    Offer "*" --> "1" Prospect : faite par
    Offer "0..1" --> "0..1" Offer : répond à
    Offer "1" --> "0..1" Reservation : mène à
    Reservation "1" --> "0..1" Sale : mène à
    Sale "1" --> "0..1" PaymentSchedule : payée selon
    Sale "1" --> "*" BuyerPayment : reçoit
    Sale "1" --> "0..1" Commission : rapporte
    Prospect "1" --> "*" IdentityCheck : vérifié par
    Sale "1" --> "1" Ownership : crée
    Property "1" --> "*" SecurityIncident : enregistre
    Property "1" --> "*" VisitorEntry : enregistre
    VisitorEntry "*" --> "0..1" Unit : visite
    GuardShift "*" --> "1" Property : garde
    GuardShift "*" --> "0..1" Membership : gardien employé par la société
    GuardShift "*" --> "0..1" Vendor : gardien d’une société de sécurité
    KeyHandover "*" --> "1" Unit : clé de
    Document "*" --> "1" AnyRecord : joint à
    Note "*" --> "1" AnyRecord : jointe à
    AuditEvent "*" --> "1" AnyRecord : concerne
    AuditEvent "*" --> "1" Membership : fait par
    Notification "*" --> "0..1" Membership : envoyée à l’employé
    Organization "1" --> "*" Invitation : envoie
    Membership "1" --> "*" MoveRequest : demande
    Owner "1" --> "*" CostShare : paie
    CostShare "*" --> "1" Property : de l’immeuble
```

### 2.12 Table de correspondance français ↔ anglais (code)

| Français | Anglais (code) | Groupe |
|---|---|---|
| Organisation | Organization | La société et ses membres |
| Succursale | Branch | La société et ses membres |
| Département | Department | La société et ses membres |
| Utilisateur | User | La société et ses membres |
| Appartenance | Membership | La société et ses membres |
| Rôle | Role | La société et ses membres |
| Privilège | Privilege | La société et ses membres |
| Bien | Property | Les biens |
| Bâtiment | Building | Les biens |
| Étage | Floor | Les biens |
| Unité | Unit | Les biens |
| PartieCommune | SharedPart | Les biens |
| Propriétaire | Owner | Les propriétaires |
| Détention | Ownership | Les propriétaires |
| MandatDeGestion | ManagementAgreement | Les propriétaires |
| Locataire | Renter | Locataires et baux |
| Bail | Lease | Locataires et baux |
| Garant | Guarantor | Locataires et baux |
| ChangementDeLoyer | RentChange | Locataires et baux |
| ÉtatDesLieux | Inspection | Locataires et baux |
| DépôtDeGarantie | Deposit | Locataires et baux |
| ObjetLaissé | LeftItem | Locataires et baux |
| DemandeDeLocation | RentRequest | Locataires et baux |
| Visite | Viewing | Locataires et baux |
| Facture | Invoice | Finance |
| LigneDeFacture | InvoiceLine | Finance |
| Avoir | CreditNote | Finance |
| Paiement | Payment | Finance |
| Imputation | Allocation | Finance |
| ÉchéancierDeRemboursement | PaymentPlan | Finance |
| Remboursement | Refund | Finance |
| Dépense | Expense | Finance |
| RelevéPropriétaire | OwnerStatement | Finance |
| VersementPropriétaire | OwnerPayout | Finance |
| ClôtureDePériode | PeriodClosing | Finance |
| DemandeDIntervention | MaintenanceRequest | Maintenance |
| OrdreDeTravail | WorkOrder | Maintenance |
| JournalDeTravaux | WorkLog | Maintenance |
| MatérielUtilisé | MaterialUse | Maintenance |
| ModèleDeCheckList | ChecklistTemplate | Maintenance |
| TravauxRécurrents | RecurringJob | Maintenance |
| PlanDePréparation | PreparationPlan | Maintenance |
| AccordPropriétaire | OwnerApproval | Maintenance |
| Fournisseur | Vendor | Fournisseurs |
| ContactFournisseur | VendorContact | Fournisseurs |
| TypeDeService | ServiceType | Fournisseurs |
| DocumentFournisseur | VendorDocument | Fournisseurs |
| ContratFournisseur | VendorContract | Fournisseurs |
| Devis | Quote | Fournisseurs |
| FactureFournisseur | VendorBill | Fournisseurs |
| PaiementFournisseur | VendorPayment | Fournisseurs |
| ÉvaluationFournisseur | VendorRating | Fournisseurs |
| FournisseurPréféré | PreferredVendor | Fournisseurs |
| Annonce | Listing | Ventes |
| MandatDeVente | SalesMandate | Ventes |
| Prospect | Prospect | Ventes |
| Offre | Offer | Ventes |
| Réservation | Reservation | Ventes |
| Vente | Sale | Ventes |
| ÉchéancierDePaiement | PaymentSchedule | Ventes |
| PaiementAcheteur | BuyerPayment | Ventes |
| Commission | Commission | Ventes |
| VérificationDIdentité | IdentityCheck | Ventes |
| IncidentDeSécurité | SecurityIncident | Sécurité des immeubles |
| Visiteur | VisitorEntry | Sécurité des immeubles |
| TourDeGarde | GuardShift | Sécurité des immeubles |
| RemiseDeClé | KeyHandover | Sécurité des immeubles |
| Document | Document | Fiches partagées |
| Note | Note | Fiches partagées |
| Notification | Notification | Fiches partagées |
| ÉvénementDAudit | AuditEvent | Fiches partagées |
| Invitation | Invitation | Fiches partagées |
| DemandeDeMutation | MoveRequest | Fiches partagées |
| QuotePartDeCharges | CostShare | Fiches partagées |

------------------------------------------------------------------------

## Historique

| Version | Date | Changement |
|---|---|---|
| 1.0 | 2026-10-09 | Première version française : diagramme de cas d'utilisation (MVP) et diagramme de classes (MVP, par groupe et complet). |
| 1.1 | 2026-10-09 | « Tenant » (le locataire) renommé « Renter » ; « tenant » désigne l'organisation, comme en multi-tenancy (glossaire N-01). |
| 1.2 | 2026-10-09 | Un utilisateur a une seule appartenance : un compte par organisation, rien n'est partagé entre organisations. |
