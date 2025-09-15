Based on my research, I can now provide you with a comprehensive analysis of the student learning progress feature in the LMS course page. Here's what I found:

## Current State Analysis

### ✅ What's Working:

1. Database Schema: The system has well-defined progress tracking tables:

- mix_class_course_progress - tracks overall course progress

- mix_class_lecture_progress - tracks individual lecture progress

- Both tables include fields for progress, status, watch_time, last_watched, etc.

1. Basic Course Structure:

- Course page (/course/[id]) displays lectures in a sidebar

- Video player is functional with basic controls

- Lecture navigation works (clicking lectures switches videos)

1. Status Management:

- Course status enum exists: "not-started", "in-progress", "completed"

- Status display in course list shows hardcoded statuses

### ❌ What's NOT Working (Missing Features):

1. No Progress Tracking Implementation:

- No video event handlers (onTimeUpdate, onProgress)

- No progress persistence to database

- No real-time progress updates

1. No GraphQL Operations for Progress:

- No queries to fetch user progress

- No mutations to update progress

- No progress data in course fragments

1. No Progress Visualization:

- No progress bars in lecture cards

- No completion indicators

- No overall course progress display

1. No API Endpoints:

- No dedicated API endpoints for progress tracking

- No webhook handlers for progress updates

1. Hardcoded Status Logic:

- Course status is hardcoded in CourseList.tsx (line 47: course.id === 1 ? "in-progress" : "completed")

- No dynamic status calculation based on actual progress

## Key Issues to Address:

1. Video Progress Tracking: The video element has no event listeners to track watch time and progress

2. Database Integration: No GraphQL operations to read/write progress data

3. Progress Persistence: No mechanism to save progress to the database

4. UI Feedback: No visual indicators of progress or completion status

5. Real-time Updates: No way to update progress as user watches videos

## Recommended Implementation Plan:

1. Create GraphQL Operations: Add queries and mutations for progress tracking

2. Implement Video Event Handlers: Add onTimeUpdate and other video events

3. Create Progress API Endpoints: Build API routes for progress updates

4. Add Progress Visualization: Show progress bars and completion status

5. Implement Progress Persistence: Save progress to database periodically

6. Add Progress Recovery: Load saved progress when user returns to course

The foundation is there with the database schema, but the actual progress tracking functionality needs to be built from scratch.

For example, in the first problem: 1. **No Progress Tracking Implementation**  
- No video event handlers (onTimeUpdate, onProgress)  
- No progress persistence to database  
- No real-time progress updates  
  
Write me a solution, based on the codebase. No code is needed.

Based on my analysis of the codebase, here's a comprehensive solution for implementing video progress tracking in the LMS course page:

## Solution for Video Progress Tracking Implementation

### 1. Video Event Handlers Implementation

Location: /pages/course/[id].tsx - Modify the existing video element

Approach:

- Add onTimeUpdate event handler to track video progress in real-time

- Implement onLoadedMetadata to get video duration for progress calculation

- Add onEnded event to mark lecture as completed

- Use onPause and onPlay events to track watch time accurately

Key Considerations:

- Debounce progress updates to avoid excessive database calls (every 5-10 seconds)

- Calculate progress percentage: (currentTime / duration) * 100

- Track both lecture-level and course-level progress

- Handle edge cases like video seeking, buffering, and page refresh

### 2. Progress Persistence to Database

GraphQL Operations Needed:

- Create new GraphQL operations in /graphql/operations/:

- upsertLectureProgress.graphql - Update individual lecture progress

- upsertCourseProgress.graphql - Update overall course progress

- getUserProgress.graphql - Fetch existing progress on page load

Database Strategy:

- Use mix_class_lecture_progress table for individual lecture tracking

- Use mix_class_course_progress table for overall course completion

- Implement upsert logic to handle both new and existing progress records

- Store watch_time in seconds and progress as percentage (0-100)

API Integration:

