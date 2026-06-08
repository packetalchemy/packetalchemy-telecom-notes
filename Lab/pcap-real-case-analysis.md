# PCAP Real Case Analysis – Core Network Troubleshooting

## 🧠 Objective

Learn how to analyze real PCAP files and extract root cause in EPC / 5GC networks.

This lab simulates real operator troubleshooting using Wireshark captures.

---

# 📡 1. What is PCAP in Telecom?

PCAP = Packet Capture file containing real network traffic.

Used for:
- Troubleshooting core network issues
- Analyzing signaling flows
- Debugging user plane problems
- Validating DNS / OCS / Policy behavior

---

# 🧪 2. Typical PCAP Sources in Operator Network

- S1 interface (eNB ↔ MME)
- S5/S8 (SGW ↔ PGW)
- N3 interface (gNB ↔ UPF)
- Diameter interfaces (OCS / PCRF / HSS)
- DNS queries from core nodes

---

# 📊 3. How to Analyze a PCAP (Methodology)

## Step 1: Identify protocol layers
- Ethernet
- IP
- UDP/TCP
- Application layer (GTP / Diameter / DNS)

---

## Step 2: Filter by key protocols

```
gtp
diameter
dns
udp.port == 2152
http2
```

---

## Step 3: Reconstruct session flow

You must answer:

- Where did session start?
- Where did it fail?
- Which node stopped responding?

---

# 📡 4. Example Case: "No Internet After Attach"

## 📌 Observed Symptoms:
- Attach SUCCESS
- IP assigned
- No internet traffic

---

## 🔍 PCAP Findings:

### 1. GTP-C Flow
✔ Create Session Request → OK  
✔ Create Session Response → OK  

👉 Session created successfully

---

### 2. DNS Traffic
❌ DNS query timeout detected  
❌ No valid response from resolver  

👉 Wrong PGW/UPF selection likely

---

### 3. Diameter (OCS)
✔ CCR sent  
❌ No CCA received  

👉 Possible OCS timeout or blocking

---

### 4. GTP-U Traffic
❌ No UDP 2152 packets observed  

👉 User plane not established

---

# 🚨 5. Root Cause Analysis

## PRIMARY ISSUE:
👉 DNS resolution failure (APN/DNN misrouting)

## SECONDARY ISSUE:
👉 OCS timeout causing session restriction

---

# 🧠 6. Final Conclusion

> “Session was successfully created in control plane, but service delivery failed due to DNS + charging layer breakdown.”

---

# 🔥 7. Senior Engineer Thinking Pattern

A junior engineer says:
- “No internet”

A senior engineer says:
- “GTP-C is OK, but DNS and Diameter are failing, so user plane is not activated.”

---

# 📊 8. Key PCAP Indicators

| Layer | What to check |
|------|--------------|
| GTP-C | Session creation |
| GTP-U | Traffic flow |
| DNS | APN resolution |
| Diameter | Charging status |
| HTTP/2 | 5G signaling |

---

# 🧠 Engineering Insight

> “PCAP analysis is not reading packets. It is reconstructing network behavior from fragments of truth.”

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | PCAP Analysis | Wireshark | EPC | 5G Core | Troubleshooting
