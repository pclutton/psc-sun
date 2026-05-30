# PSC Court Sun Guide

A mobile-first sun glare tracker for the 10 tennis courts at **Paddington Sports Club (PSC)**, Maida Vale, London W9.

Answers the question: *"Which court should I book, and will the sun be a problem?"*

---

## Using the app

### Booking time strip
Swipe left/right through the hourly booking slots (8am–9pm). The strip automatically opens at the next available booking hour. After 9pm it jumps to tomorrow at 8am — since PSC only allows bookings up to 4 hours ahead, there's nothing useful to show for tonight.

Each button displays:
- **Two glare dots** — left dot for Courts 1,2,3,6,7,8 · right dot for Courts 4,5,9,10
- **Cloud cover** — percentage and layer type (L / M / H)

### Court diagram
Tap either toggle to switch between the two court groups. The diagram shows:
- The sun's position on a compass ring, with its elevation in degrees
- A dashed amber line running from the sun, through the court centre, to an arrowhead pointing in the shadow direction
- Coloured dots at each baseline showing the glare risk for players serving from that end

### Date control
Tap the arrows or the date field to explore other days. Cloud forecast is available approximately 7 days ahead.

---

## Glare dot colours

| Dot | Meaning |
|-----|---------|
| 🟢 Green | No glare risk |
| 🟡 Amber | Moderate glare — sun within 45° of the serving/receiving line |
| 🔴 Red | Severe glare — sun within 15° of the serving/receiving line |
| ⚫ Grey | Glare suppressed by heavy low or mid-level cloud cover |

Dots are calculated separately for each end of the court, because players at opposite baselines face different directions. The time strip shows the worst-case dot for each court group.

---

## Cloud cover (L / M / H)

Cloud data is fetched hourly from [open-meteo.com](https://open-meteo.com/) for the selected date.

| Label | Cloud type | Effect on glare |
|-------|-----------|----------------|
| **L** | Low clouds — stratus, cumulus | Blocks direct sun effectively |
| **M** | Mid clouds — altostratus | Partial blocking |
| **H** | High clouds — cirrus | Barely attenuates the sun disc — glare still possible |

Glare dots turn grey only when total cloud cover exceeds 70% **and** the dominant layer is L or M. A sky full of high cirrus (H) does not suppress glare, even at 100%.

---

## How the glare model works

The model evaluates two playing roles independently for each baseline end:

**Server gaze window (elevation 15°–70°)**
During the ball toss, the server looks almost directly upward. Any sun within this elevation band and within 45° of the serving direction causes glare.

**Receiver gaze window (elevation 8°–25°)**
The receiver tracks the incoming ball low over the net. Lower-elevation sun within 45° of the receiving direction causes glare.

**Environmental clearance**
Below 8° elevation the sun is blocked by PSC's surrounding fences, trees and buildings — no glare possible regardless of azimuth.

**Severity tiers**

| Threshold | Severity |
|-----------|---------|
| Sun within 15° of serving/receiving line, within gaze window | Severe 🔴 |
| Sun within 45° of serving/receiving line, within gaze window | Moderate 🟡 |
| Outside window or beyond 45° | No glare 🟢 |

> **Note:** These thresholds are physically motivated but are being calibrated against real on-court experience. If you play a session where glare is a genuine problem, note the date, time and court — checking the elevation and sun angle in the app for that hour helps tune the model.

---

## Court layout

### Two orientation groups

| Group | Courts | Serving direction | Surface |
|-------|--------|-----------------|---------|
| **D1** | 1, 2, 3, 6, 7, 8 | ESE ↔ WNW | 1,2,3 blue hard · 6,7,8 clay |
| **D2** | 4, 5, 9, 10 | NNE ↔ SSW | Blue hard |

Courts within each group are parallel, so one diagram covers the whole group.

### Bookable hours
- Courts 1, 2, 3, 6, 7 — until **9pm** (floodlit)
- Courts 4, 5, 8, 9, 10 — until **8pm**

---

## Solar calculation

Sun position uses the **NOAA Solar Position Algorithm** implemented in JavaScript. All times are shown in **London local time (BST/GMT)** regardless of the device's timezone.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | Main app — edit this, then upload to GitHub Pages to deploy |
| `sun_tracker_map.html` | Alternative satellite map view (Leaflet + ESRI tiles) |
| `PSC_logo.png` | Club logo |
| `PSC-court-co-ordinates.xlsx` | GPS corner coordinates for all 10 courts |
| `glare_rules_explained.md` | Technical notes on the glare threshold logic |
| `PROJECT_MEMORY.md` | Development history and decisions |
