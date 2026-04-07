---
class: text-center
---

# Star schema: facts at the center, dimensions around it

```mermaid
graph LR
    T --- A[Fact: Shipment]
    S --- P[Dim: Product]
    P --- A
    C --- A
    S --- C[Dim: Customer]
    subgraph sg[ ]
        direction TB
        C --- E[Dim: Email]
        C --- D[Dim: Campaign]
    end
    D1[Dim: Retailer] --- F[Fact: Purchases]
    D2[Dim: Material] --- F
    F --- T[Dim: Date]
    F --- D4[Dim: Channel]
    T --- S[Fact: Sales]
    style D1 fill:#b36622,color:#fff
    style D2 fill:#b36622,color:#fff
    style D4 fill:#b36622,color:#fff
    style T fill:#954220,color:#fff
    style C fill:#b36622,color:#fff
    style P fill:#b36622,color:#fff
    style F fill:#7b9194,color:#fff
    style S fill:#7b9194,color:#fff
    style A fill:#7b9194,color:#fff
    style E fill:#d39235,color:#fff
    style D fill:#d39235,color:#fff
    style sg fill:none,stroke:none
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11 stroke:#222120,stroke-width:2px
```


<div style="text-align: left">

### **Fact tables** — describe events, collect measures

### **Dimension tables** — describe actors

When connected to a fact table, they ought to be stable

When shared across multiple fact tables, they are called **conformed**
</div>

<!--
The Star schema is the most common modeling pattern for analytics — and for good reason.
It's simple, readable, and maps directly to how business questions are asked.
Dimension tables describe your actors: who your customers are, what your products look like.
Fact tables describe events: what happened, involving which actors, at what time.
When you dissect an OBT, you're often reverse-engineering a Star schema.
The dimension columns you identified map to dimension tables. The fact columns map to a fact table. The keys connect them.
Once you have the Star schema, building the OBT from it is deliberate and controlled — not chaotic denormalization.
-->
