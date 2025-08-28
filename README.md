# Simple Elevation — User Guide (v1.8.4)

A lightweight web app for turning a GPX track into a crisp, publication‑ready elevation profile with climbs, POIs, and gravel segments. This guide walks you through **all** features step‑by‑step.

---

## 1) Quick Start

1. **Open the app** (the single HTML file).
2. **Upload a GPX** via **Upload GPX**.
3. The profile renders. Use the **mouse wheel** to zoom, **Shift + wheel** to pan, **double‑click** to reset the view.
4. Add features:

   * **Climb**: *Add climb* → click **start** then **end** on the chart (or on the map).
   * **POI**: *Add POI* → single click at the position (chart or map).
   * **Gravel**: *Add gravel* → click **start** then **end** (chart or map).
5. Edit names/offsets in the lists; fine‑tune style using the controls; export as **SVG/PNG** or save a **JSON project**.

---

## 2) Interface Tour

### Layout

* **Left**: feature lists and actions (**Climbs**, **POIs**, **Gravel**).
* **Middle**: interactive **map** (OpenStreetMap tiles).
* **Right**: **elevation chart** and all **style**/**export** controls.
* Columns can be resized by dragging the vertical separators.

### Chart Basics

* Clean profile with **area fill** + **outline**.
* **Y grid** every 50/100/200 m (selectable).
* **X ticks** at a fixed **km step**; the total distance is shown at the end.
* **Start/Finish** labels at the extremes (with altitude lines), individually offsettable.
* **Vertical exaggeration** compresses the top to preserve label headroom.

### Map Panel

* Hovering the map shows a synchronized **vertical guide** on the chart.
* Click the map to place **starts/ends** or **POIs** (same as clicking the chart).
* The track is auto‑fit to bounds; the hover cursor follows the closest GPX point.

---

## 3) Loading GPX

* The app accepts **GPX tracks** (`<trkpt>` or `<rtept>`). If distances are missing, it computes them cumulatively.
* Elevations can be **smoothed** (meter window) using a centered moving average.
* Waypoints (`<wpt>`) are parsed for **names**: when you create a POI the app tries to pick the nearest waypoint (within \~5 km) as its default name.

**Tip:** If your GPX has very sparse points, increase smoothing cautiously or you may flatten short ramps.

---

## 4) Controls (right panel)

### Rendering / Styling

* **Vertical exaggeration (×)** — compresses the top of the chart to create room for labels.
* **Vertical grid step** — 50/100/200 m.
* **Smoothing (m)** — moving‑average window in meters.
* **Tick step (km)** — distance between x‑axis labels.
* **Profile colors** — **fill** and **outline**.
* **Climb colors** — **line** and **fill** (the fill is a translucent overlay clipped to the area).
* **Start/Finish offsets** — vertical nudge for top labels.

### Presets

* **Save preset** — stores the full styling setup in your browser `localStorage`.
* **Load/Delete** — retrieve or remove any saved preset.
* Presets include: `vExag, yStep, smoothing, tick, fillColor, lineColor, climbColor, climbFillColor, startOffset, finishOffset`.

### Export / Import

* **Export SVG** — fonts are embedded as data‑URLs for portability; great for print.
* **Export PNG** — raster export at 2× scale with a white background.
* **Export JSON** — saves the entire project (points, features, labels, settings).
* **Import JSON** — restores a previous project exactly.

**Note:** Browsers may block downloads if the page isn’t user‑initiated; click the button again or allow popups if necessary.

---

## 5) Features: Climbs, POIs, Gravel

### 5.1 Climbs

* **Create**: click **Add climb** → click the **start** and **end** on the chart **or** on the map.
* **Edit name**: in the left list (press **Enter** to confirm).
* **Metrics**: length (km), average slope (%), end altitude (m); shown under the name in the list.
* **Label on chart**:

  * Multi‑line: **NAME** (uppercase, wrapped) on one/two lines; below it a single line with `km · % · m`.
  * The label is attached to the **end** of the climb, aligned to the vertical guide.
  * Use the ▲/▼ nudges in the list to adjust **vertical offset** (per climb).
* **Overlay**: translucent fill over the climb segment plus a climb‑colored outline.
* **KOM Badges**: add `KOMHC`, `KOM1`, `KOM2`, `KOM3`, or `KOM4` at the **start of the name** to render a circular badge at the end (e.g., `KOM2 Monte Nero`).
* **Partial km tick**: a small rotated **distance marker** is added near the bottom at the end of the climb if at least \~2 km remain in the track (to avoid clutter).

### 5.2 POIs

* **Create**: click **Add POI** → single click on chart or map.
* **Default name**: if a GPX waypoint is nearby, its name is used; otherwise `POI`.
* **Label format**: **one line**, rotated and aligned to the **exact X** of the guide: `Name · Altitude m · km km`.
* **Alignment / anchoring**: pivoted at the guide (the text starts at the guide and continues outward). Use ▲/▼ to fine‑tune vertical offset per POI.
* **Partial km tick**: same rule as climbs; added at the POI position when there’s enough remaining distance.

### 5.3 Gravel Segments

* **Create**: click **Add gravel** → set **start** and **end**.
* **Shading**: a translucent grey overlay clipped to the profile.
* **Label**: two lines (uppercase **NAME** on top, then `km` length). Anchored on the vertical at the **end** of the gravel. Vertical position is slightly above the end elevation for readability; adjust per‑segment with ▲/▼.
* **Editing the name is supported** (left panel).

---

## 6) Start & Finish Labels

* Enter **Start label** and **Finish label** (default: START/FINISH). They render at the chart’s extremes with their respective altitudes beneath.
* Use **Start/Finish offset** to raise/lower these headers without affecting the rest of the layout.

---

## 7) Interactions & Navigation

* **Zoom**: mouse wheel.
* **Pan**: hold **Shift** + wheel (or two‑finger trackpad gesture that generates horizontal scroll). The chart pans left/right while preserving span.
* **Reset view**: **double‑click** the chart.
* **Guides**: moving the mouse (map or chart) draws a vertical guide and synchronizes the map cursor to the nearest GPX point.

---

## 8) JSON Project Format

When you export a project, the JSON looks like this:

```json
{
  "meta": { "version": "1.8.4" },
  "points": [
    { "lat": 45.0, "lon": 7.0, "ele": 312.0, "eleS": 310.2, "d": 0 },
    { "lat": 45.0, "lon": 7.1, "ele": 320.0, "eleS": 318.3, "d": 1023.4 }
    // ...
  ],
  "climbs": [
    { "startIdx": 102, "endIdx": 380, "name": "KOM2 MONTE NERO", "offsetY": 8 }
  ],
  "pois": [
    { "idx": 560, "name": "Church", "offsetY": 8 }
  ],
  "gravels": [
    { "startIdx": 1280, "endIdx": 1640, "name": "White Road", "offsetY": 0 }
  ],
  "labels": { "start": "START", "finish": "FINISH" },
  "settings": {
    "vExag": 0.50,
    "yStep": 200,
    "smooth": 500,
    "tick": 10,
    "fill": "#fbf6ea",
    "line": "#ff2d96",
    "climbLine": "#15a34a",
    "climbFill": "#15a34a",
    "startOff": 0,
    "finishOff": 0
  }
}
```

* `points` keep both raw elevation `ele` and smoothed `eleS` (used for drawing).
* Feature indices (`startIdx`, `endIdx`, `idx`) refer to positions within `points`.
* You can safely edit this JSON and **Import JSON** back into the app.

---

## 9) Version Notes (what’s new in 1.8.4)

* **POIs** are now anchored on the **exact X** of their guide; label starts at the guide to avoid crossing it.
* **Distance partial markers** (small rotated numbers near the bottom) moved further down to avoid collisions: **`yBase + 42`**.
* **Gravel label** raised slightly relative to the end altitude for legibility; still vertically aligned to the segment end.
* Presets and all previous features remain fully backward‑compatible.

---

## 10) Tips & Best Practices

* **Label clarity**: if a climb/POI label touches the profile, nudge it with the ▲/▼ offset in the list. Small increments (8 px) are ideal.
* **Smoothing**: 300–600 m works well for road routes; try 100–250 m for short punchy gravel routes.
* **Grid step**: smaller steps help on compact profiles; larger steps reduce clutter on long alpine routes.
* **Colors**: maintain high contrast between the profile outline and fills for print.
* **KOM**: use the `KOM{HC|1..4}` prefix in the **name** to display the badge automatically (e.g., `KOMHC Colle delle Finestre`).

---

## 11) Troubleshooting

* **GPX loads but the chart is flat**: the GPX may lack distances; the app recomputes them, but verify units and smoothing.
* **Waypoints are not named**: ensure your GPX includes `<wpt>` entries with `<name>`.
* **Fonts look different after export**: SVG embeds webfonts as data‑URLs for portability; PNG rasterizes the result — ensure you export at sufficient resolution.
* **Nothing happens when exporting**: browser popup/download restrictions; retry or allow downloads for the page.
* **Presets disappear**: they live in `localStorage`; incognito windows or cleared site data will remove them.

---

## 12) Privacy & Data

* The app runs locally in your browser. GPX files are not uploaded anywhere.
* Presets are stored in your browser only.

---

## 13) Keyboard & Mouse Reference

* **Zoom**: wheel
* **Pan**: Shift + wheel
* **Reset view**: double‑click chart
* **Add climb**: click start → click end (chart or map)
* **Add POI**: single click (chart or map)
* **Add gravel**: click start → click end (chart or map)

---

## 14) Credits

* Map tiles: OpenStreetMap contributors.
* Fonts: Montserrat & Inter.

Enjoy building beautiful elevation profiles! If you need variations (brand colors, typography, badges, or data columns), the app is easily customizable.
