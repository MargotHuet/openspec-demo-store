# Decisions

## Accepted Decisions

### 2026-06-25: Rotation du refresh token (plutôt que sessions ou refresh longue durée)

- Decision: JWT stateless avec refresh token en rotation
- Why: permet la révocation ciblée après logout et limite la fenêtre d'exploitation d'un token volé
- Implications: nécessite une table `RefreshToken` côté backend et un intercepteur 401 → refresh côté frontend/backoffice
