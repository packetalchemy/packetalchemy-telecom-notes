# Vendor Design Deep Dive (EPC & 5GC Architecture)

## 🧠 Overview

This document explains how telecom vendors (Ericsson, Huawei, Nokia) design:
- EPC (LTE Core)
- 5G Core (SBA architecture)
- Interworking strategies
- High availability and scalability design

---

# 🏗️ 1. Design Philosophy Shift

## 📡 EPC Era (Traditional Telecom)

- Hardware-centric design
- Node-based architecture
- Static routing and configuration
- Tight coupling between functions

### Key Nodes:
- MME
- SGW
- PGW
- HSS
- PCRF

---

## 📡 5G Era (Cloud-Native Telecom)

- Software-defined network functions
- Microservices architecture
- HTTP/2 APIs (SBA)
- Cloud orchestration (Kubernetes-based in many deployments)

### Key Functions:
- AMF
- SMF
- UPF
- UDM
- PCF
- NRF

---

# 🧩 2. Vendor Architecture Principles

## 📌 Principle 1: Separation of Planes

- Control Plane → AMF / SMF / MME
- User Plane → UPF / SGW / PGW
- Management Plane → OAM systems

---

## 📌 Principle 2: Stateless Design (5G)

- Network functions should be stateless
- State stored in UDM / external DB
- Enables scaling and redundancy

---

## 📌 Principle 3: Service Discovery (NRF)

Instead of static configuration:

- NF registers itself in NRF
- Other NFs discover services dynamically

---

# 🌐 3. EPC Design (Vendor View)

## Architecture:

```
UE → eNB → MME → SGW → PGW → Internet
              ↓
             HSS
              ↓
            PCRF
```

## Design Characteristics:

- Diameter-based signaling
- Static interfaces (S1, S5, S6a, Gx, Gy)
- Centralized control
- Scaling via hardware expansion

---

# ☁️ 4. 5G Design (Vendor View)

## Architecture:

```
UE → gNB → AMF → SMF → UPF → Data Network
             ↓     ↓
            UDM   PCF
             ↓
            NRF
```

## Design Characteristics:

- API-based communication (HTTP/2 REST)
- Service discovery via NRF
- Cloud-native deployment
- Horizontal scaling

---

# 🔗 5. Interworking Design Strategy

Vendors design EPC ↔ 5GC coexistence using:

## Option 1: N26 Interface
- MME ↔ AMF connectivity
- Mobility context transfer

## Option 2: Dual-Core Deployment
- EPC handles LTE users
- 5GC handles 5G users

## Option 3: Shared UPF / PGW Anchoring
- Common user plane anchoring for continuity

---

# ⚠️ 6. Real Design Challenges

## ❌ 1. Session Continuity

- EPC uses GTP-U tunnels
- 5GC uses N3 interface

👉 Challenge: maintaining session during migration

---

## ❌ 2. Policy Translation

- PCRF (EPC)
- PCF (5GC)

👉 Vendors must map policy logic between systems

---

## ❌ 3. DNS & Service Discovery Conflict

- EPC uses APN-based DNS
- 5GC uses DNN + NRF-based discovery

---

## ❌ 4. Charging System Migration

- OCS (EPC)
- CHF (5GC charging function)

👉 Requires billing system redesign

---

# 🧠 7. Vendor Engineering Strategy

Vendors design networks with:

## 📌 High Availability
- Active-active redundancy
- Geo-redundant cores

## 📌 Scalability
- Horizontal scaling (5G)
- NF decomposition

## 📌 Backward Compatibility
- EPC must coexist with 5GC
- Gradual migration strategy

---

# 🔥 8. Real Operator Design Model

```
          +----------------------+
          |      5G Core         |
          | AMF SMF UPF PCF NRF |
          +----------------------+
                    ↑
                    | Interworking
                    ↓
          +----------------------+
          |       EPC Core       |
          | MME SGW PGW PCRF    |
          +----------------------+
```

---

# 🧠 9. Key Vendor Insight

> “Operators do not replace EPC with 5GC. They evolve it gradually.”

---

# 📌 10. Engineering Takeaway

A real telecom architect must think in terms of:

- Service continuity
- Migration risk
- Dual-stack operation
- Policy consistency
- User experience stability

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | Vendor Architecture | EPC | 5GC | Ericsson | Huawei | Nokia Design Principles
