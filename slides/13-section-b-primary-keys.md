---
layout: center
class: text-center
transition: none
---

<img src="../assets/table-principal-keys.png" class="table-reveal">

# Principal Keys *define the identity and shape of your data*

**Business Key**: natural identifier (`order_id`, `sale_id`, `product_id`)

<v-click>

**Candidate Key**: alternative identifier that could serve as PK

</v-click>
<v-click>

**Composite Key**: multiple columns together form the PK

</v-click>
<v-click>

**Surrogate Key**: synthetically generated (`uuid`, `integer`, `hash`)

<!--
TIMING: 50 seconds

Click through the types of keys. This is foundational — slow down, be precise.

"First: principal keys. These are your business entities — the qualifiers that identify the object you're researching."

Click — Business Key: "The natural identifier from the business domain. order_id, sale_id, product_id — things that already exist and mean something in the real world."

Click — Candidate Key: "There might be more than one column that could serve as a primary key. Hence: candidate."

Click — Composite Key: "You can combine more than one into a composite key. order_id + product_id together uniquely identify a line item."

Click — Surrogate Key: "And you can collapse those many columns into a single synthetic key. A UUID, an auto-incrementing integer — or, my recommendation: a deterministic HASH of the columns that form the composite key."

"Why a hash? Because it's idempotent. Run the pipeline twice, you get the same key. That is critical for safe reruns — and you WILL rerun pipelines."

TRANSITION TO NEXT: "Keys tell us WHAT we're looking at. Dimensions tell us WHO and WHERE."
-->

</v-click>
