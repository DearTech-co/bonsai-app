---
name: COMMUNITY_FORUM
description: Community forum for bonsai enthusiasts to share knowledge and get advice
domain: product
node_type: feature
status: implemented
last_updated: 2026-01-16
tags:
  - product
  - marketplace
topics:
  - forum
  - marketplace
related_concepts:
  - "[[product-vision]]"
  - "[[marketplace-features]]"
  - "[[user-types]]"
  - "[[trust-system]]"
source:
  type: codebase
  file: "bonsai-bloom/src/pages/Forum.tsx"
  date: "2026-01-16"
---

# Community Forum

The integrated community forum enables knowledge sharing and expert advice among bonsai enthusiasts.

## Core Purpose

- Build community beyond transactions
- Enable expert knowledge sharing
- Support new hobbyists with advice
- Increase platform engagement and retention

## Forum Categories

### Care Tips
General bonsai care advice, watering, fertilizing, seasonal care.

### Species Advice
Species-specific guidance, identification help, selection recommendations.

### Styling
Techniques for shaping, wiring, pruning, and artistic development.

### Troubleshooting
Problem diagnosis, pest/disease identification, recovery guidance.

### Marketplace
Buying/selling discussions, price checks, seller recommendations.

## Features

### Posts
- Rich text content with images
- Upvoting system for quality content
- View count tracking
- Pinned posts for important announcements
- Q&A format with accepted answers

### Comments
- Nested reply threads
- Upvoting on comments
- Best answer designation for Q&A posts

### Moderation
- Admin/moderator content oversight
- Flagging system for inappropriate content
- Category management

## Implementation

### Pages
- `Forum.tsx` - Main forum listing
- `ForumCategory.tsx` - Category-specific view
- `ForumPost.tsx` - Individual post view
- `ForumAsk.tsx` - Create new post

### Database Tables
- `forum_categories` - Category definitions
- `forum_posts` - Post content and metadata
- `forum_comments` - Nested comment system

## Integration with Marketplace

- Sellers can reference listings in posts
- Forum activity contributes to user reputation
- Expert contributors gain visibility
- Q&A builds trust before transactions

---

**Status:** Implemented
**Location:** `/bonsai-bloom/src/pages/Forum*.tsx`
