# Meta Ads API Reference

**Complete reference for Meta Marketing API integration**

## Authentication

### Access Token

**Type:** System User Token (long-lived)
**Location:** `~/clawd/projects/bossio-solution-dashboard/.env`
**Variable:** `META_ACCESS_TOKEN`

**Token Properties:**
- 90-day expiry (auto-refresh recommended)
- System user level (not personal account)
- Permissions: `ads_management`, `ads_read`, `business_management`

### Refresh Token (Before Expiry)

```bash
# Get new token (every 60 days recommended)
curl -X GET "https://graph.facebook.com/v18.0/oauth/access_token" \
  -d "grant_type=fb_exchange_token" \
  -d "client_id=YOUR_APP_ID" \
  -d "client_secret=YOUR_APP_SECRET" \
  -d "fb_exchange_token=CURRENT_ACCESS_TOKEN"
```

**Set reminder:** Token expires every 90 days

---

## Ad Account IDs

| Brand | Account ID | Status |
|-------|------------|--------|
| Tomb45 | 319971796 | ✅ Active |
| FFC | 2834312206725455 | ✅ Active |
| Headlines | 510699435029440 | ✅ Active |
| Pie | 1273494507315603 | ✅ Active |
| SSC | TBD | ⏸️ Not yet running Meta ads |

**Format in API calls:** `act_319971796` (prepend `act_`)

---

## Key Endpoints

### 1. Get Account Insights

**Endpoint:** `GET /{ad_account_id}/insights`

**Example:**
```bash
curl -X GET "https://graph.facebook.com/v18.0/act_319971796/insights" \
  -d "access_token=YOUR_ACCESS_TOKEN" \
  -d "fields=spend,impressions,clicks,ctr,actions,action_values" \
  -d "time_range={'since':'2026-02-12','until':'2026-02-19'}" \
  -d "level=account"
```

**Response:**
```json
{
  "data": [
    {
      "spend": "2845.67",
      "impressions": "123456",
      "clicks": "3456",
      "ctr": "2.8",
      "actions": [
        {"action_type": "purchase", "value": "87"}
      ],
      "action_values": [
        {"action_type": "purchase", "value": "8654.23"}
      ]
    }
  ]
}
```

### 2. Get Campaign List

**Endpoint:** `GET /{ad_account_id}/campaigns`

**Example:**
```bash
curl -X GET "https://graph.facebook.com/v18.0/act_319971796/campaigns" \
  -d "access_token=YOUR_ACCESS_TOKEN" \
  -d "fields=id,name,status,daily_budget,lifetime_budget,objective" \
  -d "filtering=[{'field':'status','operator':'IN','value':['ACTIVE','PAUSED']}]"
```

**Response:**
```json
{
  "data": [
    {
      "id": "123456789",
      "name": "Testing Campaign",
      "status": "ACTIVE",
      "daily_budget": "250000",
      "objective": "OUTCOME_SALES"
    }
  ]
}
```

**Note:** Budgets in cents (250000 = $2,500.00)

### 3. Get Campaign Insights

**Endpoint:** `GET /{campaign_id}/insights`

**Example:**
```bash
curl -X GET "https://graph.facebook.com/v18.0/123456789/insights" \
  -d "access_token=YOUR_ACCESS_TOKEN" \
  -d "fields=spend,impressions,clicks,ctr,frequency,actions,action_values" \
  -d "time_range={'since':'2026-02-12','until':'2026-02-19'}" \
  -d "level=campaign"
```

**Response:**
```json
{
  "data": [
    {
      "campaign_id": "123456789",
      "campaign_name": "Testing Campaign",
      "spend": "17500.00",
      "impressions": "245678",
      "clicks": "6789",
      "ctr": "2.76",
      "frequency": "3.42",
      "actions": [
        {"action_type": "purchase", "value": "156"}
      ],
      "action_values": [
        {"action_type": "purchase", "value": "35350.00"}
      ]
    }
  ]
}
```

### 4. Update Campaign Budget

**Endpoint:** `POST /{campaign_id}`

**Example (Scale +20%):**
```bash
curl -X POST "https://graph.facebook.com/v18.0/123456789" \
  -d "access_token=YOUR_ACCESS_TOKEN" \
  -d "daily_budget=300000"
```

**Example (Pause):**
```bash
curl -X POST "https://graph.facebook.com/v18.0/123456789" \
  -d "access_token=YOUR_ACCESS_TOKEN" \
  -d "status=PAUSED"
```

