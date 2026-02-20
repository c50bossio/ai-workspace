---
name: ads-manager
description: |
  Automated ad campaign management for Meta Ads and Google Ads across multiple brands.
  Use this skill for: ROAS analysis, campaign performance monitoring, pause/scale/reduce decisions,
  budget optimization, creative fatigue detection, and automated alerts. Wraps the Bossio Solution
  Dashboard ad automation system with strict profitability rules (3.0x ROAS minimum).
  
  **When to use:**
  - Analyzing campaign ROAS and making budget decisions
  - Checking if campaigns should be paused, scaled, or reduced
  - Monitoring frequency and creative fatigue
  - Sending performance alerts (SMS via Twilio)
  - Auditing ad account performance
  - Executing approved budget changes via Meta/Google APIs
  
  **Core philosophy:** Every campaign has 7 days to hit 3.0x ROAS. Period.
tools:
  - read: Access campaign logs and brand configs
  - exec: Run ad automation scripts, check ROAS, execute budget changes
  - message: Send SMS alerts via Twilio (critical issues only)
related:
  - ~/clawd/projects/bossio-solution-dashboard/ (main codebase)
  - references/roas-rules.md (decision matrix)
  - references/meta-api.md (Meta Ads integration)
  - references/google-api.md (Google Ads integration)
  - references/brands.md (account IDs, credentials)
---

# Ads Manager Skill

**Location:** `~/clawd/projects/bossio-solution-dashboard/`

## Quick Start

### Check Campaign Performance
```bash
cd ~/clawd/projects/bossio-solution-dashboard
npx tsx scripts/check-campaign-roas.ts <brand> <campaignId>
```

### View Action History
```bash
cat logs/campaign-changes.jsonl | grep '<campaignId>' | tail -5
```

### Send Test Alert
```bash
npx tsx test-sms-alert.ts
```

## Core Rules (Summary)

**ROAS Thresholds:**
- **<2.5x** = PAUSE (losing money, 7+ days)
- **2.5-3.0x** = REDUCE -20% (break-even zone)
- **3.0-3.5x** = STABLE (profitable, monitor)
- **≥3.5x** = SCALE +20% (proven winner)

**Safety Rules:**
- 72-hour cooldown between ANY budget changes
- $15/day minimum budget floor (pause if below)
- Maximum 2 consecutive reductions, then pause
- Use 7-day rolling averages (not daily variance)
- Frequency >4.5 + declining ROAS = creative refresh (not budget cut)

**Before ANY Action:**
1. Check 7-day average ROAS (not individual days)
2. Verify 72-hour cooldown has elapsed
3. Validate data quality (sufficient conversions)
4. Check frequency for creative fatigue
5. Review macro trends (30-day context)

**Special Brand Rules:**
- **Headlines:** NEVER auto-scale (manual approval only)
- **Tomb45:** Max $3,000/day total spend
- **FFC:** Max $2,000/day total spend (inventory constraints)

## Decision Checklist

Before recommending ANY ad change, validate:

1. ✅ Using 7-day rolling average (not daily)?
2. ✅ Cooldown respected (72h since last change)?
3. ✅ Frequency checked (>4.5 = creative issue)?
4. ✅ Budget change ≤20% (or pause/restore)?
5. ✅ Above $15/day floor (or pause instead)?
6. ✅ Brand-specific rules honored (Headlines, etc.)?

**See:** `references/roas-rules.md` for full decision matrix

## Common Tasks

### Daily Audit (6 AM)
```bash
cd ~/clawd/projects/bossio-solution-dashboard
npm run audit:daily
```

Generates recommendations for:
- Campaigns to pause (<2.5x ROAS for 7+ days)
- Campaigns to scale (≥3.5x ROAS, 72h cooldown met)
- Campaigns to reduce (2.5-3.0x ROAS, 30-day context)
- Creative refresh needs (frequency >4.5)

### Execute Approved Changes
```bash
npx tsx scripts/execute-campaign-change.ts \
  --brand tomb45 \
  --campaign 123456789 \
  --action pause \
  --reason "7d ROAS 2.02x for 7 days (below 2.5x)"
```

### Check Cooldown
```bash
npx tsx scripts/log-campaign-change.ts check <campaignId>
```

## Alert Types (SMS via Twilio)

**Critical Alerts (immediate SMS):**
- Budget overspend >110% before 6 PM
- ROAS drop >30% vs yesterday
- Campaign approaching 7-day deadline (<2.5x on day 6)

**Info Alerts (dashboard/Telegram only):**
- Frequency >4.5 (creative fatigue warning)
- New campaign launched
- Campaign recovered to ≥3.0x ROAS

**Configuration:** `~/clawd/projects/bossio-solution-dashboard/.env`
- Twilio Account SID: `$TWILIO_ACCOUNT_SID`
- Twilio Phone: `+18135483884`
- Recipient: `+13525568981` (Bossio)

## Log Files

