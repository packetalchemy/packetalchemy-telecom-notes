# Real Packet Log Analysis (GTP-C + Diameter)

## 🧠 Overview

This document simulates real network packet traces from LTE/EPC core networks, focusing on:
- GTP-C signaling
- Diameter (OCS / PCRF / HSS)
- Failure interpretation like NOC engineers

---

# 🔌 1. GTP-C Log Analysis

## 📦 Sample Log: Create Session Request

```
IMSI: 001010123456789
APN: internet.operator.com
RAT-Type: EUTRAN
Sender F-TEID: 10.10.10.1
```

---

## 📦 Sample Log: Create Session Response

```
Cause: Request accepted (16)
UE IP: 100.64.10.25
PGW F-TEID: 20.20.20.1
```

---

## 🧠 Interpretation

✔ Session created successfully  
✔ IP allocated  
✔ Tunnel established  

👉 GTP-C layer is healthy

---

## ❌ Failure Example

```
Cause: No resources available (73)
```

### Possible Reasons:
- PGW pool exhausted
- APN not configured
- DNS misrouting
- Policy rejection upstream

---

# 💳 2. Diameter (OCS - Gy Interface)

## 📦 CCR (Credit Control Request)

```
IMSI: 001010123456789
Rating-Group: 100
Service-Identifier: internet
Requested-Units: 1024 KB
```

---

## 📦 CCA (Credit Control Answer)

### ❌ Failure Case

```
Result-Code: 4012
```

### Interpretation:
- Subscriber has no valid credit
- Service denied or restricted
- PDP may still attach but data blocked

---

### ✅ Success Case

```
Result-Code: 2001
Granted-Service-Unit: 10240 KB
```

---

# 🧠 3. Diameter (PCRF - Gx Interface)

## 📦 Policy Response Example

```
QoS-Class-Identifier: 9
Bandwidth: 2 Mbps
Rule-Install: ALLOW_INTERNET
```

---

## ❌ Failure Case

```
Result-Code: DIAMETER_UNABLE_TO_COMPLY
```

### Meaning:
- No policy rule assigned
- Default deny applied
- Subscriber blocked

---

# 🌐 4. Combined Real Failure Case

## 📌 Scenario

- Attach SUCCESS
- IP Assigned SUCCESS
- No Internet

---

## 🔍 Logs Summary

### GTP-C
```
Create Session: SUCCESS
```

### OCS
```
Result-Code: 4012
```

### PCRF
```
No rule installed
```

### DNS
```
PGW resolved incorrectly
```

---

## 🎯 Root Cause

Primary:
- OCS blocked subscriber (4012)

Secondary:
- PCRF did not provide policy
- DNS selected wrong PGW

---

# 🧠 PacketAlchemy Insight

Real network troubleshooting is about:

> Correlating multiple protocol layers, not reading a single log.

---

## 🔧 Engineer Thinking Flow

1. Check GTP-C → Is session created?
2. Check Diameter (OCS) → Is credit allowed?
3. Check PCRF → Is policy applied?
4. Check DNS → Is routing correct?
5. Check GTP-U → Is traffic flowing?

---

# 📌 Final Rule

> “Never trust a single successful log — always validate the full chain.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | GTP-C | Diameter | EPC | LTE | NOC Troubleshooting
