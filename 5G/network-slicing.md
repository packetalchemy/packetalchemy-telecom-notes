# 5G Network Slicing (PacketAlchemy)

## 🧠 Overview

Network Slicing is a key 5G feature that allows a single physical network to be divided into multiple logical networks.

Each slice is:
- Isolated
- Customizable
- Optimized for a specific service type

---

# 🧩 1. What is a Slice?

A slice is a **virtual end-to-end network** with its own:

- AMF (or shared AMF)
- SMF
- UPF
- PCF policies
- QoS rules
- Data Network (DN)

---

# 🍰 2. Common Slice Types

## 📡 eMBB (Enhanced Mobile Broadband)
- High speed data
- Video streaming
- Internet browsing

## 🚗 URLLC (Ultra Reliable Low Latency)
- Autonomous driving
- Industrial control
- Real-time systems

## 📶 mMTC (Massive IoT)
- Sensors
- Smart meters
- IoT devices

---

# 🏗️ 3. Slice Architecture

```
                +-------------------+
                |       NRF         |
                +-------------------+
                         |
        +----------------+----------------+
        |                                 |
   +----+----+                      +-----+-----+
   |  AMF     |                      |   AMF     |
   | (Shared)  |                      | (Shared)  |
   +----+----+                      +-----+-----+
        |                                 |
   +----+----+                      +-----+-----+
   |   SMF    |                      |   SMF     |
   +----+----+                      +-----+-----+
        |                                 |
   +----+----+                      +-----+-----+
   |   UPF    |                      |   UPF     |
   +----+----+                      +-----+-----+
        |                                 |
   Internet / IoT / Industrial Network per slice
```

---

# 🌐 4. Slice Selection Process

## Step 1: UE Request

UE sends:
- S-NSSAI (Slice Identifier)
- DNN (Data Network Name)

---

## Step 2: AMF Selection

AMF selects correct:
- Slice context
- Allowed services

---

## Step 3: SMF Assignment

SMF chosen based on:
- Slice type
- Policy rules
- Load balancing

---

## Step 4: UPF Allocation

UPF selected based on:
- Geography
- Latency requirements
- Slice policy

---

# 📊 5. S-NSSAI (Slice Identifier)

Example:

```
S-NSSAI = {
  SST: 1 (eMBB)
  SD: 010203
}
```

---

# ⚠️ 6. Common Slice Issues

## ❌ Slice Not Available

- UE requests unsupported slice
- AMF rejects registration

---

## ❌ Wrong Slice Assigned

- Misconfigured NSSAI mapping
- Incorrect policy in PCF

---

## ❌ No Internet in Slice

- UPF not attached to correct DN
- SMF misconfiguration
- DNS issue inside slice

---

# 💳 7. Policy Control in Slicing (PCF)

PCF defines:

- Per-slice QoS
- Bandwidth limits
- Access rules
- Charging rules

Each slice can have **different policy behavior**

---

# 🌍 8. DNS Role in Slicing

DNS is slice-aware:

- Different UPF per slice
- Different DNN resolution
- Traffic steering per service type

---

# 🧠 9. Key Engineering Insight

Network slicing is NOT just separation.

It is:

> “Multiple virtual operators inside one physical network”

---

# 🔥 10. Troubleshooting Mindset

## Slice Issue Debug Flow:

1. Check NSSAI (is slice requested?)
2. Check AMF (slice supported?)
3. Check SMF (session created?)
4. Check UPF (traffic path?)
5. Check PCF (policy applied?)
6. Check DNS (correct routing?)

---

# 📌 Final Rule

> “In 5G slicing, the same user can behave like 3 different users depending on slice context.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | Network Slicing | SBA | AMF | SMF | UPF
