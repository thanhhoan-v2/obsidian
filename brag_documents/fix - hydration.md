```table-of-contents
title: 
style: nestedList # TOC style (nestedList|nestedOrderedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 0 # Include headings up to the specified level
include: 
exclude: 
includeLinks: true # Make headings clickable
hideWhenEmpty: false # Hide TOC if no headings are found
debugInConsole: false # Print debug info in Obsidian console
```
---
## fix: hydration

### What Caused the Hydration Issues

#### Icon Component

**Icon** component rendered differently on server vs client due to Suspense-like behavior and side-effects/mutations during render.

#### NavMenu Component

**NavMenu** produced a different DOM structure between SSR and CSR (e.g., rendering DnD list only on client without a consistent SSR fallback).

#### ChromeBanner Component

**ChromeBanner** used browser-detection directly during render, causing SSR/CSR divergence (`navigator` availability differs).

#### ToggleButton Component

**ToggleButton** contained nested interactive elements (button-inside-button) which is invalid HTML and can change the DOM tree between server and client.

### The Fixes Applied

#### 🎨 Icon Component

**Removed** Suspense usage and render-time mutations; now directly renders the SVG component and uses `suppressHydrationWarning` to avoid benign attribute diffs.

```typescript
// File: components/core/Icon.tsx
```

##### 📦 IconBox Component

**Simplified** to match the new Icon interface.

```typescript
// File: components/core/IconBox.tsx
```

##### 🧭 NavMenu Component

**Enforced** a consistent SSR/CSR structure:

- Always renders a basic list on SSR and initial client render (`!isClient`)
- Switches to the DnD-enhanced list only after mount (`isClient === true`), keeping markup stable across the hydration boundary

```typescript
// File: components/core/templates/NavMenu.tsx
```

##### 🌐 ChromeBanner Component

**Moved** browser detection into `useEffect` and gates UI behind client state (`isChromePC`), preventing server-only renders from diverging from client.

```typescript
// File: components/domain/chrome-extension/ChromeBanner.tsx
```

##### 🔘 ToggleButton Component

**Refactored** to a single button element with a non-button clickable wrapper for the right icon, removing nested buttons and stabilizing the DOM tree.

```typescript
// File: components/core/buttons/ToggleButton.tsx
```

#### Why This Works

##### ✅ Deterministic Rendering

- **No side effects or data mutations during render** → deterministic SSR output
- **Identical initial markup** for SSR and first client render → no hydration mismatch

##### ⚡ Client-First Behaviors

- **Client-only behaviors** (DnD, browser detection) are gated behind `useEffect` and local state
- **Valid, non-nested interactive elements** prevent unexpected DOM differences and accessibility issues

#### 📁 Affected Files

| Component        | File Path                                             |
| ---------------- | ----------------------------------------------------- |
| **Icon**         | `components/core/Icon.tsx`                            |
| **IconBox**      | `components/core/IconBox.tsx`                         |
| **NavMenu**      | `components/core/templates/NavMenu.tsx`               |
| **ToggleButton** | `components/core/buttons/ToggleButton.tsx`            |
| **ChromeBanner** | `components/domain/chrome-extension/ChromeBanner.tsx` |


---

## fix(graphql): resolve session field not found in query_root error

### What Caused the Error

You had a session query defined under Hasura operations (`graphql/operations/session.graphql`). Codegen treats those operations as targeting Hasura's schema (Query root: `query_root`). Hasura does not have a `session` field on `query_root`, so requests compiled against Hasura failed with **"field 'session' not found in type: query_root."**

At the same time, you actually expose `session` via a separate local GraphQL Yoga endpoint at `/api/graphql` (custom schema), not via Hasura. Mixing the operation into Hasura's ops made Apollo/codegen aim at the wrong schema.

### The Fix Applied in the Commit

#### Removed the Hasura-scoped operation

- Deleted `graphql/operations/session.graphql` (so codegen no longer expects `session` on Hasura)

#### Introduced a dedicated local query path and client

- **New hook** `hooks/useLocalSession.ts` that queries `session` against `/api/graphql` using its own ApolloClient

#### `graphql.ts`

```typescript
type Query {
  session: Session!
}
...
Query: {
  session: async (_, args, ctx) => {
    const userId = ctx.req.headers["x-hasura-user-id"] as string;
    ...
    // fetch MixUser from Hasura via getClient(...)
    return { id: uuid(), userId, me };
  },
}
```

#### `useLocalSession.ts`

```typescript
const LOCAL_SESSION_QUERY = gql`
  query LocalSession {
    session { 
      id 
      userId 
      me { 
        id 
        email 
        name 
        ... 
      } 
    }
  }
`;

const createLocalClient = () => new ApolloClient({ 
  link: new HttpLink({ 
    uri: "/api/graphql", 
    credentials: "include" 
  }), 
  cache: new InMemoryCache(), 
  credentials: "include" 
});

export const useLocalSession = () => useQuery<LocalSessionQueryResult>(
  LOCAL_SESSION_QUERY, 
  { 
    client: getLocalClient(), 
    errorPolicy: "all" 
  }
);
```

#### Additional Changes

- Updated components/pages to replace `useSessionQuery` with `useLocalSession`
- Cleaned up generated types (`types/generated.d.ts`, `graphql/generated.tsx`) to remove the conflicting session operation tied to Hasura

### Why This Works

- **Hasura schema stays clean** (no fake session field)
- The session query now targets the correct schema (local Yoga server) while still resolving `me` by fetching from Hasura under the hood within the resolver

### Side Effects/Notes

> ⚠️ **Breaking Changes**
> 
> - Any code that previously used the Hasura-generated `useSessionQuery` must use `useLocalSession`
> - Server-side usages should call the local endpoint or a shared helper mirroring `useLocalSession` semantics

✅ **Benefits**

- This separation prevents ApolloErrors stemming from schema mismatch
- Resolves hydration mismatches related to session data loading

----

## fix(auth): prevent signin modal for authenticated users

### What caused the error

Layout showed SignInModal whenever the URL hash was 'signin', without checking authentication state.

### The fix applied in the commit

Gate modal rendering with '!session?.me' so it only appears when the user is not logged in.

### Why this works

Authenticated sessions now bypass the modal despite the 'signin' hash, matching the intended behavior already used elsewhere (e.g., header logic).

### Side effects/affected files/notes

Affected file: `components/core/Layout.tsx`. No server/API changes. Signup and other modals unchanged. Relies on `AuthContext` session being available; no impact on routing or URL hash behavior.