# Block 1: Compute Cluster — Node Topology, Hardware, and Expansion Path

This document defines the compute architecture of the DAXSI platform. It replaces the previous fixed-VRAM specification with a block-scalable topology: a validated baseline that can be expanded through defined nodes without redesigning the system.

---

### 1. Design Principle

The cluster is organized by function, not by capacity. Each node has a distinct role, a distinct failure mode, and a distinct expansion path. Capacity is a consequence of the topology, not a target.

| Principle | Implementation |
|---|---|
| Functional separation | Command, inference, and storage are separate nodes |
| Fail-degraded | Loss of any node degrades capability, does not stop operation |
| Block-scalable | Each node can be expanded independently |
| Galvanic isolation | Optical links between nodes |
| Thermal isolation | Each node has its own coolant loop segment |

---

### 2. Baseline Topology

| Node | Role | Hardware | VRAM | Power (peak) | Status |
|---|---|---|---|---|---|
| 01 | Command Core | 2× Jetson AGX Orin 64GB | 128 GB unified | 0.12 kW | `[COTS]` |
| 02 | Inference Primary | 4× RTX 6000 Ada 48GB | 192 GB | 1.8 kW | `[COTS]` |
| 03 | RAG / Storage | 8× Jetson Orin NX 16GB | 128 GB unified | 0.16 kW | `[COTS]` |
| **Baseline total** | | | **448 GB combined** | **1.48 kW** | |

The baseline is fully COTS. Every component is available today.

---

### 3. Node Roles

#### Node 01 — Command Core

| Parameter | Value |
|---|---|
| Location | Front cabin console, isolated enclosure |
| Hardware | 2× Jetson AGX Orin 64GB |
| VRAM | 128 GB |
| Role | Operator interface, voice pipeline, real-time routing, manipulator control |
| Operating model | Whisper Large v3 + 9B-class local model, FP16/INT8 |
| Latency target | < 300 ms voice round-trip |

Node 01 is the only node that directly faces the operator. It does not run heavy inference. Its job is to route, translate, and control.

#### Node 02 — Inference Primary

| Parameter | Value |
|---|---|
| Location | CyberHab lower equipment deck, isolated enclosure |
| Hardware | 4× RTX 6000 Ada 48GB, PCIe cluster |
| VRAM | 192 GB |
| Role | Heavy local LLM execution, vision processing, hypothesis generation |
| Operating model | Open-weights 70B, 4-bit AWQ |
| Latency target | < 2 s for first token; ~60 s for 1k-token generation on baseline |

Node 02 is the primary reasoning engine. It runs the local Local backbone and the multi-spectral vision pipeline.

#### Node 03 — RAG and Storage

| Parameter | Value |
|---|---|
| Location | CyberHab shock-isolated vertical rack |
| Hardware | 8× Jetson Orin NX 16GB |
| VRAM | 128 GB |
| Role | Vector embedding, knowledge graph indexing, cold storage write-heads |
| Operating model | Milvus or Qdrant, distributed across 8 nodes |
| Index target | Pre-loaded academic baseline + field logs |

Node 03 is the memory of the platform. It does not generate; it retrieves and indexes.

---

### 4. Expansion Path

The baseline is not the ceiling. Each node has a defined expansion path.

| Stage | Node 02 | Node 03 | Total VRAM | Status |
|---|---|---|---|---|
| Baseline | 4× RTX 6000 Ada (192 GB) | 8× Orin NX (128 GB) | 448 GB combined | `[COTS]` |
| Stage 1 | 8× RTX 6000 Ada (384 GB) | 8× Orin NX (128 GB) | 640 GB | `[COTS]` |
| Stage 2 | 8× RTX 6000 Ada (384 GB) | 4× L40S (192 GB) | 704 GB | `[COTS]` |
| Stage 3 | External dock | External dock | ~1 TB | `[CONCEPT]` |

Stage 1 requires a second PCIe chassis in the Vault. Stage 2 replaces the Jetson array with L40S cards. Stage 3 is a trailer or static dock and is not part of the vehicle envelope.

The hard ceiling is not VRAM availability. It is the Cybertruck Powershare power and thermal envelope. At 9.6 kW continuous, the platform cannot sustain more than approximately 2.5 kW of compute without exceeding headroom for cooling and ECLSS.

---

### 5. Inter-Node Links

| Link | Medium | Bandwidth | Purpose |
|---|---|---|---|
| Node 01 ↔ Node 02 | ODU optical | 10 Gbps | Inference requests, vision frames |
| Node 02 ↔ Node 03 | ODU optical | 10 Gbps | RAG queries, embeddings |
| Node 03 ↔ Vault NVMe | Ethernet | 10 Gbps | Raw log write |
| Node 01 ↔ Manipulator | ODU optical | 1 Gbps | ROS 2 actions |
| Node 01 ↔ Portable kit | Starlink or optical | 25–100 Mbps | Field data |

No copper data conductor crosses an enclosure boundary. See `07_isolation_enclosure.md`.

---

### 6. Software Stack

| Layer | Component | Status |
|---|---|---|
| OS | Ubuntu 22.04 LTS with real-time kernel | `[COTS]` |
| Middleware | ROS 2 Humble, iceoryx | `[COTS]` |
| Inference | vLLM, TensorRT-LLM | `[COTS]` |
| Quantization | AWQ 4-bit, GPTQ | `[COTS]` |
| Vision | YOLOv11 multi-spectral, custom heads | `[COTS]` |
| Vector DB | Milvus or Qdrant | `[COTS]` |
| Fine-tuning | PEFT, LoRA, QLoRA | `[COTS]` |
| Orchestration | systemd units, ROS 2 launch | `[COTS]` |

---

### 7. Power Budget by Node

| Node | Peak | Average | Notes |
|---|---|---|---|
| Node 01 | 0.12 kW | 0.08 kW | Continuous |
| Node 02 | 1.8 kW | 1.2 kW | Burst on inference |
| Node 03 | 0.16 kW | 0.12 kW | Continuous |
| Cooling (shared) | 0.3 kW | 0.15 kW | Variable |
| **Cluster total** | **2.38 kW** | **1.55 kW** | Within 9.6 kW envelope |

The cluster leaves approximately 7.8 kW of headroom at peak for ECLSS, sensors, and charging. This headroom is the real constraint on expansion, not VRAM.

---

### 8. Open Questions

1. Second PCIe chassis for Stage 1 requires structural modification of the Vault. Not yet designed.
2. L40S thermal profile in a sealed enclosure at 40 °C ambient is not validated.
3. Milvus/Qdrant index size for full academic baseline on 128 GB VRAM requires measurement.
4. Real-time kernel latency under full sensor load requires bench validation.

See `01_FEASIBILITY/open_questions.md`.




