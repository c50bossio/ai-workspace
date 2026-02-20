# Google Ads API Reference

**Complete reference for Google Ads API integration**

## Authentication

### OAuth 2.0 Refresh Token

**Type:** OAuth2 Refresh Token (long-lived)
**Location:** `~/clawd/projects/bossio-solution-dashboard/.env`
**Variables:**
- `GOOGLE_ADS_CLIENT_ID`
- `GOOGLE_ADS_CLIENT_SECRET`
- `GOOGLE_ADS_REFRESH_TOKEN`
- `GOOGLE_ADS_DEVELOPER_TOKEN`

**Token Properties:**
- Refresh token does NOT expire (unless revoked)
- Generate access token from refresh token on each request
- Access token valid for 1 hour

### Generate Access Token

```javascript
async function getGoogleAdsAccessToken() {
  const response = await fetch('https://oauth2.googleapis.com/token', {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: new URLSearchParams({
      client_id: process.env.GOOGLE_ADS_CLIENT_ID,
      client_secret: process.env.GOOGLE_ADS_CLIENT_SECRET,
      refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN,
      grant_type: 'refresh_token'
    })
  })
  
  const data = await response.json()
  return data.access_token // Valid for 1 hour
}
```

---

## Customer IDs & MCC

### Manager Account (MCC)
**MCC ID:** `735-272-6472` (without dashes: `7352726472`)

All customer accounts managed under this MCC.

### Customer Accounts

| Brand | Customer ID | Formatted | Status |
|-------|-------------|-----------|--------|
| Tomb45 | 8137992951 | 813-799-2951 | ✅ Active |
| Headlines | 3899502299 | 389-950-5299 | ✅ Active |
| SSC | 5212367068 | 521-236-7068 | ✅ **FIXED Feb 15, 2026** |
| FFC | 1384331870 | 138-433-1870 | ✅ Active |
| Pie | 8590184784 | 859-018-4784 | ✅ Active |

**Format in API calls:** No dashes (e.g., `8137992951`)

---

## Google Ads API SDK

### Installation

```bash
npm install google-ads-api
```

### Client Setup

```typescript
import { GoogleAdsApi } from 'google-ads-api'

const client = new GoogleAdsApi({
  client_id: process.env.GOOGLE_ADS_CLIENT_ID,
  client_secret: process.env.GOOGLE_ADS_CLIENT_SECRET,
  developer_token: process.env.GOOGLE_ADS_DEVELOPER_TOKEN
})

const customer = client.Customer({
  customer_id: '8137992951',
  refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
})
```

---

## Key Operations

### 1. Query Campaign Performance

**GAQL (Google Ads Query Language):**

```typescript
const query = `
  SELECT
    campaign.id,
    campaign.name,
    campaign.status,
    campaign_budget.amount_micros,
    metrics.cost_micros,
    metrics.impressions,
    metrics.clicks,
    metrics.conversions,
    metrics.conversions_value
  FROM campaign
  WHERE campaign.status IN ('ENABLED', 'PAUSED')
    AND segments.date DURING LAST_7_DAYS
  ORDER BY metrics.cost_micros DESC
`

const campaigns = await customer.query(query)
```

**Response:**
```javascript
[
  {
    campaign: {
      id: '12345678',
      name: 'Testing Campaign',
      status: 'ENABLED'
    },
    campaign_budget: {
      amount_micros: '2500000000' // $2,500.00
    },
    metrics: {
      cost_micros: '17500000000', // $17,500.00
      impressions: '245678',
      clicks: '6789',
      conversions: 156,
      conversions_value: 35350.0
    }
  }
]
```

**Note:** Amounts in micros (1,000,000 micros = $1.00)

### 2. Get Campaign Details

```typescript
const query = `
  SELECT
    campaign.id,
    campaign.name,
    campaign.status,
    campaign.advertising_channel_type,
    campaign_budget.amount_micros,
    campaign_budget.explicitly_shared
  FROM campaign
  WHERE campaign.id = '12345678'
`

const campaign = await customer.query(query)
```

### 3. Update Campaign Budget

