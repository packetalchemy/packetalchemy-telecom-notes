# Diameter Cx/Dx: IMS Registration and the HSS

## 🔌 Overview

Cx is the Diameter interface between the CSCF (I-CSCF and S-CSCF)
and the HSS. It carries everything IMS needs to register a
subscriber: which S-CSCF serves them, their service profile, and
their authentication vectors. If Cx fails, IMS registration fails —
and VoLTE never starts, even with a perfect LTE attach behind it.

Dx is the same protocol between the CSCF and the SLF (Subscription
Locator Function). In networks with more than one HSS, the CSCF asks
the SLF first: "which HSS holds this subscriber?" The SLF answers
with the HSS address, and the CSCF then runs the normal Cx dialogue
against that HSS.

Both interfaces are defined in 3GPP TS 29.228 / TS 29.229.

---

## 🧩 Main Procedures

### UAR / UAA — User-Authorization-Request / Answer
The I-CSCF asks the HSS (via the SLF on Dx) whether this subscriber
may register, and which S-CSCF capabilities are required. The HSS
returns the S-CSCF capabilities or the name of the S-CSCF already
assigned to the subscriber.

### MAR / MAA — Multimedia-Auth-Request / Answer
The S-CSCF asks the HSS for IMS-AKA authentication vectors (RAND,
AUTN, XRES, CK, IK). The HSS returns them; the S-CSCF challenges
the UE with a SIP 401 Unauthorized carrying the AKA challenge.

### SAR / SAA — Server-Assignment-Request / Answer
The S-CSCF tells the HSS "I am now serving this subscriber". The
HSS stores the S-CSCF name and returns the full user profile:
public and private identities, service profile, and initial filter
criteria (iFC) that trigger application servers.

### LIR / LIA — Location-Info-Request / Answer
The I-CSCF asks which S-CSCF is currently serving a subscriber
(for example when routing a terminating call). The HSS returns the
assigned S-CSCF name, or the capabilities needed to select one.

### RTR / RTA — Registration-Termination-Request / Answer
The HSS tells the S-CSCF to de-register a subscriber (for example
after an administrative action or a subscription change). The
S-CSCF removes the registration and notifies the UE.

### PPR / PPA — Push-Profile-Request / Answer
The HSS pushes an updated user profile to the S-CSCF mid-registration
(for example a changed service profile or a barred service).

---

## 🔍 What the User Profile Decides

From SAA / PPA the S-CSCF learns:

- Public identities (IMPU) and the private identity (IMPI).
- The service profile: allowed services and barred services.
- Initial filter criteria (iFC): which application servers to
  invoke, and in which order.
- Charging information (primary/secondary charging function
  addresses).
- The S-CSCF name stored in the HSS for terminating-call routing.

Most "IMS registered but no voice" cases trace back to one of
these fields — a missing iFC means the call never reaches the
telephony application server.

---

## ⚠️ Common Issues

- UAR rejected: unknown subscriber, or the SLF returns the wrong
  HSS address — registration stops before authentication.
- MAR failure: authentication scheme mismatch (IMS-AKA vs.
  SIP Digest) or exhausted vectors.
- SAR not stored: the HSS never learns the S-CSCF name, so
  terminating calls cannot be routed (LIR returns no S-CSCF).
- Stale profile: PPR never delivered, S-CSCF works with an old
  service profile and invokes the wrong application servers.
- S-CSCF reselection loops: the HSS returns capabilities the
  I-CSCF cannot satisfy, and registration retries forever.
- Dx misconfiguration: the SLF points at the wrong HSS, so the
  subscriber looks unknown even though the data exists.

---

## 🔌 Related Interfaces

- Cx (I/S-CSCF ↔ HSS)
- Dx (CSCF ↔ SLF)
- Gm (UE ↔ P-CSCF)
- Mw (CSCF ↔ CSCF)
- Sh (AS ↔ HSS)
- S6a (MME ↔ HSS) — the EPC counterpart
- N8 (AMF ↔ UDM) — the 5G successor

---

## 🧠 PacketAlchemy Note

S6a decides who you are on the data network. Cx decides who you
are on the voice network.

A subscriber can attach to LTE perfectly and still never make a
call — if Cx cannot find them in the HSS, IMS registration simply
never happens.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | Diameter | IMS | VoLTE
