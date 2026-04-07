---
layout: center
class: text-center
---

# UNION BRIDGES solve what JOINs cannot

JOIN → columns side-by-side (loses direction)

UNION ALL → rows stacked underneath (preserves direction)

**The pattern**:
1. Build a **bridge table** with `UNION ALL`
2. JOIN other tables to the bridge — not to each other

Solves **FAN TRAP** · **CHASM TRAP** · **LOOPS**

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw — side-by-side comparison of JOIN result vs UNION BRIDGE result
Show: same two source tables, different approach, different row counts and structure
-->

<!--
So how do we escape the JOIN traps? UNION BRIDGES.
Here's the key insight: JOIN and UNION ALL are fundamentally different operations.
JOIN puts columns side by side. It creates new combinations of rows. It loses directionality.
UNION ALL stacks rows underneath each other. It preserves every row from every source. It maintains structure.
The UNION BRIDGE pattern builds a bridge table using UNION ALL first, then joins other tables to it — the bridge becomes your single anchor point.
This preserves the directionality of your relationships. It's immune to the Fan Trap because you're not fanning through a join.
It avoids the Chasm Trap because every row from every source is preserved.
And it breaks loops because the bridge is the one stable connection point.
It's a simple pattern. And it works.
-->
