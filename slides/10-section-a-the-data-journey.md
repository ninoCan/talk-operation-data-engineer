---
layout: center
class: text-center
---

# The DATA JOURNEY

```mermaid
flowchart LR
    A["OLTP normalized"] --> B["Dimensional Modeling"]
    B --> C["OLAP aggregated"]
    style A fill:#d39235,color:#fff,font-weight:bold
    style B fill:#b36622,color:#fff,font-weight:bold
    style C fill:#6e848c,color:#fff,font-weight:bold
    linkStyle 0,1 stroke:#954220,stroke-width:2px
```

- **OLTP**: write-optimized, normalized, 1:1 with objects
- **Dimensional modeling**: many to chose from `Star`, `Snowflake`, `Data Vault`, `Anchor` modeling
- **OLAP**: read-optimized, aggregated, denormalized

## OBTs are the  *outcome* of the journey
