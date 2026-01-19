---
name: DEPLOYMENT_ARCHITECTURE
description: Infrastructure, hosting, and deployment configuration
domain: technical
node_type: decision
status: implemented
last_updated: 2026-01-19
tags:
  - technical
  - marketplace
topics:
  - deployment
  - frontend
  - database
related_concepts:
  - "[[tech-stack]]"
  - "[[api-contracts]]"
  - "[[security-decisions]]"
  - "[[database-schema]]"
source:
  type: codebase
  file: "bonsai-bloom/vite.config.ts"
  date: "2026-01-19"
---

# Deployment Architecture

Infrastructure and deployment configuration for the Bonsai marketplace.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                        Users                                 │
└─────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                    CDN / Edge Network                        │
│                    (Lovable/Netlify)                        │
└─────────────────────────────────────────────────────────────┘
                             │
              ┌──────────────┴──────────────┐
              ▼                              ▼
┌──────────────────────┐      ┌──────────────────────────────┐
│   Static Frontend    │      │      Serverless Functions    │
│   (Vite + React)     │      │      (Netlify Functions)     │
│                      │      │                              │
│   - HTML/CSS/JS      │      │   - API webhooks             │
│   - Assets           │      │   - Background jobs          │
│   - Code-split       │      │                              │
│     chunks           │      │                              │
└──────────────────────┘      └──────────────────────────────┘
              │                              │
              └──────────────┬───────────────┘
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                     Supabase Cloud                          │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────────┐   │
│  │ PostgreSQL  │  │    Auth      │  │     Storage      │   │
│  │  Database   │  │   Service    │  │    (S3-like)     │   │
│  │    + RLS    │  │              │  │                  │   │
│  └─────────────┘  └──────────────┘  └──────────────────┘   │
│  ┌─────────────┐  ┌──────────────┐                         │
│  │  Realtime   │  │   Edge       │                         │
│  │  Websockets │  │  Functions   │                         │
│  └─────────────┘  └──────────────┘                         │
└─────────────────────────────────────────────────────────────┘
```

## Frontend Deployment

### Build Configuration

**Vite 5.4.19** with optimized production settings:

| Setting | Value | Purpose |
|---------|-------|---------|
| Target | ES2020 | Modern browser support |
| Minification | esbuild | Fast, efficient minification |
| Source maps | Production: disabled | Security + performance |
| CSS splitting | Enabled | Parallel loading |
| Chunk limit | 500KB | Performance warning threshold |

### Code Splitting Strategy

Manual chunks for optimal caching:

| Chunk | Contents | Size Estimate |
|-------|----------|---------------|
| `react-vendor` | React, ReactDOM, React Router | ~150KB |
| `radix-vendor` | 15 Radix UI components | ~100KB |
| `supabase` | Supabase client | ~80KB |
| `tanstack` | React Query | ~40KB |
| `forms` | React Hook Form, Zod | ~50KB |
| `charts` | Recharts | ~200KB (lazy loaded) |
| `utils` | clsx, tailwind-merge, cva | ~10KB |

[DECIDED: Manual chunks over automatic splitting for predictable caching]

### Asset Fingerprinting

```
assets/[name]-[hash].js
assets/[name]-[hash].css
assets/[name]-[hash].[ext]
```

Hash-based filenames enable aggressive CDN caching with instant cache invalidation on changes.

---

## Hosting Platform

### Lovable Platform

The frontend is built and deployed via [Lovable](https://lovable.dev):

- **Development:** `lovable-tagger` plugin for component tracking
- **Deployment:** Automatic builds on Git push
- **CDN:** Global edge distribution
- **SSL:** Automatic HTTPS certificates

### Netlify (Backup/Functions)

Netlify Functions available for serverless operations:

```typescript
// netlify/functions/example.ts
import type { Handler } from "@netlify/functions";

export const handler: Handler = async (event, context) => {
  return {
    statusCode: 200,
    body: JSON.stringify({ message: "Hello" }),
  };
};
```

---

## Backend Infrastructure

### Supabase Project

| Service | Usage |
|---------|-------|
| PostgreSQL | Primary database with RLS |
| Auth | Email/password, magic links |
| Storage | User uploads (avatars, listings, forum) |
| Realtime | Message notifications (future) |

### Environment Variables

```env
# Required for frontend
VITE_SUPABASE_URL=https://[project].supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=eyJhbGciOi...

# Server-side only (Netlify Functions)
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOi... # Never expose to frontend
```

[DECIDED: Anon/publishable key for client, service role for server functions only]

---

## Performance Optimizations

### Dependency Pre-bundling

```typescript
optimizeDeps: {
  include: ["react", "react-dom", "react-router-dom", "@tanstack/react-query"],
  exclude: ["recharts"],  // Heavy libs loaded on demand
}
```

### Lazy Loading Patterns

1. **Route-based:** Pages loaded on navigation
2. **Component-based:** Heavy components (charts) loaded when needed
3. **Image-based:** `loading="lazy"` on below-fold images

### Caching Strategy

| Layer | Cache Duration | Invalidation |
|-------|----------------|--------------|
| CDN | 1 year (hashed assets) | New hash on deploy |
| Browser | 1 week (HTML) | Deploy triggers revalidation |
| React Query | 5 min (stale time) | Mutation invalidation |

---

## Development Workflow

### Local Development

```bash
bun dev        # Start dev server on :8080
bun build      # Production build
bun preview    # Preview production build
```

### Dev Server Configuration

```typescript
server: {
  host: "::",    // IPv4 + IPv6
  port: 8080,    // Standard dev port
}
```

### Build Scripts

| Script | Purpose |
|--------|---------|
| `dev` | Development server with HMR |
| `build` | Production build (no sourcemaps) |
| `build:dev` | Development build (with sourcemaps) |
| `preview` | Serve production build locally |
| `lint` | ESLint code quality check |

---

## Monitoring & Observability

### Current State

- Supabase dashboard for database metrics
- Lovable deployment logs
- Browser DevTools for performance

### Future Considerations

- [OPEN] Add error tracking (Sentry)
- [OPEN] Add analytics (Plausible/PostHog)
- [OPEN] Add performance monitoring (Web Vitals)

---

## Scaling Considerations

### Current Capacity

Supabase Free/Pro tier handles:
- Up to 500MB database
- 5GB file storage
- 50,000 monthly active users

### Growth Path

1. **Database scaling:** Supabase Pro tier, read replicas
2. **CDN scaling:** Automatic with Lovable/Netlify
3. **Function scaling:** Automatic serverless scaling

---

**Status:** Implemented
**Primary Hosting:** Lovable Platform
**Backend:** Supabase Cloud
