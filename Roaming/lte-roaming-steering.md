# LTE Roaming & Steering of Roaming (SoR)

## 🌍 Overview

In LTE roaming, a subscriber attaches to a visited network while still being controlled by the home operator for authentication, policy, and charging.

Steering of Roaming (SoR) is a mechanism used by the home operator to influence which visited network the UE should select.

---

## 🧩 Key Network Elements

### Home Network
- HSS / UDM
- PCRF / PCF
- OCS (Online Charging System)
- DNS

### Visited Network
- MME / AMF
- SGW / PGW (or UPF in 5GC)
- VPLMN infrastructure

---

## 🔄 Roaming Attach Flow (LTE)

### 1. Attach Request (Visited Network)
UE attaches to visited eNodeB and MME.

### 2. Identification
Visited MME identifies IMSI as roaming subscriber.

### 3. HSS Query (S6a Diameter)
Visited MME contacts Home HSS:
- Update Location Request (ULR)
- Update Location Answer (ULA)

---

### 4. Authentication
Home HSS provides authentication vectors.

---

### 5. Session Setup
- SGW/PGW selection may be local or home-routed
- GTP-C session establishment

---

### 6. Policy & Charging
- PCRF provides policy rules
- OCS may enforce credit control (Gy interface)

---

## 🚦 Steering of Roaming (SoR)

Home operator can control roaming via:

### Methods:

- DNS-based steering
- APN restriction
- HSS subscription profile
- Diameter routing rules
- PLMN prioritization list

---

## ⚠️ Real Operator Scenarios

### Scenario 1: Preferred Roaming Partner

Subscriber should connect to Partner A, not Partner B.

→ Implemented via:
- HSS subscription profile
- PLMN list control

---

### Scenario 2: Data Restriction in Roaming

Subscriber attaches successfully but:
- No internet access
- Only SMS or voice allowed

→ Causes:
- PCRF policy restriction
- OCS credit rule (4012-like behavior)
- APN not allowed in roaming profile

---

### Scenario 3: Home Routed vs Local Breakout

- Home Routed: traffic goes to home PGW
- Local Breakout: traffic exits in visited network

---

## 🔌 Key Interfaces

- S6a (MME ↔ HSS)
- Gy (PGW ↔ OCS)
- Gx (PGW ↔ PCRF)
- S11 (MME ↔ SGW)
- S5/S8 (SGW ↔ PGW)

---

## 🧠 Troubleshooting Notes

Common roaming issues:

- No Update Location Answer (HSS issue)
- Diameter timeout (S6a failure)
- APN not allowed in roaming
- DNS resolution failure in visited network
- OCS rejects due to credit control
- Incorrect PLMN selection

---

## 📌 PacketAlchemy Note

Roaming issues are rarely single-point failures.

Most cases involve:
- Policy (PCRF)
- Charging (OCS)
- Subscription (HSS)
- Routing (DNS / Diameter)

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | LTE | Roaming | Charging
