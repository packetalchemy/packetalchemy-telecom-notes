# Diameter S6a: MME and HSS

## 🔌 Overview

S6a is the Diameter interface between MME and HSS. It carries
everything the MME needs to serve a subscriber: authentication
vectors, subscription data, location updates, and purge. If S6a
fails, attach fails — no matter how healthy the radio is.

---

## 🧩 Main Procedures

### AIR / AIA — Authentication-Information-Request / Answer
MME asks the HSS for authentication vectors (RAND, AUTN, XRES,
KASME). The HSS returns them; the MME runs EPS-AKA with the UE.

### ULR / ULA — Update-Location-Request / Answer
MME tells the HSS "this subscriber is now on me". The HSS returns
the subscription data: APN profiles, AMBR, QoS, roaming
restrictions, charging characteristics.

### CLR / CLA — Cancel-Location-Request / Answer
HSS tells the old MME to remove the subscriber (for example after
a move to a new MME).

### PUR / PUA — Purge-UE-Request / Answer
MME tells the HSS the subscriber is gone (detach, long inactivity).
The HSS marks the MME as unregistered.

### IDR / IDA — Insert-Subscriber-Data-Request / Answer
HSS pushes updated subscription data to the MME mid-session (for
example a changed APN profile or a barred service).

### DSR / DSA — Delete-Subscriber-Data-Request / Answer
HSS removes specific subscription data from the MME.

### NOR / NOA — Notify-Request / Answer
MME reports an event to the HSS (for example a terminal
capability change).

### RSR / RSA — Reset-Request / Answer
HSS restart recovery: the HSS tells MMEs it restarted, so MMEs
re-register affected subscribers.

---

## 🔍 What the Subscription Data Decides

From ULA / IDR the MME learns:

- Allowed APNs and the default APN.
- APN-AMBR and UE-AMBR.
- QoS profile per APN.
- Roaming allowed or barred, and per-APN roaming rules.
- Charging characteristics.
- Access restrictions (for example CS/PS barred).

Most "attach rejected" and "no data" cases trace back to one of
these fields.

---

## ⚠️ Common Issues

- ULR rejected: unknown subscriber, roaming not allowed, or
  HSS unreachable.
- Stale subscription data: IDR never delivered, MME works with
  old APN profile.
- Authentication failure: vectors exhausted or AUTN mismatch
  (sync failure — re-sync via AUTS).
- Purge storms: mass detach after an MME restart floods the HSS
  with PURs.
- CLR not processed: old MME keeps paging a subscriber that moved.

---

## 🔌 Related Interfaces

- S6a (MME ↔ HSS)
- S1-MME (eNodeB ↔ MME)
- S11 (MME ↔ SGW)
- S6d (SGSN ↔ HSS) — 2G/3G equivalent
- N8 (AMF ↔ UDM) — the 5G successor

---

## 🧠 PacketAlchemy Note

S6a is where the network decides who you are and what you are
allowed to do.

Radio problems are loud. Subscription problems are silent — the
attach just fails, or the data just never flows.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | Diameter | EPC | LTE
