# Sprint 4: Parallel Build (3 Features)

**Kicked off:** Feb 21, 2026 @ 1:52 AM  
**Method:** 3 sub-agents building in parallel  
**Approved by:** Bossio

---

## Feature 1: Profit Calculator 💰

**Sub-agent:** Tyler (or spawn)

**What it does:**
- Calculate true profit per product
- Revenue - COGS - Ad Attribution - Shipping
- Show margin % and $ per unit

**UI:**
- New "Profitability" tab on brand pages
- Table: Product | Revenue | COGS | Ads | Shipping | Profit | Margin%
- Summary cards: Total Profit, Avg Margin, Top Profitable Products

**Data sources:**
- Revenue: Shopify orders
- COGS: Shopify product cost (metafield or variant cost)
- Ads: Meta/Google spend attributed to product (if available)
- Shipping: Shopify shipping costs

**Files:**
- `src/lib/services/profit-calculator.ts`
- `src/components/dashboard/ProfitabilityTab.tsx`
- `src/app/api/brands/[slug]/profitability/route.ts`

---

## Feature 2: Campaign Deep Dive 📊

**Sub-agent:** Spawn new

**What it does:**
- Individual campaign detail pages
- Full metrics: impressions, clicks, CTR, conversions, ROAS, frequency
- Creative breakdown (which ads are winning)
- Daily performance chart

**UI:**
- `/brands/[slug]/campaigns/[campaignId]` page
- Metrics grid at top
- Performance chart (last 7/14/30 days)
- Creatives table with thumbnails + metrics

**Data sources:**
- Meta Ads API: campaign insights, ad insights
- Google Ads API: campaign metrics

**Files:**
- `src/app/(dashboard)/brands/[slug]/campaigns/[id]/page.tsx`
- `src/lib/services/campaign-details.ts`
- `src/app/api/campaigns/[id]/route.ts`
- `src/components/campaigns/CampaignMetrics.tsx`
- `src/components/campaigns/CreativeBreakdown.tsx`

---

## Feature 3: Auto-Execution Dashboard 🔄

**Sub-agent:** Spawn new

**What it does:**
- Visual log of all automated ad actions
- Pending recommendations with approve/reject
- Execution history with before/after metrics
- Impact tracking (did the change help?)

**UI:**
- Enhanced `/brands/[slug]/executions` page
- Pending actions at top with action buttons
- History table: Date | Campaign | Action | Before | After | Impact

**Data sources:**
- CampaignExecution table (already exists)
- PendingAction table (may need to create)

**Files:**
- `src/app/(dashboard)/brands/[slug]/executions/page.tsx` (enhance)
- `src/lib/services/execution-tracker.ts`
- `src/app/api/executions/approve/route.ts`
- `src/components/executions/PendingActions.tsx`
- `src/components/executions/ExecutionHistory.tsx`

---

## Acceptance Criteria (All Features)

- [ ] TypeScript compiles (0 errors)
- [ ] Loading/error states
- [ ] Mobile responsive
- [ ] Dark mode compatible
- [ ] Verified in browser

---

**GO GO GO! 🚀**
