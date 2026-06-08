# Real EPC Incident Case Studies

## 🧠 Overview

This document contains real-world inspired LTE/EPC failure scenarios from operator networks.

Each case shows:
- Symptoms
- Packet-level analysis
- Root cause
- Final resolution

---

# 🚨 Case 1: Attach Success but No Internet (Roaming)

## 📌 Symptoms

- UE attaches successfully
- IP is assigned
- No data traffic
- Roaming subscriber

---

## 🔍 Packet Analysis

### GTP-C:
```
Create Session → SUCCESS
IP Assigned → YES
```

### Diameter (OCS):
```
Result-Code: 4012
```

### PCRF:
```
No dynamic policy installed
Default deny applied
```

### DNS:
```
Wrong PGW selected (visited DNS override)
```

---

## 🎯 Root Cause

Primary:
- OCS blocked subscriber (4012)

Secondary:
- PCRF did not provide valid policy
- DNS misrouting in roaming domain

---

## 🛠️ Resolution

- Fix OCS charging profile
- Update PCRF roaming policy
- Correct DNS APN mapping

---

# 🚨 Case 2: Intermittent Data Drop

## 📌 Symptoms

- Internet works for few seconds
- Then stops randomly
- Reconnect restores service temporarily

---

## 🔍 Packet Analysis

### GTP-U:
```
Tunnel exists but packet loss observed
```

### SGW/PGW:
- Load imbalance detected

### QoS:
- Low priority QCI assigned

---

## 🎯 Root Cause

- Congestion in SGW path
- Improper QoS mapping (PCRF issue)
- Backhaul instability

---

## 🛠️ Resolution

- Optimize SGW selection
- Fix QoS profile in PCRF
- Upgrade transport link

---

# 🚨 Case 3: PGW Misrouting via DNS

## 📌 Symptoms

- Attach OK
- High latency
- Wrong country routing

---

## 🔍 Packet Analysis

### DNS:
```
APN resolved to wrong PGW region
```

### GTP-C:
```
Session created successfully
```

---

## 🎯 Root Cause

- DNS misconfiguration in APN mapping
- No geo-aware routing rule

---

## 🛠️ Resolution

- Fix DNS FQDN records
- Implement correct APN → PGW mapping

---

# 🚨 Case 4: Complete Data Failure (GTP-U Block)

## 📌 Symptoms

- Attach OK
- IP assigned
- No traffic at all

---

## 🔍 Packet Analysis

### GTP-U:
- No packets observed (UDP 2152 blocked)

### Firewall:
- Port 2152 filtered

---

## 🎯 Root Cause

- GTP-U blocked by firewall
- Missing tunnel forwarding rule

---

## 🛠️ Resolution

- Open UDP 2152
- Fix SGW ↔ PGW routing policy

---

# 🧠 PacketAlchemy Insight

Real EPC failures are ALWAYS multi-layer problems:

- Charging (OCS)
- Policy (PCRF)
- Routing (DNS)
- Control plane (GTP-C)
- User plane (GTP-U)

---

## 📌 Final Engineering Rule

> “Never trust a single successful layer. Always validate the full chain.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | EPC | LTE | Case Studies | NOC Troubleshooting | Core Network
