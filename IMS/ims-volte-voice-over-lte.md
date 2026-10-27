# IMS and VoLTE: How Voice Travels Over LTE

## 📖 Overview

LTE is an all-IP network: it has no circuit-switched voice domain. Voice over LTE (VoLTE) therefore carries voice as an IP Multimedia Subsystem (IMS) application over the packet-switched EPC, not as a legacy circuit call.

The UE registers to IMS over its IMS APN, and voice media travels as RTP packets inside a dedicated bearer with guaranteed bit rate. Signalling travels as SIP. If either the IMS registration or the dedicated bearer fails, there is no VoLTE call — even when mobile data works fine.

---

## 🧩 Key Elements

- **UE** — IMS client, SIP stack, voice codec (typically AMR-WB)
- **eNodeB** — radio access, carries the bearers
- **MME / SGW / PGW** — EPC control and user plane, bearer setup
- **P-CSCF** — first IMS contact point, SIP proxy for the UE
- **I-CSCF** — entry point to the home IMS network
- **S-CSCF** — serving registrar and call session control
- **HSS** — subscriber data, IMS authentication vectors, S-CSCF assignment
- **TAS (Telephony Application Server)** — supplementary services (call forwarding, barring)
- **PCRF** — policy decisions for the voice bearer

---

## 🔄 IMS Registration

1. The UE attaches to EPC and gets an IP address for the IMS APN.
2. The PGW returns the P-CSCF address in the Protocol Configuration Options (PCO).
3. The UE sends a SIP REGISTER to the P-CSCF over the Gm interface.
4. The P-CSCF forwards it via I-CSCF to an assigned S-CSCF.
5. The S-CSCF authenticates the subscriber with the HSS over Cx (IMS-AKA) and downloads the service profile.
6. A successful 200 OK means the UE is IMS-registered and reachable for voice calls.

Registration uses the default bearer of the IMS APN, with SIP signalling carried on QCI 5.

---

## 📞 VoLTE Call Flow

1. The calling UE sends SIP INVITE with an SDP offer (codec, media ports) to the called party via P-CSCF and S-CSCF.
2. The S-CSCF routes the INVITE to the terminating network and invokes the TAS for services.
3. The PCRF receives the media requirements over Rx from the P-CSCF and authorises a dedicated voice bearer over Gx to the PGW.
4. The PGW and SGW establish the dedicated bearer: QCI 1, guaranteed bit rate, with a traffic flow template matching the RTP stream.
5. SIP signalling completes (183 Session Progress, PRACK, 180 Ringing, 200 OK) and RTP media flows on the QCI 1 bearer.
6. At call end, SIP BYE tears the session down and the dedicated bearer is released.

Voice quality depends on the QCI 1 bearer, not on best-effort data throughput: QCI 1 gives low delay and low packet loss for the RTP stream.

---

## 🎧 QoS: Why Two Bearers Matter

- **QCI 5** — IMS signalling (SIP), non-GBR, high priority for signalling
- **QCI 1** — voice media (RTP), GBR, low delay budget, the bearer that actually carries the conversation
- **QCI 8/9** — default internet data, best effort, no guarantee

A UE can browse the internet perfectly while VoLTE fails: the data bearer and the voice bearer are independent, and only the dedicated bearer carries the call.

---

## ⚠️ Common Issues

- **IMS registration fails** — IMS APN missing or misconfigured, P-CSCF unreachable, HSS authentication rejected, or SIP blocked on the path.
- **UE shows data but no VoLTE icon** — check IMS APN, IMS registration status, and whether the subscription allows VoLTE.
- **Call setup succeeds but no audio** — dedicated QCI 1 bearer not established, RTP blocked or misrouted, or codec mismatch in SDP.
- **One-way audio** — asymmetric bearer or NAT/firewall issue on the RTP path; check both directions, not just signalling.
- **Call drops during LTE handover** — dedicated bearer not preserved across X2/S1 handover, or SRVCC to legacy voice not configured where LTE coverage ends.
- **SIP works but voice is choppy** — QCI 1 bearer throttled or mapped to best effort; check PCRF policy and PGW bearer binding.

---

## 🔗 Related Interfaces

- **Gm** — UE to P-CSCF (SIP)
- **Mw** — between CSCFs (SIP)
- **Cx** — S-CSCF/I-CSCF to HSS (Diameter)
- **Dx** — I-CSCF/S-CSCF to SLF (Diameter, when multiple HSSs exist)
- **Rx** — P-CSCF to PCRF (Diameter)
- **Gx** — PCRF to PGW/PCEF (Diameter)
- **SGi** — PGW to IMS network
- **S1-U / S5 / S8** — EPC user-plane bearers carrying SIP and RTP

---

## 🧠 PacketAlchemy Note

Data working does not mean voice working. VoLTE is a separate chain: IMS registration, SIP signalling, PCRF policy, and a dedicated QCI 1 bearer.

When a VoLTE call fails, check the chain in order — registration first, then signalling, then the dedicated bearer, then the media path. The first broken link is the fault; everything after it is noise.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | EPC | LTE | IMS | VoLTE
