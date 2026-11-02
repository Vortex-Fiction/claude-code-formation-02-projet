---
marp: true
theme: default
paginate: true
title: Phase 2 — Le projet qui pilote Claude
---

# Phase 2 — Le projet qui pilote Claude

Bien utiliser Claude Code en développement

Repo : `claude-code-formation-02-projet`

---

# Partage de réussite

- Une chose que Claude Code a bien faite depuis la phase 1
- Un prompt en trois parties qui a changé le résultat
- Une session qui a dérapé : quel signe l'a montré ?
- 10 minutes, deux ou trois personnes

---

# Le lendemain, vous saurez…

- écrire des `CLAUDE.md` qui disent à Claude **comment travailler ici**
- séparer ce que dit un `README.md` et ce que dit un `CLAUDE.md`
- écrire une doc qu'un agent lit sans ambiguïté, et la linter
- configurer permissions et hooks dans `settings.json`
- mesurer ce qu'un serveur MCP coûte en contexte

---

# Le problème : chaque session repart de zéro

- Claude ne se souvient pas de la session d'hier
- Sans règles écrites, il devine vos conventions
- Vous répétez les mêmes corrections, session après session
- La solution : **le projet lui-même** donne les règles

<!-- En phase 1, le prompt portait tout. En phase 2, on déplace ce qui ne change pas d'une tâche à l'autre dans le repo. Le prompt ne garde que la tâche. -->

---

# La hiérarchie des `CLAUDE.md`

- `~/.claude/CLAUDE.md` — vos préférences, pour tous les projets
- `CLAUDE.md` à la racine — les règles du repo, partagées par l'équipe
- `backend/CLAUDE.md`, `frontend/CLAUDE.md` — les règles d'un dossier
- Claude charge la racine au démarrage, et un fichier imbriqué quand il travaille dans ce dossier

<!-- Démo dans Vortex : CLAUDE.md racine, backend/CLAUDE.md, backend/libs/CLAUDE.md, e2e/CLAUDE.md. Chaque niveau n'ajoute que ce qui lui est propre. -->

---

# Une règle par ligne, au plus haut niveau

- Une ligne = une règle, à l'impératif
- Mettez la règle dans le `CLAUDE.md` le plus haut où elle s'applique
- Ne répétez jamais une règle du parent dans un fichier imbriqué
- Pas d'explication que le code donne déjà
- Court : chaque ligne coûte du contexte à **chaque** session

<!-- Un CLAUDE.md long est lu, mais pas suivi : les règles se diluent. Si une règle n'a jamais servi, supprimez-la. -->

---

# Le modèle d'un `CLAUDE.md`

```markdown
# backend

## Rules

- Each service validates its own input and throws BadRequestException.

## Where things go

- New entity → src/<entity>/ (input, service, controller, spec).

## Checks before done

- npm test and npm run lint are green.

## Pointers

- backend/README.md — the API reference.
```

<!-- C'est le gabarit de Vortex. Les quatre sections répondent aux quatre questions de Claude : qu'est-ce qui est interdit, où je mets ça, comment je sais que j'ai fini, où je lis la suite. -->

---

# `README.md` vs `CLAUDE.md`

| `README.md`                      | `CLAUDE.md`                           |
| -------------------------------- | ------------------------------------- |
| **ce qu'est** le code            | **comment Claude travaille** ici      |
| but, API publique, usage         | règles, interdits, où vont les choses |
| pour les humains (et les agents) | pour Claude                           |
| ce que le code ne dit pas        | une règle par ligne                   |

<!-- Le README sert aussi à Claude : il le lit quand il découvre une lib. Mais une règle de travail n'a rien à faire dans un README. -->

---

# Une information, un seul fichier

- Chaque information vit dans **un** fichier
- Les autres fichiers donnent son chemin
- Pas de `@fichier.md` pour importer un long document : donnez le chemin
- Le code change → le `README.md` change **dans le même commit**

<!-- Deux copies divergent toujours. Quand elles divergent, Claude choisit au hasard laquelle croire. -->

---

