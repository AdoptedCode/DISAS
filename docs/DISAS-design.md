# DISAS — Design System & Visual Specification

## 1. Design Direction & Philosophy

The DISAS design language synthesizes four distinct visual disciplines into a unified aesthetic:

$$\textbf{Scientific Cartography} + \textbf{Editorial Geographic Visualisation} + \textbf{Isometric Digital Twin} + \textbf{Liquid Glass}$$

### Core Design Tenets
1. **Map First, Intelligence on Demand:** Spatial reality is the primary canvas. Dashboards, telemetry, and graphs never overwhelm the geographic context; they float gracefully above it as translucent, contextual overlays.
2. **Progressive Disclosure:** Surfaces initially display high-level situational awareness (overall alert tier, flood extent boundaries, key counts). Detailed telemetry, AI factor breakdowns, and CCTV feeds reveal themselves only upon direct selection, hover, or semantic zoom.
3. **Miniature-World Isometric Precision:** The 3D digital twin renders Thane’s urban geography with the elegance of an architectural model or high-precision diorama—restrained, tactile, low-friction, and visually arresting without unnecessary clutter.
4. **Legibility in Crisis:** While subtle aesthetics and soft translucent glass define the ambient chrome, hazard warnings, risk levels, and life-safety markers are rendered with unequivocal contrast, immediate readability, and unambiguous color coding.

---

## 2. Color Palette & Theming

DISAS uses a calibrated palette combining scientific cartographic pastels with high-visibility emergency accents.

### 2.1 Environmental & Base Canvas Tones

```css
/* Ambient Canvas & Cartographic Base */
--canvas-deep: #0D1117;          /* Slate midnight primary background */
--canvas-surface: #161B22;       /* Secondary container surface */
--canvas-card: rgba(22, 27, 34, 0.72); /* Translucent base for Liquid Glass */

/* Pastel Terrain & Environmental Shading */
--terrain-lowland: #E2E8F0;      /* Gentle light slate (valley/plain) */
--terrain-foothill: #CBD5E1;     /* Soft contour tint (Yeoor slopes) */
--terrain-highland: #94A3B8;     /* Muted granite (ridgeline) */
--water-normal: #38BDF8;         /* Calm azure masunda lake & creek */
--water-inundation: #0284C7;     /* Active flood surface tint */
--vegetation-muted: #A7F3D0;     /* Muted celadon green for mangrove/canopy */
--urban-structure: #F1F5F9;      /* Clean architectural plaster white/off-white */
```

### 2.2 Risk Stage Tiers & Safety Indicators

| Risk Tier | Hex Code | Purpose & Semantic Meaning | Application |
| :--- | :--- | :--- | :--- |
| **Normal** | `#10B981` (Emerald) | Safe conditions, routine drainage, unobstructed roads. | Badges, clear routes, normal lake levels. |
| **Watch** | `#F59E0B` (Amber) | Water depth 15–45 cm, precautionary mobilization, pedestrian caution. | Advisory banners, rising water alerts. |
| **Warning** | `#F97316` (Orange) | Water depth 45–90 cm, ground floor flooding, road closures. | Incident markers, route severances. |
| **Critical** | `#EF4444` (Ruby/Crimson) | Water depth > 90 cm, life-safety hazard, rescue boat requirement. | High-priority SOS, evacuation orders. |

### 2.3 Data Provenance Indicator Accents

- **`MEASURED`:** Solid Emerald border (`#10B981`) with crisp dot icon.
- **`ESTIMATED`:** Dashed Amber border (`#F59E0B`) with tilde symbol ($\approx$).
- **`SIMULATED`:** Glowing Violet border (`#8B5CF6`) with physics wave icon ($\sim$).
- **`UNAVAILABLE`:** Muted Gray hatch (`#6B7280`) with slash icon ($\varnothing$).

---

## 3. The Liquid Glass UI System

The Liquid Glass paradigm provides translucent floating surfaces that preserve spatial context beneath while maintaining strict contrast for vital emergency data.

```css
/* Liquid Glass Utility Classes */
.liquid-glass-panel {
  background: rgba(15, 23, 42, 0.65);
  backdrop-filter: blur(16px) saturate(180%);
  -webkit-backdrop-filter: blur(16px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.12);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37),
              inset 0 1px 0 0 rgba(255, 255, 255, 0.15);
  border-radius: 12px;
}

.liquid-glass-interactive {
  transition: all 0.2s cubic-bezier(0.16, 1, 0.3, 1);
}
.liquid-glass-interactive:hover {
  background: rgba(30, 41, 59, 0.75);
  border-color: rgba(255, 255, 255, 0.22);
  transform: translateY(-1px);
}
```

### Visual Stacking & Depth Hierarchy
- **Level 0 (Base Canvas):** Interactive MapLibre 2D canvas or Three.js 3D WebGL viewport.
- **Level 1 (Ambient Overlays):** Semi-transparent map controls, coordinate indicators, simulation timeline scrubber at bottom center.
- **Level 2 (Contextual HUD Panels):** Collapsible lateral sheets (incident triage queue on left, CCTV camera inspector on right).
- **Level 3 (Modal Alerts & Dispatch Approvals):** High-priority confirmation dialogs with darkened backdrop and glowing perimeter border.

---

## 4. 3D Isometric Digital Twin Specification

