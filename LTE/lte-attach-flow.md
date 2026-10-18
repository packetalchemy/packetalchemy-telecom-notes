# LTE Attach Flow

## Overview

The LTE attach procedure allows a UE to register to the EPC network and establish default connectivity.

## Main Network Elements

- UE
- eNodeB
- MME
- HSS
- SGW
- PGW

## High Level Flow

1. UE sends Attach Request
2. MME authenticates subscriber with HSS
3. Default bearer <redacted> created
4. PGW allocates IP address
5. Attach Accept is sent to UE

## Step-by-Step Flow with Interfaces

### 1. Attach Request — S1-MME
UE sends Attach Request (IMSI or GUTI, requested APN, UE capabilities) via eNodeB to the MME.

### 2. Authentication — S6a (AIR/AIA)
MME fetches authentication vectors from the HSS and runs EPS-AKA with the UE.

### 3. Location Update — S6a (ULR/ULA)
MME registers at the HSS and receives the subscription data: APN profiles, AMBR, QoS, roaming rules.

### 4. Session Creation — S11 + S5/S8
MME sends Create Session Request to the SGW (S11). The SGW forwards it to the PGW (S5/S8). The PGW allocates the UE IP address, applies policy (Gx), and the default bearer <redacted> is established.

### 5. Attach Accept — S1-MME
MME sends Attach Accept (with the allocated IP, APN config, TAI list) to the UE. The UE is now attached with default connectivity.

## Failure Points per Step

| Step | Typical failure | What to check |
|---|---|---|
| 1. Attach Request | Attach Reject cause #11, #7, #8, #15 | Subscription, PLMN, tracking area |
| 2. Authentication | AUTN mismatch / sync failure | Vectors, HSS, AUTS re-sync |
| 3. Location Update | ULR rejected | Roaming allowed, HSS reachability |
| 4. Session Creation | Create Session Response reject | APN config, PGW, DNS, GTP-C |
| 5. Attach Accept | UE never completes attach | S1-MME, eNodeB, radio |

## Notes

This note maps the flow and its failure points. Detailed signaling traces and message-level analysis are in `LTE/lte-attach-procedure.md`.

## 🧠 PacketAlchemy Note

Attach is a chain: every step depends on the one before it.

When attach fails, find the first step that broke — everything after it is noise.

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | PS Core | EPC | LTE | Attach
