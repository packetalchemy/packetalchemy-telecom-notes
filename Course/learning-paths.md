# Learning Paths

Ordered paths through this repo. Start at the top of a path and read
down. Each step assumes the ones above it.

## Path 1 — EPC Foundations

1. `LTE/lte-attach-flow.md` — attach flow with failure points.
2. `LTE/lte-attach-procedure.md` — attach procedure in detail.
3. `Protocols/diameter-s6a-mme-hss.md` — S6a: MME and HSS.
4. `DNS/dns-in-epc.md` — DNS in EPC, APN resolution.
5. `Protocols/gtp-c-deep-dive.md` — GTP-C.
6. `Protocols/gtp-u-deep-dive.md` — GTP-U.
7. `Charging/ocs-overview.md` — OCS online charging.
8. `Roaming/gy-interface.md` — Gy quota lifecycle and bearer fate.
9. `Charging/pcrf-gx-policy-control.md` — PCRF / Gx policy.
10. `Protocols/pfcp-n4-sxa-control.md` — PFCP (CUPS side: Sx).
11. `Troubleshooting/epc-end-to-end-playbook.md` — put it together.

## Path 2 — 5G Core

1. `5G/5g-core-evolution.md` — from EPC to 5GC.
2. `5G/network-slicing.md` — slicing.
3. `5G/epc-5gc-interworking.md` — interworking.
4. `Protocols/pfcp-n4-sxa-control.md` — PFCP on N4.
5. `Charging/chf-5g-charging.md` — CHF converged charging.
6. `Diagrams/04-5g-core-architecture.md` — architecture diagram.
7. `Diagrams/05-5g-pdu-session-flow.md` — PDU session flow.
8. `Wireshark/full-5g-core-call-flow.md` — full call flow.
9. `Wireshark/5g-advanced-pcap-analysis.md` — advanced PCAP.

## Path 3 — Troubleshooting & War Room

1. `Protocols/packet-analysis-gtp-diameter.md` — packet analysis.
2. `Lab/wireshark-core-analysis.md` — Wireshark core analysis.
3. `Lab/deep-pcap-step-by-step-analysis.md` — deep PCAP, step by step.
4. `Lab/scenario-attach-no-data.md` — hands-on scenario.
5. `Troubleshooting/incident-simulation-no-internet.md` — incident simulation.
6. `CaseStudies/real-epc-incidents.md` — real incidents.
7. `WarRoom/global-core-outage-simulation.md` — global outage.
8. `WarRoom/multi-domain-outage-simulation.md` — multi-domain outage.

## Path 4 — Roaming

1. `Roaming/lte-roaming.md` — roaming fundamentals.
2. `Roaming/lte-roaming-steering.md` — steering.
3. `Roaming/gy-interface.md` — Gy and roaming charging.
4. `Protocols/diameter-s6a-mme-hss.md` — S6a in roaming attach.

## Path 5 — Interview Prep

1. Finish Path 1 (EPC Foundations).
2. `Interview/core-network-interview-qa.md` — Q&A.
3. `Interview/core-network-interview-pack.md` — interview pack.
4. `CaseStudies/real-epc-incidents.md` — have stories ready.
5. `WarRoom/global-core-outage-simulation.md` — talk through an outage.

---

## How to use the paths

- New to packet core: Path 1, then Path 2.
- Stuck on a live problem: Path 3, or jump to the note that matches
  your symptom via `INDEX.md`.
- Preparing for an interview: Path 5.