**Budget Format:** Cents (300000 = $3,000.00)

---

## Key Metrics

### Spend
- **Field:** `spend`
- **Type:** String (dollars, e.g., "2845.67")
- **Meaning:** Total ad spend in time range

### Impressions
- **Field:** `impressions`
- **Type:** String
- **Meaning:** Number of times ad was shown

### Clicks
- **Field:** `clicks`
- **Type:** String
- **Meaning:** Number of times ad was clicked

### CTR (Click-Through Rate)
- **Field:** `ctr`
- **Type:** String (percentage, e.g., "2.8")
- **Meaning:** (Clicks / Impressions) × 100

### Frequency
- **Field:** `frequency`
- **Type:** String (e.g., "3.42")
- **Meaning:** Average times each person saw ad
- **Fatigue Threshold:** >4.5 = audience exhausted

### Actions (Conversions)
- **Field:** `actions`
- **Type:** Array of objects
- **Format:** `[{"action_type": "purchase", "value": "87"}]`
- **Key Types:**
  - `purchase` — Completed purchases
  - `add_to_cart` — Items added to cart
  - `initiate_checkout` — Checkout started
  - `link_click` — Outbound clicks

### Action Values (Revenue)
- **Field:** `action_values`
- **Type:** Array of objects
- **Format:** `[{"action_type": "purchase", "value": "8654.23"}]`
- **Meaning:** Total revenue from conversions

### ROAS Calculation
```javascript
const purchases = data.actions?.find(a => a.action_type === 'purchase')?.value || 0
const revenue = data.action_values?.find(a => a.action_type === 'purchase')?.value || 0
const spend = parseFloat(data.spend || 0)

const roas = spend > 0 ? revenue / spend : 0
// Example: $8654.23 revenue / $2845.67 spend = 3.04x ROAS
```

---

## Time Ranges

### Date Format
**Format:** `YYYY-MM-DD` (ISO 8601)

### Common Ranges

**Last 7 days:**
```javascript
const today = new Date()
const sevenDaysAgo = new Date(today)
sevenDaysAgo.setDate(today.getDate() - 7)

const timeRange = {
  since: sevenDaysAgo.toISOString().split('T')[0], // "2026-02-12"
  until: today.toISOString().split('T')[0] // "2026-02-19"
}
```

**Last 30 days:**
```javascript
const thirtyDaysAgo = new Date(today)
thirtyDaysAgo.setDate(today.getDate() - 30)

const timeRange = {
  since: thirtyDaysAgo.toISOString().split('T')[0],
  until: today.toISOString().split('T')[0]
}
```

**Custom range:**
```javascript
const timeRange = {
  since: '2026-02-01',
  until: '2026-02-15'
}
```

---

## Rate Limits

### Limits
- **200 calls per hour** per user token
- **Burst:** 25 calls in 1 minute
- **Recovery:** 1 minute cooldown after burst

### Handling Rate Limits

**Error Response:**
```json
{
  "error": {
    "message": "Application request limit reached",
    "type": "OAuthException",
    "code": 4,
    "fbtrace_id": "..."
  }
}
```

**Best Practices:**
1. **Cache aggressively:** 5-minute cache for insights
2. **Batch requests:** Combine multiple campaigns in one call
3. **Exponential backoff:** Wait 1 min, 2 min, 4 min on errors
4. **Off-peak hours:** Schedule heavy queries 12 AM - 6 AM

**Example Retry Logic:**
```javascript
async function fetchWithRetry(url, params, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, params)
      if (response.ok) return await response.json()
      
      if (response.status === 429) { // Rate limit
        const waitTime = Math.pow(2, i) * 60 * 1000 // 1min, 2min, 4min
        await sleep(waitTime)
        continue
      }
      
      throw new Error(`HTTP ${response.status}`)
    } catch (error) {
      if (i === maxRetries - 1) throw error
    }
  }
}
```

---

## Caching Strategy

### What to Cache

| Data | TTL | Why |
|------|-----|-----|
| Account insights | 5 min | Changes frequently |
| Campaign list | 15 min | Rarely changes |
| Campaign insights | 5 min | Changes frequently |
| Campaign details | 1 hour | Rarely changes |

### Cache Keys

```javascript
// Account insights
const cacheKey = `meta:insights:account:${accountId}:${since}:${until}`

// Campaign insights
const cacheKey = `meta:insights:campaign:${campaignId}:${since}:${until}`

// Campaign list
const cacheKey = `meta:campaigns:${accountId}`
```

