# CHF: Converged Charging in 5G Core

## 💳 Overview

CHF (Charging Function) is the 5G replacement for the split
online/offline charging world. One function handles both: real-time
credit control and CDR generation. The SMF talks to the CHF over Nchf
(reference point N40).

If OCS was "online only", CHF is "converged": one charging path for
every session.

---

## 🧩 Key Concepts

- **CHF (Charging Function)** — rating, charging, CDR generation.
- **Nchf_ConvergedCharging** — the service the SMF consumes.
- **Converged charging** — online and offline charging through one
  function and one session, instead of Gy + Rf separately.
- **Rating group / service identifier** — what the charge applies to,
  same idea as in Gy.
- **Triggers** — events that make the SMF report usage or ask for
  more quota (time, volume, QoS change, RAT change, location change).

---

## 🔄 Nchf Operations

### Create
SMF opens a charging session when the PDU session starts. The CHF
answers with granted quota (if online charging applies) and the
charging parameters.

### Update
During the session the SMF reports usage and requests more quota.
Triggers (time, volume, QoS/RAT/location change) drive updates.

### Release
At PDU session end the SMF reports final usage. The CHF closes the
session and finalizes the CDR.

---

## 🔍 CHF vs OCS (Gy)

| | OCS / Gy (EPC) | CHF / Nchf (5GC) |
|---|---|---|
| Scope | Online only | Online + offline converged |
| Protocol | Diameter | HTTP/2 service-based (Nchf) |
| Offline path | Separate Rf | Same function |
| Session | Diameter session | Nchf charging session |

The quota ideas (grant, consume, report, re-grant) carry over from
Gy. What changes is the transport and the convergence.

---

## ⚠️ Common Issues

- Triggers misconfigured: SMF never reports usage, so the CHF
  charges on stale data.
- Quota exhausted but the session continues (same failure mode as
  Gy FUI not enforced — now on the Nchf side).
- CDR gaps: Release never sent because the PDU session ended
  abnormally.
- Rating group mismatch between SMF and CHF (wrong service charged).
- Offline-only subscribers routed through online-charging logic
  (or the reverse).

---

## 🔌 Related Interfaces

- Nchf / N40 (SMF ↔ CHF)
- N7 (SMF ↔ PCF) — policy that interacts with charging
- N4 (SMF ↔ UPF) — enforcement point
- Gy (PGW ↔ OCS) — the EPC predecessor

---

## 🧠 PacketAlchemy Note

CHF is not "OCS on HTTP/2".

It is the end of the online/offline split — one charging function,
one session, one record. The quota logic is familiar; the
architecture is what changed.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | 5G Core | Charging | CHF