**Using SDK:**
```typescript
import { resources } from 'google-ads-api'

async function updateCampaignBudget(customerId: string, campaignId: string, newBudgetDollars: number) {
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  // Convert dollars to micros
  const budgetMicros = Math.round(newBudgetDollars * 1000000)
  
  // Get campaign budget ID
  const query = `
    SELECT campaign_budget.id, campaign_budget.resource_name
    FROM campaign_budget
    WHERE campaign.id = '${campaignId}'
  `
  const result = await customer.query(query)
  const budgetResourceName = result[0].campaign_budget.resource_name
  
  // Update budget
  const budget = {
    resource_name: budgetResourceName,
    amount_micros: budgetMicros
  }
  
  await customer.campaignBudgets.update(budget)
  
  return {
    success: true,
    newBudget: newBudgetDollars
  }
}
```

### 4. Pause/Enable Campaign

```typescript
async function pauseCampaign(customerId: string, campaignId: string) {
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  const campaign = {
    resource_name: `customers/${customerId}/campaigns/${campaignId}`,
    status: 'PAUSED' // or 'ENABLED'
  }
  
  await customer.campaigns.update(campaign)
  
  return { success: true, campaignId, status: 'PAUSED' }
}
```

---

## Key Metrics

### Cost (Spend)
- **Field:** `metrics.cost_micros`
- **Type:** String (micros)
- **Conversion:** Divide by 1,000,000
- **Example:** `"17500000000"` = $17,500.00

### Impressions
- **Field:** `metrics.impressions`
- **Type:** String
- **Meaning:** Number of times ad was shown

### Clicks
- **Field:** `metrics.clicks`
- **Type:** String
- **Meaning:** Number of times ad was clicked

### Conversions
- **Field:** `metrics.conversions`
- **Type:** Number
- **Meaning:** Total conversions (purchases, leads, etc.)

### Conversion Value (Revenue)
- **Field:** `metrics.conversions_value`
- **Type:** Number (dollars, NOT micros)
- **Meaning:** Total revenue from conversions
- **Example:** `35350.0` = $35,350.00

### ROAS Calculation

```javascript
function calculateRoas(metrics) {
  const spend = parseInt(metrics.cost_micros) / 1000000
  const revenue = parseFloat(metrics.conversions_value)
  
  const roas = spend > 0 ? revenue / spend : 0
  return roas
}

// Example:
// cost_micros: "17500000000" → $17,500.00
// conversions_value: 35350.0 → $35,350.00
// ROAS: $35,350 / $17,500 = 2.02x
```

---

## GAQL (Google Ads Query Language)

### Syntax

**Basic Structure:**
```sql
SELECT <fields>
FROM <resource>
WHERE <conditions>
ORDER BY <field> [ASC|DESC]
LIMIT <number>
```

### Date Ranges

**Last 7 days:**
```sql
WHERE segments.date DURING LAST_7_DAYS
```

**Last 30 days:**
```sql
WHERE segments.date DURING LAST_30_DAYS
```

**Custom range:**
```sql
WHERE segments.date BETWEEN '2026-02-01' AND '2026-02-15'
```

**Today:**
```sql
WHERE segments.date = 'LAST_30_DAYS'
```

### Common Filters

**Active campaigns only:**
```sql
WHERE campaign.status = 'ENABLED'
```

**Active + Paused:**
```sql
WHERE campaign.status IN ('ENABLED', 'PAUSED')
```

**Specific campaign:**
```sql
WHERE campaign.id = '12345678'
```

**Search campaigns only:**
```sql
WHERE campaign.advertising_channel_type = 'SEARCH'
```

### Resource Types

| Resource | Purpose |
|----------|---------|
| `campaign` | Campaign-level data |
| `ad_group` | Ad group-level data |
| `ad_group_ad` | Ad-level data |
| `keyword_view` | Keyword performance |
| `campaign_budget` | Budget information |
| `customer` | Account-level data |

---

## Budget Format Conversion

**Google Ads uses micros (1,000,000 micros = $1.00)**

### Convert Dollars to Micros
```javascript
function dollarsToMicros(dollars) {
  return Math.round(dollars * 1000000)
}

// Example: $2,500.00 → 2500000000
dollarsToMicros(2500) // 2500000000
```

