# Brand Benchmark Analysis — Interaction Specification

> Version: v3.2-ui  
> File: `benchmark 2.0 renew/brand-benchmark-v3.2-ui.html`  
> Last updated: May 2026

---

## Table of Contents

1. [Page Overview](#1-page-overview)
2. [Onboarding Tour](#2-onboarding-tour)
3. [Main Page — Filter Bar](#3-main-page--filter-bar)
4. [Main Page — Brand Cards](#4-main-page--brand-cards)
5. [Main Page — Data Matrix Table](#5-main-page--data-matrix-table)
6. [Detail Side Panel — Open & Close](#6-detail-side-panel--open--close)
7. [Detail Panel — Header & Filters](#7-detail-panel--header--filters)
8. [Detail Panel — §1 Performance vs Benchmark](#8-detail-panel--1-performance-vs-benchmark)
9. [Detail Panel — §2 Trend Performance](#9-detail-panel--2-trend-performance)
10. [Detail Panel — §3 Brand Distribution](#10-detail-panel--3-brand-distribution)
11. [Detail Panel — §4 Category Comparison](#11-detail-panel--4-category-comparison)
12. [Detail Panel — §5 Brand Type Distribution](#12-detail-panel--5-brand-type-distribution)
13. [Detail Panel — §6 Underperforming Campaigns](#13-detail-panel--6-underperforming-campaigns)
14. [Global Behaviors](#14-global-behaviors)

---

## 1. Page Overview

The page consists of two main areas:

| Area | Description |
|---|---|
| **Main Page** | Full-page brand benchmark matrix. Sidebar + Top Nav + Filter Bar + one card per brand. |
| **Detail Side Panel** | Slides in from the right when any data cell is clicked. Overlays the main page without replacing it. |

**Layout structure:**
```
[Sidebar 62px] [Page Shell]
                 ├─ Top Nav (56px)
                 ├─ Breadcrumb Bar (56px)
                 └─ Content
                      ├─ Filter Card
                      └─ Brand Cards (Dolphin / iRobot / Dyson / Shark)
```

---

## 2. Onboarding Tour

### 2.1 Control Flag

```javascript
const TOUR_ENABLED = true; // Set to false to disable entirely
```

When `TOUR_ENABLED = false`, the tour is completely skipped. The localStorage key `pcv_benchmark_tour_v3_done` gates repeat views (once stored, the tour does not re-show on refresh — currently commented out in DEV mode).

### 2.2 URL Parameter Override

Appending `?tourStep=N` (0-indexed) to the URL jumps directly to that step — used for Figma captures and testing.

| Value | Behavior |
|---|---|
| `?tourStep=0` | Step 1 — Category Depth |
| `?tourStep=1` | Step 2 — Percentile Benchmarks |
| `?tourStep=2` | Step 3 — Cell Details |
| `?tourStep=3..6` | Steps 4–7 (auto-opens detail panel first) |
| `?tourStep=-1` | Hides the modal overlay; no spotlight shown |

### 2.3 Welcome Modal (Step 0 — Launch Gate)

**Trigger:** Shown automatically on page load (unless `TOUR_ENABLED = false` or `tourStep` param is present).

| Element | Interaction |
|---|---|
| **✕ Close button** | Dismisses modal; marks tour as complete (`completeTour()`) |
| **Skip tour** button | Same as ✕ |
| **Get Started** button | Hides modal, starts Step 1 of the spotlight tour |

**Backdrop:** Semi-transparent overlay blocks all page interaction while modal is open.

### 2.4 Tour Steps (Spotlight + Tooltip)

Each step has: a **spotlight** (highlight box around a target element) + a **tooltip** (floating card with step content).

#### Step 1 — Category Depth Levels
- **Target:** `.cat-depth-row` (the L1/L2/L3/L4 depth buttons in the table header)
- **Tooltip position:** Below target
- **Content:** Explains the 4 Amazon category levels. Includes a link to Amazon's official category structure docs.
- **Buttons:** Skip tour · Next →

#### Step 2 — Understanding Percentile Benchmarks
- **Target:** First data cell (`.td-m[data-val]`) in the matrix
- **Tooltip position:** Right of target
- **Content:** Explains P25 / P50 / P75 percentile logic. Tip note on direction (CTR higher = better; CPC lower = better).
- **Buttons:** Skip tour · Back · Next →

#### Step 3 — Details in each Cell
- **Target:** `.mc-vs-next` (the "VS Pxx" auto-comparison chip inside any data cell)
- **Tooltip position:** Right of target
- **Content:** Explains auto-comparison to next benchmark tier. Tip note: clicking any cell opens the full analysis panel.
- **Buttons:** Skip tour · Back · Next →

**On Next:** Programmatically clicks the first data cell (`.td-m[data-val]`) to open the detail panel, then waits 380ms for the slide-in animation before showing Step 4.

#### Step 4 — Performance vs Benchmark
- **Target:** `#dp-brand-section` (the first section inside the detail panel)
- **Tooltip position:** Below target
- **Scroll:** `dp-body` scrolled to top before showing
- **Content:** Explains how core metrics compare against P25/P50/P75 and vs. last period.
- **Buttons:** Skip tour · Back · Next →

#### Step 5 — Brand Distribution
- **Target:** `#dp-dist-section`
- **Tooltip position:** Below target
- **Scroll:** Target scrolled into view (`scrollIntoView block:nearest`)
- **Content:** Explains the brand distribution matrix (where the brand sits among all peers in each tier).
- **Buttons:** Skip tour · Back · Next →

#### Step 6 — Find Campaigns That Need Attention
- **Target:** `#dp-uc-section` (Underperforming Campaigns section)
- **Tooltip position:** Below target
- **Scroll:** Target scrolled into view
- **Content:** Explains that the top 10 highest-spend underperforming campaigns are automatically surfaced.
- **Buttons:** Skip tour · Back · Next →

**On Next:** Closes the detail panel (`dp-close` click), scrolls main page to top (`window.scrollTo top:0`), waits 380ms, then shows Step 7.

#### Step 7 — You're all set. Ready to begin?
- **Target:** `.filter-card` (the main page filter bar)
- **Tooltip position:** Below target
- **Content:** Prompts user to select a brand and start exploring.
- **Buttons:** Back · Got it!

**On Back (from Step 7):** Re-opens the detail panel by clicking the first data cell, waits 380ms, then shows Step 6.

**On "Got it!":** Calls `completeTour()` — stores `pcv_benchmark_tour_v3_done` in localStorage, hides spotlight and tooltip, sets `window.pcvTourActive = false`.

### 2.5 Tour Global Behaviors

- **ESC key:** Calls `completeTour()` from any step.
- **Scroll / Resize:** Spotlight position refreshes automatically (`positionSpotlight()`).
- **Tooltip arrow direction:** Auto-flips if the tooltip would overflow the viewport edge.
- **Cell hover tooltips:** Disabled while `window.pcvTourActive === true`.

---

## 3. Main Page — Filter Bar

Located in `.filter-card` above all brand cards.

### 3.1 Profile Selector

- **Type:** Dropdown (display only — click does not open a real dropdown in current prototype)
- **Label:** "Profile"
- **Default value:** "Select All"

### 3.2 Brand Selector

- **Type:** Dropdown (display only)
- **Label:** "Brand"
- **Default value:** "Select All"

### 3.3 Ad Type Segmented Control

Buttons: **All · SB · SD · SP**

| Action | Result |
|---|---|
| Click **All** | Shows SP + SB + SD columns in all brand tables |
| Click **SB** | Hides SP and SD column groups; shows only SB columns |
| Click **SD** | Hides SP and SB column groups; shows only SD columns |
| Click **SP** | Hides SB and SD column groups; shows only SP columns |

Active button gets `.active` class. Toggle state is applied globally across all brand table cards via CSS classes `hide-sp`, `hide-sb`, `hide-sd` on each `.matrix` element.

### 3.4 Search Button

- **Type:** Button (display only in prototype)
- Contains a search icon + "Search" label.

### 3.5 Clear / Reset Button

- **Type:** Button (display only in prototype)
- Contains a trash icon. Intended to clear all filter selections.

---

## 4. Main Page — Brand Cards

Each brand renders as a `.table-card`. There are 4 brands:

| Brand | Profiles |
|---|---|
| Dolphin | BackyardPoolSuperstore · Maytronics US |
| iRobot | iRobot Official Store |
| Dyson | Dyson Direct · Dyson Vacuum Store |
| Shark | SharkNinja Official · Shark Clean US |

### 4.1 Profile Tabs (Multi-Profile Brands Only)

Brands with multiple profiles (Dolphin, Dyson, Shark) show profile tab buttons in the card header.

| Action | Result |
|---|---|
| Click a profile tab | Tab gets `.active` class; table re-renders with that profile's data; expanded rows reset to collapsed |

Single-profile brands (iRobot) show a plain text profile label — no tabs.

### 4.2 Category Layer Depth Buttons (Per-Card)

Located in the table header's first column (`th-cat-depth`). Buttons: **L1 / L2 / L3 / L4**.

| Action | Result |
|---|---|
| Click **L1** | Table shows only top-level categories (depth 1); all child rows collapsed |
| Click **L2** | Expands to depth 2 automatically |
| Click **L3** | Expands to depth 3 |
| Click **L4** | Expands all levels (default) |

Each card's depth is independent. Clicking L2 on Dolphin does not affect iRobot's depth.

### 4.3 Tier Distribution Chips (Per-Card)

Located in `.tier-bar` between the card header and the table. Chips: **P0–P25 · P25–P50 · P50–P75 · P75–P100**

| Action | Result |
|---|---|
| Click a chip | Highlights all cells in that tier within **this card only**. Non-matching cells dim. Chip gets `.active`; others get `.inactive`. "✕ Clear filter" button appears. |
| Click the **same chip again** | Clears the filter for this card (toggle off) |
| Click **✕ Clear filter** | Removes all tier highlighting for this card |

Tier filter is per-brand — no cross-card side effects.

---

## 5. Main Page — Data Matrix Table

The main table has a fixed first column (Category / Layer) and scrollable metric columns grouped by ad type (SP / SB / SD).

### 5.1 Row Expand / Collapse

Each row with child categories shows a `.row-toggle` chevron icon in the first column.

| Action | Result |
|---|---|
| Click **▶ toggle** on a collapsed row | Expands to show immediate children |
| Click **▼ toggle** on an expanded row | Collapses that row AND all its descendants |

Leaf rows (no children) show a `.leaf` state with no clickable toggle.

### 5.2 Crosshair Row + Column Highlight

On `mouseover` of any `<td>`:
- The entire row gets `.row-hl` class (row highlight)
- All cells in the same column index across all rows get `.col-hl` class
- The hovered cell gets `.cell-hl` class (intersection highlight)

On `mouseleave` from the container, all highlights are cleared.

### 5.3 Cell Hover Tooltip

**Trigger:** `mouseover` on any `.td-m[data-val]` cell (data cells with a value).

**Disabled when:** `window.pcvTourActive === true` (during the onboarding tour).

**Tooltip content:**
```
[Metric Label]              [Value]
[Tier Badge: P0–P25 etc.]

─────────────────────────────
P25    [delta%] ↑↓   [P25 value]
P50    [delta%] ↑↓   [P50 value]   ← active benchmark row (highlighted)
P75    [delta%] ↑↓   [P75 value]
```

The highlighted row in the tooltip corresponds to the upper boundary of the cell's tier:
- t1 (P0–P25) → P25 row highlighted
- t2 (P25–P50) → P50 row highlighted
- t3 (P50–P75) → P75 row highlighted
- t4 (P75–P100) → P75 row highlighted

**Delta color logic:**
- Green (`.pos`): metric is moving in a favorable direction
- Red (`.neg`): unfavorable direction
- Gray (`.neu`): < 1% difference

**Tooltip positioning:** Follows mouse cursor. Auto-flips horizontally/vertically if near viewport edges. 80ms hide delay to allow moving into the tooltip.

### 5.4 Cell Click — Open Detail Panel

**Trigger:** Click on any `.td-m[data-val]` cell.

**Result:** 
1. Detail side panel slides in from the right
2. Panel is populated with data from the clicked row (category name, brand, metrics for all ad types)
3. Cell tooltip is hidden

---

## 6. Detail Side Panel — Open & Close

### 6.1 Opening

Triggered by clicking any data cell in the main table. The panel slides in from the right via CSS transition. A semi-transparent backdrop (`#detail-backdrop`) appears behind the panel.

The panel is populated with:
- Brand name (from the `.table-card` that owns the clicked row)
- Category name (from the `.cat-name` in the clicked row)
- Category level (depth from row class `l1-row`, `l2-row`, etc.)
- All metric values from the clicked row's cells

### 6.2 Closing

| Trigger | Result |
|---|---|
| Click **✕ close button** (`#dp-close`) | Panel slides out; backdrop disappears |
| Click **backdrop** (`#detail-backdrop`) | Same as close button |

---

## 7. Detail Panel — Header & Filters

### 7.1 Header Bar (`#dp-header`)

| Element | Interaction |
|---|---|
| **Monthly** dropdown | Display only (prototype) |
| **03-2026** date selector | Display only (prototype) |
| **✕ close button** | Closes the panel |

### 7.2 Filter Bar (`#dp-filter`)

| Filter | Behavior |
|---|---|
| **Profile** | Display only (shows "Henkel – Home Care") |
| **Brand** | Display only (shows "—") |
| **Category Layer** | Display only (shows "L2") |
| **Benchmark** | Display only (shows "P50") |
| **SP / SB / SD segmented toggle** | Active — see below |

#### SP / SB / SD Toggle

| Action | Result |
|---|---|
| Click **SP** | `activeAdtype = 'sp'`; re-renders all panel sections for SP data only |
| Click **SB** | `activeAdtype = 'sb'`; re-renders all panel sections for SB data |
| Click **SD** | `activeAdtype = 'sd'`; re-renders all panel sections for SD data |

The selected ad type affects:
- §1 Performance vs Benchmark (metric groups shown)
- §2 Trend Performance (metric list in dropdown)
- §3 Brand Distribution (matrix columns)
- §4 Category Comparison (metric columns)
- §5 Brand Type Distribution (radar axes + table columns)

---

## 8. Detail Panel — §1 Performance vs Benchmark

### 8.1 SP Layout

When `activeAdtype = 'sp'`, the section shows:
- Brand name (20px/500)
- Category breadcrumb path with chevrons
- Country: US · Peer Set Size: 15–20
- **Efficiency group card** containing CTR and CPC metric blocks

### 8.2 SB / SD Layout

When `activeAdtype = 'sb'` or `'sd'`, the section shows:
- Brand name + category name
- Country / Peer Set Size / Level metadata
- **Grouped metric rows** (horizontal scroll):
  - **SP:** Efficiency (CTR, CPC)
  - **SB:** Efficiency (CTR, CPC, CPM) + New to Brand (% Purchases, Purchase Rate, Cost/Purchase)
  - **SD:** Efficiency + New to Brand + Video Ad (Completion Rate, Cost/Completed View)

### 8.3 Metric Block

Each metric block shows:
- Metric label + ⓘ icon
- Current value + tier badge (P0–P25 etc.)
- Delta comparison columns: **P25 · P50 · P75 · Last**
  - Each shows: benchmark value + percentage delta + directional arrow
  - Color: green (favorable) / red (unfavorable) / gray (< 0.5%)

No interactive click behavior on individual metric blocks (display only).

---

## 9. Detail Panel — §2 Trend Performance

### 9.1 Metrics Dropdown

**Element:** `.dp-trend-metric-dd`

| Action | Result |
|---|---|
| Click the dropdown | Opens a menu listing all available metrics for the current `activeAdtype` |
| Select a metric | Updates `activeTrendMetric`; re-renders the SVG chart and legend; updates insights panel |

Available metrics change with the ad type:
- **SP:** CTR, CPC
- **SB:** CTR, CPC, CPM, % Purchases, Purchase Rate, Cost/Purchase
- **SD:** All SB metrics + Completion Rate, Cost/Completed View

### 9.2 Date Range Button

- **Display only** in prototype — shows "02/2025 ~ 01/2026"

### 9.3 SVG Trend Chart

The chart shows 6 months of data (Nov → Apr) with:
- **Purple line:** Brand's metric value over time
- **Blue band:** P25–P50 benchmark range
- **Orange band:** P50–P75 benchmark range

**Hover interaction (mousemove over hit area):**
- A vertical crosshair line snaps to the nearest data point
- A hover dot appears on the brand line
- A tooltip appears showing:
  - Month label
  - Brand value
  - P25 / P50 / P75 values for that month

**On mouseleave:** Crosshair, dot, and tooltip all hide.

### 9.4 Legend

Static labels rendered below the chart:
- Purple line swatch → metric name
- Blue band swatch → "Metric P25–P50"
- Orange band swatch → "Metric P50–P75"

### 9.5 Insights Panel

Located to the right of the chart.

- Shows "Select a metric to view insights" when no metric is chosen yet
- After a metric is selected, shows:
  - A chip: "[Metric] is X% above/below [Metric] P50"
  - A text description with actionable guidance

---

## 10. Detail Panel — §3 Brand Distribution

A matrix table showing where the brand and its peers fall across percentile tiers for each metric.

### 10.1 Structure

| Column | Content |
|---|---|
| First column | Tier labels: P0–P25 / P25–P50 / P50–P75 / P75–P100 |
| Metric columns | Up to 3 metrics (first 3 of the current ad type) |

Each cell contains brand name tags:
- **Blue tag** (`.me`): the current brand
- **Gray tag** (`.peer`): competitor brands

### 10.2 Tag Collapse / Expand

When a cell has more than 6 brand tags:

| Action | Result |
|---|---|
| Default view | Shows 6 tags + "+N more" button |
| Click **"+N more"** | Expands to show all tags + "Show less" button |
| Click **"Show less"** | Collapses back to 6 |

### 10.3 Metrics Dropdown (Header)

The `dp-dist-dd` dropdown in the section header shows "N Selected" — display only in prototype.

---

## 11. Detail Panel — §4 Category Comparison

A comparison table showing metrics across category levels for the current brand and its peers.

### 11.1 Table Structure

| Column | Content |
|---|---|
| Category | Category names at each depth level |
| Peer Set Size | Count of advertisers in that category |
| Metric columns | Variable based on `activeAdtype` |

Each metric cell uses the same hover tooltip as the main table (triggered via `.cc-td-metric[data-val]`).

### 11.2 Hover Tooltip

Same tooltip component as the main matrix (see §5.3). Triggered on `mouseover` of `.cc-td-metric[data-val]`.

---

## 12. Detail Panel — §5 Brand Type Distribution

Contains a radar/spider chart on the left and a campaign type comparison table on the right.

### 12.1 Radar Chart

**Axes:** Up to 8 metric axes arranged radially (depends on `activeAdtype`):
- SP: CTR, CPC
- SB: CTR, CPC, CPM, % Purchases, Purchase Rate, Cost/Purchase
- SD: All SB + Completion Rate, Cost/Completed View

The chart has concentric rings representing P0, P25, P50, P75, P100. The brand's values plot as a polygon with a semi-transparent fill.

**Hover interaction:** Hovering over a polygon vertex/segment shows a tooltip:
- Row 1: Ad type (SP / SB / SD)
- Row 2: Metric name + value
- Row 3: Percentile range (e.g. P25–P50)
- Rows 4–6: P25 / P50 / P75 absolute values

### 12.2 Legend — SP / SB / SD Toggle

Located below the radar chart. Shows colored dot + label for each ad type.

| Action | Result |
|---|---|
| Click an **active** legend item (SP / SB / SD) | Hides that ad type's polygon from the chart; legend item dims to gray |
| Click an **inactive** legend item | Re-shows that ad type's polygon; legend item restores its color |

Multiple ad types can be hidden simultaneously.

### 12.3 Campaign Type Comparison Table (Right Side)

**Fixed first column:** Campaign Type (SP / SB / SD)

Metric columns scroll horizontally. The first column (`Campaign Type`) stays sticky — does not scroll.

Each metric cell shows value + tier badge. Same hover tooltip as main matrix (`.btd-td-metric[data-val]`).

| Column | Content |
|---|---|
| Campaign Type | SP / SB / SD (sticky) |
| Peer Set Size | Integer count |
| Metric columns | CTR / CPC / CPM / etc. |

---

## 13. Detail Panel — §6 Underperforming Campaigns

A table listing the Top 10 highest-spend campaigns that are underperforming relative to benchmarks.

### 13.1 Table Structure

| Column | Content |
|---|---|
| Rank | Top 1, 2, 3 … |
| Related Campaign | Campaign name |
| State | Enabled / Paused / etc. |
| Daily Budget | Dollar value |
| Qualified Targetings | Targeting type + count chip |
| Metric columns (CTR, CPC, etc.) | Value + tier range badge (e.g. P0–P25) |

### 13.2 Cell Styling

Each metric cell in the table shows:
- The metric value
- A colored tier badge below the value (green = P0–P25, blue = P25–P50, orange = P50–P75, red = P75–P100)

This section is display only — no interactive sorting, filtering, or row click behaviors in the current prototype.

---

## 14. Global Behaviors

### 14.1 Data Architecture

All data is hardcoded as JavaScript objects (`DATA_DOLPHIN_BACKYARD`, `DATA_IROBOT`, `DATA_DYSON_DIRECT`, etc.). There are no API calls.

Percentile values are derived from a single `p50` value using fixed multipliers:
```
P25 = p50 × 0.75
P75 = p50 × 1.25
```

### 14.2 Tier Classification

```javascript
function getTier(val, p50) {
  const p25 = p50 * 0.75, p75 = p50 * 1.25;
  if (val < p25) return 't1';  // P0–P25
  if (val < p50) return 't2';  // P25–P50
  if (val < p75) return 't3';  // P50–P75
  return 't4';                 // P75–P100
}
```

### 14.3 Delta Direction (Good vs Bad)

Each metric has a `good` property: `'high'` or `'low'`.

| Metric | good | Meaning |
|---|---|---|
| CTR | high | Higher is better |
| CPC | low | Lower is better |
| CPM | low | Lower is better |
| % Purchases | high | Higher is better |
| Purchase Rate | high | Higher is better |
| Cost/Purchase | low | Lower is better |
| Completion Rate | high | Higher is better |
| Cost/Completed View | low | Lower is better |

### 14.4 Responsive / Scroll Behavior

- The main page scrolls vertically. The sidebar is fixed.
- The detail panel has its own internal scroll (`#dp-body`).
- The table header row is sticky within the table scroll container.
- The tour spotlight re-positions on `scroll` and `resize` events.

### 14.5 VS Next Tier Logic

Each cell in the main table shows a "VS Pxx" comparison to the **next benchmark tier** the brand should aim for. This is auto-calculated:

| Current Tier | good=high target | good=low target |
|---|---|---|
| t1 (P0–P25) | VS P25 | — (already best) |
| t2 (P25–P50) | VS P50 | VS P25 |
| t3 (P50–P75) | VS P75 | VS P50 |
| t4 (P75–P100) | — (already best) | VS P75 |

---

*End of document*
