---
layout: default
---

# FAN TRAP in PySpark: Aggregating After a Join

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: join first, aggregate second
# orders has 1 row per order; payments has N rows per order
result = (
    orders
    .join(payments, "order_id", "left")
    .groupBy("customer_id")
    .agg(F.sum("order_amount").alias("total_revenue"))
    # order_amount is duplicated once per payment row... silent inflation
)

# ✅ RIGHT: aggregate on the fact table before joining
order_totals = (
    orders
    .groupBy("customer_id")
    .agg(F.sum("order_amount").alias("total_revenue"))
)
result = order_totals.join(payments.groupBy("order_id").count(), "order_id", "left")
```

</div>

`order_amount` gets duplicated once per payment row.
Your revenue report is silently inflated.
Always aggregate on the grain before joining across cardinalities.
 
---
layout: default
---

# FAN TRAP in PySpark: Non-Unique Join Key

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: joining on a natural key that is not the grain
# product_category is shared across many products; this fans out silently
result = (
    sales
    .join(products, "product_category", "left")
    .groupBy("region")
    .agg(F.sum("revenue").alias("total_revenue"))
    # every sale is now duplicated once per product in that category
)

# ✅ RIGHT: join on the true grain key
result = (
    sales
    .join(products, "product_id", "left")
    .groupBy("region")
    .agg(F.sum("revenue").alias("total_revenue"))
)
```

</div>

`product_category` is a dimension attribute, not a grain key.
Joining on it fans one sale row into many...
Always verify key uniqueness with `.dropDuplicates(["join_key"]).count()`

---
layout: default
---

# CHASM TRAP in PySpark: Cartesian Explosion

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: joining two independent dimensions to the same fact via plain joins
# users have N skills and M jobs; neither dimension knows about the other
result = (
    users
    .join(skills, "user_id", "left")   # fans out to N rows per user
    .join(jobs,   "user_id", "left")   # fans out each of those N rows by M
    # outcome: N × M rows per user... quadratic explosion
)

# ✅ RIGHT: union both satellites into a bridge, then join once
skills_bridge = skills.select("user_id", F.lit("skill").alias("type"), "skill_name".alias("value"))
jobs_bridge   = jobs.select("user_id",   F.lit("job").alias("type"),   "job_title".alias("value"))

bridge = skills_bridge.unionByName(jobs_bridge)

result = bridge.join(users, "user_id", "left")
```

</div>

Skills and jobs are independent; neither determines the other's grain.
Joining both to `users` produces N × M rows per user.
Stack them with a union bridge; growth becomes N + M.

---
layout: default
---

# CHASM TRAP in PySpark: Nulls Masking

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: outer joins hide the explosion behind nulls... until aggregation
# articles have N tags and M authors; neither is mandatory
result = (
    articles
    .join(tags,     "article_id", "left")
    .join(authors,  "article_id", "left")
    .groupBy("category")
    .agg(F.countDistinct("article_id").alias("article_count"))
    # countDistinct might look fine... but sum or avg on any metric won't be
)

# ✅ RIGHT: aggregate each satellite independently, then join the summaries
tag_counts    = tags.groupBy("article_id").agg(F.count("tag").alias("num_tags"))
author_counts = authors.groupBy("article_id").agg(F.count("author_id").alias("num_authors"))

result = (
    articles
    .join(tag_counts,    "article_id", "left")
    .join(author_counts, "article_id", "left")
    .groupBy("category")
    .agg(F.sum("num_tags").alias("total_tags"), F.sum("num_authors").alias("total_authors"))
)
```

</div>

<!-- Outer joins don't prevent the explosion; they just pad the extra rows with nulls.
`countDistinct` may survive it... `sum` and `avg` won't.
Pre-aggregate each satellite to the fact grain before joining. -->

---
layout: default
---

# CHASM TRAP in PySpark: Fact-to-Fact Join

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: joining two fact tables directly on a shared dimension key
# invoices: 1 row per invoice; shipments: N rows per invoice (one per package)
result = (
    invoices
    .join(shipments, "order_id", "left")
    .agg(F.sum("invoice_amount").alias("total_billed"))
    # invoice_amount is now duplicated for every shipment package
)

# ✅ RIGHT: resolve through the shared dimension, not directly
# Pre-aggregate shipments to the invoice grain first
shipment_summary = (
    shipments
    .groupBy("order_id")
    .agg(F.count("package_id").alias("num_packages"))
)
result = (
    invoices
    .join(shipment_summary, "order_id", "left")
    .agg(F.sum("invoice_amount").alias("total_billed"))
)
```

</div>

<!--
Two fact tables should never be joined directly.
Always resolve through a shared dimension or pre-aggregate to the coarser grain.
The chasm trap hides in plain sight whenever two facts share a key.
-->

---
layout: default
---

# LOOP TRAP in PySpark: Ambiguous Join Path

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: three tables, three relationships... a cycle exists
# sales → products (via product_id)
# sales → shipments (via shipment_id)
# shipments → products (via product_id)  ← closes the loop

# Path A: sales → shipments → products
result_a = (
    sales
    .join(shipments, "shipment_id", "left")
    .join(products,  "product_id",  "left")
)

# Path B: sales → products → shipments
result_b = (
    sales
    .join(products,  "product_id",  "left")
    .join(shipments, "shipment_id", "left")
)

# result_a and result_b are NOT equivalent... which one is correct?
# The answer is business-dependent; the engine cannot decide for you.
```

