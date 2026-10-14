STATUT : TERMINÉ (2026-10-13) — 23 pages créées en plus des écrans existants et des galeries ; toutes en DESKTOP, design system Emerald ERP, version simple.

# PAGES_PROGRESS — Finir toutes les pages (simples) dans Stitch

## Récapitulatif final (nom → id Stitch, projet `18155339900589406319`)
| # | Page | Id |
|---|---|---|
| 1 | Contacts — Fiche d'un contact (Mehdi Trabelsi) | `d3b4c45c25ad4119875b6ca1fe612034` |
| 2 | Contacts — Ajouter un contact | `bb34e38b1d154ebc90cbc4c02bf78ecb` |
| 3 | Locataires et baux — Baux | `21c53c293c5c4f1ab63ee3d7a08255a6` |
| 4 | Locataires et baux — Fiche du bail | `fbec2dfca2944d8da0e8e73de91236b2` |
| 5 | Locataires et baux — Ajouter un bail | `3cbec467a6c34d98a38b227004a102d3` |
| 6 | Locataires et baux — Demandes de location | `75fd8591664f4c87bc83246ac3b8d404` |
| 7 | Finance — Factures de loyer | `32f931efcb4349dcb0f8a110c1afcafc` |
| 8 | Finance — Paiements | `0e2b32a3121d47d68599c07a6f5ab37d` |
| 9 | Finance — Enregistrer un paiement | `84b8e3b9e4f743d7a9914a38d9f16b7c` |
| 10 | Finance — Dépenses | `34911876a8ce4f93a328b1e37a2bad30` |
| 11 | Maintenance — Demandes d'intervention | `41049250e1d543c181b9228ff4495e98` |
| 12 | Maintenance — Fiche de l'intervention | `ca6c792ce4234c92b6e623dad3926e2c` |
| 13 | Maintenance — Nouvelle demande | `fcf0171418834efd8519bdf9f3216a2b` |
| 14 | Sécurité des immeubles — Incidents | `feb72bd39ac84fe0a40a11eca5a92337` |
| 15 | Sécurité des immeubles — Gardes et clés | `b0b73b8580bf4b5aa2873723d332f90a` |
| 16 | Fournisseurs — Liste | `e221c47b24aa44c2a9ec39925bdb0f5d` |
| 17 | Fournisseurs — Fiche fournisseur | `7164f41e57074cdd83ddc15b188df9cf` |
| 18 | Ventes — Annonces et offres | `6bd8c646201a43bf9bff216dd0afbbd9` |
| 19 | Ventes — Fiche de la vente | `90772fc6c6b54f59b959f1292bab0d7f` |
| 20 | Rapports — Accueil | `cafaae0db4834588b7e033f987a9d0b7` |
| 21 | Rapports — Impayés | `468f089a078c4263bf21ee9df647ad16` |
| 22 | Administration — Utilisateurs et rôles | `f951fb64c2744f9c87c1dd0f54c012bf` |
| 23 | Administration — Journal d'audit | `86a8341ca00440288d63fb9f6b39fb63` |

Écrans de la boucle « galeries » (LOOP_PROGRESS.md) : Unités – Galerie `b2f6eb89…`, Unité – Photos `41a9b279…`, Photo – Plein écran `c9d7bdb3…`, Documents – Galerie `eba7353a…`, Document – Aperçu `4d90f3b1…`, Unité – Photos (vide) `e96529eb…`, Documents – Galerie (vide) `be2c99c0…`, Contacts `aa6ba853…`.

### À nettoyer à la main dans Stitch (pas d'outil de suppression d'écran)
- Écrans propriétaires / contrats de gestion (9) : `03cfe3bb…`, `21724d9c…`, `c9be8640…`, `8993b21d…`, `d3e8e5d9…`, `99720077…`, `579fd5ef…`, `e4976aa9…`, `af2b1b43…`.
- Doublons d'appels échoués : `db372614…` et `6ba0c501…` (Unités – Galerie), `515dc982…` (Unité – Photos), `f3c248bc…` (Photo – Plein écran) ; + probablement un doublon « Finance — Factures de loyer » (appel échoué du passage 3) → garder `32f931ef…`. Images annexes de couverture (salon, cuisine, chambre, façade, salle de bain, `ceab9d2a…`) = normales.
- Anciens écrans : le tableau de bord, « Biens et unités », « Fiche du bien », « Fiche de l'unité », « Parties communes », « Biens archivés », « Carte des biens », « Fiches incomplètes » ont encore « Propriétaires » dans leur sidebar (au lieu de « Contacts ») ; les boutons « Voir » des deux fiches n'ont pas pu être ajoutés (LOOP_PROGRESS.md, bloqueur edit_screens).

