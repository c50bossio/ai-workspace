# AI Insights Feature Spec

**Priority:** High  
**Estimated Effort:** 1 day  
**Assigned:** Tyler  
**Approved by:** Bossio (Feb 21, 2026)

---

## Overview

Add AI-powered insights to the dashboard using Claude Sonnet. The AI analyzes brand metrics and explains what's driving performance in plain English.

## User Story

> As a business owner, I want to see AI-generated insights explaining my performance trends so I don't have to interpret raw numbers myself.

## Feature Requirements

### 1. AI Insights Card (Brand Detail Page)

**Location:** Brand detail page, above the metrics tabs

**UI:**
```
┌─────────────────────────────────────────────────────────┐
│ 🧠 AI Insights                            [Refresh ↻]  │
├─────────────────────────────────────────────────────────┤
│ 📈 Revenue surge driven by viral Instagram Reel        │
│ (Feb 18, 45K views) that sent organic traffic to the   │
│ store. Google Search maintained steady conversions     │
│ while Meta spend held flat — indicating strong organic │
│ lift this week.                                        │
│                                                        │
│ 💡 Recommendation: Consider boosting the viral Reel    │
│ with $50-100 to extend its reach while it's hot.       │
│                                                        │
│ ─────────────────────────────────────────────────────  │
│ Last updated: 2 hours ago                              │
└─────────────────────────────────────────────────────────┘
```

**Behavior:**
- Load cached insight on page load (don't call Claude every time)
- "Refresh" button triggers new Claude analysis
- Show loading spinner while generating
- Cache insights in database (valid for 4 hours)

### 2. Enterprise Dashboard Summary

**Location:** Top of enterprise dashboard (home page)

**UI:**
```
┌─────────────────────────────────────────────────────────┐
│ 🧠 Portfolio Insights                     [Refresh ↻]  │
├─────────────────────────────────────────────────────────┤
│ 📊 Overall: Portfolio revenue up 22% WoW. Tomb45 and   │
│ FFC driving growth, Headlines steady, SSC needs        │
│ attention (ROAS dropped to 2.1x).                      │
│                                                        │
│ 🔥 Winner: Tomb45 "Blade Guard" campaign (4.2x ROAS)   │
│ ⚠️ Watch: SSC Google Search (approaching 7-day limit)  │
└─────────────────────────────────────────────────────────┘
```

### 3. API Endpoint

**Endpoint:** `POST /api/ai/insights`

**Request:**
```json
{
  "brandSlug": "tomb45",  // or "all" for portfolio
  "forceRefresh": false
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "insight": "📈 Revenue surge driven by...",
    "recommendation": "💡 Consider boosting...",
    "generatedAt": "2026-02-21T05:27:00Z",
    "metrics": {
      "revenueChange": 28,
      "adSpendChange": 2,
      "roasAvg": 3.4
    }
  }
}
```

### 4. Database Schema

Add to `prisma/schema.prisma`:

```prisma
model AIInsight {
  id          String   @id @default(cuid())
  brandId     String?  // null = portfolio-wide insight
  brand       Brand?   @relation(fields: [brandId], references: [id])
  insight     String   @db.Text
  recommendation String? @db.Text
  metrics     Json?    // raw metrics used for generation
  generatedAt DateTime @default(now())
  expiresAt   DateTime // 4 hours from generation
  createdAt   DateTime @default(now())
  
  @@index([brandId, expiresAt])
}
```

### 5. Claude Integration

**File:** `src/lib/ai/insights-generator.ts`

**Prompt Template:**
```
You are a business analyst for an e-commerce/service business portfolio.

Analyze these metrics for {brandName} and provide:
1. A 2-3 sentence insight explaining the key performance drivers
2. One actionable recommendation

Metrics (last 7 days vs previous 7 days):
- Revenue: ${revenue} ({revenueChange}%)
- Ad Spend: ${adSpend} ({adSpendChange}%)
- ROAS: {roas}x (target: 3.0x)
- Orders: {orders} ({ordersChange}%)
- Instagram Engagement: {igEngagement} ({igChange}%)
- Top Campaign: {topCampaign} ({topCampaignRoas}x ROAS)
- Worst Campaign: {worstCampaign} ({worstCampaignRoas}x ROAS)

Be specific. Reference actual numbers. Use emoji sparingly (1-2 per insight).
Keep it concise — busy business owner reading on mobile.
```

**Model:** `claude-sonnet-4-5-20250514` (or latest Sonnet)

**API Key:** Add `ANTHROPIC_API_KEY` to `.env`

### 6. Caching Strategy

- **Cache duration:** 4 hours
- **Storage:** Database (`AIInsight` table)
- **Invalidation:** Manual refresh button, or automatic on significant metric change
- **Fallback:** If Claude API fails, show "Insights temporarily unavailable"

## Technical Implementation

### Files to Create/Modify

1. **Create:** `src/lib/ai/insights-generator.ts` — Claude API integration
2. **Create:** `src/components/ai/AIInsightsCard.tsx` — UI component
3. **Create:** `src/app/api/ai/insights/route.ts` — API endpoint
4. **Modify:** `prisma/schema.prisma` — Add AIInsight model
5. **Modify:** `src/app/(dashboard)/brands/[slug]/page.tsx` — Add card
6. **Modify:** `src/app/(dashboard)/page.tsx` — Add portfolio insights

### Environment Variables

Add to `.env`:
```
ANTHROPIC_API_KEY=sk-ant-...
```

## Acceptance Criteria

- [ ] AI Insights card displays on brand detail pages
- [ ] Portfolio insights display on enterprise dashboard
- [ ] Refresh button triggers new Claude analysis
- [ ] Insights are cached for 4 hours
- [ ] Loading state shown while generating
- [ ] Error state if Claude API fails
- [ ] TypeScript compiles with 0 errors
- [ ] Verified in live browser (not just DOM checks)

## Out of Scope (Future)

- Scheduled insight generation (cron job)
- Insight history/trending
- Custom insight prompts
- Voice readout of insights

---

**Ready for implementation.** Tag Tyler to start.
