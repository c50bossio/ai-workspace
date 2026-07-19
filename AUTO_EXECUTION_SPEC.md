# Auto-Execution System Spec

**Goal:** Turn dashboard from reporting tool → automation system that acts on recommendations.

## Current State
- ✅ MetaAdManager with pause/scale methods
- ✅ GoogleAdManager with pause method  
- ✅ Execution tracking in DB (`AdExecution` model)
- ✅ Cron job detecting losers/winners
- ⚠️ DRY_RUN=true (no actual changes yet)

## Phase 1: One-Click Execution (UI)

### 1.1 Recommendation Cards with Execute Button
Location: `src/components/recommendations/RecommendationCard.tsx`

```tsx
// Each recommendation card needs:
- "Execute" button (primary action)
- "Dismiss" button (mark as ignored)
- Preview of what will happen:
  - "Will pause campaign X (currently spending $Y/day)"
  - "Will increase budget by 20% ($X → $Y)"
```

### 1.2 Execution Confirmation Modal
Before executing, show:
- Campaign name
- Current status/budget
- Proposed change
- Expected impact
- "Confirm" / "Cancel" buttons

### 1.3 API Endpoint
`POST /api/ads/execute`
```typescript
interface ExecuteRequest {
  recommendationId: string;
  action: 'PAUSE' | 'SCALE' | 'REDUCE';
  campaignId: string;
  platform: 'meta' | 'google';
  brandSlug: string;
  // For scale/reduce:
  currentBudget?: number;
  newBudget?: number;
}
```

## Phase 2: Auto-Execution Mode

### 2.1 Brand-Level Toggle
In brand settings, add:
- `autoManage: boolean` (default: false)
- `autoScale: boolean` (default: false) 
- Headlines: autoScale ALWAYS false

### 2.2 Enable in Cron
Set env var: `AD_AUTO_EXECUTION_DRY_RUN=false`

Safety rules (already implemented):
- $15/day budget floor
- 72h cooldown between changes
- Max 20% budget change per execution
- Never pause campaigns spending >$500/day without confirmation

### 2.3 Execution Dashboard Enhancement
`/brands/[slug]/executions` should show:
- Pending executions (awaiting confirmation)
- Recent executions (last 7 days)
- Execution success/failure status
- Rollback button for recent changes

## Phase 3: Smart Automation

### 3.1 Rules Engine
Define rules per brand:
```typescript
{
  brand: 'tomb45',
  rules: [
    { condition: 'roas_7d < 2.5 && roas_30d < 2.5', action: 'PAUSE' },
    { condition: 'roas_7d >= 3.5', action: 'SCALE_20' },
    { condition: 'roas_7d < 2.5 && roas_30d >= 3.0', action: 'REDUCE_20' },
  ]
}
```

### 3.2 Notification on Execution
When auto-execution happens:
- Log to database
- Send SMS alert with details
- Show in-app notification

## Implementation Order

1. **Today:** Add Execute buttons to recommendation cards
2. **Today:** Create `/api/ads/execute` endpoint
3. **Today:** Add confirmation modal
4. **Tomorrow:** Enable auto-execution for one brand (test with Tomb45)
5. **Tomorrow:** Add rollback capability

## Files to Modify

- `src/components/recommendations/RecommendationCard.tsx` (new)
- `src/components/recommendations/ExecuteModal.tsx` (new)
- `src/app/api/ads/execute/route.ts` (new)
- `src/app/(dashboard)/brands/[slug]/executions/page.tsx` (enhance)
- `src/lib/ad-automation/execution-service.ts` (new - wraps managers)
- `.env` on Vercel: `AD_AUTO_EXECUTION_DRY_RUN=false`

## Safety Checklist

- [ ] Budget floor enforced ($15 min)
- [ ] Cooldown enforced (72h between changes)
- [ ] Max change enforced (20% per execution)
- [ ] Headlines never auto-scales
- [ ] All executions logged to database
- [ ] SMS sent on every auto-execution
- [ ] Rollback available for 7 days
