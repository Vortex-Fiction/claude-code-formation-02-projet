# Build log

This repo was built with Claude Code from a written plan. Each entry is the instruction given for one task.

## Task 1 — Start phase 2 from the phase 1 app

> Copy the phase 1 tree without its history. Remove `CLAUDE.md`, the exercises, the slides and the build log. Write a failing test first: returning a loan twice is rejected with 409 and keeps the first return date. Then add the guard in `returnLoan`. `main` has no `CLAUDE.md` and no `.claude/` folder.

## Task 2 — Add the Vale STE documentation lint

> Copy the five STE rules of Vortex (sentence length, paragraph length, should, etc., glossary) and adapt the glossary to books, authors and loans. Install Vale with npm (`@vvago/vale`), as Vortex does, so that `npm install` is enough. `npm run lint:doc` checks only the `README.md` and `CLAUDE.md` files and skips `node_modules`. Check that a bad sentence fails the lint.