### 4.1 Aesthetic & Visual Rules
- **Camera Style:** Orthographic or narrow field-of-view perspective ($FOV \le 28^\circ$) oriented at a classic $35.264^\circ$ isometric pitch angle.
- **Miniature Diorama Look:**
  - Soft directional lighting simulating overcast monsoon daylight with gentle ambient occlusion in street crevices.
  - Stylized low-polygon procedural buildings with chamfered roof edges and clean white/plaster tones.
  - Semi-translucent, refractive water planes with animated animated subtle ripples indicating current flow direction.
  - Realistic scale cues: miniature trees, animated emergency vehicle beacons, and tiny elevated pedestrian footbridges.

### 4.2 Simulation-Driven Water Progression & Elevation Coupling
- **Physical Coupling to Simulation Output:** The water surface is not an arbitrary aesthetic animation. It is mathematically and graphically bound to the output of the hydrological simulation engine:
  - **Inputs:** Rainfall intensity $R(t)$, duration, and drainage efficiency.
  - **Cellular Grid Calculation:** The backend calculates water depth $h(x, y, t)$ across the DEM elevation raster $Z_{\text{terrain}}(x, y)$.
  - **Rendered Water Surface Elevation:**
    $$W(x, y, t) = Z_{\text{terrain}}(x, y) + h(x, y, t)$$
- **Three.js Visual Mapping:**
  - The 3D water mesh vertices (or displacement shader texture) sample $W(x, y, t)$ directly.
  - Areas where $h(x, y, t) \le 0$ remain completely dry with exposed ground/road surfaces.
  - When inundation depth $h(x, y, t) > 0$, the water plane geometry dynamically conforms to the terrain depression, pooling in natural lowlands first (e.g., Teen Hath Naka underpass, Vandana basin) before breaching road crowns.
  - **Depth-Based Chromatic Progression:**
    - Shallow ($0 < h \le 15\text{cm}$): Semi-translucent pale cyan/turquoise with faint ripple distortion (`#38BDF8` with opacity 0.45).
    - Moderate ($15 < h \le 45\text{cm}$): Cerulean blue (`#0284C7` with opacity 0.70), submerged road striping faintly visible.
    - Warning ($45 < h \le 90\text{cm}$): Deep marine blue (`#0369A1` with opacity 0.88), road models darkened, warning hazard icons floating above.
    - Critical ($h > 90\text{cm}$): Opaque dark oceanic slate (`#075985` with opacity 0.95) with foam highlights along building contact edges.
  - Road segments crossing beneath the water mesh visibly dim and display submerged warning indicators.

---

## 5. Typography & Numerical Formatting

- **Primary Font Family:** Clean, neutral modern sans-serif (`Inter`, `Geist Sans`, or system sans).
- **Tabular Monospace Family:** `JetBrains Mono` or `Geist Mono` for all coordinates, timestamps, water depths, rainfall rates, and telemetry numbers.
  - Example: `19.2183° N, 72.9781° E | 114.2 mm/h | +02:45:00`
- **Typographic Scale:**
  - `Hero Title / Warning Directive`: 24px–32px (Bold / Tracking tight)
  - `Panel Header`: 14px–16px (Semibold / Uppercase subtle tracking +0.05em)
  - `Body / Descriptions`: 13px–14px (Regular / Leading relaxed)
  - `Status Badges & Micro-Labels`: 10px–11px (Medium / Monospace)

---

## 6. Interface Layout Patterns

### 6.1 Citizen Portal (`/citizen`)
- **Philosophy:** Extreme simplicity, low bandwidth resilience, zero learning curve.
- **Primary Element:** Big, prominent Red SOS button (`"Report Flood / Request Rescue"`) and instant location chip.
- **Sub-surfaces:**
  - Carousel of nearby verified dry shelters with capacity progress bars (`"Masunda High School — 120/400 spots — 450m away"`).
  - Safety advisory status pill (`"Thane Central: WARNING — Rising Water Levels"`).
  - Offline sync status indicator (`"Offline Mode Active — 1 report queued for transmission"`).

### 6.2 Operator Command Centre (`/command`)
- **Philosophy:** Maximum situational control, synchronized dual-view cartography, zero card clutter.
- **Top Bar:** Dual-mode selector (`CURRENT SITUATION` vs `SIMULATION`), scenario selector, overall city risk index, active dispatch count.
- **Central Canvas:** MapLibre 2D GIS and Three.js 3D Hotspot viewport (switchable or synchronized side-by-side).
- **Left Panel (Drawer):** Real-time incident triage feed with explainable priority scores and AI recommendations.
- **Right Panel (Drawer):** CCTV camera stream grid with AI bounding boxes and shelter occupancy gauges.
- **Bottom Shelf:** Master flood simulation scrubber (`PLAY / PAUSE`, `1x / 2x / 5x`, `T+00:00` to `T+12:00`).

### 6.3 Responder Portal (`/responder`)
- **Philosophy:** High-contrast tactical mobile interface designed for sunlight legibility and wet fingers.
- **Primary View:** Active task card with destination pin, route hazard warnings, and big action buttons: `[ ACCEPT ]`, `[ EN ROUTE ]`, `[ ON SCENE ]`, `[ RESCUE COMPLETED ]`.
- **Secondary View:** Turn-by-turn flood-avoidance waypoint list.

### 6.4 Public Information Display (`/display`)
- **Philosophy:** High-contrast broadcast visual clarity for public outdoor billboards and transit concourses.
- **Layout:** High-contrast dark background with high-visibility amber/red alert banners, large typography, live city hazard boundary map, and a scrolling ticker of official Thane Municipal Corporation directives and emergency helpline numbers (`112 / 108`).
