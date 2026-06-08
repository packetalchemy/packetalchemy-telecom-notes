# Advanced Packet Analysis (GTP-C & Diameter)

## 🧠 Overview

This document focuses on real-world packet-level interpretation of:
- GTP-C (EPC control plane)
- Diameter (Policy & Charging)
- Signaling failures in LTE networks

---

# 🔌 1. GTP-C Packet Analysis

## 📦 Create Session Request (CSR)

Typical fields inside packet:

- IMSI
- APN (internet / ims / etc.)
- RAT Type
- Sender F-TEID (SGW)
- Bearer Context
- PDN Type (IPv4 / IPv6)

---

## 📦 Create Session Response (CSRsp)

Important fields:

- Cause (Success / Failure)
- Assigned UE IP Address
- PGW F-TEID
- EPS Bearer ID
- Charging ID

---

## ⚠️ Failure Analysis

### ❌ Case: Create Session Failure

#### Possible Causes:
- PGW unreachable
- APN not configured
- DNS resolution failure
- PCRF rejection
- IP pool exhausted

#### Packet Indicator:
```
Cause: Request rejected (64)
```

---

# 🔌 2. Diameter (Policy & Charging)

## 📡 Interfaces

- Gx → PCRF ↔ PGW
- Gy → OCS ↔ PGW
- S6a → MME ↔ HSS

---

## 📦 CCR (Credit Control Request)

Sent by PGW to OCS:

Key AVPs:
- Subscription-Id (IMSI)
- Requested-Service-Unit
- Rating-Group
- Service-Identifier

---

## 📦 CCA (Credit Control Answer)

OCS response:

### Success Case:
```
Result-Code: 2001 (Success)
Granted-Service-Unit: OK
```

### Failure Case:
```
Result-Code: 4012 (Credit control not applicable / no credit)
```

---

## ⚠️ Real Charging Failures

### ❌ Case 1: No Credit

- Subscriber blocked
- PDP context rejected or restricted

---

### ❌ Case 2: Diameter Timeout

- No response from OCS
- Session stuck in pending state

---

# 🌐 3. GTP-U Packet Analysis

## 📦 Tunnel Structure

- Outer IP header
- UDP (Port 2152)
- GTP-U header
- Inner UE packet

---

## ⚠️ Common Issues

### ❌ No Traffic Flow

Possible causes:
- Missing TEID mapping
- Firewall blocking UDP 2152
- SGW/PGW routing mismatch

---

### ❌ One-way Traffic

- Uplink works, downlink fails
- TEID mismatch
- Asymmetric routing

---

# 🧩 4. Cross-Domain Failure Mapping

## 🔥 Attach OK but No Internet

| Layer | Possible Issue |
|------|----------------|
| DNS | Wrong PGW selection |
| PCRF | Policy deny |
| OCS | Credit block |
| GTP-U | Tunnel missing |

---

# 🧠 PacketAlchemy Insight

Real network issues are NOT single-layer problems.

They are **multi-domain correlation failures**:

- Control Plane (GTP-C)
- Policy (PCRF)
- Charging (OCS)
- Routing (DNS)
- User Plane (GTP-U)

---

# 🔍 Debug Strategy (Real NOC Style)

### Step 1:
Check GTP-C logs (session created?)

### Step 2:
Check Diameter logs (PCRF / OCS response)

### Step 3:
Check DNS resolution (PGW correct?)

### Step 4:
Check GTP-U tunnel (traffic flowing?)

---

# 📌 Final Rule

> "If you only check one layer, you will miss the real root cause."

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | Packet Analysis | EPC | LTE | Diameter | GTP
