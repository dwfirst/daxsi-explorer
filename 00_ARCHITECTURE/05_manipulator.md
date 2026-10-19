# Block 5: Manipulator — Retractable Arm, Stowage, and Control

This document defines the robotic manipulator of the DAXSI platform. The arm is stowed in a sealed bay under the Vault and deployed only in the Deployed operational state. It is a tool for sample handling, sensor placement, and physical interaction with terrain the vehicle cannot reach.

---

### 1. Design Principle

The manipulator is a retractable subsystem, not a permanent protrusion. It is designed to be invisible to the vehicle's aerodynamics and ground clearance when stowed.

| Principle | Implementation |
|---|---|
| Stowed invisibility | Sealed bay under Vault, no external protrusion |
| Deployment on demand | Linear actuator, ROS 2 action |
| Vibration tolerance | Two-point mechanical lock, damping inserts |
| Galvanic isolation | Optical control link |
| Fail-safe | Mechanical lock releases only under power |

---

### 2. Arm Specification

| Parameter | Value | Status |
|---|---|---|
| Degrees of freedom | 6 | `[COTS]` |
| Reach | 0.8–1.0 m | `[COTS]` |
| Payload | 3 kg | `[COTS]` |
| Repeatability | ± 0.1 mm | `[COTS]` |
| Mass | 4.5 kg | `[COTS]` |
| Control interface | ROS 2 action | `[COTS]` |
| End effector | Interchangeable gripper / sensor mount | `[COTS]` |

The arm class is represented by the igus ReBeL 6DOF or an equivalent COTS manipulator. The specific model is a configuration parameter, not an architectural commitment.

---

### 3. Stowage Bay

| Parameter | Value | Status |
|---|---|---|
| Location | Under-bed, forward of rear axle | `[COTS]` |
| Sealing | IP67 | `[COTS]` |
| Deployment | Linear actuator, rear-facing hatch | `[COTS]` |
| Lock | Two-point mechanical, power-released | `[COTS]` |
| Damping | Elastomeric inserts, tuned to 5–50 Hz | `[COTS]` |
| Access | Service hatch from Vault floor | `[COTS]` |

The bay is isolated from the habitation volume. The arm is deployed only when the vehicle is stationary and the operator has confirmed the Deployed state.

---

### 4. Deployment Sequence

| Step | Action | Control |
|---|---|---|
| 1 | Vehicle stationary, parking brake engaged | Operator |
| 2 | Deployed state confirmed on console | Operator |
| 3 | Hatch opens, linear actuator extends | ROS 2 action |
| 4 | Arm unfolds, self-check | ROS 2 action |
| 5 | End effector selected | Operator or autonomous |
| 6 | Task execution | ROS 2 action |
| 7 | Arm folds, actuator retracts | ROS 2 action |
| 8 | Hatch closes, lock engages | ROS 2 action |

The sequence is reversible at any step. Abort returns the arm to stowed position.

---

### 5. Control Architecture

| Layer | Component | Status |
|---|---|---|
| High-level | ROS 2 action server on Node 01 | `[COTS]` |
| Motion planning | MoveIt 2 | `[COTS]` |
| Low-level | Joint trajectory controller | `[COTS]` |
| Feedback | Joint encoders, force/torque sensor | `[COTS]` |
| Safety | Workspace limits, collision detection | `[COTS]` |
| Link | Optical, ODU Expanded Beam | `[COTS]` |

The arm is controlled from Node 01, not Node 02. This keeps control latency low and isolates the arm from heavy inference load.

---

### 6. Use Cases

| Use case | End effector | Status |
|---|---|---|
| Sample collection | Gripper | `[COTS]` |
| Sensor placement | Sensor mount | `[COTS]` |
| Surface inspection | Camera mount | `[COTS]` |
| Terrain probing | Probe tip | `[COTS]` |
| Object retrieval | Gripper | `[COTS]` |

The arm is not designed for heavy lifting or excavation. It is designed for precise interaction with small objects and sensor placement in locations the vehicle cannot reach.

---

### 7. Vibration Load During Stowed Travel

The stowed arm is subjected to off-road vibration. This is the primary open question.

| Concern | Mitigation | Status |
|---|---|---|
| Joint bearing wear | Two-point mechanical lock | `[COTS]` |
| Connector fatigue | Damping inserts, strain relief | `[COTS]` |
| Hatch seal degradation | IP67 seal, periodic inspection | `[COTS]` |
| Long-term alignment drift | Calibration procedure required | `[PROTO]` |

Field validation of stowed vibration tolerance is required before Phase 2 deployment.

---

### 8. Open Questions

1. Stowed vibration tolerance under sustained off-road motion is not validated.
2. End effector swap mechanism requires field-replaceable design not yet specified.
3. Arm reach vs. vehicle geometry requires CAD integration study.
4. Power draw during deployment and task execution requires measurement.

See `01_FEASIBILITY/open_questions.md`.
