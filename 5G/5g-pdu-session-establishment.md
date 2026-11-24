# 5G PDU Session Establishment: From Request to User Plane

## 📡 Overview

A **PDU session** is the 5G equivalent of an LTE PDN connection: the
logical pipe that gives a UE an IP address (or Ethernet / unstructured
transport) and a path to a data network. Establishment is a three-way
dance between the UE, the **AMF** (mobility), and the **SMF** (session
management) — with the **PCF** setting policy, the **UPF** building the
user plane, and the **gNB** reserving radio resources. It succeeds only
when every leg agrees: the subscription allows the DNN, a UPF is
available, the PCF authorises the session, and the gNB accepts the QoS
profile. A failure on any leg rejects the session with a 5GSM cause
code — and the code tells you exactly which leg said no.

---

## 🔄 The Flow Step by Step

1. **UE → AMF — PDU Session Establishment Request** (NAS, N1).
   The UE sends the DNN, the S-NSSAI, the requested PDU session type
   (IPv4 / IPv6 / IPv4v6 / Ethernet / Unstructured), and the SSC mode.
2. **AMF → SMF — Nsmf_PDUSession_CreateSMContext Request.**
   The AMF picks an SMF (via NRF discovery or configuration) and
   forwards the request with the UE location and the subscription
   data it holds.
3. **SMF checks the subscription** (via UDM) — is this DNN allowed
   for this SUPI, is the S-NSSAI in the subscribed set?
4. **SMF → PCF — Npcf_SMPolicyControl_Create.**
   The PCF returns PCC rules: authorised QoS (5QI, ARP, session AMBR),
   charging rules, and traffic steering policy.
5. **SMF selects a UPF** (see the UPF selection note) and a PSA that
   will hold the session's IP address.
6. **SMF → UPF — PFCP Session Establishment Request** (N4).
   PDRs, FARs, QERs and URRs install the forwarding and QoS rules;
   the UPF allocates the UE's IP address and returns the N3 F-TEID.
7. **SMF → AMF — Nsmf_PDUSession_CreateSMContext Response** with
   the N2 SM information: QoS profiles, the UPF's N3 tunnel endpoint,
   and the NAS PDU Session Establishment Accept for the UE.
8. **AMF → gNB — PDU Session Resource Setup Request** (NGAP, N2)
   carrying the QoS profiles and the N3 tunnel info.
9. **gNB → UE — RRC Reconfiguration** with the NAS Accept; the UE
   configures its DRB and the session becomes active.
10. **gNB → AMF → SMF — N2 response.** The gNB returns its N3
    F-TEID; the SMF sends a **PFCP Session Modification Request**
    to the UPF with the gNB tunnel endpoint — and user traffic can
    flow on N3.

---

## 🧩 Key Messages & Parameters

- **DNN** — identifies the data network (the APN of 5G). A wrong or
  unsubscribed DNN is the most common rejection.
- **S-NSSAI** — the slice the session belongs to; the SMF, UPF and
  PCF must all serve it.
- **SSC mode** — 1: anchor fixed for the session; 2: anchor may be
  relocated (new IP); 3: new anchor before the old one is released.
- **PDU session type** — IPv4, IPv6, IPv4v6, Ethernet, Unstructured.
- **5QI / ARP / session AMBR** — the QoS the PCF authorised and the
  gNB must enforce.
- **Secondary authentication** — optional EAP exchange with an
  external DN-AAA server, e.g. for enterprise DNNs.
- **Always-on PDU session** — requested by the UE for services that
  must never be released, e.g. IMS signalling.

---

## ⚠️ Common Issues

- Cause #27 "Missing or unknown APN": the DNN is not in the
  subscription or is misspelled in the UE's configuration.
- Cause #33 "Requested service option not subscribed": the DNN
  exists, but this subscriber is not entitled to it.
- Cause #69 "Insufficient resources": no UPF registered for the
  DNN/S-NSSAI, or the NRF returned no candidates — check the UPF
  profile, not the radio.
- Session accepted but no traffic: the PFCP Session Modification
  with the gNB's N3 F-TEID never reached the UPF — the downlink
  FAR still points nowhere.
- QoS rejected by the gNB: the 5QI/ARP from the PCF does not match
  what the gNB supports — the N2 setup fails and the session is
  released.
- Secondary authentication timeout: the DN-AAA server does not
  answer, and the SMF rejects the session even though the core
  is healthy.

---

## 🔌 Related Interfaces

- N1 (UE ↔ AMF) — NAS: PDU Session Establishment Request / Accept
- N2 (gNB ↔ AMF) — NGAP: PDU Session Resource Setup
- N4 (SMF ↔ UPF) — PFCP: session establishment, modification
- N3 (gNB ↔ UPF) — GTP-U: the user-plane tunnel
- Nsmf (AMF ↔ SMF) — PDU session context creation
- Npcf (SMF ↔ PCF) — SM policy control, PCC rules
- Nudm (SMF ↔ UDM) — subscription data for the DNN
- N6 (UPF ↔ DN) — breakout toward the data network

---

## 🧠 PacketAlchemy Note

Registration gets the UE into the network — PDU session
establishment gets it onto the internet. When a user has full
signal but no data, the registration is usually fine and the
session is where it broke: read the 5GSM cause code first. It
names the failing leg — subscription, UPF, policy, or radio —
and saves hours of guessing across the wrong interface.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | PDU Session | SMF | AMF | UPF | N4 | PFCP | DNN
