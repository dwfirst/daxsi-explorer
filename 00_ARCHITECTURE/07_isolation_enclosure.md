# Block 7: Isolation Enclosure Architecture — EMI, Vibration, Thermal Feedthroughs

This document defines the physical enclosure of every compute block in the DAXSI platform. The enclosure is treated as a subsystem, not as sheet metal. Each layer addresses a distinct physical threat: kinetic load, vibration, electromagnetic interference, and thermal exchange.

---

### 1. Threat Model

The Cybertruck platform exposes onboard compute to four overlapping threats that are absent in a data center.

| Threat | Source | Frequency / Range | Consequence |
|---|---|---|---|
| Kinetic impact | Debris, terrain strike, cargo shift | Impulse | Structural deformation, connector shear |
| Vibration | Drivetrain, off-road surface, manipulator motion | 5–500 Hz | Solder fatigue, connector wear, sensor drift |
| EMI — electric | Traction inverter, motor phases, Powershare | 10 kHz–1 GHz | Data corruption, clock jitter |
| EMI — magnetic | Motor fields, external sources | 50 Hz–100 kHz | Inductive coupling into signal lines |
| Thermal | GPU clusters, solar gain, external ambient | Continuous | Derating, throttling |
| Moisture / dust | Wading, dust storms, condensation | Continuous | Corrosion, short circuit |

Each layer of the enclosure addresses one or more of these threats. No single layer addresses all.

---

### 2. Layer Stack
External environment
│
▼
[L1] Aluminum structural shell, 3–5 mm, sealed
│
▼
[L2] Aluminum foam damping layer, 10–15 mm
│
▼
[L3] Faraday CYBER NC-Berry Rip Stop, double layer
│
▼
[L4] Wire-rope isolators, passive broadband
│
▼
[L5] HSLDS magnetic-pneumatic modules, adaptive
│
▼
Internal compute chassis
│
▼
[L6] Optical ports, ODU Expanded Beam
[L7] Hermetic coolant feedthroughs

text

---

### 3. Layer Specifications

#### L1 — Aluminum Structural Shell

| Parameter | Value | Status |
|---|---|---|
| Material | 6061-T6 aluminum | `[COTS]` |
| Wall thickness | 3–5 mm | `[COTS]` |
| Sealing | Gasketed, IP67 | `[COTS]` |
| Function | Structural rigidity, primary electric-field shielding, thermal mass | `[COTS]` |

Aluminum provides electric-field attenuation but does not provide magnetic shielding. Magnetic attenuation is delegated to L3.

#### L2 — Aluminum Foam Damping Layer

| Parameter | Value | Status |
|---|---|---|
| Material | Closed-cell aluminum foam | `[COTS]` |
| Thickness | 10–15 mm | `[COTS]` |
| Function | Structural vibration damping, acoustic absorption | `[COTS]` |
| Internal damping ratio | 5–10× monolithic aluminum | `[COTS]` |

The foam is bonded to the inner wall of L1. It dissipates vibration energy through micro-plastic deformation of the cell walls. This layer is passive and requires no power.

#### L3 — EMI Inner Liner

| Parameter | Value | Status |
|---|---|---|
| Material | Faraday CYBER NC-Berry Rip Stop | `[COTS]` |
| Construction | Nickel-copper plated polyester ripstop | `[COTS]` |
| Attenuation | 75 dB, 100 MHz–18 GHz | `[COTS]` |
| Thickness | 7 mil | `[COTS]` |
| Layers | Double, with seam overlap ≥ 30 mm | `[COTS]` |
| Function | Magnetic and electric field shielding, seam closure | `[COTS]` |

The two layers are offset so that seams do not align. Overlap regions are bonded with conductive adhesive. Cable and optical penetrations are wrapped and bonded to the liner to preserve shield continuity.

#### L4 — Passive Vibration Isolation

