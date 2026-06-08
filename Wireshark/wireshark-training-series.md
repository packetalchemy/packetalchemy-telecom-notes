# Wireshark Training Series – Core Network (EPC / 5GC)

## 🧠 Overview

This repository section is a structured Wireshark training series designed for telecom engineers working with:

- LTE / EPC Core Networks
- 5G Core (5GC)
- DNS / Charging / Policy systems
- GTP / Diameter / HTTP2 protocols

---

# 🎯 Training Structure

This series is divided into progressive levels:

---

## 🟢 Level 1 – Basic Packet Understanding

- Ethernet / IP / TCP / UDP basics
- How packets are structured
- How to read Wireshark interface

---

## 🟡 Level 2 – LTE EPC Analysis

Focus on:

- GTP-C (Control Plane)
- GTP-U (User Plane)
- Diameter (OCS / PCRF / HSS)
- DNS (APN resolution)

### Key Scenarios:
- Attach procedure
- IP assignment
- No internet issue
- Bearer setup failure

---

## 🔵 Level 3 – 5G Core Analysis

Focus on:

- HTTP/2 signaling (SBA)
- AMF / SMF / UPF interaction
- PFCP protocol
- NRF service discovery

### Key Scenarios:
- Registration failure
- Session setup failure
- UPF selection issue

---

## 🔴 Level 4 – Advanced Troubleshooting

Focus on:

- Cross-domain correlation
- DNS + OCS + GTP interaction
- Policy enforcement (PCRF / PCF)
- Roaming issues

### Key Scenarios:
- Intermittent connectivity
- Attach success but no data
- Charging-related service block

---

# 📡 Core Protocol Filters

## EPC:
```
gtp
diameter
dns
udp.port == 2152
```

## 5GC:
```
http2
pfcp
ngap
```

---

# 🧠 Analysis Methodology

Every Wireshark analysis should follow:

### Step 1: Identify Attach / Registration
### Step 2: Check Session Creation (GTP-C / PFCP)
### Step 3: Validate DNS Resolution
### Step 4: Check Charging System (OCS / CHF)
### Step 5: Verify User Plane (GTP-U / N3)

---

# 🔥 Senior Engineer Thinking

A junior engineer sees:
- Packets failing

A senior engineer sees:
- Dependency failure chain across:
  - DNS
  - Policy
  - Charging
  - Session control
  - User plane

---

# 🧠 Key Insight

> “Wireshark is not a packet viewer — it is a system behavior decoder.”

---

# 📁 Next Modules (to be added)

- LTE Attach Deep Dive (Packet Flow)
- GTP-U Tunnel Analysis
- Diameter Failure Scenarios
- 5G PFCP Session Debugging
- Real PCAP Case Studies
- War Room Packet Correlation

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | Wireshark Training Series | EPC | 5G Core | Packet Analysis | Vendor-Level Troubleshooting