- Create new API endpoint /pages/api/progress/update.ts for progress updates

- Use existing GraphQL client from utils/client.ts

- Implement proper error handling and retry logic

### 3. Real-time Progress Updates

State Management:

- Extend the existing currentLecture state to include progress data

- Add new state for overall course progress tracking

- Implement optimistic updates for better UX

Progress Calculation Logic:

- Lecture progress: (watched_time / lecture_duration) * 100

- Course progress: (completed_lectures / total_lectures) * 100

- Handle partial completion (e.g., 80% watched = in-progress, 100% = completed)

Update Frequency:

- Save progress every 10 seconds during video playback

- Save immediately on video end or lecture change

- Save on page unload to prevent data loss

### 4. Integration with Existing Code

Course Page Modifications:

- Extend the existing CoursePage component state management

- Modify LectureCard component to show progress indicators

- Update the video element with event handlers

- Add progress loading states and error handling

Course List Integration:

- Replace hardcoded status logic in CourseList.tsx

- Fetch real progress data from database

- Display accurate course completion status

- Show progress percentages in course cards

Database Schema Utilization:

- Leverage existing mix_class_course_progress and mix_class_lecture_progress tables

- Use the defined status enums from utils/enum.ts

- Implement proper foreign key relationships

### 5. Technical Implementation Details

Video Event Handler Structure:

typescript

// Pseudo-code structure

const handleTimeUpdate = useCallback(() => {

  // Calculate progress percentage

  // Update local state

  // Debounce database updates

}, [currentLecture, videoRef]);

const handleVideoEnd = useCallback(() => {

  // Mark lecture as completed

  // Update course progress

  // Save to database immediately

}, [currentLecture]);

Progress Data Flow:

1. Video plays → onTimeUpdate fires → Calculate progress → Update local state

2. Every 10 seconds → Save progress to database via GraphQL mutation

3. User changes lecture → Save current progress → Load new lecture progress

4. Page loads → Fetch existing progress from database → Restore video position

Error Handling:

- Implement retry logic for failed database updates

- Store progress locally as fallback (localStorage/sessionStorage)

- Handle network connectivity issues gracefully

- Provide user feedback for progress save failures

### 6. Performance Considerations

Optimization Strategies:

- Debounce database updates to prevent excessive API calls

- Use React.memo for LectureCard components to prevent unnecessary re-renders

- Implement efficient progress calculation algorithms

- Cache progress data in Apollo Client cache

Memory Management:

- Clean up event listeners on component unmount

- Implement proper state cleanup

- Avoid memory leaks in progress tracking logic

This solution leverages the existing codebase structure while adding the missing progress tracking functionality. It maintains consistency with the current architecture and integrates seamlessly with the existing GraphQL setup and database schema.

1. **No GraphQL Operations for Progress**  
- No queries to fetch user progress  
- No mutations to update progress  
- No progress data in course fragments

Based on the codebase analysis, here's a comprehensive solution for implementing GraphQL operations for progress tracking:

## Solution for GraphQL Operations for Progress

### 1. Create Progress GraphQL Fragments

Location: /graphql/fragments/

New Fragment Files Needed:

mix_class_course_progress.graphql:

- Define fragment for course-level progress data

- Include fields: id, course_id, user_id, progress, status, start_date, end_date, last_watched

- Follow existing fragment pattern from mix_class_course.graphql

mix_class_lecture_progress.graphql:

- Define fragment for lecture-level progress data

- Include fields: id, lecture_id, course_id, user_id, progress, status, watch_time, created_at, updated_at

- Match the database schema structure from types/generated.d.ts

### 2. Create Progress Query Operations

Location: /graphql/operations/

getUserCourseProgress.graphql:

- Query to fetch user's progress for a specific course

- Use existing mix_class_course query pattern from mixClassCourseById.graphql

- Include progress data in the course query

- Filter by user_id and course_id

getUserLectureProgress.graphql:

- Query to fetch progress for all lectures in a course

