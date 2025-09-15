- **Purpose**: Issues a new access token using the `refreshToken` cookie. If invalid/expired/missing, clears both tokens.

### Request flow
1. **Read cookie**: Parses `refreshToken` from `req.headers.cookie`.
2. **Refresh**:
   - Calls `fetchAccessToken(refreshToken)` from `utils/jwt`.
   - On success, sets a fresh `accessToken` cookie via `buildAccessTokenCookie` and returns 200 "OK".
3. **Error handling**:
   - If `RefreshTokenNotExistsError` | `RefreshTokenLostError` | `RefreshTokenExpiredError`:
     - Returns 401 and sets sign-out cookies via `buildSignOutCookie` (expires both `accessToken` and `refreshToken`).
   - Otherwise returns 500 "Unkonwn Error" (typo preserved).

### Cookies set
- Success: `Set-Cookie: accessToken=...` (httpOnly, SameSite=lax, secure in prod, path=/, domain in prod).
- Failure (known refresh errors): `Set-Cookie` for both `accessToken` and `refreshToken` to expired values.

### Dependencies
- `utils/jwt`: `fetchAccessToken`, `buildAccessTokenCookie`, `buildSignOutCookie`
- `utils/errors`: custom refresh token error classes
- `cookie`: for parsing cookies
- Next.js API types

### Notes
- This route relies on server-side Hasura admin access inside `fetchAccessToken`.
- The refresh token cleanup mutation in `fetchAccessToken` is fire-and-forget.