### Écarts mineurs connus (non bloquants, à régler pendant le développement)
- Sidebar : sur certains écrans le menu Biens est replié ou déplié, l'en-tête varie légèrement (page 12 : « Emerald ERP / ERP Immobilier » inversé, page 17 : avatar « SB »), élément actif parfois en vert plein.
- Données : « Résidence Ennasr II » (page 17) et « Résidence Jasmin » (pages 9, 11) n'existent pas ailleurs ; « Bâtiment C » pour C-12 dans « Unités – Galerie ».
- Mise en page : colonnes Statut/Action un peu coupées à 800 px (pages 8, 10, 14) ; colonne « Prix demandé » étroite (page 18).
- Les liens entre écrans (clic → écran cible) ne sont pas faits : ils se règlent dans le mode prototype de Stitch ou directement dans le code.

Projet Stitch : `projects/18155339900589406319` · design system « Emerald ERP » = `assets/4870031587572422575` · DESKTOP seulement.
Consigne utilisateur (2026-10-13) : finir TOUTES les pages, version SIMPLE (projet portfolio), aller vite pour passer au développement. Ne pas sur-compliquer : 1 liste / 1 filtre / 1 bouton principal par page.

## Règles de travail
- Nouvelles pages = `generate_screen_from_text` avec le design system ci-dessus. Pas de modification des écrans existants (`edit_screens` n'enregistre pas, voir LOOP_PROGRESS.md).
- Après un échec Stitch (service unavailable / network failure) : NE PAS relancer à l'aveugle ; `get_screen` / attendre, car `list_screens` ne liste pas toujours les écrans récents.
- Par passage : jusqu'à 3 pages en parallèle, puis vérification rapide (capture) de chacune, puis cocher.
- Quand tout est coché : écrire « STATUT : TERMINÉ » en haut + tableau final nom → id.

## Bloc de mise en page commun (à coller dans chaque prompt)
SHELL: same left sidebar as the other screens: logo « ERP Immobilier / Emerald ERP System », workspace « Immobilière Carthage / Toutes les succursales », Tableau de bord, Biens (Biens et unités, Carte des biens, Parties communes, Biens archivés), Contacts, Locataires et baux, Finance (badge 12), Maintenance (badge 14), Sécurité des immeubles, Fournisseurs (badge 3), Ventes, Rapports, Administration (badge 4), user card « Sami Ben Ali — Administrateur ». The item of the current module is ACTIVE (light-green background with a dot; its group expanded if it has sub-items). Top bar: breadcrumb, search ⌘K, FR / عربية / EN, bell. Style: emerald design system (primary #047857, page background #F4FBF7, white cards, 1px #D5E6DC borders, Inter, 8px radius, Lucide-style line icons, all UI text in French, no emojis), ONE primary green button per screen, simple and calm, lots of white space. Amounts in TND with 3 decimals (1 250,000 TND). Reuse the realistic data: Résidence Les Jardins de Carthage, Immeuble Le Belvédère, Appartement B-101 / B-102 / B-203 / A-204 / C-12, Contacts: Hédi Ben Salem, Sté Foncière Carthage, Mehdi Trabelsi (locataire B-203), SARL Carthage Digital, Salma Gharbi (acquéreur A-204).
Les 3 rôles : Propriétaire (pastille verte), Locataire (bleu-gris), Acquéreur (ambre).

## Checklist des pages
### Déjà existantes (ne pas refaire)
Connexion, Inscription, Mot de passe oublié, Vérif e-mail, Vérif SMS, Tableau de bord, Biens et unités, Fiche du bien, Fiche de l'unité B-102, Ajouter un bien, Ajouter plusieurs unités, Carte des biens, Parties communes, Biens archivés, Fiches incomplètes, Unités – Galerie, Unité – Photos (+ vide), Photo – Plein écran, Documents – Galerie (+ vide), Document – Aperçu, Contacts.

### À créer
- [x] 1. Contacts — Fiche d'un contact (Mehdi Trabelsi, rôles Locataire + Propriétaire) → `d3b4c45c25ad4119875b6ca1fe612034`
- [x] 2. Contacts — Ajouter un contact (formulaire simple + choix des rôles) → `bb34e38b1d154ebc90cbc4c02bf78ecb`
- [x] 3. Locataires et baux — Baux (liste) → `21c53c293c5c4f1ab63ee3d7a08255a6`
- [x] 4. Locataires et baux — Fiche du bail → `fbec2dfca2944d8da0e8e73de91236b2`
- [x] 5. Locataires et baux — Ajouter un bail → `3cbec467a6c34d98a38b227004a102d3`
- [x] 6. Locataires et baux — Demandes de location → `75fd8591664f4c87bc83246ac3b8d404`
- [x] 7. Finance — Factures de loyer (liste) → `32f931efcb4349dcb0f8a110c1afcafc` (régénéré au passage 7 ; un doublon « Finance — Factures de loyer » issu de l'appel échoué du passage 3 existe probablement → à supprimer à la main, garder cet id)
- [x] 8. Finance — Paiements (liste) → `0e2b32a3121d47d68599c07a6f5ab37d`
- [x] 9. Finance — Enregistrer un paiement → `84b8e3b9e4f743d7a9914a38d9f16b7c`
- [x] 10. Finance — Dépenses → `34911876a8ce4f93a328b1e37a2bad30`
- [x] 11. Maintenance — Demandes d'intervention (liste) → `41049250e1d543c181b9228ff4495e98`
- [x] 12. Maintenance — Fiche de l'intervention (ordre de travail) → `ca6c792ce4234c92b6e623dad3926e2c`
- [x] 13. Maintenance — Nouvelle demande → `fcf0171418834efd8519bdf9f3216a2b`
- [x] 14. Sécurité des immeubles — Incidents (liste) → `feb72bd39ac84fe0a40a11eca5a92337`
- [x] 15. Sécurité des immeubles — Gardes et clés → `b0b73b8580bf4b5aa2873723d332f90a`
- [x] 16. Fournisseurs — Liste → `e221c47b24aa44c2a9ec39925bdb0f5d`
- [x] 17. Fournisseurs — Fiche fournisseur → `7164f41e57074cdd83ddc15b188df9cf`
- [x] 18. Ventes — Annonces et offres (liste) → `6bd8c646201a43bf9bff216dd0afbbd9`
- [x] 19. Ventes — Fiche de la vente (offre / réservation) → `90772fc6c6b54f59b959f1292bab0d7f`
- [x] 20. Rapports — Accueil des rapports → `cafaae0db4834588b7e033f987a9d0b7`
- [x] 21. Rapports — Impayés (exemple de rapport) → `468f089a078c4263bf21ee9df647ad16`
- [x] 22. Administration — Utilisateurs et rôles → `f951fb64c2744f9c87c1dd0f54c012bf`
- [x] 23. Administration — Journal d'audit → `86a8341ca00440288d63fb9f6b39fb63`
- [x] 24. Vérification finale (noms, sidebar, doublons) + tableau nom → id (voir « Récapitulatif final » en haut)

## Journal
- Passage 7 — 3 pages créées et vérifiées : factures de loyer `32f931ef…` (3 cartes Facturé/Encaissé/Impayé, statuts Payée/Partielle/Impayée, sidebar Finance › Factures), fiche de la vente A-204 `90772fc6…` (stepper 4 étapes, acquéreur, réservation, offres, paiements ; bouton « Conclure la vente »), accueil des rapports `cafaae0d…` (6 cartes avec un chiffre clé ; sidebar Rapports active, pas de bouton vert, ni de bascule FR active visible dans la barre). `list_screens` ne liste toujours aucun écran récent (retard) : pas utilisable pour retrouver les écrans ; décision = régénérer la page 7 (doublon possible).
- Passage 6 — 3 pages créées et vérifiées : fournisseurs `e221c47b…` (liste + filtres par catégorie, pastilles À jour / À renouveler), fiche Hydro Services Tunis `7164f41e…` (coordonnées, 3 documents avec validité, dernières interventions ; sidebar : menu Biens replié, pas de sous-menus), annonces de vente `6bd8c646…` (3 cartes de synthèse, vignettes, prix, acquéreur avec pastille ambre ; sidebar Biens dépliée). Écarts mineurs pour la vérif finale : colonne « Prix demandé » étroite (montant sur 2 lignes) ; la fiche fournisseur affiche « Résidence Ennasr II » (bien non défini ailleurs) et « Sami Ben Ali » avec avatar « SB » (au lieu de « SA »). Aucun échec Stitch.
- Passage 5 — 3 pages créées et vérifiées : nouvelle demande `fcf01714…` (canal et priorité en cartes, zone photos pointillée, bouton « Enregistrer la demande » sous le pli ; Stitch a envoyé en plus un petit ajustement « live » des vignettes, non nécessaire), incidents `feb72bd3…` (gravité, photos, statut ; colonne « Incident » étroite, colonne Action coupée à 800 px), gardes et clés `b0b73b85…` (2 cartes : gardes du jour Employé/Société de sécurité, remises de clés). Sidebar Sécurité des immeubles avec sous-menus Incidents / Gardes et clés. Aucun échec Stitch.
- Passage 4 — 3 pages créées et vérifiées : dépenses `34911876…` (filtres, mois, tableau ; colonne Statut un peu coupée à 800 px), demandes d'intervention `41049250…` (priorités Urgente/Normale/Basse, canal Téléphone/Message/Sur place), fiche intervention MR-0184 `ca6c792c…` (stepper 4 étapes, demande, prestataire, suivi, bouton « Marquer comme terminée »). Écart mineur pour la vérif finale : sur `ca6c792c…` l'en-tête de la sidebar est « Emerald ERP / ERP Immobilier » (inversé) et le sous-menu Biens est replié ; sur `41049250…` « ERP IMMOBILIER » en majuscules. Pas de relance des appels, aucun échec.
- Passage 3 — 2 pages créées et vérifiées : paiements `0e2b32a3…` (3 modes avec icônes, statuts Encaissé / Chèque déposé / Chèque rejeté, sidebar Finance › Paiements), enregistrer un paiement `84b8e3b9…` (cartes Espèces/Chèque/Virement, champs chèque, 1 bouton vert). Page 7 « Factures de loyer » : appel en « service unavailable », NON relancé.
  - **LEÇON (à garder)** : `list_screens` est en retard de 10–30 min et n'affiche pas les écrans récents ; mais les appels Stitch échoués (« service unavailable », « network failure », appel refusé) CRÉENT en général l'écran quand même → doublons visibles plus tard. Doublons trouvés au passage 3 : `515dc98294ba4d1caee8b0ae09b0c2ef` (Unité – Photos — B-102, copie en trop de `41a9b279…`), `f3c248bc757b4bb3b99d688a2834ef73` (Photo – Plein écran, copie en trop de `c9d7bdb3…`), + images annexes (`ceab9d2a…` salle de bain, `aa5ba7fd…` façade). À supprimer à la main avec les autres doublons (`db372614…`, `6ba0c501…`).
- Passage 2 — 3 pages créées et vérifiées sur capture : fiche du bail B-203 `fbec2dfc…` (3 cartes locataire/unité/propriétaire, conditions, derniers paiements, boutons « Renouveler » vert + « Résilier »), ajouter un bail `3cbec467…` (2 sections, bouton « Enregistrer le bail »), demandes de location `75fd8591…` (liste + filtres). Sidebar « Locataires et baux » avec sous-menus Baux / Demandes de location. Rien à corriger.
- Passage 1 — 3 pages créées en parallèle et vérifiées sur capture (shell, couleurs, 1 seul bouton vert) : fiche contact `d3b4c45c…` (3 cartes de rôles Locataire/Propriétaire/Acquéreur), ajouter un contact `bb34e38b…` (3 rôles cochables, bouton « Enregistrer le contact » sous le pli), baux `21c53c29…` (sidebar « Locataires et baux › Baux » active). Rien à corriger.
- Passage 0 (2026-10-13) — Plan créé à partir de `docs/reference/05-scope-matrix.md` (modules MVP : baux, finance, maintenance, sécurité, fournisseurs, ventes, rapports, administration/audit) et du UML (Sécurité = incidents, visiteurs, gardiens/gardes, clés). Aucun écran créé.

## Hors boucle (en attente de l'utilisateur)
- LOOP_PROGRESS.md : item « Boutons Voir » bloqué (edit_screens n'enregistre pas) ; les galeries sont faites.
- À supprimer à la main dans Stitch : écrans Propriétaires/contrats de gestion (9), doublons « Unités – Galerie » `db372614…` et `6ba0c501…`.
- La sidebar des anciens écrans montre encore « Propriétaires » (les nouveaux écrans utilisent « Contacts »).
