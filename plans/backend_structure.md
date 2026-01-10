# Backend Folder Structure for CodeHub

## Overview
NestJS with modular architecture. Separation by domain (auth, users, problems). Clean architecture with controllers, services, entities.

## Root Structure
```
codehub-backend/
├── src/
│   ├── app.module.ts             # Root module
│   ├── main.ts                   # Application entry
│   ├── config/                   # Configuration
│   │   ├── database.config.ts
│   │   ├── jwt.config.ts
│   │   └── app.config.ts
│   ├── common/                   # Shared code
│   │   ├── decorators/
│   │   ├── guards/
│   │   │   ├── jwt.guard.ts
│   │   │   └── roles.guard.ts
│   │   ├── interceptors/
│   │   ├── filters/
│   │   │   └── exception.filter.ts
│   │   └── dto/                  # Shared DTOs
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.module.ts
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── jwt.strategy.ts
│   │   │   ├── dto/
│   │   │   │   ├── login.dto.ts
│   │   │   │   └── signup.dto.ts
│   │   │   └── entities/         # If needed
│   │   ├── users/
│   │   │   ├── users.module.ts
│   │   │   ├── users.controller.ts
│   │   │   ├── users.service.ts
│   │   │   ├── dto/
│   │   │   └── entities/
│   │   │       └── user.entity.ts
│   │   ├── problems/
│   │   │   ├── problems.module.ts
│   │   │   ├── problems.controller.ts
│   │   │   ├── problems.service.ts
│   │   │   ├── dto/
│   │   │   └── entities/
│   │   │       └── problem.entity.ts
│   │   ├── submissions/
│   │   │   ├── submissions.module.ts
│   │   │   ├── submissions.controller.ts
│   │   │   ├── submissions.service.ts
│   │   │   ├── dto/
│   │   │   └── entities/
│   │   │       └── submission.entity.ts
│   │   ├── courses/
│   │   │   ├── courses.module.ts
│   │   │   ├── courses.controller.ts
│   │   │   ├── courses.service.ts
│   │   │   ├── dto/
│   │   │   └── entities/
│   │   │       └── course.entity.ts
│   │   ├── progress/
│   │   │   ├── progress.module.ts
│   │   │   ├── progress.controller.ts
│   │   │   ├── progress.service.ts
│   │   │   ├── dto/
│   │   │   └── entities/
│   │   │       └── progress.entity.ts
│   │   └── leaderboard/
│   │       ├── leaderboard.module.ts
│   │       ├── leaderboard.controller.ts
│   │       ├── leaderboard.service.ts
│   │       ├── dto/
│   │       └── entities/
│   ├── shared/                   # Shared services
│   │   ├── database/
│   │   ├── cache/
│   │   └── logger/
│   └── utils/                    # Utilities
│       ├── helpers.ts
│       └── validators.ts
├── test/                         # Tests
│   ├── e2e/
│   └── unit/
├── docker/                       # Docker files
│   ├── Dockerfile
│   └── docker-compose.yml
├── migrations/                   # DB migrations
├── package.json
├── tsconfig.json
├── nest-cli.json
└── README.md
```

## Key Decisions
- **Modular Structure**: Each feature in its own module for isolation
- **Entities**: TypeORM entities in entities/ folders
- **DTOs**: Separate for input/output validation
- **Guards/Interceptors**: Centralized in common/ for reuse
- **Config**: Environment-based configs
- **Tests**: Unit and e2e tests

## Scalability
- Easy to split modules into microservices later
- Shared code in common/ and shared/
- Migrations for DB versioning

This structure ensures maintainable, testable, and scalable backend code.