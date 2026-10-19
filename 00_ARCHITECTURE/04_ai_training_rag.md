# Block 4: AI Layer — Local Backbone, Orbital Validation, and Human-in-the-Loop

This document defines the AI architecture of the DAXSI platform. The layer is split into three tiers with distinct roles: local inference, orbital validation, and human arbitration. No tier is optional except the orbital one.

---

### 1. Design Principle

The AI layer is model-agnostic at the edge and xAI-aligned at the orbit. This is a deliberate split.

| Tier | Location | Role | Dependency |
|---|---|---|---|
| Local backbone | Node 02, Node 01 | Inference, vision, routing | None |
| Orbital validation | xAI Grok via Starlink | Cross-check, enrichment, weight updates | Starlink link |
| Human-in-the-Loop | Operator console | Arbitration, annotation, training trigger | Operator |

The platform is functional without the orbital tier. It is not functional without the human tier.

---

### 2. Local Backbone

| Parameter | Value | Status |
|---|---|---|
| Model class | Open-weights 70B | `[COTS]` |
| Quantization | 4-bit AWQ | `[COTS]` |
| Execution platform | Node 02, 4× RTX 6000 Ada | `[COTS]` |
| Inference engine | vLLM, TensorRT-LLM | `[COTS]` |
| Latency target | < 2 s for 1k-token generation | `[COTS]` |
| Model identity | Model-agnostic; any open-weights checkpoint | `[COTS]` |

The local backbone is not tied to a specific model family. The checkpoint is a configuration parameter, not an architectural commitment. This allows substitution as open-weights models improve.

#### Command-tier model

| Parameter | Value | Status |
|---|---|---|
| Role | Voice, routing, fast context | `[COTS]` |
| Hardware | Node 01, 2× Jetson AGX Orin | `[COTS]` |
| Model class | 9B, FP16/INT8 | `[COTS]` |
| Voice pipeline | Whisper Large v3 | `[COTS]` |
| Latency target | < 300 ms round-trip | `[COTS]` |

---

### 3. Orbital Validation Layer

| Parameter | Value | Status |
|---|---|---|
| Provider | xAI Grok | `[COTS]` |
| Transport | Starlink Mobility | `[COTS]` |
| Payload | Compressed knowledge graphs, not raw data | `[COTS]` |
| Direction | Bidirectional | `[COTS]` |
| Frequency | Periodic, operator-triggered or scheduled | `[COTS]` |

The orbital layer does not receive raw sensor data. It receives compressed, structured knowledge graphs: mathematical representations of what was detected and how it was interpreted. This preserves bandwidth and keeps raw empirical data local.

The orbital layer returns validated graphs and, optionally, adapter weights. It does not return base model weights.

---

### 4. Human-in-the-Loop Pipeline

The operator is not a user. The operator is the scientific mentor.

| Stage | Action | Trigger |
|---|---|---|
| Detection | Vision core flags anomaly | YOLOv11 multi-spectral |
| Alert | Operator notified on console | Anomaly score threshold |
| Validation | Operator tags, corrects, annotates | Voice or console |
| Storage | Validated log written to empirical vault | Confirmation |
| Training | LoRA/QLoRA on validated logs | Static state or overnight |
| Deployment | Adapter weights loaded into backbone | After validation |

The operator's corrections are the ground truth. The model does not self-validate.

---

### 5. Cascading RAG Architecture

The knowledge base is fully local. It has three tiers.

| Tier | Location | Content | Latency |
|---|---|---|---|
| 1 | Node 02 VRAM partition | Active session embeddings (current 50 km radius) | Microseconds |
| 2 | Node 03 Jetson array | Pre-loaded academic baseline (physics, chemistry, materials, geology, biomimetics) | Milliseconds |
| 3 | Empirical vault NVMe | Raw sensor logs, append-only | Seconds |

Tier 1 is ephemeral. Tier 2 is the sovereign knowledge graph. Tier 3 is the immutable record.

The vector database runs on Node 03 using Milvus or Qdrant. Index size for the full academic baseline on 128 GB VRAM is an open question. See `01_FEASIBILITY/open_questions.md`.

---

### 6. Fine-Tuning Pipeline

| Parameter | Value | Status |
|---|---|---|
| Method | LoRA / QLoRA | `[COTS]` |
| Trigger | Static monitoring state or overnight charging | `[COTS]` |
| Data source | Validated operator logs only | `[COTS]` |
| Base weights | Frozen | `[COTS]` |
| Adapter storage | Empirical vault, versioned | `[COTS]` |
| Rollback | Previous adapter preserved | `[COTS]` |

Fine-tuning never modifies base model weights. Adapters are versioned and can be rolled back. An adapter that degrades performance on a held-out validation set is rejected.

---

### 7. Failure and Degradation

| Failure | Effect | Mitigation |
|---|---|---|
| Node 02 loss | Heavy inference unavailable | Node 01 command model continues |
| Starlink loss | No orbital validation | Local autonomy continues |
| Node 03 loss | RAG unavailable | Tier 1 cache continues |
| Operator unavailable | No arbitration | Platform enters Idle state |
| Adapter corruption | Degraded model | Rollback to previous adapter |

The AI layer degrades gracefully. It does not fail closed.

---

### 8. Open Questions

1. Index size for full academic baseline on 128 GB VRAM requires measurement.
2. LoRA fine-tuning on validated logs during static monitoring requires power budget validation.
3. Knowledge graph compression ratio for Starlink uplink requires field measurement.
4. Operator annotation throughput under field conditions is unknown.

See `01_FEASIBILITY/open_questions.md`.
