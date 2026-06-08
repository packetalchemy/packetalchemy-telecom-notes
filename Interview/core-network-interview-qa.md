# Core Network Interview Questions (EPC / 5G)

## 🧠 Objective

This document simulates real interview questions asked by telecom operators and vendors (Ericsson, Huawei, Nokia).

---

# 📡 SECTION 1 – LTE / EPC

---

## ❓ Q1: User has IP but no internet. How do you troubleshoot?

### 🧠 Expected Answer:

I would analyze all EPC layers:

1. Check Attach procedure (MME ↔ HSS)
2. Verify GTP-C session creation (SGW ↔ PGW)
3. Check DNS resolution for APN
4. Validate OCS response (Diameter CCR/CCA)
5. Check GTP-U traffic (UDP 2152)

### 🔥 Root cause possibilities:
- DNS misrouting
- OCS blocking (4012)
- GTP-U tunnel failure
- PCRF policy rejection

---

## ❓ Q2: What is the difference between GTP-C and GTP-U?

### 🧠 Answer:

- GTP-C → Control plane (session creation, bearer setup)
- GTP-U → User plane (actual data traffic)

> GTP-C creates session, GTP-U carries traffic

---

## ❓ Q3: What happens during LTE Attach?

### 🧠 Flow:

UE → MME → HSS → SGW → PGW

Steps:
- Authentication
- Security setup
- Session creation
- IP allocation
- Bearer activation

---

# ☁️ SECTION 2 – 5G CORE

---

## ❓ Q4: What is SMF role in 5G?

### 🧠 Answer:

SMF (Session Management Function):
- Creates PDU sessions
- Selects UPF
- Communicates with PCF
- Controls PFCP sessions

---

## ❓ Q5: What is PFCP?

### 🧠 Answer:

PFCP = protocol between SMF and UPF used to:
- Create session
- Modify session
- Delete session

> It controls user plane behavior

---

## ❓ Q6: Why can PDU session fail even if registration is successful?

### 🧠 Answer:

Because registration is only control plane.

Failure can occur due to:
- PCF policy rejection
- UPF unreachable
- SMF session failure
- DNN not authorized

---

# 🌐 SECTION 3 – TROUBLESHOOTING

---

## ❓ Q7: How do you analyze a PCAP?

### 🧠 Method:

1. Check registration / attach
2. Check session creation (GTP-C / PFCP)
3. Check DNS resolution
4. Check policy (OCS / PCF)
5. Check user plane (GTP-U)

---

## ❓ Q8: What causes intermittent data in LTE?

### 🧠 Answer:

- GTP-U packet loss
- DNS instability
- OCS timeout spikes
- PGW overload
- routing issues

---

# 🔥 SECTION 4 – WAR ROOM SCENARIO

---

## ❓ Q9: Network down in multiple countries. What do you check first?

### 🧠 Answer:

I would check:

- DNS health (global impact)
- OCS response time
- PCRF policy distribution
- UPF / PGW load
- Diameter signaling delay

Then correlate failures across domains.

---

## 🧠 Senior Engineer Thinking

> “A network issue is never isolated. It is always a chain reaction across control, policy, and user plane systems.”

---

# 🎯 FINAL INSIGHT

If:
- Control plane is OK
- But user plane is down

👉 Then issue is in:
- GTP-U
- PFCP
- Policy (PCF/PCRF)
- Charging (OCS)

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | Interview Preparation | EPC | 5G Core | Wireshark | Vendor-Level Troubleshooting
