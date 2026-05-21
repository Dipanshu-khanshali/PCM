# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Auth**: JWT (jsonwebtoken + bcryptjs)
- **File upload**: Multer

## Structure

```text
artifacts-monorepo/
├── artifacts/              # Deployable applications
│   ├── college-mgmt/       # React + Vite frontend (dark red theme)
│   └── api-server/         # Express API server
├── lib/                    # Shared libraries
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts
│   └── src/seed.ts         # Database seeder
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## College Management System Features

### Authentication
- JWT-based login (jsonwebtoken + bcryptjs)
- Roles: admin, teacher, student
- Default admin: admin@college.edu / admin123
- Default teacher: teacher@college.edu / teacher123
- Default student: student@college.edu / student123

### API Routes (all under /api)
- `POST /auth/login` — Login, returns JWT token
- `GET /auth/me` — Get current user info
- `GET/POST /students` — List/create students
- `PUT/DELETE /students/:id` — Update/delete student
- `GET/POST /teachers` — List/create teachers
- `PUT/DELETE /teachers/:id` — Update/delete teacher
- `GET/POST /courses` — List/create courses
- `PUT/DELETE /courses/:id` — Update/delete course
- `GET/POST /attendance` — Get/mark attendance
- `GET/POST /results` — Get/add results
- `PUT /results/:id` — Update result
- `POST /upload` — File upload (multer, stored in /uploads)
- `GET /stats` — Dashboard statistics

### Database Schema (PostgreSQL via Drizzle)
- `users` — Auth users with role (admin|teacher|student)
- `students` — Student profiles
- `teachers` — Teacher profiles
- `courses` — Course catalog
- `attendance` — Daily attendance records
- `results` — Exam results with auto-calculated grades

### Frontend Pages (React + Vite, dark black + red theme)
- `/login` — Login page
- `/` — Admin dashboard with stats
- `/students` — Student management CRUD
- `/teachers` — Teacher management CRUD
- `/courses` — Course management CRUD
- `/attendance` — Attendance marking and viewing
- `/results` — Results/grades management

## Seeding
Run: `pnpm --filter @workspace/scripts run seed`

## TypeScript & Composite Projects

Every package extends `tsconfig.base.json` which sets `composite: true`. The root `tsconfig.json` lists all packages as project references.

## Root Scripts

- `pnpm run build` — runs `typecheck` first, then recursively runs `build` in all packages
- `pnpm run typecheck` — runs `tsc --build --emitDeclarationOnly` using project references

## Packages

### `artifacts/college-mgmt` (`@workspace/college-mgmt`)

React + Vite frontend with dark theme (black + red). Uses React Query for data fetching, wouter for routing, framer-motion for animations.

### `artifacts/api-server` (`@workspace/api-server`)

Express 5 API server with JWT auth, bcrypt password hashing, Multer file uploads.

### `lib/db` (`@workspace/db`)

Database layer using Drizzle ORM with PostgreSQL.

### `scripts` (`@workspace/scripts`)

Utility scripts — run seed with `pnpm --filter @workspace/scripts run seed`.
