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

Denormalization silently **generate duplicates**

</v-click>

<v-click>

And can **lose** data


*The table isn't wrong. It's opaque.*
</v-click>

<!--
Let's start with the fundamentals. A table in First Normal Form — 1NF — simply means each column holds one atomic value and there are no duplicate rows. That's it. That's the only constraint.
So a 1NF table can contain your customers, their orders, their products, and their purchase history — all in one row, repeated for every combination.
The table isn't technically wrong. It satisfies 1NF.
But it's opaque — you can't see the entities inside it without looking carefully.
And when you denormalize, it becomes easy to hide rows or create unintended duplicates without any visible error.
That's what we're going to learn to see.
-->


