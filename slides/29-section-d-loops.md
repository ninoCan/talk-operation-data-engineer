---
layout: center
class: text-center
---

<div style="height: 30vh;overflow: hidden; display: flex; justify-content: center;">
  <img src="../assets/loop.png" style="mix-blend-mode: multiply;">
</div>

# Loops in JOIN chains make results unpredictable

Given **n tables**: at most **n − 1 relationships** without forming a loop.

<div ml-40 style="text-align: left;">

Extra relations create **ambiguous JOIN paths**:
- SQL engines choose a path, or fails to compile a plan
- Often introduced silently when building OBTs with n+1 relations

<!--
TIMING: 50 seconds

"The loop. Three or more tables connected in a cycle."

"Think of a sale: a customer buys three products, all shipped together in one shipment. You want to join the shipment back to the products for more detail."

"Two paths exist: shipment → sales → products. Or: shipment → products directly."

"This ambiguity leads to silent errors. Some SQL engines cannot determine the correct path. Some fail to compile a query plan at all. The ones that do compile... may pick the wrong path and give you numbers that look plausible but are wrong."

"The rule is simple: with n tables, you can have at most n − 1 relationships before you create a loop. Three tables: two safe relationships. Add a third relationship — loop."

HUMOR: "Three tables, three relationships. Sounds perfectly reasonable. SQL nods, picks a path at random, and hands you confidently wrong results. The worst kind of bug — the one that doesn't fail."

"The union bridge eliminates this: you join everything to the bridge, never to each other, so there's no cycle to form."

TRANSITION TO NEXT: "Let's wrap this up."
-->

</div>

<!--
And the last trap is sneaky because it looks like it should just work.

N tables support AT most N-1 relationships in joins.
-->
