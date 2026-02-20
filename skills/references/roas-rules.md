# ROAS Rules & Decision Matrix

**Complete decision logic for ad automation system**

## ROAS Tiers

### <2.5x = LOSING MONEY 🔴

**Reality:**
- Revenue: $100
- Ad Spend: $40+ (2.5x or worse)
- COGS: $35-40
- Overhead: $10-15
- **Profit: $0 or NEGATIVE** ❌

**Action:** PAUSE after 7 days
- Gives algorithm time to stabilize (7-day test)
- But won't let losses continue indefinitely
- Exception: If 30-day average is strong (≥3.0x), reduce -20% instead

### 2.5-3.0x = BREAK-EVEN 🟡

**Reality:**
- Revenue: $100
- Ad Spend: $33-40
- COGS + Overhead: $50
- **Profit: $10-17** (minimal)

**Action:** REDUCE -20% OR MONITOR
- If 30-day average ≥3.0x: Reduce -20% (temporary dip)
- If 30-day average <2.5x: PAUSE (sustained poor performance)
- Monitor closely for improvement

### 3.0-3.5x = PROFITABLE 🟢

**Reality:**
- Revenue: $100
- Ad Spend: $28-33
- COGS + Overhead: $50
- **Profit: $17-22** ✅

**Action:** STABLE (monitor, don't change unless trending down)
- Campaign is profitable
- Not quite "scale aggressively" level yet
- Watch for trends (improving vs declining)

### ≥3.5x = WINNING 💎

**Reality:**
- Revenue: $100
- Ad Spend: <$28.57
- COGS + Overhead: $50
- **Profit: >$21.43** 🚀

**Action:** SCALE +20%
- Well above profit threshold
- Proven winner
- Safe to invest more
- Check cooldown (72h since last change)

---

## Complete Decision Matrix

| 7-Day ROAS | 30-Day ROAS | Days Active | Frequency | Action | Budget Change | Reason |
|------------|-------------|-------------|-----------|--------|---------------|---------|
| **≥3.5x** | Any | 3+ | <4.5 | **SCALE** | +20% | Well above target, scale aggressively |
| **≥3.5x** | Any | 3+ | ≥4.5 | **CREATIVE REFRESH** | Launch new | High performance but audience fatigued |
| **3.0-3.5x** | Any | 3+ | <4.5 | **STABLE** | No change | Profitable, monitor trends |
| **3.0-3.5x** | Any | 3+ | ≥4.5 | **CREATIVE REFRESH** | Launch new | Frequency too high, refresh proactively |
| **2.5-3.0x** | ≥3.0x | 7+ | Any | **REDUCE** | -20% | Short-term dip, long-term OK |
| **2.5-3.0x** | <2.5x | 7+ | Any | **PAUSE** | 0 | Sustained break-even, not worth it |
| **<2.5x** | Any | 7+ | Any | **PAUSE** | 0 | Losing money, stop bleeding |
| **<2.5x** | Any | 3-6 | Any | **FLAG** | 0 | Monitor closely, may pause soon |

---

## 5-Point Validation (Before ANY Action)

### 1. Time Check
- [ ] Has campaign run for ≥7 days?
- [ ] Is last budget change ≥72 hours ago?

**If NO:** Don't act yet, let algorithm stabilize

### 2. Data Quality
- [ ] Are we using 7-day rolling average (not daily)?
- [ ] Do we have sufficient conversions (≥10 for statistical significance)?
- [ ] Is tracking working (no data gaps)?

**If NO:** Get more data before deciding

### 3. Significance Check
- [ ] Is this a trend or variance?
- [ ] What's the daily ROAS range?
- [ ] Is current performance outside normal variance?

**Example:** Campaign ranges 1.5x-4.0x daily (avg 2.8x)
- One day at 1.5x = variance, not a problem
- 7 days straight <2.5x = trend, action needed

### 4. Cooldown Validation
```bash
# Check last change
cat logs/campaign-changes.jsonl | grep "campaignId\":\"$CAMPAIGN_ID" | tail -1

# Verify timestamp >72h ago
```

**If cooldown not met:** Cancel action, wait until 72h elapsed

### 5. Macro Trends
- [ ] What's 30-day average ROAS?
- [ ] Is brand having issues across all campaigns?
- [ ] Are there external factors (seasonality, competitors)?

**Context matters:** Don't react to temporary market conditions

---

## Safety Constraints

### 72-Hour Cooldown

**Rule:** Minimum 72 hours between ANY budget changes

**Why:** Algorithm needs time to adapt
- Meta/Google learning phases last 3-7 days
- Budget changes reset learning progress
- Changing too frequently = chaos

**Tracking:** `logs/campaign-changes.jsonl`
- Check timestamp of last action
- Calculate hours elapsed
- Block action if <72h

**Exceptions:** NONE (even emergency pauses should be rare)

### Maximum 2 Reductions

**Rule:** After 2 consecutive reductions, PAUSE instead of 3rd

**Example:**
```
Day 0: $100/day
Day 7: Reduce to $80 (-20%) - 1st reduction
Day 14: Reduce to $64 (-20%) - 2nd reduction
Day 21: PAUSE (don't reduce to $51.20) - would be 3rd
```

**Why:**
- 2 reductions = 36% total budget cut
- If still not profitable after 36% cut, campaign is failing
- Kill it rather than slowly bleeding

### $15/Day Minimum Budget Floor

**Rule:** Never reduce below $15/day

**Why:**
- Meta/Google algorithms need ≥50 events/day to optimize
- Below $15/day = typically <50 conversions/day
- Data quality degrades significantly
- Effectively "paused" but still wasting money

**Decision:**
- If reduction would drop below $15 → PAUSE instead
- Exception: New campaign testing ($10-15/day for 3-5 days OK, then scale or kill)

**Example:**
```
Campaign at $20/day, underperforming
-20% = $16/day ✅ (above $15 floor)

Campaign at $16/day, still underperforming
-20% = $12.80/day ❌ (below $15 floor)
Action: PAUSE (don't drop below floor)
```

### Account-Level Budget Caps

**Rule:** Total daily spend per brand should not exceed capacity

| Brand | Max Daily Spend | Reason |
|-------|-----------------|--------|
| Tomb45 | $3,000/day | Current capacity (~$2,800/day tested) |
| FFC | $2,000/day | Inventory constraints (physical products) |
| Headlines | $500/day | Local service, limited market demand |
| Pie | $1,000/day | Service business, lead capacity |
| SSC | TBD | Not yet established (start conservative) |

**What if we want to scale but at cap?**
1. Pause worst performer
2. Reallocate budget to winner
3. Net spend stays same, ROAS improves

**Example (Tomb45 at $2,800/day cap):**
```
Testing Campaign: $2,500/day @ 2.02x ROAS (weak)
DYNAMIC CATALOG: $30/day @ 3.82x ROAS (strong)

Want to scale DYNAMIC but at cap:
→ Reduce Testing $2,500 → $2,000 (-$500 freed)
→ Scale DYNAMIC $30 → $100 (+$70)
→ Reallocate remaining $430 to new tests
→ Total: $2,800/day (same), better ROAS mix
```

---

## Frequency Integration

**Rule:** Check frequency BEFORE making budget decisions

### Frequency Thresholds

| Frequency | Meaning | Action Needed |
|-----------|---------|---------------|
| <2.0 | Very fresh | Safe to scale, no fatigue |
| 2.0-3.0 | Healthy | Monitor, normal range |
| 3.0-3.5 | Getting tired | Creative refresh within 1-2 weeks |
| 3.5-4.5 | Tired | Creative refresh NOW |
| >4.5 | Exhausted | Pause + refresh immediately |

### Decision Matrix with Frequency

**Scenario 1: High ROAS + High Frequency**
- ROAS: 3.8x (great!)
- Frequency: 5.2 (too high!)
- **Action:** CREATIVE REFRESH (not budget change)
- **Why:** Audience is fatigued, not algorithm issue

**Scenario 2: Declining ROAS + Normal Frequency**
- ROAS: 2.8x (below 3.0x)
- Frequency: 2.5 (normal)
- **Action:** REDUCE -20%
- **Why:** Algorithm or audience quality issue, not creative fatigue

**Scenario 3: Declining ROAS + High Frequency**
- ROAS: 2.8x (below 3.0x)
- Frequency: 4.8 (high)
- **Action:** CREATIVE REFRESH (not budget cut)
- **Why:** Creative fatigue is the root cause

**Key Rule:**
- Frequency <3.0 → Budget changes OK
- Frequency 3.0-4.5 + declining ROAS → Consider creative refresh
- Frequency >4.5 + any ROAS → Creative refresh NOW

---

## Budget Restoration Process

**When reduced campaign recovers (7d ROAS ≥3.0x for 3+ days):**

### Step-by-Step

1. **Verify recovery:**
   - 7-day ROAS ≥3.0x
   - 3+ consecutive days at this level
   - No other red flags (frequency, CPA, etc.)

2. **Increase +20% from CURRENT budget:**
   ```
   Current (reduced): $80/day
   Restore to: $96/day (+20% from $80)
   NOT: $100/day (original)
   ```

3. **Monitor for 7 days:**
   - If ROAS stays ≥3.0x → continue
   - If ROAS drops <3.0x → hold at this level

4. **Gradually return to original:**
   ```
   Week 1: $80 → $96 (+20%)
   Week 2: $96 → $100 (+4%, back to original) ✅
   ```

### Why Gradual?

- Prevents shocking the algorithm
- Lets it stabilize at each level
- Reduces risk of performance drop
- Confirms recovery is real, not variance

### Example Timeline

```
Day 0: $100/day @ 2.8x ROAS
Day 7: Reduce to $80 (-20%) due to dip
Day 14: Still at $80/day @ 2.9x ROAS (improving)
Day 21: Recovers to $80/day @ 3.2x ROAS ✅
Day 22: Restore to $96 (+20% from $80)
Day 29: Still strong @ 3.1x ROAS
Day 30: Restore to $100 (original) ✅
```

**Don't:** Jump immediately back to original budget (risks performance drop)

---

## Creative Refresh Triggers

### When to Refresh Creative (NOT Budget)

**Trigger 1: High Frequency + Declining ROAS**
- Frequency >4.5
- ROAS declining 3+ days in a row
- **Root Cause:** Audience fatigue
- **Solution:** New creative, not budget cut

**Trigger 2: Declining CTR (Click-Through Rate)**
- CTR dropping 3+ days straight
- ROAS may still be OK (for now)
- **Root Cause:** Creative is boring
- **Solution:** Fresh hooks, new visuals

**Trigger 3: Creative Age (Proactive)**
- Creative >3 weeks old
- Even if performing well
- **Root Cause:** Fatigue coming soon
- **Solution:** Proactive refresh before performance drops

### Creative Refresh Process

1. **Identify 3-5 new concepts:**
   - Different hooks (opening line/question)
   - Different visuals (new footage, colors, layouts)
   - Different offers (discount, bundle, urgency)

2. **Launch as NEW campaigns:**
   - $50/day each (5 campaigns = $250/day total)
   - Fresh algorithm start
   - Don't refresh inside existing campaign

3. **Test for 7 days:**
   - Apply same 7-day profitability rule
   - Winners (≥3.0x ROAS) → Keep
   - Losers (<2.5x ROAS) → Pause

4. **Scale winners:**
   - Once winner hits ≥3.5x ROAS → Scale +20%
   - Repeat until reaches original campaign budget

5. **Pause old campaign:**
   - Once replacement reaches same budget level
   - Only then pause fatigued campaign
   - Don't pause before replacement is scaled

### Why New Campaigns?

**Don't:** Edit creative inside existing campaign
- Resets algorithm learning
- Loses all historical data
- May drop performance

**Do:** Launch new campaigns with new creative
- Fresh algorithm start
- Keeps old campaign running until replacement ready
- Can A/B test side-by-side

---

## Special Brand Rules

### Headlines: NO AUTO-SCALE

**Rule:** Never auto-scale Headlines campaigns (manual approval only)

**Why:**
- Local service business (10 barbershop locations)
- Limited market demand
- Over-scaling saturates market quickly
- Manual review ensures capacity can handle

**Action:**
- If Headlines campaign hits ≥3.5x ROAS → FLAG for manual review
- Send recommendation to Bossio
- Wait for approval before scaling
- Log as "recommended scale, awaiting approval"

### Tomb45: $3,000/Day Cap

**Rule:** Total Tomb45 daily spend should not exceed $3,000

**Current:** ~$2,800/day tested, working well

**When at cap:**
1. Prioritize highest ROAS campaigns
2. Pause/reduce lowest performers
3. Reallocate budget to winners
4. Net spend stays ≤$3,000

### FFC: $2,000/Day Cap (Inventory)

**Rule:** Total FFC daily spend limited to $2,000/day

**Why:** Physical inventory constraints
- Custom automotive parts
- Can't scale beyond inventory capacity
- Over-selling = backorders, unhappy customers

**When at cap:**
1. Don't scale unless inventory confirmed
2. Pause campaigns for out-of-stock products
3. Focus on in-stock, high-margin items

---

## Examples (Real Scenarios)

### Example 1: Scale Winner
```
Campaign: DYNAMIC CATALOG (Tomb45)
7-day ROAS: 3.82x ✅
30-day ROAS: 3.65x ✅
Frequency: 2.1 ✅
Current budget: $30/day
Last change: 8 days ago ✅

Validation:
✅ ROAS ≥3.5x (scale threshold)
✅ Cooldown met (72h+)
✅ Frequency OK (<4.5)
✅ Trend is positive

Recommendation: SCALE
New budget: $36/day (+20%)
Reason: "7d ROAS 3.82x (≥3.5x), cooldown met, frequency healthy"
```

### Example 2: Pause Loser
```
Campaign: Testing Campaign (Tomb45)
7-day ROAS: 2.02x ❌
30-day ROAS: 2.15x ❌
Days at <2.5x: 7 ❌
Current budget: $2,500/day
Last change: 10 days ago ✅

Validation:
❌ ROAS <2.5x (pause threshold)
❌ 30-day also weak (not temporary dip)
✅ 7+ days at this level (trend confirmed)
✅ Cooldown met

Recommendation: PAUSE
New budget: $0/day
Reason: "7d ROAS 2.02x for 7+ days, 30d ROAS also <2.5x (sustained loss)"
```

### Example 3: Reduce (Temporary Dip)
```
Campaign: Winner Campaign (FFC)
7-day ROAS: 2.8x ⚠️
30-day ROAS: 3.4x ✅
Frequency: 2.5 ✅
Current budget: $500/day
Last change: 5 days ago ❌

Validation:
⚠️ 7d ROAS <3.0x (below target)
✅ 30d ROAS strong (historically good)
✅ Frequency normal (not creative fatigue)
❌ Cooldown NOT met (only 5 days since last change)

Recommendation: WAIT
New budget: No change
Reason: "Temporary dip in historically strong campaign, cooldown not met. Monitor 2 more days."
```

### Example 4: Creative Refresh (NOT Budget)
```
Campaign: Old Winner (Tomb45)
7-day ROAS: 2.8x ⚠️
30-day ROAS: 3.5x ✅
Frequency: 5.2 ❌
Current budget: $200/day
Last change: 15 days ago ✅

Validation:
⚠️ ROAS declining (was 3.5x, now 2.8x)
❌ Frequency >4.5 (audience fatigued)
✅ Cooldown met
✅ Historically strong campaign

Recommendation: CREATIVE REFRESH
Action: Launch 5 new campaigns at $50/day each ($250 total)
Reason: "High frequency (5.2) + declining ROAS = audience fatigue. Budget cut won't fix. Need fresh creative."

Don't: Reduce budget to $160 (-20%) ❌
Do: Launch new creative tests ✅
```

---

## Quick Reference Table

| ROAS | Frequency | Action | Budget Change | Why |
|------|-----------|--------|---------------|-----|
| ≥3.5x | <4.5 | SCALE | +20% | Proven winner |
| ≥3.5x | ≥4.5 | REFRESH | Launch new | Fatigue coming |
| 3.0-3.5x | <4.5 | STABLE | No change | Profitable, monitor |
| 2.5-3.0x + 30d ≥3.0x | Any | REDUCE | -20% | Temp dip |
| 2.5-3.0x + 30d <2.5x | Any | PAUSE | 0 | Sustained weak |
| <2.5x | Any | PAUSE | 0 | Losing money |

**Remember:** Always use 7-day average + check cooldown + validate data quality!

---

**Last Updated:** 2026-02-19  
**Source:** AD_SYSTEM_RULES.md, AD_DECISION_CHECKLIST.md, ROAS_THRESHOLDS.md
