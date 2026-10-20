# Safety and Ethics

This document defines the safety case and ethical framework of the DAXSI platform. It covers operational hazards, human override, autonomous behavior limits, and environmental responsibility.

---

### 1. Operational Hazards

| Hazard | Source | Severity | Mitigation |
|---|---|---|---|
| Vehicle motion during manipulator deployment | Operator error | High | Interlock: arm only deploys when parked |
| High voltage exposure | Powershare 240V | High | Sealed PDU, lockout procedure |
| Coolant leak | Loop A/B | Medium | Leak sensor, automatic derating |
| Battery thermal runaway | Traction battery | High | Vehicle-native BMS, no modification |
| Radiation exposure | Natural sources | Low | Shielding, dosimetry |
| Dust inhalation | Field environment | Low | HEPA ECLSS, positive pressure |
| Optical hazard | LiDAR 905/1550 nm | Medium | Eye-safe class, no direct viewing |
| Acoustic hazard | High-SPL sources | Low | Hearing protection, distance |

---

### 2. Human Override

The operator has absolute override authority in all states.

| Override | Method | Latency target |
|---|---|---|
| Stop mission | Physical button + console | < 1 s |
| Stop manipulator | Physical E-stop | < 0.5 s |
| Stop data collection | Console | < 2 s |
| Disable orbital sync | Console | < 5 s |
| Enter Idle state | Console | < 10 s |

No autonomous decision can override a human stop command. The override channel is physically separate from the main compute path.

---


### 2a. Independent Emergency Stop

The E-stop is a physical circuit, independent of the main compute path and independent of the vehicle's software stack.

| Property | Specification | Status |
|---|---|---|
| Type | Physical button + relay | `[COTS]` |
| Power source | Dedicated battery, 12V | `[COTS]` |
| Independence | Separate from Node 01/02/03 | `[COTS]` |
| Function | Cuts manipulator power, signals Idle state | `[COTS]` |
| Test interval | Before each deployment | `[COTS]` |
| Failure mode | Fail-safe (opens on loss of power) | `[COTS]` |

The E-stop does not depend on ROS 2, on the operator console, or on any network link. It is a hardware interlock. Its only function is to remove power from the manipulator and signal the platform to enter Idle state.

An E-stop that has not been tested within the last deployment cycle is treated as non-functional.
### 3. Autonomous Behavior Limits

| Behavior | Allowed | Conditions |
|---|---|---|
| Anomaly detection | Yes | Always |
| Anomaly flagging | Yes | Always |
| Hypothesis generation | Yes | Always |
| Manipulator motion | No | Requires operator confirmation |
| Vehicle motion | No | Platform does not drive itself |
| Orbital sync | No | Requires operator consent |
| Adapter deployment | No | Requires operator approval |
| Data deletion | No | Requires operator action |

The platform is autonomous in observation, not in action.

---

### 4. Environmental Responsibility

| Principle | Implementation |
|---|---|
| Leave no trace | No waste discharge, no chemical release |
| Minimize disturbance | Stationary observation preferred over driving |
| Protect wildlife | No active interference, no baiting |
| Respect protected areas | Operator responsibility, no automated override |
| Minimize noise | Electric drive, low-noise fans |

The platform is designed to observe without altering the environment.

---

### 5. Data Ethics

| Principle | Implementation |
|---|---|
| Operator ownership | See `data_sovereignty.md` |
| Consent for transfer | Explicit per-session |
| No covert collection | All sensors visible and documented |
| No personal data | Platform does not target humans |
| No surveillance | No facial recognition, no tracking |

The platform is a scientific instrument, not a surveillance system.

---

### 6. Failure Safety

| Failure | Safe state | Recovery |
|---|---|---|
| Compute loss | Idle | Operator restart |
| Coolant loss | Derated | Operator repair |
| Starlink loss | Local autonomy | Automatic retry |
| Sensor loss | Partial perception | Operator alert |
| Manipulator fault | Stowed | Manual recovery |
| ECLSS fault | Positive pressure maintained | Operator alert |
| Power loss | Battery-only | Operator decision |

No failure results in uncontrolled motion, uncontrolled emission, or data loss.

---

### 7. Regulatory Considerations

| Area | Status |
|---|---|
| Vehicle modification | Requires local regulatory approval |
| Radio transmission (Starlink) | Licensed service |
| LiDAR eye safety | Class 1 eye-safe |
| Radiation source | None (passive detection only) |
| Environmental impact | No discharge, minimal disturbance |

Regulatory compliance is the operator's responsibility. The platform provides the technical basis, not the legal authorization.

---

### 8. Open Questions

1. E-stop latency under full compute load is not measured.
2. Autonomous anomaly flagging rate under field conditions is unknown.
3. Environmental impact assessment for sensitive areas is not defined.
4. Insurance and liability model for autonomous observation is not addressed.