### Convert Micros to Dollars
```javascript
function microsToDollars(micros) {
  return parseInt(micros) / 1000000
}

// Example: "2500000000" → $2,500.00
microsToDollars("2500000000") // 2500
```

**Important:** Budgets in micros, but conversion value is already in dollars!

```javascript
// ❌ WRONG
const revenue = parseInt(metrics.conversions_value) / 1000000

// ✅ CORRECT
const revenue = parseFloat(metrics.conversions_value)
```

---

## Rate Limits

### Limits
- **15,000 operations per day** per developer token
- **1,000 operations per minute** (burst)
- **No hourly limit** (unlike Meta)

**Operation = one API request (query, update, etc.)**

### Best Practices

1. **Batch queries when possible**
   - Combine multiple campaigns in one query
   - Use `WHERE campaign.id IN (...)` 

2. **Cache aggressively**
   - 5-minute cache for metrics
   - 15-minute cache for campaign lists
   - 1-hour cache for configuration

3. **Query only what you need**
   - Don't fetch all fields if only need cost + conversions
   - Smaller responses = faster queries

4. **Schedule heavy queries off-peak**
   - Daily audits at 6 AM (low traffic)
   - Backfill historical data overnight

### Handling Rate Limits

**Error Response:**
```json
{
  "error": {
    "code": 429,
    "message": "Resource has been exhausted (e.g. check quota).",
    "status": "RESOURCE_EXHAUSTED"
  }
}
```

**Retry Logic:**
```typescript
async function queryWithRetry(customer: any, query: string, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await customer.query(query)
    } catch (error: any) {
      if (error.message?.includes('RESOURCE_EXHAUSTED')) {
        const waitTime = Math.pow(2, i) * 1000 // 1s, 2s, 4s
        console.warn(`Rate limit hit, waiting ${waitTime}ms`)
        await sleep(waitTime)
        continue
      }
      throw error
    }
  }
  throw new Error('Max retries exceeded')
}
```

---

## Caching Strategy

### What to Cache

| Data | TTL | Why |
|------|-----|-----|
| Campaign metrics | 5 min | Changes frequently |
| Campaign list | 15 min | Rarely changes |
| Budget info | 1 hour | Changes manually only |
| Account details | 1 hour | Rarely changes |

### Cache Keys

```javascript
// Campaign metrics
const cacheKey = `google:metrics:${customerId}:${campaignId}:last7days`

// Campaign list
const cacheKey = `google:campaigns:${customerId}`

// All campaigns metrics
const cacheKey = `google:metrics:${customerId}:all:last7days`
```

### Implementation

```typescript
import { Redis } from '@upstash/redis'

async function getCampaignMetrics(customerId: string, campaignId: string) {
  const cacheKey = `google:metrics:${customerId}:${campaignId}:last7days`
  
  // Try cache first
  const cached = await redis.get(cacheKey)
  if (cached) return cached
  
  // Fetch from API
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  const query = `
    SELECT
      campaign.id,
      metrics.cost_micros,
      metrics.conversions_value
    FROM campaign
    WHERE campaign.id = '${campaignId}'
      AND segments.date DURING LAST_7_DAYS
  `
  
  const data = await customer.query(query)
  
  // Cache for 5 minutes
  await redis.setex(cacheKey, 300, data)
  
  return data
}
```

---

## Error Handling

### Common Errors

**1. Authentication Failed**
```json
{
  "error": {
    "code": 401,
    "message": "Request is missing required authentication credential.",
    "status": "UNAUTHENTICATED"
  }
}
```
**Fix:** Refresh access token from refresh token

**2. Invalid Customer ID**
```json
{
  "error": {
    "code": 400,
    "message": "Invalid customer ID 'XXX'.",
    "status": "INVALID_ARGUMENT"
  }
}
```
**Fix:** Verify customer ID is correct (no dashes, correct number)

**3. Permission Denied**
```json
{
  "error": {
    "code": 403,
    "message": "The caller does not have permission",
    "status": "PERMISSION_DENIED"
  }
}
```
**Fix:** Check MCC has access to customer account

**4. GAQL Syntax Error**
```json
{
  "error": {
    "code": 400,
    "message": "Query syntax error: ...",
    "status": "INVALID_ARGUMENT"
  }
}
```
**Fix:** Validate GAQL syntax, check field names

