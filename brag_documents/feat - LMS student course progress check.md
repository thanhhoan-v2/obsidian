## Workings

1. **Database Schema**: The system has well-defined progress tracking tables

- `mix_class_course_progress` - tracks overall course progress
- `mix_class_lecture_progress` - tracks individual lecture progress
- Both tables include fields for progress, status, watch_time, last_watched, etc.

2. **Basic Course Structure**

- Course page (/course/[id]) displays lectures in a sidebar
- Video player is functional with basic controls
- Lecture navigation works (clicking lectures switches videos)

3. **Status Management**

- Course status enum exists: "not-started", "in-progress", "completed"

## Problems

1. **No Progress Tracking Implementation**

- No video event handlers (onTimeUpdate, onProgress)
- No progress persistence to database
- No real-time progress updates

2. **No GraphQL Operations for Progress**

- No queries to fetch user progress
- No mutations to update progress
- No progress data in course fragments

3. **No Progress Visualization**

- No progress bars in lecture cards
- No completion indicators
- No overall course progress display

4. **No API Endpoints**

- No dedicated API endpoints for progress tracking
- No webhook handlers for progress updates

5. **Hardcoded Status Logic**

- Course status is hardcoded in `CourseList.tsx` (line 47: `course.id === 1 ? "in-progress" : "completed"`)
- No dynamic status calculation based on actual progress

## Recommendations

1. Create GraphQL Operations: Add queries and mutations for progress tracking
2. Implement Video Event Handlers: Add onTimeUpdate and other video events
3. Create Progress API Endpoints: Build API routes for progress updates
4. Add Progress Visualization: Show progress bars and completion status
5. Implement Progress Persistence: Save progress to database periodically
6. Add Progress Recovery: Load saved progress when user returns to course