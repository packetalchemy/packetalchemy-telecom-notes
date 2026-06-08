# Core Network Interview Pack (EPC / 5GC)

## 🧠 Overview

This document contains real interview questions and structured answers for:
- EPC (LTE Core)
- 5G Core (5GC)
- Troubleshooting scenarios
- Architecture design thinking

---

# 📡 1. Basic Core Network Questions

## ❓ What happens during LTE attach?

### Answer:
1. UE sends Attach Request
2. MME authenticates with HSS
3. SGW/PGW session created (GTP-C)
4. IP assigned to UE
5. PCRF applies policy
6. Data path established via GTP-U

---

## ❓ Difference between SGW and PGW?

### Answer:
- SGW → Mobility anchor, handles user plane forwarding
- PGW → Internet gateway, IP allocation, policy enforcement point

---

# 💳 2. Charging Questions

## ❓ What is OCS role?

### Answer:
OCS performs real-time charging control using Diameter Gy interface.
It decides:
- Allow
- Block
- Restrict service

---

## ❓ What does Result-Code 4012 mean?

### Answer:
Subscriber has no valid credit or service is restricted.

---

# 🌐 3. DNS Questions

## ❓ Why is DNS important in EPC?

### Answer:
DNS is used for:
- APN to PGW mapping
- Routing decisions
- Roaming resolution

Misconfiguration leads to wrong PGW selection and service failure.

---

# ☁️ 4. 5G Core Questions

## ❓ What replaces MME in 5G?

### Answer:
AMF (Access and Mobility Management Function)

---

## ❓ What replaces PCRF?

### Answer:
PCF (Policy Control Function)

---

## ❓ What is NRF?

### Answer:
Network Repository Function.
It acts as a service registry for all 5G network functions.

---

# 🔄 5. Interworking Questions

## ❓ How does EPC and 5GC interwork?

### Answer:
- N26 interface connects MME and AMF
- Subscriber context is transferred
- Session continuity is maintained
- DNS + policy mapping ensures compatibility

---

# ⚠️ 6. Troubleshooting Scenarios

## ❓ User has IP but no internet?

### Structured Answer:

Check in order:

1. GTP-C → Is session created?
2. OCS → Is subscriber blocked (4012)?
3. PCRF → Is policy applied?
4. DNS → Correct PGW/UPF selected?
5. GTP-U → Is traffic flowing?

---

## ❓ Intermittent data issue?

### Answer:
Likely causes:
- SGW/UPF overload
- QoS mismatch
- Transport network instability
- Packet loss in GTP-U path

---

# 🧠 7. Architecture Question

## ❓ Design a modern mobile operator?

### Answer structure:

- EPC for LTE users
- 5GC for 5G users
- Interworking via N26
- DNS-driven routing
- Centralized charging (OCS/CHF)
- Policy control (PCRF/PCF)
- Geo-redundant core

---

# 🔥 8. Advanced Interview Questions

## ❓ Why is 5G architecture better than EPC?

### Answer:
- Service-based architecture (SBA)
- Cloud-native design
- Stateless network functions
- Scalable microservices
- Dynamic service discovery (NRF)

---

## ❓ What is biggest failure point in telecom networks?

### Answer:
DNS + Policy + Charging interaction layer  
Because it affects routing, access, and monetization simultaneously.

---

# 🧠 Final