- Return array of lecture progress records

- Include lecture details with progress data

- Use for populating lecture cards with progress indicators

getUserAllCoursesProgress.graphql:

- Query for course list page (/mypage/courses)

- Fetch progress for all user's courses

- Include course details with progress status

- Replace hardcoded status logic in CourseList.tsx

### 3. Create Progress Mutation Operations

upsertCourseProgress.graphql:

- Mutation to create or update course-level progress

- Use insert_mix_class_course_progress with on_conflict resolution

- Handle both new progress creation and existing progress updates

- Follow pattern from existing mutations in pages/api/

upsertLectureProgress.graphql:

- Mutation to create or update lecture-level progress

- Use insert_mix_class_lecture_progress with upsert logic

- Handle progress percentage and watch time updates

- Support batch updates for multiple lectures

updateLectureProgress.graphql:

- Simple update mutation for real-time progress tracking

- Update only progress, watch_time, and last_watched fields

- Optimized for frequent updates during video playback

### 4. Update Existing Course Fragments

Modify mix_class_course.graphql:

- Add progress relationship to existing course fragment

- Include mix_class_course_progresses field

- Add computed progress percentage field

- Maintain backward compatibility with existing queries

Modify mix_class_lecture.graphql:

- Add progress relationship to lecture fragment

- Include mix_class_lecture_progresses field

- Add progress status and completion indicators

- Support progress visualization in lecture cards

### 5. Integration with Existing Code

Update Course Page Query:

- Modify mixClassCourseById.graphql to include progress data

- Add progress fragments to the existing query

- Filter progress by current user ID

- Load existing progress on page initialization

Update Course List Query:

- Modify mixClassCourse.graphql to include progress data

- Add progress status calculation

- Replace hardcoded status logic in CourseList.tsx

- Display real progress percentages

Update Generated Types:

- Run GraphQL codegen to generate new TypeScript types

- Update graphql/generated.tsx with new operations

- Ensure type safety for progress data structures

### 6. Progress Data Structure Design

Course Progress Schema:

typescript

// Based on existing Mix_Class_Course_Progress type

interface CourseProgress {

  id: number;

  course_id: number;

  user_id: string;

  progress: number; // 0-100 percentage

  status: 'not-started' | 'in-progress' | 'completed';

  start_date?: string;

  end_date?: string;

  last_watched?: string;

}

Lecture Progress Schema:

typescript

// Based on existing Mix_Class_Lecture_Progress type

interface LectureProgress {

  id: number;

  lecture_id: number;

  course_id: number;

  user_id: string;

  progress: number; // 0-100 percentage

  status: 'not-started' | 'in-progress' | 'completed';

  watch_time: number; // seconds watched

  created_at: string;

  updated_at: string;

}

### 7. Query Optimization Strategy

Efficient Data Fetching:

- Use single query to fetch course with all progress data

- Implement proper caching with Apollo Client

- Use fetchPolicy: "cache-and-network" for real-time updates

- Minimize over-fetching with specific field selection

Progress Calculation:

- Calculate course progress from lecture progress data

- Implement server-side progress aggregation where possible

- Use GraphQL computed fields for derived progress values

- Cache progress calculations to avoid repeated computation

### 8. Error Handling and Fallbacks

GraphQL Error Handling:

- Implement proper error boundaries for progress queries

- Handle network failures gracefully

- Provide fallback UI when progress data is unavailable

- Log progress tracking errors for debugging

Data Consistency:

- Implement optimistic updates for better UX

- Handle race conditions in progress updates

- Ensure data consistency between course and lecture progress

- Implement retry logic for failed mutations

### 9. Integration Points

Course Page Integration:

- Update CoursePage component to use new progress queries

- Modify LectureCard to display progress indicators

- Implement progress loading states

- Handle progress data in video event handlers

Course List Integration:

- Update CourseList component to use real progress data

- Replace hardcoded status with dynamic progress calculation