### Implementation (Upstash Redis)

```typescript
import { Redis } from '@upstash/redis'

const redis = new Redis({
  url: process.env.UPSTASH_REDIS_REST_URL,
  token: process.env.UPSTASH_REDIS_REST_TOKEN
})

async function getCampaignInsights(campaignId: string, since: string, until: string) {
  const cacheKey = `meta:insights:campaign:${campaignId}:${since}:${until}`
  
  // Try cache first
  const cached = await redis.get(cacheKey)
  if (cached) return cached
  
  // Fetch from API
  const data = await fetch(/* Meta API call */)
  
  // Cache for 5 minutes
  await redis.setex(cacheKey, 300, data)
  
  return data
}
```

---

## Error Handling

### Common Errors

**1. Invalid Access Token**
```json
{
  "error": {
    "message": "Invalid OAuth access token",
    "type": "OAuthException",
    "code": 190
  }
}
```
**Fix:** Refresh token

**2. Permission Denied**
```json
{
  "error": {
    "message": "Insufficient permissions",
    "type": "OAuthException",
    "code": 10
  }
}
```
**Fix:** Check token has `ads_management` permission

**3. Rate Limit**
```json
{
  "error": {
    "message": "Application request limit reached",
    "type": "OAuthException",
    "code": 4
  }
}
```
**Fix:** Wait 1-5 minutes, implement exponential backoff

**4. Campaign Not Found**
```json
{
  "error": {
    "message": "(#803) Some of the aliases you requested do not exist",
    "type": "OAuthException",
    "code": 803
  }
}
```
**Fix:** Verify campaign ID is correct, check if campaign was deleted

### Error Handling Code

```typescript
async function handleMetaApiError(error: any) {
  if (!error.response) throw error
  
  const { code, message } = error.response.data.error
  
  switch (code) {
    case 190: // Invalid token
      console.error('Meta token expired - refresh needed')
      // Send alert to team
      throw new Error('Meta API token expired')
      
    case 4: // Rate limit
      console.warn('Meta API rate limit hit - backing off')
      await sleep(60000) // Wait 1 minute
      // Retry request
      break
      
    case 10: // Permission denied
      console.error('Meta API permission denied')
      throw new Error('Insufficient permissions')
      
    default:
      console.error(`Meta API error ${code}: ${message}`)
      throw error
  }
}
```

---

## Budget Format Conversion

**Meta API uses cents (100 = $1.00)**

### Convert Dollars to Cents
```javascript
function dollarsToCents(dollars) {
  return Math.round(dollars * 100)
}

// Example: $2,500.00 → 250000
dollarsToCents(2500) // 250000
```

### Convert Cents to Dollars
```javascript
function centsToDollars(cents) {
  return cents / 100
}

// Example: 250000 → $2,500.00
centsToDollars(250000) // 2500
```

### Safe Budget Updates
```javascript
async function updateCampaignBudget(campaignId, newBudgetDollars) {
  const budgetCents = dollarsToCents(newBudgetDollars)
  
  // Validate
  if (budgetCents < 1500) { // $15 minimum
    throw new Error('Budget below $15/day minimum')
  }
  
  // Update via API
  const response = await fetch(`https://graph.facebook.com/v18.0/${campaignId}`, {
    method: 'POST',
    body: new URLSearchParams({
      access_token: process.env.META_ACCESS_TOKEN,
      daily_budget: budgetCents.toString()
    })
  })
  
  return response.json()
}
```

---

## Batch Requests

**Get insights for multiple campaigns in one call**

### Batch Endpoint

**Endpoint:** `POST /` (Graph API root)
**Body:** JSON with `batch` array

**Example:**
```javascript
const batch = [
  {
    method: 'GET',
    relative_url: '123456789/insights?fields=spend,actions,action_values&time_range={"since":"2026-02-12","until":"2026-02-19"}'
  },
  {
    method: 'GET',
    relative_url: '987654321/insights?fields=spend,actions,action_values&time_range={"since":"2026-02-12","until":"2026-02-19"}'
  }
]

const response = await fetch('https://graph.facebook.com/v18.0/', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    access_token: process.env.META_ACCESS_TOKEN,
    batch: JSON.stringify(batch)
  })
})
```

**Benefits:**
- 1 API call instead of N calls
- Faster response time
- Avoids rate limits
- Recommended for 5+ campaigns

**Limits:**
- Max 50 requests per batch
- Total response size <5MB

---

## Code Examples

### Fetch Campaign ROAS (7-day)

```typescript
import axios from 'axios'

