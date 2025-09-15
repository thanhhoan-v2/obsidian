[[Asxits/Companies/TalentGetGo/index]]
## Films Page Dataflow Analysis

### **Step 1: Data Fetching (Server-Side)**
**Location**: `src/pages/films/index.tsx` - `getStaticProps` function

```typescript
export const getStaticProps: GetStaticProps = async () => {
  try {
    // 1. Execute GraphQL query using Apollo Client
    const { data } = await client.query({
      query: GET_ALL_FILMS,
    });

    // 2. Extract films from GraphQL response
    const filmsFromEdges: FilmEdge[] = data?.allFilms?.edges || [];

    // 3. Transform data to match component interface
    const graphqlFilms = filmsFromEdges.map(
      (edge: FilmEdge, index: number) => ({
        id: (index + 1).toString(),
        title: edge.node.title,
        director: edge.node.director,
        releaseDate: edge.node.releaseDate,
        openingCrawl: edge.node.openingCrawl,
      })
    );

    return {
      props: { films: graphqlFilms || [] },
      revalidate: 86400, // ISR: Revalidate once per day
    };
  } catch (error) {
    console.error('Error fetching films with GraphQL:', error);
    return {
      props: { films: [] },
      revalidate: 3600, // Retry in 1 hour if error
    };
  }
};
```

**Explanation**: 
- Uses Next.js `getStaticProps` for Static Site Generation (SSG) with Incremental Static Regeneration (ISR)
- Apollo Client executes the `GET_ALL_FILMS` GraphQL query against the SWAPI GraphQL endpoint
- Data is transformed from GraphQL edges format to a simplified interface for components
- Error handling returns empty array and shorter revalidation time

### **Step 2: GraphQL Query Definition**
**Location**: `src/lib/queries.ts`

```typescript
export const GET_ALL_FILMS = gql`
  query GetAllFilms {
    allFilms {
      edges {
        node {
          title
          director
          releaseDate
          openingCrawl
        }
      }
    }
  }
`;
```

**Explanation**: 
- Defines the GraphQL query structure using Apollo Client's `gql` tag
- Requests only the necessary fields for the films list page
- Uses the SWAPI GraphQL schema structure with edges/nodes pattern

### **Step 3: Apollo Client Configuration**
**Location**: `src/lib/apollo-client.ts`

```typescript
import { ApolloClient, InMemoryCache } from '@apollo/client';

const client = new ApolloClient({
  uri: 'https://swapi-graphql.netlify.app/graphql',
  cache: new InMemoryCache(),
});

export default client;
```

**Explanation**: 
- Configures Apollo Client to connect to the SWAPI GraphQL endpoint
- Uses in-memory cache for client-side caching
- This client is used for server-side data fetching in `getStaticProps`

### **Step 4: Component Props Interface**
**Location**: `src/pages/films/index.tsx`

```typescript
interface FilmsPageProps {
  films: {
    id: string;
    director: string;
    openingCrawl: string;
    releaseDate: string;
    title: string;
  }[];
}
```

**Explanation**: 
- Defines the TypeScript interface for the films data structure
- Ensures type safety between server-side data and client-side components
- Matches the transformed data structure from `getStaticProps`

## **Complete Dataflow Summary**

1. **Server-Side Data Fetching**: `getStaticProps` → Apollo Client → SWAPI GraphQL API
2. **Data Transformation**: GraphQL edges → Simplified film objects
3. **Static Generation**: Next.js generates static HTML with data
4. **Client-Side Hydration**: React hydrates the static HTML
5. **Component Rendering**: Films data flows to UI components
6. **Interactive Features**: Loading states, animations, and navigation
7. **SEO Optimization**: Meta tags and structured data for search engines

The architecture follows Next.js best practices with Static Site Generation, TypeScript for type safety, and a clean separation between data fetching, transformation, and presentation layers.