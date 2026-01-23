---
name: MONETIZATION_STRATEGY
description: Detailed implementation plan for marketplace revenue generation
domain: business
node_type: decision
status: emergent
last_updated: 2026-01-23
tags:
  - business
  - marketplace
topics:
  - monetization
  - pricing
related_concepts:
  - "[[pricing-strategy]]"
  - "[[market-analysis]]"
  - "[[competitors]]"
  - "[[user-types]]"
  - "[[marketplace-features]]"
  - "[[legal-requirements]]"
  - "[[trust-system]]"
source:
  type: documentation
  date: "2026-01-23"
---

# Monetization Strategy

Detailed implementation plan for generating revenue from the Bonsai marketplace.

## Revenue Streams Overview

| Stream | Phase | Complexity | Revenue Potential |
|--------|-------|------------|-------------------|
| Featured Listings | 2 | Low | Medium |
| Business Accounts | 2-3 | Medium | High |
| Premium Badges | 3 | Low | Low-Medium |
| Transaction Fees | 4 | High | High |
| Advertising | 4+ | Medium | Medium |

## Phase 1: Free Growth (Current)

[DECIDED: MVP is completely free]

**Goal:** Build critical mass of users and listings

**Metrics to Track:**
- Monthly active users (MAU)
- Listings created per month
- Transaction volume (reported)
- User retention rate

**Exit Criteria for Phase 2:**
- 500+ active monthly users
- 200+ active listings
- Demonstrated transaction activity
- User survey showing willingness to pay

## Phase 2: Featured Listings

[PROPOSED: First revenue stream]

### How It Works

Sellers pay to boost listing visibility:
- Appear at top of search results
- Featured section on homepage
- Highlighted in category pages

### Pricing Model

| Option | Price (ZAR) | Duration | Visibility |
|--------|-------------|----------|------------|
| Basic Boost | R29 | 7 days | Top of category |
| Premium Boost | R79 | 14 days | Homepage + category |
| Super Boost | R149 | 30 days | All placements + badge |

### Implementation Requirements

- [x] Featured listings table (database ready)
- [x] Admin approval workflow
- [ ] Payment integration (Paystack/Yoco)
- [ ] Seller dashboard for purchasing
- [ ] Analytics for featured performance

### Revenue Projection

| MAU | Conversion | ARPU | Monthly Revenue |
|-----|------------|------|-----------------|
| 500 | 5% | R50 | R1,250 |
| 1,000 | 8% | R60 | R4,800 |
| 5,000 | 10% | R70 | R35,000 |

## Phase 3: Business Accounts

### Target Segment

- Nurseries
- Garden shops
- Professional bonsai artists
- High-volume individual sellers

### Features Included

| Feature | Free | Business |
|---------|------|----------|
| Listings | 10/month | Unlimited |
| Photos per listing | 5 | 15 |
| Analytics | Basic | Advanced |
| Verification badge | No | Yes |
| Priority support | No | Yes |
| Featured credits | 0 | 2/month |
| Custom profile | No | Yes |

### Pricing Model

| Tier | Monthly (ZAR) | Annual (ZAR) | Target |
|------|---------------|--------------|--------|
| Starter | R199 | R1,990 | Small nurseries |
| Professional | R499 | R4,990 | Medium businesses |
| Enterprise | R999 | R9,990 | Large operations |

### Implementation Requirements

- [ ] Subscription management system
- [ ] Stripe/recurring billing integration
- [ ] Tier-based feature flags
- [ ] Business verification workflow
- [ ] Enhanced analytics dashboard

## Phase 4: Transaction Fees

### Model Options

**Option A: Success Fee**
- 5-8% of transaction value
- Only charged on completed sales
- Requires payment integration

**Option B: Connection Fee**
- Flat fee per buyer contact revealed
- R5-15 per connection
- Easier to implement

**Option C: Hybrid**
- Free listings, fee on successful sale OR
- Monthly subscription with no fees

### Considerations

- Payment integration complexity (Paystack, Yoco, PayFast)
- Escrow requirements for trust
- Chargebacks and disputes
- Off-platform transaction avoidance

[OPEN] Decision pending based on Phase 2-3 learnings

## Alternative Revenue Streams

### Advertising

**When:** 5,000+ MAU
**Types:**
- Sponsored listings from nurseries
- Related product ads (pots, tools, soil)
- Event promotion (club shows, workshops)

**Approach:**
- Native advertising only (no banner ads)
- Clearly marked as sponsored
- Relevant to bonsai community

### Affiliate Partnerships

**Potential Partners:**
- Bonsai tool suppliers
- Soil/fertilizer companies
- Pottery/pot makers
- Insurance providers

**Model:** Commission on referred sales

### Premium Content

**Potential Offerings:**
- Expert care guides
- Video tutorials
- Species database access
- Valuation tools

[OPEN] Validate demand before building

## Pricing Psychology

### Key Principles

1. **Anchor High** - Show premium option first
2. **Decoy Effect** - Middle option looks best
3. **Loss Aversion** - "Don't miss out" messaging
4. **Social Proof** - Show other sellers using features
5. **ZAR Pricing** - Local currency, local context

### A/B Testing Plan

- Test R29 vs R39 for basic boost
- Test monthly vs annual pricing display
- Test free trial vs immediate purchase

## Payment Integration

### Recommended: Paystack

**Why:**
- SA-focused, ZAR native
- Low fees (2.9% + R1)
- Easy integration
- Supports subscriptions

**Alternatives:**
- Yoco (good for mobile)
- PayFast (established)
- Stripe (international)

### Implementation Priority

1. One-time payments (featured listings)
2. Recurring subscriptions (business accounts)
3. Escrow/marketplace payments (transaction fees)

## Success Metrics

| Metric | Phase 2 Target | Phase 3 Target |
|--------|----------------|----------------|
| MRR (Monthly Recurring Revenue) | R5,000 | R25,000 |
| Paying customers | 50 | 150 |
| Conversion rate | 5% | 10% |
| Churn rate | <10% | <8% |
| LTV:CAC ratio | 3:1 | 4:1 |

## Open Questions

- [OPEN] Payment provider selection
- [OPEN] Pricing A/B test design
- [OPEN] Free trial vs freemium for business accounts
- [OPEN] Refund policy for digital purchases

---

**Status:** Emergent
**Next:** User research on willingness to pay for features
