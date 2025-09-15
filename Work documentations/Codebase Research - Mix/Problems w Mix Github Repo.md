 
- Update Next.js version from 13.5.6 -> 15.5.2
- Hydration error
 
---
### Important, Urgent
- Repeated code
	- getClient() -> create utility functions
	- Container -> Create a common page layouts
	- constants and configs, like LIMIT or OFFSET -> centralize in one place
	- useAuth() -> custom hook for authentication
- Large components -> Split into smaller ones
	- pages/contents/original/[id].tsx (1032 lines) (violates SRP)
	- pages/mypage/index.tsx (265 lines)
- Inconsistent error handlings -> create error handling utility, to unify the error handling and make it consistent.
	- some pages use try-catch w redirects
	- some use early returns
	- some use log errors
- Business logic and UI logic is mixed together
	- not in separation of concerns
	- hard to debug
	- not reusable and maintainable
	- this can cause unecessary re-renders, which bad for perf
	- examples: pages/index.tsx - tags logic -> extract to custom hook
- Loose typing and any usages -> create proper types
	- pages/payments/start.tsx - let me: any = null

--- 
### Not Important, Not Urgent
 
 - Use pnpm over npm