---
name: LEGAL_REQUIREMENTS
description: South African legal and regulatory compliance for the marketplace
domain: business
node_type: decision
status: emergent
last_updated: 2026-01-23
tags:
  - business
  - marketplace
topics:
  - legal
  - safety
related_concepts:
  - "[[market-analysis]]"
  - "[[pricing-strategy]]"
  - "[[user-types]]"
  - "[[security-decisions]]"
  - "[[trust-system]]"
  - "[[monetization-strategy]]"
source:
  type: documentation
  date: "2026-01-23"
---

# Legal Requirements

South African legal and regulatory considerations for operating the Bonsai marketplace.

## Regulatory Framework

### Consumer Protection Act (CPA)

[DECIDED: Comply with CPA requirements]

The CPA applies to marketplaces facilitating B2C transactions.

**Key Requirements:**
- Clear terms and conditions
- Right to return goods (cooling-off period for direct marketing)
- Transparent pricing (no hidden fees)
- Product descriptions must be accurate
- Dispute resolution process

**Our Approach:**
- Comprehensive Terms of Service
- Clear refund/return policy guidance
- Seller guidelines for accurate descriptions

### Electronic Communications and Transactions Act (ECTA)

**Key Requirements:**
- Valid electronic contracts
- Data message requirements
- Consumer rights for electronic transactions

**Our Approach:**
- Click-wrap agreements for ToS
- Email confirmations for transactions
- Clear cancellation procedures

### Protection of Personal Information Act (POPIA)

[DECIDED: Full POPIA compliance required]

**Key Requirements:**
- Lawful processing of personal information
- Purpose limitation
- Data minimization
- Security safeguards
- Data subject rights (access, correction, deletion)

**Our Approach:**
- Privacy Policy clearly stating data usage
- Minimal data collection
- Secure storage (Supabase with RLS)
- User data export/deletion capabilities
- Cookie consent where required

### Companies Act

**Business Registration:**
- [OPEN] Determine company structure (Pty Ltd vs sole proprietor)
- [OPEN] CIPC registration requirements
- Tax registration (SARS)

## Plant-Specific Regulations

### CITES (Convention on International Trade in Endangered Species)

**Relevance:** Some rare plant species may be protected.

**Requirements:**
- Cannot facilitate trade in CITES-listed species without permits
- May need to flag/warn about protected species

**Our Approach:**
- [PROPOSED] Add species database with protection status
- Warning system for potentially protected species
- Seller responsibility acknowledgment

### Agricultural Pests Act

**Requirements:**
- Cannot facilitate trade in declared weeds or invasive species
- Phytosanitary considerations for cross-province shipping

**Our Approach:**
- [PROPOSED] Prohibited species list
- Province-specific shipping warnings

## Platform Liability

### Intermediary Status

[DECIDED: Operate as intermediary, not seller]

As a marketplace platform, we facilitate transactions but do not sell directly.

**Implications:**
- Reduced liability for seller misrepresentation
- Still responsible for platform safety
- Must have takedown procedures for illegal content

### Content Moderation

**Requirements:**
- Remove illegal content when notified
- Don't actively facilitate illegal transactions
- Maintain records for legal requests

**Our Approach:**
- Report/flag system implemented
- Moderation queue for flagged content
- Admin tools for content removal

## Financial Regulations

### Payment Processing

[OPEN] If we handle payments directly:
- Financial Services Board registration?
- Payment Card Industry (PCI) compliance
- Anti-money laundering (AML) considerations

**Current Approach:**
- Phase 1: No payment processing (seller handles directly)
- Phase 2: Evaluate payment integration requirements

### Tax Considerations

**VAT:**
- If turnover exceeds R1M, VAT registration required
- Marketplace fees may be VATable supplies

**Seller Obligations:**
- Sellers responsible for their own tax obligations
- Consider providing transaction summaries for tax purposes

## Terms of Service Requirements

### Must Include:

1. **User obligations** - Accurate listings, legal goods only
2. **Platform limitations** - Intermediary status, no warranties
3. **Dispute resolution** - Process for buyer/seller conflicts
4. **Termination rights** - When we can remove users/listings
5. **Intellectual property** - User-generated content rights
6. **Limitation of liability** - Platform liability caps

### Status:
- [DECIDED] ToS drafted and in place
- [OPEN] Legal review recommended before scale

## Compliance Roadmap

| Phase | Action | Priority |
|-------|--------|----------|
| MVP | Basic ToS, Privacy Policy | ✅ Done |
| MVP | POPIA compliance | ✅ Done |
| Growth | Legal review of ToS | High |
| Growth | Species protection warnings | Medium |
| Scale | Payment compliance | Future |
| Scale | Company registration | Future |

## Open Questions

- [OPEN] Legal review of current ToS
- [OPEN] Insurance requirements for marketplace
- [OPEN] Cross-border transaction implications (if expanding beyond SA)
- [OPEN] Age verification requirements?

---

**Status:** Emergent
**Next:** Professional legal review of Terms of Service
