# Virtual Mobile Operator Design (EPC + 5GC)

## 🧠 Overview

This document describes how to design a complete mobile operator network from scratch using:
- EPC (LTE Core)
- 5G Core (5GC)
- Interworking architecture
- Charging, policy, DNS, and routing systems

---

# 🏢 1. Operator Vision

A modern mobile operator must support:

- 4G LTE users
- 5G SA users
- 5G NSA users
- Roaming subscribers
- IoT devices
- Enterprise slicing customers

---

# 🏗️ 2. High-Level Architecture

```
                INTERNET / DN
                      ↑
        +-------------------------------+
        |        CORE NETWORK           |
        |                               |
        |  5GC          EPC             |
        | AMF SMF UPF  MME SGW PGW     |
        | PCF UDM NRF  HSS PCRF        |
        +-------------------------------+
                      ↑
                RAN (LTE / NR)
                      ↑
                    UE
```

---

# 🌐 3. Core Domains Design

## 📡 EPC Domain

- MME → mobility control
- SGW → user plane anchor
- PGW → internet gateway
- HSS → subscriber database
- PCRF → policy + charging control

---

## ☁️ 5GC Domain

- AMF → registration & mobility
- SMF → session management
- UPF → user plane forwarding
- UDM → subscriber data
- PCF → policy control
- NRF → service discovery

---

# 💳 4. Charging Architecture

## EPC:
- OCS (Online Charging System)
- Offline billing system

## 5GC:
- CHF (Charging Function)

---

## Flow:

UE → PGW/SMF → OCS/CHF → Allow or Block

---

# 🌍 5. DNS & Routing Design

## Responsibilities:

- APN resolution (EPC)
- DNN resolution (5GC)
- PGW selection
- UPF selection
- Roaming routing

---

# 🔗 6. Interworking Strategy

## Key Requirement:
EPC and 5GC must coexist.

### Methods:

- N26 interface (MME ↔ AMF)
- Dual core deployment
- Shared subscriber database (HSS ↔ UDM sync)
- Policy translation (PCRF ↔ PCF)

---

# 🍰 7. Network Slicing Strategy

Operator supports:

## eMBB
- High speed internet users

## URLLC
- Industrial / low latency users

## mMTC
- IoT massive devices

Each slice has:
- Separate policy
- Separate QoS
- Possibly separate UPF

---

# ⚠️ 8. Real Operator Challenges

## ❌ 1. Roaming Complexity
- Multiple DNS layers
- Home vs visited core mismatch

## ❌ 2. Charging Inconsistency
- OCS vs CHF alignment issues

## ❌ 3. Policy Conflict
- PCRF vs PCF mismatch

## ❌ 4. Session Continuity
- EPC ↔ 5GC handover breaks

---

# 🧠 9. Engineering Principles

A real operator design must ensure:

- High availability (99.999%)
- Geo-redundancy
- Seamless mobility
- Unified subscriber experience
- Real-time charging accuracy

---

# 🔥 10. Final Architecture Philosophy

> “An operator is not a network. It is a distributed real-time decision system for billions of sessions.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | Virtual Operator Design | EPC | 5GC | Network Architecture | Vendor-Level Thinking
