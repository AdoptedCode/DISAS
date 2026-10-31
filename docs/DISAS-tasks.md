# DISAS — Implementation Roadmap, Tasks & Acceptance Criteria

This document defines the structured 8-stage implementation roadmap for building the **DISAS (Disaster Intelligence & Situational Awareness System)** hackathon prototype. Every stage must satisfy its defined acceptance criteria, pass functional test suites, update project documentation, and be signed off at a designated review checkpoint before proceeding to the subsequent stage.

---

## Stage Overview

| Stage | Focus Area | Status |
| :---: | :--- | :---: |
| **Stage 1** | Project Foundation, Workspace Setup & Cinematic Landing Page | *Current Stage (Awaiting User Design)* |
| **Stage 2** | Application Shell, Authentication, Role Routing & Spatial Database | Queued |
| **Stage 3** | Geographic 2D GIS & 3D Isometric Flood Simulation | Queued |
| **Stage 4** | Citizen SOS Reporting & CCTV Computer Vision Analytics | Queued |
| **Stage 5** | Multi-Source AI Verification & Incident Prioritisation | Queued |
| **Stage 6** | Dynamic Shelters, Evacuation Routing & Responder Coordination | Queued |
| **Stage 7** | Simulated Resilient Offline Mesh & Store-and-Forward Sync | Queued |
| **Stage 8** | Public Information Display, Impact Dashboard & Final Polish | Queued |

---

## Detailed Stage Specifications

### Stage 1: Project Foundation & Cinematic Landing Page
- **Primary Objective:** Establish the monorepo/workspace scaffolding, Next.js frontend, Tailwind CSS design system with Liquid Glass primitives, and create the cinematic animated isometric landing page.
- **Key Deliverables:**
  - Workspace configuration: TypeScript, Tailwind CSS, shadcn/ui components, ESLint, Prettier.
  - Design system tokens implemented: Liquid glass panel styles, cartographic pastel palette, alert color variables.
  - Cinematic Landing Page (`/`):
    - Hero section with animated Three.js isometric diorama of Thane (waterfront + city blocks).
    - Storytelling narrative introducing the 4 operational interfaces.
    - Role selector / entry points to the 4 portals.
    - Performance-optimized WebGL canvas with fallback for lower-spec devices.
- **Acceptance Criteria:**
  - Responsive across desktop, tablet, and mobile displays without horizontal scroll overflow.
  - Three.js isometric hero renders at a consistent 60fps on modern browsers with smooth camera drift/rotation.
  - Interactive navigation links route cleanly to `/citizen`, `/command`, `/responder`, and `/display`.
- **Testing Requirements:**
  - Automated Playwright/Vitest smoke test verifying route navigation and WebGL canvas mounting.
  - Lighthouse performance score $\ge 85$.
- **Review Checkpoint:** User review of landing page aesthetics, animation fluidity, and design alignment.

---

### Stage 2: Application Foundation, Authentication & Spatial Database
- **Primary Objective:** Build the shared application shell, demo authentication & role-switching architecture (distinguished from production IAM), and FastAPI backend with PostgreSQL/PostGIS.
- **Key Deliverables:**
  - Backend project setup (`backend/`): FastAPI, SQLAlchemy/SQLModel, Alembic migrations, Pydantic schemas.
  - PostgreSQL + PostGIS Docker container configured with spatial extensions enabled.
  - Seed migration for Thane geographic bounding box, administrative wards, and baseline DEM metadata.
  - Role-based routing guard in Next.js for Citizen, Operator, Responder, and Public roles.
  - 1-click Demo Role Switcher in application header/drawer allowing instant frictionless evaluation across all 4 portals.
  - Documented production authentication migration path (OIDC/SAML/MeriPehchan + MFA).
  - Shared WebSocket event connection and heartbeat handling.
- **Acceptance Criteria:**
  - FastAPI server starts cleanly and serves interactive OpenAPI docs at `/docs`.
  - Database schema initializes tables: `flood_zones`, `shelters`, `incidents`, `responder_units`, `operational_audit_log`.
  - Demo role switcher enables immediate client-side and session switching between all 4 interfaces without credential prompts.
- **Testing Requirements:**
  - Backend unit tests via `pytest` for healthcheck, database connection, and auth token generation.
  - PostGIS spatial query test (verifying `ST_Contains` and distance queries).
- **Review Checkpoint:** Verification of local dev container startup and database connectivity.

