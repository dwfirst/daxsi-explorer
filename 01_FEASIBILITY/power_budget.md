# Power Budget  Peak and Average Draw by Subsystem

This document defines the electrical power budget of the DAXSI platform. It separates peak draw from average draw, identifies the binding constraint, and defines the operational states that determine load.

---

### 1. Source and Envelope

| Parameter | Value | Status |
|---|---|---|
| Primary source | Cybertruck Powershare, 240V NEMA 14-50 | `[COTS]` |
| Continuous rating | 9.6 kW (40 A) | `[COTS]` |
| Secondary source | Roof solar, 1.2 kW peak | `[COTS]` |
| Tertiary source | STC thermal harvester | `[PROTO]` |
| Internal distribution | 48V DC rail, shielded | `[COTS]` |
| Failsafe rail | 48V auxiliary, vehicle-native | `[COTS]` |

The 9.6 kW Powershare rating is the hard ceiling. Solar and STC reduce net draw but do not raise the ceiling.

---

### 2. Subsystem Power Table

| Subsystem | Peak (kW) | Average (kW) | Idle (kW) | Notes |
|---|---|---|---|---|
| Node 01 Command Core | 0.12 | 0.08 | 0.03 | Continuous |
| Node 02 Inference Primary | 1.20 | 0.80 | 0.10 | Burst on inference |
| Node 03 RAG / Storage | 0.16 | 0.12 | 0.05 | Continuous |
| Cooling loop A | 0.20 | 0.10 | 0.02 | Variable |
| Cooling loop B | 0.10 | 0.05 | 0.01 | Variable |
| ECLSS | 0.25 | 0.12 | 0.05 | Variable |
| Climate control | 0.15 | 0.08 | 0.02 | Variable |
| Sensor matrix | 0.15 | 0.10 | 0.03 | Continuous |
| Manipulator | 0.30 | 0.05 | 0.00 | Deployed state only |
| Portable kit charging | 0.10 | 0.03 | 0.00 | Intermittent |
| Starlink Mobility | 0.05 | 0.03 | 0.00 | Periodic |
| **Total** | **2.78** | **1.56** | **0.31** | |

Peak total is 2.78 kW. Average total is 1.56 kW. Idle total is 0.31 kW.

---

### 3. Operational State Budgets

| State | Peak (kW) | Average (kW) | Duration | Notes |
|---|---|---|---|---|
| Idle | 0.31 | 0.31 | Indefinite | Parked, minimal monitoring |
| Static monitoring | 1.80 | 1.50 | Weeks | Full sensors, no motion |
| Mobile survey | 2.78 | 2.30 | Hours | Full sensors, driving |
| Deployed | 2.78 | 2.10 | Hours | Manipulator active, kit deployed |

All states are within the 9.6 kW envelope. Peak headroom at Deployed state is 6.82 kW.

---

### 4. Headroom Analysis

| Scenario | Draw (kW) | Headroom (kW) | Status |
|---|---|---|---|
| Baseline, mobile survey | 2.78 | 6.82 | `[COTS]` |
| Stage 1 expansion (8 RTX 6000 Ada) | 3.98 | 5.62 | `[PROTO]` |
| Stage 2 expansion (8 RTX + 4 L40S) | 4.78 | 4.82 | `[PROTO]` |
| Stage 3 expansion (external dock) | 6.50 | 3.10 | `[CONCEPT]` |

Expansion is limited by cooling capacity and thermal rejection, not by electrical headroom. At Stage 2, the roof radiator area becomes the binding constraint.

---

### 5. Static Monitoring Budget

Static monitoring is the longest-duration state. It determines battery endurance.

| Load | Draw (kW) |
|---|---|
| Node 01 | 0.08 |
| Node 02 (reduced) | 0.40 |
| Node 03 | 0.12 |
| Cooling | 0.15 |
| ECLSS | 0.12 |
| Climate | 0.08 |
| Sensors | 0.10 |
| **Total** | **1.05** |

With solar input of 0.6 kW average (50% of peak, accounting for angle and weather), net draw is 0.45 kW. Cybertruck traction battery capacity is approximately 123 kWh. At 0.45 kW net draw, endurance is approximately 270 hours, or 11 days, without recharging.

This is the basis for the multi-week station-keeping claim. It assumes favorable solar conditions. In overcast conditions, endurance drops to approximately 5 days.

---

### 6. STC and PCM Contribution

| Source | Contribution | Status |
|---|---|---|
| STC thermal harvester | 812% reduction in cooling load | `[PROTO]` |
| PCM buffer | Shifts heating load from battery to stored heat | `[COTS]` |

STC does not generate net power. It reduces the cooling load by converting low-grade heat to 48V rail current. The 812% estimate is from data-center PoC, not vehicle validation.

---

### 7. Open Questions

1. STC contribution in vehicle thermal environment is not validated.
2. Static monitoring endurance under overcast conditions requires field measurement.
3. Manipulator peak draw during dynamic motion exceeds the 0.30 kW estimate in some commercial arms. Requires measurement.
4. Portable kit charging while deployed reduces Deployed state headroom.

See `01_FEASIBILITY/open_questions.md`.

