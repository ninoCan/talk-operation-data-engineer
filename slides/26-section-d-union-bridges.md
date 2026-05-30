---
layout: two-cols-header
class: text-center
---

::default::

# UNION BRIDGES

Solve what JOINs cannot

::left::

JOIN → entities side-by-side (loses directionality)

UNION → stacked underneath (preserves direction)

<div style="text-align: left">

**The pattern**:
1. Build a **bridge table** with `UNION ALL`
2. JOIN other tables to the bridge — not to each other

</div>

Solves **FAN TRAP** · **CHASM TRAP** · **LOOPS**

::right::

<div style="height: 30vh; overflow: hidden; display: flex; justify-content: center;">
  <img src="../assets/union-bridge.png" style="mix-blend-mode: multiply;">
</div>
