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

<div ml-10 style="text-align: left">

### **Fact tables**: describe events, collect measures

### **Dimension tables**: describe actors

- When connected to a fact table, ought to be *stable*. If not, attach **satellite dimensions** → Snowflake

- When shared across multiple fact tables, they are called **conformed**

<!--
TIMING: 60 seconds

"I'll cover only the star schema — it's the most widespread, and it's enough for our purposes."

Point to the diagram:
"Fact tables sit at the center. They describe events and collect the measures. Dimension tables radiate outward — they describe the actors and their attributes."

"Dimensions connected to a fact table should be STABLE. The columns shouldn't change underneath you while the pipeline is running."

"If they DO change — like email addresses or marketing campaign names — consider splitting them into satellite dimensions. Park them in a separate table and point to the most recent value. That turns your star into a snowflake schema. But that's a topic for another talk."

"One more term worth knowing: when a dimension is shared across multiple fact tables, it's called a CONFORMED dimension. The date dimension in this diagram is the classic example — both Sales and Purchases share it. It may come up in questions, so: now you know the word."

LIKELY QUESTION: "What about Data Vault or Anchor Modeling?"
A: Valid alternatives, especially for auditability and schema evolution. Star schema is the simplest starting point and covers 80% of OLAP use cases. The other approaches add complexity in exchange for specific guarantees — worth researching once you're comfortable with star.

TRANSITION TO NEXT: "Now: the most skipped step."
-->
</div>
