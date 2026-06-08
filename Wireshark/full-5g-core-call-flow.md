# Full 5G Core End-to-End Call Flow (SA Architecture)

## 🧠 Objective

This document explains the complete 5G Standalone (SA) call flow from UE registration to user plane data session establishment.

Focus: AMF, SMF, UPF, PCF, NRF interaction.

---

# 📡 1. 5G Core Architecture Flow

```
UE → gNB → AMF → SMF → UPF → Data Network (DN)
              ↕     ↕
             PCF    NRF
```

---

# 📶 2. Step 1 – Registration (Control Plane)

## 📌 NGAP Signaling:

```
1. UE → gNB : Registration Request
2. gNB → AMF : Initial UE Message (NGAP)
3. AMF → UE : Authentication Request
4. UE → AMF : Authentication Response
```

✔ UE authenticated successfully

---

# 🔐 3. Step 2 – Security & Context Setup

```
5. AMF → UE : Security Mode Command
6. UE → AMF : Security Mode Complete
7. AMF ↔ UDM : Subscriber Data Retrieval
```

✔ UE context established in AMF

---

# 🌐 4. Step 3 – Session Establishment (SMF)

```
8. AMF → SMF : PDU Session Establishment Request
9. SMF → AMF : Session Context Setup
10. SMF → UPF : PFCP Session Establishment Request
11. UPF → SMF : PFCP Session Response
```

✔ User plane session created

---

# 📡 5. Step 4 – Policy Control (PCF)

```
12. SMF ↔ PCF : Policy Control Request/Response
```

### Actions:
- QoS assignment
- Slice selection
- Traffic rules installation

✔ Policy applied successfully

---

# 🔍 6. Step 5 – Service Registration (NRF)

```
13. SMF → NRF : Service Discovery
14. NRF → SMF : NF Profile Response
```

✔ Network function discovery completed

---

# 🟢 7. Step 6 – User Plane Activation (UPF)

```
UE ⇄ gNB ⇄ UPF ⇄ Data Network (Internet / IMS / App Server)
```

### Protocol:
- GTP-U over N3 interface
- UDP encapsulation

---

# 📊 Wireshark View:

Filter:
```
http2 || pfcp || ngap || gtp
```

---

# ⚠️ 8. Failure Scenarios in 5G Core

## ❌ Scenario A: Registration OK, No Data

Check:
- PFCP session not established
- UPF unreachable
- SMF policy failure

---

## ❌ Scenario B: PDU Session Failure

Check:
- SMF ↔ PCF policy reject
- Slice mismatch
- DNN not found

---

## ❌ Scenario C: Intermittent Data

Check:
- UPF overload
- PFCP session modification failure
- GTP-U packet loss

---

# 🔍 9. Protocol
