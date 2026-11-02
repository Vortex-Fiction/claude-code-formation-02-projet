# backend

## Rules

- esbuild bundles the API and does not emit decorator metadata. Give each constructor parameter an explicit `@Inject(Class)` or `@Inject(TOKEN)`.
- Each service validates its own input with the helpers of `src/validation.ts` and throws `BadRequestException`. Do not add a `ValidationPipe` or `class-validator`.
- A controller passes the raw `@Body() body: unknown` to its service.
- Throw `NotFoundException` for a missing row and `ConflictException` for a forbidden state.
- Test a service with a real in-memory database: `createDatabase(':memory:')`. Do not use mocks.

## Where things go

- New entity → `src/<entity>/`: `<entity>.input.ts`, `<entity>.service.ts`, `<entity>.controller.ts`, `<entity>.service.spec.ts`.
- New table → `src/db/schema.ts` and `src/db/create-tables.ts`.
- New module → register it in `src/app.module.ts`.

## Checks before done

- `npm run typecheck --workspace backend` is green.
