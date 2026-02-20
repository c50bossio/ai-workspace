# Brand Configurations

**Complete brand registry with account IDs, spend limits, and special rules**

## Brand Portfolio Overview

| Brand | Type | Tier | Max Daily Spend | Special Rules |
|-------|------|------|-----------------|---------------|
| Tomb45 | E-commerce | 1 | $3,000 | None |
| FFC | E-commerce | 1 | $2,000 | Inventory constraints |
| Headlines | Barbershop chain | 2 | $500 | **NO AUTO-SCALE** |
| Pie | Service business | 2 | $1,000 | None |
| SSC | E-commerce + Physical | 2 | TBD | Growing |
| UNKWN Pro | E-commerce | 2 | TBD | Boosting only (no full ads yet) |

**Tier 1:** Revenue drivers (70% of budget allocation)
**Tier 2:** Growth brands (25% of budget allocation)

---

## 1. Tomb45

### Business Profile
- **Type:** E-commerce (barber/salon products)
- **Revenue Channels:** Shopify, Amazon
- **Market:** B2B + B2C (professional barbers + consumers)
- **Priority:** Tier 1 (Revenue Driver)

### Ad Platforms

**Meta Ads:**
- **Ad Account ID:** `319971796`
- **Format:** `act_319971796` (with prefix)
- **Status:** ✅ Active
- **Average Monthly Spend:** ~$85K

**Google Ads:**
- **Customer ID:** `8137992951`
- **Formatted:** `813-799-2951`
- **Via MCC:** `735-272-6472`
- **Status:** ✅ Active

**Amazon Ads:**
- **Status:** 🔄 Not yet integrated (planned)
- **SP-API:** Pending setup

### E-commerce

**Shopify:**
- **Store:** `tomb-45.myshopify.com`
- **Access Token:** In `.env`
- **Status:** ✅ Connected

**Amazon:**
- **Seller Central:** Active
- **SP-API:** 🔄 Not connected yet
- **Sales Volume:** Significant (need API for tracking)

### Social Media

**Instagram:**
- **Handle:** `@officialtomb45`
- **API Status:** 🔄 Needs setup
- **Engagement:** High

**YouTube:**
- **Channel:** `@tomb45`
- **API Status:** 🔄 Needs setup
- **Content:** Product demos, tutorials

### Budget & Rules

**Max Daily Spend:** $3,000/day
- **Current capacity:** ~$2,800/day tested
- **Scaling room:** +$200/day available

**Special Rules:** None (standard automation applies)

**ROAS Target:** 3.0x minimum
- **Current average:** 2.5-2.8x (needs improvement)
- **Best performers:** 3.0-3.8x range

---

## 2. Florida's Finest Customs (FFC)

### Business Profile
- **Type:** E-commerce (custom automotive parts/accessories)
- **Revenue Channels:** Shopify
- **Market:** B2C (car enthusiasts)
- **Priority:** Tier 1 (Revenue Driver)

### Ad Platforms

**Meta Ads:**
- **Ad Account ID:** `2834312206725455`
- **Format:** `act_2834312206725455`
- **Status:** ✅ Active
- **Average Monthly Spend:** ~$60K

**Google Ads:**
- **Customer ID:** `1384331870`
- **Formatted:** `138-433-1870`
- **Via MCC:** `735-272-6472`
- **Status:** ✅ Active

### E-commerce

**Shopify:**
- **Store:** `floridasfinestcustom.myshopify.com`
- **Access Token:** In `.env`
- **Status:** ✅ Connected

### Social Media

**Instagram:**
- **Handle:** `@floridafinestcustomworks_`
- **API Status:** 🔄 Needs setup
- **Engagement:** Moderate

**YouTube:**
- **Channel:** `@floridasfinestcustoms`
- **API Status:** 🔄 Needs setup
- **Content:** Custom builds, before/after

### Budget & Rules

**Max Daily Spend:** $2,000/day
- **Constraint:** Physical inventory
- **Why:** Custom parts, can't scale beyond inventory capacity
- **Risk:** Over-selling = backorders

**Special Rules:**
- Don't scale unless inventory confirmed
- Pause campaigns for out-of-stock products
- Focus on in-stock, high-margin items

**ROAS Target:** 3.0x minimum
- **Current average:** Check latest data
- **Inventory consideration:** High-margin items may justify lower ROAS

---

## 3. Headlines Barbershops

### Business Profile
- **Type:** Barbershop chain (10 locations)
- **Revenue Channels:** In-person services, bookings
- **Market:** Local (Tampa Bay area)
- **Priority:** Tier 2 (Growth Brand)

