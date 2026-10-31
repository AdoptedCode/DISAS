# DISAS — Disaster Intelligence & Situational Awareness System

## 1. Executive Summary

**DISAS (Disaster Intelligence & Situational Awareness System)** is an integrated, next-generation disaster response and intelligence platform designed to bridge the gap between predictive simulation, real-time multi-source sensory data, field operational coordination, and public emergency communication.

Built as an end-to-end working hackathon prototype, DISAS addresses the life-critical challenges of urban flooding. It provides municipal disaster managers, emergency responders, and everyday citizens with a shared, synchronized operational picture powered by 2D/3D geospatial simulation, computer-vision intelligence, and explainable AI-assisted incident triage.

---

## 2. Core Identity & Geographic Scope

- **Platform Name:** DISAS
- **Full Title:** Disaster Intelligence & Situational Awareness System
- **Initial Hazard Domain:** Urban Flash Flooding & Monsoon Waterlogging
- **Pilot Geography:** Thane, Maharashtra, India (19.2183° N, 72.9781° E)
  - *Context:* Thane is a dense, rapidly growing metropolitan hub bordered by the Ulhas River estuary, Thane Creek, and the Yeoor Hills. It experiences intense localized monsoon deluges, complex drainage choke points, tidal backwater influences, and high population exposure across diverse urban typologies (high-density informal settlements, low-lying arterial transit corridors, suburban commercial districts, and industrial belts).
- **Long-Term Vision:** An extensible, modular multi-hazard operational system capable of expanding to regional cyclone tracking, industrial chemical incident monitoring, landslide risk evaluation, and deployment across varied global geographies.

---

## 3. The Problem Space

During rapid-onset urban flood events, municipal emergency operations centers face three systemic breakdowns:

1. **Information Asymmetry & Data Silos:**
   - Rain gauge telemetry, municipal CCTV networks, citizen SOS calls, social feeds, and field responder reports operate on completely disconnected systems.
   - Decision-makers suffer from cognitive overload trying to synthesize fragmented spreadsheets, radio chatter, and disparate CCTV video walls.
2. **Delayed & Coarse Simulation:**
   - Traditional hydrological simulation models take hours to run on specialized clusters and output static flood extent maps that fail to incorporate real-time drainage blockages or live storm progression.
3. **Communication Collapse & Uncoordinated Action:**
   - Evacuation orders are issued across broad administrative zones rather than adaptive, risk-prioritized corridors.
   - Cellular tower congestion or power station submersion sever communication when citizens and first responders need it most, lacking offline-resilient store-and-forward mesh topologies.

---

## 4. Key Platform Capabilities

### 4.1 Synchronized 2D GIS & 3D Isometric Digital Twin
- **City-Wide Macro View:** High-performance 2D GIS visualization using MapLibre GL displaying administrative boundaries, watershed basins, road networks, elevation contours, critical infrastructure, and dynamic flood depth heatmaps.
- **Micro Hotspot 3D Twin:** Real-time Three.js isometric rendering for identified vulnerability hotspots (e.g., Vandana Cinema junction, Teen Hath Naka, Ghodbunder lowlands), visualizing elevation-based water pooling, building inundation, traffic bottlenecks, and bridge clearance levels in an intuitive miniature-world aesthetic.
- **Dual Operating Modes:**
  - *Current Situation Mode:* Live operational picture aggregating verified sensor readings, validated citizen reports, active CCTV detections, and deployed units.
  - *Simulation Mode:* Sandbox environment enabling operators to alter rainfall intensity (mm/hr), storm duration, tidal backwater levels, and drainage efficiency to forecast flood progression over a 12-hour timeline with full play/pause, rewind, and speed controls.

### 4.2 Multi-Source Intelligence & Computer Vision
- **CCTV Analytics Architecture:** Modular visual detection pipeline designed for authorized municipal/traffic feed integration in production, running entirely on simulated video feeds and synthetic frame fixtures at synthetic demonstration locations for the prototype. Focuses on flood depth estimation, road passability, vehicle submersion, debris accumulation at culverts, and crowd congregation.
- **Citizen Report Verification:** Ingestion of citizen-submitted incident reports with geolocated photographs, automatically processed for visual flood confirmation, duplicate deduplication, and cross-correlation against nearby CCTV and topography.
- **Explainable Decision Support:** Multi-factor scoring algorithms producing explicit, transparent risk scores, incident severity tiers, shelter suitability ratings, and evacuation route risk indexes with linked supporting evidence.

### 4.3 Capacity-Aware Evacuation & Resource Coordination
- **Dynamic Population Estimation:** Baseline demographic census data combined with dynamic diurnal mobility estimates to calculate populations exposed to rising water.
- **Shelter Allocation Engine:** Intelligent capacity tracking that balances official shelters, operator-approved facilities, and AI-identified candidate safe zones, while strictly requiring human verification before candidate sites are made public.
- **Flood-Aware Evacuation Routing:** Graph-based pathfinding through the street network incorporating dynamic water levels, bridge clearances, and reported road blockages. System provides risk-assessed routes while explicitly avoiding dangerous "guaranteed safe" claims.
- **Human-in-the-Loop Dispatch:** AI resource matching recommends optimal boat, ambulance, and NDRF/SDRF rescue team assignments, but enforces mandatory human operator sign-off for all consequential operational dispatches.

