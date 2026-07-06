# Requirements

## Product Intent

Permettre à l'admin de créer/modifier/supprimer une bannière promotionnelle (titre, texte, couleur de fond) depuis le backoffice, et l'afficher sur le frontend.

## Accepted Requirements

- Une seule bannière promotionnelle existe à la fois (modèle singleton, pas de liste ni de planification)
- L'admin (backoffice) peut créer la bannière : titre, texte, couleur de fond (choisie librement, format hexadécimal)
- L'admin peut modifier la bannière existante (mêmes champs)
- L'admin peut supprimer la bannière
- Le frontend affiche la bannière si elle existe, sans authentification requise
- Si aucune bannière n'existe (jamais créée ou supprimée), le frontend n'affiche rien
- Les endpoints backoffice (création/modification/suppression) sont protégés, rôle ADMIN requis (cohérent avec l'initiative `add-user-auth`)
- La lecture de la bannière côté frontend reste publique, comme les autres endpoints `/api/frontend/**`

## Out Of Scope

- Planification (date de début/fin d'affichage)
- Plusieurs bannières simultanées ou historique des bannières passées
- Ciblage par page, audience ou A/B testing
