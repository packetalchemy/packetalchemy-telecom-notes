# Wireshark Lab: Core Network Traffic Analysis (EPC / 5GC)

## 🧠 Objective

Understand how LTE and 5G Core network traffic appears in Wireshark and how to troubleshoot real operator issues using packet analysis.

---

# 📡 1. Key Protocols in Core Networks

## EPC (LTE)
- GTP-C (Control Plane)
- GTP-U (User Plane)
- Diameter (OCS / PCRF / HSS)
- DNS (APN resolution)

## 5G Core
- HTTP/2 (SBA signaling)
- NGAP (gNB ↔ AMF)
- GTP-U (User Plane via UPF)
- PFCP (SMF ↔ UPF)

---

# 🔍 2. Important Wireshark Filters

## GTP-C (Control Plane)
```
gtp
```

## GTP-U (User Plane)
```
udp.port == 2152
```

## Diameter (Charging / Policy)
```
diameter
```

## DNS
```
dns
```

## 5G HTTP/2 (SBA)
```
http2
```

---

# 📡 3. LTE Attach Flow (Packet View)

## Step-by-step in Wireshark:

### 1. Attach Request
UE → MME

### 2. Authentication Request
MME ↔ HSS (Diameter S6a)

### 3. Create Session Request
MME → SGW → PGW (GTP-C)

### 4. IP Allocation
PGW assigns IP

### 5. Bearer Setup Complete
Data path established

---

# 🌐 4. GTP-U Traffic (Data Plane)

## What you should see:

- UDP packets on port 2152
- Encapsulated IP packets inside GTP tunnel
- Source: SGW / UPF
- Destination: PGW / Internet

---

## ⚠️ Problem Indicators:

- No GTP-U packets → no data flow
- One-way traffic → tunnel issue
- High packet loss → transport problem

---

# 💳 5. Diameter (OCS / Policy)

## Important Interfaces:
- Gy → OCS (charging)
- Gx → PCRF (policy)
- S6a → HSS (auth)

---

## Common Messages:
- CCR (Credit-Control-Request)
- CCA (Credit-Control-Answer)

---

## ⚠️ Failure Cases:

| Symptom | Meaning |
|--------|--------|
| No CCA | OCS failure |
| 4012 | No credit / blocked |
| Timeout | Diameter routing issue |

---

# 🌍 6. DNS in Core Networks

## What to check:

- APN → PGW resolution (LTE)
- DNN → UPF selection (5G)

## Wireshark Indicators:

- NXDOMAIN → wrong config
- Slow response → DNS congestion
- Wrong IP → routing issue

---

# ☁️ 7. 5G Core Traffic (SBA)

## HTTP/2 Messages:
- AMF ↔ SMF communication
- SMF ↔ UPF session control
- PCF policy exchange

---

## PFCP (SMF ↔ UPF):
- Session Establishment Request
- Session Modification Request

---

# ⚠️ 8. Real Troubleshooting Scenarios

## Scenario 1: No Internet but IP Assigned

Check:
- DNS response
- GTP-U traffic
- OCS response (4012?)
- PCRF policy installation

---

## Scenario 2: Attach Success but No Data

Check:
- GTP-C session creation
- Bearer activation
- PGW/UPF selection

---

## Scenario 3: Intermittent Connectivity

Check:
- Packet loss in GTP-U
- DNS instability
- OCS timeout spikes

---

# 🧠 9. Engineering Insight

> “Wireshark is not just a tool. It is a translation layer between network behavior and engineering reasoning.”

---

# 🔥 10. How Senior Engineers Use Wireshark

They don’t:
- فقط packet نگاه کنن ❌

They:
- correlation بین DNS + GTP + OCS + Policy پیدا می‌کنن ✔
- flow کامل session رو rebuild می‌کنن ✔
- root cause پیدا می‌کنن ✔

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | Wireshark | EPC | 5G Core | Packet Analysis | Troubleshooting
