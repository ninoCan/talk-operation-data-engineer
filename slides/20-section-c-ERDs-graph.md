---
layout: center
class: text-center
---

<div mb-20 style="display: flex; justify-content: center;">
  <img src="../assets/ERD.png" style="scale: 130%; clip-path: inset(25px 15px 5px 20px); mix-blend-mode: multiply;">
</div>

# ERDs define the ontology of your data

**FK → PK** defines directionality of who owns the relationship

<!--
Entity-Relationship Diagrams are more than technical documentation — they're the ontology of your data.
They define what entities exist, what their attributes are, and how they relate to each other.
The PK to FK relationship defines directionality. Customer places orders. Orders contain items. Products appear in items.
That directionality is critical, because when you JOIN tables, you often lose it.
Time-series tables are a special case: they use a composite key of the entity ID plus a timestamp, because the same entity has many rows over time.
Keep this diagram in your head — you'll need it when we talk about JOINs.
-->
