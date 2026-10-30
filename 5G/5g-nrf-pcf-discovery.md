# NRF and PCF Discovery in 5G Core

## 📡 Overview

In 5G Core there is no fixed peer configuration for most control-plane
signalling. A Network Function (NF) finds the NF it needs at run time
through the **NRF (NF Repository Function)**. An AMF that needs policy
control does not dial a hard-coded PCF address — it asks the NRF
"which PCF serves this PLMN, slice, and DNN?" and gets a profile back.

Two discovery stories matter most: **NF discovery via the NRF** and
**PCF discovery** — because a registration, a PDU session, or a policy
decision can fail before any real signalling starts, simply because
discovery returned nothing, or the wrong instance.

---

## 🧩 NRF Building Blocks

- **NF profile** — the record each NF registers: NF instance ID, NF
  type (AMF, SMF, PCF, UDM, ...), PLMN, supported S-NSSAIs and DNNs,
  FQDN or IP endpoints, offered services, plus capacity, priority,
  and load used for selection.
- **Nnrf_NFManagement** — registration, deregistration, update, and
  subscription to NF status changes. Each NF sends a periodic
  heartbeat; if heartbeats stop, the NRF stops handing out the profile.
- **Nnrf_NFDiscovery** — the query service. Filters include target NF
  type, requester NF type, service names, S-NSSAI, DNN, and PLMN.
- **Nnrf_AccessToken** — OAuth2 authorization. A consumer requests a
  token scoped to a producer service (e.g. `Npcf_SMPolicyControl`);
  the producer validates it before serving the request.
- **SCP (Service Communication Proxy)** — optional middle layer. With
  delegated discovery, the consumer sends to the SCP, and the SCP
  queries the NRF, selects an instance, and routes the message.

---

## 🔄 How PCF Discovery Works

1. **PCF registers** — each PCF instance registers its profile with
   the NRF, including the S-NSSAIs and DNNs it serves and whether it
   handles AM, SM, or UE policy.
2. **AMF needs AM policy** — during registration, the AMF discovers
   a PCF for AM policy control (Npcf_AMPolicyControl, interface N15),
   filtering by PLMN and the UE's S-NSSAI.
3. **SMF needs SM policy** — during PDU session establishment, the
   SMF discovers a PCF for SM policy control (Npcf_SMPolicyControl,
   interface N7), filtering by PLMN, S-NSSAI, and DNN.
4. **BSF binds the choice** — the Binding Support Function stores the
   PCF binding for the UE's SUPI, so later sessions land on the same
   PCF instance instead of a random one.
5. **Re-discovery on failure** — if the selected PCF stops answering,
   the consumer re-queries the NRF and fails over to another profile
   instead of failing the whole procedure.

---

## ⚠️ Common Issues

- Discovery returns nothing: the PCF registered without the S-NSSAI
  or DNN the AMF/SMF filters on — up, but invisible to that query.
- Wrong PCF selected: capacity and priority left at defaults, so
  traffic piles onto one instance while others sit idle.
- Session works, policy does not: the access token scope does not
  cover Npcf_SMPolicyControl, so the PCF rejects the request.
- Flapping after a restart: the old profile is still cached while the
  new instance registers with a new instance ID — stale endpoints
  until the cache or NRF subscription updates.
- BSF binding missing: each new PDU session lands on a different PCF,
  splitting one subscriber's policy state across instances.

---

## 🔌 Related Interfaces

- Nnrf (NRF) — NFManagement, NFDiscovery, AccessToken services
- N15 (AMF ↔ PCF) — AM policy control
- N7 (SMF ↔ PCF) — SM policy control, PCC rules
- N5 (AF ↔ PCF) — application function influence on policy
- Nbsf (BSF) — PCF binding for a subscriber

---

## 🧠 PacketAlchemy Note

Discovery failures look like policy, registration, or session
failures — but the root cause is one step earlier.

When a procedure fails before any policy or session signalling, check
discovery first: what did the NRF return, what did the query filter
on, and is the returned profile still alive?

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | 5G Core | NRF | PCF | Discovery | SBA | Nnrf
