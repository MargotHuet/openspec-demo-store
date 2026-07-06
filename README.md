# openspec-demo-store

Projet de démonstration pour le framework **OpenSpec** (spec-driven development, multi-repo).

Ce repo est le **point d'entrée** du projet `openspec-demo` : il ne contient aucun code applicatif. C'est le *context store* OpenSpec — il héberge la coordination des changements qui traversent plusieurs repos (initiatives : requirements, design, décisions, tâches partagées) ainsi que l'orchestration Docker de la stack complète.

## Architecture multi-repo

| Repo | Rôle | Port |
|------|------|------|
| [openspec-demo-store](https://github.com/MargotHuet/openspec-demo-store) *(ce repo)* | Context store — coordination des initiatives cross-repo, orchestration Docker | — |
| [openspec-demo-backend](https://github.com/MargotHuet/openspec-demo-backend) | Spring Boot 3 + JPA + PostgreSQL/H2 — logique métier, API frontend & backoffice | 8080 |
| [openspec-demo-frontend](https://github.com/MargotHuet/openspec-demo-frontend) | Next.js 14 — application cliente (catalogue produits) | 3000 |
| [openspec-demo-backoffice](https://github.com/MargotHuet/openspec-demo-backoffice) | Next.js 14 — interface d'administration (CRUD produits) | 3001 |

Le backend expose deux API REST séparées, consommées respectivement par le frontend (lecture seule) et le backoffice (CRUD complet) :

- `/api/frontend/**` → produits en stock, détail, recherche
- `/api/backoffice/**` → CRUD complet sur les produits

## Ce repo (`openspec-demo-store`)

```
openspec-demo-store/
├── .openspec-store/
│   └── store.yaml        ← métadonnées d'enregistrement du context store
├── initiatives/
│   └── <id>/              ← une initiative = un changement cross-repo
│       ├── initiative.yaml
│       ├── requirements.md
│       ├── design.md
│       ├── decisions.md
│       ├── questions.md
│       └── tasks.md
└── docker-compose.yml     ← orchestration de la stack complète
```

## Workflow cross-repo

Le projet utilise deux notions OpenSpec pour coordonner le travail entre repos :

1. **Context store** (ce repo) — le lieu neutre où vivent les *initiatives* : une initiative regroupe `requirements.md`, `design.md`, `decisions.md`, `questions.md` et `tasks.md` partagés entre tous les repos concernés.
2. **Workspace** (local, `openspec workspace ...`) — relie sur la machine du développeur les dossiers réels des repos (backend, frontend, backoffice) pour qu'un agent puisse naviguer entre eux depuis une seule session. C'est une construction locale à la machine, pas un repo Git.

**Déroulé type d'une tâche cross-repo** (ex : ajout d'une fonctionnalité qui touche backend + frontend + backoffice) :

1. Créer l'initiative dans ce repo : `openspec initiative create <id> --store openspec-demo-store --title "..." --summary "..."`
2. Remplir `requirements.md` / `design.md` de l'initiative : quels repos sont impactés, quels contrats d'API changent, dans quel ordre.
3. Dans chaque repo impacté, créer le change OpenSpec correspondant (`openspec/changes/<id>/`) en référence à l'initiative — le backend est toujours implémenté en premier, frontend et backoffice peuvent suivre en parallèle une fois ses endpoints disponibles.
4. Chaque repo implémente ses tâches indépendamment (`openspec-apply-change`), en cochant `tasks.md` au fur et à mesure.
5. Une fois tous les repos à jour, la checklist `tasks.md` de l'initiative (dans ce repo) est cochée et l'initiative peut être archivée.

Commandes utiles pour inspecter l'état cross-repo :

```bash
openspec workspace doctor --workspace openspec-demo   # vérifie les liens vers les 3 repos
openspec context-store list                            # liste les stores enregistrés
openspec initiative list --store openspec-demo-store    # liste les initiatives en cours
```

## Démarrage local

Les 3 repos applicatifs et ce repo doivent être clonés côte à côte (même dossier parent), car `docker-compose.yml` référence les autres repos par chemin relatif :

```
<parent>/
├── openspec-demo-store/
├── openspec-demo-backend/
├── openspec-demo-frontend/
└── openspec-demo-backoffice/
```

**Lancer la stack complète (PostgreSQL + backend + frontend + backoffice)** — depuis ce repo :
```bash
docker compose up --build
```

**Lancer un repo isolément en dev**, voir le README de chaque repo (`openspec-demo-backend`, `openspec-demo-frontend`, `openspec-demo-backoffice`).

## Variables d'environnement

| Variable | Valeur par défaut | Description |
|----------|-------------------|-------------|
| `NEXT_PUBLIC_API_URL` | `http://localhost:8080` | URL du backend, utilisée par le frontend et le backoffice |
