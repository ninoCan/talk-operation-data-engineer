---
layout: center
class: text-center
transition: none
---

<img src="../assets/table-full-color.png" class="table-reveal">

#  Aggregations & Technical fields
<br>

**Aggregations**: integrate measures over time grains. EG. `mean()`, `sum()`, `window()`

**Technical Fields**: metadata or used for operations. EG. `created_at`, `batch_id`, `source`, `is_deleted`

*These might be created by you!*

<!--
TIMING: 40 seconds

"Here's the key point: measures and time granularities get aggregated together. Mathematical operations are performed on them

"The result is an aggregation column. And the critical thing here is that these don't exist in your source. YOU create them. They're the output of your pipeline logic."

"Then there are technical fields. These are metadata: columns used for debugging or joining tables together. created_at, batch_id, source, is_deleted. Often null in production records. They serve the PIPELINE, not the analyst."

 if you confuse a technical field for a measure, you'll aggregate something that was never meant to be aggregated."
-->
