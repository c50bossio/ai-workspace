# SSC Website Redesign Specification
**Version:** 1.0  
**Date:** Feb 20, 2026  
**Prepared by:** Atlas (COO)

---

## Executive Summary

This spec outlines a comprehensive redesign for Sunshine State Chrome and Parts (SSC) based on:
1. **2026 e-commerce UX best practices** (Baymard, Shopify, BigCommerce research)
2. **Competitor analysis** (Big Rig Chrome Shop, Raney's, 4 State Trucks)
3. **Conversion optimization** principles for large-catalog retail stores

**Goal:** Transform SSC from a basic Shopify store into a professional, easy-to-navigate parts retailer that competes with industry leaders.

---

## Part 1: Navigation Architecture

### Current Problem
SSC has basic navigation that doesn't help customers find products efficiently. For a retail store with many products, navigation is THE conversion lever.

### Recommended Structure

#### Primary Navigation (5-7 items max)

```
[LOGO] [Search Bar ─────────────────] [Phone] [Account] [Cart]

| Shop by Truck | Shop by Part | Brands | Deals | Resources |
```

#### Mega Menu: Shop by Truck
```
┌──────────────────────────────────────────────────────────────────┐
│ PETERBILT          │ KENWORTH           │ FREIGHTLINER          │
│ ─────────────────  │ ─────────────────  │ ─────────────────     │
│ Peterbilt 379      │ Kenworth W900      │ Freightliner Cascadia │
│ Peterbilt 386      │ Kenworth T680      │ Freightliner Classic  │
│ Peterbilt 388      │ Kenworth T660      │ Freightliner Columbia │
│ Peterbilt 389      │ Kenworth W800      │ Freightliner FLD      │
│ View All →         │ View All →         │ View All →            │
├──────────────────────────────────────────────────────────────────┤
│ VOLVO              │ INTERNATIONAL      │ MACK                  │
│ ─────────────────  │ ─────────────────  │ ─────────────────     │
│ Volvo VNL          │ International LT   │ Mack Anthem           │
│ Volvo 780          │ International      │ Mack Pinnacle         │
│ View All →         │ View All →         │ View All →            │
└──────────────────────────────────────────────────────────────────┘
```

#### Mega Menu: Shop by Part
```
┌──────────────────────────────────────────────────────────────────┐
│ EXTERIOR           │ INTERIOR           │ LIGHTING              │
│ ─────────────────  │ ─────────────────  │ ─────────────────     │
│ Bumpers            │ Door Panels        │ Headlights            │
│ Grilles            │ Dash Panels        │ Marker Lights         │
│ Visors             │ Headliners         │ Cab Lights            │
│ Fenders            │ Floor Mats         │ LED Strips            │
│ Hood Accessories   │ Seat Covers        │ Work Lights           │
├──────────────────────────────────────────────────────────────────┤
│ CHROME             │ AIR & EXHAUST      │ ACCESSORIES           │
│ ─────────────────  │ ─────────────────  │ ─────────────────     │
│ Window Chops       │ Air Ride Kits      │ Cleaning Products     │
│ Rear Panels        │ Exhaust Stacks     │ Polishing Supplies    │
│ Step Plates        │ Air Cleaners       │ Tools                 │
└──────────────────────────────────────────────────────────────────┘
```

### Mobile Navigation
- **Hamburger menu** with "Shop" label (don't hide categories behind mystery icon)
- **Thumb-zone optimization** — primary actions within easy reach
- **Sticky header** with search + cart always visible
- **2-tap max** to reach any product category

---

## Part 2: Homepage Redesign

### Above the Fold (First Screen)

```
┌─────────────────────────────────────────────────────────────────┐
│ [TRUST BAR: Free Shipping | Made in USA | Warranty | Phone]    │
├─────────────────────────────────────────────────────────────────┤
│ [HEADER: Logo | Search | Account | Cart]                        │
├─────────────────────────────────────────────────────────────────┤
│ [MEGA MENU NAVIGATION]                                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│     HERO BANNER                                                 │
│     "Premium Semi Truck Parts & Chrome Accessories"             │
│     [Shop by Truck ▼]  [Shop by Part ▼]                        │
│                                                                 │
│     Hero image: Professional truck with chrome upgrades         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Below the Fold (Scroll)

#### Section 1: Shop by Truck Brand (Visual Cards)
```
┌─────────────────────────────────────────────────────────────────┐
│ SHOP BY TRUCK                                                   │
│                                                                 │
│ [Peterbilt]  [Kenworth]  [Freightliner]  [Volvo]  [Mack]       │
│   [image]      [image]      [image]       [image]   [image]    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Section 2: Featured Categories (Popular Parts)
```
┌─────────────────────────────────────────────────────────────────┐
│ POPULAR CATEGORIES                                              │
│                                                                 │
│ [Bumpers] [Visors] [Window Chops] [Door Panels] [Lighting]     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Section 3: Best Sellers (Social Proof)
```
┌─────────────────────────────────────────────────────────────────┐
│ BEST SELLERS                                            [View All]│
│                                                                 │
│ [Product 1]    [Product 2]    [Product 3]    [Product 4]       │
│  ★★★★★ (23)    ★★★★★ (18)    ★★★★☆ (12)    ★★★★★ (31)        │
│  $XXX          $XXX          $XXX          $XXX                │
│  [Add to Cart] [Add to Cart] [Add to Cart] [Add to Cart]       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Section 4: Why Buy From Us
```
┌─────────────────────────────────────────────────────────────────┐
│ WHY TRUCKERS CHOOSE US                                          │
│                                                                 │
│ 🇺🇸 Made in USA    🚚 Free Shipping    ✅ Perfect Fit Guarantee │
│ 🛡️ Warranty        📞 Expert Support   ⚡ Fast Delivery         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Section 5: New Arrivals + Sale Items
```
┌─────────────────────────────────────────────────────────────────┐
│ NEW ARRIVALS                    │ ON SALE NOW                   │
│ [Product grid]                  │ [Product grid with % off]     │
└─────────────────────────────────────────────────────────────────┘
```

---

## Part 3: Product Listing Pages (Category Pages)

### Key Features

#### 1. Faceted Filtering (Left Sidebar)
```
FILTERS
─────────────
Truck Make
☐ Peterbilt (124)
☐ Kenworth (89)
☐ Freightliner (156)

Truck Model
☐ 389 (45)
☐ W900 (32)
☐ Cascadia (67)

Price Range
[$___] - [$___]

Material
☐ Chrome
☐ Stainless Steel
☐ Aluminum

Availability
☐ In Stock (230)
☐ Ships in 3-5 days (45)
```

#### 2. Sorting Options
```
Sort by: [Best Selling ▼] [Price: Low-High] [Price: High-Low] [Newest] [Rating]
```

#### 3. Product Cards
Each product card should show:
- **High-quality image** (zoom on hover)
- **Product name** (clear, descriptive)
- **Fits:** Quick fitment info (e.g., "Fits Peterbilt 389")
- **Price** (prominent)
- **Rating** (stars + review count)
- **Availability** badge ("In Stock" / "Low Stock")
- **Quick Add to Cart** button

#### 4. Breadcrumbs
```
Home > Peterbilt > Peterbilt 389 > Door Panels
```

---

## Part 4: Product Detail Pages

### Layout
```
┌─────────────────────────────────────────────────────────────────┐
│ Breadcrumbs: Home > Category > Subcategory > Product            │
├────────────────────────────┬────────────────────────────────────┤
│                            │ PRODUCT NAME                       │
│   [Main Image]             │ ★★★★★ (23 reviews) | SKU: XXX      │
│                            │                                    │
│   [Thumbnail] [Thumb] [T]  │ $XXX.XX                            │
│                            │                                    │
│                            │ FITS:                              │
│                            │ • Peterbilt 389 (2005-2024)        │
│                            │ • Peterbilt 388 (2006-2020)        │
│                            │                                    │
│                            │ ✅ In Stock — Ships Tomorrow       │
│                            │                                    │
│                            │ Qty: [1] [ADD TO CART]             │
│                            │                                    │
│                            │ 🚚 Free Shipping on orders $99+    │
│                            │ 🛡️ 1-Year Warranty                 │
│                            │ ↩️ 30-Day Returns                   │
├────────────────────────────┴────────────────────────────────────┤
│ [Description] [Specifications] [Fitment] [Reviews]              │
├─────────────────────────────────────────────────────────────────┤
│ CUSTOMERS ALSO BOUGHT                                           │
│ [Related Product 1] [Related Product 2] [Related Product 3]     │
└─────────────────────────────────────────────────────────────────┘
```

### Key Elements

1. **Clear fitment info** — Truckers NEED to know if it fits their specific truck
2. **Multiple product images** — Different angles, installed shots
3. **Stock status** — Clear availability + shipping estimate
4. **Trust badges** — Warranty, returns, shipping near the buy button
5. **Reviews** — Star rating + written reviews (social proof)
6. **Cross-sells** — "Customers also bought" increases AOV

---

## Part 5: Search Experience

### Requirements

1. **Prominent search bar** — Large, always visible in header
2. **Autocomplete** — Show product suggestions as user types
3. **Search by part number** — Many truckers know exact part numbers
4. **Search by truck** — "Peterbilt 389 bumper" should work
5. **Typo tolerance** — "Peterbuit" should still find Peterbilt
6. **No results page** — Show alternatives, not dead ends

### Example Autocomplete
```
Search: "pete 389 bump..."

┌─────────────────────────────────────────────┐
│ PRODUCTS                                    │
│ Chrome Front Bumper - Peterbilt 389    $599 │
│ Deer Guard Bumper - Peterbilt 389      $799 │
│ Bumper Trim Kit - Peterbilt 389        $149 │
│                                             │
│ CATEGORIES                                  │
│ Peterbilt 389 > Bumpers (24 products)      │
└─────────────────────────────────────────────┘
```

---

## Part 6: Mobile Optimization

### Critical for Truckers
Many truckers browse on phones from their cab. Mobile experience is non-negotiable.

### Requirements

1. **Thumb-friendly navigation** — Menu, cart, search within thumb reach
2. **Tap targets ≥44px** — Big enough for rough hands
3. **Click-to-call phone number** — One tap to call
4. **Simplified filters** — Full-screen filter overlay on mobile
5. **Sticky add-to-cart** — Fixed button when scrolling product pages
6. **Fast load times** — Compress images, lazy load below fold

---

## Part 7: Trust & Conversion Elements

### Trust Signals (Throughout Site)

| Element | Placement |
|---------|-----------|
| Free Shipping | Trust bar, product pages, cart |
| Made in USA | Trust bar, footer, about page |
| Warranty info | Product pages, trust bar |
| Phone number | Header, footer, contact page |
| SSL badge | Checkout |
| Payment icons | Footer, checkout |
| Reviews/ratings | Product pages, homepage |

### Urgency Elements

| Element | When to Use |
|---------|-------------|
| "Low Stock" badge | When inventory < 5 |
| "X people viewing" | High-traffic products |
| Sale countdown | Time-limited promotions |
| "Ships tomorrow" | Orders before cutoff |

---

## Part 8: Footer

### Structure
```
┌─────────────────────────────────────────────────────────────────┐
│ SHOP               │ CUSTOMER SERVICE  │ ABOUT US              │
│ Shop by Truck      │ Contact Us        │ Our Story             │
│ Shop by Part       │ Shipping Info     │ Why Choose Us         │
│ New Arrivals       │ Returns & Refunds │ Blog                  │
│ Sale Items         │ FAQ               │ Careers               │
│ Gift Cards         │ Track Order       │                       │
├─────────────────────────────────────────────────────────────────┤
│ CONNECT            │ NEWSLETTER                                │
│ [FB] [IG] [YT]     │ Get exclusive deals [Email____] [SIGN UP] │
├─────────────────────────────────────────────────────────────────┤
│ [Payment Icons: Visa MC Amex PayPal ShopPay]                   │
│ © 2026 Sunshine State Chrome and Parts | Privacy | Terms       │
└─────────────────────────────────────────────────────────────────┘
```

---

## Part 9: Implementation Priority

### Phase 1: Quick Wins (Tyler doing now)
- [x] Trust bar
- [x] FAQ page
- [x] Why Buy From Us section
- [ ] Product description cleanup
- [ ] Translation string fixes

### Phase 2: Navigation Overhaul
- [ ] Implement mega menu structure
- [ ] Create "Shop by Truck" category pages
- [ ] Create "Shop by Part" category pages
- [ ] Add breadcrumbs to all pages
- [ ] Improve search with autocomplete

### Phase 3: Product Page Upgrades
- [ ] Add fitment info to all products
- [ ] Implement stock badges
- [ ] Add related products / cross-sells
- [ ] Enable product reviews
- [ ] Multiple product images

### Phase 4: Mobile Optimization
- [ ] Mobile navigation redesign
- [ ] Touch-friendly filters
- [ ] Sticky add-to-cart
- [ ] Performance optimization

---

## Competitor Reference

### Big Rig Chrome Shop (Industry Leader)
**What they do well:**
- Navigation organized by truck brand → part type
- Every brand has: Interior, Exterior, Exhaust, Collision
- Universal parts category for cross-brand accessories
- Prominent phone number (800-714-6093)
- Blog with content marketing

**Structure:**
```
Peterbilt | Kenworth | Freightliner | International | Western Star | Volvo | Mack | Universal | Lighting | Mechanical | Blog
```

### Key Takeaway
SSC should mirror this brand-first navigation. Truckers think "I need parts for my Peterbilt 389" not "I need a chrome bumper" — the truck comes first.

---

## Success Metrics

Track these after redesign:

| Metric | Current | Target |
|--------|---------|--------|
| Bounce rate | ? | <40% |
| Pages per session | ? | >4 |
| Add-to-cart rate | ? | >8% |
| Conversion rate | ? | >2.5% |
| Avg order value | ? | +15% |
| Mobile conversion | ? | >1.5% |

---

## Resources

- [Baymard E-commerce Navigation Best Practices](https://baymard.com/blog/ecommerce-navigation-best-practice)
- [Shopify E-commerce Design Guide](https://www.shopify.com/blog/best-ecommerce-sites)
- [BigCommerce Design Examples](https://www.bigcommerce.com/articles/ecommerce/best-ecommerce-website-design/)

---

*Spec prepared by Atlas. For implementation questions, coordinate with Tyler.*
