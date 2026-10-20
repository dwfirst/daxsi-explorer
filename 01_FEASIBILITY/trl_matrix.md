# TRL Matrix — Technology Readiness Level by Component

This document assigns a Technology Readiness Level (TRL) to every major component of the DAXSI platform. TRL is defined on the NASA scale: 1 is basic principle, 9 is flight-proven in operational environment. The purpose is to separate what can be built today from what requires development.

---

### 1. TRL Scale Reference

| TRL | Definition |
|---|---|
| 1 | Basic principles observed |
| 2 | Technology concept formulated |
| 3 | Experimental proof of concept |
| 4 | Component validated in laboratory |
| 5 | Component validated in relevant environment |
| 6 | Prototype demonstrated in relevant environment |
| 7 | Prototype demonstrated in operational environment |
| 8 | System complete and qualified |
| 9 | System proven in operational environment |

---

### 2. Compute Cluster

| Component | TRL | Basis | Status |
|---|---|---|---|
| Jetson AGX Orin 64GB | 9 | Shipping product, deployed in robotics | `[COTS]` |
| Jetson Orin NX 16GB | 9 | Shipping product | `[COTS]` |
| RTX 6000 Ada 48GB | 9 | Shipping product | `[COTS]` |
| 4-GPU PCIe cluster in sealed enclosure | 6 | Prototype demonstrated in lab, not in vehicle | `[PROTO]` |
| 8-GPU expansion chassis | 4 | Component validated in lab, not integrated | `[PROTO]` |
| L40S in sealed enclosure | 5 | Component validated, thermal profile not validated | `[PROTO]` |
| External dock (~1 TB) | 2 | Concept only | `[CONCEPT]` |

---

### 3. Sensor Matrix

| Component | TRL | Basis | Status |
|---|---|---|---|
| Cubert ULTRIS X20 | 8 | Commercial product, field-deployed | `[COTS]` |
| Specim AFX10 | 9 | Commercial, airborne baseline | `[COTS]` |
| ATH1700-17 | 7 | Commercial, limited field data under vibration | `[PROTO]` |
| Specim SX25 | 8 | Commercial, static deployment | `[COTS]` |
| FLIR Boson 640 | 9 | Shipping product, widely deployed | `[COTS]` |
| Ouster OS1-128 | 9 | Shipping product, IP68/69K | `[COTS]` |
| SBM-20 Geiger | 9 | Shipping product | `[COTS]` |
| Fluxgate magnetometer | 9 | Shipping product | `[COTS]` |
| Bosch BME680 | 9 | Shipping product | `[COTS]` |
| ReSpeaker Mic Array v2.0 | 8 | Commercial, widely deployed | `[COTS]` |
| Bosch BNO055 | 9 | Shipping product | `[COTS]` |

---

### 4. Isolation Enclosure

| Component | TRL | Basis | Status |
|---|---|---|---|
| 6061-T6 aluminum shell | 9 | Standard fabrication | `[COTS]` |
| Aluminum foam damping | 7 | Commercial, limited vehicle integration data | `[PROTO]` |
| Faraday CYBER NC-Berry Rip Stop | 7 | Commercial, 75 dB rating, not vehicle-validated | `[PROTO]` |
| Wire-rope isolators | 9 | Aerospace standard | `[COTS]` |
| HSLDS magnetic-pneumatic modules | 4 | Lab prototype, no field data | `[PROTO]` |
| ODU Expanded Beam ports | 9 | MIL-DTL-38999 qualified | `[COTS]` |
| Hermetic coolant feedthroughs | 9 | Standard industrial | `[COTS]` |

---

### 5. Thermal and Energy Harvesting

| Component | TRL | Basis | Status |
|---|---|---|---|
| Liquid loop A (GPU to roof) | 7 | Commercial, vehicle integration not validated | `[PROTO]` |
| Liquid loop B (chassis contact) | 8 | Commercial, widely deployed | `[COTS]` |
| PCM buffer | 6 | Prototype demonstrated in EV context | `[PROTO]` |
| STC thermal harvester | 4 | PoC demonstrated in data center, not vehicle | `[PROTO]` |

---

### 6. AI Layer

| Component | TRL | Basis | Status |
|---|---|---|---|
| Open-weights 70B inference on 4× RTX 6000 Ada | 9 | Standard deployment | `[COTS]` |
| 4-bit AWQ quantization | 9 | Standard practice | `[COTS]` |
| Whisper Large v3 on Jetson | 9 | Standard deployment | `[COTS]` |
| Milvus/Qdrant on 8× Orin NX | 6 | Prototype, index size not measured | `[PROTO]` |
| LoRA/QLoRA on validated logs | 8 | Standard practice | `[COTS]` |
| xAI Grok via Starlink | 8 | Commercial API, link not field-validated | `[PROTO]` |
| Knowledge graph compression for uplink | 5 | Component validated, ratio not measured | `[PROTO]` |

---

### 7. Manipulator

| Component | TRL | Basis | Status |
|---|---|---|---|
| igus ReBeL 6DOF class arm | 9 | Shipping product | `[COTS]` |
| Stowage bay with IP67 seal | 6 | Prototype, not vehicle-validated | `[PROTO]` |
| ROS 2 action control | 9 | Standard | `[COTS]` |
| Vibration tolerance in stowed position | 4 | Not validated | `[PROTO]` |

---

### 8. Portable Kit

| Component | TRL | Basis | Status |
|---|---|---|---|
| LiSen iSpecField-HH | 8 | Commercial, field-deployed | `[COTS]` |
| LiSen iSpecField-WNIR-HH | 8 | Commercial, field-deployed | `[COTS]` |
| ECOPROBE 5 | 8 | Commercial, field-deployed | `[COTS]` |
| Starlink Mini | 9 | Shipping product | `[COTS]` |
| Compression strategy | 6 | Prototype, ratio not measured | `[PROTO]` |

---

### 9. CyberHab

| Component | TRL | Basis | Status |
|---|---|---|---|
| Carbon fiber pod shell | 7 | Commercial, vehicle integration not validated | `[PROTO]` |
| Dry composting lavatory | 8 | Commercial, widely deployed | `[COTS]` |
| HEPA H14 + charcoal ECLSS | 9 | Standard | `[COTS]` |
| 48V heat pump | 8 | Commercial | `[COTS]` |
| Positive-pressure cabin | 8 | Standard | `[COTS]` |
| Solar array 1.2 kW | 9 | Standard | `[COTS]` |

---

### 10. System-Level Assessment

| Phase | Target TRL | Basis |
|---|---|---|
| Phase 0 (bench) | 4 | Component validated in laboratory |
| Phase 1 (static) | 5 | Component validated in relevant environment |
| Phase 2 (mobile) | 6 | Prototype demonstrated in relevant environment |
| Phase 3 (autonomous) | 7 | Prototype demonstrated in operational environment |

Current system-level TRL: 2-3. The concept is documented and the architecture is specified, but no bench validation has been performed. No software component has been implemented and no test report has been produced. Individual COTS components are TRL 8-9. The gap is integration and validation, not invention.

---

### 11. Critical Path Items

Components with TRL below 6 that block system-level readiness.

| Component | TRL | Blocking |
|---|---|---|
| HSLDS modules | 4 | Phase 2 adaptive isolation |
| STC thermal harvester | 4 | Phase 2 energy recovery |
| Stowed manipulator vibration | 4 | Phase 2 mobile survey |
| Knowledge graph compression | 5 | Phase 3 orbital validation |
| Milvus/Qdrant index size | 6 | Phase 1 RAG |

These items define the development schedule. Everything else is integration.


