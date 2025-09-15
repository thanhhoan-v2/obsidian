### High-level overview
- **Stack**: Next.js (pages router) + React 18 + Apollo Client (GraphQL) + GraphQL Yoga API route, Emotion for styling, Storybook for UI, Toss Payments, Google OAuth.
- **Purpose**: Marketing content/media site with user accounts, payments, and a Chrome extension build target.

### Project structure
- **App shell**
  - `pages/_app.tsx`: Initializes providers (theme, Google OAuth, Apollo Client), global `<Layout>`, SEO/meta.
  - `pages/_document.tsx`: Injects FB Pixel noscript.
  - `components/`: Design system and domain components (buttons, inputs, modals, templates, payments, contents, etc.).
  - `contexts/`: Lightweight global contexts (`AuthContext`).
	  - `hooks/`: Reusable React hooks (auth, bookmarks, scrolling, window size, etc.).
  - `styles/`: Global CSS, theme tokens, animations, z-index.
  - `public/` and `extension/`: Static assets. `extension/` hosts a prebuilt Next export for the Chrome extension.
- **Data layer**
  - `pages/api/graphql.ts`: GraphQL Yoga server endpoint.
  - `graphql/`: GraphQL fragments, operations, and generated hooks/types (`graphql/generated.tsx` from codegen).
  - `utils/client.ts`: SSR-friendly Apollo Client for server-side queries.
  - `utils/jwt.ts`: Access/refresh token helpers, cookie builders, and JWT expiry checks.
- **Routing**
  - `pages/`: All user-facing routes (home, search, content detail, course detail, mypage, payments).
  - `pages/api/`: REST-ish Next API routes for auth, payments, misc.
- **Build/tooling**
  - `package.json`: Scripts for dev/build/start, GraphQL codegen, Storybook, and extension build.
  - `codegen.yml`: GraphQL Code Generator configuration.
  - `next.config.js`: Next configuration.
  - Copilot configs under `copilot/` for AWS deploys.

### Key files to know
- `pages/_app.tsx`:
```1:249:pages/_app.tsx
function MyApp({ ... }) {
  const client = useMemo(() => {
    const cache = new InMemoryCache();
    if (pageProps?.__APOLLO_CACHE_DATA__) {
      cache.restore(pageProps.__APOLLO_CACHE_DATA__);
    }
    return createClient(cache);
  }, []);
  ...
  <MixThemeProvider>
    <GoogleOAuthProvider clientId={process.env.NEXT_PUBLIC_GOOGLE_CLIENT_ID}>
      <ApolloProvider client={client}>
        <Layout browserName={browserName} deviceType={deviceType}>
          <Component {...pageProps} />
        </Layout>
      </ApolloProvider>
    </GoogleOAuthProvider>
  </MixThemeProvider>
}
```
- `pages/api/graphql.ts`:
```1:58:pages/api/graphql.ts
const server = createServer({
  schema: {
    typeDefs: [DateTimeTypeDefinition, typeDefs],
    resolvers: {
      Query: {
        session: async (_, args, ctx) => {
          const userId = ctx.req.headers["x-hasura-user-id"] as string;
          if (!userId) throw new Error("userId not exists!");
          const value = ctx.req.headers.cookie;
          if (typeof value !== "string") throw new Error("cookie is not exists!");
          return { id: uuid(), userId };
        },
      },
      Mutation: { ok() { return "ok"; } },
    },
  },
  endpoint: "/api/graphql",
  cors: { credentials: true },
});
```
- `graphql/generated.tsx`: Generated Apollo hooks and types from `graphql/operations/*.graphql`.
- `utils/jwt.ts`: Token/cookie helpers used for SSR refresh and client-side refresh handling.
- `pages/api/refresh.ts`, `pages/api/signout.ts`: Auth support endpoints.
- `pages/payments/*` and `pages/api/payments/*`: Payments UI and APIs.
- `components/providers/MixThemeProvider.tsx`: Theme provider setup.
- `components/core/Layout.tsx`: App layout frame.

