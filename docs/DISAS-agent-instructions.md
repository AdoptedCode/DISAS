# DISAS — Agent Development Guidelines & Instructions

This document provides mandatory operational instructions for any AI assistant or autonomous development agent working within the **DISAS (Disaster Intelligence & Situational Awareness System)** codebase.

---

## 1. Operating Principles & Agent Behaviour

1. **Autonomous Execution on Approved Scope:** Work autonomously on tasks within the currently approved development stage. Do not prompt the user to manually write boilerplate or implement code that you can generate and test.
2. **Strict Stage Gating:** Do not skip ahead or implement features belonging to future stages without explicit authorization from the Product Owner (the user). Refer to `docs/DISAS-tasks.md` for stage boundaries.
3. **Preserve Architectural Memory:** Treat `docs/DISAS-memory.md` as the authoritative record. When new architectural choices or user decisions occur, record them in the change log. Never silently reverse or alter previously established decisions.
4. **No Placeholders or Fake Interfaces:** When implementing a stage, build genuine, functioning code with working state and data contracts. Do not generate stubbed "mock UI" buttons that do nothing or fabricate non-functional prototypes.
5. **Rigorous Verification:** Test code before claiming completion. Run tests, verify bundle builds, and inspect runtime outputs. Never fabricate or assume test results.
6. **Cost & Dependency Control:** Do not introduce paid APIs, cloud services, or large unvetted third-party libraries without presenting the trade-offs and obtaining user approval first.

---

## 2. Session Startup Checklist

At the beginning of any session or task:

1. **Read Core Documentation:**
   - Review `docs/DISAS.md` for project scope.
   - Review `docs/DISAS-memory.md` for recent decisions and active assumptions.
   - Review `docs/DISAS-tasks.md` to identify the active stage and specific acceptance criteria.
   - Review `docs/DISAS-rules.md` to ensure safety and ethical guardrails are respected.
2. **Inspect Workspace State:** Check `git status`, test suite status, and verify the local environment before writing code.
3. **Clarify Ambiguities:** If user requirements are underspecified or contradict existing architecture, flag the ambiguity explicitly rather than making unverified assumptions.

---

## 3. Coding & Engineering Standards

### 3.1 Frontend (Next.js & TypeScript)
- Use the Next.js App Router paradigm (`app/` directory).
- Enforce strict TypeScript typing (`noImplicitAny: true`). Avoid `any` types; define typed interfaces for all geospatial and incident payloads.
- Adhere to the Liquid Glass design system defined in `docs/DISAS-design.md`. Use Tailwind utility classes with consistent opacity, backdrop blur, and border tokens.
- Keep components modular, accessible, and performant. Ensure WebGL canvases (Three.js and MapLibre) handle window resizing and proper memory cleanup on unmount.

### 3.2 Backend (FastAPI & Python)
- Organize endpoints under `api/v1/` with Pydantic schemas for request validation and response models.
- Implement asynchronous endpoint handlers (`async def`) for database queries and WebSocket broadcasting.
- Utilize SQLAlchemy 2.0+ / GeoAlchemy2 or SQLModel for database interactions with PostGIS spatial types.
- Ensure all calculation engines (hydrological simulation, priority scoring) are covered by automated unit tests in `tests/`.

### 3.3 Documentation & Codebase Hygiene
- Maintain docstrings and comments on all non-trivial algorithmic logic (e.g., cell water propagation, priority score weighting).
- Preserve existing comments and docstrings when modifying files.
- Ensure file paths, URLs, and code symbols in responses and documentation are formatted as clickable links (`file:///...`).

---

## 4. Stage Transition Protocol

When completing a development stage:

1. Verify that all **Functional Acceptance Criteria** for the stage in `docs/DISAS-tasks.md` have been met.
2. Run all unit and integration tests for the stage. Ensure zero failing tests.
3. Update `docs/DISAS-tasks.md` marking the stage as completed.
4. Update `docs/DISAS-memory.md` recording any technical details, constants, or architectural refinements introduced during the stage.
5. Present a concise summary of deliverables to the user and request explicit authorization before proceeding to the next stage.
