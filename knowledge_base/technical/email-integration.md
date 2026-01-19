---
name: EMAIL_INTEGRATION
description: Loops.so email service integration for transactional and automated emails
domain: technical
node_type: decision
status: implemented
last_updated: 2026-01-19
tags:
  - technical
  - marketplace
topics:
  - api
  - deployment
related_concepts:
  - "[[tech-stack]]"
  - "[[deployment-architecture]]"
  - "[[api-contracts]]"
  - "[[security-decisions]]"
  - "[[marketplace-features]]"
  - "[[user-journey-maps]]"
source:
  type: codebase
  file: "bonsai-bloom/src/lib/email.ts"
  date: "2026-01-19"
---

# Email Integration

The Bonsai marketplace uses [Loops.so](https://loops.so) for transactional and automated email communications.

## Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────┐
│  React Frontend │────▶│ Netlify Functions │────▶│  Loops API  │
│  (src/lib/email)│     │ (server-side)     │     │             │
└─────────────────┘     └──────────────────┘     └─────────────┘
                              │
                              │ LOOPS_API_KEY
                              │ (env variable)
                              ▼
```

[DECIDED: Server-side proxy via Netlify Functions to protect API key]

---

## Netlify Functions

Three serverless functions handle email operations:

| Function | Endpoint | Purpose |
|----------|----------|---------|
| `send-transactional` | `/api/send-transactional` | Send one-off transactional emails |
| `sync-contact` | `/api/sync-contact` | Create/update contacts in Loops |
| `send-event` | `/api/send-event` | Trigger automated email sequences |

### send-transactional

Sends emails using pre-built templates from the Loops dashboard.

```typescript
POST /api/send-transactional
{
  "transactionalId": "welcome",
  "email": "user@example.com",
  "dataVariables": {
    "userName": "John",
    "loginUrl": "https://nebari.co.za/login"
  },
  "addToAudience": true
}
```

### sync-contact

Syncs user data to Loops for segmentation and personalization.

```typescript
POST /api/sync-contact
{
  "email": "user@example.com",
  "userId": "uuid-123",
  "firstName": "John",
  "isSeller": true,
  "sellerType": "individual",
  "locationProvince": "Gauteng",
  "trustScore": 85
}
```

### send-event

Triggers Loops automations (multi-email sequences).

```typescript
POST /api/send-event
{
  "email": "user@example.com",
  "eventName": "userSignedUp",
  "eventProperties": {
    "signupSource": "homepage"
  }
}
```

---

## Allowed Events

Events are restricted to a whitelist for security:

### User Lifecycle
- `userSignedUp` - New account created
- `userVerifiedEmail` - Email address confirmed
- `profileCompleted` - Profile setup finished

### Seller Events
- `sellerOnboarded` - First listing created
- `sellerVerificationRequested` - Verification submitted
- `sellerVerified` - Verification approved
- `listingCreated` - New listing published
- `listingViewed` - Listing received views (batched)
- `listingSold` - Transaction completed

### Buyer Events
- `inquirySent` - Message sent to seller
- `purchaseCompleted` - Purchase finalized
- `reviewLeft` - Review submitted

### Forum Events
- `forumPostCreated` - New forum post
- `forumAnswerAccepted` - Answer marked as accepted

### Engagement Events
- `favoriteAdded` - Item added to wishlist
- `messageReceived` - New message notification
- `weeklyDigestEligible` - Weekly digest trigger

---

## Transactional Email Templates

Create these templates in the [Loops dashboard](https://app.loops.so/transactional):

| ID | Purpose | Data Variables |
|----|---------|----------------|
| `welcome` | Welcome email | userName, loginUrl |
| `password-reset` | Password reset | resetUrl |
| `verify-email` | Email verification | verifyUrl |
| `new-message` | Message notification | senderName, messagePreview, conversationUrl |
| `listing-inquiry` | Inquiry received | buyerName, listingTitle, messageUrl |
| `listing-sold` | Sale notification | listingTitle, salePrice, buyerName |
| `purchase-confirmation` | Purchase receipt | listingTitle, salePrice, sellerName |
| `seller-verified` | Verification approved | - |
| `seller-rejected` | Verification rejected | reason |
| `new-review` | Review received | reviewerName, rating, reviewText |
| `forum-reply` | Forum reply | postTitle, replyPreview, postUrl |
| `answer-accepted` | Answer accepted | questionTitle, postUrl |

---

## Frontend Client

The `src/lib/email.ts` module provides a typed client for the email service:

```typescript
import {
  sendTransactionalEmail,
  syncContact,
  sendEvent,
  sendWelcomeEmail,
  syncUserToLoops,
  TRANSACTIONAL_IDS,
} from "@/lib/email";

// Send welcome email
await sendWelcomeEmail("user@example.com", "John");

// Sync user profile
await syncUserToLoops("user@example.com", "user-uuid", {
  displayName: "John Doe",
  isSeller: true,
  sellerType: "individual",
  locationProvince: "Gauteng",
});

// Trigger automation
await sendEvent({
  email: "user@example.com",
  eventName: "listingCreated",
  eventProperties: {
    listingTitle: "Japanese Maple",
    species: "Acer palmatum",
  },
});
```

---

## Configuration

### Environment Variables

```bash
# Server-side only (Netlify dashboard)
LOOPS_API_KEY=your_api_key_here
```

[DECIDED: API key stored in Netlify environment, never exposed to client]

### Netlify Configuration

The `netlify.toml` routes `/api/*` to Netlify Functions:

```toml
[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/:splat"
  status = 200
```

---

## Local Development

To test email functions locally:

1. Install Netlify CLI: `npm i -g netlify-cli`
2. Create `.env` with `LOOPS_API_KEY`
3. Run `netlify dev` instead of `npm run dev`

This runs the Netlify Functions locally alongside the Vite dev server.

---

## Integration Points

### User Signup (AuthContext)

```typescript
// After successful signup
await syncUserToLoops(email, userId, { displayName });
await sendEvent({ email, eventName: "userSignedUp" });
```

### Profile Update (ProfileEdit)

```typescript
// After profile save
await syncUserToLoops(email, userId, profileData);
await sendEvent({ email, eventName: "profileCompleted" });
```

### Listing Sold (SellerDashboard)

```typescript
// After marking as sold
await sendEvent({
  email: sellerEmail,
  eventName: "listingSold",
  eventProperties: { listingTitle, salePrice, buyerName },
});
```

### New Message (Messages)

```typescript
// After sending message
await sendEvent({
  email: recipientEmail,
  eventName: "messageReceived",
  eventProperties: { senderName, listingTitle },
});
```

---

## Security Considerations

1. **API Key Protection** - Key only exists server-side in Netlify Functions
2. **Event Whitelist** - Only allowed events can be triggered
3. **Input Validation** - All parameters validated before sending to Loops
4. **Rate Limiting** - Loops has built-in rate limits; consider adding app-level limits

---

## Open Questions

- [OPEN] Email delivery monitoring dashboard
- [OPEN] Bounce handling and unsubscribe flow
- [OPEN] A/B testing for email templates

---

**Status:** Implemented
**SDK:** loops v1.x
**Dashboard:** https://app.loops.so
