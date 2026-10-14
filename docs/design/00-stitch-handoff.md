# Stitch design — handoff (2026-10-14)

Start here in a new session. The screens live in Google Stitch, not in this repo.

> **SECURITY — do this first.** The commit `9dd3724` "Add Stitch MCP server" put a Google API key in clear text in `.mcp.json`. That commit is on the PUBLIC GitHub branch `claude/vigilant-ptolemy-lb9o4q` (the repo's default branch). **Rotate / delete that key in Google Cloud now**, then connect the Stitch MCP with the new key through your user-level config (`claude mcp add --scope user`) or an environment variable, never in a committed file. `main` does not contain the key; do not merge or cherry-pick `9dd3724`.

- **Stitch project:** « Real Estate Operations ERP » = `projects/18155339900589406319`
- **Design system:** « Emerald ERP » = `assets/4870031587572422575` (primary `#047857`, page `#F4FBF7`, cards white, borders `#D5E6DC`, Inter, 8 px radius, Lucide-style line icons, all UI text in French, one green primary button per screen).
- **Format:** desktop only, French only, simple pages (portfolio project, decision of 2026-10-13: "keep it simple, move fast to development").
- **Tool:** the Stitch MCP (`mcp__stitch__*`). Download a screen's HTML with `get_screen` → `htmlCode.downloadUrl`, its picture with `screenshot.downloadUrl` (append `=w1280` for a size).

## State

All planned pages are done. Full name → id table: [PAGES_PROGRESS.md](PAGES_PROGRESS.md) (« Récapitulatif final »). Gallery work (photos and documents galleries, full screen viewers, empty states): [LOOP_PROGRESS.md](LOOP_PROGRESS.md).

| Module | Screens |
|---|---|
| Auth | Connexion, Inscription, Mot de passe oublié, Vérif e-mail, Vérif SMS |
| Tableau de bord | 1 |
| Biens | Biens et unités, Fiche du bien, Fiche de l'unité, Ajouter un bien, Ajouter plusieurs unités, Carte des biens, Parties communes, Biens archivés, Fiches incomplètes |
| Galeries | Unités – Galerie, Unité – Photos (+ vide), Photo – Plein écran, Documents – Galerie (+ vide), Document – Aperçu |
| Contacts | liste, fiche, ajouter (3 rôles : Propriétaire, Locataire, Acquéreur) |
| Locataires et baux | baux, fiche, ajouter, demandes de location |
| Finance | factures, paiements, enregistrer un paiement, dépenses |
| Maintenance | demandes, fiche de l'intervention, nouvelle demande |
| Sécurité des immeubles | incidents, gardes et clés |
| Fournisseurs | liste, fiche |
| Ventes | annonces et offres, fiche de la vente |
| Rapports | accueil, impayés |
| Administration | utilisateurs et rôles, journal d'audit |

## Decisions taken

- The whole « Propriétaires » section and the « Contrats de gestion » screens were dropped (too dense, the three roles were missing). Replaced by one **Contacts** page where one contact can hold several roles (glossary N-09: Owner, Renter, Buyer).
- Sidebar item « Propriétaires » → « Contacts ». Old screens still show the old label.
- No renter/owner portal in the MVP (D-010): only staff screens.

## Still to do by hand in Stitch (the MCP cannot delete screens)

- Delete the 9 owner / management-contract screens, the duplicate screens created by failed calls, and the probable duplicate « Finance — Factures de loyer » (lists in PAGES_PROGRESS.md).
- Add the « Voir » buttons on « Fiche du bien » and « Fiche de l'unité » (the MCP edit did not save, see LOOP_PROGRESS.md « Bloqueurs »), and link screens in Stitch prototype mode.
- Replace « Propriétaires » by « Contacts » in the sidebar of the old screens.

## Lessons about the Stitch MCP (save time next session)

1. `edit_screens` does **not** edit in place. Either it returns only DOM operations (nothing saved — check with `get_screen`: same `htmlCode` file id = not saved) or it creates a copy with a new id.
2. A failed or refused call (« service unavailable », « network failure », refused tool use) usually **still creates the screen** on Stitch's side → duplicates appear later.
3. `list_screens` lags by 10–30 minutes and is huge; do not rely on it to confirm a recent screen. Keep the ids returned by `generate_screen_from_text`.
4. `generate_screen_from_text` works well with this recipe: design system id + `deviceType: DESKTOP` + a prompt made of (a) the sidebar/top-bar block, (b) the page content with realistic French data, (c) « Nothing else ». Three calls in parallel are fine.
5. Reuse the realistic data: Résidence Les Jardins de Carthage, Immeuble Le Belvédère, Appartements B-101 / B-102 / B-203 / A-204 / C-12, Mehdi Trabelsi (locataire B-203), Sté Foncière Carthage, Hédi Ben Salem, Salma Gharbi (acquéreur A-204).

## Next phase: development

Not started. The tech stack is **not decided** (see the architecture vision in `docs/phase-2/architecture/01-architecture-vision.md`). Suggested first step for the next session: decide the stack, then build the shell (sidebar + top bar) and the list/detail/form patterns once, and reuse them for the 23 pages.

Source documents: `docs/phase-1/model/02-diagrammes-uml-fr.md` (UML, French), `docs/reference/01-glossary.md`, `docs/reference/05-scope-matrix.md` (MVP scope).
