# LTE Roaming Fundamentals

## 🌍 Overview

LTE roaming lets a subscriber attach to a visited network (VPLMN) while
their subscription, charging and policy stay anchored at the home network
(HPLMN). Two architectures decide where user traffic goes and which nodes
talk to each other.

---

## 🧩 Key Nodes and Interfaces

- **HSS (Home)** — subscription data, authentication vectors.
- **MME (Visited)** — mobility management in the visited network.
- **SGW (Visited)** — serving gateway in the visited network.
- **PGW (Home or Visited)** — depends on the architecture below.
- **S6a (MME ↔ HSS)** — authentication, location update, subscriber data.
- **S8 (Visited SGW ↔ Home PGW)** — user plane in home-routed roaming.
- **S9 (Home PCRF ↔ Visited PCRF)** — policy exchange in roaming.
- **S10 (MME ↔ MME)** — MME relocation during handover.

---

## 🔄 Architecture 1: Home-Routed Roaming

- The PGW stays in the **home** network.
- User traffic tunnels from the visited SGW to the home PGW over S8.
- Charging (Gy), policy (Gx) and APN handling work exactly as at home.
- Trade-off: higher latency (traffic trombones through home) and home
  network carries the load, but full control and simple charging.

---

## 🔄 Architecture 2: Local Breakout (LBO)

- The PGW is in the **visited** network.
- User traffic breaks out locally; the home network keeps control via S6a
  and S9 only.
- Trade-off: lower latency and less home load, but charging and policy
  enforcement split across two operators, which is where most roaming
  disputes come from.

---

## 🔑 Attach in Roaming

1. UE attaches to the visited MME.
2. Visited MME asks the home HSS over S6a: authentication vectors,
   subscription data, roaming restrictions.
3. HSS checks whether roaming is allowed for this subscriber and APN.
4. Default bearer is created: home-routed via S8, or LBO via the
   visited PGW.
5. Policy is exchanged over S9 between home and visited PCRF.

---

## 🧭 Steering of Roaming

Operators influence which visited network a subscriber picks:

- **Preferred PLMN lists** on the SIM.
- **Steering of Roaming (SoR)** — the home network can push an updated
  preferred list during attach (via S6a) or by other means.
- Steering is a commercial decision with technical side effects: a
  steered subscriber may land on a partner with weaker coverage or
  different LBO support.

---

## ⚠️ Common Issues

- Roaming not allowed in HSS subscription (attach rejected).
- APN blocked for roaming in subscription data.
- LBO charging disputes: usage reported by visited PGW, settled by
  home OCS.
- S9 policy mismatch: home and visited PCRF disagree on QoS or
  gating.
- Steering loops: subscriber bounced between two visited networks.
- Data roaming disabled on the device (most common "no data abroad"
  case — check the device first).

---

## 🔌 Related Notes

- `Roaming/lte-roaming-steering.md` — steering in detail.
- `Roaming/gy-interface.md` — Gy quota lifecycle and roaming charging.
- `Protocols/diameter-s6a-mme-hss.md` — S6a messages.

---

## 🧠 PacketAlchemy Note

Roaming is not "attach to a foreign network".

It is **split control**: the visited network moves the packets, the home
network owns the subscriber, the policy and the bill. Most roaming
failures live in that split.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | Roaming | LTE | EPC
