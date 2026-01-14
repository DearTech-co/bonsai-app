---
name: TECH_STACK
description: The technology choices and architecture for the Bonsai marketplace frontend
domain: technical
node_type: decision
status: implemented
last_updated: 2026-01-13
tags:
  - technical
  - frontend
  - architecture
topics:
  - frontend
  - database
  - auth
  - deployment
related_concepts:
  - "[[database-schema]]"
  - "[[marketplace-features]]"
  - "[[trust-system]]"
source:
  type: codebase
  file: "bonsai-bloom/"
  date: "2026-01-13"
---

# Tech Stack

The Bonsai marketplace is built with a modern React-based frontend stack, using Supabase as the backend-as-a-service.

## Frontend Framework

- **Vite 5.4.19** - Build tool and development server
- **React 18.3.1** - UI library
- **TypeScript 5.8.3** - Type safety throughout the codebase
- **React Router DOM 7.12.0** - Client-side routing

[DECIDED: React + Vite chosen for fast development, TypeScript for maintainability]

## UI & Styling

- **shadcn-ui** - Component library built on Radix UI (51 components)
- **Tailwind CSS 3.4.17** - Utility-first CSS framework
- **Radix UI** - 20+ headless accessible components
- **Lucide React** - Icon library
- **Embla Carousel** - Image carousel for listings

[DECIDED: shadcn-ui provides customizable, accessible components without vendor lock-in]

## Backend & Data

- **Supabase 2.90.1** - PostgreSQL database + Authentication
- **TanStack React Query 5.83.0** - Server state management and caching
- **React Hook Form 7.61.1** - Form state management
- **Zod 3.25.76** - Runtime schema validation

[DECIDED: Supabase chosen for rapid development with PostgreSQL, auth, and realtime built-in]

## Key Implementation Details

- Path alias: `@` → `./src` for clean imports
- Mobile-responsive with custom `use-mobile` hook
- Dark mode support via next-themes
- Generated TypeScript types from Supabase schema
- Row-level security (RLS) on all database tables

## Development Tooling

- ESLint with React plugins
- PostCSS with Autoprefixer
- Bun package manager (with npm fallback)

## Environment

```
VITE_SUPABASE_URL - Supabase project URL
VITE_SUPABASE_PUBLISHABLE_KEY - Supabase anon key
```

---

**Status:** Implemented
**Location:** `/bonsai-bloom/`
