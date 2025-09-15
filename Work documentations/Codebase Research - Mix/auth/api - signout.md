- **Purpose**: Logs the user out by clearing auth cookies and redirecting to `/`.

### Flow
1. Calls `buildSignOutCookie()` from `utils/jwt` to create expired `accessToken` and `refreshToken` cookies.
2. Sets `Set-Cookie` header with those expired cookies.
3. Sends a `302` redirect to `/`.

### Why HTTP (not GraphQL)
- As noted in the comment, sign-out is handled via an HTTP endpoint so cookies can be reliably cleared and propagated to the client (`Set-Cookie` headers), aligning with Hasura guidance.

### Dependencies
- **`utils/jwt`**: `buildSignOutCookie`
- **Next.js**: API route types (`NextApiRequest`, `NextApiResponse`)