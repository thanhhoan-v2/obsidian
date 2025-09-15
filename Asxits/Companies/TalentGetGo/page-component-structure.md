# Page Component Structure - Star Wars Marketing Website

This document provides a detailed breakdown of the component structure for each page in the Space Odyssey Star Wars marketing website.

---

## 🏠 Home Page (`/`)

**File**: `src/pages/index.tsx`

### Component Structure

```
Home Page
├── SEO Components
│   ├── NextSeo (Dynamic meta tags)
│   └── Head (Static meta tags)
├── Main Container
│   ├── Background Video (`/landing-video.mp4`)
│   ├── PageHeader (Fixed navigation)
│   └── Hero Section
│       ├── Motion.div (Animated title)
│       │   └── "The Force Lives in All of Us" text
│       ├── BoxReveal (Logo reveal animation)
│       │   ├── TalentGetGo Logo
│       │   ├── XIcon (separator)
│       │   └── Star Wars Logo
│       └── Motion.div (CTA buttons)
│           ├── InteractiveHoverButton (Films)
│           └── InteractiveHoverButton (Characters)
├── Featured Characters Section
│   ├── Motion.div (Section animation)
│   ├── "Legends of the Galaxy" title
│   ├── Description text
│   └── CardCarousel (Featured characters)
│       └── Character cards (6 featured characters)
├── Featured Films Section
│   ├── Motion.div (Section animation)
│   ├── BoxReveal ("Chronicles of the Force" title)
│   ├── Description text
│   └── Film Grid
│       ├── Loading state (if isLoading)
│       ├── FilmGridCard components (3 random films)
│       └── Error state (if no films)
└── PageFooter
```

### Key Features
- **Hero Section**: Full-screen video background with animated text
- **Featured Characters**: Carousel with 6 iconic characters
- **Featured Films**: Grid of 3 randomly selected films
- **Animations**: Framer Motion animations throughout
- **SEO**: Comprehensive meta tags and Open Graph

### Data Flow
- **Static Props**: Fetches films via GraphQL and characters via REST API
- **Revalidation**: 24-hour ISR for fresh content
- **Error Handling**: Fallback states for API failures

---

## 🎬 Films List Page (`/films`)

**File**: `src/pages/films/index.tsx`

### Component Structure

```
Films Page
├── SEO Components
│   ├── NextSeo (Dynamic meta tags)
│   └── Head (Static meta tags)
├── PageLayout (Common layout wrapper)
│   ├── PageHeader
│   ├── Main Content
│   │   ├── Page Title
│   │   │   └── "Explore the complete Star Wars saga" text
│   │   ├── FilmGridStats (Statistics component)
│   │   │   └── Film statistics display
│   │   └── Films Grid
│   │       ├── Loading State
│   │       │   └── FilmGridCardSkeleton (6 skeletons)
│   │       ├── Films Display
│   │       │   ├── "All Films" title
│   │       │   └── Motion.div (Grid animation)
│   │       │       └── FilmGridCard components
│   │       │           └── Sorted by episode ID
│   │       └── Error State
│   │           └── "No films available" message
│   └── PageFooter
```

### Key Features
- **Responsive Grid**: 1 column on mobile, 2 columns on desktop
- **Loading States**: Skeleton components during data fetch
- **Statistics**: Film count and metadata display
- **Animations**: Staggered entrance animations for film cards
- **Sorting**: Films sorted by episode ID

### Data Flow
- **Static Props**: Fetches all films via GraphQL
- **Revalidation**: 24-hour ISR
- **Loading UX**: 1-second loading simulation for better UX

---

## 🎭 Film Detail Page (`/films/[id]`)

**File**: `src/pages/films/[id].tsx`

### Component Structure

```
Film Detail Page
├── SEO Components
│   ├── NextSeo (Dynamic meta tags)
│   └── Head (Static meta tags)
├── PageLayout (Common layout wrapper)
│   ├── PageHeader
│   ├── Main Content
│   │   ├── Film Header Section
│   │   │   ├── BoxReveal (Film title animation)
│   │   │   ├── Badge components
│   │   │   │   ├── Episode badge
│   │   │   │   ├── Director badge (with ClapperboardIcon)
│   │   │   │   └── Release date badge (with Calendar icon)
│   │   │   └── TextReveal (Opening crawl animation)
│   │   └── Related Content Sections
│   │       ├── Characters Section (if characters exist)
│   │       │   ├── Section title with count
│   │       │   └── CharacterGridCard components
│   │       │       └── Responsive grid (1-4 columns)
│   │       ├── Planets Section (if planets exist)
│   │       │   ├── Section title with count
│   │       │   └── PlanetCard components
│   │       │       └── Responsive grid (1-4 columns)
│   │       └── Starships Section (if starships exist)
│   │           ├── Section title with count
│   │           └── StarshipCard components
│   │               └── Responsive grid (1-4 columns)
│   └── PageFooter
```

