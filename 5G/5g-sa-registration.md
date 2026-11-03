# 5G SA Registration Deep Dive

## 📡 Overview

Registration is the first procedure a UE performs in 5G Standalone
(SA). Before any PDU session, any data, or any service, the UE must
attach to the 5G Core (5GC): it selects a cell, talks to an AMF, proves
its identity, and receives the network's view of what it is allowed to
do — its registered PLMN, tracking area, slice, and security context.

Registration is not just "attach with a new name". In 5GC it is a
service-based procedure: the AMF coordinates AUSF, UDM, and PCF over
HTTP/2 service-based interfaces, and the result is a registered UE
context in the AMF plus a NAS security context shared with the UE.

---

## 🧩 Key Functions

- **UE** — sends the NAS Registration Request with its 5G-S-TMSI or
  SUCI, requested NSSAI, and registration type (initial, mobility,
  periodic, or emergency).
- **gNB** — carries NAS over RRC and NGAP, and selects the AMF when
  no AMF is already assigned, using the requested NSSAI and PLMN.
- **AMF** — owns the procedure: identity check, authentication
  trigger, NAS security setup, PCF policy association, and the final
  Registration Accept.
- **AUSF / UDM** — authenticate the subscriber with 5G-AKA, based on
  the credentials stored for the SUPI.
- **PCF** — provides AM policy: service area restrictions, RFSP index,
  and UE policy (URSP, ANDSP) via the AMF.
- **NSSF** — consulted when the requested NSSAI needs slicing
  decisions the AMF cannot make alone.

---

## 🔄 Procedure Step by Step

1. **RRC connection** — the UE connects to the gNB and sends an RRC
   Setup Complete carrying the NAS Registration Request.
2. **AMF selection** — the gNB forwards it in an NGAP Initial UE
   Message to the selected AMF.
3. **Identity resolution** — if the UE presented a 5G-S-TMSI the AMF
   cannot resolve, or no temporary identity at all, the AMF asks for
   identity and the UE answers with its SUCI (concealed SUPI).
4. **Authentication (5G-AKA)** — the AMF asks the AUSF (Nausf), which
   asks the UDM (Nudm) for authentication vectors. The UE answers the
   challenge; on success both sides derive the anchor key K_AUSF and
   then K_SEAF.
5. **NAS security** — the AMF runs a Security Mode Command/Complete
   exchange, activating NAS ciphering and integrity protection.
6. **Policy association** — the AMF creates an AM Policy Association
   with the PCF (Npcf_AMPolicyControl, N15) and fetches the allowed
   NSSAI, service area restrictions, and UE policy.
7. **Registration Accept** — the AMF sends the accept with the 5G-GUTI,
   TAI list, allowed NSSAI, and periodic registration timer. The UE
   replies Registration Complete, and the UE is registered.

---

## ⚠️ Common Issues

- Registration rejected, cause "illegal UE": the SUPI is unknown in
  the UDM or barred — an identity and subscription problem, not radio.
- Authentication fails repeatedly: the UDM credentials, the AUSF
  routing, or the UE's USIM profile do not match — check Nausf and
  Nudm before blaming the radio.
- Registration succeeds but no slice: the requested S-NSSAI is not in
  the allowed NSSAI — the subscription, NSSF, or PCF policy does not
  permit that slice for this subscriber.
- UE keeps re-registering: the periodic timer, TAI list, or mobility
  registration is misconfigured, so the UE leaves and re-enters the
  registered area.
- Security Mode failure: NAS integrity or ciphering algorithms
  offered by the UE are not supported by the AMF configuration.

---

## 🔌 Related Interfaces

- N1 (UE ↔ AMF) — NAS Registration Request / Accept / Complete
- N2 (gNB ↔ AMF) — NGAP Initial UE Message, NAS transport
- N8 (AMF ↔ UDM) — subscription data, authentication data
- N12 (AMF ↔ AUSF) — 5G-AKA authentication service
- N13 (AUSF ↔ UDM) — authentication vector retrieval
- N15 (AMF ↔ PCF) — AM policy control
- N22 (AMF ↔ NSSF) — slice selection assistance

---

## 🧠 PacketAlchemy Note

Most registration failures are not radio failures — they are identity,
authentication, subscription, or slice-policy failures wearing a NAS
reject cause.

Read the reject cause first, then walk the chain in order: identity,
AUSF/UDM authentication, PCF policy, and only then the radio.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | Registration | AMF | 5G-AKA | NAS | N1 | N2