### How data flows

- **GraphQL/Apollo**
  - Client is created in `pages/_app.tsx` with an `HttpLink` to `process.env.NEXT_PUBLIC_GRAPHQL_ENDPOINT` and `credentials: include` for cookies.
  - An `errorLink` intercepts Hasura JWT expiry errors; it calls `/api/refresh` to refresh the access token and retries the failed operation. On 401, it calls `/api/signout` and reloads.
  - On SSR, `MyApp.getInitialProps` optionally restores an access token if only a refresh token exists and fetches the session with `getClient(...)` to gate onboarding redirects.

- **Authentication**
  - Cookies: `refreshToken` and `accessToken` tracked server-side via `utils/jwt.ts` helpers.
  - SSR flow in `MyApp.getInitialProps`:
    - If `refreshToken` and no `accessToken`: fetch new access token and set cookie; otherwise clear cookies.
    - If both exist: query `SessionDocument`; if profile is incomplete (`position`/`career` are null), redirect to home with `#signup-form`.
  - Client flow in Apollo `errorLink`: On token expiry, transparently refresh and retry; otherwise sign out.

- **Server-side GraphQL**
  - `pages/api/graphql.ts` is a Yoga-based endpoint used for some server-side queries (e.g., session); it expects `x-hasura-user-id` and cookies to be present. Main app GraphQL likely points to a Hasura endpoint via `NEXT_PUBLIC_GRAPHQL_ENDPOINT` (the `watch` script triggers Hasura remote schema reloads).

- **Payments**
  - UI pages: `pages/payments/start.tsx`, `pages/payments/success.tsx`, `pages/payments/fail.tsx`, and `pages/payments/form/index.tsx`.
  - API routes under `pages/api/payments/*` integrate with Toss Payments (`utils/toss-payments.ts`), creating payments, confirming them, and handling success/failure callbacks.

- **Chrome extension mode**
  - If `process.env.BUILD_TARGET === "extension"`, `axios.defaults.baseURL` is set to `https://mix.day` in `_app`.
  - Extension assets live under `extension/` with `manifest.json` and a prebuilt `next/` directory.
  - Build script: `npm run build-extension` executes `scripts/build_extension.sh`.

- **UI theming and components**
  - Global theming via `MixThemeProvider` and Emotion tokens in `styles/theme.ts`.
  - A rich component library under `components/core/*` (banners, buttons, inputs, modals, tabs, templates).
  - Domain components for chrome extension, contents, classes, and payments under `components/domain/*`.

- **Analytics and 3rd parties**
  - FB Pixel injected in `_document` and conditionally in `_app`.
  - Google OAuth via `@react-oauth/google` provider.
  - Crisp chat injected when not in extension mode.

### Notable conventions and patterns
- **Pages router** (no `app/` directory usage): classic Next routing with SSR/CSR mix.
- **SSR auth guard** happens centrally in `MyApp.getInitialProps` rather than per-page guards.
- **GraphQL codegen**: Author GraphQL in `graphql/operations/*.graphql`; generate typed hooks into `graphql/generated.tsx`.
- **Cookie-based auth** with Hasura-compatible JWTs; automatic refresh and retry pipeline.

### Where to start exploring
- For pages: check `pages/index.tsx`, `pages/contents/[id].tsx`, `pages/course/[id].tsx`, `pages/mypage/*`.
- For data: inspect queries/mutations in `graphql/operations/*` and their usage in pages/components; see the generated hooks in `graphql/generated.tsx`.
- For auth: `utils/jwt.ts`, `pages/api/refresh.ts`, `pages/api/signout.ts`, and the Apollo `errorLink` in `_app.tsx`.
- For payments: `utils/toss-payments.ts`, `pages/api/payments/*`, and `pages/payments/*`.

If you want, I can diagram the request lifecycle (SSR + client retry) or map specific pages to the GraphQL operations they use.