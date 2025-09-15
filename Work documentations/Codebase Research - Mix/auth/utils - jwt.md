Here’s what `utils/jwt.ts` does:

- generateAccessToken(userId)
  - Creates a signed JWT for Hasura with claims:
    - `x-hasura-allowed-roles`: ["guest", "user"]
    - `x-hasura-default-role`: "user"
    - `x-hasura-user-id`: userId
  - Secret: `process.env.HASURA_GRAPHQL_JWT_SECRET`
  - Expiry: production 15m, else 5s

- fetchAccessToken(refreshToken?)
  - Validates presence/type of refresh token; throws `RefreshTokenNotExistsError` if missing.
  - Queries Hasura (via `getClient` and admin secret header) using `MixUserByRefreshTokenDocument` to find the user by refresh token.
  - If no user, throws `RefreshTokenLostError`.
  - If `refreshTokenExpiry` is missing or past (`new Date(user.refreshTokenExpiry + "Z") < new Date()`):
    - Clears refresh token fields via `UpdateMeDocument` mutation (best-effort, not awaited).
    - Throws `RefreshTokenExpiredError`.
  - Otherwise returns a fresh access token using `generateAccessToken(user.id)`.

- Cookie builders
  - getDefaultCookieOptions(): httpOnly, secure in prod, SameSite=lax, domain set to `mix.day` in prod, path `/`, default expires in 3 days.
  - buildAccessTokenCookie(accessToken): returns a `Set-Cookie` header array for `accessToken`.
  - buildRefreshTokenCookie(refreshToken, refreshTokenExpiry): returns cookie for `refreshToken` with provided expiry.
  - buildSignOutCookie(): returns expired cookies for both `accessToken` and `refreshToken`.

- isHasuraJWTExpired(error: GraphQLErrors)
  - Checks if any GraphQL error has `extensions.code === "invalid-jwt"`.

Key dependencies and assumptions:
- Uses Apollo-generated documents from `graphql/generated`.
- Uses `getClient` from `utils/client` with the Hasura admin secret for server-side queries.
- Custom errors from `utils/errors`: `RefreshTokenNotExistsError`, `RefreshTokenLostError`, `RefreshTokenExpiredError`.