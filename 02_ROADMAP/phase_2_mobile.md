# Phase 2 - Mobile Survey

Duration: 9 months. Objective: validate the platform in motion. The vehicle drives, the sensor matrix collects data, the manipulator is integrated, and the portable kit is field-tested.

---

### 1. Scope

Phase 2 proves that the platform works while moving. It does not prove autonomous missions or orbital validation.

| In scope | Out of scope |
|---|---|
| Driving data collection | Autonomous missions |
| Manipulator deployment | LoRA fine-tuning in field |
| Portable kit field test | Orbital validation loop |
| Vibration validation | Long-duration autonomy |
| HSLDS module integration | STC thermal harvester |
| Starlink link test | Knowledge graph uplink |

---

### 2. Hardware Additions

| Subsystem | Quantity | Status |
|---|---|---|
| Manipulator arm (6DOF) | 1 | `[COTS]` |
| Stowage bay + actuator | 1 | `[COTS]` |
| HSLDS modules | 3 | `[PROTO]` |
| Portable kit (full) | 1 | `[COTS]` |
| Starlink Mini | 1 | `[COTS]` |
| Vibration sensors (reference) | 4 | `[COTS]` |

---

### 3. Tasks

| ID | Task | Deliverable | Status |
|---|---|---|---|
| P2-01 | Integrate manipulator and stowage bay | Deployable arm | `[PLANNED]` |
| P2-02 | Validate stowed vibration tolerance | Vibration report | `[PLANNED]` |
| P2-03 | Integrate HSLDS modules | Adaptive isolation | `[PLANNED]` |
| P2-04 | Field-test portable kit | Kit field report | `[PLANNED]` |
| P2-05 | Test Starlink link from moving vehicle | Link report | `[PLANNED]` |
| P2-06 | Collect mobile dataset | MCAP dataset | `[PLANNED]` |
| P2-07 | Validate sensor calibration in motion | Calibration drift report | `[PLANNED]` |
| P2-08 | Measure real-world vibration spectrum | Vibration spectrum | `[PLANNED]` |
| P2-09 | Test manipulator deployment sequence | Deployment log | `[PLANNED]` |
| P2-10 | Measure power budget in mobile state | Power report | `[PLANNED]` |
| P2-11 | Test data integrity over 100 km | Integrity report | `[PLANNED]` |
| P2-12 | Validate portable kit link modes | Link mode report | `[PLANNED]` |

---

### 4. Success Criteria

| Criterion | Target | Method |
|---|---|---|
| Stowed manipulator vibration | No damage after 1000 km | Inspection + vibration log |
| Sensor calibration drift | < 5% over 100 km | Before/after calibration |
| Data integrity | No lost frames over 100 km | MCAP inspection |
| Starlink link | > 10 Mbps average | Throughput log |
| Portable kit mass | < 12 kg | Weighing |
| HSLDS isolation | > 10 dB at 5-20 Hz | Accelerometer comparison |

---

### 5. Deliverables

| Deliverable | Format | Location |
|---|---|---|
| Mobile dataset | MCAP + Parquet | Empirical vault |
| Vibration report | Markdown | `02_ROADMAP/phase_2_report.md` |
| Kit field report | Markdown | `02_ROADMAP/phase_2_report.md` |
| Link report | Markdown | `02_ROADMAP/phase_2_report.md` |
| Calibration drift records | CSV | Empirical vault |

---

### 6. Exit Criteria

Phase 2 is complete when:

1. All twelve tasks are executed.
2. 100 km mobile dataset is collected.
3. Manipulator is validated in stowed and deployed states.
4. Portable kit is field-tested in all link modes.
5. Go/no-go decision for Phase 3 is recorded.

---

### 7. Risk

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Stowed manipulator vibration damage | Medium | Arm replacement | Damping inserts, lock |
| Sensor calibration drift | High | Data quality | Frequent recalibration |
| Starlink link dropouts | High | Data sync gaps | Local buffering |
| HSLDS module failure | Medium | Reduced isolation | Wire-rope fallback |
| Portable kit overweight | High | Operator fatigue | Drop WNIR instrument |
