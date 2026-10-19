# Block 6: Portable Kit — Handheld Measurement Suite and Field Link

This document defines the portable measurement kit carried by the operator into locations the vehicle cannot reach. The kit is a thin client: it collects, compresses, and transmits to Node 02 or to the orbital layer. It does not run heavy AI locally.

---

### 1. Design Principle

The kit extends the sensor matrix beyond the vehicle envelope without duplicating the compute cluster.

| Principle | Implementation |
|---|---|
| Thin client | Collection and compression only; no local inference |
| Field link | Starlink Mini or optical tether to Node 01 |
| Low mass | Total kit mass under 8 kg |
| Battery life | 8 hours continuous, 24 hours intermittent |
| Environmental rating | IP67, −20 °C to +50 °C |

The kit is not a second laboratory. It is a remote probe with a human carrier.

---

### 2. Kit Composition

| Component | Function | Mass | Status |
|---|---|---|---|
| LiSen iSpecField-HH | Spectroradiometer, 200–1100 nm | 1.8 kg | `[COTS]` |
| LiSen iSpecField-WNIR-HH | Spectroradiometer, 350–2500 nm | 2.0 kg | `[COTS]` |
| ECOPROBE 5 | PID + IR gas analyzer, VOC/CH4/CO2 | 3.0 kg | `[COTS]` |
| Starlink Mini | Satellite link, 25–40 W | 1.16 kg | `[COTS]` |
| Tablet | ROS 2 client, compressed uplink | 0.6 kg | `[COTS]` |
| Battery pack | 48V, 300 Wh | 1.5 kg | `[COTS]` |
| **Total** | | **~10 kg** | |

Total mass slightly exceeds the 8 kg target. The WNIR instrument is the heaviest single item and can be omitted for missions that do not require 2500 nm coverage.

---

### 3. Field Link Architecture

| Link | Medium | Bandwidth | Range | Status |
|---|---|---|---|---|
| Primary | Starlink Mini | 25–100 Mbps | Global (sky view required) | `[COTS]` |
| Secondary | Optical tether | 10 Gbps | 100 m | `[COTS]` |
| Tertiary | Wi-Fi 6 | 100 Mbps | 500 m line-of-sight | `[COTS]` |

The kit does not depend on a single link. If Starlink is obstructed, the optical tether connects to the vehicle. If the vehicle is out of range, Wi-Fi covers the gap.

---

### 4. Data Flow
Operator in field
│
▼
[Handheld instruments]
│ raw data, local storage
▼
[Tablet]
│ compression, metadata tagging
▼
[Link: Starlink / optical / Wi-Fi]
│
▼
[Node 01] ──► [Node 02] ──► [Node 03]
│
▼
[Knowledge graph update]
│
▼
[Operator feedback on tablet]

text

The kit receives back annotations, hypotheses, and next-step guidance from Node 02. This closes the loop between field collection and onboard inference.

---

### 5. Compression Strategy

Raw hyperspectral and gas data are too large for satellite uplink. The kit compresses before transmission.

| Data type | Compression | Ratio | Status |
|---|---|---|---|
| Hyperspectral cube | PCA + quantization | 20:1 | `[COTS]` |
| Gas time series | Delta encoding | 10:1 | `[COTS]` |
| Spectra | Downsample + metadata | 50:1 | `[COTS]` |
| Images | JPEG XL | 5:1 | `[COTS]` |

Compression is lossy for transmission and lossless for local storage. The kit retains the full raw dataset on internal storage until it is physically returned to the vehicle.

---

### 6. Operational Modes

| Mode | Description | Link | Power |
|---|---|---|---|
| Tethered | Operator within 100 m of vehicle | Optical | 15 W |
| Local | Operator within Wi-Fi range | Wi-Fi 6 | 20 W |
| Remote | Operator beyond vehicle range | Starlink Mini | 40 W |
| Offline | No link available | None | 10 W |

In Offline mode, the kit stores data locally and synchronizes when a link is restored.

---

### 7. Open Questions

1. Total kit mass exceeds 8 kg target by ~2 kg. WNIR instrument is the primary contributor.
2. Starlink Mini power draw at 40 W reduces battery life to ~6 hours continuous.
3. Compression ratio for hyperspectral cubes under field conditions requires measurement.
4. Tablet ruggedness under IP67 field use requires validation.

See `01_FEASIBILITY/open_questions.md`.
