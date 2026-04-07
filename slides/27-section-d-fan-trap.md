---
layout: center
class: text-center
---

# Fan Traps explode your measures

**FAN TRAP** — facts fan out and double-count:

`Sales (FK) >---+ Products (PK)` → joining fans product data across all sales rows → sum double-counts

<!--
The Fan Trap happens when you join a fact table through a one-to-many relationship.
Take Sales joined to Products. Products has one row per product. Sales has many rows per product.
When you join them, the product attributes get fanned out across all the sales rows.
Now if you sum the sales amount, it looks correct. But if you try to sum any product-level metric, you'll double-count every single time.
-->
