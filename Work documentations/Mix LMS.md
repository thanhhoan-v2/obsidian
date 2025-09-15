# Mix Class Test Build - Project Documentation

## Project Overview

**Project ID:** MIX142  
**Project Name:** Mix Class Test Build  
**Team Members:** Kim Young-chan, Jang Kyung-eun, Yoon Ji-ho  
**Impact Score:** 3  
**Effort Score:** 5  
**Status:** QA in Progress

### Sub-tasks Included

- Classroom video player
- Page routing setup
- Course component
- Lecture list component
- Tablet screen ratio planning
- Completed lecture display indicators

---

## Development Kanban Board

|Task Name|Status|Assignee|ID|
|---|---|---|---|
|Course Component|Deployed ✅|Kim Young-chan|MIX146|
|Page Routing Setup|Deployed ✅|Kim Young-chan|MIX145|
|Lecture List Component|Deployed ✅|Jang Kyung-eun|MIX144|
|Classroom Video Player|Deployed ✅|Jang Kyung-eun|MIX143|
|Tablet Screen Ratio Planning|Deployed ✅|Yoon Ji-ho|MIX158|
|Completed Lecture Display|Deployed ✅|Yoon Ji-ho|MIX159|

**Project Philosophy:** "Why create a test build? → Implement minimal functionality first, then make next decisions"

---

## Table of Contents

0. What the completed test build will look like
1. Introduction
2. Entities and Attributes
3. Requirements Definition
4. Storyboard
5. UI Design

---

## 1. Introduction

### Terminology Definitions

To prevent future confusion, we're establishing standard terminology:

**Korean Standard Terms:**

- **강좌 (Gangwa)** = Course: A systematic educational program designed for learning a specific topic
- **강의 (Gang-ui)** = Lecture: Individual lessons on specific topics

**Competitor Terminology Variations:**

- Courses are called: Class, Course
- Lectures are called: Session, Lecture, Lesson

**Mix Class Terminology:**

- **Educational Service Name:** Mix Class
- **Course:** Course
- **Lecture:** Lecture

---

## 2. Entities and Attributes

_Note: Attributes not used in the test build are marked with strikethrough_

### Course Attributes (Collected from other online education sites)

- Course name
- Course thumbnail
- Progress rate
- Instructor name
- Total number of lectures
- Total lecture duration
- Course access period
- ~~Certificate availability~~
- ~~Course difficulty level~~
- ~~Course description~~
- ~~Course objectives~~
- Lecture sequence
- ~~Study materials availability~~
- ~~Course publication date~~
- ~~Last update date~~
- ~~Number of reviews~~
- ~~Average satisfaction rating (5-point scale)~~
- ~~Number of students~~
- ~~Category~~
- Learning status (Not started, In progress, Completed)
- ~~Course price~~
- ~~Course review count~~
- ~~Newly uploaded course indicator~~
- ~~Shopping cart button~~
- ~~Bookmark/Like button~~

### Database Schema

#### User

- **UserID** (PK): Unique user identifier (already exists)
- **Name**: User name (already exists)
- **Email**: User email (already exists)
- **IsInternal**: Internal employee status

_Note: Distinguishes between internal employees and regular users. Only internal employees can access "My Classroom" menu._

#### MyClassroom (My Classroom)

- **ClassroomID** (PK): Unique classroom identifier
- **UserID** (FK): User ID (references User table)
- **CourseID** (FK): Course ID (references Course table)
- **AssignedBy**: Administrator who assigned the course (Admin UserID)
- **AssignedDate**: Course assignment date

#### Enrollment (Enrollment Record)

- **EnrollmentID** (PK): Unique enrollment identifier
- **UserID** (FK): User ID (references User table)
- **CourseID** (FK): Course ID (references Course table)
- **ProgressRate**: Progress percentage
- **ExpiryDate**: Course expiration date

#### Course

- **CourseID** (PK): Unique course identifier
- **Title**: Course title
- **Instructor**: Instructor name
- **ImgUrl**: Course thumbnail hosting address
- **Description**: Course description
- **Duration**: Course access period
- **Created_at**: Creation date

~~**Existing Purchase-Related Attributes:**~~

- ~~price: Regular price~~
- ~~stock: Total inventory~~
- ~~discount_price: Discounted price~~
- ~~discount_stock: Discount inventory~~
- ~~discount_start_time: Discount start time~~
- ~~discount_end_time: Discount end time~~
- ~~order_end_time: Product sales end time~~

#### Lecture

- **LectureID** (PK): Unique lecture identifier
- **CourseID** (FK): Course ID (references Course table)
- **Title**: Lecture title
- **Order**: Lecture sequence
- **MaterialID** (FK): Learning material ID (references Material table)

#### ~~Material (Learning Materials)~~

- **MaterialID** (PK): Unique material identifier
- **LectureID** (FK): Lecture ID (references Lecture table)
- **FileName**: File name
- **FilePath**: File path

#### ~~Test~~

- **TestID** (PK): Unique test identifier
- **CourseID** (FK): Course ID (references Course table)
- **TestTitle**: Test title
- **MaxScore**: Maximum score

#### ~~TestResult (Test Results)~~

- **ResultID** (PK): Unique result identifier
- **TestID** (FK): Test ID (references Test table)
- **UserID** (FK): User ID (references User table)
- **Score**: Score achieved
- **DateTaken**: Test date

#### ~~Feedback~~

- **FeedbackID** (PK): Unique feedback identifier
- **CourseID** (FK): Course ID (references Course table)
- **UserID** (FK): User ID (references User table)
- **Rating**: Rating (1-5 scale)
- **Comment**: Review content

#### ~~Analytics~~

- **AnalyticsID** (PK): Unique analytics identifier
- **CourseID** (FK): Course ID (references Course table)
- **AverageProgress**: Average progress rate
- **AverageRating**: Average rating
- **TotalEnrollments**: Total number of enrolled students

---

## 3. Requirements Definition

### Pages to Implement

**Total: 3 pages**

1. My Page (existing page with added "My Classroom" menu)
2. My Classroom
3. Lecture Playback Page

### User Journey 

1. Access "My Page"
2. Navigate to "My Classroom"
3. View available courses
4. Click Course to navigate to Lecture Playback Page
5. View Complete Lecture List in Lecture Playback Page
6. Select & Play lecture

### User Stories

|Category|Story|
|---|---|
|My Page|Users can navigate from "My Page" to "My Classroom"|
|My Classroom|Users can view available courses in "My Classroom"|
|My Classroom|Users can click on a course to navigate to "Lecture Playback Page"|
|Lecture Playback Page|Users can view the complete lecture list and sequence|
|Lecture Playback Page|Users can click on lectures in the list to play them|

---

## 4. Storyboard

_Reference: 내 강의실_SB_ver.1.1.pptx_

### User Flow Diagram

```
1. My Page Access
    ↓
2. Navigate to 'My Classroom'
    ↓
3. View Available Courses
    ↓
4. Click Course to Access 'Lecture Playback Page'
    ↓
5. View Complete Lecture List in Playback Page
    ↓
6. Select and Play Individual Lectures
```

---

## 5. UI Design

**Design Reference:** [Figma Link](https://www.figma.com/design/zR0IEvyuT3deNI9aDCwnGD/MIX?node-id=907428714&t=ceCBRu7q2QLL1vxZ1)

---

## Project Notes

- This is a **test build** focused on implementing core functionality with minimal features
- The goal is to validate the concept before making further product decisions
- All development tasks have been completed and are in QA phase
- The system is designed primarily for internal employees initially
- Future iterations will expand functionality based on test build feedback