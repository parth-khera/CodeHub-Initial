# Database Schema for CodeHub

## Overview
The database uses PostgreSQL for relational data management. Schema is normalized to reduce redundancy and ensure data integrity. Indexes on frequently queried fields for performance.

## Tables and Relations

### 1. Users
- **Purpose**: Store user accounts and profiles
- **Fields**:
  - id (UUID, PK)
  - email (VARCHAR, UNIQUE)
  - username (VARCHAR, UNIQUE)
  - password_hash (VARCHAR)
  - role (ENUM: user, admin)
  - created_at (TIMESTAMP)
  - updated_at (TIMESTAMP)
  - profile_image_url (VARCHAR, NULL)
  - bio (TEXT, NULL)
  - xp (INTEGER, DEFAULT 0)
  - streak_days (INTEGER, DEFAULT 0)
  - last_activity (TIMESTAMP)

### 2. Courses
- **Purpose**: Learning paths and courses
- **Fields**:
  - id (UUID, PK)
  - title (VARCHAR)
  - description (TEXT)
  - difficulty (ENUM: beginner, intermediate, advanced)
  - created_by (UUID, FK to Users)
  - created_at (TIMESTAMP)
  - updated_at (TIMESTAMP)
  - is_published (BOOLEAN, DEFAULT FALSE)

### 3. Lessons
- **Purpose**: Individual lessons within courses
- **Fields**:
  - id (UUID, PK)
  - course_id (UUID, FK to Courses)
  - title (VARCHAR)
  - content (TEXT)  // Markdown or HTML
  - order_index (INTEGER)
  - created_at (TIMESTAMP)
  - updated_at (TIMESTAMP)

### 4. Problems
- **Purpose**: Coding problems
- **Fields**:
  - id (UUID, PK)
  - lesson_id (UUID, FK to Lessons, NULL)  // Can be standalone
  - title (VARCHAR)
  - description (TEXT)
  - difficulty (ENUM: easy, medium, hard)
  - category (VARCHAR)  // e.g., DSA, SQL, System Design
  - supported_languages (JSON)  // Array of languages
  - time_limit (INTEGER)  // in seconds
  - memory_limit (INTEGER)  // in MB
  - test_cases (JSON)  // Hidden test cases
  - sample_inputs (JSON)
  - sample_outputs (JSON)
  - created_by (UUID, FK to Users)
  - created_at (TIMESTAMP)
  - updated_at (TIMESTAMP)
  - is_published (BOOLEAN, DEFAULT FALSE)

### 5. Submissions
- **Purpose**: User code submissions
- **Fields**:
  - id (UUID, PK)
  - user_id (UUID, FK to Users)
  - problem_id (UUID, FK to Problems)
  - code (TEXT)
  - language (VARCHAR)
  - status (ENUM: pending, accepted, wrong_answer, time_limit, runtime_error)
  - execution_time (FLOAT, NULL)
  - memory_used (FLOAT, NULL)
  - submitted_at (TIMESTAMP)
  - test_results (JSON)  // Detailed results

### 6. Progress
- **Purpose**: Track user progress
- **Fields**:
  - id (UUID, PK)
  - user_id (UUID, FK to Users)
  - course_id (UUID, FK to Courses, NULL)
  - lesson_id (UUID, FK to Lessons, NULL)
  - problem_id (UUID, FK to Problems, NULL)
  - status (ENUM: not_started, in_progress, completed)
  - completed_at (TIMESTAMP, NULL)
  - score (INTEGER, NULL)

### 7. Achievements
- **Purpose**: Badges and achievements
- **Fields**:
  - id (UUID, PK)
  - user_id (UUID, FK to Users)
  - achievement_type (VARCHAR)  // e.g., first_solve, streak_30
  - description (TEXT)
  - unlocked_at (TIMESTAMP)

### 8. Leaderboard_Stats
- **Purpose**: Aggregate stats for leaderboards
- **Fields**:
  - id (UUID, PK)
  - user_id (UUID, FK to Users)
  - total_solves (INTEGER, DEFAULT 0)
  - total_xp (INTEGER, DEFAULT 0)
  - ranking (INTEGER, NULL)  // Computed periodically
  - updated_at (TIMESTAMP)

## Relations
- Users → Submissions (1:N)
- Users → Progress (1:N)
- Users → Achievements (1:N)
- Users → Leaderboard_Stats (1:1)
- Courses → Lessons (1:N)
- Lessons → Problems (1:N)
- Problems → Submissions (1:N)
- Courses/Lessons/Problems → Progress (1:N via FKs)

## Indexes
- Users: email, username
- Problems: difficulty, category
- Submissions: user_id, problem_id, status
- Progress: user_id, status
- Leaderboard_Stats: ranking

## Constraints
- Foreign keys with CASCADE on delete where appropriate
- Unique constraints on critical fields
- Check constraints for enums and positive values

This schema supports the core features while allowing for future extensions like tags, comments, or social features.