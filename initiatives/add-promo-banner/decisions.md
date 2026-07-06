# Decisions

## Accepted Decisions

### 2026-07-06: Modèle singleton plutôt que liste de bannières

- Decision: une seule bannière existe à la fois (pas de liste, pas de planification)
- Why: la demande initiale décrit "la" bannière au singulier, sans besoin de multi-bannières ni de programmation dans le temps
- Implications: endpoints backoffice sans `{id}` dans l'URL (`/api/backoffice/banner`), contrainte d'unicité à prévoir côté backend

### 2026-07-06: Couleur de fond au format hexadécimal

- Decision: la couleur de fond est stockée et saisie en format hexadécimal (ex: `#FF5733`), via un `<input type="color">` côté backoffice
- Why: format simple à valider et directement exploitable en CSS côté frontend, sans ambiguïté de parsing (vs noms de couleurs ou rgb())
- Implications: validation backend par regex (`^#[0-9A-Fa-f]{6}$`)
