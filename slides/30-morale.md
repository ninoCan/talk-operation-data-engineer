---
layout: center
---

# The dissection framework: your new lens 🤓

1. **Classify** before you touch —> 6 roles
2. **Model** with Star/Snowflake —> let the dissection guide the structure
3. **Bridge** before *JOIN* to preserve proper cardinality & hierarchy

<!--
TIMING: 40 seconds

"Here's what to take home."

"One: classify before you touch. Know your six column types before you write a single line of pipeline code. Ambiguity in column roles is the root of most OBT bugs."

"Two: model the journey. Build an internal hierarchy of tables that leads deliberately to the One Big Table. Don't flatten everything in one shot."

"Three: bridge before you join. Reach for a union bridge before you reach for a join. The bridge preserves cardinalities. The join erases them."

"Next time you open a schema that looks completely opaque... you'll have a framework. And it won't feel so scary."

TRANSITION TO NEXT: "That's the scalpel. Now go use it."
-->