- Add progress visualization (progress bars, completion badges)

- Implement progress-based sorting and filtering

Authentication Integration:

- Ensure progress queries are properly authenticated

- Use existing session management from useAuth hook

- Implement proper user ID filtering in all progress queries

- Maintain security for user-specific progress data

This solution creates a complete GraphQL layer for progress tracking that integrates seamlessly with the existing codebase architecture, following the established patterns and maintaining consistency with the current GraphQL setup.

1. **No Progress Visualization**  
- No progress bars in lecture cards  
- No completion indicators  
- No overall course progress display

Based on the codebase analysis, here's a comprehensive solution for implementing progress visualization in the LMS:

## Solution for Progress Visualization

### 1. Lecture Card Progress Visualization

Location: /components/domain/class/LectureCard.tsx

Visual Elements to Add:

- Progress Bar: Horizontal progress bar showing completion percentage

- Completion Badge: Checkmark or completion icon for finished lectures

- Progress Percentage: Text display of progress (e.g., "75% 완료")

- Watch Time Indicator: Show actual time watched vs total duration

- Status Color Coding: Different colors for not-started, in-progress, completed

Design Integration:

- Extend existing Card styled component with progress elements

- Use existing theme colors from styles/theme.ts

- Follow current design patterns from other components

- Maintain responsive design for different screen sizes

Progress Bar Implementation:

- Create new styled component for progress bar

- Use CSS gradients or background colors for visual appeal

- Implement smooth animations for progress updates

- Handle edge cases (0%, 100%, loading states)

### 2. Course List Progress Display

Location: /components/domain/class/CourseList.tsx

Visual Enhancements:

- Course Progress Bar: Overall completion percentage for each course

- Status Badges: Visual indicators for course status (not-started, in-progress, completed)

- Progress Statistics: Show completed lectures vs total lectures

- Last Watched Info: Display when user last accessed the course

- Completion Date: Show when course was completed

Status Display Updates:

- Replace hardcoded status logic (line 47: course.id === 1 ? "in-progress" : "completed")

- Use real progress data from GraphQL queries

- Implement dynamic status calculation based on progress percentage

- Add hover effects and interactive elements

Progress Calculation Logic:

- Course progress = (completed lectures / total lectures) * 100

- Status determination: 0% = not-started, 1-99% = in-progress, 100% = completed

- Handle edge cases and partial completion scenarios

### 3. Course Page Overall Progress Display

Location: /pages/course/[id].tsx

Header Progress Section:

- Course Progress Bar: Large progress bar at the top of the course page

- Progress Statistics: "3 of 10 lectures completed" display

- Estimated Time Remaining: Calculate based on remaining content

- Completion Percentage: Prominent display of overall progress

Lecture List Enhancements:

- Progress Indicators: Mini progress bars in each lecture card

- Completion Status: Visual checkmarks for completed lectures

- Current Lecture Highlighting: Enhanced highlighting for active lecture

- Progress Summary: Collapsible section showing detailed progress breakdown

Visual Hierarchy:

- Use existing Text component variants for different text sizes

- Implement proper spacing using existing design system

- Add subtle animations for progress updates

- Ensure accessibility with proper contrast and focus states

### 4. Progress Component Library

New Components to Create:

ProgressBar.tsx:

- Reusable progress bar component

- Support for different sizes (small, medium, large)

- Customizable colors and animations

- Accessibility features (ARIA labels, keyboard navigation)

ProgressBadge.tsx:

- Status indicator badges

- Different styles for each status type

- Icon integration with existing Icon component

- Consistent styling across the application

ProgressStats.tsx:

- Statistics display component

- Shows completion ratios and percentages

- Time-based statistics (watch time, remaining time)

- Responsive layout for different screen sizes

### 5. Visual Design System Integration

Theme Integration:

- Use existing color palette from styles/theme.ts

- Implement progress-specific color variants

- Ensure consistency with current design language

- Support dark/light theme variations

