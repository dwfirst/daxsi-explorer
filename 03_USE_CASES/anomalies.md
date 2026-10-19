# Use Case - Anomalies

This document defines the anomaly detection domain of the DAXSI platform. An anomaly is any observation that does not match the pre-loaded academic baseline within a defined tolerance. Anomalies are the primary product of the platform.

---

### 1. Domain Scope

| In scope | Out of scope |
|---|---|
| Environmental anomalies | Prediction |
| Radiation anomalies | Attribution without evidence |
| Magnetic anomalies | Classification without baseline |
| Gas anomalies | Remediation |
| Thermal anomalies | Policy decisions |

The platform detects and characterizes. It does not explain anomalies without evidence.

---

### 2. Detection Methods

| Anomaly type | Primary sensor | Detection method | Status |
|---|---|---|---|
| Radiation | SBM-20 | Threshold crossing | `[COTS]` |
| Magnetic | Fluxgate | Deviation from baseline | `[COTS]` |
| Gas | BME680, NDIR | Concentration spike | `[COTS]` |
| Thermal | FLIR Boson | Temperature anomaly | `[COTS]` |
| Optical | Hyperspectral | Spectral deviation | `[COTS]` |
| Acoustic | ReSpeaker array | Frequency anomaly | `[COTS]` |
| Spatial | LiDAR | Geometry anomaly | `[COTS]` |

---

### 3. Baseline Comparison

Every anomaly is compared against the pre-loaded academic baseline in RAG tier 2.

| Step | Action | Tool |
|---|---|---|
| 1 | Detection | Sensor threshold or model |
| 2 | Localization | RTK-GPS + IMU |
| 3 | Baseline query | RAG tier 2 |
| 4 | Deviation score | Local backbone |
| 5 | Operator alert | Console |
| 6 | Operator validation | Voice or console |
| 7 | Record creation | Empirical vault |
| 8 | Orbital query | Starlink + Grok |

---

### 4. Anomaly Record Format

| Field | Type | Description |
|---|---|---|
| ID | UUID | Unique record identifier |
| Timestamp | ISO 8601 | PTP-synchronized |
| Position | RTK-GPS | Lat, lon, alt |
| Sensor source | Enum | Which sensor triggered |
| Raw value | Float | Measured value |
| Baseline value | Float | Expected value |
| Deviation | Float | Absolute or relative |
| Confidence | Float | Model confidence |
| Operator note | Text | Free annotation |
| Orbital validation | Text | Grok output, if available |
| Follow-up required | Boolean | Operator decision |

---

### 5. Priority Classification

| Priority | Criteria | Response |
|---|---|---|
| P0 | Immediate safety risk | Operator alert, stop mission |
| P1 | Strong deviation, high confidence | Immediate operator review |
| P2 | Moderate deviation, medium confidence | Scheduled operator review |
| P3 | Weak deviation, low confidence | Log only, batch review |

Priority is assigned by the local backbone and confirmed by the operator.

---

### 6. Validation Criteria

An anomaly record is valid when:

1. Sensor is calibrated within the last mission.
2. PTP alignment is within 1 µs.
3. Baseline value is documented.
4. Operator has reviewed the record.
5. Record is stored in the empirical vault.

An anomaly without operator review is flagged as unconfirmed and excluded from training.

---

### 7. Open Questions

1. Baseline coverage for rare anomalies is incomplete.
2. False positive rate in field conditions is not measured.
3. Operator alert fatigue over multi-week missions is unknown.
4. Orbital validation latency for P0 anomalies is not defined.
