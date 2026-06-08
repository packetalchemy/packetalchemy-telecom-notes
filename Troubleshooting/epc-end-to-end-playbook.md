# EPC End-to-End Troubleshooting Playbook

## 🧠 Overview

This playbook provides a structured approach to troubleshooting LTE/EPC issues across:

- Control Plane (GTP-C, Diameter)
- User Plane (GTP-U)
- Policy (PCRF)
- Charging (OCS)
- Routing (DNS)

---

# 🚨 Problem Classification Model

## Step 1: Identify Symptom

- Attach failure
- Attach success but no data
- Data session drops
- Roaming issue
- Slow internet

---

## Step 2: Map to Domain

| Symptom | Likely Domain |
|----------|--------------|
| Attach failure | HSS / MME / S6a |
| No internet | PCRF / OCS / DNS / GTP-U |
| Roaming issue | HSS / DNS / PCRF |
| Slow data | SGW / PGW / GTP-U |

---

# 🔍 Scenario 1: Attach OK but No Internet

## 📌 Symptoms
- UE attached successfully
- IP address assigned
- No browsing possible

## 🔎 Check List

### 1. PCRF (Gx)
- Policy rule applied?
- APN allowed?

### 2. OCS (Gy)
- Credit available?
- Result code (2001 vs 4012)?

### 3. DNS
- PGW resolved correctly?

### 4. GTP-U
- Tunnel established?
- UDP 2152 open?

---

## 🎯 Root Cause Patterns

- PCRF denies service
- OCS blocks session
- Wrong PGW selection (DNS issue)
- GTP-U tunnel missing

---

# 🌍 Scenario 2: Roaming Attach OK but No Data

## 📌 Symptoms
- UE registered in visited network
- No internet access

## 🔎 Check List

### 1. S6a (HSS)
- Update Location success?

### 2. PCRF
- Roaming policy restriction?

### 3. OCS
- Roaming charging rules applied?

### 4. DNS
- Home vs Visited DNS conflict?

---

## 🎯 Root Cause Patterns

- Roaming barred in subscription
- PCRF restrictive policy
- Local breakout misconfiguration
- DNS override failure

---

# ⚡ Scenario 3: Slow Internet

## 📌 Symptoms
- Attach OK
- Data works but slow

## 🔎 Check List

### 1. GTP-U Path
- SGW/PGW optimal?

### 2. QoS (PCRF)
- QCI low priority?

### 3. Backhaul
- Congestion?

### 4. Roaming routing
- Indirect path?

---

## 🎯 Root Cause Patterns

- Suboptimal SGW/PGW selection
- Low QoS profile (QCI)
- Congested transport network
- Long roaming path

---

# 🧩 Unified Troubleshooting Flow

```
UE Issue
  ↓
Check Attach (MME/HSS)
  ↓
Check Policy (PCRF)
  ↓
Check Charging (OCS)
  ↓
Check Routing (DNS)
  ↓
Check User Plane (GTP-U)
```

---

# 🧠 PacketAlchemy Insight

Most EPC issues are NOT isolated.

They are **chain failures across multiple domains**:

- DNS selects wrong PGW
- PCRF denies policy
- OCS blocks session
- GTP-U fails silently

---

# 📌 Final Rule

> "If Attach is OK but data is not working → always check PCRF, OCS, DNS, and GTP-U in that order."

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | EPC | Troubleshooting | LTE | 5G Core