| Parameter | Value | Status |
|---|---|---|
| Type | Wire-rope isolator | `[COTS]` |
| Mounting | Four-point per chassis | `[COTS]` |
| Frequency range | 5–200 Hz attenuation | `[COTS]` |
| Function | Broadband passive isolation, no power required | `[COTS]` |

Wire-rope isolators are selected over elastomeric mounts because they tolerate temperature extremes, do not degrade under UV, and do not outgas in a sealed enclosure.

#### L5 — Adaptive Vibration Isolation

| Parameter | Value | Status |
|---|---|---|
| Type | HSLDS magnetic-pneumatic module | `[PROTO]` |
| Mounting | One per chassis, in series with L4 | `[PROTO]` |
| Function | Tunable negative stiffness, low-frequency isolation | `[PROTO]` |
| Control | ROS 2 node, accelerometer feedback | `[PROTO]` |

HSLDS modules are at prototype maturity. They are not required for Phase 0 or Phase 1. Phase 2 introduces them after bench validation. See `01_FEASIBILITY/open_questions.md`.

#### L6 — Optical Ports

| Parameter | Value | Status |
|---|---|---|
| Type | ODU Expanded Beam | `[COTS]` |
| Standard | MIL-DTL-38999 / VITA 95/96 | `[COTS]` |
| Insertion loss | < 0.15 dB multimode | `[COTS]` |
| Mating cycles | 50,000 without cleaning | `[COTS]` |
| Temperature range | −65 °C to +105 °C | `[COTS]` |
| Sealing | IP67 with protective cap | `[COTS]` |

Expanded-beam construction uses a lens instead of physical fiber-end contact. This removes the primary failure mode of optical connectors in field conditions: contamination of the fiber end face. All inter-block data links use this port type. No copper data cables cross the enclosure boundary.

#### L7 — Coolant Feedthroughs

| Parameter | Value | Status |
|---|---|---|
| Type | Hermetic bulkhead fitting | `[COTS]` |
| Fluid | Dielectric coolant | `[COTS]` |
| Sealing | Dual O-ring, leak-tested | `[COTS]` |
| Function | Liquid loop entry and exit without breaking EMI shield | `[COTS]` |

Coolant lines are non-conductive and pass through the shield at a single bonded penetration. The feedthrough body is electrically bonded to L1 and L3 to preserve shield continuity.

---

### 4. Inter-Block Topology

| Link | Type | Rationale |
|---|---|---|
| Node 01 ↔ Node 02 | Optical, ODU | Galvanic isolation, high bandwidth |
| Node 02 ↔ Node 03 | Optical, ODU | Galvanic isolation, high bandwidth |
| Sensor matrix ↔ Node 01 | Optical, ODU | Prevents sensor-side ground loops |
| Portable kit ↔ Node 01 | Optical or Starlink | Field link, no copper |
| Manipulator ↔ Node 01 | Optical, ODU | Motor noise isolation |
| Power distribution | 48V DC, shielded | Single bonded entry per block |

No copper data conductor crosses an enclosure boundary. Power conductors are shielded and bonded at a single point to avoid ground loops.

---

### 5. Open Questions

1. HSLDS module validation under combined vibration and thermal load — no published field data.
2. Long-term behavior of aluminum foam under repeated thermal cycling — accelerated life test required.
3. Optical port contamination rate in dust-heavy environments — field data required.
4. Faraday liner seam integrity after repeated enclosure opening — maintenance procedure required.

---

### 6. Bill of Materials (Baseline)

| Item | Quantity per block | Status |
|---|---|---|
| 6061-T6 enclosure | 1 | `[COTS]` |
| Aluminum foam sheet | 1 set | `[COTS]` |
| Faraday CYBER NC-Berry Rip Stop | 1 set, double layer | `[COTS]` |
| Wire-rope isolator | 4 | `[COTS]` |
| HSLDS module | 1 | `[PROTO]` |
| ODU Expanded Beam port | 2–4 | `[COTS]` |
| Hermetic coolant feedthrough | 2 | `[COTS]` |
