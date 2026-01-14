---
name: TRUST_SYSTEM
description: Rating and trust mechanisms for marketplace participants
domain: product
node_type: feature
status: implemented
last_updated: 2026-01-12
tags:
  - product
  - ratings
topics:
  - marketplace
  - safety
  - reputation
related_concepts:
  - "[[user-types]]"
  - "[[product-vision]]"
  - "[[marketplace-features]]"
---

# Trust System

Building trust between buyers and sellers is critical for marketplace success.

## Core Principles

1. **Bidirectional** - Both buyers AND sellers get rated
2. **Transparent** - Ratings visible to all users
3. **Earned** - Trust built through successful transactions
4. **Protected** - Abuse prevention mechanisms

## Rating Components

### Transaction Ratings
- 1-5 star rating after each transaction
- Optional written review
- Ratings tied to verified transactions only

### Trust Score Calculation
- Weighted average of ratings
- Recency weighting (newer ratings count more)
- Volume factor (more transactions = more reliable score)
- [OPEN] Exact algorithm TBD

### Seller-Specific Metrics
- Response time
- Shipping speed (if applicable)
- Description accuracy
- Communication quality

### Buyer-Specific Metrics
- Payment reliability
- Communication quality
- Fair dealing

## Trust Badges/Levels

Potential tier system:
- New User (< 5 transactions)
- Verified (5+ transactions, 4.0+ rating)
- Trusted (20+ transactions, 4.5+ rating)
- Top Rated (50+ transactions, 4.8+ rating)

## Abuse Prevention

- Cannot rate without completed transaction
- Rate limiting on reviews
- Flagging system for suspicious patterns
- Admin review for disputed ratings
- [OPEN] Fake review detection approach?

## Implementation Notes

Implemented in the `ratings` table with:
- `rating` - 1-5 star value
- `review` - Optional text feedback
- `rating_type` - buyer_to_seller or seller_to_buyer
- `listing_id` - Ties rating to specific transaction
- Trust score stored on `profiles.trust_score`

Seller verification via `profiles.is_verified` boolean.

## Open Questions

- [OPEN] Trust score calculation algorithm
- [OPEN] Appeal process for unfair ratings?

---

**Status:** Implemented
**Location:** `bonsai-bloom/supabase/migrations/`
