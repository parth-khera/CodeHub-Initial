# Frontend Folder Structure for CodeHub

## Overview
Next.js App Router with TypeScript. Modular structure for scalability. Separation of concerns with reusable components.

## Root Structure
```
codehub-frontend/
├── app/                          # Next.js App Router pages
│   ├── (auth)/                   # Auth layout group
│   │   ├── login/
│   │   │   └── page.tsx
│   │   ├── signup/
│   │   │   └── page.tsx
│   ├── dashboard/
│   │   └── page.tsx
│   ├── courses/
│   │   ├── [id]/
│   │   │   └── page.tsx
│   │   └── page.tsx
│   ├── problems/
│   │   ├── [id]/
│   │   │   └── page.tsx
│   │   └── page.tsx
│   ├── profile/
│   │   └── page.tsx
│   ├── leaderboard/
│   │   └── page.tsx
│   ├── layout.tsx                # Root layout
│   ├── loading.tsx
│   ├── not-found.tsx
│   └── globals.css
├── components/                   # Reusable UI components
│   ├── ui/                       # Base UI (buttons, inputs)
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── ...
│   ├── layout/                   # Layout components
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   └── Footer.tsx
│   ├── forms/                    # Form components
│   │   ├── LoginForm.tsx
│   │   └── ProblemSubmitForm.tsx
│   ├── editor/                   # Monaco editor wrapper
│   │   └── CodeEditor.tsx
│   └── ...
├── lib/                          # Utilities and configs
│   ├── api/                      # API client functions
│   │   ├── auth.ts
│   │   ├── problems.ts
│   │   └── ...
│   ├── hooks/                    # Custom React hooks
│   │   ├── useAuth.ts
│   │   ├── useProblems.ts
│   │   └── ...
│   ├── utils/                    # Helper functions
│   │   ├── formatters.ts
│   │   └── validators.ts
│   ├── constants/                # App constants
│   │   └── config.ts
│   └── types/                    # TypeScript types
│       ├── user.ts
│       ├── problem.ts
│       └── ...
├── styles/                       # Styling
│   ├── globals.css
│   └── tailwind.config.js
├── public/                       # Static assets
│   ├── images/
│   └── icons/
├── middleware.ts                 # Next.js middleware for auth
├── next.config.js
├── package.json
├── tsconfig.json
└── README.md
```

## Key Decisions
- **App Router**: For better performance and SEO
- **Component Organization**: Grouped by feature (ui, forms, editor)
- **API Layer**: Centralized in lib/api/ for easy maintenance
- **Types**: Colocated with related logic in lib/types/
- **Styling**: Tailwind for utility-first CSS, consistent with design
- **Middleware**: For route protection and redirects

## Scalability
- Feature-based folders for large components
- Shared components in components/ui/
- Environment-specific configs in lib/constants/

This structure supports rapid development and easy refactoring as the app grows.