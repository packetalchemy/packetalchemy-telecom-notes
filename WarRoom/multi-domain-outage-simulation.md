# War Room Simulation – Multi-Country Core Network Outage

## 🧠 Objective

Simulate a real operator War Room scenario involving simultaneous failures across EPC and 5G Core networks in multiple regions.

This is a senior-level troubleshooting exercise used in Tier-3 / Vendor escalation environments.

---

# 🌍 1. Incident Overview

## 🚨 Global Impact:
- Country A: LTE data failure
- Country B: 5G PDU session failures
- Country C: Intermittent connectivity
- Roaming services degraded

---

# 📡 2. Initial Symptoms

## EPC Users:
- Attach SUCCESS
- IP assigned
- No internet access

## 5G Users:
- Registration SUCCESS
- PDU session FAIL

## Roaming Users:
- Authentication delays
- Session drops

---

# 🔥 3. War Room Hypothesis

Multiple domains involved:

- DNS routing instability
- OCS / CHF charging degradation
- PCF policy failures
- UPF / SGW congestion
- Inter-PLMN routing issue

---

# 🧪 4. EPC Investigation

## 📌 Observations:

- GTP-C sessions OK
- GTP-U traffic dropping
- DNS resolution delayed or incorrect
- Diameter CCR timeout spikes

---

## ⚠️ EPC Root Symptoms:

- PGW overload
- DNS misrouting
- OCS latency

---

# ☁️ 5. 5G Core Investigation

## 📌 Observations:

- NGAP registration OK
- HTTP/2 SMF ↔ PCF failures
- PFCP session instability
- UPF partially unreachable

---

## ⚠️ 5G Root Symptoms:

- PCF policy degradation
- SMF session reject rate increase
- UPF congestion / overload

---

# 🌐 6. Cross-Domain Correlation

| Domain | Issue | Impact |
|------|------|--------|
| DNS | misrouting | wrong PGW/UPF selection |
| OCS | timeout | session blocking |
| PCF | policy failure | PDU reject |
| UPF | overload | no user plane |
| SGW/PGW | congestion | GTP-U loss |

---

# 🚨 7. Root Cause Chain

## PRIMARY FAILURE:
👉 Central DNS routing instability

## CASCADING EFFECTS:

1. Wrong PGW / UPF selection
2. OCS timeout increases
3. PCF policy mismatch
4. GTP-U session instability
5. Multi-region service degradation

---

# 🧠 8. Senior Engineer War Room Thinking

Junior engineer:
- “Network is down everywhere”

Senior engineer:
- “We are observing a cascading failure initiated by DNS misrouting, propagating into policy, charging, and user plane instability across EPC and 5GC domains.”

---

# 🔥 9. Recovery Actions

## Step 1:
Stabilize DNS resolution

## Step 2:
Restart OCS / CHF interfaces

## Step 3:
Re-sync PCF policies

## Step 4:
Clear affected GTP sessions

## Step 5:
Re-establish PFCP sessions

---

# 📊 10. War Room Checklist

- [ ] DNS health check
- [ ] Diameter response time
- [ ] PCF policy status
- [ ] PFCP session stability
- [ ] GTP-U traffic validation
- [ ] Regional load balancing status

---

# 🧠 Engineering Insight

> “A War Room incident is never a single failure. It is always a chain reaction across multiple control systems.”

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | War Room Simulation | EPC | 5G Core | Vendor-Level Incident Analysis | Multi-Domain Troubleshooting
