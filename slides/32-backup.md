---
layout: default
---

# UNION BRIDGE in PySpark

<div class="code-left">

```python
from pyspark.sql import functions as F

# 1. Build the bridge with UNION ALL (preserves all rows)
bridge = (
    sales.select("product_id", "customer_id", F.lit("sale").alias("type"), "amount")
    .unionByName(
        returns.select("product_id", "customer_id", F.lit("return").alias("type"), "amount")
    )
)

# 2. JOIN tables to the bridge — not to each other
result = (
    bridge
    .join(products, "product_id", "left")
    .join(customers, "customer_id", "left")
)
```

</div>

No row explosion. No directionality loss. FAN/CHASM/LOOP safe.

---
layout: default
---

# Anti-Fact models & non-conformed granularities

**Anti-Fact tables** — record *absence* of events:

| customer_id | date | no_purchase |
|-------------|------|-------------|
| 42 | 2024-01-15 | true |
| 43 | 2024-01-15 | true |

Use for: churn detection, gap analysis, SLA violations.

**Non-conformed granularities** — Fact tables with incompatible time grains:
- Sales Fact: **daily** grain
- Budget Fact: **monthly** grain

Cannot JOIN directly → aggregate to a common grain first.

---
layout: default
---

# Full 7-role reference card

| Role | Color | Description | Example columns |
|------|-------|-------------|----------------|
| Keys | 🔵 | Identity & relationships | `order_id`, `customer_uuid` |
| Dimensions | 🟢 | Entity attributes | `country_code`, `product_category` |
| Facts | 🟠 | Point-in-time metrics | `revenue`, `quantity_sold` |
| Temporal | 🟣 | Time grain | `order_date`, `week_start` |
| Technical | ⚫ | Operational metadata | `_batch_id`, `created_at`, `is_deleted` |
| Aggregations | 🔴 | Pre-computed summaries | `ytd_revenue`, `avg_30d_value` |
| Indices | 🟡 | Partition/sort keys | `partition_date`, `_sort_key` |

*Indices: partition keys and sort keys in warehouse systems (Parquet, BigQuery, Redshift, Snowflake). From the original talk proposal — included here for completeness.*
