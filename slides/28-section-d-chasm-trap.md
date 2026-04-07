---
layout: center
class: text-center
---

# Chasm traps explode quadratically

**CHASM TRAP** — rows vanish on both sides:

`Skills (FK) >---+ Users (PK) +---< Languages (FK)` → users without skills AND languages disappear entirely

<!--
The Chasm Trap is subtler. You have Users in the middle, with Skills on one side and Languages on the other.
Users without skills survive the first join. Users without languages survive the second join.
But when you join all three, the users without both simply vanish.
Both traps produce wrong numbers silently. No errors. No warnings. Just incorrect analytics that nobody catches until a stakeholder asks the right question.
-->
