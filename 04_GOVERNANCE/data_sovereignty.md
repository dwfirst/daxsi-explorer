# Data Sovereignty

This document defines the data ownership, access, and retention model of the DAXSI platform. It is a governance document, not a technical one. It defines who owns what, and under which conditions data leaves the platform.

---

### 1. Ownership

| Data class | Owner | Location | Transfer |
|---|---|---|---|
| Raw sensor logs | Platform operator | Empirical vault | Never leaves without explicit export |
| Validated annotations | Platform operator | Empirical vault | Never leaves without explicit export |
| Trained adapters | Platform operator | Empirical vault | Optional upload to orbital layer |
| Knowledge graphs | Platform operator | Empirical vault | Compressed upload to orbital layer |
| Base model weights | Model provider | Read-only | Not owned by operator |
| Academic baseline | Public domain | Node 03 | Pre-loaded, not transmitted |

The operator owns everything the platform generates. The platform owns nothing.

---

### 2. Data Classes

| Class | Definition | Retention | Encryption |
|---|---|---|---|
| Raw | Unprocessed sensor output | Indefinite | At rest, AES-256 |
| Processed | Calibrated, time-aligned data | Indefinite | At rest, AES-256 |
| Annotated | Operator-validated records | Indefinite | At rest, AES-256 |
| Derived | Model outputs, hypotheses | Indefinite | At rest, AES-256 |
| Transmitted | Knowledge graphs sent to orbital layer | N/A | In transit, TLS 1.3 |

No data class is deleted automatically. Deletion is an explicit operator action.

---

### 3. Transfer Rules

| Destination | Allowed data | Conditions |
|---|---|---|
| Orbital layer (xAI) | Compressed knowledge graphs | Operator approval per session |
| Orbital layer (xAI) | LoRA adapters | Operator approval per adapter |
| Orbital layer (xAI) | Raw sensor data | Never |
| Portable kit | Annotations, hypotheses | Always |
| Portable kit | Raw sensor data | Never |
| External storage | Any | Physical export only |

Raw sensor data never leaves the platform over a network. Physical export is the only path.

---

### 4. Operator Consent

Every transfer to the orbital layer requires explicit operator consent.

| Transfer type | Consent model | Default |
|---|---|---|
| Knowledge graph | Per-session approval | Off |
| LoRA adapter | Per-adapter approval | Off |
| Model update | Per-update approval | Off |
| Telemetry | None | Off |

Consent is logged with timestamp, operator ID, and data hash. A transfer without a consent record is a policy violation.

---

### 5. Retention and Deletion

| Action | Trigger | Authority |
|---|---|---|
| Retention | Default | Automatic |
| Archive | Operator decision | Operator |
| Delete | Operator decision | Operator |
| Secure erase | Decommission | Operator + second party |

Deletion is a two-step process: mark for deletion, confirm deletion. No automatic purge.

---

### 6. Audit

Every data access is logged.

| Event | Logged fields |
|---|---|
| Read | Timestamp, user, data ID |
| Write | Timestamp, user, data ID |
| Transfer | Timestamp, user, data ID, destination |
| Delete | Timestamp, user, data ID, confirmation |

Logs are stored in the empirical vault and are immutable.

---

### 7. Open Questions

1. Encryption key management for multi-operator scenarios is not defined.
2. Legal jurisdiction for data collected across borders is not addressed.
3. Secure erase procedure for NVMe SSDs requires validation.
4. Audit log integrity under adversarial conditions is not tested.