**Campaign Action Log:**
- **Location:** `logs/campaign-changes.jsonl`
- **Format:** JSONL (one action per line)
- **Required fields:** `timestamp`, `campaignId`, `brand`, `platform`, `action`, `oldBudget`, `newBudget`, `reason`, `executedBy`
- **Purpose:** Track all budget changes, validate 72h cooldown

**Example Log Entry:**
```json
{
  "timestamp": "2026-02-19T06:00:00Z",
  "campaignId": "123456789",
  "campaignName": "Testing Campaign",
  "brand": "Tomb45",
  "platform": "meta",
  "action": "reduce",
  "oldBudget": 2500,
  "newBudget": 2000,
  "reason": "7d ROAS 2.02x (below 3.0x)",
  "executedBy": "atlas"
}
```

## API Access

**Meta Ads:**
- Token stored in `.env` (system user token)
- See `references/meta-api.md` for endpoint details

**Google Ads:**
- OAuth refresh token in `.env`
- MCC Account: 735-272-6472
- See `references/google-api.md` for SDK usage

## Budget Restoration

**When campaign recovers (7-day ROAS ≥3.0x for 3+ days):**

1. Increase +20% from CURRENT budget (not original)
2. Monitor for 7 days
3. If continues strong, increase +20% again
4. Gradually return to original budget over 2-3 weeks

**Example:**
```
Original: $100/day
Reduced to: $80/day (-20%)
--- Recovers to 3.2x ROAS ---
Restore to: $96/day (+20% from $80)
--- Still strong 7 days later ---
Restore to: $100/day (back to original)
```

**Don't:** Jump immediately back to original budget (shocks algorithm)

## Creative Refresh Workflow

**Trigger:** Frequency >4.5 + ROAS declining

**Process:**
1. Identify 3-5 new creative concepts (different hooks/visuals)
2. Launch as NEW campaigns ($50/day each)
3. Test for 7 days
4. Keep winners (≥3.0x ROAS)
5. Pause losers (<2.5x ROAS)
6. Pause old fatigued campaign once replacement scales

**Why new campaigns?** Fresh algorithm start (don't refresh inside existing campaign - loses learnings)

## Error Handling

**Common Issues:**

1. **API rate limit hit:**
   - Wait 1 hour
   - Implement exponential backoff
   - Cache more aggressively

2. **Insufficient data (<7 days):**
   - Don't make decisions
   - Flag for review when data sufficient

3. **Cooldown violation:**
   - Cancel action
   - Log warning
   - Wait until 72h elapsed

4. **Budget below $15 floor:**
   - Pause instead of reducing
   - Never operate below minimum

## References

- **[ROAS Rules & Decision Matrix](references/roas-rules.md)** - Complete decision logic
- **[Meta Ads API](references/meta-api.md)** - Endpoints, authentication, metrics
- **[Google Ads API](references/google-api.md)** - SDK usage, query syntax
- **[Brand Configs](references/brands.md)** - Account IDs, spend limits, special rules

## Examples

### Example 1: Scale Winner
```typescript
// Campaign: DYNAMIC CATALOG
// 7-day ROAS: 3.82x
// Current budget: $30/day
// Last change: 7 days ago ✅

Recommendation: SCALE
New budget: $36/day (+20%)
Reason: "7d ROAS 3.82x (≥3.5x threshold), cooldown met"
```

### Example 2: Pause Loser
```typescript
// Campaign: Testing Campaign
// 7-day ROAS: 2.02x for 7 days
// 30-day ROAS: 2.15x
// Current budget: $2,500/day

Recommendation: PAUSE
New budget: $0/day
Reason: "7d ROAS <2.5x for 7+ days, 30d also weak"
```

### Example 3: Creative Refresh (NOT Budget Cut)
```typescript
// Campaign: Old Winner
// 7-day ROAS: 2.8x (declining from 3.5x)
// Frequency: 5.2 (too high!)
// Current budget: $200/day

Recommendation: CREATIVE REFRESH
Action: Launch 3-5 new campaigns at $50/day each
Reason: "High frequency + declining ROAS = audience fatigue"
Don't: Reduce budget (won't fix fatigue)
```

## Migration from Old System

If coming from manual ad management:

1. **Enable dry-run mode** in settings
2. **Test for 1 week** (logs actions but doesn't execute)
3. **Review execution logs** daily
4. **Disable dry-run** when confident
5. **Monitor first 3 days** closely

## Troubleshooting

**"Campaign was paused but I didn't approve it"**
- Check `logs/campaign-changes.jsonl` for reason
- Look for `executedBy` field
- Verify it was automated (not manual)

**"SMS alert didn't send"**
- Check Twilio balance (low credits?)
- Verify recipient number in `.env`
- Check `test-sms-alert.ts` script

**"ROAS calculation seems wrong"**
- Verify conversion tracking setup
- Check attribution window (7-day click, 1-day view)
- Compare to Meta Ads Manager directly

---

**Last Updated:** 2026-02-19  
**Maintainer:** Atlas (COO)  
**Support:** #bossio-dashboard
