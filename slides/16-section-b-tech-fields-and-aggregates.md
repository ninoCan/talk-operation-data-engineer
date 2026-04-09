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

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — technical grey, aggregations red)
-->

<!--
Technical fields are the columns the business doesn't care about but engineering does.
When was this row inserted? Which batch process created it? Is this a soft delete?
Critical for operations and debugging — but they carry no business meaning.
Aggregations are the pre-computed summaries — year-to-date revenue, 30-day moving averages.
They exist for performance: instead of computing them at query time, someone stored the result directly in the table.
But here's the trap: aggregations embed assumptions about query patterns.
If those patterns change, the aggregations become stale — or worse, silently wrong.
When you're dissecting an OBT, find the aggregation columns and ask: who computed these? When? Are they still valid?
-->
