# DISAS — Product Memory, Decisions & Architectural Constraints

This document serves as the authoritative single source of truth for all validated product decisions, architectural commitments, system constraints, unverified assumptions, and data discovery protocols for the **DISAS (Disaster Intelligence & Situational Awareness System)** project.

---

## 1. The 60 Validated Product Decisions

### Domain 1: Product Identity, Purpose & Geographic Scope
- **D01:** Platform name is officially **DISAS** (Disaster Intelligence & Situational Awareness System).
- **D02:** Initial hazard domain is strictly **Urban Flood Intelligence and Emergency Coordination**.
- **D03:** Pilot geography is **Thane, Maharashtra, India** (encompassing Thane Municipal Corporation jurisdiction, Thane Creek, and the Ulhas River estuary).
- **D04:** Long-term platform vision is an extensible multi-hazard disaster intelligence architecture supporting regional expansion and multi-disaster scenarios.
- **D05:** Product delivery priority is an **end-to-end working hackathon prototype** with genuine data flow, deterministic execution, and realistic simulations.

### Domain 2: Four Unified Application Interfaces
- **D06:** Single unified codebase and shared backend serving all four role-specific operational interfaces.
- **D07:** **Citizen Portal (`/citizen`):** Flood warnings, 1-tap SOS emergency reporting, photo upload, location sharing, verified shelter availability, and offline survival guides.
- **D08:** **Operator Command Centre (`/command`):** Geographic intelligence, synchronized 2D GIS and 3D isometric twin, 12h flood simulation controls, CCTV AI detection grid, incident triage queue, and resource dispatch management.
- **D09:** **Responder Portal (`/responder`):** Field-optimized mobile tactical assignments, flood-aware turn-by-turn routing, live status updates (en route, on scene, evacuated), and shelter check-in.
- **D10:** **Public Information Display (`/display`):** Fullscreen, high-contrast read-only broadcast display for public transit hubs and digital billboards showing official directives, verified shelter capacities, and emergency helplines.

### Domain 3: Hydrological Flood Simulation Subsystem
- **D11:** City-wide geographic coverage across the entire Thane municipal administrative area in 2D GIS.
- **D12:** Detailed 3D isometric simulation focused on selected vulnerability hotspots (e.g., Teen Hath Naka, Vandana Cinema basin, Kalwa bridgehead).
- **D13:** Terrain and elevation-based cellular grid simulation modeling infiltration, surface runoff, and gravitational water propagation.
- **D14:** Dynamic operator controls for rainfall intensity (mm/hr), storm duration, soil infiltration rates, and drainage/tidal efficiency.
- **D15:** Incorporation of historical monsoon flood profiles to inform initial conditions and runoff coefficients.
- **D16:** Full temporal simulation scrubber controls: Play, Pause, Rewind, and Speed multipliers (1x, 2x, 5x) across a 12-hour projection timeline.
- **D17:** Standardized risk-stage classification: `Normal` (<15cm), `Watch` (15–45cm), `Warning` (45–90cm), and `Critical` (>90cm).

### Domain 4: Isometric Digital Twin & Visual Simulation Coupling
- **D18:** Three.js-powered orthographic/isometric diorama aesthetic balancing scientific precision with clean miniature-city visualization.
- **D19:** Procedural terrain generation integrating roads, buildings, bridges, vegetation, and water bodies.
- **D20:** **Direct Simulation Coupling:** 3D flood visualization must be mathematically driven by simulation output ($W(x, y, t) = Z_{\text{terrain}}(x, y) + h(x, y, t)$); arbitrary aesthetic water elevation is strictly forbidden.
- **D21:** Full camera interaction controls: smooth rotation, zoom, tilt, and focal hotspot selection.
- **D22:** Dual visual levels: simplified macro city-wide view in 2D MapLibre and detailed micro hotspot rendering in 3D Three.js.
- **D23:** Absolute performance mandate: visual complexity must never compromise 60fps usability or client responsiveness.

