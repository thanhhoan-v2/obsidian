- **Purpose**: Renders the homepage feed with three sections and adapts content to user tags and device size. Prefetches initial data on SSR and tracks scroll analytics.

### Key flows

- **Session and user context**
  - Fetches session to get `userId` and `userGroup`.
```60:63:pages/index.tsx
const { data: sessionData } = useSessionQuery();
const userId = sessionData?.session?.me?.id;
const userGroup = sessionData?.session?.me?.group_id;
```

- **User interest tags**
  - If logged in, loads user’s interest tags for personalization.
```64:67:pages/index.tsx
const { data: userInterestTagsData } = useGetUserInterestTagsQuery({
  variables: { userId: userId! },
  skip: !userId,
});
```

- **Active tag selection logic (priority)**
  1) If query `?tag=` exists and is valid, use it as a single-tag filter.
  2) Else if the user has saved interest tags, use those.
  3) Else fallback by `userGroup`:
     - `master`: show all (tags = null)
     - `shinhaninvest`: default list + Shinhan-specific tags
     - `general`/`thesmc`/undefined: default tags
```69:104:pages/index.tsx
const tags = useMemo((): Mix_Content_Tag_Enum_Enum[] | null => {
  const { tag } = router.query;
  if (typeof tag === "string" && ORDERED_MIX_CONTENT_TAG_ENUM.includes(tag as ...)) {
    return [tag as Mix_Content_Tag_Enum_Enum];
  }
  const userTags = userInterestTagsData?.mix_user_interest_tag;
  if (userTags?.length) {
    return userTags.map(item => item.tag_value as Mix_Content_Tag_Enum_Enum);
  }
  switch (userGroup) {
    case "master": return null;
    case "shinhaninvest": return [...DEFAULT_TAG_LIST, ...SHINHAN_INVEST_TAG_LIST] as ...;
    case "general":
    case "thesmc":
    case undefined:
    default: return DEFAULT_TAG_LIST as ...;
  }
}, [router.query, userInterestTagsData, userGroup]);
```

- **Sections and responsive rendering**
  - Sections: `OriginalSection`, `RecentSection`, `MixedSection`.
  - Desktop shows `MixedSection` (and hides mobile/tablet sections).
  - Tablet/mobile show `OriginalSection` (only when tags === null) and `RecentSection`.
  - All queries use `buildQueryVariables` with `LIMIT`.
```198:213:pages/index.tsx
<SectionBox css={...desktop-only...}>
  <MixedSection
    data-observer-id="mixed"
    contentSection="mixed"
    variables={buildQueryVariables("recent", tags, LIMIT.default)}
  />
</SectionBox>
```
```181:197:pages/index.tsx
<SectionBox css={...tablet/mobile...}>
  {tags === null && <SectionSeperator />}
  <RecentSection
    data-observer-id="recent"
    contentSection="recent"
    variables={buildQueryVariables("recent", tags, LIMIT.default)}
  />
</SectionBox>
```
```159:178:pages/index.tsx
{tags === null && (
  <SectionBox css={...tablet/mobile...}>
    <OriginalSection
      data-observer-id="original"
      contentSection="original-main"
      variables={buildQueryVariables("original", null, LIMIT.original)}
    />
  </SectionBox>
)}
```

- **Scroll analytics**
  - IntersectionObserver fires a GA event once per section when it enters view.
```114:149:pages/index.tsx
const observer = new IntersectionObserver((entries) => {
  for (const entry of entries) {
    const sections = ["original", "recent", "mixed"];
    const targetId = entry.target.getAttribute("data-observer-id");
    ...
    if (entry.isIntersecting && !touched[section]) {
      fireEvent({ event: "scroll_depth", params: { scrolled_section: section }});
      setTouched((prev) => ({ ...prev, [section]: true }));
    }
  }
}, { root: document.querySelector("index-page-container"), rootMargin: "0% 0px -100% 0px" });
```

- **SSR prefetch**
  - On the server, preloads:
    - `original` with no tags (for mobile-only section) and
    - `recent` with either the URL tag or null.
  - Extracts Apollo cache into props.
```219:249:pages/index.tsx
await client.query({ query: MixContentDocument, variables: buildQueryVariables("original", null, LIMIT.original) });
await client.query({ query: MixContentDocument, variables: buildQueryVariables("recent", tags, LIMIT.default) });
return { props: { __APOLLO_CACHE_DATA__: client.extract() } };
```

- **Canonical link**
  - Adds `<link rel="canonical" href="https://mix.day/" />` for SEO.

### Takeaways
- Tags drive what content appears; `null` means “all” and unlocks the extra `OriginalSection`.
- Desktop vs mobile layouts are controlled by CSS breakpoints, not separate routes.
- Analytics track first exposure per section.
- SSR seeds Apollo cache for a faster first paint.