### 4.4 Simulated Resilient Offline Communication
- **Mesh & Gateway Simulation:** Emulation of Bluetooth Low Energy (BLE) peer-to-peer mesh and LoRa long-range gateway communication.
- **Store-and-Forward Client Architecture:** Progressive Web App (PWA) client with IndexedDB local caching, offline incident queuing, vector tile caching, priority message queuing, and automatic bi-directional reconciliation upon network reconnection.

---

## 5. Four Unified Application Interfaces

DISAS unifies four specialized, role-tailored interfaces backed by a single shared API, real-time WebSocket bus, and PostgreSQL/PostGIS spatial database:

```
+-------------------------------------------------------------------------+
|                              DISAS CORE                                 |
|          FastAPI + PostGIS + WebSockets + Intelligence Engines          |
+-------------------+-----------------+-----------------+-----------------+
        |                   |                 |                 |
        v                   v                 v                 v
+---------------+   +---------------+ +---------------+ +---------------+
|    Citizen    |   |   Operator    | |   Responder   | | Public Display|
|    Portal     |   |Command Centre | |    Portal     | |   (Broadcast) |
+---------------+   +---------------+ +---------------+ +---------------+
| * SOS Alert   |   | * 2D/3D Twin  | | * Active Tasks| | * Live Map    |
| * Photo Report|   | * Simulation  | | * Turn Routing| | * Safe Shelters|
| * Safe Shelters|  | * CCTV Matrix | | * Field Status| | * Evac Zones  |
| * Offline Sync|   | * AI Dispatch | | * Offline Sync| | * High-Contrast|
+---------------+   +---------------+ +---------------+ +---------------+
```

1. **Citizen Portal (`/citizen`):**
   - Clean, accessible, mobile-first interface for residents of Thane.
   - One-tap SOS reporting with geolocation and photographic evidence upload.
   - Real-time advisory level, nearest verified safe shelters, and offline-cached survival guides.
2. **Operator Command Centre (`/command`):**
   - Professional dual-pane tactical workstation for municipal emergency coordinators.
   - Synchronized 2D GIS and 3D isometric twin, simulation timeline controls, CCTV monitoring grid with AI bounding boxes, incident triage queue, and resource dispatch management.
3. **Responder Portal (`/responder`):**
   - Field-optimized, high-contrast, tactile interface for rescue teams, ambulance drivers, and disaster volunteers.
   - Task assignment queue, flood-aware turn-by-turn guidance, field status telemetry (en route, on scene, evacuated), and shelter intake check-in.
4. **Public Information Display (`/display`):**
   - High-visibility, low-latency, read-only broadcast dashboard designed for public digital billboards, transit station screens, and municipal lobby monitors.
   - Clear official directives, verified shelter capacities, critical zone warnings, and emergency helpline details.

---

## 6. Pilot Geography: Thane Profile

| Parameter | Specification |
| :--- | :--- |
| **Coordinates** | 19.2183° N, 72.9781° E |
| **Elevation Range** | 2 meters (Creek waterfront) to 120+ meters (Yeoor Foothills) |
| **Water Bodies** | Thane Creek, Ulhas River, Masunda Lake, Upvan Lake, Kachrali Lake |
| **Monsoon Inundation Hotspots** | Vandana Cinema / Naupada, Teen Hath Naka, Kalwa Bridge approaches, Diva East, Ghodbunder Highway underpasses |
| **Critical Transport Nodes** | Thane Central Railway Station, Eastern Express Highway (EEH), Ghodbunder Road (SH 42) |
| **Administrative Body** | Thane Municipal Corporation (TMC) / District Disaster Management Authority (DDMA) |

---

## 7. Hackathon Prototype Scope & Delivery Principles

1. **Complete End-to-End Workflow:** The prototype prioritizes a connected, authentic user journey—from citizen flood report submission and CCTV visual detection, to operator simulation, AI-assisted dispatch, responder execution, and public notification.
2. **Realistic Simulated Infrastructure:** External hardware and municipal dependencies (physical CCTV camera streams, live LoRa radios, commercial drones) are simulated with authentic data schemas, synthetic video/image fixtures at synthetic demonstration locations, and network latency/failure injection. No live connection or access to real municipal CCTV infrastructure is used or implied.
3. **Simulation-Driven 3D Digital Twin:** The 3D isometric twin is directly coupled to and driven by the hydrological grid simulation calculations (elevation, rainfall inputs, drainage, and water depth matrices), not arbitrary independent animations.
4. **No Fabricated Capabilities:** Any metric generated by simulation is clearly tagged as `SIMULATED`; any heuristic estimate is marked `ESTIMATED`; only confirmed human or physical sensor inputs are marked `MEASURED`.
5. **Deterministic Local Execution:** The complete prototype runs predictably via local Docker environments or unified local dev commands without mandating external cloud services or billable APIs.
