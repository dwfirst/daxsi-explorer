# Phase 1 - Static Laboratory on Cybertruck

Duration: 6 months. Objective: integrate the full sensor matrix and compute cluster into the Cybertruck platform in a stationary configuration. No driving, no manipulator.

---

### 1. Scope

Phase 1 proves that the platform works as a stationary laboratory. The vehicle is parked. The manipulator is stowed. The portable kit is docked.

| In scope | Out of scope |
|---|---|
| Full sensor matrix | Mobile survey |
| All three compute nodes | Manipulator deployment |
| Isolation enclosures | Portable kit field use |
| Thermal loops A and B | Starlink orbital validation |
| PCM buffer | LoRA fine-tuning |
| ECLSS | Autonomous missions |

---

### 2. Hardware Integration

| Subsystem | Quantity | Status |
|---|---|---|
| Node 01 enclosure | 1 | `[COTS]` |
| Node 02 enclosure | 1 | `[COTS]` |
| Node 03 enclosure | 1 | `[COTS]` |
| Sensor mounts (roof, A-pillar, tailgate) | 1 set | `[COTS]` |
| Liquid loop A + roof radiator | 1 | `[COTS]` |
| Liquid loop B | 1 | `[COTS]` |
| PCM buffer | 1 | `[COTS]` |
| ECLSS unit | 1 | `[COTS]` |
| CyberHab pod | 1 | `[COTS]` |
| ODU optical ports | 6-8 | `[COTS]` |

---

### 3. Tasks

| ID | Task | Deliverable | Status |
|---|---|---|---|
| P1-01 | Install and seal Node 01/02/03 enclosures | Isolated compute | `[PLANNED]` |
| P1-02 | Mount sensor matrix | Full coverage | `[PLANNED]` |
| P1-03 | Install liquid loop A and roof radiator | Thermal path | `[PLANNED]` |
| P1-04 | Install liquid loop B | Thermal path | `[PLANNED]` |
| P1-05 | Install PCM buffer | Thermal buffer | `[PLANNED]` |
| P1-06 | Install ECLSS | Life support | `[PLANNED]` |
| P1-07 | Install optical inter-node links | Galvanic isolation | `[PLANNED]` |
| P1-08 | Calibrate all sensors | Calibration records | `[PLANNED]` |
| P1-09 | Run full pipeline at static load | 72-hour continuous run | `[PLANNED]` |
| P1-10 | Measure thermal derating | Thermal report | `[PLANNED]` |
| P1-11 | Measure EMI shield effectiveness | EMI report | `[PLANNED]` |
| P1-12 | Measure power budget in all states | Power report | `[PLANNED]` |

---

### 4. Success Criteria

| Criterion | Target | Method |
|---|---|---|
| Continuous operation | 72 hours without failure | Log analysis |
| Thermal derating | < 20% at 40 C ambient | Thermal sensors |
| EMI attenuation | > 60 dB at 1 GHz | Spectrum analyzer |
| PTP alignment | < 1 µs across nodes | PTP daemon log |
| Power draw, static | < 1.8 kW peak | Power meter |
| Sensor calibration | All sensors within spec | Calibration records |

---

### 5. Deliverables

| Deliverable | Format | Location |
|---|---|---|
| Integrated platform | Physical | Vehicle |
| Thermal report | Markdown | `02_ROADMAP/phase_1_report.md` |
| EMI report | Markdown | `02_ROADMAP/phase_1_report.md` |
| Power report | Markdown | `02_ROADMAP/phase_1_report.md` |
| Calibration records | CSV | Empirical vault |
| 72-hour dataset | MCAP | Empirical vault |

---

### 6. Exit Criteria

Phase 1 is complete when:

1. All twelve tasks are executed.
2. Success criteria are measured.
3. 72-hour continuous run is completed without failure.
4. Reports are committed.
5. Go/no-go decision for Phase 2 is recorded.

---

### 7. Risk

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Thermal derating exceeds 20% | High | Reduced inference | Add PCM, increase radiator |
| EMI attenuation below 60 dB | Medium | Data corruption | Add liner layer, improve seams |
| Enclosure weight exceeds estimate | Medium | CG shift | Rebalance pod layout |
| Optical port contamination | High | Link failure | Field cleaning procedure |
| ECLSS power exceeds budget | Low | Reduced endurance | Derate climate control |
