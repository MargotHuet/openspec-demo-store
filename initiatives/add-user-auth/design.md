# Design

## Context

Sécuriser le backoffice (ADMIN) et permettre la connexion sur le frontend (USER) via JWT stateless. Backend/frontend/backoffice implémentés ; reste à vérifier le flux Docker Compose complet (CORS + cookies cross-origin entre ports 3000/3001/8080).

## Approach

- JWT stateless généré par le backend (`JwtService`) ; rotation du refresh token, révocation en base (`RefreshTokenRepository`)
- `SecurityFilterChain` Spring Security : sessions STATELESS, `permitAll` sur `/api/auth/**` et `/api/frontend/**`, rôle ADMIN requis sur `/api/backoffice/**`
- Frontend et backoffice : contexte d'authentification en mémoire + intercepteur 401 → refresh automatique → retry

## Affected Areas

- backend (Spring Security + JWT)
- frontend (login/register UI + cookie)
- backoffice (login UI + middleware de protection)

## Dependencies

- Le backend doit être implémenté en premier ; frontend et backoffice consomment ensuite ses endpoints `/api/auth/**`

## Risks

- Cookies cross-origin entre les ports 3000/3001/8080 en environnement Docker Compose (CORS + credentials) — à vérifier (voir tasks.md)
