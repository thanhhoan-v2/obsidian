## Film Details Page Dataflow

### 1. **Static Site Generation (SSG) Setup**

**Step 1: Path Generation (`getStaticPaths`)**
```typescript
// src/pages/films/[id].tsx - lines 320-350
export const getStaticPaths: GetStaticPaths = async () => {
  try {
    // Fetch all films using Apollo Client
    const { data } = await client.query({
      query: GET_ALL_FILMS,
    });

    // Extract films from GraphQL response
    const filmsFromEdges: FilmEdge[] = data?.allFilms?.edges || [];
    const paths = filmsFromEdges.map((edge: FilmEdge, index: number) => ({
      params: { id: (index + 1).toString() }, // Generate ID based on index
    }));

    return {
      paths,
      fallback: false,
    };
  } catch (error) {
    console.error('Error getting film paths:', error);
    return {
      paths: [],
      fallback: false,
    };
  }
};
```

**Explanation**: This function runs at build time to generate all possible film detail page paths. It fetches all films from the GraphQL API and creates static paths for each film.

### 2. **Data Fetching (`getStaticProps`)**

**Step 2: Film Data Retrieval**
```typescript
// src/pages/films/[id].tsx - lines 352-387
export const getStaticProps: GetStaticProps = async ({ params }) => {
  try {
    const id = params?.id as string;

    // Fetch specific film details using GraphQL
    const { data } = await client.query({
      query: GET_FILM_BY_ID,
      variables: { filmID: id },
    });

    if (!data?.film) {
      return {
        notFound: true,
      };
    }

    const graphqlFilm = data.film;

    // Convert GraphQL film to SWAPI format
    const film = convertGraphQLFilmToSWAPI(
      {
        id,
        title: graphqlFilm.title,
        director: graphqlFilm.director,
        releaseDate: graphqlFilm.releaseDate,
        openingCrawl: graphqlFilm.openingCrawl,
      },
      parseInt(id) - 1
    );

    // Convert related data
    const characters: Person[] = (
      graphqlFilm.characterConnection?.edges || []
    ).map((edge: CharacterEdge, index: number) =>
      convertGraphQLCharacterToSWAPI(edge.node, index)
    );

    return {
      props: {
        film,
        characters,
        planets: graphqlFilm.planetConnection?.edges || [],
        starships: graphqlFilm.starshipConnection?.edges || [],
      },
      revalidate: 86400, // Revalidate once per day
    };
  } catch (error) {
    console.error('Error fetching film details:', error);
    return {
      notFound: true,
    };
  }
};
```

**Explanation**: This function fetches the specific film data and its related entities (characters, planets, starships) using GraphQL queries. It converts the GraphQL response to a format compatible with the UI components.

### 3. **GraphQL Query Structure**

**Step 3: GraphQL Query Definition**
```typescript
// src/lib/queries.ts - lines 15-50
export const GET_FILM_BY_ID = gql`
  query GetFilmById($filmID: ID!) {
    film(filmID: $filmID) {
      title
      director
      releaseDate
      openingCrawl
      characterConnection {
        edges {
          node {
            name
            birthYear
            gender
            height
            mass
            hairColor
            skinColor
            eyeColor
          }
        }
      }
      planetConnection {
        edges {
          node {
            name
            climates
            terrains
            population
          }
        }
      }
      starshipConnection {
        edges {
          node {
            name
            model
            manufacturers
            starshipClass
          }
        }
      }
    }
  }
`
```

**Explanation**: This GraphQL query fetches all the necessary details for a single film, including its characters, planets, and starships, in a single request.

---

### 4. **Data Conversion for UI**

**Step 4: Data Conversion Functions**
```typescript
// src/pages/films/[id].tsx - lines 71-107
function convertGraphQLFilmToSWAPI(film: GraphQLFilm, index: number) {
  return {
    title: film.title,
    episode_id: parseInt(film.id) || index + 1,
    opening_crawl: film.openingCrawl,
    director: film.director,
    producer: '', // Not available in GraphQL
    release_date: film.releaseDate,
    characters: [], // Would need to fetch separately
    planets: [], // Would need to fetch separately
    starships: [], // Would need to fetch separately
    vehicles: [], // Not available in GraphQL
    species: [], // Not available in GraphQL
    created: '', // Not available
    edited: '', // Not available
    url: `/films/${film.id}`,
  };
}

function convertGraphQLCharacterToSWAPI(
  char: CharacterEdge['node'],
  index: number
) {
  return {
    name: char.name,
    height: char.height?.toString() || 'unknown',
    mass: char.mass?.toString() || 'unknown',
    hair_color: char.hairColor || 'unknown',
    skin_color: char.skinColor || 'unknown',
    eye_color: char.eyeColor || 'unknown',
    birth_year: char.birthYear || 'unknown',
    gender: char.gender || 'unknown',
    homeworld: '', // Not available in this context
    url: `/characters/${index + 1}`,
  };
}
```

**Explanation**: These functions adapt the GraphQL data structure to the format expected by the UI components, ensuring type safety and consistency.

---

If you want a more detailed breakdown of any specific step (e.g., how a card component works, or how the GraphQL query is structured), let me know!