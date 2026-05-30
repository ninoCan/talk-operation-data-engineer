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
TIMING: 40 seconds

"Let's talk about ERDs — entity relationship diagrams. If you're lucky, your company has them. If not, you'll need to reverse-engineer them from the schema and the team's domain knowledge."

"What's inside an ERD is a graph. Foreign keys in one table point to primary keys in another. That's a DIRECTION, not just a link."

"That direction tells you something important: who acts on what. Who owns the relationship. The child table points to the parent."

"You have to stitch the tables together respecting that direction. Get it backwards and you lose data — silently, no error raised."

LIKELY QUESTION: "What if there are no ERDs?"
A: Reverse-engineer them. Interview the domain experts, look at foreign key constraints in the DDL, or trace the data lineage from application code. It's slower, but the step is not optional.

TRANSITION TO NEXT: "And to stitch things together safely, we use normalization."
-->
