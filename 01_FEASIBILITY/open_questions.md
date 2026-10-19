# Open Questions — Unresolved Engineering and Validation Items

This document is the single register of unresolved items across the DAXSI platform. Each item is tracked until it is closed by test, analysis, or design change. An item is not a weakness; it is a scheduled task.

---

### 1. Format

| Field | Meaning |
|---|---|
| ID | Sequential identifier |
| Item | The question or unresolved issue |
| Domain | Compute, sensors, thermal, isolation, AI, manipulator, kit, hab |
| Impact | What is blocked until resolved |
| Method | How it will be resolved (test, analysis, design) |
| Phase | Earliest phase where resolution is required |
| Status | Open, in progress, closed |

---

### 2. Register

| ID | Item | Domain | Impact | Method | Phase | Status |
|---|---|---|---|---|---|---|
| OQ-01 | STC efficiency at vehicle-scale heat flow | Thermal | Energy recovery estimate | Bench test | 2 | Open |
| OQ-02 | Radiator capacity at 40 °C ambient, 15 m/s airflow | Thermal | Node 02 peak derating | Wind tunnel | 1 | Open |
| OQ-03 | PCM discharge time at −30 °C ambient | Thermal | Cold-weather endurance | Simulation + field | 1 | Open |
| OQ-04 | HSLDS module validation under combined vibration and thermal load | Isolation | Adaptive isolation | Bench + field | 2 | Open |
| OQ-05 | Aluminum foam behavior under repeated thermal cycling | Isolation | Long-term damping | Accelerated life test | 1 | Open |
| OQ-06 | Faraday liner seam integrity after repeated opening | Isolation | EMI shield continuity | Maintenance procedure test | 1 | Open |
| OQ-07 | Optical port contamination rate in dust | Isolation | Maintenance interval | Field test | 2 | Open |
| OQ-08 | SWIR snapshot sensor reliability under vibration | Sensors | ATH1700-17 deployment | Vibration table | 1 | Open |
| OQ-09 | Infrasound coverage below 20 Hz | Sensors | Acoustic field completeness | Geophone integration study | 2 | Open |
| OQ-10 | Hyperspectral data volume compression strategy | Sensors | Real-time pipeline | Algorithm development | 1 | Open |
| OQ-11 | ESP-NOW mesh range under vehicle body obstruction | Sensors | Peripheral telemetry | Field measurement | 1 | Open |
| OQ-12 | Milvus/Qdrant index size for full academic baseline | AI | RAG tier 2 capacity | Measurement | 1 | Open |
| OQ-13 | LoRA fine-tuning power budget during static monitoring | AI | Training schedule | Measurement | 2 | Open |
| OQ-14 | Knowledge graph compression ratio for Starlink uplink | AI | Orbital validation bandwidth | Field measurement | 3 | Open |
| OQ-15 | Operator annotation throughput under field conditions | AI | Human-in-the-loop rate | Field study | 2 | Open |
| OQ-16 | Stowed manipulator vibration tolerance | Manipulator | Phase 2 readiness | Vibration table + field | 2 | Open |
| OQ-17 | End effector swap mechanism field-replaceable design | Manipulator | Mission flexibility | Design study | 2 | Open |
| OQ-18 | Arm reach vs. vehicle geometry | Manipulator | Deployment envelope | CAD integration | 1 | Open |
| OQ-19 | Portable kit mass reduction to 8 kg target | Kit | Operator load | Design trade study | 2 | Open |
| OQ-20 | Starlink Mini power draw vs. battery life | Kit | Field endurance | Measurement | 1 | Open |
| OQ-21 | Tablet ruggedness under IP67 field use | Kit | Kit reliability | Environmental test | 1 | Open |
| OQ-22 | Dry composting lavatory multi-week performance | Hab | Habitation endurance | Field validation | 2 | Open |
| OQ-23 | Pod mass confirmation after hardware selection | Hab | CG and stability | Weighing | 1 | Open |
| OQ-24 | Positive-pressure ECLSS power at −30 °C | Hab | Cold-weather endurance | Environmental chamber | 1 | Open |
| OQ-25 | 8-GPU expansion chassis structural integration | Compute | Stage 1 expansion | Design study | 2 | Open |
| OQ-26 | L40S thermal profile in sealed enclosure at 40 °C | Compute | Stage 2 expansion | Thermal test | 2 | Open |
| OQ-27 | Real-time kernel latency under full sensor load | Compute | Node 01 determinism | Bench test | 1 | Open |
| OQ-28 | Second PCIe chassis structural modification of Vault | Compute | Stage 1 expansion | CAD + structural analysis | 2 | Open |

---

### 3. Resolution Flow
Open
│
▼
In progress ──► Closed (test/analysis)
│
▼
Design change required
│
▼
Re-open as new item

text

An item is closed only when a documented result exists in the repository. Verbal resolution is not closure.

---

### 4. Critical Path Items

The following items block phase transitions and must be resolved before the phase begins.

| Phase | Blocking items |
|---|---|
| Phase 1 → Phase 2 | OQ-08, OQ-10, OQ-12, OQ-18, OQ-20, OQ-23, OQ-24, OQ-27 |
| Phase 2 → Phase 3 | OQ-01, OQ-04, OQ-07, OQ-09, OQ-13, OQ-15, OQ-16, OQ-17, OQ-19, OQ-22, OQ-25, OQ-26, OQ-28 |

Items not on the critical path may be resolved in parallel with phase execution.

---

### 5. Governance

The register is reviewed at each phase gate. Items that remain open beyond their phase are escalated. Items that are resolved are moved to a closed register with reference to the test report.

No item is deleted. Closed items remain in the register with a resolution reference.
