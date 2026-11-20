# P-CSCF Discovery: How a UE Finds Its Way to IMS

## 📡 Overview

Before a UE can register for VoLTE or any IMS service, it must
find the P-CSCF (Proxy-Call Session Control Function) — the
first SIP contact point of the IMS core. Without a P-CSCF
address, there is no SIP REGISTER, no IMS registration, and no
voice call, even when the LTE attach and the default bearer are
perfectly healthy.

Discovery happens when the UE sets up the IMS PDN connection
(the IMS APN): the network hands the UE one or more P-CSCF
addresses, and the UE uses them to start its SIP registration
over the Gm interface. If that handover fails, VoLTE fails
silently at step zero.

---

## 🔍 How the UE Learns the P-CSCF Address

There is no single mechanism — the UE tries them in a defined
preference order, and operators usually rely on the first one.

### 1. PCO — Protocol Configuration Options (most common)

During the PDN Connectivity procedure for the IMS APN, the UE
includes a PCO request for the P-CSCF address. The PGW/SMF
returns the address (IPv4, IPv6, or both) in the PCO of the
Activate Default EPS Bearer Context / PDU Session Establishment
Accept message (3GPP TS 24.008 / 24.301 / 24.501).

This is the normal path in live networks: the P-CSCF address
arrives together with the IMS bearer, so no extra lookup is
needed.

### 2. DHCP

If PCO returns nothing, the UE can ask DHCP: option 120 (SIP
Servers, RFC 3361) for IPv4, or DHCPv6 options 21/22 (SIP
server domain names / IPv6 addresses, RFC 3319) for IPv6.
This path is more common on fixed and Wi-Fi access than on
mobile, but it is a valid fallback.

### 3. DNS — NAPTR / SRV

The UE can resolve the IMS domain itself, typically
`ims.mnc<MNC>.mcc<MCC>.3gppnetwork.org` (TS 23.003), through
a NAPTR query that points to SRV records (`_sip._udp`,
`_sip._tcp`), which finally resolve to A / AAAA records.
DNS is the fallback when the network hands the UE a domain
name instead of an address.

### 4. Pre-configured Address

Some UEs carry a static P-CSCF address in their IMS settings.
It works, until the operator renumbers the P-CSCF — then every
such UE breaks at once, which is why this method is a last
resort, not a design.

### 5G Note

In 5G SA the same idea applies: the SMF returns the P-CSCF
address in the PCO of the PDU Session Establishment Accept
for the IMS DNN, and DNS/DHCP remain as fallbacks. The
mechanisms did not change — only the bearer that carries
them did.

---

## 🔄 What the UE Does With the List

- **Try in order.** The network usually returns several
  P-CSCF addresses. The UE tries the first; if the SIP
  REGISTER times out, it tries the next.
- **Re-discover on failure.** If every address in the list
  fails, the UE starts discovery again — a new PCO request
  on the IMS PDN, or a fresh DNS lookup — rather than
  giving up on IMS.
- **One P-CSCF per registration.** The UE registers through
  a single P-CSCF at a time; the rest of the list is purely
  a failover reserve.
- **P-CSCF is per-PLMN.** After roaming or a PLMN change, the
  old addresses are useless — the UE must discover the
  visited network's P-CSCF again.

Once discovered, the P-CSCF is more than an address: it is
the SIP proxy that terminates the UE's security association
(IPsec), applies SigComp compression, and talks to the PCRF /
PCF over Rx so the voice bearer gets its QoS.

---

## ⚠️ Common Issues

- **Empty PCO:** the IMS APN is misconfigured on the PGW/SMF
  and no P-CSCF address is returned — the UE attaches fine
  but VoLTE never starts. The first thing to check is the
  PCO in the PDN Connectivity Accept.
- **DNS NAPTR missing:** the `ims.mnc….3gppnetwork.org` zone
  has no NAPTR/SRV records, so the DNS fallback path dies
  and only PCO works — invisible until PCO breaks too.
- **IPv6-only P-CSCF, IPv4-only UE (or vice versa):** the
  address family returned does not match what the UE can
  use, and registration times out for no visible reason.
- **Stale P-CSCF after renumbering:** UEs with a static
  address keep trying a dead P-CSCF; the fix is on the
  provisioning side, not in the core.
- **Wrong P-CSCF while roaming:** the UE keeps the home
  P-CSCF list after moving to a visited network, and SIP
  REGISTERs go nowhere.
- **P-CSCF reachable, REGISTER rejected:** discovery worked
  but the P-CSCF cannot reach the I-CSCF or the HSS — now
  the problem is Cx/Dx and IMS registration, not discovery.

---

## 🔌 Related Interfaces

- Gm (UE ↔ P-CSCF) — SIP signalling, the interface discovery
  exists to reach
- PCO / NAS (UE ↔ MME/AMF ↔ PGW/SMF) — the usual delivery
  channel for the address
- DNS (UE ↔ DNS) — NAPTR / SRV / A / AAAA resolution of the
  IMS domain
- DHCP (UE ↔ DHCP) — option 120 / DHCPv6 options 21–22
- Rx (P-CSCF ↔ PCRF / PCF) — QoS authorisation for the voice
  bearer once registration succeeds
- Mw (P-CSCF ↔ I-CSCF / S-CSCF) — where SIP goes after the
  P-CSCF
- Cx/Dx (I-CSCF ↔ HSS) — subscriber lookup during registration

---

## 🧠 PacketAlchemy Note

VoLTE does not start with a SIP REGISTER. It starts with a
question: "where is my P-CSCF?" If the answer is missing,
wrong, or in the wrong address family, the phone shows full
signal and still cannot make a call.

When a UE attaches cleanly but never registers to IMS, check
the PCO first — the address the network handed over is the
cheapest thing to verify and the most common thing to be
wrong.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | IMS | P-CSCF | VoLTE | SIP | DNS
