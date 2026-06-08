# 5G Core Evolution (EPC → 5GC)

## 🧠 Overview

5G Core (5GC) introduces a complete architectural shift from EPC.

Instead of node-based architecture (MME, SGW, PGW), 5GC uses:
- Service-Based Architecture (SBA)
- Microservice-like Network Functions (NFs)
- HTTP/2 + REST APIs instead of Diameter

---

# 🏗️ 1. Architecture Change

## 📡 EPC (LTE)

- MME
- SGW
- PGW
- HSS
- PCRF

---

## 📡 5G Core

- AMF (Access & Mobility Management Function)
- SMF (Session Management Function)
- UPF (User Plane Function)
- UDM (Unified Data Management)
- PCF (Policy Control Function)
- NRF (Network Repository Function)

---

# 🔄 2. Key Concept Shift

| EPC | 5G Core |
|-----|--------|
| MME | AMF |
| SGW/PGW | UPF + SMF |
| PCRF | PCF |
| HSS | UDM |
| Diameter | HTTP/2 APIs |
| Point-to-point | Service-Based Architecture |

---

# 🌐 3. Service-Based Architecture (SBA)

In 5GC:

- Network Functions expose APIs
- Communication is service-based
- NRF acts as service registry

---

## 📡 Example Flow

```
AMF → NRF (find SMF)
SMF → UPF (session setup)
SMF → PCF (policy request)
SMF → UDM (subscriber data)
```

---

# 📡 4. 5G Session Establishment

## Step 1: UE Registration
UE → AMF

## Step 2: Authentication
AMF → UDM

## Step 3: Session Setup
AMF → SMF

## Step 4: Policy Decision
SMF → PCF

## Step 5: User Plane Setup
SMF → UPF

---

# 📦 5. User Plane in 5G

## UPF replaces:
- SGW
- PGW

## Responsibilities:
- Packet forwarding
- QoS enforcement
- Traffic steering

---

# ⚠️ 6. Common 5G Issues

## ❌ Registration Failure
- AMF unreachable
- UDM auth failure

## ❌ Session Establishment Failure
- SMF policy reject
- PCF rule missing

## ❌ No Internet
- UPF misrouting
- Data network (DN) issue

---

# 🌍 7. DNS Role in 5G

DNS is still critical for:

- NRF discovery
- NF service lookup
- UPF selection
- DNN (Data Network Name) resolution

---

# 💳 8. Policy Control (PCF)

PCF replaces PCRF:

- QoS rules
- Access control
- Slice-based policy
- Session control rules

---

# 🧠 9. Key Engineering Insight

5G is NOT just faster LTE.

It is:

> “A cloud-native, API-driven telecom system”

---

# 🔥 10. Troubleshooting Mindset Shift

## EPC Thinking:
- Check nodes
- Check interfaces (GTP, Diameter)

## 5G Thinking:
- Check services
- Check APIs
- Check NF discovery (NRF)
- Check HTTP/2 transactions

---

# 📌 Final Rule

> “In 5G, you don’t debug links — you debug services.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | SBA | AMF | SMF | UPF | Network Evolution
