# DISAS — Operational Rules, Safety Guardrails & Governance

## 1. Principles of Operational Safety

DISAS is designed to operate in high-stakes emergency conditions where automated errors can have direct physical consequences. The system enforces strict architectural and behavioural boundaries to balance artificial intelligence assistance with human accountability.

---

## 2. Consequential Action & Autonomous Decision Restrictions

### Rule 2.1: Mandatory Human-in-the-Loop for Dispatch
- **Rule:** The system, including any AI heuristic or background intelligence engine, **SHALL NEVER** autonomously dispatch emergency rescue teams, boats, ambulances, or police assets.
- **Implementation:**
  - AI engines may generate ranked recommendations (e.g., *"Recommend NDRF Boat #2 based on 4.2 min ETA and 6-person capacity"*).
  - Every dispatch action requires an explicit, authenticated confirmation click from an Operator in the Command Centre.
  - All dispatch confirmations are permanently recorded in the `operational_audit_log` with user ID, timestamp, and the recommendation context presented at the time of decision.

### Rule 2.2: Prohibition of Automatic Shelter Designation
- **Rule:** AI-identified safe structures or dry high-elevation zones **SHALL NEVER** be automatically published as verified shelters to the Citizen Portal or Public Information Display.
- **Implementation:**
  - Structures detected as dry or elevated by elevation analysis or satellite/CCTV telemetry are categorized strictly as `AI_CANDIDATE`.
  - Only authorized Municipal Operators or Field Responders can promote an `AI_CANDIDATE` to `OPERATOR_APPROVED` or `OFFICIAL` status after confirming structural integrity, accessibility, and sanitation/security readiness.

### Rule 2.3: Route Safety & Evacuation Guidance Restrictions
- **Rule:** The system **SHALL NEVER** describe any evacuation path, road corridor, or exit route as *"Guaranteed Safe"*, *"100% Secure"*, or *"Cleared"*.
- **Implementation:**
  - All routing outputs must be termed *"Assessed Lowest-Risk Route"* or *"Current Evaluated Route"*.
  - Every routing payload and user interface display must prominently append the standard safety caveat:
    > *"Route assessed based on available telemetry and simulation. Local flash flooding, manhole displacement, fallen power lines, or sudden surge may occur. Exercise visual caution on site."*
  - If a route's risk score exceeds acceptable thresholds (e.g., crossing known submergence zones > 30 cm), the UI must display a prominent warning banner advising users to seek immediate vertical refuge if mobility is impaired.

---

## 3. Data Veracity & Provenance Labelling

### Rule 3.1: Strict Provenance Classification
Every piece of data displayed across all four DISAS interfaces must carry an explicit veracity label:

| Veracity Tag | Definition | Examples | UI Treatment |
| :--- | :--- | :--- | :--- |
| **`MEASURED`** | Verified physical sensor telemetry, confirmed official municipal rain gauge, or verified field responder ground observation. | Physical IMD rain gauge, ultrasound water depth sensor at Masunda Lake. | Solid green indicator; badge: `Verified Measurement` |
| **`ESTIMATED`** | Algorithmic approximations derived from demographic models, historical census extrapolation, or mobile density proxies. | Estimated 350 residents in inundated ground-floor slum tenements. | Amber dashed border; badge: `Statistical Estimate` |
| **`SIMULATED`** | Synthetic predictive outputs computed by the hydrological physics engine or scenario sandbox. | Forecast flood depth at T+3h under 100 mm/hr cloudburst. | Violet pulsing badge; watermarked: `Simulated Projection` |
| **`UNAVAILABLE`** | Missing, obstructed, or unverified data where sensors are offline or telemetry has timed out. | CCTV camera offline due to power severance at Diva Substation. | Neutral grey hatched pattern; badge: `Telemetry Offline` |

### Rule 3.2: Dual Operating Mode Segregation
- The Operator Command Centre must distinctly toggle between **Current Situation Mode** and **Simulation Mode**.
- When in **Simulation Mode**, the interface header must display a persistent, high-contrast banner:
  `[ SIMULATION MODE ACTIVE — DISPLAYING HYPOTHETICAL FLOOD PROJECTION ]`
- Simulated data must never bleed into the active Citizen Portal alert feed or Public Information Display.

### Rule 3.3: Mandatory Simulation-Coupling for 3D Flood Visualisation
- **Rule:** The 3D isometric digital twin's water surface geometry **SHALL ALWAYS** be computationally bound to the hydrological simulation engine's depth and water surface elevation calculations $W(x, y, t) = Z_{\text{terrain}}(x, y) + h(x, y, t)$.
- **Implementation:**
  - Water planes must not be animated via disconnected aesthetic timers or arbitrary height offsets.
  - Inundation progression must explicitly reflect terrain contours, rainfall inputs, soil infiltration, and drainage parameters.
  - When switching simulation scenarios or scrubbing the timeline, the 3D water mesh must update deterministically in lockstep with the 2D GIS isobath layers.

---

## 4. Explainable AI & Auditability Standards

