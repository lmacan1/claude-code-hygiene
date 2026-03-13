# API / Backend Rules

Append these to universal.md for REST/GraphQL API projects, backend services, or server-side applications.

## .claudeignore

Add to your project's `.claudeignore`:

```
migrations/
seeds/
db/dumps/
swagger-output.json
openapi-generated/
coverage/
```

## Routes

- Follow the existing URL naming convention. Check 2-3 existing routes before creating new ones.
- Check for auth middleware patterns — most projects apply auth at the router level, not per-handler.
- Don't duplicate validation logic. Check for existing validation schemas/middleware before writing new validators.

## Database

- Read models/schema definitions, not migration files. Migrations are history — models are current state.
- Check which ORM/query builder the project uses before writing queries. Don't mix patterns.
- Check the project's migration tool (`knex migrate`, `prisma migrate`, `alembic`, `rails db:migrate`) before creating migrations manually.

## Testing

- Scope tests to the module you changed. Don't run the full test suite for a single endpoint change.
- Prefer unit tests for new business logic. Use integration tests only for route-level behavior.
- Check for existing test fixtures, factories, or seed helpers before creating test data inline.
- Don't mock the database unless the project already does. If there's a test database setup, use it.