### Key Features
- **Dynamic Content**: Film-specific data and relationships
- **Responsive Design**: Adaptive grid layouts for different screen sizes
- **Animations**: Staggered animations for content sections
- **Badge System**: Visual indicators for film metadata
- **Related Content**: Characters, planets, and starships from the film

### Data Flow
- **Static Paths**: Pre-generates paths for all films
- **Static Props**: Fetches film details and related data via GraphQL
- **Fallback**: `false` for 404 handling
- **Revalidation**: 24-hour ISR

---

## 👥 Characters List Page (`/characters`)

**File**: `src/pages/characters/index.tsx`

### Component Structure

```
Characters Page
├── SEO Components
│   ├── NextSeo (Dynamic meta tags)
│   └── Head (Static meta tags)
├── PageLayout (Common layout wrapper)
│   ├── PageHeader
│   ├── Main Content
│   │   ├── CharacterGridHeader
│   │   │   └── Page title and description
│   │   ├── Motion.div (Search bar animation)
│   │   │   └── CharacterSearchBar
│   │   │       ├── Search input
│   │   │       └── Loading indicator
│   │   ├── CharacterSearchInfo
│   │   │   ├── Search status
│   │   │   ├── Result count
│   │   │   └── Search query display
│   │   ├── CharacterGrid
│   │   │   ├── CharacterGridCard components
│   │   │   ├── CharacterGridCardSkeleton (loading)
│   │   │   └── Empty state
│   │   └── CharacterSearchStates
│   │       ├── Loading states
│   │       ├── Load more button
│   │       ├── Clear search button
│   │       └── End of results message
│   └── PageFooter
```

### Key Features
- **Infinite Scroll**: Automatic loading of more characters
- **Real-time Search**: Debounced search with instant results
- **Loading States**: Skeleton components and loading indicators
- **Search Functionality**: Client-side filtering and API search
- **Responsive Grid**: Adaptive character card layout

### Data Flow
- **Static Props**: Fetches initial characters via REST API
- **Custom Hook**: `useCharacters` manages search and pagination
- **Infinite Scroll**: `useInfiniteScroll` hook for automatic loading
- **Caching**: 30-minute client-side cache for performance

### Custom Hooks Used
- `useCharacters`: Manages character state, search, and pagination
- `useInfiniteScroll`: Handles scroll-based loading
- `useMounted`: Client-side mounting detection

---

## 👤 Character Detail Page (`/characters/[id]`)

**File**: `src/pages/characters/[id].tsx`

### Component Structure

```
Character Detail Page
├── SEO Components
│   ├── NextSeo (Dynamic meta tags)
│   └── Head (Static meta tags)
├── PageLayout (Common layout wrapper)
│   ├── PageHeader
│   ├── Main Content
│   │   ├── Character Profile Section
│   │   │   ├── Motion.div (Profile animation)
│   │   │   ├── Character Image
│   │   │   │   ├── Background blur effect
│   │   │   │   ├── Profile image (300x300)
│   │   │   │   └── Border and shadow effects
│   │   │   ├── Motion.div (Name animation)
│   │   │   │   └── Character name (large text)
│   │   │   └── InteractiveHoverButton
│   │   │       └── Favorite character functionality
│   │   ├── Character Information Cards
│   │   │   ├── Card wrapper
│   │   │   └── Grid layout (1-2 columns)
│   │   │       ├── CharacterDetailBioCard
│   │   │       │   ├── Gender
│   │   │       │   ├── Birth year
│   │   │       │   └── Homeworld
│   │   │       └── CharacterDetailAppearanceCard
│   │   │           ├── Height
│   │   │           ├── Mass
│   │   │           ├── Hair color
│   │   │           ├── Skin color
│   │   │           └── Eye color
│   │   └── Homeworld Section (if available)
│   │       ├── Motion.div (Section animation)
│   │       ├── "Homeworld" title
│   │       └── PlanetCard
│   │           └── Homeworld information
│   └── PageFooter
```

### Key Features
- **Dynamic Images**: Character-specific profile images
- **Information Cards**: Bio and appearance details
- **Homeworld Integration**: Related planet information
- **Animations**: Staggered entrance animations
- **Responsive Design**: Adaptive layout for different screen sizes

### Data Flow
- **Static Paths**: Pre-generates paths for first 10 characters
- **Static Props**: Fetches character details via REST API
- **Fallback**: `blocking` for on-demand page generation
- **Image Loading**: Dynamic character image loading with fallbacks

