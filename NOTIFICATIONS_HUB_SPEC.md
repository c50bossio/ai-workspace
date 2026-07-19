# Smart Notifications Hub

**Priority:** Next after Sprint 3 fixes  
**Est:** 4-5 hours  
**Approved by:** Bossio (Feb 21, 2026)

---

## Overview

Unified notification center that aggregates all alerts (ads, inventory, social, forecasts) into one manageable inbox with actions.

## Problem

Currently alerts are:
- Scattered across SMS, Discord, email
- No central place to see all alerts
- No bulk actions (dismiss all, snooze category)
- Alert fatigue from too many channels

## Solution

**One inbox to rule them all:**
- All alerts in one place on dashboard
- Snooze, dismiss, bulk actions
- Priority levels (critical → info)
- Filter by type, brand, date

---

## Features

### 1. Unified Inbox

**Location:** `/notifications` page + dropdown in header

**UI:**
```
┌─────────────────────────────────────────────────────────────┐
│ 🔔 Notifications                        Mark all read │ ⚙️ │
├─────────────────────────────────────────────────────────────┤
│ Filters: [All ▼] [All Brands ▼] [All Types ▼] [Clear]      │
├─────────────────────────────────────────────────────────────┤
│ 🔴 CRITICAL • 5 min ago                                     │
│ Tomb45 "Blade Guard" — 2 days until stockout                │
│ [View Product] [Snooze 24h] [Dismiss]                       │
├─────────────────────────────────────────────────────────────┤
│ 🟡 WARNING • 1 hour ago                                     │
│ FFC ROAS dropped to 2.1x (below 2.5x threshold)             │
│ [View Campaign] [Snooze 24h] [Dismiss]                      │
├─────────────────────────────────────────────────────────────┤
│ 🔵 INFO • 3 hours ago                                       │
│ Tomb45 Instagram post hit 1,000 likes                       │
│ [View Post] [Dismiss]                                       │
└─────────────────────────────────────────────────────────────┘
```

### 2. Header Bell Icon

**Location:** Top-right header (all pages)

**Behavior:**
- Badge shows unread count
- Dropdown shows 5 most recent
- "View all" links to full inbox
- Clicking notification marks as read

### 3. Notification Categories

| Category | Sources | Priority |
|----------|---------|----------|
| **Ad Alerts** | ROAS drops, budget overruns, pause recommendations | Critical/Warning |
| **Inventory** | Low stock, stockout predictions | Critical/Warning |
| **Social** | Viral posts, engagement spikes | Info |
| **Forecasts** | Revenue predictions, trend changes | Info |
| **System** | Deployment success, cron failures | Info/Warning |

### 4. Actions

- **Snooze:** 1h, 4h, 24h, 7d options
- **Dismiss:** Mark as handled
- **Bulk Actions:** Select multiple → dismiss/snooze all
- **Deep Link:** Click to go to relevant page (campaign, product, post)

### 5. Notification Preferences

**Location:** `/settings/notifications`

**Options per category:**
- ✅ Show in dashboard
- ✅ Send SMS (critical only)
- ✅ Send Discord
- ✅ Send email digest

---

## Database Schema

```prisma
model Notification {
  id          String   @id @default(cuid())
  userId      String   // Who should see this
  brandId     String?  // Which brand (null = system-wide)
  category    String   // ad_alert, inventory, social, forecast, system
  priority    String   // critical, warning, info
  title       String
  message     String   @db.Text
  deepLink    String?  // URL to navigate to
  read        Boolean  @default(false)
  snoozedUntil DateTime?
  dismissedAt DateTime?
  createdAt   DateTime @default(now())
  
  user        User     @relation(fields: [userId], references: [id])
  brand       Brand?   @relation(fields: [brandId], references: [id])
  
  @@index([userId, read, createdAt])
  @@index([brandId])
}
```

---

## API Endpoints

### GET /api/notifications
List notifications with filters

**Query params:**
- `read`: true/false/all
- `brand`: slug
- `category`: ad_alert/inventory/social/forecast/system
- `priority`: critical/warning/info
- `limit`: number (default 20)
- `cursor`: pagination

### PATCH /api/notifications/:id
Update notification (read, snooze, dismiss)

### POST /api/notifications/bulk
Bulk actions (dismiss all, snooze all matching filter)

---

## Files to Create

1. `src/app/(dashboard)/notifications/page.tsx` — Full inbox
2. `src/components/notifications/NotificationBell.tsx` — Header dropdown
3. `src/components/notifications/NotificationCard.tsx` — Single notification
4. `src/components/notifications/NotificationFilters.tsx` — Filter bar
5. `src/app/api/notifications/route.ts` — List + bulk actions
6. `src/app/api/notifications/[id]/route.ts` — Single notification actions
7. `src/lib/services/notification-service.ts` — Create/manage notifications
8. Update `prisma/schema.prisma` — Add Notification model

---

## Integration Points

When these events happen, create notification:
1. **Ad alert triggered** → notification
2. **Inventory alert** → notification  
3. **Instagram viral post** → notification
4. **Forecast significant change** → notification
5. **Cron job failure** → notification

---

## Acceptance Criteria

- [ ] Notifications page shows all alerts
- [ ] Header bell shows unread count
- [ ] Filter by category, brand, priority works
- [ ] Snooze hides notification until time passes
- [ ] Dismiss marks as handled
- [ ] Bulk actions work
- [ ] Deep links navigate correctly
- [ ] TypeScript compiles (0 errors)
- [ ] Mobile responsive

---

**Ready when Sprint 3 bugs are fixed.**