### Domain 5: Multi-Source Intelligence & Computer Vision Analytics
- **D24:** CCTV analytics architecture designed for authorized municipal/traffic feed integration in production.
- **D25:** **Synthetic Demonstration Locations:** The prototype operates strictly with simulated feeds and synthetic fixtures at demonstration coordinates; no tap or connection to real municipal/police CCTV is used or implied.
- **D26:** Automated visual feature detection targeting water depth estimation, stalled vehicles, road passability, crowd clusters, and drain blockages.
- **D27:** Ingestion and automated verification of citizen-submitted incident photos.
- **D28:** Multi-source incident correlation linking citizen reports with nearby CCTV detections and topography within a 250m radius.
- **D29:** Transparent, explainable AI scoring for incident severity, priority score, shelter suitability, and route risk.
- **D30:** AI recommendations must explicitly cite supporting evidence, factor weights, and constituent sensor/report IDs.

### Domain 6: Population, Shelters & Evacuation Routing
- **D31:** Baseline demographic and ward census data utilized to estimate population exposure in flooded sectors.
- **D32:** Dynamic population simulation accounting for diurnal commuting patterns when live observations are unavailable.
- **D33:** Explicit distinction between statistical estimates (`ESTIMATED`) and confirmed physical counts (`MEASURED`).
- **D34:** Three-tier shelter taxonomy: `OFFICIAL`, `OPERATOR_APPROVED`, and `AI_CANDIDATE`.
- **D35:** **Shelter Gating Rule:** AI-identified candidate safe zones must NEVER automatically publish as verified shelters without human operator approval.
- **D36:** Capacity-aware shelter allocation tracking assigned evacuees separately from confirmed arrivals.
- **D37:** Flood-aware routing that avoids impassable submerged roads (>45cm) and strictly appends safety caveats (**NEVER** claim routes are "guaranteed safe").

### Domain 7: Incidents & Resource Coordination
- **D38:** Citizen reporting workflow capturing geolocation coordinates, hazard category, description, and photographs.
- **D39:** Corroboration-driven dynamic incident confidence and priority scoring.
- **D40:** Full incident lifecycle management (`REPORTED` $\rightarrow$ `VERIFIED` $\rightarrow$ `DISPATCHED` $\rightarrow$ `IN_PROGRESS` $\rightarrow$ `RESOLVED` / `REJECTED`).
- **D41:** Management of emergency assets: NDRF/SDRF rescue boats, ambulances, evacuation buses, civic de-watering pumps.
- **D42:** AI-assisted resource matching proposing optimal unit dispatch based on proximity, asset capability, and incident severity.
- **D43:** **Mandatory Human-in-the-Loop:** Dispatch actions require explicit authenticated operator sign-off; immutable operational audit logging is enforced.

### Domain 8: Resilient Offline Simulation
- **D44:** Simulation of peer-to-peer Bluetooth Low Energy (BLE) mesh and long-range LoRa gateway communication.
- **D45:** Simulated connectivity degradation toggles (normal 4G/Wi-Fi, packet drop/latency, total network partition).
- **D46:** Client-side store-and-forward architecture using browser IndexedDB (Dexie.js) for queuing reports and caching tiles.
- **D47:** Priority-ordered message syncing, cryptographic UUIDs, vector clock tracking, and deterministic conflict resolution upon reconnection.
- **D48:** Clear, unambiguous UI labelling identifying offline networking as **"Simulated Mesh Network"**.

