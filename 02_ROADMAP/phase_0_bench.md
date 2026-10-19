# Phase 0  Bench Validation

Duration: 3 months. Objective: validate the core signal chain on a bench before any vehicle integration. No Cybertruck is required in this phase.

---

### 1. Scope

Phase 0 proves that the sensor-to-inference pipeline works. It does not prove vehicle integration, thermal performance, or field reliability.

| In scope | Out of scope |
|---|---|
| ROS 2 pipeline | Vehicle integration |
| One hyperspectral sensor | Full sensor matrix |
| One LiDAR | Manipulator |
| One GPU inference node | Thermal loops |
| PTP time sync | Starlink link |
| iceoryx zero-copy | ECLSS |

---

### 2. Hardware

| Item | Quantity | Status |
|---|---|---|
| Workstation with 1 RTX 6000 Ada | 1 | `[COTS]` |
| Jetson AGX Orin 64GB dev kit | 1 | `[COTS]` |
| Cubert ULTRIS X20 or equivalent | 1 | `[COTS]` |
| Ouster OS1-128 | 1 | `[COTS]` |
| FLIR Boson 640 | 1 | `[COTS]` |
| PTP-capable network switch | 1 | `[COTS]` |
| Calibration targets | 1 set | `[COTS]` |

Total bench cost is dominated by the RTX 6000 Ada and the hyperspectral sensor.

---

### 3. Tasks

| ID | Task | Deliverable | Status |
|---|---|---|---|
| P0-01 | Install ROS 2 Humble, iceoryx, PTP | Working middleware stack | `[PLANNED]` |
| P0-02 | Integrate ULTRIS X20 driver | Live hyperspectral stream | `[PLANNED]` |
| P0-03 | Integrate Ouster OS1-128 driver | Live point cloud | `[PLANNED]` |
| P0-04 | Integrate FLIR Boson driver | Live thermal stream | `[PLANNED]` |
| P0-05 | Validate PTP sync across all sensors | < 1 µs alignment | `[PLANNED]` |
| P0-06 | Validate iceoryx zero-copy path | CPU overhead < 5% | `[PLANNED]` |
| P0-07 | Run open-weights 70B on 1 GPU | Baseline inference latency | `[PLANNED]` |
| P0-08 | Run YOLOv11 multi-spectral on fused stream | Detection on test targets | `[PLANNED]` |
| P0-09 | Record MCAP dataset with geospatial tags | Reference dataset | `[PLANNED]` |
| P0-10 | Document pipeline latency end-to-end | Latency report | `[PLANNED]` |

---

### 4. Success Criteria

| Criterion | Target | Method |
|---|---|---|
| PTP alignment | < 1 µs across sensors | Oscilloscope / PTP daemon log |
| iceoryx CPU overhead | < 5% at full stream | `top` / `perf` |
| Inference latency | < 2 s for 1k tokens on 1 GPU | Timed benchmark |
| Detection accuracy | > 80% on calibration targets | Manual annotation |
| Dataset integrity | No dropped frames over 1 hour | MCAP inspection |

Failure to meet a criterion does not stop the phase. It becomes an open question and is carried forward.

---

### 5. Deliverables

| Deliverable | Format | Location |
|---|---|---|
| Middleware stack | Working repo | `phase_0/` branch |
| Driver integrations | ROS 2 packages | `phase_0/` branch |
| Reference dataset | MCAP + Parquet | External storage |
| Latency report | Markdown | `02_ROADMAP/phase_0_report.md` |
| Open questions update | Register entries | `01_FEASIBILITY/open_questions.md` |

---

### 6. Exit Criteria

Phase 0 is complete when:

1. All ten tasks are executed.
2. Success criteria are measured and documented.
3. Open questions are updated with new findings.
4. Latency report is committed.
5. A go/no-go decision is recorded for Phase 1.

A no-go decision is a valid outcome. It redirects effort before vehicle integration begins.

---

### 7. Risk

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Hyperspectral driver not ROS 2 native | High | Schedule slip | Budget 2 weeks for adapter |
| iceoryx incompatibility with sensor driver | Medium | Pipeline redesign | Fallback to standard DDS |
| 70B model does not fit on 1 GPU at 4-bit | Low | Reduce to 34B | Accept smaller baseline |
| PTP hardware not available on dev kit | Medium | Use software PTP | Accept < 100 µs alignment |

