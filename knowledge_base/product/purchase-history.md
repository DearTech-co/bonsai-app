---
name: PURCHASE_HISTORY
description: Feature allowing buyers to track inquiries and completed purchases
domain: product
node_type: feature
status: implemented
last_updated: 2026-01-14
tags:
  - product
  - marketplace
topics:
  - marketplace
  - listings
related_concepts:
  - "[[marketplace-features]]"
  - "[[user-types]]"
  - "[[database-schema]]"
  - "[[trust-system]]"
source:
  type: codebase
  file: "bonsai-bloom/src/pages/PurchaseHistory.tsx"
  date: "2026-01-14"
---

# Purchase History

Enables buyers to view all bonsai they've inquired about or purchased from sellers.

## Feature Overview

### Tabs
- **All** - Combined view of purchases and inquiries
- **Purchased** - Listings the buyer has bought (marked as sold to them)
- **Inquired** - Active listings the buyer has messaged about

### Key Behaviors

1. **Inquired** only shows active listings (not sold to others)
2. **Purchased** shows `sold_at` date badge on items
3. Grid layout matches Favorites page pattern

## Database Changes

### New Columns on `listings`
- `buyer_id` - UUID reference to buyer's profile (set when sold)
- `sold_at` - Timestamp when sale was completed

### Constraints
- `listings_sold_integrity` - Ensures buyer_id/sold_at only set when status='sold'
- `ON DELETE RESTRICT` - Prevents orphaned sales if buyer profile deleted

### New Functions
- `mark_listing_sold(listing_id, buyer_id)` - Secure RPC to mark sale
- `get_purchase_history(profile_id)` - Returns purchase/inquiry history

## Security

### Trigger: `validate_sold_transition`
Enforces business rules at database level:
- Only seller can modify sold-related fields
- Buyer must have messaged seller about listing
- Only active listings can be sold
- Sold status is final (immutable)

### RLS Policy Update
Buyers can view their purchased listings even after sold.

## Seller Flow

1. Seller clicks "Mark as Sold" on active listing
2. Dialog shows interested buyers (from message threads)
3. Seller selects buyer and confirms
4. Listing status changes to 'sold', buyer_id and sold_at populated
5. Seller stats updated (total_sales_count, total_sales_value)

## Implementation

### Files Created
- `src/pages/PurchaseHistory.tsx` - Main page component
- `src/components/seller/MarkAsSoldDialog.tsx` - Buyer selection dialog
- `supabase/migrations/20260114100000_add_purchase_history.sql`

### Files Modified
- `src/components/seller/SellerListingsTable.tsx` - Added Mark as Sold action
- `src/components/layout/Header.tsx` - Added navigation link
- `src/App.tsx` - Added /purchases route
- `src/pages/Discover.tsx` - Added buyer_id, sold_at to Listing type

---

**Status:** Implemented
**Location:** `/bonsai-bloom/src/pages/PurchaseHistory.tsx`
