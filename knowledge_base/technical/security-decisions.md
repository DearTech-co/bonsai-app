---
name: SECURITY_DECISIONS
description: Security architecture, RLS policies, and access control decisions
domain: technical
node_type: decision
status: implemented
last_updated: 2026-01-19
tags:
  - technical
  - marketplace
topics:
  - auth
  - permissions
  - safety
related_concepts:
  - "[[database-schema]]"
  - "[[tech-stack]]"
  - "[[api-contracts]]"
  - "[[user-types]]"
  - "[[trust-system]]"
source:
  type: codebase
  file: "bonsai-bloom/supabase/migrations/20260113125442_*.sql"
  date: "2026-01-19"
---

# Security Decisions

Comprehensive security architecture for the Bonsai marketplace, documenting Row-Level Security (RLS) policies, access control patterns, and data protection strategies.

## Core Security Principles

1. **Defense in Depth** - Multiple layers of security (RLS, application checks, validation triggers)
2. **Least Privilege** - Users only access data they own or need
3. **Audit Trail** - All admin actions are logged
4. **Data Minimization** - Public views hide sensitive fields

[DECIDED: RLS as primary security layer with application-level validation as backup]

---

## Row-Level Security (RLS)

All tables have RLS enabled. Policies follow consistent patterns.

### User Data Policies

```sql
-- Standard owner-only pattern for personal data
CREATE POLICY "Users can view their own data"
  ON table FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can modify their own data"
  ON table FOR UPDATE
  USING (auth.uid() = user_id);
```

### Public Read Policies

```sql
-- Public content (listings, forum posts)
CREATE POLICY "Anyone can view active listings"
  ON listings FOR SELECT
  USING (status = 'active');
```

### Admin Override Policies

```sql
-- Admin access using security definer function
CREATE POLICY "Admins can manage all data"
  ON table FOR ALL
  USING (public.has_role(auth.uid(), 'admin'));
```

---

## Role-Based Access Control (RBAC)

### Role Hierarchy

| Role | Permissions |
|------|-------------|
| `user` | Standard user access, own data only |
| `moderator` | User + forum moderation, content flagging |
| `admin` | Full access, verification, user management |

### Implementation

Roles stored separately from profiles (security best practice):

```sql
CREATE TABLE user_roles (
  user_id UUID REFERENCES auth.users(id),
  role app_role NOT NULL,
  UNIQUE (user_id, role)
);
```

Role check function prevents RLS recursion:

```sql
CREATE FUNCTION has_role(_user_id UUID, _role app_role)
RETURNS BOOLEAN
SECURITY DEFINER  -- Bypasses RLS for role check
AS $$
  SELECT EXISTS (
    SELECT 1 FROM user_roles
    WHERE user_id = _user_id AND role = _role
  )
$$;
```

[DECIDED: Separate user_roles table prevents circular RLS dependencies]

---

## Data Protection

### Sensitive Fields

The `profiles` table contains sensitive data hidden from public queries:

| Field | Sensitivity | Protection |
|-------|-------------|------------|
| `phone` | High | profiles_public view |
| `whatsapp` | High | profiles_public view |
| `total_sales_value` | Medium | profiles_public view |
| `total_sales_count` | Medium | profiles_public view |
| `verification_requested_at` | Low | profiles_public view |
| `verified_at` | Low | profiles_public view |

### Public View Pattern

```sql
CREATE VIEW profiles_public AS
SELECT
  id, username, display_name, avatar_url, bio,
  location_city, location_province,
  is_seller, is_verified_seller, seller_type,
  trust_score, response_time_hours
FROM profiles;
-- Excludes: phone, whatsapp, sales data, timestamps
```

[DECIDED: View-based field filtering rather than column-level RLS (not supported)]

---

## Input Validation

### Database-Level Validation

Trigger-based validation catches issues before data enters:

```sql
CREATE FUNCTION validate_profile_data()
RETURNS TRIGGER AS $$
BEGIN
  -- Username: 3-30 chars, alphanumeric + underscore
  IF NEW.username !~ '^[a-zA-Z0-9_]+$' THEN
    RAISE EXCEPTION 'Invalid username format';
  END IF;

  -- WhatsApp: South African phone format
  IF NEW.whatsapp !~ '^(\+?27|0)?[0-9]{9,10}$' THEN
    RAISE EXCEPTION 'Invalid WhatsApp number format';
  END IF;

  RETURN NEW;
END;
$$;
```

### Validated Fields

| Field | Validation Rule |
|-------|----------------|
| `username` | 3-30 chars, `[a-zA-Z0-9_]` only |
| `display_name` | Max 100 chars |
| `bio` | Max 500 chars |
| `whatsapp` | SA phone format: `+27xxxxxxxxx` or `0xxxxxxxxx` |
| `phone` | Max 20 chars |

---

## Admin Actions & Audit Trail

### Verification Workflow

All verification actions logged with admin ID and timestamp:

```sql
CREATE TABLE seller_verification_log (
  profile_id UUID NOT NULL,
  admin_id UUID NOT NULL,
  action TEXT CHECK (action IN ('approved', 'rejected', 'revoked')),
  reason TEXT,
  created_at TIMESTAMPTZ DEFAULT now()
);
```

### Secure Admin Functions

Admin operations use `SECURITY DEFINER` to bypass RLS while enforcing role checks:

```sql
CREATE FUNCTION approve_seller_verification(p_profile_id UUID)
RETURNS VOID AS $$
BEGIN
  -- Role check first
  IF NOT has_role(auth.uid(), 'admin') THEN
    RAISE EXCEPTION 'Unauthorized: Admin access required';
  END IF;

  -- Update profile
  UPDATE profiles SET is_verified_seller = true WHERE id = p_profile_id;

  -- Log action
  INSERT INTO seller_verification_log (profile_id, admin_id, action)
  VALUES (p_profile_id, get_admin_profile_id(), 'approved');
END;
$$ SECURITY DEFINER;
```

---

## Security Fixes Applied

### Fix 1: Favorites RLS (Critical)

**Issue:** Policies incorrectly referenced `profiles.user_id = profiles.user_id`
**Fix:** Corrected to reference `favorites.user_id`

### Fix 2: Profile Validation (Medium)

**Issue:** No server-side validation for profile fields
**Fix:** Added trigger-based validation with SA-specific formats

### Fix 3: Trust Score Updates (Medium)

**Issue:** Trust score not recalculated on rating changes
**Fix:** Added trigger to auto-update on INSERT/UPDATE/DELETE of ratings

### Fix 4: Public Profile Access (Low)

**Issue:** Conflicting RLS policies exposing all profile data
**Fix:** Created profiles_public view for controlled field exposure

---

## Authentication Security

### Session Management

- JWT tokens with 1-hour expiry (Supabase default)
- Automatic refresh via Supabase client
- Secure httpOnly cookies for session storage

### Password Requirements

Handled by Supabase Auth:
- Minimum 6 characters (configurable)
- No complexity requirements by default
- Rate limiting on auth endpoints

---

## Open Questions

- [OPEN] Implement rate limiting on API endpoints
- [OPEN] Add IP-based blocking for abuse prevention
- [OPEN] Consider 2FA for high-value sellers

---

**Status:** Implemented
**Security Migration:** `20260113125442_*.sql`