### Error Handling Code

```typescript
async function handleGoogleAdsError(error: any) {
  const status = error.details?.status || error.status
  const message = error.message
  
  switch (status) {
    case 'UNAUTHENTICATED':
      console.error('Google Ads authentication failed - refresh token')
      // Generate new access token
      break
      
    case 'RESOURCE_EXHAUSTED':
      console.warn('Google Ads rate limit hit')
      // Implement backoff
      break
      
    case 'PERMISSION_DENIED':
      console.error('Google Ads permission denied')
      throw new Error('Insufficient permissions')
      
    case 'INVALID_ARGUMENT':
      console.error('Google Ads invalid argument:', message)
      throw new Error('Invalid query or parameters')
      
    default:
      console.error('Google Ads error:', status, message)
      throw error
  }
}
```

---

## Code Examples

### Fetch Campaign ROAS (7-day)

```typescript
async function getCampaignRoas(customerId: string, campaignId: string) {
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  const query = `
    SELECT
      campaign.id,
      campaign.name,
      metrics.cost_micros,
      metrics.conversions,
      metrics.conversions_value
    FROM campaign
    WHERE campaign.id = '${campaignId}'
      AND segments.date DURING LAST_7_DAYS
  `
  
  const results = await customer.query(query)
  const data = results[0]
  
  const spend = parseInt(data.metrics.cost_micros) / 1000000
  const revenue = parseFloat(data.metrics.conversions_value)
  const conversions = parseFloat(data.metrics.conversions)
  
  const roas = spend > 0 ? revenue / spend : 0
  
  return {
    campaignId,
    campaignName: data.campaign.name,
    spend,
    revenue,
    conversions,
    roas,
    days: 7
  }
}
```

### Get All Active Campaigns

```typescript
async function getAllCampaigns(customerId: string) {
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  const query = `
    SELECT
      campaign.id,
      campaign.name,
      campaign.status,
      campaign_budget.amount_micros,
      metrics.cost_micros,
      metrics.conversions_value
    FROM campaign
    WHERE campaign.status IN ('ENABLED', 'PAUSED')
      AND segments.date DURING LAST_7_DAYS
    ORDER BY metrics.cost_micros DESC
  `
  
  const campaigns = await customer.query(query)
  
  return campaigns.map(c => ({
    id: c.campaign.id,
    name: c.campaign.name,
    status: c.campaign.status,
    budget: parseInt(c.campaign_budget.amount_micros) / 1000000,
    spend: parseInt(c.metrics.cost_micros) / 1000000,
    revenue: parseFloat(c.metrics.conversions_value),
    roas: parseFloat(c.metrics.conversions_value) / (parseInt(c.metrics.cost_micros) / 1000000)
  }))
}
```

### Scale Campaign Budget

```typescript
async function scaleCampaignBudget(
  customerId: string,
  campaignId: string,
  increasePercent: number = 20
) {
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  // Get current budget
  const query = `
    SELECT
      campaign.name,
      campaign_budget.id,
      campaign_budget.resource_name,
      campaign_budget.amount_micros
    FROM campaign
    WHERE campaign.id = '${campaignId}'
  `
  
  const result = await customer.query(query)
  const budgetData = result[0].campaign_budget
  
  const currentBudgetDollars = parseInt(budgetData.amount_micros) / 1000000
  const newBudgetDollars = currentBudgetDollars * (1 + increasePercent / 100)
  const newBudgetMicros = Math.round(newBudgetDollars * 1000000)
  
  // Validate minimum
  if (newBudgetDollars < 15) {
    throw new Error('Budget would be below $15/day minimum')
  }
  
  // Update budget
  const budget = {
    resource_name: budgetData.resource_name,
    amount_micros: newBudgetMicros
  }
  
  await customer.campaignBudgets.update(budget)
  
  return {
    campaignId,
    campaignName: result[0].campaign.name,
    oldBudget: currentBudgetDollars,
    newBudget: newBudgetDollars,
    increase: increasePercent
  }
}
```

### Pause Campaign

