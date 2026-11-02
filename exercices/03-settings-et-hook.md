# Exercice 3 — `settings.json` et un hook

**Objectif :** que Claude ne demande plus la permission pour ce qui est sûr, refuse ce qui est dangereux, et formate chaque fichier qu'il modifie.

## Consignes

1. Créez `.claude/settings.json` (partagé dans le repo, pas `settings.local.json`). Lancez `claude` à la racine du repo.
2. Dans `permissions.allow` : `npm test`, `npm run lint`, `npm run lint:doc`.
3. Dans `permissions.deny` : la lecture des fichiers `.env*` (dans tous les dossiers) et `git push --force`.
4. Ajoutez un hook `PostToolUse` sur `Write|Edit` qui lance Prettier sur le fichier modifié.
   - Écrivez le hook en Node (`.claude/hooks/format.mjs`) : il doit marcher sous Windows et macOS.
   - Utilisez la forme exec : `"command": "node"` et `"args": ["${CLAUDE_PROJECT_DIR}/.claude/hooks/format.mjs"]`. Pas de shell, donc le même JSON marche sous Windows et macOS. `${CLAUDE_PROJECT_DIR}` est le dossier où la session a démarré : le hook marche même après un `cd` de Claude.
   - Le hook lit le JSON de l'outil sur l'entrée standard et prend `tool_input.file_path`.
5. Testez le hook à la main avant de le brancher : envoyez-lui un JSON sur l'entrée standard.
6. Ouvrez une nouvelle session et vérifiez avec `/permissions` et `/hooks`.

## Vérifiez

- Demandez à Claude : « Lance les tests. » Il ne doit pas demander la permission.
- Cassez la mise en forme d'un fichier (indentation, guillemets), puis demandez à Claude d'y changer une ligne. Le fichier doit ressortir formaté.
- Créez un faux `backend/.env` (`SECRET=test`, il est ignoré par git), puis demandez à Claude : « Lis le fichier `backend/.env`. » L'outil Read doit être refusé.
- Demandez ensuite : « Affiche `backend/.env` avec `cat`. » La commande doit être refusée : une règle `Read` couvre aussi les commandes de fichiers que Claude Code reconnaît dans Bash (`cat`, `head`, `tail`, `sed`, `tee`) et les redirections comme `< backend/.env`.
- Limite : la règle ne couvre pas une commande qui lit le fichier sans le nommer, comme `grep -r SECRET .` lancé depuis `backend/`, ni un script Node qui ouvre le fichier lui-même. Pour bloquer tous les processus, il faut le sandbox, activé et configuré (macOS, Linux, WSL 2 ; pas sous Windows natif).

## Réussi si

- Les quatre vérifications passent.
- Le même `settings.json` marche sur le portable Windows ou macOS de votre voisin.
