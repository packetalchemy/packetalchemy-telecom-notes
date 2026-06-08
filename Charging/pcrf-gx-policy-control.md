# PCRF / Policy Control in LTE (Gx Interface)

## 🧠 Overview

PCRF (Policy and Charging Rules Function) is responsible for real-time policy control in LTE networks.

It decides:
- What the user can access
- Bandwidth limits
- QoS rules
- Charging behavior

---

## 🔌 Key Interface: Gx

PCRF communicates with PGW (PCEF) via the **Gx interface** using Diameter protocol.

---

## 🧩 Main Network Elements

- UE (User Equipment)
- PGW (PCEF function)
- PCRF
- OCS (Online Charging System)

---

## 🔄 Basic Policy Flow

### 1. Session Establishment
UE requests data session (APN)

### 2. PGW Request
PGW sends:
- CCR (Credit Control Request) to PCRF

### 3. PCRF Decision
PCRF evaluates:
- Subscription profile
- QoS rules
- Charging rules
- Roaming restrictions

### 4. Policy Response
PCRF sends:
- CCA with policy rules

---

## 📊 What PCRF Controls

### 🎯 QoS Control
- QCI (Quality of Service Class Identifier)
- Bandwidth limits
- Priority handling

### 🌐 Access Control
- Allowed / blocked APN
- Service restriction
- Roaming policies

### 💳 Charging Interaction
- Works with OCS for credit-based decisions
- May block or allow based on balance

---

## ⚠️ Real Network Scenarios

### Case 1: Full Access Allowed

PCRF response:
- Allow internet APN
- Assign QoS profile

➡️ User gets full internet access

---

### Case 2: Limited Access

PCRF response:
- Allow attach
- Restrict bandwidth
- Block specific services (e.g. video streaming)

---

### Case 3: No Internet Access

PCRF response:
- Default bearer only
- No data policy assigned

➡️ User:
- Attached successfully
- But no internet access

---

## 🌍 Roaming Impact

In roaming:
- PCRF applies stricter policies
- Some APNs are blocked
- Steering of Roaming rules may apply
- OCS and PCRF may both restrict access

---

## 🔌 Interfaces Summary

- Gx → PCRF ↔ PGW
- Gy → PGW ↔ OCS
- S6a → MME ↔ HSS

---

## 🧠 Troubleshooting Notes

Common PCRF issues:

- No Gx response (Diameter timeout)
- Wrong QoS assignment
- APN not allowed
- Roaming restriction applied
- Conflict with OCS policy
- Default rule applied (deny all)

---

## 📌 PacketAlchemy Insight

Most "No Internet" issues in LTE are NOT RF problems.

They are usually caused by:
- PCRF policy denial
- OCS credit restriction
- APN misconfiguration

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | LTE | PCRF | Policy Control
