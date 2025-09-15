The problem is `getClient` function is used repeatedly all over the codebase:  

- Used in 60+ places
- 2 use-cases
    - SSR pages (with cookies and context)
    - API routes (with admin secret and null context)
- Code duplication

I'm thinking of _refactoring into 3 distinct utility functions._  
This will  

- Reduce the code duplication
- Make the intent clearer (SSR - Admin - API)
- Centralize the config logic
- And make maintenance easier

![[file-20250910141729705.png]]

![[file-20250910141739340.png]]

![[file-20250910141745856.png]]