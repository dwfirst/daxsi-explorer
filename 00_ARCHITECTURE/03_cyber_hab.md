# Block 3: CyberHab — Expedition Pod, Dry Architecture, and ECLSS

This document defines the habitation and equipment pod mounted on the Cybertruck Vault. The architecture is dry: no water mass is carried. All life-support and climate systems operate without liquid storage.

---

### 1. Structural Integration

| Parameter | Value | Status |
|---|---|---|
| Mounting | Cybertruck Vault OEM utility rails | `[COTS]` |
| Shell material | Carbon fiber sandwich panel | `[COTS]` |
| Insulation | R-14, zero thermal bridge | `[COTS]` |
| Exterior finish | Matte graphite | `[COTS]` |
| Windows | None (equipment and habitation volume) | `[COTS]` |
| Sealing | IP67 | `[COTS]` |

The pod is un-windowed by design. The operator observes through the vehicle cabin and through sensor feeds, not through pod glass.

---

### 2. Mass and Center of Gravity

Heavy elements are placed flat against the bed floor, between the axles. Light elements occupy the upper volume.

| Element | Mass (est.) | Position | Status |
|---|---|---|---|
| Node 02 rack | 45 kg | Bed floor, center | `[COTS]` |
| Node 03 rack | 30 kg | Bed floor, center | `[COTS]` |
| Coolant reservoir | 12 kg | Bed floor, rear | `[COTS]` |
| PCM buffer | 25 kg | Bed floor, center | `[COTS]` |
| Sleeping platform | 15 kg | Upper, side-folding | `[COTS]` |
| ECLSS unit | 18 kg | Upper, rear | `[COTS]` |
| Manipulator bay | 20 kg | Under-bed, sealed | `[COTS]` |
| **Total pod mass** | **~165 kg** | | |

Center of gravity is maintained below the bed rail line. No water tank is carried. See Section 5.

---

### 3. Habitation Matrix

| Zone | Function | Status |
|---|---|---|
| Research and command | Retractable desk, 14-inch operator display, diagnostics panel | `[COTS]` |
| Sleeping | Folding memory-foam platform, vertical stow | `[COTS]` |
| Wet block | Dry composting lavatory, positive-pressure ventilation | `[COTS]` |
| Storage | Modular bins, shock-isolated | `[COTS]` |

The wet block is a dry system. It uses a composting toilet with positive-pressure ventilation and a HEPA exhaust filter. No water is used for sanitation. No greywater is generated.

---

### 4. Energy Lifecycle

| Source | Capacity | Status |
|---|---|---|
| Cybertruck traction battery via Powershare | 9.6 kW continuous | `[COTS]` |
| Roof solar array | 1.2 kW peak | `[COTS]` |
| MPPT charge controller | 48V output | `[COTS]` |
| PCM thermal buffer | 25 kg phase-change material | `[COTS]` |
| STC thermal harvester | Low-grade waste heat to 48V | `[PROTO]` |

The pod does not carry a dedicated generator. All power comes from the vehicle traction battery, the solar array, and thermal harvesting.

---

### 5. Dry Architecture — Removed Systems

The following systems are removed from the architecture. They are not present in any phase.

| Removed system | Reason |
|---|---|
| 60 L freshwater tank | Mass, freezing risk, contamination risk |
| UV-C inline sterilizer | Requires water loop |
| Induction shower | Requires water supply and greywater handling |
| Greywater tank | Consequence of shower |
| Water heater | Consequence of shower |

Sanitation is dry composting. Operator hygiene is managed with dry wipes and external facilities where available. The pod is not designed for indefinite closed-loop habitation; it is designed for multi-week field deployment with periodic resupply.

---

### 6. ECLSS — Environmental Control and Life Support

| Subsystem | Specification | Status |
|---|---|---|
| Particulate filtration | HEPA H14 | `[COTS]` |
| Chemical filtration | Activated charcoal matrix | `[COTS]` |
| Cabin pressure | Slight positive pressure | `[COTS]` |
| Climate control | 48V variable-speed heat pump | `[COTS]` |
| Heat recycling | Node 02 waste heat to cabin via liquid-to-air exchanger | `[COTS]` |
| CO2 monitoring | NDIR sensor, linked to ECLSS | `[COTS]` |
| VOC monitoring | BME680, linked to ECLSS | `[COTS]` |

Positive cabin pressure repels external dust, soot, and radioactive particulate. The HEPA and charcoal stages are sized for continuous operation during static monitoring.

---

### 7. Thermal Integration

The pod has three thermal paths.

| Path | Source | Destination | Status |
|---|---|---|---|
| Liquid loop A | Node 02 RTX cluster | Roof radiator behind solar array | `[COTS]` |
| Liquid loop B | Node 01 command core | Chassis-contact cooling blocks | `[COTS]` |
| PCM buffer | Node 02 waste heat | Cabin heating during cold cycles | `[COTS]` |

Loop A rejects heat to the roof radiator. Loop B rejects heat to the chassis structural mass during cold-weather operation. The PCM buffer stores excess heat during operation and releases it during cold static periods.

See `01_FEASIBILITY/thermal_budget.md` for calculation details.

---

### 8. Manipulator Bay

| Parameter | Value | Status |
|---|---|---|
| Location | Under-bed, sealed, forward of rear axle | `[COTS]` |
| Sealing | IP67 | `[COTS]` |
| Deployment | Linear actuator, ROS 2 action | `[COTS]` |
| Stowed lock | Two-point mechanical, damping inserts | `[COTS]` |
| Access | Rear-facing hatch | `[COTS]` |

The bay is isolated from the habitation volume. The manipulator is deployed only in the Deployed operational state.

---

### 9. Open Questions

1. Dry composting lavatory performance over multi-week deployment requires field validation.
2. PCM buffer sizing for sub-zero static monitoring requires thermal simulation.
3. Pod mass estimate requires confirmation after hardware selection.
4. Positive-pressure ECLSS power draw at −30 °C ambient requires measurement.

See `01_FEASIBILITY/open_questions.md`.
