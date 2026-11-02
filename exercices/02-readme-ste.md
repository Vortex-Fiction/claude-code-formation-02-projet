# Exercice 2 — Un README en STE

**Objectif :** écrire un `README.md` qu'une personne et un agent lisent sans ambiguïté, et laisser Vale le vérifier.

## Le contexte

`libs/shared` contient les types partagés entre l'API et l'app Vue. Il n'a pas de `README.md`.

## Consignes

1. Demandez à Claude d'écrire `libs/shared/README.md`, en anglais, en Simplified Technical English (STE) :
   - ce qu'est la lib, en une phrase ;
   - son API publique (les types exportés) ;
   - comment l'importer depuis `backend` et `frontend`.
2. N'écrivez que ce que le code ne dit pas. Pas la liste des champs de chaque type : le code la donne.
3. Lancez `npm run lint:doc`. Donnez les erreurs à Claude et laissez-le corriger.
4. Comparez avec votre `CLAUDE.md` de l'exercice 1 : une information ne doit vivre que dans un seul des deux.

## Rappel STE

- Une phrase : 25 mots au plus. Un paragraphe : 6 phrases au plus.
- Pas de « should » : « must », « do not » ou l'impératif.
- Pas de « etc. ».
- Les mots du glossaire : `loan`, `borrow`, `return`, `borrower`, `author`, `book` (`.vale/styles/STE/Glossary.yml`).

## Réussi si

- `npm run lint:doc` est vert.
- Le README dit ce qu'est la lib ; le `CLAUDE.md` dit comment Claude doit y travailler. Aucune ligne répétée entre les deux.
