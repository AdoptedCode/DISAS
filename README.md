# DISAS — Disaster Intelligence & Situational Awareness System

[![Prototype Status](https://img.shields.io/badge/Status-Project%20Initialization-blue.svg)](#)
[![Pilot Geography](https://img.shields.io/badge/Geography-San Francisco2C%20-orange.svg)](#)
[![Stack](https://img.shields.io/badge/Stack-Next.js%20%7C%20FastAPI%20%7C%20PostGIS%20%7C%20Three.js-emerald.svg)](#)

**DISAS (Disaster Intelligence & Situational Awareness System)** is an integrated disaster intelligence and emergency response platform built for urban flood simulation, computer-vision threat detection, AI-assisted incident triage, capacity-aware evacuation, and multi-agency operational coordination.

Pilot Geography: **Thane, Maharashtra, India**.

---

## 1. The Four Operational Portals

DISAS delivers a unified single-application platform serving four dedicated operational interfaces:

| Portal | Route | Primary Audience | Key Capabilities |
| :--- | :---: | :--- | :--- |
| **Citizen Portal** | `/citizen` | Residents & evacuees | 1-tap SOS reporting, photo uploads, verified shelter locations, offline survival guidance. |
| **Operator Command Centre** | `/command` | Municipal coordinators & disaster managers | Synchronized 2D GIS & 3D isometric twin, 12h flood simulation, CCTV AI detection grid, incident triage queue, resource dispatch. |
| **Responder Portal** | `/responder` | Rescue teams, boats, ambulance units | Tactical field assignments, flood-aware turn-by-turn routing, live status telemetry, shelter check-ins. |
| **Public Information Display** | `/display` | Outdoor LED screens, transit hubs | Ultra-high contrast read-only directives, live hazard zones, verified shelter capacity, emergency helplines. |

---

## 2. Core Architecture & Visual Direction

- **Visual Language:** *Scientific cartography + editorial geographic visualisation + isometric digital twin + Liquid Glass*.
- **Frontend:** Next.js (App Router), TypeScript, Tailwind CSS, shadcn/ui, MapLibre GL (2D GIS), Three.js (3D Isometric Twin), Dexie.js (IndexedDB offline sync).
- **Backend:** Python 3.11+, FastAPI (asynchronous REST + WebSockets), GeoPandas, Rasterio.
- **Data Layer:** PostgreSQL 16 + PostGIS 3.4 spatial database.
- **Resilience:** Simulated store-and-forward mesh network emulation (BLE / LoRa) with client-side offline queuing.

---

## 3. Project Documentation Directory

All architectural standards, rules, and roadmaps are maintained within the [`docs/`](file:///Users/tejaswi/Documents/DISAS/docs/) directory:

- 📖 **[`docs/DISAS.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS.md)** — Project overview, mission, problem space, pilot geography profile, and system capabilities.
- 🏗️ **[`docs/DISAS-architecture.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS-architecture.md)** — Detailed technical architecture, data flows, subsystem diagrams, and PostGIS database schema.
- 🛡️ **[`docs/DISAS-rules.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS-rules.md)** — AI safety rules, mandatory human-in-the-loop dispatch rules, route safety disclaimers, data veracity labels, and RBAC matrix.
- 🎨 **[`docs/DISAS-design.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS-design.md)** — Visual identity, Liquid Glass specifications, isometric digital twin guidelines, and color tokens.
- 🗺️ **[`docs/DISAS-tasks.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS-tasks.md)** — Staged implementation roadmap across 8 stages, with detailed acceptance criteria and testing checkpoints.
- 🧠 **[`docs/DISAS-memory.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS-memory.md)** — Authoritative record of all product decisions, fixed architectural constraints, and active assumptions.
- 🤖 **[`docs/DISAS-agent-instructions.md`](file:///Users/tejaswi/Documents/DISAS/docs/DISAS-agent-instructions.md)** — Operational protocol, stage gating rules, and coding standards for development agents.

---

## 4. Proposed Repository Structure

```text
DISAS/
├── docs/                             # Project specifications & operational memory
│   ├── DISAS.md                      # Project overview & scope
│   ├── DISAS-architecture.md         # System architecture & DB schema
│   ├── DISAS-rules.md                # Safety rules, AI guardrails & RBAC
│   ├── DISAS-design.md               # Design system & visual specifications
│   ├── DISAS-tasks.md                # 8-stage implementation roadmap
│   ├── DISAS-memory.md               # Product decisions & decision log
│   └── DISAS-agent-instructions.md   # Guidelines for future agent sessions
├── frontend/                         # Next.js 15+ TypeScript application
│   ├── app/                          # App Router pages (/citizen, /command, /responder, /display)
│   ├── components/                   # UI components, Liquid Glass panels, WebGL twins
│   │   ├── ui/                       # Base shadcn primitives
│   │   ├── map/                      # MapLibre 2D GIS components
│   │   ├── twin/                     # Three.js 3D isometric twin components
│   │   └── common/                   # Shared headers, status badges, veracity pills
│   ├── lib/                          # Client API clients, WebSocket hooks, IndexedDB store
│   └── public/                       # Static assets, map styles, 3D textures
├── backend/                          # FastAPI Python backend service
│   ├── api/                          # REST & WebSocket endpoints
│   ├── core/                         # Configuration, security, database sessions
│   ├── models/                       # PostGIS SQLAlchemy & Pydantic models
│   ├── services/                     # Hydrological simulation, AI scoring, CCTV analytics
│   └── data/                         # Thane DEM elevation rasters, geo-fixtures, seed scripts
├── docker-compose.yml                # Local orchestration (PostGIS, Backend, Frontend)
└── README.md                         # Repository index & getting started
```

---

## 5. Development Roadmap Progress

- [x] **Project Initialization:** Master documentation, architecture definitions, safety rules, and workspace roadmap established.
- [ ] **Stage 1:** Project foundation and cinematic animated isometric landing page (*Awaiting User Design*).
- [ ] **Stage 2:** Application shell, demo role-switching & auth architecture, and PostGIS database setup.
- [ ] **Stage 3:** Geographic 2D GIS map and simulation-driven 3D isometric flood digital twin.
- [ ] **Stage 4:** Citizen SOS reporting and synthetic CCTV computer vision analytics.
- [ ] **Stage 5:** Multi-source AI verification and explainable incident prioritisation.
- [ ] **Stage 6:** Dynamic shelters, flood-aware routing, and responder field coordination.
- [ ] **Stage 7:** Simulated resilient offline mesh networking and store-and-forward sync.
- [ ] **Stage 8:** Public information display, impact analytics dashboard, and final polish.
