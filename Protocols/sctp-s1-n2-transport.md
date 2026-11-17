# SCTP in Telecom: The Transport Behind S1-MME and N2

## 🔌 Overview

SCTP (Stream Control Transmission Protocol, RFC 4960) is the
transport protocol that carries the control plane between the
radio network and the core: S1AP on S1-MME (eNodeB ↔ MME, SCTP
port 36412), NGAP on N2 (gNB ↔ AMF, port 38412), and X2AP on
X2-C (eNodeB ↔ eNodeB, port 36422).

Neither TCP nor UDP fits this job. TCP has one ordered byte
stream, so a single delayed message blocks everything behind
it (head-of-line blocking), and it binds to one IP address —
if that path dies, the connection dies. UDP has no reliability,
no ordering, and no failure detection at all. SCTP was designed
for exactly this gap: reliable, message-oriented delivery with
multiple independent streams and multiple IP addresses per
endpoint.

One fact shapes every outage that involves it: there is a
single SCTP association between one eNodeB and one MME (and
one between a gNB and an AMF). If that association goes down,
the whole eNodeB — every UE on it — is cut off from the core,
even though the radio and the user plane are perfectly healthy.

---

## 🧩 How an Association Is Built

### The Four-Way Handshake

- **INIT** — the initiator offers its addresses and parameters.
- **INIT ACK** — the peer answers with a **State Cookie** that
  holds all the association state, instead of allocating
  resources immediately.
- **COOKIE ECHO** — the initiator returns the cookie.
- **COOKIE ACK** — the association is established.

The cookie is the key design point: the server commits no
memory until the client proves it is real, so a flood of INITs
cannot exhaust the MME the way a SYN flood exhausts a TCP
server.

### Multihoming

Each endpoint can advertise several IP addresses. One path is
primary; the others are backups. If the primary path fails,
traffic moves to a secondary address without tearing the
association down. This is why an MME or AMF is usually reached
on two or more addresses — and why a second address that is
unreachable in practice is worse than none at all, because the
failover silently fails.

### Heartbeat and Path Monitoring

SCTP sends periodic HEARTBEAT chunks on idle paths and expects
HEARTBEAT ACKs. A path that stops answering is marked inactive,
and retransmissions move to another path. This is how the core
notices a dead link before any subscriber does.

### Multi-Streaming

An association carries many independent streams. A delayed
message on one stream does not block the others. S1AP and NGAP
use this deliberately: stream 0 carries non-UE-associated
procedures (setup, reset, overload), while UE-associated
signalling runs on separate streams — so one stuck UE procedure
cannot freeze setup or paging for the rest of the node.

---

## 🔍 Why It Matters to the Core

- **All signalling, one pipe.** Attach, handover preparation,
  paging, bearer setup — everything on S1-MME / N2 rides one
  association. Its health is the node's health.
- **Fast, silent failure detection.** Heartbeats catch a dead
  path in seconds; TCP would only notice when an application
  message times out.
- **Ordered delivery per stream.** S1AP and NGAP messages arrive
  in order within a stream, so the core never sees a handover
  command before the request that triggered it.
- **Message boundaries preserved.** Each S1AP / NGAP message is
  one SCTP message — no stream reassembly, no guessing where a
  message ends.

---

## ⚠️ Common Issues

- **Association never establishes:** a firewall or router drops
  SCTP (IP protocol 132) while TCP passes — INIT never arrives,
  and the eNodeB looks "unreachable" although ping works.
- **One path down, no failover:** the secondary address is
  advertised but not routable, so SCTP keeps retransmitting
  onto a dead path instead of switching.
- **Heartbeat mis-tuning:** too aggressive and a congested path
  flaps; too lax and a dead path is only noticed after
  subscribers fail.
- **Association flap takes the whole node down:** every flap
  forces a fresh S1 / N2 setup, and every UE on the eNodeB or
  gNB loses its signalling path at once.
- **Middlebox blindness:** NAT devices and load balancers that
  understand only TCP/UDP silently drop or mangle SCTP, which
  is why S1-MME and N2 are normally run on direct routed paths.
- **Wrong port in the firewall rule:** 36412 is S1-MME, 38412 is
  N2, 36422 is X2-C — opening one does not open the others.

---

## 🔌 Related Interfaces

- S1-MME (eNodeB ↔ MME) — S1AP over SCTP, port 36412
- N2 (gNB ↔ AMF) — NGAP over SCTP, port 38412
- X2-C (eNodeB ↔ eNodeB) — X2AP over SCTP, port 36422
- Xn-C (gNB ↔ gNB) — XnAP over SCTP, port 38412
- S1-U / N3 — GTP-U over UDP (user plane, not SCTP)
- Diameter — can also run over SCTP as an alternative to TCP

---

## 🧠 PacketAlchemy Note

TCP gives you one ordered pipe on one address. SCTP gives you
many independent streams on many addresses — and a heartbeat
that tells you a path died before your subscribers do.

When an entire eNodeB goes dark while its radio is fine, don't
start with the UE. Start with the SCTP association.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | SCTP | S1-MME | N2 | NGAP