async function getCampaignRoas(campaignId: string) {
  const today = new Date()
  const sevenDaysAgo = new Date(today)
  sevenDaysAgo.setDate(today.getDate() - 7)
  
  const timeRange = {
    since: sevenDaysAgo.toISOString().split('T')[0],
    until: today.toISOString().split('T')[0]
  }
  
  const response = await axios.get(
    `https://graph.facebook.com/v18.0/${campaignId}/insights`,
    {
      params: {
        access_token: process.env.META_ACCESS_TOKEN,
        fields: 'spend,actions,action_values,frequency',
        time_range: JSON.stringify(timeRange),
        level: 'campaign'
      }
    }
  )
  
  const data = response.data.data[0]
  
  const spend = parseFloat(data.spend || 0)
  const revenue = parseFloat(
    data.action_values?.find(a => a.action_type === 'purchase')?.value || 0
  )
  const frequency = parseFloat(data.frequency || 0)
  
  const roas = spend > 0 ? revenue / spend : 0
  
  return {
    campaignId,
    spend,
    revenue,
    roas,
    frequency,
    days: 7
  }
}
```

### Scale Campaign Budget

```typescript
async function scaleCampaignBudget(campaignId: string, increasePercent: number = 20) {
  // Get current budget
  const response = await axios.get(
    `https://graph.facebook.com/v18.0/${campaignId}`,
    {
      params: {
        access_token: process.env.META_ACCESS_TOKEN,
        fields: 'daily_budget,name'
      }
    }
  )
  
  const currentBudgetCents = parseInt(response.data.daily_budget)
  const currentBudgetDollars = currentBudgetCents / 100
  
  // Calculate new budget
  const newBudgetDollars = currentBudgetDollars * (1 + increasePercent / 100)
  const newBudgetCents = Math.round(newBudgetDollars * 100)
  
  // Validate minimum
  if (newBudgetDollars < 15) {
    throw new Error('Budget would be below $15/day minimum')
  }
  
  // Update budget
  await axios.post(
    `https://graph.facebook.com/v18.0/${campaignId}`,
    new URLSearchParams({
      access_token: process.env.META_ACCESS_TOKEN,
      daily_budget: newBudgetCents.toString()
    })
  )
  
  return {
    campaignId,
    campaignName: response.data.name,
    oldBudget: currentBudgetDollars,
    newBudget: newBudgetDollars,
    increase: increasePercent
  }
}
```

### Pause Campaign

```typescript
async function pauseCampaign(campaignId: string, reason: string) {
  await axios.post(
    `https://graph.facebook.com/v18.0/${campaignId}`,
    new URLSearchParams({
      access_token: process.env.META_ACCESS_TOKEN,
      status: 'PAUSED'
    })
  )
  
  // Log to campaign-changes.jsonl
  const logEntry = {
    timestamp: new Date().toISOString(),
    campaignId,
    platform: 'meta',
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

## Testing

### Test Account (Sandbox)

Meta provides test ad accounts for development:
- No real money spent
- Simulated impressions/clicks/conversions
- Full API access

**Create test account:**
https://developers.facebook.com/docs/marketing-api/testing

### Dry-Run Mode

Before executing real budget changes, test with dry-run:

```typescript
const DRY_RUN = process.env.DRY_RUN === 'true'

async function updateCampaign(campaignId: string, params: any) {
  if (DRY_RUN) {
    console.log(`[DRY-RUN] Would update campaign ${campaignId}:`, params)
    return { success: true, dryRun: true }
  }
  
  // Real API call
  return await axios.post(`https://graph.facebook.com/v18.0/${campaignId}`, params)
}
```

**Enable dry-run:**
```bash
export DRY_RUN=true
npm run audit:daily
```

---

## Documentation Links

- **Marketing API Docs:** https://developers.facebook.com/docs/marketing-api/
- **Insights API:** https://developers.facebook.com/docs/marketing-api/insights
- **Campaign Management:** https://developers.facebook.com/docs/marketing-api/campaign-management
- **Error Codes:** https://developers.facebook.com/docs/graph-api/using-graph-api/error-handling
- **Rate Limits:** https://developers.facebook.com/docs/graph-api/overview/rate-limiting

---

**Last Updated:** 2026-02-19  
**Integration Status:** ✅ Active (4 accounts connected)
