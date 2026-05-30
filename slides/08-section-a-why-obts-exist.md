---
layout: center
class: text-center
---

# One table, many entities: a recipe for the confusion

OBTs often mix **multiple entities** in every row.

<v-click>

Horizontal redundancy is acceptable

</v-click>
<v-click>

Denormalization can silently **generate duplicates**...

</v-click>

<v-click>

...or can facilitate **data loss**

*The table isn't wrong. It's opaque.*
</v-click>

<!--
TIMING: 50 seconds

The core insight: one row encodes many entities. Name it clearly.

"What makes these tables confusing is that one row often encodes many different entities. And many entities means many potential sources of confusion — and a lot of redundant data."

Click 1 — Horizontal redundancy: "Columns that look the same, stacked side by side — that's fine! Horizontal repetition is NOT the problem."

Click 2 — Duplicates: "The problem is when repetition appears ACROSS rows. Row-level duplicates create multiple sources of truth. Which one do you trust?"

Click 3 — Data loss: "Even worse: denormalization makes it dangerously easy to silently lose data. And I mean silently — no error, no warning, just fewer rows than there should be."

Land on: "The table isn't wrong. It's opaque."

TRANSITION TO NEXT: "There's another reason these are hard to get right. Tables are leaky abstractions."
-->
