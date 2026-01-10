# API Design for CodeHub

## Overview
APIs follow REST principles with versioning (/api/v1/). JSON responses. Authentication via JWT in Authorization header. Rate limiting applied. Error responses standardized.

## Authentication Endpoints

### POST /api/v1/auth/login
- **Body**: { email, password }
- **Response**: { access_token, refresh_token, user }
- **Purpose**: User login

### POST /api/v1/auth/signup
- **Body**: { email, username, password }
- **Response**: { access_token, refresh_token, user }
- **Purpose**: User registration

### POST /api/v1/auth/refresh
- **Body**: { refresh_token }
- **Response**: { access_token }
- **Purpose**: Refresh access token

### POST /api/v1/auth/logout
- **Headers**: Authorization: Bearer <token>
- **Response**: { message: "Logged out" }
- **Purpose**: Invalidate tokens

## User Endpoints

### GET /api/v1/users/me
- **Auth**: Required
- **Response**: User profile data
- **Purpose**: Get current user info

### PUT /api/v1/users/me
- **Auth**: Required
- **Body**: Partial user update
- **Response**: Updated user
- **Purpose**: Update profile

### GET /api/v1/users/{id}/stats
- **Auth**: Required
- **Response**: { xp, streak, solves }
- **Purpose**: User stats

## Course Endpoints

### GET /api/v1/courses
- **Query**: ?difficulty=intermediate&page=1
- **Response**: Paginated list of courses
- **Purpose**: List courses

### GET /api/v1/courses/{id}
- **Response**: Course details with lessons
- **Purpose**: Get course info

### POST /api/v1/courses (Admin only)
- **Auth**: Admin
- **Body**: Course data
- **Response**: Created course
- **Purpose**: Create course

## Problem Endpoints

### GET /api/v1/problems
- **Query**: ?difficulty=easy&category=DSA&page=1
- **Response**: Paginated problems
- **Purpose**: List problems

### GET /api/v1/problems/{id}
- **Response**: Problem details (no test cases)
- **Purpose**: Get problem

### POST /api/v1/problems (Admin only)
- **Auth**: Admin
- **Body**: Problem data
- **Response**: Created problem

## Submission Endpoints

### POST /api/v1/submissions
- **Auth**: Required
- **Body**: { problem_id, code, language }
- **Response**: Submission status (async)
- **Purpose**: Submit code

### GET /api/v1/submissions/{id}
- **Auth**: Required (own submissions)
- **Response**: Submission result
- **Purpose**: Get submission details

### GET /api/v1/submissions?problem_id={id}
- **Auth**: Required
- **Response**: User's submissions for problem

## Progress Endpoints

### GET /api/v1/progress
- **Auth**: Required
- **Response**: User's progress across courses/problems
- **Purpose**: Get progress

### PUT /api/v1/progress/{id}
- **Auth**: Required
- **Body**: { status: "completed" }
- **Response**: Updated progress

## Leaderboard Endpoints

### GET /api/v1/leaderboard
- **Query**: ?period=weekly&page=1
- **Response**: Paginated leaderboard
- **Purpose**: Global rankings

### GET /api/v1/leaderboard/me
- **Auth**: Required
- **Response**: User's rank

## Admin Endpoints

### GET /api/v1/admin/users
- **Auth**: Admin
- **Response**: User list

### PUT /api/v1/admin/problems/{id}/publish
- **Auth**: Admin
- **Response**: Publish problem

## Error Handling
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

Response format: { error: { code, message } }

## Rate Limiting
- Auth endpoints: 5/min
- Submissions: 10/min per user
- General: 100/min per user

## Versioning
- Current: v1
- Future versions for breaking changes

This API design supports all core features with extensibility for future additions.