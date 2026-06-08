# GTP-C Deep Dive (EPC Control Plane)

## 🔌 Overview

GTP-C (GPRS Tunneling Protocol - Control Plane) is used in EPC to manage:
- Session creation
- Bearer management
- TEID allocation
- Path control between SGW and PGW

It is one of the most critical protocols in LTE core networks.

---

## 🧩 Where GTP-C is Used

- MME ↔ SGW (S11 interface)
- SGW ↔ PGW (S5/S8 interface)

---

## 🔄 Key Procedures

### 1. Create Session

Triggered when UE attaches and requests data connectivity.

### 2. Modify Bearer

Used when QoS or routing changes are needed.

### 3. Delete Session

Triggered during detach or session release.

---

## 📦 Create Session Request (CSR)

Sent by MME → SGW → PGW

### Contains:

- IMSI
- MSISDN (optional)
- APN
- Bearer Context
- EPS Bearer ID
- PDN Type
- RAT Type
- Serving Network

---

## 📦 Create Session Response (CSRsp)

Returned by PGW → SGW → MME

### Contains:

- Assigned IP Address
- TEID (Downlink / Uplink)
- Bearer Context Confirmed
- QoS Profile
- Charging ID

---

## 🧠 TEID Concept (Very Important)

TEID = Tunnel Endpoint Identifier

Used to:
- Identify GTP tunnel
- Route user plane traffic correctly

Each session has:
- Uplink TEID
- Downlink TEID

---

## 🔄 Simplified GTP-C Flow

MME → SGW → PGW:

1. Create Session Request
2. Bearer Context Setup
3. IP Allocation
4. Create Session Response

---

## 🌍 Role in LTE Attach

GTP-C is responsible for:

- Creating default bearer
- Allocating IP address
- Establishing user plane path
- Connecting UE to internet

Without GTP-C → No data session

---

## ⚠️ Common Issues

### ❌ 1. Create Session Timeout

Causes:
- SGW unreachable
- PGW failure
- DNS issue
- Diameter/PCRF blocking

---

### ❌ 2. No IP Allocation

Causes:
- PGW pool exhausted
- APN mismatch
- Policy rejection (PCRF)

---

### ❌ 3. Bearer Setup Failure

Causes:
- QoS mismatch
- TEID allocation failure
- Routing issue

---

## 🌐 Interaction with Other Systems

### PCRF (Policy)
- May reject session via Gx

### OCS (Charging)
- May block session via Gy

### DNS
- Used to find PGW

---

## 🧠 PacketAlchemy Insight

GTP-C is the "control brain" of LTE data sessions.

If GTP-C fails:
- UE may attach successfully
- But no data session exists

---

## 🔍 Troubleshooting Checklist

- Check Create Session Request/Response logs
- Verify SGW ↔ PGW connectivity
- Check TEID consistency
- Validate APN mapping
- Check PCRF/OCS responses
- Verify DNS resolution for PGW

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | LTE | EPC | GTP-C
