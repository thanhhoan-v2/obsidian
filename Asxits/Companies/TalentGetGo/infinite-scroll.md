1. **Initial Load**: Characters page loads with first 10 characters
2. **Scroll Detection**: When user scrolls near bottom (1000px threshold)
3. **API Call**: Triggers `loadMore()` which calls `handleSearch()` with next page
4. **Data Update**: New characters are appended to existing array
5. **Visual Feedback**: Loading spinner shows during fetch
6. **State Update**: `hasMore` is updated based on API response
7. **End State**: Shows "end of results" when no more data### 1. **Core Infinite Scroll Hook** (`src/hooks/use-infinite-scroll.ts`)


```typescript
export function useInfiniteScroll({
  loadMore,
  hasMore,
  loading,
}: UseInfiniteScrollProps) {
  useEffect(() => {
    const handleScroll = () => {
      if (loading || !hasMore) return;

      if (
        window.innerHeight + document.documentElement.scrollTop >=
        document.documentElement.offsetHeight - 1000
      ) {
        loadMore();
      }
    };

    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, [loadMore, hasMore, loading]);
}
```

**Key Features:**
- **Scroll Detection**: Monitors when user scrolls within 1000px of the bottom
- **Loading Protection**: Prevents multiple simultaneous requests when `loading` is true
- **HasMore Check**: Only triggers when there are more items to load
- **Cleanup**: Properly removes event listener on unmount

The infinite scroll implementation provides a smooth, performant user experience with proper loading states, error handling, and search integration. It's currently only implemented on the Characters page but could be easily extended to other pages like Films if needed.