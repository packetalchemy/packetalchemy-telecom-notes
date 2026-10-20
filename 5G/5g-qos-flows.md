# 5G QoS Flows: From 5QI to Enforcement

## 📡 Overview

5G Core replaces LTE's bearer-per-QCI model with **QoS flows**. A QoS
flow is the finest granularity of QoS differentiation inside a PDU
session. Every user-plane packet in the session is classified into a
QoS flow identified by a **QFI (QoS Flow Identifier)**, and each flow
carries a **5QI (5G QoS Identifier)** that defines its treatment.

One idea: the bearer is dead in 5GC. The QoS flow is its successor —
lighter, per-packet, and negotiated end to end.

---

## 🧩 Building Blocks

- **QFI** — 0..63, marks every packet of the flow (GTP-U extension
  header on N3/N9, SDAP on the radio). QFI 0..? values 0-63; flow
  created by the SMF.
- **5QI** — defines packet delay budget, packet error rate, priority
  level, and whether the flow is GBR, Delay-Critical GBR, or non-GBR.
- **ARP (Allocation and Retention Priority)** — priority for admission
  control: can this flow pre-empt others, and can it be pre-empted?
- **AMBR (Aggregate Maximum Bit Rate)** — per-UE (UE-AMBR, via AMF)
  and per-session (Session-AMBR, via SMF). Applies to all non-GBR
  flows together.
- **GBR / MBR** — per-GBR-flow guaranteed and maximum bit rates.
- **RQA (Reflective QoS Attribute)** — lets the UE derive uplink QoS
  from downlink packets, no signalling needed.
- **Default QoS flow** — every PDU session has exactly one; it carries
  everything not matched to a dedicated flow. Non-GBR, survives for
  the life of the session.

---

## 🔄 How a QoS Flow Is Born

1. **PCF decides** — via N7, the PCF sends PCC rules to the SMF:
   service data flow filters, 5QI, ARP, GBR/MBR if needed.
2. **SMF creates the flow** — assigns a QFI, builds the QoS profile,
   and programs the UPF over N4 with a QER (QoS Enforcement Rule).
3. **SMF notifies the AMF** — over N11 with N2 SM information.
4. **AMF signals the RAN** — N2 PDU Session Resource Setup/Modify
   Request carries the QoS profiles; the RAN maps flows to DRBs
   (Data Radio Bearers) — several flows can share one DRB.
5. **UE gets the rules** — NAS signalling tells the UE the QFI, QoS
   rules, and mapping for uplink classification (SDAP marks UL).

---

## 📊 Key 5QI Values

- **1** — GBR, conversational voice (100 ms, 10⁻²)
- **2** — GBR, conversational video (150 ms)
- **3** — GBR, real-time gaming / V2X (50 ms)
- **4** — GBR, buffered streaming (300 ms)
- **65/66** — Mission-critical push-to-talk / non-GBR signalling
- **5** — non-GBR, IMS signalling (100 ms)
- **6** — non-GBR, voice/video interactive gaming (300 ms)
- **7** — non-GBR, interactive gaming (100 ms)
- **8/9** — non-GBR, buffered video / TCP best effort (300 ms)
- **80–86** — Delay-Critical GBR (low-latency eMBB/URLLC)

Non-standardized 5QIs (128–254) let operators define their own
characteristics; the 5QI is then just an index into a local table.

---

## 🔁 LTE Interworking

On handover to EPC, each QoS flow maps to an **EPS bearer** with an
equivalent QCI (5QI 1 → QCI 1, 5QI 9 → QCI 9, etc.). The mapping is
defined in 3GPP TS 23.501/23.502. GBR flows become GBR bearers;
non-GBR flows collapse toward the default bearer. Mismatched
mapping tables are a classic source of voice-quality drops at
5G→4G handover.

---

## ⚠️ Common Issues

- Voice sounds fine on 5G but degrades after handover to LTE: the
  5QI→QCI mapping table differs between SMF and MME/PGW.
- GBR flow admitted but throughput capped: MBR set too low, or the
  RAN mapped the GBR flow onto a DRB shared with best-effort traffic.
- Everything works except one app: the PCC rule's SDF filter never
  matches, so traffic falls into the default QoS flow (5QI 9).
- Congestion kills signalling: IMS signalling flow (5QI 5) shares
  resources with data because ARP was copied from the default flow.
- Session-AMBR ignored: UPF QER not installed over N4 — the SMF
  decided, the UPF never enforced.

---

## 🔌 Related Interfaces

- N7 (SMF ↔ PCF) — PCC rules with QoS parameters
- N11 (AMF ↔ SMF), N2 (AMF ↔ RAN) — QoS profile delivery
- N4 (SMF ↔ UPF) — QER enforcement
- N1 (UE ↔ AMF) — NAS QoS rules to the UE
- S5/S8 + Gx — the LTE side of interworking

---

## 🧠 PacketAlchemy Note

QoS bugs rarely live in the 5QI value itself.

They live in the **chain** — the PCF's rule, the SMF's QFI, the N4
QER, the RAN's DRB mapping. One broken link and the 5QI on paper
means nothing on the air.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | QoS | 5QI | QFI | SMF | PCF

