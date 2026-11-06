# UPF Selection by SMF in 5G Core

## 📡 Overview

During PDU session establishment, the **SMF (Session Management
Function)** must choose which **UPF (User Plane Function)** will
anchor the session — and sometimes which intermediate UPFs sit in
front of it. Unlike LTE, where the SGW/PGW choice is largely fixed
by APN and MME configuration, 5G selection is dynamic: the SMF
weighs the DNN, the slice (S-NSSAI), the UE's location, the UPF's
load and capabilities, and whether the traffic needs a local edge
breakout. A wrong choice does not fail the session — it silently
adds latency, overloads one node, or routes edge traffic across the
country.

---

## 🧩 Selection Inputs

- **DNN and S-NSSAI** — the primary filters. Each UPF profile
  declares the DNNs and slices it serves; a UPF that does not list
  the session's DNN/S-NSSAI is never a candidate.
- **UE location (TAI)** — the SMF uses the UE's Tracking Area
  Identity to prefer a UPF close to the UE, keeping the N3 path
  short and the latency low.
- **PDU session type** — IPv4, IPv6, IPv4v6, Ethernet, or
  unstructured. A UPF that does not support the requested type is
  excluded.
- **UPF capabilities** — support for UL Classifier (UL CL),
  Branching Point (BP), PDU Session Anchor (PSA), ATSSS steering,
  and buffering / downlink data notification.
- **Load and capacity** — the NRF profile carries load and capacity
  figures; the SMF spreads new sessions instead of piling them on
  one UPF.
- **DNAI (Data Network Access Identifier)** — for edge computing:
  the SMF picks a UPF that can reach the local data network
  identified by the DNAI, often a PSA deployed at the edge site.
- **Local policy** — operator-configured rules, e.g. a dedicated
  UPF for a corporate DNN or for roaming subscribers.

---

## 🔄 How Selection Works

1. **Discover** — the SMF queries the NRF
   (`Nnrf_NFDiscovery`) for UPF profiles matching the DNN,
   S-NSSAI, and optionally the TAI or DNAI.
2. **Rank candidates** — the SMF scores the returned profiles by
   proximity, load, capacity, and capability match, applying local
   policy on top.
3. **Associate over N4** — the SMF establishes a PFCP association
   with the chosen UPF if one does not exist, then sends a PFCP
   Session Establishment Request (PDRs, FARs, QERs, URRs).
4. **Anchor and intermediates** — the SMF picks a PSA UPF that
   holds the session's IP address, and inserts an I-UPF (with UL CL
   or BP) when traffic must be split — e.g. one branch to the
   central DN, one to a local edge DN.
5. **SSC mode decides re-selection** — SSC mode 1 keeps the same
   anchor for the session's life; mode 2 releases and re-establishes
   with a new anchor (new IP address); mode 3 makes a new anchor
   before releasing the old one.
6. **Re-select on change** — on UE mobility into a new area, or on
   UPF failure, the SMF may insert, remove, or relocate UPFs via
   PFCP Session Modification, updating the N3/N9 tunnels.

---

## ⚠️ Common Issues

- Anchor too far from the UE: the TAI was not passed or not used
  as a filter, so sessions land on a central UPF and latency grows.
- One UPF overloaded: load and capacity in the NRF profile are
  stale or ignored, and every new session picks the same instance.
- No candidate found: no UPF registered for that DNN/S-NSSAI
  combination — the session fails with cause "insufficient
  resources" even though UPFs are up.
- Edge traffic takes the long way: the DNAI was missing from the
  selection criteria, so the SMF never picked the local PSA.
- SSC mode 2 surprise: the anchor is relocated, the UE gets a new
  IP address, and long-lived connections break.
- N4 association missing: the SMF tries to create a session on a
  UPF it never associated with — PFCP requests time out.

---

## 🔌 Related Interfaces

- N4 (SMF ↔ UPF) — PFCP: association, session establishment,
  modification, deletion
- Nnrf (NRF) — NFDiscovery for UPF profiles
- N3 (gNB ↔ UPF) — GTP-U tunnel for user traffic
- N9 (UPF ↔ UPF) — GTP-U between intermediate and anchor UPFs
- N6 (UPF ↔ DN) — breakout toward the data network
- Nsmf (AMF ↔ SMF) — PDU session context, SSC mode, DNAI

---

## 🧠 PacketAlchemy Note

UPF selection is where control-plane decisions become user-plane
reality. A session that "works" can still be slow, unbalanced, or
unable to reach the edge — because the anchor was chosen by
default, not by design.

When latency is high or one UPF is saturated while others are idle,
check selection first: what did the NRF return, which filters were
applied, and does the chosen UPF actually match the UE's location
and the session's DNN?

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | SMF | UPF | N4 | PFCP | Edge | NRF
