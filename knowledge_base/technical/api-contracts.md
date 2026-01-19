---
name: API_CONTRACTS
description: Supabase API endpoints, functions, and data contracts
domain: technical
node_type: reference
status: implemented
last_updated: 2026-01-19
tags:
  - technical
  - marketplace
topics:
  - api
  - database
  - auth
related_concepts:
  - "[[database-schema]]"
  - "[[tech-stack]]"
  - "[[security-decisions]]"
  - "[[trust-system]]"
  - "[[user-types]]"
  - "[[marketplace-features]]"
source:
  type: codebase
  file: "bonsai-bloom/src/integrations/supabase/types.ts"
  date: "2026-01-19"
---

# API Contracts

The Bonsai marketplace uses Supabase as a Backend-as-a-Service, providing PostgreSQL database access, authentication, and serverless functions through auto-generated REST and realtime APIs.

## Database Tables (REST API)

All tables are accessed via Supabase client with auto-generated TypeScript types.

### Core Entities

| Table | Primary Operations | Key Fields |
|-------|-------------------|------------|
| `profiles` | CRUD | id, user_id, username, trust_score, is_verified_seller |
| `listings` | CRUD | id, seller_id, species, price, status |
| `listing_images` | CRUD | id, listing_id, image_url, display_order |
| `ratings` | Create, Read | reviewer_id, reviewed_id, rating, review_type |
| `messages` | CRUD | sender_id, receiver_id, listing_id, is_read |
| `favorites` | Create, Delete | user_id, listing_id |

### Forum Entities

| Table | Primary Operations | Key Fields |
|-------|-------------------|------------|
| `forum_categories` | Read | id, name, slug, display_order |
| `forum_posts` | CRUD | id, author_id, category_id, title, is_answered |
| `forum_comments` | CRUD | id, post_id, author_id, is_accepted_answer |
| `forum_post_images` | CRUD | id, post_id, image_url |
| `forum_comment_images` | CRUD | id, comment_id, image_url |

### System Entities

| Table | Primary Operations | Key Fields |
|-------|-------------------|------------|
| `user_roles` | Admin CRUD | user_id, role (admin/moderator/user) |
| `seller_verification_log` | Admin Read/Create | profile_id, admin_id, action |
| `notification_preferences` | CRUD | profile_id, email_*, push_* |
| `featured_listings` | Admin CRUD | listing_id, priority, expires_at |
| `featured_listing_requests` | CRUD | listing_id, status |

## Database Views

### profiles_public

A security view exposing only non-sensitive profile fields for public queries.

**Exposed fields:**
- id, user_id, username, display_name, avatar_url, bio
- location_city, location_province
- is_seller, is_verified_seller, seller_type
- trust_score, response_time_hours
- created_at, updated_at

**Hidden fields:**
- phone, whatsapp
- total_sales_value, total_sales_count
- verification_requested_at, verified_at

## Database Functions (RPC)

### Trust & Verification

```typescript
// Calculate user trust score (0-100)
calculate_trust_score(profile_uuid: string): number

// Check if user has a specific role
has_role(_user_id: string, _role: 'admin' | 'moderator' | 'user'): boolean

// Check if authenticated user owns a profile
is_profile_owner(profile_user_id: string): boolean
```

### Admin Functions

```typescript
// Seller verification workflow
approve_seller_verification(p_profile_id: string, p_reason?: string): void
reject_seller_verification(p_profile_id: string, p_reason?: string): void
revoke_seller_verification(p_profile_id: string, p_reason?: string): void

// Featured listing management
approve_featured_request(p_request_id: string, p_starts_at?: string, p_expires_at?: string, p_admin_notes?: string): void
reject_featured_request(p_request_id: string, p_admin_notes?: string): void
```

### Transaction Functions

```typescript
// Mark a listing as sold with buyer association
mark_listing_sold(p_listing_id: string, p_buyer_id: string): void

// Get user's purchase/inquiry history
get_purchase_history(p_user_profile_id: string): {
  history_type: string,
  inquiry_date: string,
  listing_id: string
}[]
```

## Enums

```typescript
// User permission levels
type app_role = 'admin' | 'moderator' | 'user'

// Featured listing request status
type featured_request_status = 'pending' | 'approved' | 'rejected'
```

## Authentication

Supabase Auth handles all authentication flows:

| Flow | Method |
|------|--------|
| Email/Password signup | `supabase.auth.signUp()` |
| Email/Password login | `supabase.auth.signInWithPassword()` |
| Magic link | `supabase.auth.signInWithOtp()` |
| Password reset | `supabase.auth.resetPasswordForEmail()` |
| Session refresh | Automatic via Supabase client |

## Storage Buckets

| Bucket | Purpose | Access |
|--------|---------|--------|
| `avatars` | Profile images | Public read, authenticated write |
| `listings` | Listing photos | Public read, authenticated write |
| `forum` | Forum post/comment images | Public read, authenticated write |
| `reviews` | Review images | Public read, authenticated write |

## Query Patterns

### Listing with Images

```typescript
const { data } = await supabase
  .from('listings')
  .select(`
    *,
    listing_images (*),
    profiles:seller_id (username, avatar_url, trust_score)
  `)
  .eq('status', 'active')
  .order('created_at', { ascending: false });
```

### Forum Post with Comments

```typescript
const { data } = await supabase
  .from('forum_posts')
  .select(`
    *,
    profiles_public:author_id (*),
    forum_comments (
      *,
      profiles_public:author_id (*)
    ),
    forum_post_images (*)
  `)
  .eq('id', postId)
  .single();
```

## Error Handling

Standard Supabase error structure:

```typescript
interface PostgrestError {
  message: string;
  details: string | null;
  hint: string | null;
  code: string;
}
```

Common error codes:
- `23505` - Unique constraint violation
- `23503` - Foreign key violation
- `42501` - Insufficient privileges (RLS)
- `PGRST116` - Row not found

---

**Status:** Implemented
**Types Location:** `bonsai-bloom/src/integrations/supabase/types.ts`