### Locations (10)

| Location | Address |
|----------|---------|
| Avalon Park | 33913 FL-54, Wesley Chapel, FL |
| Bexley | 16936 Focus Lp, Land O' Lakes, FL |
| Bloomingdale | 909 E Bloomingdale Ave, Brandon, FL |
| Brandon | 10274 Causeway Blvd., Tampa, FL |
| Land O Lakes | 23104 Florida 54, Lutz, FL |
| New Tampa | 6431 E County Line Rd #103, Tampa, FL |
| North Dale/Carrollwood | 13032 N Dale Mabry Hwy, Tampa, FL |
| Sunlake | 19455 Shumard Oak Drive #103, Land O Lakes, FL |
| Tampa Palms | 17024 Palm Pointe Dr, Tampa, FL |
| Channelside | TBD (location pending) |

### Ad Platforms

**Meta Ads:**
- **Ad Account ID:** `510699435029440`
- **Format:** `act_510699435029440`
- **Status:** ✅ Active
- **Average Monthly Spend:** ~$15K

**Google Ads:**
- **Customer ID:** `3899502299`
- **Formatted:** `389-950-5299`
- **Via MCC:** `735-272-6472`
- **Status:** ✅ Active

### Google Business Profile

**Locations:** 10 verified
**API Status:** ✅ Connected
**Metrics:**
- Reviews, impressions, calls
- Direction requests, bookings
- Maps views, search views

### Social Media

**Instagram:**
- **Handle:** `@headlinesbarbershops`
- **API Status:** 🔄 Needs setup
- **Engagement:** Moderate

### Budget & Rules

**Max Daily Spend:** $500/day
- **Constraint:** Local market demand
- **Why:** 10 locations in Tampa Bay area
- **Risk:** Over-saturation, diminishing returns

**⚠️ SPECIAL RULE: NO AUTO-SCALE**
- **Why:** Service capacity limits (barber chairs, appointment slots)
- **Action:** Flag campaigns for manual review
- **Approval:** Required from Bossio before scaling
- **Manual override:** Updated Feb 14, 2026

**ROAS Target:** 3.0x minimum
- **Service business:** Higher margins than e-commerce
- **Lifetime value:** Repeat customers justify acquisition cost

**Example:**
```typescript
if (brand === 'Headlines' && roas >= 3.5) {
  // Don't auto-scale!
  return {
    action: 'FLAG_FOR_MANUAL_REVIEW',
    message: 'Headlines campaign hit 3.5x ROAS - awaiting manual approval to scale',
    reason: 'Service capacity constraints'
  }
}
```

---

## 4. Pie Accounting

### Business Profile
- **Type:** Accounting firm (service business)
- **Revenue Channels:** Client services, retainers
- **Market:** Small businesses (Tampa area)
- **Priority:** Tier 2 (Growth Brand)

### Ad Platforms

**Meta Ads:**
- **Ad Account ID:** `1273494507315603`
- **Format:** `act_1273494507315603`
- **Status:** ✅ Active
- **Average Monthly Spend:** ~$10K

**Google Ads:**
- **Customer ID:** `8590184784`
- **Formatted:** `859-018-4784`
- **Via MCC:** `735-272-6472`
- **Status:** ✅ Active

### Google Business Profile

**Locations:** 1
**Address:** 5909 Johns Road, Tampa, FL
**API Status:** ✅ Connected

### Social Media

**Instagram:**
- **Handle:** `@pieaccounting`
- **API Status:** 🔄 Needs setup
- **Engagement:** Low (B2B audience)

### CRM

**Platform:** Go High Level (GHL)
**API Status:** 🔄 Not yet integrated
**Metrics:** Leads, pipeline, deal stages

### Budget & Rules

**Max Daily Spend:** $1,000/day
- **Constraint:** Lead processing capacity
- **Why:** Service business, can't handle 50+ leads/day
- **Risk:** Lead quality drops, sales team overwhelmed

**Special Rules:** None (standard automation)

**ROAS Target:** 3.0x minimum
- **Lead value:** $1,500-5,000 per client (LTV)
- **Conversion rate:** ~10-15% (lead → client)
- **Acceptable CPL:** $50-150 per lead

**Example Calculation:**
```
Lead Value: $2,000 (avg LTV)
Conversion Rate: 10%
Acceptable CPL: $200 ($2,000 × 10%)

If spending $500/day and getting 5 leads/day:
CPL: $100 ✅ (below $200 target)
```

