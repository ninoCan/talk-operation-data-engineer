---
layout: center
class: text-center
---

# The Dissection

*Time to pick up the scalpel.*

---

# Every column plays a role — most schemas ignore this

Six functional roles in every OBT:

| Role | Color | What it represents |
|------|-------|--------------------|
| Keys | 🔵 blue | Identity & relationships |
| Dimensions | 🟢 green | Entity attributes |
| Facts | 🟠 orange | Numeric metrics |
| Temporal | 🟣 purple | Time grain |
| Technical | ⚫ grey | Operational metadata |
| Aggregations | 🔴 red | Pre-computed summaries |

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — raw state, undifferentiated)
Place the color legend above or beside the raw OBT grid
Why: Prepares the audience for the progressive color reveal on following slides
-->

<!--
Here's the insight that changes everything: not all columns in a table are the same.
I know that sounds obvious. But look at a 90-column OBT and tell me which columns are keys, which are metrics, which are metadata.
You can't tell at a glance. And that's the problem.
Every OBT I've encountered contains exactly six types of columns. Six functional roles.
Once you learn to see them, you can read any table.
Let me walk you through each one. As I do, watch the color-coded grid appear behind the chaos.
-->

---

# Keys define the identity and shape of your data

- **Business Key** — natural identifier (`order_id`, `user_email`, `product_sku`)
- **Candidate Key** — alternative identifier that could serve as PK
- **Composite Key** — multiple columns together form the PK
- **Surrogate Key** — system-generated (`uuid`, auto-increment integer)

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — key columns highlighted blue)
Progressive reveal: same grid, blue columns light up
-->

<!--
Let's start with keys — the columns that define identity and relationships.
The business key is the natural identifier. The thing your business actually talks about.
An order ID. A user email. A product SKU. These exist in your domain before the database does.
The surrogate key is the system-generated one — a UUID or an auto-incrementing integer. It exists for the database, not for the business.
The composite key is when you need multiple columns together to uniquely identify a row. Very common in time-series tables — you need both the entity ID and the timestamp.
And the candidate key is any column that could be the primary key, but isn't the chosen one.
When you're reading an OBT, keys are your anchor points. Find them first. They tell you what entity the row belongs to.
-->

---

# Dimensions describe the state of an entity over time

- Attributes of an entity — the **who** and the **what**
- Subject to change: Slowly Changing Dimensions (SCD)
- Types: **simple** (flat value) · **composite** (multi-field) · **business** (carries domain meaning)

*Examples: `customer_country`, `product_category`, `channel_name`*

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — dimension columns highlighted green)
-->

<!--
Dimensions describe attributes — the who, what, and where.
Your customer's country. Your product's category. The sales channel.
The key thing about dimensions is that they evolve. Your customer might change their country. Your product might change its category.
This is what Slowly Changing Dimensions — SCDs — are about.
When you see a column in an OBT that describes a property of an entity, that's a dimension.
And here's why this matters for dissection: if you see the same entity attributes repeated across rows with different timestamps, you're looking at an SCD in a denormalized form.
That's a sign you can normalize it back into a proper dimension table.
-->

---

# Facts capture *what happened*; temporal captures *when*

**Facts / Measures** — point-in-time numeric metrics:
`revenue`, `quantity_sold`, `session_duration`

**Temporal** — the time grain of the row:
- 1NF: one unique granularity per row
- OLAP: multiple grains possible (weekly · monthly · quarterly)

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — facts orange, temporal purple)
-->

<!--
Facts are the numeric metrics — the things you measure.
Revenue, quantities, durations, counts. They're point-in-time: they represent what happened at a specific moment.
Temporal columns define the grain — the level of detail the row represents.
In a well-normalized table, each row has one temporal granularity.
But in an OBT, you often find multiple: a daily revenue column AND a monthly revenue column AND a year-to-date column, all in the same row.
That's a sign the table is mixing temporal granularities — which makes it much harder to aggregate correctly.
When you spot this pattern, you've found your first major structural issue. Flag it.
-->

---

# Technical fields and aggregations are not business data

**Technical Fields** — metadata for operations, not meaning:
`created_at`, `batch_id`, `_source`, `_etl_version`, `is_deleted`

**Aggregations** — pre-computed summaries embedded in the table:
`total_revenue_ytd`, `avg_order_value_30d`

They **reduce normality** — use them deliberately.

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

---
layout: center
---

# The hidden schema was there all along

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — FULLY color-coded)
Keys: blue · Dimensions: green · Facts: orange · Temporal: purple · Technical: grey · Aggregations: red
This is the anchor visual — the full reveal of the dissection framework.
Make it large, clear, readable from the back of the room.
-->

*Same table. Now you can read it.*

<!--
Here it is. The same table we showed at the beginning.
Ninety columns. But now they have colors. Now they have roles.
The blue columns are your keys — your identity anchors.
The green columns are your dimensions — your entity attributes.
The orange columns are your facts — your numeric metrics.
The purple columns are your temporal markers — your time grain.
The grey columns are your technical fields — operational metadata.
The red columns are your aggregations — pre-computed summaries.
Nothing about the table changed. But now you can read it.
That's the dissection framework.
-->