### Rule 4.1: Explicit Factor Decomposition
No AI recommendation (e.g., incident severity ranking, priority score, shelter allocation) shall be presented as an opaque score or "black box" number.
- Every score must expose its constituent factors:
  - Depth weighting ($w_{\text{depth}}$)
  - Vulnerability / Demographic exposure ($w_{\text{pop}}$)
  - Critical infrastructure proximity ($w_{\text{infra}}$)
  - Time elapsed since first report ($w_{\text{time}}$)
  - Source corroboration count ($w_{\text{source}}$)
- Hovering or expanding any AI badge in the UI must display the natural-language explanation and supporting sensor or report IDs.

### Rule 4.2: Immutable Operational Audit Logging
- Every operational state transition must write an immutable record to `operational_audit_log`.
- Logged actions include:
  - Incident status change (e.g., `REPORTED` $\rightarrow$ `VERIFIED` $\rightarrow$ `DISPATCHED` $\rightarrow$ `RESOLVED`).
  - Resource dispatch approvals and overrides of AI recommendations.
  - Shelter activation, capacity updates, and closure.
  - Broadcast of emergency warnings or evacuation directives.

---

## 5. Privacy, PII & Surveillance Guardrails

### Rule 5.1: Citizen Report Privacy
- Citizen phone numbers, full residential addresses, and raw personal identifiers must only be visible to authenticated Operators and assigned Responders.
- Publicly visible incident pins on maps must display aggregated categories (e.g., *"Waterlogging > 50cm reported"*) and round spatial coordinates to 3 decimal places (~110 meters precision) to prevent targeted harassment or doxxing.

### Rule 5.2: CCTV & Computer Vision Telemetry (Synthetic Demonstration Mandate)
- **Synthetic Demonstration Boundaries:**
  - For the hackathon prototype, all CCTV feeds, video frames, and camera metadata operate strictly as **synthetic demonstration fixtures** placed at representative demonstration coordinates.
  - The system **SHALL NOT** connect to, tap into, or claim access to real municipal CCTV networks (e.g., Thane Municipal Corporation or Traffic Police surveillance feeds).
  - The UI must explicitly tag CCTV tiles as `[ SYNTHETIC DEMONSTRATION FEED ]`.
- **Environmental Analytics Focus:**
  - Computer vision analytics (both mock streams and any production integrations) must focus strictly on environmental and situational features:
    - Water accumulation depth and surface reflections.
    - Vehicle stalls and traffic blockages.
    - Choked culverts, overflowing stormwater drains.
    - General crowd density or trapped person bounding boxes.
  - Facial recognition, license plate recognition, and individual biometric tracking are **strictly forbidden** from the DISAS processing pipeline.

---

## 6. Role-Based Access Control (RBAC) & Authentication Modes

### 6.1 Demo Role Switching vs Production Authentication
- **Prototype Mode (Current):** Employs an instant 1-click role switcher in the application shell to allow frictionless evaluation between Citizen, Operator, Responder, and Public Display interfaces. Evaluators can transition roles instantly without entering external credentials.
- **Production Mode (Future Architecture):** Enforces enterprise-grade OpenID Connect (OIDC) / SAML 2.0 with government single-sign-on (e.g., MeriPehchan), hardware-backed MFA (FIDO2/TOTP) for operators, PKI card authentication for field responders, and strict cryptographic signature verification on all operational tokens.

### 6.2 Permission Matrix

| Permission / Capability | Citizen Portal | Operator Command | Responder Portal | Public Display |
| :--- | :---: | :---: | :---: | :---: |
| **View Live Public Advisory & Shelter Locations** | Yes | Yes | Yes | Yes |
| **Submit Emergency SOS & Photo Report** | Yes | Yes | Yes | No |
| **View 2D City-Wide GIS & 3D Isometric Twin** | Limited (2D) | Full (2D + 3D) | Limited (2D) | View-Only 2D |
| **Execute & Adjust Hydrological Simulation** | No | Full Controls | No | No |
| **View Raw CCTV Stream & CV Bounding Boxes** | No | Full Access | No | No |
| **Triage & Verify Incident Reports** | No | Full Authority | Verification Only | No |
| **Dispatch Teams & Allocate Vehicles/Boats** | No | Full Authority | No | No |
| **Update Field Task Status (En Route/On Scene)** | No | Override Only | Full Authority | No |
| **Promote Candidate Shelters to Official** | No | Full Authority | Field Validate | No |
| **Broadcast Public City-Wide Emergency Alert** | No | Full Authority | No | No |

---

## 7. Simulated Offline Networking Rules

- Any offline communication features (Bluetooth Low Energy mesh and LoRa gateway emulation) must be explicitly noted in the UI as **"Simulated Mesh Networking"**.
- The system must never mislead operators or citizens into believing hardware antennas or radios are physically transmitting unless actual hardware transceivers are integrated.
- Local client queues must employ deterministic cryptographic UUIDs and vector clocks to resolve synchronization conflicts gracefully upon reconnection.