</div>

<!-- --- -->
<!-- layout: default -->
<!-- --- -->

<!-- <div class="code-left"> -->

<!-- ``` -->
<!-- # ✅ RIGHT: break the loop by picking one authoritative path explicitly -->
<!-- # and document why -->
<!-- result = ( -->
<!--     sales -->
<!--     .join(shipments, "shipment_id", "left")   # shipment owns the delivery context -->
<!--     .join(products,  "product_id",  "left")   # product details hang off the shipment -->
<!-- ) -->
<!-- ``` -->

<!-- </div> -->

<!-- Three relationships among three tables... a loop is guaranteed. -->
<!-- Two valid paths exist; they produce different row sets. -->
<!-- Pick one path explicitly and document the business reason; never let the engine guess. -->

---
layout: default
---

# LOOP TRAP in PySpark: Shared Entity On a Cycle

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: employees appear in both projects and departments
# projects → employees (via employee_id, project lead)
# departments → employees (via employee_id, department head)
# projects → departments (via department_id)  ← closes the loop

# Joining all three fans rows out unpredictably
result = (
    projects
    .join(departments, "department_id", "left")
    .join(employees,   "employee_id",   "left")
    # employee_id exists in both projects and departments...
    # which employee_id drives the join to employees?
    # Spark will silently pick one; it may not be the one you intended.
)
```

</div>

<!-- # ✅ RIGHT: resolve each relationship independently, alias to break ambiguity -->
<!-- project_leads = ( -->
<!--     employees -->
<!--     .select(F.col("employee_id"), F.col("name").alias("lead_name")) -->
<!-- ) -->
<!-- dept_heads = ( -->
<!--     employees -->
<!--     .select(F.col("employee_id"), F.col("name").alias("head_name")) -->
<!-- ) -->

<!-- result = ( -->
<!--     projects -->
<!--     .join(project_leads, projects["lead_employee_id"] == project_leads["employee_id"], "left") -->
<!--     .join(departments,   "department_id", "left") -->
<!--     .join(dept_heads,    departments["head_employee_id"] == dept_heads["employee_id"], "left") -->
<!-- ) -->
<!-- ``` -->

<!-- </div> -->

The same `employees` table plays two roles; one as project lead, one as department head.
Joining on `employee_id` without aliasing lets Spark pick the path silently.
Resolve each role into a named alias; make every relationship explicit.

---
layout: default
---

# LOOP TRAP in PySpark: SCD on a Temporal Cycle

<div class="code-left">

```python
from pyspark.sql import functions as F

# ❌ WRONG: a slowly changing dimension creates a hidden loop
# sales → customers (via customer_id, current snapshot)
# sales → dates (via date_id)
# customers → dates (via valid_from / valid_to)  ← SCD type 2 closes the loop

# Joining all three without anchoring the time context is ambiguous
result = (
    sales
    .join(customers, "customer_id", "left")   # which version of the customer?
    .join(dates,     "date_id",     "left")   # date of the sale... or the SCD window?
    # two paths to dates; the SCD window and the transaction date conflict
)
```

</div>

<!-- # ✅ RIGHT: anchor the SCD join explicitly to the transaction date -->
<!-- # breaking the temporal loop before it forms -->
<!-- result = ( -->
<!--     sales -->
<!--     .join( -->
<!--         customers, -->
<!--         (sales["customer_id"] == customers["customer_id"]) & -->
<!--         (sales["sale_date"]   >= customers["valid_from"])  & -->
<!--         (sales["sale_date"]   <  customers["valid_to"]), -->
<!--         "left" -->
<!--     ) -->
<!--     .join(dates, sales["date_id"] == dates["date_id"], "left") -->
<!-- ) -->
<!-- ``` -->

<!-- </div> -->

A type-2 slowly changing dimension links both to the fact and to a date range.
Two paths to `dates` exist; the transaction date and the SCD validity window.
Anchor the SCD join to the transaction date explicitly; collapse the loop before it opens.

---
layout: default
---

# UNION BRIDGE in PySpark

<div class="code-left">

```python
from pyspark.sql import functions as F

# 1. Build the bridge with UNION ALL (preserves all rows)
bridge = (
    sales.select(F.lit("sales").alias("source"), "sale_id", "product_id")
    .unionByName(F.lit("products").alias("source"), "product_id")
)

# 2. JOIN tables to the bridge — not to each other
result = (
    bridge
    .join(sales, "sales_id", "left")
    .join(products, "product_id", "left")
)
```

</div>

No data loss. No row explosion. No directionality loss.

Fan/Chasm/Loop safe.
