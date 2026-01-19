---
name: MARKETPLACE_FEATURES
description: Complete feature set implemented in the Bonsai marketplace
domain: product
node_type: feature
status: implemented
last_updated: 2026-01-13
tags:
  - product
  - marketplace
topics:
  - marketplace
  - listings
  - search
  - forum
  - media
related_concepts:
  - "[[tech-stack]]"
  - "[[database-schema]]"
  - "[[user-types]]"
  - "[[trust-system]]"
  - "[[api-contracts]]"
  - "[[security-decisions]]"
  - "[[user-journey-maps]]"
  - "[[user-personas]]"
source:
  type: codebase
  file: "bonsai-bloom/src/"
  date: "2026-01-13"
---

# Marketplace Features

The Bonsai marketplace MVP includes a comprehensive set of features across marketplace, community, and user management domains.

## Marketplace Features

### Listing Discovery
- Advanced filtering: province, city, species, price range, seller type
- Listing cards with key bonsai details
- View count tracking for popularity
- Responsive grid layout

### Listing Details
- Image carousel for multiple photos
- Full bonsai specifications (species, age, height, pot style)
- Seller profile preview with trust score
- Contact seller button
- Favorite/wishlist toggle

### Seller Dashboard
- Listings management table
- Create/edit listing dialog
- Image uploader with drag-and-drop
- Sales statistics display

## Community Forum

### Categories
- Care Tips
- Species Advice
- Styling
- Troubleshooting
- Marketplace discussions

### Post Features
- Rich text posts with images
- Upvoting system
- View count tracking
- Pinned posts for important content
- Q&A with accepted answers

### Comments
- Nested replies
- Upvoting
- Best answer designation

## User Features

### Authentication
- Email-based signup/login via Supabase
- Profile setup onboarding flow
- Session management

### Profile Management
- Avatar upload
- Display name and bio
- Location settings (city/province)
- Seller type selection
- Contact preferences (WhatsApp, phone)

### Messaging
- In-app messaging system
- Conversation threads linked to listings
- Read status tracking

### Favorites
- Save listings to wishlist
- Quick access from profile

## Admin Features

### Admin Dashboard
- User management
- Content moderation
- Platform statistics

### Role-Based Access
- Admin, moderator, user roles
- Permission-gated features

## Pages Implemented

19 page components:
- Index (homepage)
- Discover (marketplace)
- ListingDetail
- SellerProfile
- SellerDashboard
- Forum, ForumCategory, ForumPost, ForumAsk
- Messages
- Profile, ProfileEdit, ProfileSetup
- Login, Signup
- Favorites
- Settings
- Admin
- NotFound (404)

---

**Status:** Implemented (MVP complete)
**Location:** `/bonsai-bloom/src/pages/`
