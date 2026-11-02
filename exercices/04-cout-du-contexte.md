# Exercice 4 — Le coût du contexte : MCP ou CLI

**Objectif :** mesurer ce qu'un serveur MCP coûte en tokens, au démarrage et pendant la tâche, et le comparer à la même tâche avec une CLI.

## Consignes

1. Lancez `/mcp` : notez les serveurs déjà actifs, y compris les connecteurs claude.ai. Désactivez-les tous.
2. Nouvelle session. Lancez `/context` et notez le total et la part « MCP tools ».
3. Activez un serveur MCP : celui que vous utilisez déjà, ou le serveur GitHub. Nouvelle session, `/context` encore. Notez la différence. Si le serveur a beaucoup d'outils, Claude Code peut différer leurs descriptions : seuls les noms sont chargés, et le coût arrive au premier appel.
4. Avec le MCP actif, demandez : « Liste les 5 derniers commits de ce repo et les issues ouvertes du repo de la formation. » Lancez `/context` après la réponse.
5. Désactivez le MCP. Nouvelle session. Même demande, mais Claude doit utiliser `git log` et `gh issue list`. `/context` après la réponse.
6. Remplissez le tableau ci-dessous.

| Mesure                          | Sans MCP | Avec MCP |
| ------------------------------- | -------- | -------- |
| Contexte au démarrage           |          |          |
| Contexte après la tâche         |          |          |
| Nombre d'appels d'outils        |          |          |
| Résultat correct ? (oui ou non) |          |          |

## Réussi si

- Le tableau est rempli avec de vrais chiffres.
- Vous savez dire, pour votre serveur MCP, s'il vaut son coût, au démarrage et à chaque appel.
- Le MCP n'est actif que dans les projets qui en ont besoin : portée `local` (par défaut, pour vous seul dans ce projet) ou `project` (partagé dans `.mcp.json`), plutôt que `user`.
