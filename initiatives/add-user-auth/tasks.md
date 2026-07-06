# Tasks

## Coordination Tasks

- [x] Backend : endpoints `/api/auth/**` (register/login/refresh/logout) + protection `/api/backoffice/**` (rôle ADMIN)
- [x] Backoffice : page de connexion, protection des routes côté client, intercepteur refresh
- [x] Frontend : inscription/connexion, affichage du statut utilisateur dans le header
- [x] Vérifications API : 401 sans token, 403 avec rôle USER, refresh token, révocation après logout
- [ ] Vérifier le flux Docker Compose complet (CORS + cookies cross-origin entre ports 3000/3001/8080)