```typescript
async function pauseCampaign(customerId: string, campaignId: string, reason: string) {
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  const campaign = {
    resource_name: `customers/${customerId}/campaigns/${campaignId}`,
    status: 'PAUSED'
  }
  
  await customer.campaigns.update(campaign)
  
  // Log to campaign-changes.jsonl
  const logEntry = {
    timestamp: new Date().toISOString(),
    campaignId,
    platform: 'google',
    action: 'pause',
    reason,
    executedBy: 'atlas'
  }
  
  await fs.appendFile(
    'logs/campaign-changes.jsonl',
    JSON.stringify(logEntry) + '\n'
  )
  
  return { success: true, campaignId, action: 'paused' }
}
```

---

## Batch Queries

**Combine multiple campaigns in one query:**

```typescript
const campaignIds = ['12345678', '87654321', '11223344']

const query = `
  SELECT
    campaign.id,
    campaign.name,
    metrics.cost_micros,
    metrics.conversions_value
  FROM campaign
  WHERE campaign.id IN (${campaignIds.map(id => `'${id}'`).join(',')})
    AND segments.date DURING LAST_7_DAYS
`

const results = await customer.query(query)
// Returns array with all campaigns in one call
```

**Benefits:**
- 1 API call instead of N calls
- Faster response time
- Avoids rate limit issues
- Recommended for 5+ campaigns

---

## Conversion Tracking

### Conversion Actions

**List conversion actions:**
```typescript
const query = `
  SELECT
    conversion_action.id,
    conversion_action.name,
    conversion_action.type,
    conversion_action.status
  FROM conversion_action
  WHERE conversion_action.status = 'ENABLED'
`

const conversions = await customer.query(query)
```

**Common conversion types:**
- `PURCHASE` — E-commerce purchases
- `ADD_TO_CART` — Items added to cart
- `LEAD` — Lead form submissions
- `PAGE_VIEW` — Key page views
- `PHONE_CALL` — Phone calls from ads

### Attribution Window

**Default:** 30-day click, 1-day view
- Conversions counted within 30 days of ad click
- Or within 1 day of ad impression

**Custom attribution:**
```typescript
const query = `
  SELECT
    campaign.id,
    metrics.conversions,
    metrics.all_conversions
  FROM campaign
  WHERE segments.date DURING LAST_7_DAYS
    AND segments.conversion_action = 'customers/${customerId}/conversionActions/123'
`
```

---

## Testing

### Test Mode

Google Ads API has no dedicated sandbox, but you can:
1. Use test customer accounts (low spend)
2. Enable dry-run mode in code
3. Query data without making changes

**Dry-Run Example:**
```typescript
const DRY_RUN = process.env.DRY_RUN === 'true'

async function updateCampaign(customerId: string, campaignId: string, params: any) {
  if (DRY_RUN) {
    console.log(`[DRY-RUN] Would update campaign ${campaignId}:`, params)
    return { success: true, dryRun: true }
  }
  
  const customer = client.Customer({
    customer_id: customerId,
    refresh_token: process.env.GOOGLE_ADS_REFRESH_TOKEN
  })
  
  return await customer.campaigns.update(params)
}
```

---

## Documentation Links

- **Google Ads API Docs:** https://developers.google.com/google-ads/api/docs/start
- **GAQL Reference:** https://developers.google.com/google-ads/api/docs/query/overview
- **Reports Guide:** https://developers.google.com/google-ads/api/docs/reporting/overview
- **Node.js SDK:** https://github.com/Opteo/google-ads-api
- **Error Codes:** https://developers.google.com/google-ads/api/docs/error-codes

---

## Troubleshooting

**"Customer not found"**
- Verify customer ID (no dashes)
- Check MCC has access to customer
- Ensure customer ID is correct

**"GAQL syntax error"**
- Validate field names (case-sensitive!)
- Check resource type (campaign vs ad_group vs keyword_view)
- Use GAQL validator: https://developers.google.com/google-ads/api/fields/v14/overview

**"No data returned"**
- Check date range (might be too narrow)
- Verify campaign has spend in that period
- Ensure campaign is not deleted

**"Permission denied"**
- Check OAuth scopes include `adwords`
- Verify MCC has admin access
- Refresh token might be revoked

---

**Last Updated:** 2026-02-19  
**Integration Status:** ✅ Active (5 accounts connected)
