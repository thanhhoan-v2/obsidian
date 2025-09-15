- **Purpose**: Stores all query/mutation documents used by codegen.
- **How it’s wired**: `codegen.yml` points `documents: "graphql/**/*.graphql"`, generating typed hooks and types into `graphql/generated.tsx`.
- **Related folder**: Reusable fragments live in `graphql/fragments/` and are spread in operations (e.g., `...mix_content`).

### What’s inside
- **Queries** (examples): `mixContent.graphql`, `mixContentByPk.graphql`, `search.graphql`, `session.graphql`, `order.graphql`, `mixClassCourse*.graphql`, etc.
- **Mutations** (examples): `insertBookmark.graphql`, `deleteBookmark.graphql`, `upsertContentHistory.graphql`, `insertOrderOne.graphql`, `updateOrderByPk.graphql`, `upsertUserInterestTags.graphql`, etc.
- Some files include comments or view-based queries (e.g., `mixContentViews.graphql` for “popular last 24h”).

### How it’s consumed
- Run graphql-codegen (via your project scripts) to produce typed Apollo hooks in `graphql/generated.tsx`.
- Components import the generated hooks (not the `.graphql` files directly) and pass strongly-typed variables.

If you did intend a real import alias like `@operations/...`, we can add a path alias in `tsconfig.json` and corresponding webpack/Next alias, but currently none is configured.