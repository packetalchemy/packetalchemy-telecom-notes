# PCAP Simulation Lab – Real Operator Scenario

## 🧠 Objective

This lab simulates a real PCAP capture from a mobile operator network.

You will act as a Core Network Engineer analyzing a live failure scenario.

No real file is needed — we simulate Wireshark behavior.

---

# 📡 1. Scenario Description

## User Complaint:
- “I have signal, but no internet”

## Network Status:
- LTE Attach SUCCESS
- IP assigned
- Services not working

---

# 🧪 2. Simulated PCAP Flow

## 📌 Frame 1–4: Attach Flow

```
Frame 1: UE → MME : Attach Request
Frame 2: MME → HSS : Authentication Request
Frame 3: HSS → MME : Authentication Response
Frame 4: MME → SGW : Create Session Request
Frame 5: SGW → PGW : Create Session Request
Frame 6: PGW → MME : Create Session Response
```

✔ Attach successful

---

## 📌 Frame 7–10: DNS Phase

```
Frame 7: PGW → DNS : APN query (internet.operator.com)
Frame 8: DNS → PGW : delayed response
Frame 9: PGW retry DNS query
Frame 10: DNS → PGW : NXDOMAIN
```

❌ DNS failure detected

---

## 📌 Frame 11–14: Charging Check (OCS)

```
Frame 11: PGW → OCS : CCR-Initial
Frame 12: OCS → PGW : Timeout
Frame 13: PGW retry CCR
Frame 14: No CCA received
```

❌ Charging system degraded

---

## 📌 Frame 15–18: GTP-U Data Plane

```
Frame 15: UE → SGW : data packet
Frame 16: SGW → PGW : no tunnel response
Frame 17: UDP 2152 missing
Frame 18: packet drop
```

❌ No user plane established

---

# 🔍 3. Wireshark Interpretation

## Observed Issues:

- GTP-C session OK
- DNS resolution FAILED
- OCS timeout present
- GTP-U tunnel NOT active

---

# 🎯 4. Root Cause Analysis

## PRIMARY ROOT CAUSE:
👉 DNS failure (NXDOMAIN)

## SECONDARY EFFECT:
👉 OCS retry storm due to session instability

---

# 🧠 5. Senior Engineer Interpretation

Instead of saying:

❌ “No internet because DNS failed”

Senior engineer says:

✔ “DNS failure caused incorrect APN resolution, which prevented proper PGW selection, leading to incomplete GTP-U tunnel establishment and OCS retry loops.”

---

# 🔥 6. What This Lab Teaches

- Packet flow reconstruction
- Multi-layer correlation (DNS + GTP + Diameter)
- Root cause identification
- TAC-level reasoning

---

# 📊 7. Debug Checklist

When analyzing any PCAP:

### Step 1:
Check Attach (MME/HSS)

### Step 2:
Check Session (GTP-C)

### Step 3:
Check DNS resolution

### Step 4:
Check OCS / PCRF

### Step 5:
Check GTP-U traffic

---

# 🧠 Engineering Insight

> “A PCAP is not a file. It is a story of how a network failed.”

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | PCAP Simulation | EPC | 5G Core | Wireshark Training | Vendor-Level Troubleshooting