# Écrire pour une machine : STE

- Simplified Technical English : l'anglais de la documentation aéronautique
- Une phrase : 25 mots au plus
- Un paragraphe : 6 phrases au plus
- « must », « do not », l'impératif — jamais « should »
- Un mot = un sens, toujours le même mot pour la même chose

<!-- Pourquoi l'anglais : le code, les messages d'erreur et la plupart des données d'entraînement sont en anglais. Pourquoi STE : un agent prend « should » pour une option. « Must » ne laisse pas de choix. -->

---

# Le glossaire

- Un terme par concept du domaine : `loan`, `borrow`, `borrower`
- Les synonymes sont interdits : pas `rental`, pas `customer`
- Le même mot dans le code, la doc et les prompts
- Dans ce repo : `.vale/styles/STE/Glossary.yml`

<!-- Dans Vortex, le glossaire est un tableau Markdown, et la règle Vale est générée à partir de lui. Un synonyme dans la doc, c'est un concept de plus pour Claude. -->

---

# Vale : linter la doc comme le code

```bash
npm run lint:doc
```

```text
 libs/shared/README.md
 3:1   error  Keep a sentence at 25 words or fewer (STE).
 5:3   error  Use 'borrower', not 'customer' (glossary).
```

- Claude lit les erreurs et corrige seul
- Dans le `CLAUDE.md` : « run `npm run lint:doc` when you change a `README.md` or a `CLAUDE.md` »

<!-- Le lint ferme la boucle : on ne demande pas à Claude d'être rigoureux, on lui donne un outil qui le vérifie. -->

---

# Les commentaires dans le code

- Par défaut : **pas de commentaire**
- Des noms explicites et des fonctions courtes à la place
- Jamais un commentaire qui répète le code
- S'il en faut un : en anglais, et il explique **pourquoi**

<!-- Claude commente beaucoup par défaut. Une ligne dans le CLAUDE.md règle le problème pour toutes les sessions. -->

---

# Des principes ordonnés

1. Correctness over everything
2. Security over simplicity
3. Readability over cleverness
4. Simplicity over reuse
5. Explicit over implicit

<!-- Extrait du CLAUDE.md de Vortex. Une liste de principes ne suffit pas : quand deux principes se contredisent, Claude doit savoir lequel gagne. L'ordre décide. -->

---

# `settings.json` : les permissions

- `.claude/settings.json` — partagé dans le repo
- `.claude/settings.local.json` — le vôtre, pas commité
- `allow` : ce qui ne demande plus la permission (`npm test`)
- `deny` : refusé si le motif correspond (`.env*`, `git push --force`)
- `/permissions` pour voir et modifier

---

# Permissions : les pièges

```json
{
  "permissions": {
    "allow": ["Bash(npm test)", "Bash(npm run lint)"],
    "deny": ["Read(./**/.env*)", "Bash(git push --force *)"]
  }
}
```

- Une commande composée (`cd x && npm test`) doit passer **chaque** partie : une seule non autorisée, et Claude redemande
- `git push --force *` ne couvre pas `git push origin main --force`
- Une seule commande simple par appel : écrivez-le dans le `CLAUDE.md`

<!-- Retour d'expérience Vortex : la règle « One Bash call = one simple command » existe parce que les commandes enchaînées (cd, pipes, redirections) déclenchaient sans cesse des demandes de permission. -->

---

# `deny` : un filet de sécurité, pas une garantie

- Une règle `Bash` compare le **texte** de la commande, tel que Claude l'écrit
- Une autre forme passe à côté : `/usr/bin/git push …`, `sh -c '…'`
- `deny` sur `Read` ou `Edit` couvre les outils de fichiers et `cat`, pas un script Node qui ouvre le fichier lui-même
- Sous Windows, l'outil PowerShell a ses propres règles : `PowerShell(...)`
- Pour un vrai blocage : le sandbox, appliqué par l'OS

