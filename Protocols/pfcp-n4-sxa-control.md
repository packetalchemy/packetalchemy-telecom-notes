# PFCP: How the Control Plane Drives the User Plane

## 🔌 Overview

PFCP (Packet Forwarding Control Protocol) is the protocol that lets the
control plane program the user plane. In 5G Core it runs on N4 between
SMF and UPF. In EPC CUPS it runs on Sx between the control and user
plane parts of SGW/PGW (SGW-C/SGW-U, PGW-C/PGW-U).

One idea: the SMF thinks in rules, the UPF enforces them.

---

## 🧩 PFCP Session Rules

A PFCP session carries a set of rules. The UPF installs and enforces
them without per-packet signalling.

- **PDR (Packet Detection Rule)** — identifies traffic: match on
  F-TEID, SDF filter, source interface, UE IP.
- **FAR (Forwarding Action Rule)** — what to do with matched packets:
  forward, drop, buffer, duplicate.
- **QER (QoS Enforcement Rule)** — bitrate, gating, marking (DSCP).
- **URR (Usage Reporting Rule)** — measure usage for charging.
- **BAR (Buffering Action Rule)** — buffering behavior (for example
  during paging).
- **F-TEID (Fully Qualified TEID)** — the tunnel endpoint the UPF uses
  to forward GTP-U packets.

---

## 🔄 PFCP Session Lifecycle

### 1. Session Establishment
SMF sends PFCP Session Establishment Request with all rules. The UPF
allocates resources and answers with its F-TEID.

### 2. Session Modification
SMF updates rules (new PDR/FAR, changed QoS, new URR) as the session
evolves — handover, new QoS flow, charging event.

### 3. Session Deletion
SMF tears the session down. The UPF releases rules and reports final
usage (URR).

### Node-Level Messages
- **Heartbeat** — node liveness between SMF and UPF.
- **Node Report** — the UPF reports events (for example user-plane
  path failure).
- **Association Setup/Release** — the SMF–UPF relationship itself.

---

## 🔍 PFCP vs GTP-C

- GTP-C (S11, S5/S8, N11) manages bearers and sessions between
  control-plane nodes.
- PFCP manages the **forwarding behavior** inside the user-plane node.
- In CUPS/5GC the split is explicit: GTP-C talks between control
  nodes, PFCP programs the UPF.

---

## ⚠️ Common Issues

- Session exists in the SMF but traffic is dropped: the UPF never
  installed the PDR/FAR, or the F-TEID in the PDR is wrong.
- Silent drops after handover: stale FAR still points at the old
  path.
- QoS not enforced: QER missing or mismatched with the 5QI/QCI.
- Charging gaps: URR not installed, or usage reports lost on
  session deletion.
- Heartbeat loss: SMF marks the UPF down and re-selects, tearing
  sessions that were still healthy.

---

## 🔌 Related Interfaces

- N4 (SMF ↔ UPF) — 5G Core
- Sx (SGW-C ↔ SGW-U, PGW-C ↔ PGW-U) — EPC CUPS
- N11 (AMF ↔ SMF), S11 (MME ↔ SGW-C)

---

## 🧠 PacketAlchemy Note

Most PFCP outages are not protocol failures.

They are a **missing or mismatched rule** — the SMF thinks the UPF
enforces something the UPF never received.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | 5G Core | PFCP | UPF | EPC
