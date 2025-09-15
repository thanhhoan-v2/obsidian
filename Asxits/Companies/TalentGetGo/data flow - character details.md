## Character Detail Page Dataflow: From Data to UI

Here's a comprehensive breakdown of the dataflow in the character detail page, tracing each step from data fetching to UI rendering:

### **Step 1: Static Site Generation (SSG) - Data Fetching**

**Location**: `src/pages/characters/[id].tsx` (lines 280-367)

```typescript
export const getStaticProps: GetStaticProps = async ({ params }) => {
  try {
    const id = params?.id as string;

    // 1. Fetch character details from SWAPI.tech
    const characterResponse = await fetch(
      `https://www.swapi.tech/api/people/${id}`
    );
    if (!characterResponse.ok) {
      return { notFound: true };
    }

    const characterData: SwapiTechCharacterResponse =
      await characterResponse.json();

    // 2. Convert SWAPI.tech character to SWAPI format
    const character = convertSwapiTechCharacterToSWAPI(
      characterData.result.properties,
      id
    );

    // 3. Fetch homeworld if available
    let homeworld: IPlanet | null = null;
    if (characterData.result.properties.homeworld) {
      try {
        const homeworldResponse = await fetch(
          characterData.result.properties.homeworld
        );
        if (homeworldResponse.ok) {
          const homeworldData: SwapiPlanetResponse =
            await homeworldResponse.json();
          if (homeworldData.message === 'ok') {
            homeworld = convertSwapiPlanetToSWAPI(
              homeworldData.result.properties
            );
          }
        }
      } catch (error) {
        console.error('Error fetching homeworld:', error);
      }
    }

    return {
      props: {
        paramsId: id,
        character,
        homeworld,
        starships: [],
      },
      revalidate: 86400, // Revalidate once per day
    };
  } catch (error) {
    console.error('Error fetching character details:', error);
    return {
      notFound: true,
    };
  }
};
```

**Explanation**: 
- Next.js calls `getStaticProps` at build time or during revalidation
- Fetches character data from SWAPI.tech API
- Converts the API response to internal format using `convertSwapiTechCharacterToSWAPI`
- Fetches homeworld data if available
- Returns props to be passed to the component

### **Step 2: Data Transformation**

**Location**: `src/pages/characters/[id].tsx` (lines 60-85)

```typescript
function convertSwapiTechCharacterToSWAPI(
  char: SwapiTechCharacterResponse['result']['properties'],
  id: string
): IPerson {
  return {
    name: char.name,
    height: char.height || 'unknown',
    mass: char.mass || 'unknown',
    hair_color: char.hair_color || 'unknown',
    skin_color: char.skin_color || 'unknown',
    eye_color: char.eye_color || 'unknown',
    birth_year: char.birth_year || 'unknown',
    gender: char.gender || 'unknown',
    homeworld: char.homeworld || 'unknown',
    films: [], // Would need to fetch separately
    species: [], // Not available in this context
    vehicles: [], // Not available in this context
    starships: [], // Not available in this context
    created: char.created || '',
    edited: char.edited || '',
    url: `/characters/${id}`,
  };
}
```

**Explanation**: 
- Transforms raw API data into standardized `IPerson` interface
- Handles missing data with fallback values
- Normalizes data structure for consistent component usage



### **Step 4: Client-Side Image Loading**

**Location**: `src/pages/characters/[id].tsx` (lines 97-110)

```typescript
useEffect(() => {
  const loadImage = async () => {
    try {
      const image = await getCharacterImageById(characterId || 1);
      if (image) setCharacterImage(image.src);
    } catch (error) {
      console.warn(
        `Failed to load character image for ${characterId}:`,
        error
      );
    }
  };
  loadImage();
}, [characterId]);
```

**Explanation**: 
- Uses `useEffect` to load character image asynchronously
- Calls `getCharacterImageById` from `src/utils/assets.ts`
- Updates local state with image source
- Handles errors gracefully with fallback

### **Step 5: Image Asset Resolution**

**Location**: `src/utils/assets.ts` (lines 18-32)

```typescript
export const getCharacterImageById = async (
  characterId: number
): Promise<StaticImageData | null> => {
  try {
    const image = await import(`@/assets/characters-id/${characterId}.jpg`);
    return image.default;
  } catch (error) {
    try {
      const fallbackImage = await import('@/assets/characters-id/1.jpg');
      return fallbackImage.default;
    } catch {
      return null;
    }
  }
};
```

**Explanation**: 
- Dynamically imports character image by ID
- Uses webpack's dynamic import for code splitting
- Provides fallback to default image if specific image not found
- Returns `StaticImageData` for Next.js Image optimization



## **Summary of Dataflow**

1. **Server-Side**: `getStaticProps` fetches data from SWAPI.tech API
2. **Data Transformation**: Raw API data converted to internal interfaces
3. **Props Passing**: Transformed data passed to component as props
4. **Client-Side**: Image loading and state management
5. **SEO Setup**: Dynamic meta tags and Open Graph data
6. **UI Rendering**: Framer Motion animations and component composition
7. **Interactive Features**: Favorites system with localStorage persistence
8. **Responsive Design**: Mobile-first approach with responsive grids
9. **Error Handling**: Graceful fallbacks for missing data and images
10. **Performance**: Static generation with ISR for optimal loading

This dataflow ensures a smooth user experience with proper loading states, error handling, and interactive features while maintaining SEO optimization and performance.