---

## 5. Sunshine State Chrome (SSC)

### Business Profile
- **Type:** E-commerce + Physical location (automotive chrome/accessories)
- **Revenue Channels:** Shopify, in-person sales
- **Market:** B2C (car enthusiasts, Tampa area)
- **Priority:** Tier 2 (Growth Brand)

### Ad Platforms

**Meta Ads:**
- **Status:** ⏸️ Not yet running Meta ads (planned expansion)
- **Account ID:** TBD

**Google Ads:**
- **Customer ID:** `5212367068`
- **Formatted:** `521-236-7068`
- **Via MCC:** `735-272-6472`
- **Status:** ✅ **FIXED Feb 15, 2026** (was inaccessible)

### E-commerce

**Shopify:**
- **Store:** `0wfybq-b2.myshopify.com`
- **Access Token:** In `.env`
- **Status:** ✅ Connected

### Google Business Profile

**Locations:** 1
**Address:** 13658 N 12th, Tampa, FL
**API Status:** ✅ Connected

### Social Media

**Instagram:**
- **Handle:** `@sunshine_state_chrome`
- **API Status:** 🔄 Needs setup
- **Engagement:** Low (newer account)

### Budget & Rules

**Max Daily Spend:** TBD (start conservative)
- **Recommended start:** $100-200/day
- **Test for 30 days, then scale**

**Special Rules:** None

**ROAS Target:** 3.0x minimum
- **Growing brand:** May be more patient in early testing
- **Acceptable:** 2.5-3.0x for first 30 days while algorithm learns

---

## 6. UNKWN Pro

### Business Profile
- **Type:** E-commerce (apparel/lifestyle)
- **Revenue Channels:** Shopify
- **Market:** B2C (streetwear, lifestyle)
- **Priority:** Tier 2 (Growth Brand)

### Ad Platforms

**Meta Ads:**
- **Status:** ⏸️ Boosting posts only (not full campaigns yet)
- **Account ID:** TBD (when full ads launch)

**Google Ads:**
- **Status:** ⏸️ Not yet running

### E-commerce

**Shopify:**
- **Store:** `unkwnpro.myshopify.com`
- **Access Token:** In `.env`
- **Status:** ✅ Connected

### Social Media

**Instagram:**
- **Handle:** `@unkwnpro`
- **API Status:** 🔄 Needs setup
- **Engagement:** Moderate

### Budget & Rules

**Max Daily Spend:** TBD (not yet running ads)

**Special Rules:** None

**Next Steps:**
1. Transition from boosting to full ad campaigns
2. Set up conversion tracking
3. Launch with $50-100/day test budget
4. Test for 30 days, scale winners

---

## Google Ads MCC (Manager Account)

**MCC ID:** `735-272-6472` (formatted)
**MCC ID:** `7352726472` (API format, no dashes)

**Managed Accounts:**
1. Tomb45 (813-799-2951)
2. FFC (138-433-1870)
3. Headlines (389-950-5299)
4. Pie (859-018-4784)
5. SSC (521-236-7068) — **FIXED Feb 15, 2026**

**Admin Access:** ✅ Full access to all accounts
**API Access:** ✅ OAuth refresh token configured

---

## Credentials & Secrets

**Location:** `~/clawd/projects/bossio-solution-dashboard/.env`

**Required Variables:**
```bash
# Meta Ads
META_ACCESS_TOKEN=<system_user_token>

# Google Ads
GOOGLE_ADS_CLIENT_ID=<oauth_client_id>
GOOGLE_ADS_CLIENT_SECRET=<oauth_client_secret>
GOOGLE_ADS_REFRESH_TOKEN=<refresh_token>
GOOGLE_ADS_DEVELOPER_TOKEN=<developer_token>

# Shopify (per store)
SHOPIFY_TOMB45_TOKEN=<access_token>
SHOPIFY_FFC_TOKEN=<access_token>
SHOPIFY_SSC_TOKEN=<access_token>
SHOPIFY_UNKWN_TOKEN=<access_token>

# Twilio SMS
TWILIO_ACCOUNT_SID=$TWILIO_ACCOUNT_SID
TWILIO_AUTH_TOKEN=<auth_token>
TWILIO_PHONE_NUMBER=+18135483884
TWILIO_RECIPIENT=+13525568981

# Database
DATABASE_URL=<neon_postgres_url>

# Redis (optional)
UPSTASH_REDIS_REST_URL=<redis_url>
UPSTASH_REDIS_REST_TOKEN=<redis_token>
```

