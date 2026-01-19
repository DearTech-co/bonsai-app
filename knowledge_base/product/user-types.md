---
name: USER_TYPES
description: The different user roles in the Bonsai marketplace
domain: product
node_type: concept
status: implemented
last_updated: 2026-01-12
tags:
  - product
  - marketplace
topics:
  - marketplace
  - auth
  - permissions
related_concepts:
  - "[[product-vision]]"
  - "[[trust-system]]"
  - "[[marketplace-features]]"
  - "[[security-decisions]]"
  - "[[api-contracts]]"
  - "[[user-personas]]"
  - "[[database-schema]]"
---

# User Types

The Bonsai marketplace supports multiple user types with different capabilities.

## Primary User Types

### 1. Individual Seller
- Personal collectors selling from their collection
- Standard verification level
- Full rating history visible
- Can also buy (dual role)

### 2. Business Seller
- Nurseries, garden shops, professional sellers
- Enhanced profile with business details
- Potentially higher verification requirements
- May have different fee structure
- Can also buy inventory

### 3. Buyer
- Users primarily interested in purchasing
- Can browse, search, and buy
- Rate sellers after transactions
- Receive ratings from sellers

### 4. Moderator
- Community forum oversight
- Content moderation capabilities
- Cannot see private transaction details
- Appointed by admin

### 5. Admin
- Full system access
- User management
- Dispute resolution
- Analytics and reporting

## Dual Roles

Users can operate as both buyer and seller simultaneously:
- Single account, multiple roles
- Separate rating scores for buying vs selling
- Combined trust score possible

## Implementation Notes

All user types are implemented in the database with the following:
- `profiles.seller_type` - enum for individual, nursery, shop
- `user_roles` table for admin, moderator, user roles
- Dual role support via single account with seller_type

## Open Questions

- [OPEN] Business verification process?
- [OPEN] Moderator recruitment/compensation?

---

**Status:** Implemented
**Location:** `bonsai-bloom/src/integrations/supabase/types.ts`
