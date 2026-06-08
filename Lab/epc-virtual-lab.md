# EPC Virtual Lab (PacketAlchemy)

## 🧪 Overview

This is a virtual LTE/EPC lab designed to simulate real operator core network behavior.

It helps understand:
- Control Plane (GTP-C, Diameter)
- User Plane (GTP-U)
- Policy Control (PCRF)
- Charging (OCS)
- Routing (DNS)

---

# 🏗️ 1. Virtual Network Topology

```
             +------------------+
             |      HSS         |
             +------------------+
                      |
                      | S6a (Diameter)
                      |
+------+      +------+------+
| UE   | ---> |    MME      |
+------+      +------+------+
                      |
                      | S11 (GTP-C)
                      |
               +------+------+
               |     SGW      |
               +------+------+
                      |
            +---------+----------+
            |                    |
        S5/S8                 Gx / Gy
            |                    |
       +----+----+        +-----+-----+
       |   PGW    | -----> | PCRF / OCS |
       +----+----+        +-----------+
            |
         Internet
```

---

# 🔄 2. Full Attach Simulation Flow

## Step 1: UE Attach
UE → MME

## Step 2: Authentication
MME → HSS (S6a)

## Step 3: Session Creation
MME → SGW → PGW (GTP-C)

## Step 4: Policy Decision
PGW → PCRF (Gx)

## Step 5: Charging Decision
PGW → OCS (Gy)

## Step 6: IP Allocation
PGW assigns IP to UE

## Step 7: User Data Flow
UE ↔ Internet (via GTP-U tunnel)

---

# 🌐 3. DNS Role in Lab

DNS is used for:

- PGW selection
- APN resolution
- Roaming routing

Example:
```
internet.operator.com → PGW1 / PGW2 / PGW3
```

---

# 💳 4. Charging Behavior (OCS)

| Condition | Result |
|----------|--------|
| 2001 | Allow data |
| 4012 | Block / restrict |
| Timeout | Session pending |

---

# 🧠 5. Policy Control (PCRF)

PCRF defines:

- QoS (QCI)
- Bandwidth
- Allowed services
- Roaming restrictions

---

# 📡 6. Traffic Plane Separation

## Control Plane:
- GTP-C
- Diameter

## User Plane:
- GTP-U (UDP 2152)

---

# ⚠️ 7. Failure Simulation Scenarios

## ❌ Scenario A: No Internet

- GTP-C OK
- IP assigned OK
- OCS = 4012
- Result: No data

---

## ❌ Scenario B: Wrong PGW

- DNS misroutes APN
- Traffic goes to wrong region

---

## ❌ Scenario C: GTP-U Broken

- Tunnel exists
- No packet flow
- Firewall blocking UDP 2152

---

# 🧠 PacketAlchemy Insight

A real EPC network is NOT a single system.

It is a **chain of independent decision engines**:

- DNS → routing decision
- PCRF → policy decision
- OCS → charging decision
- GTP-C → session decision
- GTP-U → data transport

---

# 🎯 Engineering Goal

If you understand this lab:

✔ You can debug real operator issues  
✔ You can understand vendor diagrams  
✔ You can analyze Wireshark captures  
✔ You can design core networks logically  

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | EPC Lab | LTE | 5G Core | Network Simulation