**Security:**
- `.env` file is git-ignored (never commit secrets!)
- Production secrets in Vercel environment variables
- Local development uses separate test credentials

---

## SMS Alert Configuration

**Service:** Twilio
**Account SID:** `$TWILIO_ACCOUNT_SID`
**Phone Number:** `+18135483884`
**Default Recipient:** `+13525568981` (Bossio)

### Brand-Specific SMS Numbers

Configured in `BrandAlertSettings` database table:

| Brand | SMS Number | Override Default? |
|-------|------------|-------------------|
| Tomb45 | (default) | No |
| FFC | (default) | No |
| Headlines | (default) | No |
| Pie | (default) | No |
| SSC | (default) | No |

**Future:** Can configure brand-specific phone numbers
**Example:** Headlines alerts go to barbershop manager, not Bossio

### Alert Types (SMS)

**Critical Alerts (immediate SMS):**
- Budget overspend >110% before 6 PM
- ROAS drop >30% vs yesterday
- Campaign approaching 7-day deadline (<2.5x on day 6)

**Info Alerts (dashboard/Telegram only):**
- Frequency >4.5 (creative fatigue)
- New campaign launched
- Campaign recovered to ≥3.0x ROAS

---

## Quick Reference Table

| Brand | Meta ID | Google ID | Max Spend | Special Rule |
|-------|---------|-----------|-----------|--------------|
| **Tomb45** | 319971796 | 813-799-2951 | $3,000 | None |
| **FFC** | 2834312206725455 | 138-433-1870 | $2,000 | Inventory |
| **Headlines** | 510699435029440 | 389-950-5299 | $500 | **NO AUTO-SCALE** |
| **Pie** | 1273494507315603 | 859-018-4784 | $1,000 | None |
| **SSC** | — | 521-236-7068 | TBD | None |
| **UNKWN Pro** | — | — | TBD | Not running ads |

---

## Brand Priority Matrix

### Tier 1: Revenue Drivers (70% budget)
- **Tomb45** — Highest revenue, most mature
- **FFC** — Second highest, proven profitability

**Strategy:**
- Optimize aggressively
- Scale winners fast
- Large budget allocation ($5K/day combined)

### Tier 2: Growth Brands (25% budget)
- **Headlines** — Local market, service business
- **Pie** — Lead gen, service business
- **SSC** — Growing e-commerce + local

**Strategy:**
- Test and learn
- More patient with ROAS (2.5-3.0x acceptable early)
- Smaller budgets ($2K/day combined)

### Tier 3: Experimental (5% budget)
- **UNKWN Pro** — Not yet running ads

**Strategy:**
- Launch when ready
- Small test budgets ($50-100/day)
- Focus on conversion tracking first

---

## Chrome Browser Profiles (for manual work)

**Location:** MacBook Chrome profiles

| Profile | Name | Email | Brand |
|---------|------|-------|-------|
| Default | Person 1 | c50bossio@gmail.com | Personal |
| Profile 1 | florida | floridasfinestcustom@gmail.com | FFC |
| Profile 2 | Christopher | headlineswpb@gmail.com | Headlines |
| Profile 4 | Dandre | tomb45academy@gmail.com | Tomb45 |
| Profile 8 | Sunshine State | — | SSC (needs login) |
| Profile 9 | Pie Accounting | — | Pie (needs login) |

**Use case:**
- Manual campaign creation
- Visual creative uploads
- Account settings changes
- Billing updates

---

## Data Sources Summary

| Data Type | Tomb45 | FFC | Headlines | Pie | SSC | UNKWN Pro |
|-----------|--------|-----|-----------|-----|-----|-----------|
| **Meta Ads** | ✅ | ✅ | ✅ | ✅ | ⏸️ | ⏸️ |
| **Google Ads** | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Shopify** | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **Amazon** | 🔄 | ❌ | ❌ | ❌ | ❌ | ❌ |
| **Instagram** | 🔄 | 🔄 | 🔄 | 🔄 | 🔄 | 🔄 |
| **YouTube** | 🔄 | 🔄 | ❌ | ❌ | ❌ | ❌ |
| **GBP** | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ |
| **CRM** | ❌ | ❌ | ❌ | 🔄 | ❌ | ❌ |

**Legend:**
- ✅ Connected
- 🔄 Needs setup
- ⏸️ Planned but not yet running
- ❌ Not applicable

---

**Last Updated:** 2026-02-19  
**Maintainer:** Atlas (COO)  
**Source:** BRANDS.md, TOOLS.md, PRD.md
