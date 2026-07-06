# Design

## Context

Permettre à l'admin de créer/modifier/supprimer une bannière promotionnelle (titre, texte, couleur de fond) depuis le backoffice, et l'afficher sur le frontend.

## Approach

- Nouvelle entité `PromoBanner` côté backend, singleton en base (une seule ligne à la fois)
- Endpoints :
  - `GET /api/frontend/banner` (public) → bannière courante, ou 204 si aucune n'existe
  - `GET /api/backoffice/banner` (ADMIN) → bannière courante, pour pré-remplir le formulaire d'édition
  - `POST /api/backoffice/banner` (ADMIN) → créer la bannière (409 si une existe déjà)
  - `PUT /api/backoffice/banner` (ADMIN) → modifier la bannière existante (404 si aucune)
  - `DELETE /api/backoffice/banner` (ADMIN) → supprimer la bannière existante
- Backoffice : section "Bannière promotionnelle" avec formulaire (titre, texte, `<input type="color">` pour la couleur de fond), qui bascule entre état "créer" et état "modifier/supprimer" selon qu'une bannière existe déjà
- Frontend : composant affiché en haut des pages, chargé au démarrage via `GET /api/frontend/banner` ; ne rend rien si la réponse est vide (204)

## Affected Areas

- backend (entité + endpoints `/api/backoffice/banner` et `/api/frontend/banner`)
- backoffice (formulaire de gestion de la bannière)
- frontend (composant d'affichage de la bannière)

## Dependencies

- Le backend doit être implémenté en premier ; backoffice et frontend consomment ensuite ses endpoints
- Réutilise la protection ADMIN déjà en place (`SecurityConfig`) issue de l'initiative `add-user-auth`

## Risks

- Concurrence sur le singleton : deux requêtes de création simultanées pourraient créer deux lignes si l'unicité n'est pas garantie en base (contrainte à prévoir côté backend)
