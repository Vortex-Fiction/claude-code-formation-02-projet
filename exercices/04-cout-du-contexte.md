# Exercice 4 — Le coût du contexte : MCP ou CLI

**Objectif :** mesurer ce qu'un serveur MCP coûte en tokens avant même la première question, et le comparer à la même tâche avec une CLI.

## Consignes

1. Nouvelle session, aucun serveur MCP actif. Lancez `/context` et notez le total et la part « MCP tools ».
2. Activez un serveur MCP : celui que vous utilisez déjà, ou le serveur GitHub. Nouvelle session, `/context` encore. Notez la différence.
3. Avec le MCP actif, demandez : « Liste les 5 derniers commits de ce repo et les issues ouvertes du repo de la formation. » Lancez `/context` après la réponse.
4. Désactivez le MCP. Nouvelle session. Même demande, mais Claude doit utiliser `git log` et `gh issue list`. `/context` après la réponse.
5. Remplissez le tableau ci-dessous.

| Mesure                          | Sans MCP | Avec MCP |
| ------------------------------- | -------- | -------- |
| Contexte au démarrage           |          |          |
| Contexte après la tâche         |          |          |
| Nombre d'appels d'outils        |          |          |
| Résultat correct ? (oui ou non) |          |          |

## Réussi si

- Le tableau est rempli avec de vrais chiffres.
- Vous savez dire, pour votre serveur MCP, s'il vaut son coût au démarrage de chaque session.
- Le MCP n'est actif que dans les projets qui en ont besoin (`/mcp`, ou la portée `project` plutôt que `user`).
