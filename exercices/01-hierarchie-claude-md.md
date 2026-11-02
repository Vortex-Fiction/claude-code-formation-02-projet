# Exercice 1 — La hiérarchie des `CLAUDE.md`

**Objectif :** donner à Claude les règles du projet, chacune à un seul endroit, au niveau le plus haut où elle s'applique.

## Consignes

1. Lisez le code avec Claude en mode plan : « Quelles conventions ce repo suit-il déjà ? Liste-les, sans rien écrire. »
2. Écrivez trois fichiers, en anglais :
   - `CLAUDE.md` à la racine : ce qui vaut partout (commandes, style, tests, commits) ;
   - `backend/CLAUDE.md` : ce qui ne vaut que pour l'API (où va un service, comment il valide son entrée, `@Inject` explicite) ;
   - `frontend/CLAUDE.md` : ce qui ne vaut que pour l'app Vue (le client `api.ts`, pas de framework CSS).
3. Suivez le même modèle dans chaque fichier : `## Rules`, `## Where things go`, `## Checks before done`, `## Pointers`.
4. Une règle par ligne. Pas de paragraphe, pas d'explication que le code donne déjà.
5. Ne répétez jamais dans un fichier imbriqué une règle du fichier parent.
6. `npm run lint:doc` doit être vert.

## Vérifiez

Fermez Claude, ouvrez une nouvelle session dans `backend/` et demandez :

> Où dois-je mettre un nouveau service pour les catégories de livres, et comment doit-il valider son entrée ?

## Réussi si

- Claude répond avec la règle de `backend/CLAUDE.md`, sans relire tout le code.
- Aucune règle n'apparaît dans deux fichiers.
- Chaque fichier tient sur un écran.
- `npm run lint:doc` est vert.
