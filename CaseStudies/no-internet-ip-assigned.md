# Case Study: IP Assigned but No Internet Access

## 🧠 Scenario Overview

A subscriber successfully attaches to the network and receives an IP address, but cannot access any internet services.

Symptoms:
- Attach successful (MME/AMF OK)
- IP address assigned
- DNS resolution failing or inconsistent
- No HTTP/HTTPS traffic
- Intermittent service in some cases

---

# 📡 1. Initial Observations

## EPC Side:
- MME: OK
- SGW: OK
- PGW: Session established

## 5GC Side:
- AMF: OK
- SMF: Session created
- UPF: Uncertain traffic flow

---

# 🔍 2. Key Investigation Areas

## Step 1: GTP-C Check

- Is session created successfully?
- Are Create Session / Modify Bearer messages successful?

👉 If YES → move forward

---

## Step 2: DNS Validation

Check:
- APN resolution (EPC)
- DNN resolution (5GC)
- Correct PGW / UPF selection

⚠️ Issue found in many cases:
- Wrong DNS response
- Cached stale entry
- Regional DNS mismatch

---

## Step 3: OCS / Charging Check

Verify:
- Diameter CCR/CCA flow
- Result-Code = 2001 (OK) or 4012 (blocked)

👉 If blocked → no internet despite IP

---

## Step 4: Policy Control (PCRF / PCF)

Check:
- QoS rules applied?
- Rule installation success?
- Bearer policy active?

⚠️ Misconfig leads to:
- Data throttling
- Zero rating issues
- Full session block

---

## Step 5: GTP-U / User Plane Check

Verify:
- Tunnel established?
- Packet flow on UDP 2152 (EPC)?
- N3 interface traffic (5GC)?

👉 If tunnel exists but no traffic:
- UPF/SGW forwarding issue

---

# 🌐 3. Wireshark Indicators

## EPC (GTP-U):
- UDP port 2152 traffic missing or one-way
- No Echo response from PGW

## Diameter:
- CCR sent but no CCA response
- Timeout observed

## DNS:
- NXDOMAIN or delayed response

---

# ⚠️ 4. Root Cause Scenarios

## Scenario A: DNS Failure
- Wrong PGW/UPF selection
- Session created but no routing

## Scenario B: OCS Blocking
- Subscriber out of credit (4012)
- Policy denies data session

## Scenario C: GTP-U Failure
- Tunnel established but no forwarding
- SGW/UPF misconfiguration

## Scenario D: Policy Issue
- PCRF/PCF rule not installed
- Bearer marked inactive

---

# 🛠️ 5. Resolution Steps

1. Validate DNS response consistency
2. Check OCS/CHF response codes
3. Restart policy session (PCRF/PCF)
4. Re-establish GTP session if needed
5. Verify UPF / SGW forwarding tables

---

# 🧠 Engineering Insight

> “In telecom networks, having an IP does not mean having connectivity. It only means session creation succeeded — not service delivery.”

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | EPC | 5G Core | Troubleshooting | Wireshark Case Studies