Animation System:

- Leverage existing animation utilities from styles/animation/

- Implement smooth progress bar animations

- Add micro-interactions for better UX

- Use CSS transitions for state changes

Typography Integration:

- Use existing Text component variants

- Implement proper text hierarchy for progress information

- Ensure readability across different progress states

- Support internationalization for Korean text

### 6. Progress Data Visualization

Chart Components:

- Circular Progress: For overall course completion

- Linear Progress: For individual lecture progress

- Timeline View: Show learning journey and milestones

- Statistics Dashboard: Comprehensive progress overview

Data Representation:

- Progress percentages with visual indicators

- Time-based progress (minutes watched vs total duration)

- Completion milestones and achievements

- Learning streak and consistency metrics

### 7. Interactive Progress Features

User Interactions:

- Click to Resume: Click on lecture to resume from last position

- Progress Hover: Show detailed progress info on hover

- Quick Actions: Mark as complete, reset progress options

- Progress Filtering: Filter lectures by completion status

Real-time Updates:

- Live progress updates during video playback

- Smooth animations for progress changes

- Visual feedback for progress saves

- Error states for failed progress updates

### 8. Accessibility and Usability

Accessibility Features:

- ARIA labels for screen readers

- Keyboard navigation support

- High contrast mode support

- Focus indicators for interactive elements

Mobile Responsiveness:

- Touch-friendly progress indicators

- Optimized layouts for mobile devices

- Swipe gestures for progress navigation

- Responsive text and icon sizes

### 9. Performance Optimization

Rendering Optimization:

- Use React.memo for progress components

- Implement efficient re-rendering strategies

- Optimize progress calculations

- Minimize DOM updates during progress changes

Visual Performance:

- Use CSS transforms for smooth animations

- Implement efficient progress bar rendering

- Optimize icon and image loading

- Handle large numbers of progress indicators

### 10. Integration with Existing Components

Component Updates:

- Modify LectureCard to include progress visualization

- Update CourseList to show real progress data

- Enhance CoursePage with progress display

- Integrate with existing Button and Text components

State Management:

- Extend existing state structures for progress data

- Implement progress state synchronization

- Handle progress loading and error states

- Maintain consistency across components

API Integration:

- Connect progress visualization to GraphQL queries

- Implement real-time progress updates

- Handle progress data caching

- Ensure data consistency across views

This solution creates a comprehensive progress visualization system that enhances the user experience while maintaining consistency with the existing design system and codebase architecture.

1. **No API Endpoints**  
- No dedicated API endpoints for progress tracking  
- No webhook handlers for progress updates

Based on the codebase analysis, here's a comprehensive solution for implementing API endpoints for progress tracking:

## Solution for API Endpoints for Progress Tracking

### 1. Progress Tracking API Endpoints

Location: /pages/api/progress/

New API Endpoint Structure:

/pages/api/progress/update.ts:

- Purpose: Update individual lecture progress in real-time

- Method: POST

- Payload: { lectureId, courseId, progress, watchTime, status }

- Response: Updated progress data or error

- Rate Limiting: Implement debouncing to prevent excessive calls

- Authentication: Use existing session validation from useAuth hook

/pages/api/progress/batch-update.ts:

- Purpose: Update multiple lecture progress records at once

- Method: POST

- Payload: Array of progress updates

- Use Case: When user completes multiple lectures or switches between lectures

- Optimization: Reduce database calls by batching updates

/pages/api/progress/course-completion.ts:

- Purpose: Handle course completion logic and status updates

- Method: POST

- Payload: { courseId, completionData }

- Logic: Calculate overall course progress, update course status

- Triggers: Send completion notifications, update user achievements

### 2. Progress Data Retrieval Endpoints

/pages/api/progress/user-progress.ts:

- Purpose: Fetch user's progress for all courses

- Method: GET

- Query Params: userId, courseId (optional)

- Response: Complete progress data with course and lecture details

