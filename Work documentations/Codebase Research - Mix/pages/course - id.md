- **Purpose**: Renders the course player page for `/course/[id]` with a video area and a fixed right sidebar listing lectures.

- **Data fetching (SSR)**:
  - Auth check: Queries `SessionDocument`. If no `session.me.id`, redirects to `/#signin`.
  - Loads course: Queries `MixClassCourseByIdDocument` with the numeric `id`. On error, redirects to `/mypage/courses`.
  - Provides `mix_class_course` as `result` prop to the page.

- **Layout and UI**:
  - Header with a back `Link` to `/mypage/courses` and a `Divider`.
  - Main content split:
    - Left: Responsive video container showing the current lecture’s `video_url`.
    - Right: Fixed sidebar (`LectureListWrap`) showing the course title and a list of lectures via `LectureCard`.

- **State and behavior**:
  - `currentLecture` state tracks `{ order, progress }` for the selected lecture (defaults to order 1).
  - `videoUrl` derived from `lectures_list` based on `currentLecture.order`.
  - `videoRef` is set up for potential programmatic control (commented controls for jumping between sections exist but are disabled).

- **Styling/tech**:
  - Uses Emotion (`css`, `styled`) and app theme (`theme`, `zIndex`).
  - `LectureListWrap` is a styled fixed panel (width 399px), keeping the player visible on the left.

- **Error and edge handling**:
  - Throws if the dynamic `id` param is missing or non-string before fetching.
  - Redirects to courses list on course query failure.
  - Video gracefully shows fallback text if no `video_url`.

- **Key components and imports**:
  - `LectureCard` renders each lecture in the list and updates `currentLecture`.
  - GraphQL: `MixClassCourseByIdDocument`, `SessionDocument`.
  - Utilities: `getClient` for cookie-authenticated GraphQL client, `MixError` enum.