### Domain 9: Design System & Visual Hierarchy
- **D49:** Unified aesthetic across all interfaces: *Scientific cartography + editorial geographic visualisation + isometric digital twin + Liquid Glass*.
- **D50:** Restrained pastel environmental color palette (slate lowlands, muted granite highlands, calm azure lakes).
- **D51:** Floating Liquid Glass UI panels (`backdrop-blur-md`, subtle specular borders, semi-translucent dark slate surfaces).
- **D52:** Clear typographic hierarchy using neutral modern sans-serif paired with tabular monospaced numerals for coordinates and telemetry.
- **D53:** Uncompromising legibility for safety alerts (`Normal`: Emerald, `Watch`: Amber, `Warning`: Orange, `Critical`: Crimson).
- **D54:** UX principle: "Map first, intelligence on demand" with progressive disclosure.

### Domain 10: Technology, Infrastructure & Governance
- **D55:** Frontend stack: Next.js 15 (App Router) + TypeScript + Tailwind CSS + shadcn/ui.
- **D56:** Backend stack: Python 3.11+ + FastAPI (async REST + WebSockets) + GeoPandas + Rasterio.
- **D57:** Spatial Database: PostgreSQL 16 + PostGIS 3.4.
- **D58:** Containerization & Hosting: Local Docker Compose environment; Railway as optional cloud hosting backend.
- **D59:** Zero billable cloud dependency mandate: Core demonstration operates reliably with pre-packaged local datasets.
- **D60:** **Authentication Architecture:** Prototype features an instant 1-click Demo Role Switcher for frictionless evaluation, while architecture formally documents production federated government SSO (OIDC/SAML/MeriPehchan) and MFA.

---

## 2. Unverified Assumptions & Discovery Protocols

| Assumption | Category | Status | Evaluation, Discovery & Fallback Protocol |
| :--- | :--- | :--- | :--- |
| **10m Thane DEM** | Geospatial Elevation | *Unverified Assumption* | **Data Discovery Protocol:**<br>1. Evaluate **Copernicus DEM GLO-30** (30m, open under European Space Agency Copernicus license, high vertical accuracy ~2m).<br>2. Evaluate **NASA SRTM GL1** (30m, public domain).<br>3. Review **Survey of India / Bhuvan** open data terms.<br>**Licensing & Attribution:** Attribute ESA/Copernicus or NASA appropriately.<br>**Resolution:** Resample 30m raster to 10m/15m grid cells via bilinear interpolation for simulation.<br>**Zero-Dependency Fallback:** If open DEM download or license compliance fails during offline setup, automatically activate pre-packaged **Synthetic Calibrated DEM** in `backend/data/` calibrated to Thane's known geographic landmarks (Yeoor 120m $\rightarrow$ Creek 2m). |
| **CCTV Feeds** | Video & Analytics | *Synthetic Demonstration Only* | All camera feeds are synthetic fixtures at demonstration coordinates. No live access to Thane Municipal Corporation or Traffic Police feeds is implied or used. Feeds are clearly labelled in UI as `[ SYNTHETIC DEMONSTRATION FEED ]`. |
| **Authentication Model** | Identity & Access | *Demo Role Switcher* | Prototype uses a 1-click demo role switcher (Citizen, Operator, Responder, Public Display) to enable immediate evaluator inspection. Production federated IAM/SSO is documented as a future deployment target. |
| **Landing Page Design** | UI / Presentation | *Pending User Input* | Awaiting explicit user design guidelines, storytelling copy, and layout preferences before initiating Stage 1 implementation. |

---

## 3. Decision Change Log

| Date | Author / Agent | Decision Summary | Rationale |
| :--- | :--- | :--- | :--- |
| 2026-10-31 | Master Initializer | Established initial architecture, stack, 4-portal taxonomy, and 8-stage roadmap. | Initial kickoff. |
| 2026-10-31 | Lead Architect | 1. Reframed 10m DEM as unverified assumption with discovery/fallback protocol.<br>2. Explicitly designated CCTV feeds as synthetic demonstration fixtures only.<br>3. Formally distinguished demo role switching from production government SSO.<br>4. Codified simulation-twin mathematical coupling ($W = Z + h$).<br>5. Enumerated all 60 core product decisions. | Product Owner refinement request and precision alignment. |
