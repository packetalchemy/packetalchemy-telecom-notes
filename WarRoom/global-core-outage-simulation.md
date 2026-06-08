# Global War Room Simulation (Core Network Outage)

## 🧠 Scenario Overview

A major mobile operator is experiencing a **partial nationwide outage** affecting both LTE and 5G users.

Symptoms reported:
- Users cannot access internet
- Attach success rate is high
- Voice/SMS still working
- Roaming users heavily impacted
- Intermittent service restoration in some regions

---

# 🚨 1. Initial Impact Assessment

## Observations:
- ✔ Attach success (MME / AMF OK)
- ❌ Data sessions failing
- ❌ High latency in multiple regions
- ❌ Roaming degradation

👉 This immediately isolates:
- RAN is healthy
- Core data plane / policy / routing issue

---

# 🔍 2. Control Plane Check

## EPC:
- MME OK
- SGW OK
- PGW partially responsive

## 5GC:
- AMF OK
- SMF intermittent failures
- UPF unstable in some regions

---

# 💳 3. Charging System Status

## OCS:
- High latency observed
- CCR timeout spikes
- Some sessions rejected with 4012

## CHF (5GC):
- API response delays
- Partial failure in policy binding

---

# 🌐 4. DNS & Routing Failure

## Symptoms:
- Wrong PGW selection
- Incorrect UPF mapping
- Regional imbalance

## Root Indicators:
- DNS response delay increased
- FQDN resolution failures
- Cache inconsistency between nodes

---

# 🔗 5. GTP-C / GTP-U Analysis

## GTP-C:
- Session creation delayed
- Some Create Session failures

## GTP-U:
- Tunnel established but no traffic in affected regions
- UDP 2152 packet loss detected

---

# ☁️ 6. 5G Core (SBA) Issues

## Observations:
- NRF service discovery slow
- SMF unable to locate UPF consistently
- PCF policy fetch timeout

---

# 🧩 7. Cross-Domain Correlation

| Domain | Status |
|--------|-------|
| RAN | OK |
| MME / AMF | OK |
| SGW / PGW | DEGRADED |
| SMF / UPF | DEGRADED |
| OCS / CHF | DEGRADED |
| DNS | MAJOR ISSUE |
| GTP-U | PARTIAL LOSS |

---

# 🎯 8. Root Cause Hypothesis

## PRIMARY ROOT CAUSE:
👉 DNS inconsistency across core regions

This caused:
- Wrong PGW selection
- Incorrect UPF routing
- SMF service discovery failures

---

## SECONDARY EFFECTS:
- OCS overload due to retry storms
- PCRF/PCF policy delays
- GTP-U tunnel instability

---

# 🛠️ 9. Resolution Strategy

## Phase 1: Stabilization
- Flush DNS caches in core nodes
- Redirect traffic to healthy PGW clusters
- Force SMF → UPF re-registration

## Phase 2: Recovery
- Restart OCS load balancers
- Sync NRF service registry
- Re-align PCRF/PCF policies

## Phase 3: Validation
- Verify attach success rate
- Monitor GTP-U traffic restoration
- Check roaming normalization

---

# 📊 10. Post-Incident Analysis

## Key Learnings:

- DNS is a **critical control-plane dependency**
- Core network failures are usually **cascading, not isolated**
- Policy + charging + routing are tightly coupled
- 5G SBA increases dependency on service discovery (NRF)

---

# 🧠 PacketAlchemy Insight

> “In modern telecom networks, DNS is no longer just name resolution — it is the backbone of service routing.”

---

# 📌 Final Engineering Rule

> “A global outage is never a single fault. It is always a chain reaction across DNS, policy, charging, and session control.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | War Room Simulation | EPC | 5GC | Global Outage Analysis | Vendor-Level Troubleshooting
