chore: upgrade Next.js from 13.5.6 to 14.2.32 and fix TypeScript errors

- Upgrade Next.js to 14.2.32 (major version bump)
- Update React to 18.3.1 and React DOM to 18.3.1
- Update Emotion packages (@emotion/react, @emotion/styled) to latest versions
- Update TypeScript types (@types/react, @types/react-dom) to 18.2.0
- Update ESLint config to match Next.js 14 version
- Remove @next/font dependency (now built into Next.js 14)
- Fix TypeScript compilation errors caused by stricter type checking:
    - Fix ContentCard component type definitions to properly handle content prop
    - Update ContentCTACard to exclude content from HTML attributes
    - Fix scaffold components type conflicts
    - Update ShareModal to exclude content from HTML attributes
    - Fix ContentBookmark component prop passing to prevent HTML attribute conflicts
- Update package-lock.json with all dependency changes