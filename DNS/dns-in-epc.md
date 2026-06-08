# DNS in EPC (Evolved Packet Core)

## 🌐 Overview

In LTE/EPC networks, DNS is a critical component used for:
- APN resolution
- PGW selection
- Roaming routing
- Node discovery

DNS in telecom is NOT just internet DNS — it is part of core network control plane behavior.

---

## 🧩 Where DNS is Used in EPC

### 1. APN Resolution
UE requests an APN (e.g. internet.operator.com)

DNS helps locate:
- PGW FQDN
- Correct gateway for subscriber

---

### 2. PGW Selection
Based on:
- APN name
- PLMN (home or roaming)
- Subscriber profile

DNS returns:
- Home PGW
- Visited PGW
- Local Breakout PGW

---

### 3. Roaming Scenarios
DNS is used heavily in roaming for:
- VPLMN routing
- S8 Home Routed traffic
- Local Breakout decision

---

## 🔌 Key DNS Concepts in Telecom

### 🧭 FQDN (Fully Qualified Domain Name)

Example:
- pgw1.epc.operator.com
- pgw-roaming.visited.net

FQDN maps to:
- IP address of PGW / network node

---

### 📡 APN Based DNS Query

Example:
```
internet.mncXXX.mccYYY.gprs
```

DNS resolves this to:
- Correct PGW IP

---

## 🔄 DNS Flow in EPC

### 1. UE Requests APN
UE → MME → SGW → PGW selection trigger

### 2. DNS Query
PGW or control function queries DNS:
- Which PGW should be used?

### 3. DNS Response
Returns:
- IP address of selected PGW

### 4. Session Established
GTP-C session created toward resolved PGW

---

## 🌍 Roaming Impact on DNS

In roaming:

- Home DNS vs Visited DNS conflict
- APN rewritten by visited network
- Local breakout selection depends on DNS
- Incorrect DNS → wrong PGW selection

---

## ⚠️ Real Network Issues

### Case 1: No PGW Found

- DNS query fails
- Wrong APN format
- Missing FQDN record

➡️ Result:
- Attach succeeds
- No data session

---

### Case 2: Wrong PGW Selection

- DNS returns wrong node
- Traffic routed to wrong region

➡️ Result:
- High latency
- Service degradation

---

### Case 3: Roaming DNS Mismatch

- Visited network overrides DNS
- Home routing broken

➡️ Result:
- PDP context failure
- APN reject

---

## 🔌 DNS in LTE vs Internet DNS

| LTE EPC DNS | Internet DNS |
|-------------|-------------|
| PGW selection | Website resolution |
| APN routing | Domain resolution |
| Core control | User application |

---

## 🧠 PacketAlchemy Insight

DNS in EPC is not about "name resolution".

It is a **traffic steering mechanism for mobile core networks**.

---

## 🔧 Troubleshooting Checklist

- Check APN format
- Verify FQDN records
- Validate DNS in visited network
- Check roaming DNS overrides
- Verify PGW IP mapping
- Confirm diameter + DNS consistency

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | LTE | DNS | EPC Routing
