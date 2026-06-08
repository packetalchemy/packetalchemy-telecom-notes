# GTP Deep Dive – EPC Attach & User Plane Analysis

## 🧠 Objective

This document provides a deep packet-level breakdown of LTE EPC Attach and User Plane establishment using GTP-C and GTP-U.

Goal: Understand how a subscriber gets connectivity from packet perspective.

---

# 📡 1. Key GTP Concepts

## GTP has 2 planes:

### 🔵 GTP-C (Control Plane)
- Session creation
- Bearer setup
- Tunnel management

### 🟢 GTP-U (User Plane)
- Actual user data transport
- Encapsulated IP packets
- Runs over UDP port 2152

---

# 📶 2. LTE Attach Flow (Packet-Level)

## 📌 Frame 1: Attach Request
```
UE → MME
```

Subscriber requests network access.

---

## 📌 Frame 2: Authentication
```
MME ↔ HSS (Diameter S6a)
```

User identity verification.

---

## 📌 Frame 3: Create Session Request
```
MME → SGW → PGW
(GTP-C)
```

This is where session creation starts.

---

## 📌 Frame 4: Create Session Response
```
PGW → SGW → MME
```

Includes:
- IP address allocation
- QoS profile
- TEID assignment

✔ Session created successfully

---

## 📌 Frame 5: Bearer Setup
```
MME → SGW → PGW
```

Default bearer activated.

---

# 🌐 3. User Plane Activation (GTP-U)

## 📌 What happens:

Once session is active:

```
UE ⇄ SGW ⇄ PGW ⇄ Internet
```

Traffic is encapsulated in GTP-U tunnel.

---

## 📡 Wireshark View:

Filter:
```
udp.port == 2152
```

You will see:

- Encapsulated IP packets
- TEID identifiers
- Tunnel endpoints (SGW / PGW)

---

# ⚠️ 4. Common Failure Scenarios

## ❌ Case 1: No GTP-U traffic

### Symptoms:
- IP assigned
- No internet

### Root causes:
- SGW/PGW tunnel not established
- TEID mismatch
- Transport issue

---

## ❌ Case 2: GTP-C success but no data

### Symptoms:
- Create Session OK
- No UDP 2152 packets

### Root causes:
- User plane not activated
- Firewall blocking GTP-U
- Routing issue

---

## ❌ Case 3: One-way traffic

### Symptoms:
- Upload works
- Download fails

### Root causes:
- Asymmetric routing
- SGW/PGW misconfiguration

---

# 🔍 5. Key Wireshark Indicators

## GTP-C:
- Create Session Request
- Modify Bearer Request
- Delete Session Request

## GTP-U:
- UDP 2152 packets
- TEID mismatch
- Packet loss patterns

---

# 🧠 6. Senior Engineer Interpretation

A junior engineer says:
- “No internet, GTP problem”

A senior engineer says:
- “GTP-C session is established, but GTP-U tunnel is not forwarding due to TEID mismatch or SGW/PGW path issue.”

---

# 🔥 7. Engineering Insight

> “GTP-C creates the illusion of connectivity. GTP-U defines the reality of connectivity.”

---

# 📊 8. Debug Checklist

### Step 1:
Verify Attach success

### Step 2:
Check Create Session Response

### Step 3:
Validate TEID assignment

### Step 4:
Check GTP-U traffic (UDP 2152)

### Step 5:
Confirm end-to-end path

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | GTP Deep Dive | EPC | Wireshark Analysis | Packet-Level Troubleshooting