- Caching: Implement Redis caching for frequently accessed data

- Use Case: Course list page, progress dashboard

/pages/api/progress/lecture-progress.ts:

- Purpose: Get specific lecture progress data

- Method: GET

- Query Params: lectureId, userId

- Response: Detailed lecture progress with watch history

- Use Case: Resume video from last position, show detailed progress

/pages/api/progress/statistics.ts:

- Purpose: Get aggregated progress statistics

- Method: GET

- Query Params: userId, timeRange (optional)

- Response: Learning analytics, completion rates, time spent

- Use Case: Progress dashboard, learning insights

### 3. Webhook Handlers for Progress Updates

Location: /pages/api/progress/webhooks/

/pages/api/progress/webhooks/sync-progress.ts:

- Purpose: Sync progress data across different devices/sessions

- Trigger: When user logs in from different device

- Logic: Merge progress data, resolve conflicts

- Use Case: Multi-device learning experience

/pages/api/progress/webhooks/cleanup.ts:

- Purpose: Clean up old or invalid progress data

- Trigger: Scheduled job or manual trigger

- Logic: Remove duplicate entries, fix data inconsistencies

- Maintenance: Keep database clean and optimized

/pages/api/progress/webhooks/analytics.ts:

- Purpose: Send progress data to analytics services

- Trigger: On significant progress milestones

- Integration: Connect with existing GA tracking from lib/ga.ts

- Data: Learning patterns, completion rates, engagement metrics

### 4. API Implementation Patterns

Follow Existing Patterns:

- Use same error handling as pages/api/payments/confirm.ts

- Implement similar authentication as pages/api/oauth/ endpoints

- Follow GraphQL client pattern from existing API files

- Use consistent response format across all endpoints

Error Handling Strategy:

typescript

// Based on existing patterns from payments API

try {

  // Progress update logic

  res.status(200).json({ success: true, data: updatedProgress });

} catch (error) {

  console.error("[PROGRESS_UPDATE_FAILED]", error);

  res.status(400).json({ 

    success: false, 

    error: "Progress update failed",

    code: "PROGRESS_UPDATE_ERROR" 

  });

}

Authentication Integration:

- Use existing session validation from utils/auth.ts

- Implement user ID extraction from session

- Ensure progress data belongs to authenticated user

- Handle unauthorized access gracefully

### 5. Database Integration

GraphQL Mutations:

- Use existing GraphQL client from utils/client.ts

- Implement upsert operations for progress tables

- Handle database transactions for data consistency

- Use admin client for progress operations (similar to payments API)

Data Validation:

- Validate progress percentages (0-100)

- Ensure watch time is reasonable

- Check lecture/course ownership

- Validate status transitions

Optimization Strategies:

- Implement database connection pooling

- Use prepared statements for frequent queries

- Cache frequently accessed progress data

- Implement efficient indexing for progress queries

### 6. Real-time Progress Updates

WebSocket Integration (Optional):

- /pages/api/progress/websocket.ts:

- Real-time progress synchronization across devices

- Live progress updates during video playback

- Collaborative learning features

- Instant progress sharing

Server-Sent Events:

- /pages/api/progress/stream.ts:

- Stream progress updates to client

- Reduce polling overhead

- Better performance for real-time updates

- Fallback for WebSocket connections

### 7. Progress Analytics Endpoints

/pages/api/progress/analytics/learning-patterns.ts:

- Purpose: Analyze user learning patterns

- Data: Watch time patterns, completion rates, learning streaks

- Use Case: Personalized learning recommendations

/pages/api/progress/analytics/course-insights.ts:

- Purpose: Course-level analytics for instructors

- Data: Student progress, common drop-off points, completion rates

- Privacy: Aggregate data only, no individual tracking

/pages/api/progress/analytics/engagement.ts:

- Purpose: Track learning engagement metrics

- Data: Session duration, video interaction, progress velocity

- Integration: Connect with existing GA tracking

