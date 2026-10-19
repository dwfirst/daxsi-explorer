# Use Case - Materials and Chemistry

This document defines the materials and chemistry research domain of the DAXSI platform. It describes what is measured, which sensors are used, and what constitutes a valid result.

---

### 1. Domain Scope

| In scope | Out of scope |
|---|---|
| Stress and degradation analysis | Synthesis |
| Corrosion detection | Bulk chemical processing |
| Mineral identification | Industrial production |
| Composite inspection | Destructive testing |
| Surface chemistry | Long-term aging studies |

The platform observes and characterizes. It does not synthesize or process materials.

---

### 2. Measurement Methods

| Method | Sensor | Output | Status |
|---|---|---|---|
| Hyperspectral reflectance | ULTRIS X20, ATH1700-17, SX25 | Material signature | `[COTS]` |
| Thermal emission | FLIR Boson 640 | Thermal anomaly map | `[COTS]` |
| LiDAR geometry | Ouster OS1-128 | Surface morphology | `[COTS]` |
| Gas emission | BME680, NDIR, electrochemical | Volatile signature | `[COTS]` |
| Radiation | SBM-20 | Radiation flux | `[COTS]` |

No method is destructive. All measurements are non-contact or minimally invasive.

---

### 3. Sensor-to-Material Mapping

| Material property | Primary sensor | Secondary sensor |
|---|---|---|
| Mineral composition | SWIR 900-2500 nm | VNIR 400-1000 nm |
| Moisture content | SWIR 1450/1940 nm | LWIR thermal |
| Oxidation state | VNIR 400-1000 nm | Gas emission |
| Surface roughness | LiDAR | VIS imaging |
| Thermal conductivity | LWIR | Contact probe (kit) |
| Volatile emission | Gas analyzer | LWIR |

---

### 4. Experiment Protocol

| Step | Action | Tool |
|---|---|---|
| 1 | Detect anomaly | YOLOv11 multi-spectral |
| 2 | Operator confirms target | Console |
| 3 | Capture hyperspectral cube | ULTRIS / ATH1700 |
| 4 | Capture thermal map | FLIR Boson |
| 5 | Capture LiDAR geometry | Ouster OS1-128 |
| 6 | Sample gas if applicable | Kit gas analyzer |
| 7 | Compare against baseline | RAG tier 2 |
| 8 | Operator annotates | Console |
| 9 | Store validated record | Empirical vault |
| 10 | Trigger fine-tuning | Static state |

---

### 5. Result Format

| Field | Type | Description |
|---|---|---|
| ID | UUID | Unique record identifier |
| Timestamp | ISO 8601 | PTP-synchronized |
| Position | RTK-GPS | Lat, lon, alt |
| Orientation | IMU quaternion | Local reference |
| Spectral cube | Array | Wavelength, intensity |
| Thermal map | Array | Temperature |
| Point cloud | Array | XYZ, intensity |
| Gas signature | Array | Species, concentration |
| Operator note | Text | Free annotation |
| Model hypothesis | Text | Local backbone output |
| Orbital validation | Text | Grok output, if available |

---

### 6. Validation Criteria

A materials record is valid when:

1. All sensors are calibrated within the last mission.
2. PTP alignment is within 1 µs.
3. Operator has annotated the record.
4. Baseline comparison is documented.
5. Record is stored in the empirical vault.

A record that fails any criterion is flagged and excluded from training.

---

### 7. Open Questions

1. SWIR sensor field reliability under vibration is not validated. See OQ-08.
2. Hyperspectral compression for storage is not defined. See OQ-10.
3. Mineral identification accuracy against academic baseline is not measured.
4. Corrosion detection minimum detectable area is not defined.
