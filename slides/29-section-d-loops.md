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

</div>
