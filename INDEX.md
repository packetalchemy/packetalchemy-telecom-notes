# PacketAlchemy Notes Index

Every note in this repo, by folder, with tags and related notes.
Use this page to find a topic, or to check a topic is not already
covered before writing a new note.

Tags: `#EPC` `#5GC` `#Charging` `#Policy` `#Diameter` `#GTP` `#PFCP`
`#DNS` `#Roaming` `#Troubleshooting` `#Wireshark` `#Lab` `#Interview`
`#Architecture` `#5G` `#LTE`

---

## 5G
- `5G/5g-core-evolution.md` — 5G Core evolution from EPC. `#5GC` `#EPC`
- `5G/epc-5gc-interworking.md` — EPC ↔ 5GC interworking. `#5GC` `#EPC`
- `5G/network-slicing.md` — Network slicing (eMBB / URLLC / mMTC). `#5GC` `#Architecture`

## Architecture
- `Architecture/production-hardening-99999.md` — Production hardening. `#Architecture`

## Blueprint
- `Blueprint/full-operator-design-book.md` — Full operator design. `#Architecture`

## CaseStudies
- `CaseStudies/no-internet-ip-assigned.md` — No internet despite IP assigned. `#Troubleshooting` `#EPC`
- `CaseStudies/real-epc-incidents.md` — Real EPC incidents. `#Troubleshooting` `#EPC`

## Charging
- `Charging/chf-5g-charging.md` — CHF converged charging in 5G. `#5GC` `#Charging`
- `Charging/ocs-overview.md` — OCS online charging in LTE. `#EPC` `#Charging`
- `Charging/pcrf-gx-policy-control.md` — PCRF / Gx policy control. `#EPC` `#Policy`

## Course
- `Course/README.md` — Course overview. `#Course`
- `Course/learning-paths.md` — Ordered learning paths across the repo. `#Course`

## Diagrams
- `Diagrams/01-epc-architecture.md` — EPC architecture diagram. `#EPC`
- `Diagrams/02-lte-attach-flow.md` — LTE attach flow diagram. `#LTE`
- `Diagrams/03-gtp-u-tunnel.md` — GTP-U tunnel diagram. `#GTP`
- `Diagrams/04-5g-core-architecture.md` — 5G Core architecture diagram. `#5GC`
- `Diagrams/05-5g-pdu-session-flow.md` — 5G PDU session flow diagram. `#5GC`
- `Diagrams/06-dns-routing-flow.md` — DNS routing flow diagram. `#DNS`
- `Diagrams/07-war-room-overview.md` — War room overview diagram. `#Troubleshooting`
- `Diagrams/lte-end-to-end-flow.md` — LTE end-to-end flow. `#LTE` `#EPC`

## DNS
- `DNS/dns-in-epc.md` — DNS in EPC (APN resolution, NAPTR/SRV). `#DNS` `#EPC`

## Interview
- `Interview/core-network-interview-pack.md` — Core network interview pack. `#Interview`
- `Interview/core-network-interview-qa.md` — Core network interview Q&A. `#Interview`

## Lab
- `Lab/deep-pcap-step-by-step-analysis.md` — Deep PCAP analysis, step by step. `#Lab` `#Wireshark`
- `Lab/epc-virtual-lab.md` — Virtual EPC lab. `#Lab`
- `Lab/pcap-real-case-analysis.md` — Real-case PCAP analysis. `#Lab` `#Wireshark`
- `Lab/pcap-simulation-lab.md` — PCAP simulation lab. `#Lab` `#Wireshark`
- `Lab/scenario-attach-no-data.md` — Hands-on scenario: attach OK, no data. `#Lab` `#Troubleshooting`
- `Lab/wireshark-core-analysis.md` — Wireshark core analysis. `#Lab` `#Wireshark`

## LTE
- `LTE/lte-attach-flow.md` — LTE attach flow with failure points per step. `#LTE` `#EPC`
- `LTE/lte-attach-procedure.md` — LTE attach procedure (detailed). `#LTE` `#EPC`

## Operator
- `Operator/virtual-operator-design.md` — Virtual operator design. `#Architecture`

## Protocols
- `Protocols/diameter-s6a-mme-hss.md` — Diameter S6a: MME and HSS. `#Diameter` `#EPC`
- `Protocols/gtp-c-deep-dive.md` — GTP-C deep dive. `#GTP` `#EPC`
- `Protocols/gtp-u-deep-dive.md` — GTP-U deep dive. `#GTP`
- `Protocols/packet-analysis-gtp-diameter.md` — Packet analysis: GTP-C and Diameter. `#GTP` `#Diameter` `#Wireshark`
- `Protocols/pfcp-n4-sxa-control.md` — PFCP: control plane drives user plane. `#PFCP` `#5GC`
- `Protocols/real-packet-log-analysis.md` — Real packet log analysis. `#Wireshark` `#Troubleshooting`

## Roaming
- `Roaming/gy-interface.md` — Gy quota lifecycle and bearer fate. `#Charging` `#Roaming`
- `Roaming/lte-roaming-steering.md` — LTE roaming steering. `#Roaming`
- `Roaming/lte-roaming.md` — LTE roaming fundamentals. `#Roaming` `#EPC`

## Tools
- `Tools/wireshark-lte-analysis.md` — Wireshark LTE analysis. `#Wireshark` `#LTE`

## Troubleshooting
- `Troubleshooting/epc-end-to-end-playbook.md` — EPC end-to-end playbook. `#Troubleshooting` `#EPC`
- `Troubleshooting/incident-simulation-no-internet.md` — Incident simulation: no internet. `#Troubleshooting`

## Vendor
- `Vendor/design-deep-dive-epc-5gc.md` — Vendor design deep dive, EPC/5GC. `#Architecture`

## WarRoom
- `WarRoom/global-core-outage-simulation.md` — Global core outage simulation. `#Troubleshooting`
- `WarRoom/multi-domain-outage-simulation.md` — Multi-domain outage simulation. `#Troubleshooting`

## Wireshark
- `Wireshark/5g-advanced-pcap-analysis.md` — 5G advanced PCAP analysis. `#5GC` `#Wireshark`
- `Wireshark/full-5g-core-call-flow.md` — Full 5G Core call flow. `#5GC` `#Wireshark`
- `Wireshark/full-epc-end-to-end-call-flow.md` — Full EPC end-to-end call flow. `#EPC` `#Wireshark`
- `Wireshark/gtp-deep-dive-attach.md` — GTP deep dive: attach. `#GTP` `#Wireshark`
- `Wireshark/wireshark-training-series.md` — Wireshark training series. `#Wireshark`

## Articles (site-ready, long-form)
- `Articles/pfcp-n4-smf-controls-upf.md` — PFCP on N4: how the SMF really controls the UPF. `#PFCP` `#5GC`

## Persian versions (FA)
- `FA/pfcp-n4.md` — PFCP روی N4 (نسخه فارسی). `#PFCP` `#FA`
- `FA/gy-interface.md` — واسط Gy: چرخه عمر کوتا و سرنوشت bearer (نسخه فارسی). `#Charging` `#FA`

---

## Recently added
- 2026-10-18: PFCP note, CHF note, S6a note, Gy + roaming fills, attach-flow expansion, learning paths, index, first site article, first Persian versions, first lab scenario.

---

## How to use this index
- **Find a topic:** search this page for a keyword or tag.
- **Before writing:** confirm the topic is not already listed here.
- **Related notes:** each note's "Related" section links sideways; this
  index is the master map.
