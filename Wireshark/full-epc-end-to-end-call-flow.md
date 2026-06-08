# Full EPC End-to-End Call Flow (Attach → Internet)

## 🧠 Objective

This document explains the complete LTE EPC call flow from initial attach to full internet connectivity, correlating all major network functions.

---

# 📡 1. High-Level Architecture Flow

```
UE → eNB → MME → SGW → PGW → Internet
              ↕      ↕
            HSS     DNS / OCS / PCRF
```

---

# 📶 2. Step 1 – Attach Procedure (Control Plane Start)

## 📌 Frame Flow:

```
1. UE → MME : Attach Request
2. MME → HSS : Authentication Request
3. HSS → MME : Authentication Response
4. MME → UE : Security Setup
```

✔ Subscriber authenticated successfully

---

# 📡 3. Step 2 – Session Creation (GTP-C)

```
5. MME → SGW : Create Session Request
6. SGW → PGW : Create Session Request
7. PGW → SGW : Create Session Response
8. SGW → MME : Create Session Response
```

### Key Outputs:
- IP address assigned
- TEID allocated
- Default bearer created

✔ Control plane established

---

# 🌐 4. Step 3 – DNS Resolution

```
9. PGW → DNS : APN resolution request
10. DNS → PGW : IP response
```

### Possible outcomes:

✔ Success → correct PGW routing  
❌ Failure → NXDOMAIN / timeout

---

# 💳 5. Step 4 – Charging (OCS / Diameter)

```
11. PGW → OCS : CCR-Initial
12. OCS → PGW : CCA
```

### Result Codes:

| Code | Meaning |
|------|--------|
| 2001 | OK |
| 4012 | Insufficient credit |
| Timeout | OCS failure |

---

# 📡 6. Step 5 – Policy Control (PCRF)

```
13. PGW ↔ PCRF : Gx Interface
```

### Actions:
- QoS assignment
- Rule installation
- Bearer control

✔ Policy applied → service allowed

---

# 🟢 7. Step 6 – User Plane Activation (GTP-U)

```
UE ⇄ SGW ⇄ PGW ⇄ Internet
(UDP 2152 tunnel)
```

### Wireshark View:

- Encapsulated IP packets
- TEID mapping active
- Bidirectional traffic

---

# ⚠️ 8. Failure Correlation Scenarios

## ❌ Scenario A: No Internet but IP Assigned

Check:
- DNS failure
- OCS blocked (4012)
- GTP-U not established

---

## ❌ Scenario B: Attach Success but No Data

Check:
- GTP-C session OK
- PGW unreachable
- PCRF policy missing

---

## ❌ Scenario C: Intermittent Connectivity

Check:
- GTP-U packet loss
- DNS latency
- OCS timeout spikes

---

# 🔍 9. Full Correlation Model

## Layer Mapping:

| Layer | Function | Failure Impact |
|------|----------|----------------|
| DNS | Routing decision | Wrong PGW/UPF |
| OCS | Charging control | Service blocked |
| PCRF | Policy control | QoS restriction |
| GTP-C | Session control | No bearer |
| GTP-U | Data plane | No traffic |

---

# 🧠 10. Senior Engineer View

A junior engineer says:
- “No internet issue”

A senior engineer says:
- “Control plane is established, but service delivery is broken due to DNS misrouting and OCS/PCRF interaction failure affecting GTP-U activation.”

---

# 🔥 11. Key Engineering Insight

> “EPC is not a linear flow. It is a dependency graph of decisions across multiple control systems.”

---

# 📊 12. Final Debug Checklist

### Step 1:
Verify Attach (MME/HSS)

### Step 2:
Verify Session (GTP-C)

### Step 3:
Check DNS resolution

### Step 4:
Validate OCS response

### Step 5:
Confirm PCRF policy

### Step 6:
Verify GTP-U traffic

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | EPC Full Flow | Wireshark | GTP | DNS | OCS | PCRF | Troubleshooting
