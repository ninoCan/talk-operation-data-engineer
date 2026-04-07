---
layout: center
class: text-center
transition: none
---

<img src="../assets/table-principal-keys.png" class="table-reveal">

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
