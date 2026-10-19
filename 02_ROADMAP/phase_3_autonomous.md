# Phase 3 - Autonomous Missions

Duration: 12 months. Objective: close the loop between field collection, onboard fine-tuning, and orbital validation. The platform operates autonomously for extended periods with human oversight.

---

### 1. Scope

Phase 3 proves that the platform can run multi-week missions, train on its own validated data, and synchronize knowledge graphs with the orbital layer.

| In scope | Out of scope |
|---|---|
| Multi-week autonomous operation | Full self-driving |
| LoRA fine-tuning in field | Unsupervised training |
| Orbital validation via Starlink | Real-time teleoperation |
| Knowledge graph uplink | Global model training |
| STC thermal harvester | Vehicle redesign |
| Autonomous anomaly detection | Autonomous sample return |

---

### 2. Hardware Additions

| Subsystem | Quantity | Status |
|---|---|---|
| STC thermal harvester array | 1 | `[PROTO]` |
| Enhanced Starlink antenna | 1 | `[COTS]` |
| Extended empirical vault storage | 1 | `[COTS]` |
| Redundant Node 01 | 1 | `[COTS]` |

---

### 3. Tasks

| ID | Task | Deliverable | Status |
|---|---|---|---|
| P3-01 | Integrate STC harvester | Thermal recovery | `[PLANNED]` |
| P3-02 | Deploy LoRA fine-tuning pipeline | Adapter training | `[PLANNED]` |
| P3-03 | Implement knowledge graph compression | Uplink-ready graphs | `[PLANNED]` |
| P3-04 | Establish orbital validation loop | Bidirectional sync | `[PLANNED]` |
| P3-05 | Run 7-day autonomous mission | Mission log | `[PLANNED]` |
| P3-06 | Run 30-day autonomous mission | Mission log | `[PLANNED]` |
| P3-07 | Validate adapter rollback | Rollback procedure | `[PLANNED]` |
| P3-08 | Measure STC contribution | Thermal report | `[PLANNED]` |
| P3-09 | Validate anomaly detection autonomy | Detection report | `[PLANNED]` |
| P3-10 | Test operator override in all states | Override log | `[PLANNED]` |
| P3-11 | Measure long-term data integrity | Integrity report | `[PLANNED]` |
| P3-12 | Document lessons learned | Lessons report | `[PLANNED]` |

---

### 4. Success Criteria

| Criterion | Target | Method |
|---|---|---|
| 7-day mission | No critical failure | Mission log |
| 30-day mission | No critical failure | Mission log |
| LoRA adapter quality | No degradation on validation set | Benchmark |
| Knowledge graph compression | > 20:1 ratio | Measurement |
| Orbital sync | > 95% success rate | Sync log |
| STC contribution | > 5% cooling load reduction | Thermal measurement |
| Operator override | < 60 s response | Timed test |

---

### 5. Deliverables

| Deliverable | Format | Location |
|---|---|---|
| 7-day mission log | Markdown + MCAP | `02_ROADMAP/phase_3_report.md` |
| 30-day mission log | Markdown + MCAP | `02_ROADMAP/phase_3_report.md` |
| LoRA adapters | Binary | Empirical vault |
| Knowledge graphs | JSON-LD | Empirical vault |
| Lessons report | Markdown | `02_ROADMAP/phase_3_report.md` |

---

### 6. Exit Criteria

Phase 3 is complete when:

1. All twelve tasks are executed.
2. 30-day autonomous mission is completed.
3. LoRA adapters are validated and rolled back at least once.
4. Orbital validation loop is operational.
5. Final system report is committed.

---

### 7. Risk

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| LoRA adapter degradation | Medium | Model quality | Held-out validation set |
| Orbital sync failure | High | Knowledge loss | Local retry queue |
| STC underperformance | High | Energy budget | Defer to Phase 4 |
| 30-day mission failure | Medium | Schedule slip | Incremental mission length |
| Operator override latency | Low | Safety | Dedicated override channel |

---

### 8. Post-Phase 3

Phase 3 is the final planned phase. Beyond it, the platform is a validated research instrument. Further development is mission-specific and outside the scope of this repository.