---

### Stage 3: Geographic 2D Map & 3D Isometric Flood Simulation
- **Primary Objective:** Implement the synchronized 2D MapLibre GIS map and 3D Three.js isometric twin, directly driven by the hydrological grid simulation calculations.
- **Key Deliverables:**
  - Terrain Elevation Ingestion: Evaluation of open DEM sources (Copernicus GLO-30 / NASA SRTM 30m resampled to 10m/15m) with automatic fallback to pre-packaged synthetic calibrated DEM for zero-dependency offline execution.
  - MapLibre GL 2D map component displaying Thane roads, water bodies, and elevation contours.
  - Three.js 3D isometric twin component focusing on key Thane hotspots (e.g., Vandana Cinema / Teen Hath Naka).
  - Hydrological flood simulation engine (FastAPI):
    - Grid-based cellular water propagation model driven by rainfall intensity (mm/hr), storm duration, and drainage capacity.
    - Mathematical calculation of water surface elevation: $W(x, y, t) = Z_{\text{terrain}}(x, y) + h(x, y, t)$.
    - 12-hour simulation timeline generator outputting 15-minute isobath frames and depth matrices.
  - Simulation-Twin Coupling: Three.js water mesh vertices directly sample $W(x, y, t)$, rising realistically in depressions and underpasses.
  - Interactive timeline scrubber widget: Play/pause, speed controls (1x, 2x, 5x), and timestep jump.
  - Dual Operating Mode toggle: `Current Situation` vs `Simulation`.
- **Acceptance Criteria:**
  - Adjusting rainfall parameters dynamically updates flood isobaths on both 2D map and 3D isometric twin.
  - 3D water surface elevation strictly corresponds to simulation grid outputs; no disconnected or arbitrary water animations.
  - Flood risk stages (Normal, Watch, Warning, Critical) render with exact agreed colors.
  - Both views remain locked to identical simulation timesteps.
  - Veracity badges clearly identify all simulated layers as `SIMULATED`.
- **Testing Requirements:**
  - Hydrological calculation unit tests (mass conservation: runoff + infiltration + drainage = rainfall volume).
  - Component integration tests verifying timeline scrub updates state correctly across 2D and 3D engines.
- **Review Checkpoint:** Demonstration of 2D/3D synchronized simulation playback.

---

### Stage 4: Citizen SOS Reporting & CCTV Computer Vision Analytics
- **Primary Objective:** Implement citizen emergency reporting with image uploads and synthetic CCTV camera intelligence feeds at demonstration locations.
- **Key Deliverables:**
  - Citizen Portal reporting workflow: 1-tap SOS, geolocation capture, category selection, and photo upload.
  - Synthetic CCTV Network:
    - 6 pre-configured synthetic CCTV camera feeds situated at synthetic demonstration locations along key Thane choke points.
    - Explicit UI labeling: `[ SYNTHETIC DEMONSTRATION FEED ]` with zero connection or claim of access to real municipal/police CCTV.
    - Computer vision detection overlay rendering simulated bounding boxes for water accumulation, stalled vehicles, and stranded persons.
  - Spatial incident clustering engine grouping nearby reports within a 250m radius.
- **Acceptance Criteria:**
  - Citizen report submitted on mobile immediately generates an incident record in PostGIS and emits a WebSocket event.
  - Command Centre CCTV grid displays synthetic demonstration loops with synchronized AI detection overlays and confidence percentages.
  - PII guardrails strictly mask phone numbers and full street addresses on public layers.
- **Testing Requirements:**
  - End-to-end test of citizen report creation to WebSocket broadcast.
  - Synthetic CCTV stream fixture verification test.
- **Review Checkpoint:** Visual review of CCTV detection interface and incident ingestion speed.

---

### Stage 5: Multi-Source AI Verification & Incident Prioritisation
- **Primary Objective:** Create the explainable AI incident triage, deduplication, and priority scoring engine.
- **Key Deliverables:**
  - Cross-source correlation engine linking citizen photos with adjacent CCTV detections and flood depths.
  - Explainable Priority Scoring formula combining water depth, population exposed, hospital/infrastructure proximity, and corroboration count.
  - Interactive Command Centre triage queue:
    - Filterable by severity (`CRITICAL`, `HIGH`, `MEDIUM`, `LOW`).
    - Factor breakdown modal explaining the mathematical scoring logic and supporting evidence.
  - Human approval workflow for incident verification and status progression.
