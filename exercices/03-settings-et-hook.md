# Exercice 3 — `settings.json` et un hook

**Objectif :** que Claude ne demande plus la permission pour ce qui est sûr, refuse ce qui est dangereux, et formate chaque fichier qu'il modifie.

## Consignes

1. Créez `.claude/settings.json` (partagé dans le repo, pas `settings.local.json`). Lancez `claude` à la racine du repo.
2. Dans `permissions.allow` : `npm test`, `npm run lint`, `npm run lint:doc`.
3. Dans `permissions.deny` : la lecture des fichiers `.env*` (dans tous les dossiers) et `git push --force`.
4. Ajoutez un hook `PostToolUse` sur `Write|Edit` qui lance Prettier sur le fichier modifié.
   - Écrivez le hook en Node (`.claude/hooks/format.mjs`) : il doit marcher sous Windows et macOS.
   - Pas de `jq`, pas de `||` dans la commande. Seule variable : `$CLAUDE_PROJECT_DIR`, le dossier où la session a démarré. Le hook marche ainsi même après un `cd` de Claude pendant la session.
   - Le hook lit le JSON de l'outil sur l'entrée standard et prend `tool_input.file_path`.
5. Testez le hook à la main avant de le brancher : envoyez-lui un JSON sur l'entrée standard.
6. Ouvrez une nouvelle session et vérifiez avec `/permissions` et `/hooks`.

## Vérifiez

- Demandez à Claude : « Lance les tests. » Il ne doit pas demander la permission.
- Cassez la mise en forme d'un fichier (indentation, guillemets), puis demandez à Claude d'y changer une ligne. Le fichier doit ressortir formaté.
- Créez un faux `backend/.env` (`SECRET=test`, il est ignoré par git), puis demandez à Claude : « Lis le fichier `backend/.env`. » L'outil Read doit être refusé.
- Limite : une règle `Read` n'empêche pas `cat backend/.env` dans Bash. Claude vous demandera la permission : refusez. Seul le sandbox bloque aussi Bash.

## Réussi si

- Les trois vérifications passent.
- Le même `settings.json` marche sur le portable Windows ou macOS de votre voisin.
