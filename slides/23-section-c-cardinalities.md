---
layout: center
class: text-center
---

# Know the numbers

<div style="height: 30vh; overflow: hidden; display: flex; justify-content: center;">
  <img src="../assets/cardinalities.png" style="mix-blend-mode: multiply;">
</div>

### M-N relationship are not well-behaved in the relational model

**Association Table/Bridge** contains PK & FK forming a composite key → can surrogate

<!--
TIMING: 50 seconds

ENERGY HOOK: "Cardinalities. The step everyone skips. Don't be everyone."

"One-to-one: wonderfully clean. One country, one capital. No ambiguity."

"One-to-many: generally fine, with a few edge cases we'll get to in the next section."

"Many-to-many: this is where things get troublesome."

"Think of citizens and countries. A citizen can hold multiple nationalities. A country has many citizens. If you naively join these, you get a mess."

"The solution: introduce a bridge table — also called an association table — that breaks the many-to-many into a chain of one-to-many relationships. Think of the passport: it gives each citizen a well-defined nationality in a specific context. The bridge table is built by stitching together the primary and foreign keys of both original tables into a new one — and you can surrogate that composite key."

HUMOR: "Many-to-many: the relationship status that even databases find complicated."

TRANSITION TO NEXT: "Alright. We've dissected, modeled, analyzed. Now we join."
-->
