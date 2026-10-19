# Block 2: Sensor Matrix — Full-Spectrum Perception by Physical Field

This document defines the sensor architecture of the DAXSI platform. Sensors are organized by physical field, not by device list. Each field maps to the best available COTS instrument for a moving platform, with explicit selection rationale.

---

### 1. Design Principle

The Sensor Matrix is a perception system, not a collection of instruments. Each sensor is selected against four criteria.

| Criterion | Requirement |
|---|---|
| Spectral coverage | Fills a gap not covered by another sensor |
| Motion tolerance | Operates under vehicle vibration and motion |
| Environmental rating | IP67 or better, −40 °C to +85 °C |
| Interface | GigE, GMSL2, USB 3.0, or UART with ROS 2 driver |

Pushbroom hyperspectral instruments are excluded from the mobile platform. They require stable motion and produce distortion under vibration. Snapshot instruments are preferred wherever the spectral range permits.

---

### 2. Physical Field Map

| Field | Sub-range | Sensor | Weight | Interface | Status |
|---|---|---|---|---|---|
| EM — optical | UV–VIS 350–1000 nm | Cubert ULTRIS X20 | 350 g | GigE | `[COTS]` |
| EM — optical | VNIR 400–1000 nm | Specim AFX10 | 2.4 kg | GigE | `[COTS]` |
| EM — optical | SWIR 900–1700 nm | ATH1700-17 | 1.1 kg | GigE | `[COTS]` |
| EM — optical | SWIR ext. 960–2500 nm | Specim SX25 | 4.0 kg | GigE | `[COTS]` |
| EM — thermal | LWIR 7.7–12.3 µm | FLIR Boson 640 | 7.5 g | GMSL2 / USB | `[COTS]` |
| EM — ranging | 905 / 1550 nm | Ouster OS1-128 | 950 g | Ethernet UDP | `[COTS]` |
| EM — ionizing | gamma / beta | SBM-20 Geiger | 40 g | UART | `[COTS]` |
| EM — magnetic | 3-axis | Fluxgate magnetometer | 80 g | I2C / UART | `[COTS]` |
| Chemical | VOC, CO2, CH4, NOx | Bosch BME680 + NDIR + electrochemical cells | 30 g | I2C / UART | `[COTS]` |
| Acoustic | audible + infrasound | ReSpeaker Mic Array v2.0 | 120 g | USB Audio | `[COTS]` |
| Kinetic | 9-DOF | Bosch BNO055 | 5 g | I2C | `[COTS]` |

---

### 3. Selection Rationale

#### Optical — Hyperspectral

| Instrument | Type | Rationale |
|---|---|---|
| Cubert ULTRIS X20 | Snapshot | 164 channels, 350–1000 nm, no moving parts, tolerant to vibration |
| Specim AFX10 | Pushbroom | Fixed-wing baseline reference, not for mobile use |
| ATH1700-17 | Snapshot | 25 fps, 100 channels, 900–1700 nm, direct mineral and moisture discrimination |
| Specim SX25 | Pushbroom | 960–2500 nm, 392 channels, full chemical signature, static deployment only |

The mobile platform carries ULTRIS X20 and ATH1700-17 as primary hyperspectral instruments. AFX10 and SX25 are ground-truth references, deployed from the portable kit or static station.

#### Optical — Thermal

FLIR Boson 640 is selected over larger LWIR instruments because of its 7.5 g mass, 640×512 resolution, and GMSL2 interface. A larger LWIR hyperspectral instrument (Specim AisaOWL, 13.1 kg) is excluded from the mobile platform and reserved for static deployment.

#### Ranging

Ouster OS1-128 provides 200 m range, 5.2M points/s, IP68/69K rating, and −40 °C operation. It is the only LiDAR in the selection that meets all four criteria without modification.

#### Chemical

The chemical suite is layered: BME680 for general air quality, NDIR for CO2 and CH4, electrochemical cells for specific gases. No single sensor covers the full chemical field.

#### Acoustic

ReSpeaker Mic Array v2.0 provides 4-microphone spatial audio with USB Audio Class interface. Infrasound coverage is limited to 20 Hz; sub-20 Hz detection requires a dedicated geophone, which is not in the baseline.

---

### 4. Spatial Placement

| Location | Sensors | Rationale |
|---|---|---|
| Roof ridge platform | LiDAR, LWIR, panoramic VIS, weather station | Above aerodynamic and thermal boundary layer |
| A-pillars and side sails | UV/VIS matrices, side LiDAR | Zero blind spots for volumetric mapping |
| Vault tailgate edge | Ground-facing stereo, Geiger tube | Terrain tracking, radiation isolated from cabin electronics |
| Underbody (protected) | IMU, magnetometer | Chassis reference, magnetic field measurement |
| Portable kit | Handheld spectroradiometer, gas analyzer | Locations inaccessible to the vehicle |

All sensors are mounted on vibration-isolated platforms. See `07_isolation_enclosure.md`.

---

### 5. Data Ingestion Pipeline

| Stage | Technology | Function |
|---|---|---|
| Capture | GigE Vision, GMSL2, USB 3.0 | Raw sensor streams |
| Transport | iceoryx shared memory | Zero-copy to Node 02s |
| Peripheral telemetry | ESP-NOW mesh | Low-frequency sensor data, bypasses main network |
| Time sync | IEEE 1588 PTP | Microsecond alignment across all nodes |
| Storage | MCAP, Parquet | Time-series and geospatial indexing |
| Ground truth | RTK-GPS + IMU | Every record tagged with position and orientation |

Time synchronization is the foundation of the pipeline. A gas surge, an optical frame, and a LiDAR return must be aligned to the same timestamp before they can be correlated.

---

### 6. Calibration and Ground Truth

Every sensor requires calibration against a known reference before field deployment.

| Sensor | Calibration method | Frequency |
|---|---|---|
| Hyperspectral | Spectralon reference panel | Before each mission |
| Thermal | Blackbody reference | Monthly |
| LiDAR | Surveyed targets | Before each mission |
| Gas | Certified gas mixtures | Monthly |
| Radiation | Known source | Annually |
| IMU | Static and dynamic bench | Before each mission |

Calibration records are stored in the empirical vault with timestamps. An uncalibrated sensor is excluded from the dataset.

---

### 7. Open Questions

1. SWIR snapshot sensor (ATH1700-17) field reliability under vibration is not validated.
2. Infrasound coverage below 20 Hz requires a geophone not in the baseline.
3. Hyperspectral data volume (up to 800 MB/s) requires real-time compression strategy not yet defined.
4. ESP-NOW mesh range under vehicle body obstruction requires field measurement.

See `01_FEASIBILITY/open_questions.md`.


