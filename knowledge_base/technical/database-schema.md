---
name: DATABASE_SCHEMA
description: PostgreSQL database structure for the Bonsai marketplace
domain: technical
node_type: decision
status: implemented
last_updated: 2026-01-13
tags:
  - technical
  - database
topics:
  - database
  - api
  - auth
  - storage
related_concepts:
  - "[[tech-stack]]"
  - "[[trust-system]]"
  - "[[user-types]]"
  - "[[marketplace-features]]"
source:
  type: codebase
  file: "bonsai-bloom/supabase/migrations/"
  date: "2026-01-13"
---

# Database Schema

The Bonsai marketplace uses PostgreSQL via Supabase with 13 migrations tracking the schema evolution.

## Core Tables

### profiles
User accounts with seller information.
- `seller_type` - enum: individual, nursery, shop
- `trust_score` - Calculated reputation score
- `is_verified` - Verification badge status
- `city`, `province` - South African locations
- `whatsapp_number`, `phone_number` - Contact methods

### listings
Bonsai product listings.
- `species`, `age`, `height`, `pot_style` - Bonsai details
- `price`, `is_negotiable` - Pricing information
- `status` - enum: active, sold, reserved, archived
- `view_count` - Popularity tracking
- `seller_id` - Foreign key to profiles

### listing_images
Multiple images per listing.
- `image_url` - Storage URL
- `display_order` - Carousel ordering
- `is_primary` - Main display image

### ratings
Bidirectional review system.
- `rater_id`, `rated_id` - Both parties rated
- `listing_id` - Transaction context
- `rating` - 1-5 stars
- `review` - Text feedback
- `rating_type` - buyer_to_seller or seller_to_buyer

### messages
Buyer-seller communication.
- `sender_id`, `receiver_id` - Participants
- `listing_id` - Context for conversation
- `is_read` - Read status tracking

### forum_categories
Forum organization.
- Categories: Care Tips, Species Advice, Styling, Troubleshooting, Marketplace
- `display_order` - Menu ordering
- `description` - Category purpose

### forum_posts
Forum discussions.
- `upvote_count`, `view_count` - Engagement metrics
- `is_pinned` - Important posts
- `is_answered` - Q&A resolution

### forum_comments
Nested comment system.
- `parent_comment_id` - Self-referential for nesting
- `is_accepted` - Best answer designation
- `upvote_count` - Community voting

### favorites
User wishlists.
- Unique constraint: one favorite per user-listing pair

### user_roles
Role-based access control.
- Roles: admin, moderator, user
- Enables permission checks throughout the app

## Security

[DECIDED: Row-level security (RLS) enabled on all tables]

Each table has appropriate policies:
- Users can only modify their own data
- Public read access where appropriate
- Admin/moderator elevated permissions

## South Africa Specifics

- Province enum matches SA provinces
- City validation for major SA cities
- Phone number format validation for SA numbers

---

**Status:** Implemented
**Migrations:** 13 SQL files in `supabase/migrations/`