### 8. API Security and Rate Limiting

Rate Limiting Implementation:

- Limit progress updates to prevent abuse

- Implement per-user rate limits

- Use Redis for distributed rate limiting

- Handle rate limit exceeded gracefully

Data Security:

- Validate all input data

- Sanitize progress data before database storage

- Implement proper CORS policies

- Use HTTPS for all progress endpoints

Access Control:

- Ensure users can only access their own progress

- Implement proper authorization checks

- Validate course/lecture ownership

- Handle edge cases and security vulnerabilities

### 9. API Documentation and Testing

API Documentation:

- Document all endpoints with OpenAPI/Swagger

- Include request/response examples

- Document error codes and handling

- Provide integration examples

Testing Strategy:

- Unit tests for each endpoint

- Integration tests with database

- Load testing for high-traffic scenarios

- Error scenario testing

### 10. Integration with Existing Systems

GraphQL Integration:

- Use existing GraphQL operations where possible

- Create new mutations for progress-specific operations

- Maintain consistency with existing query patterns

- Leverage existing type definitions

Authentication Integration:

- Use existing session management

- Integrate with useAuth hook

- Maintain security standards

- Handle session expiration gracefully

Error Handling Integration:

- Use existing MixError patterns from models/Error.ts

- Implement consistent error responses

- Log errors using existing logging systems

- Provide user-friendly error messages

Monitoring and Logging:

- Integrate with existing monitoring systems

- Log progress tracking events

- Monitor API performance

- Track error rates and patterns

This solution creates a comprehensive API layer for progress tracking that integrates seamlessly with the existing codebase architecture, following established patterns and maintaining consistency with the current system design.

1. **Hardcoded Status Logic**  
- Course status is hardcoded in `CourseList.tsx` (line 47: `course.id === 1 ? "in-progress" : "completed"`)  
- No dynamic status calculation based on actual progress

Based on the codebase analysis, here's a comprehensive solution for replacing hardcoded status logic with dynamic progress-based calculations:

## Solution for Dynamic Status Calculation

### 1. Replace Hardcoded Status Logic in CourseList.tsx

Current Problem (Line 47):

typescript

const status = course.id === 1 ? "in-progress" : "completed";

Solution Approach:

- Remove hardcoded status assignment

- Implement dynamic status calculation based on real progress data

- Create utility functions for status determination

- Integrate with GraphQL progress queries

Implementation Strategy:

- Fetch progress data for each course

- Calculate status based on completion percentage

- Handle edge cases and loading states

- Implement proper error handling

### 2. Create Progress Calculation Utilities

Location: /utils/progress.ts (new file)

Status Calculation Logic:

typescript

// Based on existing COURSE_STATUS_ENUM_KOREAN from utils/enum.ts

export const calculateCourseStatus = (progress: number): string => {

  if (progress === 0) return "not-started";

  if (progress === 100) return "completed";

  return "in-progress";

};

export const calculateProgressPercentage = (

  completedLectures: number,

  totalLectures: number

): number => {

  if (totalLectures === 0) return 0;

  return Math.round((completedLectures / totalLectures) * 100);

};

Progress Aggregation Functions:

- Calculate overall course progress from lecture progress

- Determine completion status based on thresholds

- Handle partial completion scenarios

- Implement progress validation logic

### 3. Update CourseList Component Logic

Modify CourseList.tsx:

- Remove hardcoded status logic

- Add progress data fetching

- Implement dynamic status calculation

- Add loading states for progress data

- Handle cases where progress data is unavailable

New Component Structure:

typescript

// Replace hardcoded logic with dynamic calculation

const getCourseStatus = (course: CourseWithProgress) => {

  const progress = calculateProgressPercentage(

    course.completed_lectures || 0,

    course.total_lectures || 0

  );

  return calculateCourseStatus(progress);

};

Progress Data Integration:

- Fetch progress data alongside course data

- Use existing GraphQL queries with progress fragments

