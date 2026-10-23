# LTE Handover — X2 and S1

## Overview

Handover moves a UE's radio and core connectivity from a source eNodeB to a target eNodeB without dropping the data <redacted> LTE supports two flavors: X2-based handover between neighboring eNodeBs, and S1-based handover (with MME and SGW involved) when X2 is not possible.

---

## X2-Based Handover

The fast, eNodeB-to-eNodeB path. Used when the source and target eNodeBs have a direct X2 interface.

1. UE sends a Measurement Report; the source eNodeB decides to hand over.
2. Source eNodeB sends **Handover Request** over X2 to the target eNodeB.
3. Target eNodeB reserves radio resources and replies **Handover Request Acknowledge**.
4. Source eNodeB forwards in-flight downlink data over X2 to the target (**data forwarding**), and tells the UE to reconfigure (**RRC Connection Reconfiguration**).
5. UE attaches to the target cell; the target eNodeB sends **Path Switch Request** to the MME over S1-MME.
6. MME asks the SGW to **Modify Bearer** (new target eNodeB TEID), and the user plane switches.

The MME and SGW learn about the move only at step 5 — the radio decision stays between the eNodeBs.

---

## S1-Based Handover

Used when no X2 interface exists, or when the MME/SGW must change. Heavier, more signaling, more failure surface.

1. Source eNodeB sends **Handover Required** to the MME over S1-MME.
2. The source MME sends **Forward Relocation Request** (GTP-C) to the target MME (if the MME changes).
3. MME sends **Handover Request** to the target eNodeB over S1-MME.
4. Target eNodeB replies **Handover Request Acknowledge**; the MME passes resources back via **Forward Relocation Response**.
5. UE reconfigures onto the target cell; **Handover Notify** confirms completion.
6. MME/SGW update bearers with **Modify Bearer** (new eNodeB F-TEIDs); indirect data forwarding tunnels are set up through the SGW.

---

## Key Differences

| Point | X2 handover | S1 handover |
|---|---|---|
| Trigger path | eNodeB → eNodeB (X2-AP) | eNodeB → MME (S1-AP) |
| MME involved | Only at path switch | Controls the whole procedure |
| MME/SGW change | No | Possible |
| Data forwarding | Direct over X2 | Indirect via SGW |
| Speed | Faster, less signaling | Slower, more signaling |

---

## Common Issues

- **Missing X2 neighbor relation** — eNodeBs never establish X2, so every handover falls back to S1 with extra delay.
- **Path Switch Request failure** — MME rejects because the target TAI is outside the UE's TAI list or the subscription forbids it.
- **Bearer modification timeout** — SGW does not answer Modify Bearer; user plane stalls while the radio moved fine.
- **Data forwarding gaps** — out-of-order or lost packets during the forwarding window, visible as TCP retransmissions right after handover.
- **Too-late handover** — RRC Reconfiguration arrives after radio link failure; the UE re-establishes instead of handing over.
- **Target admission rejection** — target eNodeB has no resources (Handover Preparation Failure over X2/S1).

---

## Related Interfaces

- X2 (X2-AP) — eNodeB to eNodeB handover signaling
- S1-MME (S1-AP) — eNodeB to MME, Handover Required / Request
- S1-U — eNodeB to SGW user plane, re-pointed after handover
- S10/S11 (GTP-C) — MME relocation and bearer modification

---

## 🧠 PacketAlchemy Note

X2 keeps the core out of the loop — fast when it works, invisible when it breaks.

When handover complaints land, check the type first: a radio problem before path switch is an X2/eNodeB problem; a stalled <redacted> after the UE moved is an S1/core problem.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | LTE | Mobility | Handover