<!-- Le deny arrête la commande que Claude écrit d'habitude, ce n'est pas une barrière autour du programme. Autre exemple : « git -C . push --force » échappe à « Bash(git push --force *) ». Sous Windows, Claude Code peut lancer ses commandes avec l'outil PowerShell plutôt que Bash. Un préfixe « cd … && » ne suffit pas à le contourner : Claude Code vérifie chaque sous-commande. Sous Windows avec Git Bash, une règle deny Bash coupe l'outil PowerShell, sauf si CLAUDE_CODE_USE_POWERSHELL_TOOL=1 est défini ou si une règle PowerShell(...) existe : dans ce cas, écrivez aussi la règle PowerShell. Sources : code.claude.com/docs/en/permissions et code.claude.com/docs/en/tools-reference. -->

---

# Les hooks

- Une commande que **Claude Code** lance, pas Claude
- `PostToolUse` sur `Write|Edit` : formater le fichier modifié
- `PreToolUse` : bloquer une action avant qu'elle parte
- `Stop` : vérifier à la fin de chaque réponse
- Écrits en Node : ils marchent sous Windows et macOS

<!-- Une règle dans le CLAUDE.md, Claude peut l'oublier. Un hook, non : c'est le harnais qui l'exécute. Ce qui doit arriver à chaque fois va dans un hook. -->

---

# La barre de statut

- Le modèle, la branche, le contexte utilisé
- `/statusline` pour la configurer
- Voir le contexte monter **avant** que la session dérape

<!-- Lien avec le momentum de la phase 1 : la barre de statut est l'alerte que Claude ne donne pas. -->

---

# La mémoire

- Claude écrit des faits durables dans un dossier mémoire, par projet
- Une correction répétée → une mémoire (« ne jamais lancer `sed -i` »)
- Un index `MEMORY.md` chargé à chaque session
- Relisez-la : une mémoire fausse est une règle fausse

<!-- Démo : les mémoires feedback de Vortex. Chacune a un « Why » : sans la raison, Claude applique la règle à côté. -->

---

# MCP : des outils, et leur coût

- Un serveur MCP ajoute des outils à Claude : GitHub, Drive, une base…
- Les noms des outils sont chargés **à chaque session** ; leurs descriptions aussi, ou au premier besoin quand il y en a beaucoup
- Une réponse d'outil MCP peut être énorme : elle reste dans le contexte
- `/mcp` montre ce qui est actif (connecteurs claude.ai compris), `/context` ce que ça coûte

---

# MCP ou CLI ?

- Une CLI connue (`git`, `gh`, `gcloud`) : Claude la sait déjà, coût zéro au démarrage
- Un MCP : utile quand il n'y a pas de CLI, ou pour un service authentifié
- Activez un MCP **par projet**, pas pour tout
- Mesurez avant de garder

<!-- Exercice 4 : la même tâche avec le MCP GitHub et avec gh. Les chiffres font l'argument. -->

---

# Démo : Vortex

- La hiérarchie des `CLAUDE.md`
- `npm run vortex -- lint doc` sur un README
- Le hook Prettier en `PostToolUse`
- Les mémoires feedback
- `/context` avec et sans MCP

---

# Pratique guidée

```bash
git clone https://github.com/Vortex-Fiction/claude-code-formation-02-projet.git
cd claude-code-formation-02-projet
npm install
npm run back      # terminal 1
npm run front     # terminal 2
claude            # terminal 3
```

On fait ensemble l'exercice 1 : `exercices/01-hierarchie-claude-md.md`

---

# Exercices à faire chez soi

- `exercices/02-readme-ste.md` — un README en STE, vérifié par Vale
- `exercices/03-settings-et-hook.md` — permissions et un hook de formatage
- `exercices/04-cout-du-contexte.md` — MCP ou CLI, chiffres à l'appui
- Une réponse possible : la branche `solution`

Gardez une réussite à partager en ouverture de la phase 3.

---

# La suite : Phase 3 — Spec, plan, code, revue

- Brainstorm → spec → plan → TDD → revue
- Git avec Claude : une tâche, un commit ; les worktrees
- Les sous-agents : quand déléguer, quoi leur dire
- La revue : l'agent se relit, vous relisez l'agent
