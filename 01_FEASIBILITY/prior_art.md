# Prior Art - Related Work and Differentiation

This document lists prior work in autonomous scientific systems and explains how DAXSI differs. It exists to support the originality claim in `INTELLECTUAL_PROPERTY.md` with specific references rather than a general assertion.

---

### 1. Related Systems

| System | Year | Institution | Scope | Reference |
|---|---|---|---|---|
| Coscientist | 2023 | Carnegie Mellon University | Autonomous chemical synthesis with LLM planning | Boiko et al., Nature 2023 |
| A-Lab | 2023 | Lawrence Berkeley National Laboratory | Autonomous materials synthesis and characterization | Szymanski et al., Nature 2023 |
| ChemCrow | 2023 | EPFL | LLM-augmented chemistry tool use | Bran et al., 2023 |
| Claude Computer Use | 2024 | Anthropic | General computer control by LLM | Anthropic, 2024 |
| Figure 01 | 2024 | Figure AI | Humanoid robot with LLM control | Figure AI, 2024 |

---

### 2. Comparison

| Dimension | Coscientist | A-Lab | ChemCrow | DAXSI |
|---|---|---|---|---|
| Environment | Fixed laboratory | Fixed laboratory | Software only | Mobile platform |
| Primary function | Synthesis | Synthesis | Tool use | Observation and data collection |
| Sensor suite | Narrow (chemistry) | Narrow (chemistry) | None | Full-spectrum (optical, thermal, LiDAR, chemical, radiation, acoustic) |
| Compute location | Cloud or local workstation | Local workstation | Cloud | Local edge cluster, no network dependency |
| Human role | Supervisor | Supervisor | User | Scientific mentor in the loop |
| Output | Synthesized compounds | Synthesized compounds | Chemical plans | First-principles empirical datasets |
| Mobility | None | None | None | Cybertruck platform |
| Orbital validation | None | None | None | xAI Grok via Starlink |

---

### 3. Differentiation

DAXSI differs from prior work in four specific ways.

**1. Mobility.** Coscientist, A-Lab, and ChemCrow operate in fixed laboratories. DAXSI is designed to move through terrain and collect data where no laboratory exists. This is not a feature of the AI layer; it is a property of the physical platform.

**2. Observation, not synthesis.** Prior systems focus on synthesizing compounds or planning chemical reactions. DAXSI focuses on observing and characterizing the physical world. It does not synthesize. It measures.

**3. Full-spectrum perception.** Prior systems use narrow sensor suites specific to chemistry. DAXSI uses a full-spectrum matrix: optical (UV to SWIR), thermal (LWIR), LiDAR, chemical, radiation, acoustic, and kinetic. This is broader than any single prior system.

**4. Sovereign local compute.** Prior systems depend on cloud infrastructure or fixed workstations. DAXSI runs entirely on a local edge cluster with no network dependency. The orbital layer is optional and receives compressed knowledge graphs, not raw data.

---

### 4. What DAXSI Does Not Claim

DAXSI does not claim to be the first autonomous scientific system. Coscientist and A-Lab preceded it. DAXSI does not claim to be the first system to use LLMs for scientific reasoning. ChemCrow preceded it.

DAXSI claims to be the first architecture that combines mobility, full-spectrum perception, sovereign local compute, and human-in-the-loop mentorship in a single platform for first-principles data collection.

This claim is specific and falsifiable. It can be checked against the prior art listed above.

---

### 5. Open Questions

1. A systematic literature review of mobile scientific platforms is not yet performed.
2. Patent landscape for autonomous data collection in mobile platforms is not surveyed.
3. Direct comparison with field-deployed instruments (e.g., Mars rovers) is not included.
