# Online Charging System (OCS) in LTE

## 💳 Overview

The Online Charging System (OCS) is responsible for real-time credit control in mobile networks.

It decides whether a subscriber is allowed to:
- Start a data session
- Continue usage
- Or be blocked due to insufficient credit or policy

---

## 🧩 Key Components

- PGW (PCEF function)
- OCS (Online Charging System)
- PCRF (Policy Control)
- HSS (Subscription data)

---

## 🔌 Main Interface: Gy (Diameter)

The PGW communicates with OCS using the **Gy interface** based on Diameter protocol.

---

## 🔄 Basic Charging Flow

### 1. Session Request
UE requests data session (APN activation)

### 2. Credit Request
PGW sends:
- CCR (Credit-Control-Request) → OCS

### 3. OCS Decision
OCS responds with:
- CCA (Credit-Control-Answer)

---

## 📊 OCS Response Codes (Important)

### ✅ 2001 - Success
- Subscriber has enough credit
- Data session allowed

### ❌ 4012 - Credit Control Not Applicable / No Credit
- Subscriber has no balance
- Session rejected or restricted

---

## 🔄 Real Behavior in Network

### Case 1: Successful Session

PGW → OCS:
- CCR (Initial Request)

OCS → PGW:
- CCA (Granted Units)

➡️ UE gets IP and internet access

---

### Case 2: No Credit (4012)

PGW → OCS:
- CCR (Initial Request)

OCS → PGW:
- CCA (Result Code 4012)

➡️ UE behavior:
- Attach may succeed
- Data session blocked
- Redirect / walled garden possible

---

## 🌍 Roaming Impact

In roaming scenarios:
- OCS may apply stricter policies
- Some APNs are blocked
- Only SMS/voice allowed

---

## ⚠️ Common Issues

- No response from OCS (Diameter timeout)
- Wrong APN mapping
- Subscriber barred in roaming
- PCRF policy conflict
- Credit exhausted mid-session

---

## 🔌 Related Interfaces

- Gy (PGW ↔ OCS)
- Gx (PGW ↔ PCRF)
- S6a (MME ↔ HSS)

---

## 🧠 PacketAlchemy Note

OCS is not just "billing system".

It is a **real-time traffic gatekeeper** that directly controls user experience in LTE/5G networks.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | Charging | LTE | EPC
