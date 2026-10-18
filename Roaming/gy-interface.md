# Gy Interface: Quota Lifecycle and What Happens to the Bearer

## 💳 Overview

The Gy interface (Diameter, between PGW and OCS) does more than approve or
reject a session. It runs a quota lifecycle: the OCS grants credit in slices,
the PGW reports usage, and when credit runs out the OCS tells the PGW exactly
what to do with the bearer.

This note covers the lifecycle itself. Message structure (CCR/CCA) and the
OCS side are covered in `Protocols/packet-analysis-gtp-diameter.md` and
`Charging/ocs-overview.md`.

---

## 🧩 Quota Building Blocks

- **Rating Group / Service-Identifier** — what the quota applies to (APN,
  service, flow).
- **MSCC (Multiple-Services-Credit-Control)** — one CCR can carry several
  quota requests, one per rating group.
- **GSU (Granted-Service-Unit)** — the slice the OCS grants: volume
  (CC-Total-Octets, CC-Input-Octets, CC-Output-Octets), time (CC-Time), or
  events (CC-Service-Specific-Units).
- **USU (Used-Service-Unit)** — what the PGW consumed from that grant.
- **RSU (Requested-Service-Unit)** — what the PGW asks for next.
- **Validity-Time** — how long the grant is valid. When it expires, the PGW
  must re-request even if quota remains.

---

## 🔄 Quota Lifecycle

### 1. Initial Grant
PGW sends CCR (Initial Request) with RSU. OCS answers CCA with GSU and
Validity-Time. The session starts.

### 2. Update Loop
As the subscriber consumes the grant, the PGW sends CCR (Update Request)
with USU and a new RSU. The OCS answers with a fresh GSU.

### 3. Re-Auth (RAR)
The OCS can force a re-authorization mid-session with Re-Auth-Request
(RAR) — for example after a tariff change or a policy update.

### 4. Termination
At session end the PGW sends CCR (Terminate Request) with the final USU,
so the OCS can settle the account.

---

## 🚨 Quota Exhaustion: Fate of the Bearer

When credit runs out, the OCS answers with FUI (Final-Unit-Indication).
The PGW (PCEF) must enforce it. Three outcomes:

### Terminate
- The bearer / PDN session is torn down.
- Typical for prepaid subscribers with no balance and no fallback.

### Redirect
- The user is redirected to a top-up / notification server (walled garden).
- Traffic to the redirect destination is allowed; everything else is blocked.

### Restrict Access
- The session stays up but with restricted service (for example only
  certain APNs or low QoS), per operator policy.

---

## 🌍 Gy in Roaming

- **Home-routed roaming:** the PGW is in the home network, so Gy works
  exactly as at home.
- **Local breakout (LBO):** the visited PGW serves traffic locally while
  charging stays anchored at the home OCS. The visited network must still
  report usage and enforce FUI, so Gy behavior in LBO is a common source
  of roaming charging disputes.
- Roaming OCS policies are often stricter: some APNs blocked, tighter
  quotas, shorter Validity-Time.

---

## ⚠️ Common Issues

- Quota exhausted mid-session but the bearer stays up (FUI not enforced).
- Validity-Time expiry treated as session end instead of re-request.
- USU/GSU mismatch across rating groups (wrong service charged).
- Diameter timeout on CCR Update → PGW behavior depends on
  Credit-Control-Failure-Handling (continue vs terminate).
- Redirect loop: top-up page itself consumes quota.

---

## 🔌 Related Interfaces

- Gy (PGW ↔ OCS)
- Gx (PGW ↔ PCRF)
- S6a (MME ↔ HSS)
- S9 (Home PCRF ↔ Visited PCRF, roaming policy)

---

## 🧠 PacketAlchemy Note

Gy is not a one-time approval check.

It is a **real-time credit loop** — grant, consume, report, re-grant — and
the bearer lives or dies by the OCS answer at each step.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | Charging | Roaming | LTE | EPC
