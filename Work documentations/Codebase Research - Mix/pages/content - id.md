- **What this page is**: A Next.js dynamic route for `/contents/[id]` that never renders UI. It uses server-side logic to record analytics/history and then redirects.

- **High-level flow**:
  1. Read `id` from the route params.
  2. Build an authenticated GraphQL client using request cookies.
  3. Try to fetch the current user (`session.me`); ignore errors.
  4. Fetch the content by `id`.
  5. Fire-and-forget: upsert a view history record for the user.
  6. Using an admin client (with `HASURA_ADMIN_SECRET`), insert a `mix_content_view` row for analytics, identifying the viewer by user id, IP, or user-agent.
  7. Redirect:
     - If `content.isoriginal` is true, redirect to internal route `/contents/original/${content.id}`.
     - Otherwise, redirect to the external `content.originalurl`.

- **Key details**:
  - The page component returns `null` because users are always redirected in `getServerSideProps`.
  - IP is derived from `x-forwarded-for` (first value) or `req.socket.remoteAddress` as a fallback.
  - The upsert of history uses the user’s identity if available; otherwise, tracking falls back to IP or user-agent.
  - The admin mutation is run with elevated privileges via `HASURA_ADMIN_SECRET`.
  - Type safety relies on generated GraphQL types (`graphql/generated`).
  - Errors are thrown early for missing `id`, missing `content`, or invalid `originalurl`.

- **Why two clients?**
  - **User client** (`cookie`-based): respects the viewer’s session for personalized queries and mutations.
  - **Admin client** (`x-hasura-admin-secret`): used only for privileged analytics insert.

- **Non-blocking writes**:
  - History upsert is intentionally not awaited to avoid delaying the redirect.
  - The admin view insert is awaited only as a mutation call is issued, but its result isn’t used; the redirect proceeds.

- **Redirect outcomes**:
  - Internal: `/contents/original/[id]` for original content handled within the app.
  - External: `content.originalurl` for non-original content.

- **Why `Page` returns `null`**:
  - This component is never rendered client-side since a redirect response is always returned from the server.

- **Identifiers used for tracking**:
  - `me?.id || ip || userAgent` in that priority order, ensuring a best-effort unique identifier.