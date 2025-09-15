- **Purpose**: Automatically refreshes an expired Hasura JWT and retries the failed GraphQL operation; if refresh fails, signs the user out.

### Flow
- On GraphQL error:
  - Checks `isHasuraJWTExpired(graphQLErrors)`.
  - If expired:
    - Calls `GET /api/refresh` via `axios` to issue a new access token cookie.
    - Uses `fromPromise(...).flatMap(() => forward(operation))` to retry the original operation once refresh is done.
    - If refresh returns 401, calls `GET /api/signout` and then reloads the page.
- On network error:
  - Logs the error to console.

### Notable details
- **`fromPromise` + `flatMap`**: Bridges the async refresh into the Apollo link chain, ensuring the original operation is retried after success.
- **Cookie-based auth**: Refresh/signout endpoints manage `Set-Cookie`; no tokens handled in JS directly.
- **Expired detection**: Delegated to `isHasuraJWTExpired` in `utils/jwt` (looks for `extensions.code === "invalid-jwt"`).