- **Acceptance Criteria:**
  - Corroborated incidents automatically raise confidence score ($\ge 0.85$) and adjust priority queue ranking.
  - No incident is dispatched without explicit operator confirmation button click.
  - Audit trail logs every operator triage action with timestamp and user ID.
- **Testing Requirements:**
  - Unit tests for priority scoring algorithm with deterministic edge-case fixtures.
  - Audit log immutability verification test.
- **Review Checkpoint:** Walkthrough of AI triage explanation panel and operator decision flow.

---

### Stage 6: Dynamic Shelters, Evacuation Routing & Responder Coordination
- **Primary Objective:** Build dynamic shelter capacity tracking, flood-aware turn-by-turn routing, and the Responder field portal.
- **Key Deliverables:**
  - Shelter management module:
    - Tracking total capacity, assigned evacuees, and confirmed arrivals.
    - Handling `OFFICIAL`, `OPERATOR_APPROVED`, and `AI_CANDIDATE` shelters.
    - Operator approval workflow to promote AI candidate shelters to official status.
  - Flood-Aware Turn-by-Turn Routing Engine:
    - Graph-based pathfinding across Thane road network penalized by live flood depths.
    - Prohibition of "guaranteed safe" language; insertion of mandatory safety caveats.
  - Responder Portal (`/responder`):
    - Mobile-first task execution queue for rescue teams (accept task, route guidance, mark on-scene, confirm rescued count).
- **Acceptance Criteria:**
  - Routing engine automatically avoids flooded road segments exceeding 45 cm depth.
  - Routes display risk level with explicit caution caveats.
  - Responder task status transitions update the Command Centre in real time.
- **Testing Requirements:**
  - Pathfinding unit test verifying edge avoidance when flood depth exceeds threshold.
  - Shelter capacity boundary tests (preventing over-allocation).
- **Review Checkpoint:** Live simulation of incident creation $\rightarrow$ dispatch $\rightarrow$ responder route navigation $\rightarrow$ shelter arrival.

---

### Stage 7: Simulated Resilient Offline Communication
- **Primary Objective:** Emulate mesh network resilience and offline store-and-forward synchronization for disconnected conditions.
- **Key Deliverables:**
  - Client-side IndexedDB report queue via Dexie.js for Citizen and Responder portals.
  - Simulated Bluetooth mesh / LoRa gateway state machine with controllable network degradation toggles:
    - `Normal Connectivity (4G/Wi-Fi)`
    - `Degraded (High Latency / Packet Drop)`
    - `Complete Network Partition (Offline Mesh Mode)`
  - Background synchronization manager with deterministic conflict resolution (vector clocks / latest operator update priority).
- **Acceptance Criteria:**
  - When disconnected, citizen reports and responder field status updates persist locally in IndexedDB without data loss.
  - Clear UI indicator states: `"Simulated Mesh Network Active — N items queued"`.
  - Upon network restoration, all queued messages sync to backend in priority order; conflict resolutions are logged cleanly.
- **Testing Requirements:**
  - Offline mode unit tests simulating disconnected network events and subsequent flush upon reconnect.
- **Review Checkpoint:** Demonstration of end-to-end report creation during simulated cellular blackout followed by automated recovery sync.

---

### Stage 8: Public Information Display, Impact Dashboard & Final Polish
- **Primary Objective:** Construct the high-visibility public broadcast screen, comprehensive disaster impact analytics, and overall UX polish.
- **Key Deliverables:**
  - Public Information Display (`/display`):
    - Fullscreen, ultra-high contrast dark theme for public outdoor LED screens and transit hubs.
    - Real-time advisory ticker, verified shelter availability cards, critical hazard zones, and emergency helplines.
  - Command Centre Impact Analytics:
    - Aggregate metrics: Total estimated population at risk, citizens safely evacuated, active rescue teams deployed, road kilometers submerged.
  - Performance optimization: Tile caching, asset compression, Three.js geometry instancing.
  - Final documentation update and presentation walkthrough guide.
- **Acceptance Criteria:**
  - Public Display updates autonomously via WebSockets without manual refresh.
  - System operates smoothly across all interfaces simultaneously without CPU lockup.
  - End-to-end demo script documented and validated.
- **Testing Requirements:**
  - End-to-end system regression test across all 4 portals.
  - Bundle size and asset audit.
- **Review Checkpoint:** Final prototype review and user acceptance sign-off.
