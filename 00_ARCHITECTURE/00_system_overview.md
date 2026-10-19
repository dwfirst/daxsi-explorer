# Block 0: System Overview — Data Flow, Boundaries, and Integration Points

This document defines the top-level system: what enters, what leaves, which subsystems exchange what, and where the boundaries of the platform lie. It is the reference map for all other architecture documents.

---

### 1. System Boundaries

The DAXSI platform is defined by four boundaries. Anything outside them is not part of the system.

| Boundary | Defined by | Excludes |
|---|---|---|
| Physical | Cybertruck envelope + CyberHab pod + deployed manipulator + portable kit radius | External infrastructure, fixed base stations |
| Compute | Node 01 + Node 02 + Node 03 | Cloud clusters, xAI data centers |
| Data | Sensor matrix + empirical vault + knowledge graph | Open web, social media, scraped corpora |
| Authority | Human operator in the loop | Autonomous decision without human arbitration |

The platform is self-sufficient within these boundaries. It does not require continuous connectivity to function.

---

### 2. Data Flow
Physical World
│
▼
[Sensor Matrix] ─────────────────────────────────────┐
optical, thermal, LiDAR, radiation, │
chemical, acoustic, kinetic │
│ │
│ raw streams, timestamped via PTP │
▼ │
[ROS 2 Middleware] │
iceoryx zero-copy for high-bandwidth streams │
ESP-NOW mesh for low-frequency telemetry │
│ │
▼ │
[Isolation Enclosure] │
EMI, vibration, thermal protection │
│ │
▼ │
[Compute Cluster] │
Node 01: routing, voice, real-time control │
Node 02: inference, vision, hypothesis generation │
Node 03: RAG index, knowledge graph, cold storage │
│ │
├──► [Manipulator] ← ROS 2 action │
├──► [Thermal Harvesting] → 48V rail │
│ │
▼ │
[AI Layer] │
Local backbone: open-weights 70B │
Orbital validation: xAI Grok via Starlink │
Human-in-the-Loop: operator console │
│ │
▼ │
[Knowledge Graph] ── compressed ──► Starlink ──► xAI │
│ │
▼ │
[Empirical Vault] ← raw logs, append-only │

text

---

### 3. Subsystem Interfaces

Each interface is a contract. It defines what is exchanged, in which format, and at what rate.

| From | To | Medium | Format | Rate | Status |
|---|---|---|---|---|---|
| Sensor matrix | ROS 2 | GigE / GMSL2 / USB | ROS 2 messages | up to 800 MB/s | `[COTS]` |
| ROS 2 | Node 01 | iceoryx shared memory | zero-copy frames | 10 Gbps | `[COTS]` |
| ESP32 mesh | Gateway | ESP-NOW | serialized telemetry | < 1 kbps per node | `[COTS]` |
| Node 01 | Node 02 | ODU optical | inference requests | 10 Gbps | `[COTS]` |
| Node 02 | Node 03 | ODU optical | RAG queries, embeddings | 10 Gbps | `[COTS]` |
| Node 03 | Empirical vault | NVMe over Ethernet | Parquet / MCAP | 10 Gbps | `[COTS]` |
| Node 01 | Manipulator | ODU optical | ROS 2 actions | 1 Gbps | `[COTS]` |
| Node 01 | Portable kit | Starlink / optical | compressed packets | 25–100 Mbps | `[COTS]` |
| Node 02 | xAI layer | Starlink | knowledge graphs | 10–50 Mbps | `[COTS]` |
| Powershare | PDU | 240V AC | electrical | 9.6 kW max | `[COTS]` |
| PDU | All blocks | 48V DC shielded | electrical | 2.4 kW peak | `[COTS]` |

No copper data conductor crosses an enclosure boundary. Optical links are used wherever galvanic isolation is required.

---

### 4. Operational States

The platform operates in four states. Transitions are controlled by the operator or by autonomous logic with human override.

| State | Description | Compute draw | Sensors | Manipulator | Starlink |
|---|---|---|---|---|---|
| **Idle** | Parked, low-power monitoring | 0.2 kW | Minimal | Stowed | Off |
| **Static monitoring** | Stationary observation, weeks | 1.5 kW | Full | Stowed | Periodic |
| **Mobile survey** | Driving, data collection | 2.3 kW | Full | Stowed | Off |
| **Deployed** | Stationary, manipulator active, kit deployed | 2.3 kW | Full | Active | On |

State transitions are logged with timestamps and operator ID.

---

### 5. Failure Modes and Degradation

The platform is designed to degrade, not to fail abruptly.

| Failure | Effect | Mitigation | Status |
|---|---|---|---|
| Node 02 loss | Inference degraded | Fall back to Node 01 backbone (smaller model) | `[COTS]` |
| Node 03 loss | RAG unavailable | Local cache on Node 02 continues | `[COTS]` |
| Starlink loss | No orbital validation | Full local autonomy continues | `[COTS]` |
| Sensor loss | Partial perception | Redundant coverage from overlapping sensors | `[COTS]` |
| Coolant leak | Thermal throttling | Automatic derating, operator alert | `[COTS]` |
| Powershare loss | Battery-only operation | Static monitoring continues at 1.5 kW | `[COTS]` |
| HSLDS failure | Reduced vibration isolation | Wire-rope isolators remain functional | `[PROTO]` |

No single failure removes the platform's ability to collect data and preserve it locally.

---

### 6. Integration Points

The platform is designed to integrate with external systems without depending on them.

| External system | Integration | Dependency |
|---|---|---|
| xAI Grok | API over Starlink | Optional |
| Tesla Powershare | 240V AC outlet | Required for continuous operation |
| Starlink constellation | Satellite link | Optional |
| Academic baselines | Pre-loaded knowledge graph | Required, pre-loaded |
| Operator | Voice, console, portable kit | Required |

The platform is functional without xAI and without Starlink. It is not functional without an operator.

---

### 7. Document Map

| Document | Scope |
|---|---|
| `01_compute_cluster.md` | Node topology, hardware, expansion path |
| `02_sensor_matrix.md` | Sensor selection by physical field |
| `03_cyber_hab.md` | Habitation pod, ECLSS, dry architecture |
| `04_ai_training_rag.md` | Model hierarchy, RAG tiers, fine-tuning |
| `05_manipulator.md` | Retractable arm, stowage, control |
| `06_portable_kit.md` | Handheld suite, field link |
| `07_isolation_enclosure.md` | Enclosure layers, EMI, vibration |
