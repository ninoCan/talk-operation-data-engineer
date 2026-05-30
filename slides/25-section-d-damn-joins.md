---
layout: center
class: text-center
---

# JOINs are tricky

<div style="text-align: left">

#### **INNER/LEFT/RIGHT JOIN** can both:
- lose records when one between the FK or PK is absent `→ missing data`
- explode rows when modelling n-m relationships `→ duplicates`
#### **OUTER JOIN**: can mitigate these problems but...
#### **All JOINs**: erase the PK → FK direction from the result

<!--
TIMING: 50 seconds

ENERGY HOOK (late in talk, re-engage): "Joins. The bane of every data engineer's existence. Or at least, the source of 80% of the bugs."

"Joins are tricky. They have side effects that are easy to miss."

"INNER, LEFT, RIGHT joins can all lose records — whenever a foreign key has no matching primary key in the other table. That's missing data."

"Or they can CREATE duplicates — whenever a key matches multiple rows. That's inflated data."

"You might say: outer joins fix this, right? Partially. But all joins share one fundamental problem: they erase directionality."

"When you join two rows, you place columns side by side. The original relationship — which table owned the key, which was the child — disappears. The schema no longer tells you. Six months later, when someone new opens this table, that context is gone."

TRANSITION TO NEXT: "There's a better way. Union bridges."
-->

</div>
