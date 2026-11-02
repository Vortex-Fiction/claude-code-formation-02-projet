# Exercice 3 — `settings.json` et un hook

**Objectif :** que Claude ne demande plus la permission pour ce qui est sûr, refuse ce qui est dangereux, et formate chaque fichier qu'il modifie.

## Consignes

1. Créez `.claude/settings.json` (partagé dans le repo, pas `settings.local.json`).
2. Dans `permissions.allow` : `npm test`, `npm run lint`, `npm run lint:doc`.
3. Dans `permissions.deny` : la lecture des fichiers `.env*` et `git push --force`.
4. Ajoutez un hook `PostToolUse` sur `Write|Edit` qui lance Prettier sur le fichier modifié.
   - Écrivez le hook en Node (`.claude/hooks/format.mjs`) : il doit marcher sous Windows et macOS.
   - Pas de `jq`, pas de `||` dans la commande. Seule variable : `$CLAUDE_PROJECT_DIR`, pour que le hook marche même si la session démarre dans `backend/`.
   - Le hook lit le JSON de l'outil sur l'entrée standard et prend `tool_input.file_path`.
5. Testez le hook à la main avant de le brancher : envoyez-lui un JSON sur l'entrée standard.
6. Ouvrez une nouvelle session et vérifiez avec `/permissions` et `/hooks`.

## Vérifiez

- Demandez à Claude : « Lance les tests. » Il ne doit pas demander la permission.
- Cassez la mise en forme d'un fichier (indentation, guillemets), puis demandez à Claude d'y changer une ligne. Le fichier doit ressortir formaté.
- Demandez à Claude : « Lis le fichier `.env`. » Il doit refuser.

## Réussi si

- Les trois vérifications passent.
- Le même `settings.json` marche sur le portable Windows ou macOS de votre voisin.
