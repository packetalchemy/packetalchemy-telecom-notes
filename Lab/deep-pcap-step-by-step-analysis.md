# Deep PCAP Analysis – Frame-by-Frame Core Network Debugging

## 🧠 Objective

This lab teaches how senior engineers analyze PCAP files frame-by-frame to identify root cause in EPC / 5GC networks.

---

# 📡 1. What Makes This "Deep" Analysis?

Instead of just saying:
- “No internet”

We reconstruct:

- Exact packet flow
- Timing issues
- Missing responses
- Node-level failure

---

# 🧪 2. Example PCAP Flow (LTE Attach + Data Failure)

We assume following flow in Wireshark:

---

## 📌 Frame 1–5: Attach Procedure

```
Frame 1: UE → MME : Attach Request
Frame 2: MME → HSS : Authentication Request
Frame 3: HSS → MME : Authentication Response
Frame 4: MME → SGW : Create Session Request
Frame 5: PGW → MME : Create Session Response
```

✔ Result: Attach SUCCESS

---

## 📌 Frame 6–10: DNS Resolution Phase

```
Frame 6: PGW → DNS : APN Query
Frame 7: DNS → PGW : No Response (timeout)
Frame 8: PGW retries DNS query
Frame 9: DNS still no response
Frame 10: fallback failed
```

❌ Problem detected here

---

## 📌 Frame 11–15: Diameter (OCS Check)

```
Frame 11: PGW → OCS : CCR-Initial
Frame 12: OCS → PGW : (no response)
Frame 13: PGW retries CCR
Frame 14: timeout reached
Frame 15: session marked as restricted
```

❌ Charging layer degraded

---

## 📌 Frame 16–20: GTP-U (User Plane)

```
Frame 16: UE sends data packet
Frame 17: SGW receives nothing
Frame 18: No UDP 2152 tunnel established
Frame 19: Packet dropped
Frame 20: Retransmission
```

❌ No user plane traffic

---

# 🚨 3. Root Cause Correlation

Now we correlate layers:

| Layer | Status |
|------|--------|
| Attach (MME/HSS) | OK |
| Session (GTP-C) | OK |
| DNS | FAILED |
| OCS | DEGRADED |
| GTP-U | NOT ESTABLISHED |

---

# 🎯 4. Final Root Cause

## PRIMARY ISSUE:
👉 DNS failure caused incorrect or missing PGW resolution

## SECONDARY ISSUE:
👉 OCS timeout caused session restriction fallback

---

# 🧠 5. Senior Engineer Thinking Model

A junior engineer says:
- “DNS is down”

A senior engineer says:
- “DNS failure blocked APN resolution, which prevented correct PGW selection, causing OCS retries and GTP-U non-establishment.”

---

# 🔥 6. Key Insight

> “The failure is never in one packet. It is in the relationship between packets.”

---

# 📊 7. Debug Checklist (Real TAC Method)

When analyzing PCAP:

### Step 1:
- Check Attach flow

### Step 2:
- Check DNS resolution

### Step 3:
- Check Diameter (OCS / PCRF)

### Step 4:
- Check GTP-U traffic

### Step 5:
- Correlate timing between failures

---

# 🧠 8. Engineering Insight

> “Packet analysis is not reading data. It is reconstructing system behavior from partial evidence.”

---

## 👤 Author

PacketAlchemy  
Telecom Core Engineering | Deep PCAP Analysis | EPC | 5G Core | Vendor TAC Level Troubleshooting
