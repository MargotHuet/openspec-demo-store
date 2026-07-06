# Requirements

## Product Intent

Sécuriser le backoffice (ADMIN) et permettre la connexion sur le frontend (USER) via JWT stateless. Backend/frontend/backoffice implémentés ; reste à vérifier le flux Docker Compose complet (CORS + cookies cross-origin entre ports 3000/3001/8080).

## Accepted Requirements

- Le backend expose des endpoints d'authentification : register, login, refresh (avec rotation), logout (révocation)
- Les routes `/api/backoffice/**` nécessitent un JWT valide avec le rôle ADMIN
- Les routes `/api/frontend/**` en lecture restent publiques ; un futur endpoint `/api/frontend/orders` sera protégé (rôle USER)
- Le frontend permet inscription + connexion, et affiche le statut de l'utilisateur dans le header
- Le backoffice affiche une page de connexion, redirige vers le dashboard après authentification, et protège toutes ses pages côté client
- Toutes les fonctions `api.ts` (frontend et backoffice) transmettent le token via `Authorization: Bearer <token>`, avec intercepteur 401 → refresh automatique → retry

## Out Of Scope

- Provider OAuth tiers (Google, GitHub)
- Refresh token longue durée sans rotation
