# EPC ↔ 5GC Interworking (LTE / 5G Integration)

## 🧠 Overview

Interworking between EPC (LTE Core) and 5G Core (5GC) is essential for:
- Gradual migration from LTE to 5G
- Non-Standalone (NSA) operation
- Roaming continuity between LTE and 5G
- Service fallback scenarios

---

# 📡 1. NSA vs SA Concept

## 📶 NSA (Non-Standalone)

- LTE (EPC) is still anchor
- 5G NR is added for data boost
- EPC handles control plane

👉 EPC + 5G radio combined

---

## 📶 SA (Standalone)

- Full 5G Core (5GC)
- No EPC dependency
- AMF/SMF/UPF handle everything

---

# 🏗️ 2. Architecture Comparison

| Feature | EPC (LTE) | 5GC |
|--------|----------|-----|
| Control | MME | AMF |
| Session | SGW/PGW | SMF + UPF |
| Policy | PCRF | PCF |
| Auth | HSS | UDM |
| Signaling | Diameter | HTTP/2 SBA |

---

# 🔄 3. NSA Interworking Flow

```
UE
 ↓
eNB (LTE anchor)
 ↓
MME (EPC control)
 ↓
SGW → PGW
 ↓
Internet

AND

UE
 ↓
gNB (5G NR)
 ↓
Data boost via EPC anchor
```

---

# 🌐 4. EPC ↔ 5GC Mobility

## 📌 Scenario: LTE → 5G handover

### Step 1:
UE connected to EPC

### Step 2:
5G coverage detected

### Step 3:
Dual connectivity established

### Step 4:
Traffic partially offloaded to 5G

---

# 🔗 5. Interworking Functions

## 📡 N26 Interface (Key Element)

Connects:
- MME (EPC)
- AMF (5GC)

Used for:
- Mobility context transfer
- Session continuity

---

# ⚠️ 6. Common Interworking Issues

## ❌ 1. Handover Failure LTE → 5G

Causes:
- N26 interface down
- Context mismatch
- Authentication failure (UDM/HSS sync issue)

---

## ❌ 2. Data Break During Transition

Causes:
- GTP tunnel re-establishment failure
- SMF/PGW mismatch
- DNS inconsistency between EPC and 5GC

---

## ❌ 3. IP Change Problem

- EPC uses PGW-assigned IP
- 5GC uses UPF-based IP

👉 IP continuity may break without proper anchoring

---

# 💳 7. Policy Continuity (PCRF ↔ PCF)

## EPC:
- PCRF controls policy

## 5GC:
- PCF controls policy

### Problem:
- Policy mismatch during migration

👉 Result:
- Speed drop
- Session reset
- Access restriction

---

# 🌍 8. DNS in Interworking

DNS must support:
- EPC APN resolution
- 5G DNN resolution
- Dual routing logic

Failure leads to:
- Wrong core selection
- Session drop
- No service in hybrid coverage

---

# 📡 9. User Plane Continuity

## EPC:
- GTP-U tunnels (SGW ↔ PGW)

## 5GC:
- N3 interface (gNB ↔ UPF)

### Challenge:
- Seamless tunnel transition is difficult
- Often causes packet loss during handover

---

# 🧠 10. Engineering Insight

Interworking is NOT simple coexistence.

It is:

> “Two different core architectures trying to behave like one network”

---

# 🔥 11. Troubleshooting Flow

When issues happen in interworking:

1. Check UE mode (NSA or SA)
2. Check MME ↔ AMF (N26)
3. Check session anchoring (PGW / UPF)
4. Check policy alignment (PCRF vs PCF)
5. Check DNS consistency
6. Check user plane continuity (GTP-U vs N3)

---

# 📌 Final Rule

> “Most interworking problems are caused by mismatch between EPC anchoring and 5GC session continuity.”

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | EPC ↔ 5GC | NSA/SA | Interworking | Core Network Evolution
