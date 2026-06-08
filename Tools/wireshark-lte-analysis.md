# Wireshark LTE / EPC Packet Analysis

## 🧠 Overview

Wireshark is one of the most powerful tools for analyzing LTE/EPC traffic.

In mobile core networks, it is used to inspect:
- GTP-C signaling
- GTP-U user traffic
- Diameter (OCS / PCRF / HSS)
- DNS queries

---

# 📦 1. GTP-C in Wireshark

## 🔍 Filter:
```
gtp
```

---

## 📡 Create Session Request

### Key fields:
- IMSI
- APN
- Sender F-TEID
- Bearer Context
- RAT Type

---

## 📡 Create Session Response

### Important fields:
- Cause
- UE IP Address
- PGW F-TEID
- EPS Bearer ID

---

## ❌ Failure Example

```
Cause: No resources available (73)
```

### Interpretation:
- PGW pool issue
- APN misconfiguration
- DNS routing failure
- Policy rejection upstream

---

# 💳 2. Diameter Analysis (OCS / PCRF)

## 🔍 Filter:
```
diameter
```

---

## 📡 CCR (Credit-Control-Request)

### Important AVPs:
- IMSI
- Rating-Group
- Requested-Service-Unit

---

## 📡 CCA (Credit-Control-Answer)

### Success:
```
Result-Code: 2001
```

### Failure:
```
Result-Code: 4012
```

---

## 🧠 Interpretation:

- 2001 → Service allowed
- 4012 → No credit / restricted service

---

# 🌐 3. DNS in Wireshark

## 🔍 Filter:
```
dns
```

---

## 📡 Example Query

```
APN: internet.operator.com
Query Type: A / AAAA
```

### Issue Example:
- Wrong PGW resolved
- Missing record
- Timeout

---

# 📡 4. GTP-U Traffic

## 🔍 Filter:
```
udp.port == 2152
```

---

## 📦 Packet Structure

- Outer IP header
- UDP 2152
- GTP-U header
- Inner UE IP packet

---

## ❌ Problem Cases

### No traffic flow:
- Tunnel missing
- Firewall blocking UDP 2152

### One-way traffic:
- TEID mismatch
- routing asymmetry

---

# 🧩 5. End-to-End Correlation

## 📌 Scenario: No Internet

### GTP-C:
✔ Session created

### Diameter:
❌ 4012 (OCS block)

### DNS:
⚠ Wrong PGW selected

### GTP-U:
❌ No traffic flow

---

# 🧠 PacketAlchemy Insight

Wireshark does NOT tell you the root cause directly.

It only shows symptoms.

👉 Engineer job = correlation across layers

---

# 🔧 Debug Workflow

1. Check GTP-C → session exists?
2. Check Diameter → policy & charging
3. Check DNS → correct routing?
4. Check GTP-U → traffic flowing?

---

## 📌 Final Rule

> “Wireshark shows packets, not problems. Engineers find problems by correlation.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | Wireshark | LTE | EPC | Packet Analysis | NOC Tools
