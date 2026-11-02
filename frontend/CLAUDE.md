# frontend

## Rules

- Call the API only through the functions of `src/api.ts`. Do not call `fetch` in a component.
- Use relative `/api` paths. The Vite proxy sends them to the API on port 3000.
- Show an API error with its message in the error banner of `App.vue`.
- Do not add a CSS framework or a UI component library.

## Where things go

- New page → `src/components/<Name>List.vue` and a tab in `src/App.vue`.
- New API call → a typed function in `src/api.ts`.

## Checks before done

- Open http://localhost:5173 and use the changed page once.
