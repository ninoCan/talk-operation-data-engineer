---
layout: center
class: text-center
transition: none
---

<img src="../assets/table-time-grains.png" class="table-reveal">

# Measures: *what happened*; Temporal Grains: *when*
<br>

**Measures/Facts**: point-in-time numeric metrics (`revenue`, `quantity_sold`, `session_duration`, ...)

**Temporal**: the time grain of the row (`sold_at`, `processing_time`, `year`, ...)

In OLAP: multiple grains possible (weekly, monthly, quarterly, ...)

<!--
TIMING: 40 seconds

"Measures describe what happened. They are point-in-time numeric values — also called facts."

"Revenue at this moment. Quantity sold today. Session duration of this visit. These are things that happened once and were recorded."

"A measure can have more than one time granularity. Weekly revenue is different from monthly revenue, which is different from quarterly revenue. Each is a different grain — and in a properly structured table, each grain gets its own row or its own column, clearly labeled."

"This sounds obvious, but it trips people up constantly. If you mix grains without labeling them, your aggregations will be silently wrong. The SUM of mixed weekly and monthly revenue is not a useful number."

TRANSITION TO NEXT: "And there are two more types that often get overlooked — and these are the ones YOU usually create."
-->
