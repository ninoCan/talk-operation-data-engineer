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

