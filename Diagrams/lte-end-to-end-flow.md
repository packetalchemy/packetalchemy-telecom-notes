# LTE End-to-End Signaling & Data Flow Diagrams

## 📡 1. LTE Attach + Data Session (Full Flow)

```mermaid
sequenceDiagram
    participant UE
    participant eNB as eNodeB
    participant MME
    participant HSS
    participant SGW
    participant PGW
    participant PCRF
    participant OCS

    UE->>eNB: Attach Request
    eNB->>MME: Initial UE Message

    MME->>HSS: Authentication / Update Location
    HSS-->>MME: Auth Vectors

    MME->>UE: Security Setup
    UE-->>MME: Security Complete

    MME->>SGW: Create Session Request (GTP-C)
    SGW->>PGW: Create Session Request

    PGW->>PCRF: Policy Request (Gx)
    PCRF-->>PGW: Policy Response

    PGW->>OCS: Credit Request (Gy)
    OCS-->>PGW: Credit Response

    PGW-->>SGW: Create Session Response (IP + TEID)
    SGW-->>MME: Session Established

    MME->>UE: Attach Accept
    UE-->>MME: Attach Complete
```

---

## 🌍 2. Roaming Architecture Flow

```mermaid
flowchart LR
    UE --> eNB
    eNB --> VMME[Visited MME]
    VMME --> HSS[Home HSS]

    VMME --> VSGW[Visited SGW]
    VSGW --> HPGW[Home PGW]

    HPGW --> PCRF
    HPGW --> OCS

    HPGW --> Internet[(Internet)]
```

---

## 🔌 3. GTP-C vs GTP-U Separation

```mermaid
flowchart TB

subgraph Control Plane
A[UE Attach] --> B[MME]
B --> C[GTP-C Session Setup]
C --> D[SGW / PGW Control]
end

subgraph User Plane
E[UE Data] --> F[eNodeB]
F --> G[GTP-U Tunnel]
G --> H[SGW]
H --> I[PGW]
I --> J[Internet]
end
```

---

## 🌐 4. DNS-Based PGW Selection

```mermaid
flowchart LR
UE --> MME
MME --> DNS
DNS --> PGW1[PGW Home]
DNS --> PGW2[PGW Roaming]
DNS --> PGW3[Local Breakout PGW]

PGW1 --> Internet
PGW2 --> Internet
PGW3 --> Internet
```

---

## 💳 5. Charging + Policy Interaction

```mermaid
flowchart TB

UE --> PGW
PGW --> PCRF
PGW --> OCS

PCRF -->|Policy Rules| PGW
OCS -->|Credit Control| PGW

PGW --> Internet
```

---

## 🧠 PacketAlchemy Insight

In real networks:

- Attach success ≠ Internet success
- Each domain can independently block service:
  - DNS → wrong routing
  - PCRF → policy denial
  - OCS → credit block
  - GTP-U → transport failure

---

## 📌 Design Philosophy

These diagrams represent:

✔ Control Plane (GTP-C, Diameter)  
✔ User Plane (GTP-U)  
✔ Policy Control (PCRF)  
✔ Charging (OCS)  
✔ Routing (DNS)

---

## 👤 Author

PacketAlchemy  
Telecom Engineering Notes | EPC | LTE | 5G Core | Network Architecture
