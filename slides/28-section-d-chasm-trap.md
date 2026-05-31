---
layout: center
class: text-center
---

# Chasm traps explode quadratically

**CHASM TRAP**: joining multiple satellite dimensions explodes rows

<div style="height: 30vh;overflow: hidden; display: flex; justify-content: center;">
  <img src="../assets/chasm-trap.png" style="mix-blend-mode: multiply;">
</div>

- wrong join generates `#(SKILLS) * #(JOBS)` rows → **Quadratic**

- with union we have `#(SKILLS) + #(JOBS)` rows → **Linear**

<!--
TIMING: 50 seconds

"The chasm trap. It happens when two satellite dimensions are both joined back to a central fact."

"Think of a LinkedIn profile — everyone here has one. A user has several skills. The same user has several jobs. Now you want one reporting table that denormalizes all three."

"With plain joins, the worst case is a Cartesian product. If a user has n skills and m jobs, you end up with n × m rows. That's QUADRATIC growth."

"With union bridges, each row appears exactly once. At most n + m rows. LINEAR growth."

"At small scale this is invisible. At large scale — millions of users, dozens of skills, dozens of jobs — you've just created a table that explodes your compute budget and corrupts your aggregations. The joins looked correct. The numbers looked plausible. The table was wrong."
-->
