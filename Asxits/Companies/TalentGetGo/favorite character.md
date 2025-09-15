
## Here are the exact locations of the localStorage code in your project:

### 1. **Storage Key Definition**
```1:1:src/utils/constants.ts
export const STORAGE_KEY = 'favoriteCharacters';
```

### 2. **Main localStorage Implementation**
```31:70:src/components/animated/interactive-hover-button.tsx
// Load favorites from localStorage on mount
useEffect(() => {
  try {
    const stored = localStorage.getItem(STORAGE_KEY);
    if (stored) {
      const parsedItems = JSON.parse(stored) as CharacterCardProps[];
      setItems(parsedItems);

      // Check if current character is already a favorite
      if (characterName && characterUrl) {
        const isFavorite = parsedItems.some(
          (item) =>
            item.characterUrl === characterUrl ||
            item.characterName === characterName
        );
        setIsAlreadyFavorite(isFavorite);
      }
    }
  } catch (error) {
    console.error('Error loading from localStorage:', error);
  }
  setIsInitialized(true);
}, [characterName, characterUrl]);

// Save to localStorage when items change (but not on initial load)
useEffect(() => {
  if (isInitialized) {
    try {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
    } catch (error) {
      console.error('Error saving to localStorage:', error);
    }
  }
}, [items, isInitialized]);

const addItem = () => {
  if (characterName && characterUrl) {
    setItems((prevItems) => {
      // Check if character already exists (by URL or name)
      const characterExists = prevItems.some(
        (item) =>
          item.characterUrl === characterUrl ||
          item.characterName === characterName
      );

      if (characterExists) {
        // Remove from favorites
        setIsAlreadyFavorite(false);
        return prevItems.filter(
          (item) =>
            item.characterUrl !== characterUrl &&
            item.characterName !== characterName
        );
      }

      // Add to favorites
      setIsAlreadyFavorite(true);
      return [
        ...prevItems,
        {
          characterName,
          characterUrl,
        },
      ];
    });
  }
};
```

### 3. **Character Grid Card localStorage Reading**
```31:50:src/components/characters/grid/character-grid-card.tsx
// Load favorites from localStorage on mount
useEffect(() => {
  try {
    const stored = localStorage.getItem(STORAGE_KEY);
    if (stored) {
      const parsedItems = JSON.parse(stored) as CharacterGridCardProps[];
      setItems(parsedItems);

      // Check if current character is already a favorite
      if (characterName && characterUrl) {
        const isFavorite = parsedItems.some(
          (item) =>
            item.characterUrl === characterUrl ||
            item.characterName === characterName
        );
        setIsAlreadyFavorite(isFavorite);
      }
    }
  } catch (error) {
    console.error('Error loading from localStorage:', error);
  }
  setIsInitialized(true);
}, [characterName, characterUrl]);
```

### 4. **Test Files with localStorage Mocking**
- `jest.setup.js` - Global localStorage mock
- `__tests__/components/animated/interactive-hover-button.test.tsx` - Component tests
- `__tests__/components/characters/character-grid-card.test.tsx` - Grid card tests

### 5. **Usage in Character Detail Page**
```200:205:src/pages/characters/[id].tsx
<InteractiveHoverButton
  characterName={character.name}
  characterUrl={character.url}
/>
```

### 6. **Documentation References**
- `README.md` - Mentions localStorage persistence
- `PROJECT_DEEP_DIVE.md` - Documents favorites system

The localStorage implementation is primarily in the `InteractiveHoverButton` component, which handles both reading and writing to localStorage, while the `CharacterGridCard` component only reads from localStorage to display favorite status indicators.