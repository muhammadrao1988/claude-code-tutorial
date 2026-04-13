---
description: Scaffold a new REST API endpoint with router, schema, and test
---

I need a new API endpoint. Ask me:
1. What resource? (e.g., products, users, orders)
2. What operation? (e.g., list, create, update, delete)

Then create these files following the existing patterns in the codebase:

1. **Router** in `app/routers/` -- the endpoint handler
2. **Schema** in `app/schemas/` -- Pydantic request/response models
3. **Test** in `tests/` -- at least 2 test cases (success + error)

Follow the patterns you see in existing router and schema files. Run the tests after creating them.
