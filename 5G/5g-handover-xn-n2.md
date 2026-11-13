# 5G Handover: Xn and N2 Procedures

## 📡 Overview

In 5G Standalone (SA), handover moves a connected UE from a source
gNB to a target gNB without dropping its PDU sessions. The user plane
is re-anchored, the QoS flows are re-established on the new radio leg,
and the AMF and SMF stay in the loop only as far as the procedure
needs them.

There are two handover families in 5GC. Xn-based handover runs
directly between the two gNBs over the Xn interface; the AMF is
involved only at the end, through the Path Switch. N2-based handover
runs through the AMF over N2, and is used when there is no Xn link
between the gNBs, when the target belongs to a different AMF, or when
the source gNB decides the core must coordinate the move.

In both cases the N3 tunnel moves from the source gNB to the
target gNB, the UPF anchor normally stays put, and in-flight
downlink data is forwarded so the application barely notices.

---

## 🧩 Key Functions

- **UE** — measures neighbour cells, reports them, and executes the
  handover command (RRC Reconfiguration with reconfigurationWithSync).
- **Source gNB** — decides the handover from measurement reports,
  prepares the target, forwards buffered data, and releases the UE
  context.
- **Target gNB** — admits the UE, allocates radio resources and a new
  N3 tunnel endpoint, and confirms it is ready to receive the UE.
- **AMF** — coordinates N2-based handover and, in both families,
  processes the Path Switch Request that moves the N2/N3 association
  to the target gNB.
- **SMF / UPF** — the SMF updates the N4 session so the UPF switches
  the downlink N3 tunnel to the target gNB; the UPF anchor normally
  does not change.
---

## 🔄 Procedure Step by Step

### Xn-based handover

1. **Measurement and decision** — the UE reports neighbour cells
   (A3 event); the source gNB selects a target gNB.
2. **Handover preparation** — the source gNB sends an XnAP Handover
   Request with the UE context, PDU sessions, and QoS flows. The
   target gNB admits what it can serve and replies with Handover
   Request Acknowledge, including the RRC reconfiguration for the UE
   and its N3 tunnel endpoint.
3. **Execution** — the source gNB sends the RRC Reconfiguration to
   the UE, forwards buffered downlink data to the target over Xn,
   and sends an SN Status Transfer with PDCP sequence numbers so no
   packet is duplicated or lost.
4. **Radio switch** — the UE synchronises to the target cell and
   sends RRC Reconfiguration Complete; uplink now flows via the target.
5. **Path Switch** — the target gNB sends an NGAP Path Switch
   Request to the AMF; the SMF updates the UPF over N4 (PFCP Session
   Modification) and downlink traffic moves to the target gNB.
6. **Cleanup** — the AMF replies Path Switch Request Acknowledge
   and the source gNB releases the UE context.

### N2-based handover

1. **Handover Required** — the source gNB sends an NGAP Handover
   Required to the AMF, naming the target gNB and the PDU sessions
   to move.
2. **Preparation via the core** — the AMF (and a target AMF, if
   inter-AMF) forwards a Handover Request to the target gNB. The SMF
   is consulted per PDU session; sessions that cannot move are listed
   as failed, and the target gNB allocates N3 endpoints for the rest.
3. **Handover Command** — the AMF returns the Handover Command; the
   source gNB forwards the RRC reconfiguration to the UE.
4. **Execution** — the UE attaches to the target cell. Indirect data
   forwarding runs through the UPF (N3 forwarding tunnel) because
   there is no Xn path for direct forwarding.
5. **Completion** — the target gNB sends Handover Notify; the AMF
   tells the SMF to switch the N3 tunnels, and the source gNB
   releases its context.

---

## ⚠️ Common Issues

- Handover preparation fails: the target gNB rejects the PDU
  sessions — check radio resources and the QoS profiles the target
  can support, not the core first.
- Path Switch fails after a successful radio switch: the AMF or SMF
  cannot switch the N3 tunnel — check N2/N4 reachability and the
  SMF's view of the PDU session, because the UE is already on the
  new cell while its downlink is still aimed at the old one.
- Data loss during the move: SN Status Transfer missing or the
  forwarding tunnel not established — buffered downlink packets are
  dropped instead of forwarded to the target gNB.
- Handover loops or ping-pong: measurement thresholds or handover
  margins are too aggressive, so the UE bounces between two cells —
  a radio parameter problem, not a core problem.
- Inter-AMF handover fails: the UE context transfer between AMFs
  breaks, often on subscription or slice data the target AMF cannot
  resolve — check N14 and the UDM data.

---

## 🔌 Related Interfaces

- Xn (gNB ↔ gNB) — XnAP Handover Request / Acknowledge, SN Status Transfer
- N2 (gNB ↔ AMF) — NGAP Handover Required / Command / Notify, Path Switch Request
- N3 (gNB ↔ UPF) — GTP-U user plane tunnel, switched to the target gNB
- N4 (SMF ↔ UPF) — PFCP Session Modification to re-aim the downlink tunnel
- N11 (AMF ↔ SMF) — PDU session context during handover
- N14 (AMF ↔ AMF) — UE context transfer in inter-AMF handover

---

## 🧠 PacketAlchemy Note

A handover is two moves at once: a radio move and a tunnel move.
When the radio move succeeds and the user still has no data, the
tunnel move is the suspect — check the Path Switch and the N4 update
before touching the radio parameters.

Xn handover is the fast path: keep the Xn links healthy and most
handovers never need the AMF at all.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | Handover | Xn | N2 | NGAP | Path Switch | gNB | AMF | SMF | UPF
