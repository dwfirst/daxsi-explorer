# Thermal Budget  Heat Generation, Rejection, and Harvesting

This document defines the thermal budget of the DAXSI platform. It covers heat generation by node, rejection paths, PCM buffering, and STC harvesting. The binding constraint is roof radiator area, not coolant capacity.

---

### 1. Heat Generation by Node

| Node | Electrical draw (kW) | Heat load (kW) | Notes |
|---|---|---|---|
| Node 01 | 0.12 | 0.12 | Near 100% to heat |
| Node 02 | 1.20 | 1.20 | Near 100% to heat |
| Node 03 | 0.16 | 0.16 | Near 100% to heat |
| Cooling pumps | 0.10 | 0.05 | Partial to heat |
| PDU losses | 0.05 | 0.05 | Conversion losses |
| **Total compute heat** | | **1.58** | Peak |

All electrical power drawn by compute becomes heat. The only exception is optical power emitted by sensors, which is negligible.

---

### 2. Rejection Paths

| Path | Capacity (kW) | Medium | Status |
|---|---|---|---|
| Loop A  roof radiator | 1.50 | Liquid-to-air | `[COTS]` |
| Loop B  chassis contact | 0.40 | Conduction | `[COTS]` |
| PCM buffer | 0.30 (transient) | Phase change | `[COTS]` |
| STC harvester | 0.15 (recovery) | Thermal-to-electric | `[PROTO]` |
| **Total rejection** | **2.35** | | |

Total rejection capacity exceeds peak heat load by 0.77 kW. This margin covers ambient temperature derating.

---

### 3. Roof Radiator Sizing

| Parameter | Value | Status |
|---|---|---|
| Area | 1.8 m | `[COTS]` |
| Coolant inlet temp | 55 C | `[COTS]` |
| Ambient temp (design) | 40 C | `[COTS]` |
| Delta-T | 15 C | `[COTS]` |
| Airflow (vehicle moving) | 15 m/s | `[COTS]` |
| Rejection capacity | 1.50 kW | `[COTS]` |

At 40 C ambient and 15 m/s airflow, a 1.8 m radiator rejects approximately 1.50 kW. This is sufficient for Node 02 at average load, but not at peak. Peak load is handled by PCM buffering.

In static monitoring, airflow is zero. Radiator capacity drops to approximately 0.40 kW by natural convection. This is why PCM buffering is required for static states.

---

### 4. PCM Buffer

| Parameter | Value | Status |
|---|---|---|
| Material | Paraffin-based phase-change material | `[COTS]` |
| Mass | 25 kg | `[COTS]` |
| Melting point | 58 C | `[COTS]` |
| Latent heat | 200 kJ/kg | `[COTS]` |
| Total storage | 5.0 MJ | `[COTS]` |
| Charge time (peak load) | 45 min | `[COTS]` |
| Discharge time (cabin heating) | 8 hours | `[COTS]` |

The PCM buffer absorbs heat during peak inference and releases it during cold static periods. It is not a net energy source; it is a time-shift buffer.

At 0.30 kW transient absorption, 5.0 MJ provides approximately 4.6 hours of buffering. This covers the gap between peak inference and radiator capacity.

---

### 5. STC Thermal Harvester

| Parameter | Value | Status |
|---|---|---|
| Technology | Semiconductor-sensitized thermal cell | `[PROTO]` |
| Operating temperature | 3080 C | `[PROTO]` |
| Conversion efficiency | 13% | `[PROTO]` |
| Output | 48V DC, 0.050.15 kW | `[PROTO]` |
| Integration point | Loop A return line | `[PROTO]` |

STC is not a primary power source. It recovers low-grade heat that would otherwise be rejected to ambient. At 13% efficiency and 1.5 kW heat flow, output is 0.0150.045 kW. The 0.15 kW upper estimate assumes a larger array than the baseline.

STC is a Phase 2 item. It is not required for Phase 0 or Phase 1.

---

### 6. Cold-Weather Operation

| Scenario | Heat source | Status |
|---|---|---|
| Cabin heating, engine off | PCM buffer | `[COTS]` |
| Cabin heating, compute active | Node 02 waste heat via liquid-to-air | `[COTS]` |
| Battery warming | Chassis contact loop B | `[COTS]` |
| Extreme cold (30 C) | PCM + heat pump | `[COTS]` |

At 30 C ambient, radiator capacity is irrelevant. All heat is retained. The PCM buffer and Node 02 waste heat provide cabin heating without drawing from the traction battery.

This is the primary value of the thermal architecture: in cold conditions, waste heat is a resource, not a problem.

---

### 7. Thermal Derating

| Condition | Derating | Mitigation |
|---|---|---|
| Ambient > 45 C | Node 02 throttles to 60% | PCM buffer, reduced inference |
| Radiator blocked | Node 02 throttles to 40% | Operator alert, cleaning |
| Coolant leak | Node 02 shuts down | Failsafe to Node 01 |
| Static, no airflow | Node 02 limited to 0.40 kW | PCM buffer |

Derating is graceful. The platform does not shut down; it reduces capability.

---

### 8. Open Questions

1. Radiator capacity at 40 C ambient and 15 m/s airflow requires wind-tunnel validation.
2. PCM discharge time under 30 C ambient requires thermal simulation.
3. STC efficiency at vehicle-scale heat flow is unvalidated.
4. Chassis contact cooling capacity depends on vehicle thermal mass, not measured.

See `01_FEASIBILITY/open_questions.md`.
