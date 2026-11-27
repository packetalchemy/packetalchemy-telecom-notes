# SRVCC: Keeping the Voice Call Alive from VoLTE to 2G/3G

## 📡 Overview

A VoLTE call rides on LTE packet bearers, but LTE coverage
does not reach everywhere. When a user on an active VoLTE
call walks out of LTE coverage, the call must survive the
move to 2G or 3G — a network with no IMS, no SIP, and no
packet voice bearer. That survival is SRVCC: Single Radio
Voice Call Continuity (3GPP TS 23.216).

The "single radio" part is the hard constraint. The UE has
one radio: it cannot stay on LTE and tune to 2G/3G at the
same time. The handover is break-before-make at the radio
level, so continuity has to be prepared in advance — the
call is anchored in IMS from the moment it is set up, and
when the radio drops, the session is transferred to the
circuit-switched domain instead of being dropped.

Without SRVCC, every VoLTE call that leaves LTE coverage
ends in silence. With it, the user hears a short gap —
typically under 300 ms with enhanced SRVCC — and keeps
talking.

---

## 🔄 How SRVCC Works

The trick is that the voice session is anchored in IMS
before the handover ever happens. The transfer itself has
three parallel legs: the session transfer in IMS, the radio
handover to GERAN/UTRAN, and the media switch.

### 1. Anchoring at call setup (aSRVCC)

When a VoLTE call is established, it is anchored at the
ATCF (Access Transfer Control Function) in the IMS core,
with the ATGW (Access Transfer Gateway) anchoring the media
path (TS 23.237). Anchoring is decided at call setup — a
call that was never anchored cannot be transferred later,
no matter how good the handover preparation is.

The HSS stores one number per subscriber for this purpose:
the STN-SR (Session Transfer Number for SRVCC). It is
downloaded to the MME over S6a during attach, so the MME
already knows where to send the session when the time
comes.

### 2. The handover trigger

The eNodeB sees the LTE signal fading and the 2G/3G
neighbour cell getting stronger (measurement reports,
typically a B2 event). It decides a PS-to-CS handover is
needed and tells the MME.

### 3. MME to MSC Server over Sv

The MME sends a PS-to-CS Handover Request to the MSC
Server over the Sv interface (GTPv2-based), including the
STN-SR and the target cell. The MSC Server must be
SRVCC-enhanced: it prepares resources in the target
GERAN/UTRAN and reserves the circuit-switched leg.

### 4. Session transfer in IMS

In parallel, the MSC Server initiates session transfer by
sending a SIP INVITE with the STN-SR to the ATCF. The ATCF
recognises its own anchored session and transfers it: the
remote party's media is re-pointed from the LTE bearer <redacted>
the ATGW and the circuit-switched leg through the MSC
Server and the MGW.

### 5. The UE retunes

The MME sends the Handover Command to the UE via the
eNodeB. The UE leaves LTE, tunes to the 2G/3G cell, and
completes the radio handover. The voice path is now fully
circuit-switched, and the LTE bearers are released.

### Enhanced SRVCC (eSRVCC)

Classic SRVCC transfers the session only after the handover
starts, which stretches the voice gap. Enhanced SRVCC
prepares the session transfer in advance — the ATCF and
ATGW are primed before the UE retunes — cutting the
interruption to around 300 ms, close to an intra-LTE
handover. Most live networks run eSRVCC.

---

## ⚠️ Common Issues

- **Call never anchored:** the VoLTE call bypassed the
  ATCF (anchoring disabled or misconfigured), so there is
  nothing to transfer — the call drops at the LTE edge
  even though SRVCC is "enabled".
- **STN-SR missing:** the HSS has no STN-SR for the
  subscriber, or S6a did not deliver it to the MME — the
  MME cannot address the session transfer and the
  handover fails at step one.
- **MSC Server not SRVCC-enhanced:** the Sv request is
  rejected, or the target side cannot prepare the CS leg
  — check the MSC Server's SRVCC support and the Sv
  interface first.
- **Target cell not prepared:** the GERAN/UTRAN cell has
  no free resources or the handover preparation times
  out — the UE stays on a dying LTE cell until the call
  drops.
- **Long voice gap:** classic SRVCC instead of eSRVCC,
  or a slow session transfer — the call survives but the
  user hears seconds of silence and hangs up.
- **SRVCC in alerting phase:** a call still ringing when
  the handover happens needs the alerting-phase transfer
  path; without it, the call fails before it is answered.
- **Emergency calls:** SRVCC does not apply to emergency
  calls — they are handled by CSFB, not by session
  transfer, so a dropped emergency call at the LTE edge
  is a different problem.

---

## 🔌 Related Interfaces

- Sv (MME ↔ MSC Server) — PS-to-CS handover signalling,
  GTPv2-based
- S6a (MME ↔ HSS) — delivers the STN-SR to the MME
- Gm (UE ↔ P-CSCF) — SIP signalling for the anchored
  VoLTE call
- Mw (P-CSCF ↔ I-CSCF / S-CSCF) — SIP path to the ATCF
- Mg / Mi (MSC Server ↔ IMS) — session transfer signalling
- Mc (MSC Server ↔ MGW) — media gateway control for the
  CS leg
- S1-MME (eNodeB ↔ MME) — handover decision and command
- IuCS / A (MSC Server ↔ RNC / BSC) — target radio
  preparation

---

## 🧠 PacketAlchemy Note

SRVCC is decided at call setup, not at handover time. If
the call was never anchored at the ATCF and the STN-SR was
never delivered to the MME, no handover procedure can save
it — the radio will simply drop.

When VoLTE calls die at the LTE coverage edge, check the
anchoring first: the STN-SR in the HSS, the ATCF in the
call path, and the Sv interface to the MSC Server. The
radio part is usually the last thing to blame.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | IMS | SRVCC | VoLTE | MSC Server | ATCF | Sv
