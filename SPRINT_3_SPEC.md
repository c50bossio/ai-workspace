# Sprint 3: Dashboard Intelligence Suite

**Kicked off:** Feb 21, 2026 @ 12:50 AM  
**Approved by:** Bossio  
**Method:** 4-Layer System (Atlas → Tyler → Atlas Review → Live Verify)

---

## 🎯 Deliverables (4 Features in Parallel)

### Feature 1: Inventory Alerts 📦
**Priority:** P0  
**Est:** 3-4 hours

**What it does:**
- Monitors Shopify inventory levels
- Calculates days until stockout based on sales velocity
- Alerts when products hit critical levels

**Thresholds:**
- 🔴 CRITICAL: < 3 days of stock
- 🟡 WARNING: < 7 days of stock
- 🟢 HEALTHY: > 7 days of stock

**UI:**
- Alert card on brand detail page
- Badge count on enterprise dashboard
- SMS alert for critical stockouts (optional)

**Files to create:**
- `src/lib/services/inventory-monitor.ts`
- `src/components/dashboard/InventoryAlertsCard.tsx`
- `src/app/api/inventory/alerts/route.ts`

**Data flow:**
```
Shopify API → Get inventory levels
Shopify API → Get sales velocity (last 7 days)
Calculate: inventory / (sales_per_day) = days_remaining
If days_remaining < threshold → Generate alert
```

---

### Feature 2: Product Performance Dashboard 📊
**Priority:** P0  
**Est:** 3-4 hours

**What it does:**
- Shows top 10 products by revenue, units, margin
- Trend indicators (rising stars vs declining)
- Week-over-week comparison

**UI:**
- New tab on brand detail page: "Products"
- Sortable table with sparkline charts
- Filter by date range (7d, 30d, 90d)

**Files to create:**
- `src/components/dashboard/ProductPerformanceTable.tsx`
- `src/app/api/products/performance/route.ts`
- Add "Products" tab to brand detail page

**Columns:**
| Product | Revenue | Units | AOV | Trend | WoW Change |
|---------|---------|-------|-----|-------|------------|

---

### Feature 3: Revenue Forecasting 📈
**Priority:** P1  
**Est:** 4-5 hours

**What it does:**
- Predicts next 7/14/30 day revenue
- Uses linear regression on historical data
- Shows confidence intervals

**UI:**
- Forecast card on brand detail page
- Line chart with actual + projected
- "On track" / "Behind" / "Ahead" indicator vs goals

**Files to create:**
- `src/lib/services/forecasting.ts`
- `src/components/dashboard/ForecastCard.tsx`
- `src/app/api/forecast/route.ts`

**Algorithm:**
```typescript
// Simple linear regression on last 30 days
// y = mx + b where x = day number, y = revenue
// Project forward to get forecast
```

---

### Feature 4: Portfolio AI Insights 🧠
**Priority:** P1  
**Est:** 2-3 hours

**What it does:**
- AI analysis across ALL brands (not just individual)
- Shows on enterprise dashboard (home page)
- Highlights winners, losers, opportunities

**UI:**
- Large insight card at top of enterprise dashboard
- 3-4 bullet points covering portfolio health
- Refresh button

**Files to create:**
- `src/lib/services/portfolio-insights.ts`
- `src/components/dashboard/PortfolioInsightsCard.tsx`
- Update enterprise dashboard page

**Prompt template:**
```
Analyze this portfolio of 6 brands:
- Tomb45: $X revenue, Y% change, Z ROAS
- FFC: ...
- Headlines: ...
[etc]

Provide 3-4 insights about:
1. Overall portfolio health
2. Top performer and why
3. Brand needing attention
4. One actionable recommendation
```

---

## 📋 Acceptance Criteria (All Features)

- [ ] TypeScript compiles (0 errors)
- [ ] Tested in live browser (not just DOM)
- [ ] Loading states implemented
- [ ] Error handling (graceful fallbacks)
- [ ] Mobile responsive
- [ ] Dark mode compatible

---

## 🔄 Workflow

1. **Atlas** specs features (this doc) ✅
2. **Tyler** implements (parallel where possible)
3. **Atlas** code reviews each PR
4. **Atlas** verifies in live browser
5. **Merge** only after verification passes

---

## 📊 Progress Tracker

| Feature | Status | PR | Reviewed | Verified |
|---------|--------|----|---------|---------| 
| Inventory Alerts | 🔄 In Progress | - | - | - |
| Product Performance | 🔄 In Progress | - | - | - |
| Forecasting | 🔄 In Progress | - | - | - |
| Portfolio Insights | 🔄 In Progress | - | - | - |

---

**Let's ship it! 🚀**
