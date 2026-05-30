# PSC Court Sun Guide

A mobile-first sun glare tracker for the 10 tennis courts at **Paddington Sports Club (PSC)**, 51 Castellain Road, Maida Vale, London W9.

Helps players and bookers see at a glance which booking hours have sun glare problems, and whether cloud cover is likely to reduce that glare.

---

## Live app

Hosted on GitHub Pages — pinned to home screen on iPhone via Safari → Share → Add to Home Screen. Opens full-screen with no browser bar.

---

## What it shows

### Booking time strip
Horizontal scrollable row of booking slots (8am – 9pm). On page load it snaps to the next available booking hour. After 9pm it automatically jumps to tomorrow at 8am. Each button shows:
- Two glare dots (left = Courts 1,2,3,6,7,8 · right = Courts 4,5,9,10)
- Cloud cover % and type (L / M / H)

**Glare dot colours:**
| Colour | Meaning |
|--------|---------|
| 🟢 Green | No glare risk |
| 🟡 Amber | Moderate glare — sun within 45° of serving line, elevation <30° |
| 🔴 Red | Severe glare — sun directly in players' eyes, elevation <20° |
| ⚫ Grey | Glare suppressed by >70% low (L) or mid (M) cloud cover |

### Court diagram
Single diagram showing the selected court group with a toggle to switch between the two groups. Displays:
- Sun position on a compass ring (yellow disc + elevation label)
- Dashed amber line from sun through court centre to the opposite side (arrow points away from sun — the shadow direction)
- Glare dots at each baseline end
- Red north arrow (top-right)

### Sun stats
Elevation, azimuth, compass direction, sunrise, solar noon and sunset times — always in London local time (BST/GMT).

### Cloud cover
Fetched hourly from [open-meteo.com](https://open-meteo.com/) for the selected date. Each booking slot shows:
- Total cloud cover %
- **L** = low clouds dominant (stratus/cumulus — blocks direct sun most effectively)
- **M** = mid clouds dominant (altostratus — partial blocking)
- **H** = high clouds dominant (cirrus — barely attenuates the sun disc)

Glare is suppressed (dots turn grey) only when total cloud cover exceeds 70% **and** the dominant layer is L or M. High cirrus at even 100% cover does not reliably block glare.

### Date control
Navigate to any date to see predicted sun positions. Cloud cover forecast is available approximately 7 days ahead.

---

## Court layout

### Two orientation groups

| Group | Courts | Serving direction | Surface |
|-------|--------|------------------|---------|
| **D1** | 1, 2, 3, 6, 7, 8 | ESE ↔ WNW (bearing ~126°/306°) | 6,7,8 clay · 1,2,3 blue hard |
| **D2** | 4, 5, 9, 10 | NNE ↔ SSW (bearing ~36°/216°) | All blue hard |

Courts within each group are parallel, so one diagram per group is sufficient.

### Floodlights
- Courts **1, 2, 3, 6, 7** — bookable until **9pm**
- Courts **4, 5, 8, 9, 10** — bookable until **8pm**

---

## Solar algorithm

NOAA Solar Position Algorithm (SPA) implemented in JavaScript. Verified against NOAA's own online calculator at [gml.noaa.gov/grad/solcalc](https://gml.noaa.gov/grad/solcalc/).

All times are always displayed in **Europe/London** (BST in summer, GMT in winter) regardless of the device's timezone, using the browser `Intl` API.

---

## Glare rules

```
if sun below horizon OR angDiff > 60°  →  No glare (green)
if angDiff < 30°  AND  elevation < 20° →  Severe  (red)
if angDiff < 45°  AND  elevation < 30° →  Moderate (amber)
otherwise                               →  No glare (green)
if cloud suppressed (L/M > 70%)        →  Override to grey
```

**angDiff** = shortest angular difference between the sun's azimuth and the direction a player faces when serving (0° = sun directly in their eyes).

These thresholds are provisional and being calibrated against real on-court feedback.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | Main app — mobile-first, served via GitHub Pages |
| `sun_tracker_map.html` | Satellite map version (Leaflet + ESRI tiles) |
| `sun_tracker.html` | Original canvas prototype |
| `PSC_logo.png` | Club logo shown in the header |
| `PSC-court-co-ordinates.xlsx` | GPS corner coordinates for all 10 courts |
| `glare_rules_explained.md` | Detailed notes on the glare threshold logic |
| `PROJECT_MEMORY.md` | Development notes and decisions |

---

## Known issues / future work

- **Glare rule calibration** — thresholds need tuning against real on-court reports
- **Building shadows** — the houses on Castellain Road and boundary walls cast morning/evening shadows on some courts; not yet modelled
- **Court 6 BL coordinate** — corrected geometrically from the other three corners using the parallelogram rule (original spreadsheet had a copy-paste error)