- Implement proper data loading states

- Handle progress data errors gracefully

### 4. Create Progress-Aware Course Type

Type Definition Updates:

typescript

// Extend existing course type with progress data

interface CourseWithProgress extends MixClassCourse {

  progress?: {

    percentage: number;

    completed_lectures: number;

    total_lectures: number;

    last_watched?: string;

    status: 'not-started' | 'in-progress' | 'completed';

  };

}

GraphQL Fragment Updates:

- Modify mixClassCourse.graphql to include progress data

- Add progress calculation fields

- Include lecture completion counts

- Add last watched timestamp

### 5. Implement Dynamic Status Display

Status Display Logic:

- Use existing STATUS_INFO object from utils/enum.ts

- Map calculated status to display properties

- Implement proper color coding based on status

- Add progress percentage display

Visual Status Indicators:

- Replace static status with dynamic progress bars

- Show completion percentages

- Display last watched information

- Add progress-based visual cues

### 6. Create Progress Calculation Hooks

Location: /hooks/useCourseProgress.ts (new file)

Hook Implementation:

typescript

export const useCourseProgress = (courseId: number) => {

  // Fetch progress data

  // Calculate status and percentage

  // Return progress information

  // Handle loading and error states

};

Progress State Management:

- Manage progress data state

- Implement progress calculation logic

- Handle real-time progress updates

- Provide progress-related utilities

### 7. Update Course Page Progress Logic

Course Page Integration:

- Implement dynamic progress calculation for current course

- Update lecture progress based on actual data

- Calculate overall course completion

- Display real-time progress updates

Lecture Progress Calculation:

- Calculate individual lecture completion

- Determine lecture status based on watch time

- Update course progress when lectures are completed

- Handle progress persistence

### 8. Implement Progress-Based Features

Smart Course Recommendations:

- Recommend courses based on completion patterns

- Suggest next courses based on progress

- Implement learning path suggestions

- Add progress-based course filtering

Progress Analytics:

- Track learning velocity

- Calculate completion rates

- Monitor learning patterns

- Generate progress insights

### 9. Handle Edge Cases and Error States

Data Validation:

- Validate progress data integrity

- Handle missing progress data

- Implement fallback status logic

- Ensure data consistency

Error Handling:

- Handle GraphQL query errors

- Implement progress calculation fallbacks

- Display appropriate error states

- Provide user feedback for errors

Loading States:

- Show loading indicators while fetching progress

- Implement skeleton loading for course cards

- Handle partial data loading

- Provide smooth loading transitions

### 10. Performance Optimization

Efficient Progress Calculation:

- Cache progress calculations

- Implement memoization for expensive operations

- Use efficient data structures

- Minimize unnecessary recalculations

Data Fetching Optimization:

- Batch progress queries

- Implement proper caching strategies

- Use optimistic updates

- Reduce API calls through smart fetching

### 11. Integration with Existing Systems

GraphQL Integration:

- Extend existing queries with progress data

- Use existing fragments and types

- Maintain backward compatibility

- Follow existing query patterns

State Management Integration:

- Integrate with existing Apollo Client cache

- Use existing state management patterns

- Implement proper cache invalidation

- Maintain data consistency

UI Component Integration:

- Update existing components with progress data

- Maintain existing design patterns

- Implement consistent progress display

- Ensure responsive design

### 12. Testing and Validation

Unit Testing:

- Test progress calculation functions

- Validate status determination logic

- Test edge cases and error scenarios

- Ensure calculation accuracy

Integration Testing:

- Test progress data fetching

- Validate GraphQL integration

- Test real-time progress updates

- Ensure proper error handling

User Experience Testing:

- Validate progress display accuracy

- Test loading states and transitions

- Ensure responsive design

- Verify accessibility compliance

This solution completely eliminates hardcoded status logic and replaces it with a dynamic, data-driven system that accurately reflects actual user progress, providing a much more meaningful and accurate learning experience