---

## 🧩 Common Components Used Across Pages

### Layout Components
```
PageLayout
├── PageHeader
│   ├── Navigation menu
│   ├── Theme toggle
│   └── Logo
└── PageFooter
    ├── Social links
    ├── Copyright
    └── External links
```

### Animated Components
```
Animated Components
├── BoxReveal
│   └── Content reveal animations
├── TextReveal
│   └── Text animation effects
├── InteractiveHoverButton
│   └── Advanced hover interactions
└── Motion.div
    └── Framer Motion animations
```

### UI Components
```
UI Components
├── Card
│   ├── CardContent
│   ├── CardHeader
│   └── CardFooter
├── Badge
│   ├── Episode badge
│   ├── Director badge
│   └── Release date badge
├── Skeleton
│   ├── FilmGridCardSkeleton
│   └── CharacterGridCardSkeleton
└── Avatar
    └── Character profile images
```

### Character Components
```
Character Components
├── CharacterGrid
│   ├── CharacterGridCard
│   ├── CharacterGridHeader
│   └── CharacterGridStats
├── Character Search
│   ├── CharacterSearchBar
│   ├── CharacterSearchInfo
│   └── CharacterSearchStates
└── Character Detail
    ├── CharacterDetailBioCard
    └── CharacterDetailAppearanceCard
```

### Film Components
```
Film Components
├── FilmGridCard
│   ├── Film title
│   ├── Director
│   ├── Release date
│   └── Opening crawl excerpt
├── FilmGridCardSkeleton
└── FilmGridStats
```

### Resource Components
```
Resource Components
├── PlanetCard
│   ├── Planet name
│   ├── Climate
│   ├── Terrain
│   └── Population
└── StarshipCard
    ├── Starship name
    ├── Model
    ├── Manufacturer
    └── Class
```

---

## 🔄 Data Flow Architecture

### API Integration
```
Data Sources
├── SWAPI GraphQL
│   ├── Film data
│   ├── Character relationships
│   ├── Planet data
│   └── Starship data
└── SWAPI.tech REST API
    ├── Character details
    ├── Search functionality
    └── Pagination
```

### State Management
```
State Management
├── Apollo Client
│   ├── GraphQL caching
│   ├── Network state
│   └── Optimistic updates
├── Custom Hooks
│   ├── useCharacters
│   ├── useInfiniteScroll
│   └── useMounted
└── Local Storage
    ├── Theme preferences
    └── Favorite characters
```

### Component Communication
```
Component Communication
├── Props (Parent → Child)
├── Custom Hooks (Shared state)
├── Context (Theme, etc.)
└── Event Handlers (Child → Parent)
```

---

## 🎨 Animation Strategy

### Page-Level Animations
- **Entrance Animations**: Framer Motion for page transitions
- **Staggered Effects**: Sequential component animations
- **Scroll Triggers**: Intersection Observer for scroll-based animations

### Component-Level Animations
- **Hover Effects**: Interactive button and card animations
- **Loading States**: Skeleton component animations
- **Content Reveals**: BoxReveal and TextReveal components

### Performance Optimizations
- **GPU Acceleration**: Hardware-accelerated transforms
- **Reduced Motion**: Accessibility considerations
- **Lazy Loading**: Non-critical animation delays

---

## 📱 Responsive Design

### Breakpoint Strategy
```
Responsive Breakpoints
├── Mobile (320px - 768px)
│   ├── Single column layouts
│   ├── Stacked components
│   └── Touch-friendly interactions
├── Tablet (768px - 1024px)
│   ├── Two-column grids
│   ├── Medium-sized components
│   └── Hybrid layouts
└── Desktop (1024px+)
    ├── Multi-column layouts
    ├── Hover effects
    └── Advanced interactions
```

### Component Adaptations
- **Grid Systems**: Responsive grid layouts
- **Typography**: Scalable text sizes
- **Spacing**: Adaptive padding and margins
- **Images**: Responsive image sizing

---

## 🔍 SEO & Performance

### SEO Strategy
- **Static Generation**: Pre-rendered pages for search engines
- **Dynamic Meta Tags**: Page-specific meta information
- **Structured Data**: Rich snippets for search results
- **Sitemap**: Dynamic sitemap generation

### Performance Optimizations
- **Code Splitting**: Route-based automatic splitting
- **Image Optimization**: Next.js automatic image optimization
- **Caching**: Client-side and server-side caching
- **Bundle Analysis**: Optimized bundle sizes

---

This component structure demonstrates a well-organized, scalable architecture that prioritizes user experience, performance, and maintainability while showcasing advanced React patterns and modern web development practices. 