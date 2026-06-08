# GTP-U Deep Dive (User Plane)

## 📡 Overview

GTP-U (GPRS Tunneling Protocol - User Plane) is responsible for carrying user data packets between:
- eNodeB ↔ SGW
- SGW ↔ PGW

It transports actual user traffic such as:
- Web browsing
- Video streaming
- App data

---

## 🧩 Where GTP-U Works

### LTE Architecture

- eNodeB → SGW (S1-U interface)
- SGW → PGW (S5/S8 interface)

---

## 📦 What GTP-U Carries

GTP-U encapsulates:
- IP packets from UE
- Inner user IP packet
- Outer tunnel headers

---

## 🔄 GTP-U Packet Structure

Each packet contains:

- Outer IP Header (network transport)
- GTP-U Header
- Inner IP Packet (UE traffic)

---

## 🧠 Tunnel Concept

GTP-U uses tunnels between nodes.

Each tunnel is identified by:

- TEID (Tunnel Endpoint Identifier)
- Source IP (Node)
- Destination IP (Peer Node)

---

## 🔌 Simplified Data Flow

UE → eNodeB → SGW → PGW → Internet

Return path:
Internet → PGW → SGW → eNodeB → UE

---

## 📊 GTP-U vs GTP-C

| Feature | GTP-C | GTP-U |
|--------|-------|-------|
| Type | Control Plane | User Plane |
| Purpose | Session setup | Data transfer |
| Protocol | Signaling | Payload |
| Traffic | Low | High |

---

## 🌍 Role in LTE Sessions

Once GTP-C creates the session:
- GTP-U carries real traffic
- Tunnel is established using TEIDs

Without GTP-U:
- UE may attach successfully
- But no internet traffic flows

---

## ⚠️ Common Issues

### ❌ 1. No Data Flow

Causes:
- Missing GTP-U tunnel
- SGW/PGW routing issue
- Firewall blocking UDP 2152

---

### ❌ 2. One-way Traffic

Causes:
- TEID mismatch
- Incorrect tunnel mapping
- Asymmetric routing issue

---

### ❌ 3. High Latency

Causes:
- Suboptimal SGW/PGW selection
- Roaming path inefficiency
- Backhaul congestion

---

## 🔍 Key Troubleshooting Points

- Verify UDP port 2152 (GTP-U)
- Check tunnel TEID mapping
- Validate SGW ↔ PGW path
- Check packet encapsulation
- Confirm routing symmetry

---

## 🌐 Interaction with Other Systems

- GTP-C → creates tunnel
- PCRF → may restrict traffic
- OCS → may block session
- DNS → affects PGW selection

---

## 🧠 PacketAlchemy Insight

GTP-U is where "network becomes visible".

If GTP-U breaks:
- Everything looks fine in attach
- But user experiences "no internet"

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | LTE | EPC | GTP-U
