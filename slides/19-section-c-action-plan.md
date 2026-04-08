---
layout: center
---

# Outline: Action Plan

<div style="text-align: left">

0. **Collect** the business requirements
1. **Start** with ERD datasets, time-series, and data marts as inputs
2. **Re-normalize** if upstream is denormalized — dissect first
3. **Choose a model**: Star · Snowflake · Data Vault · Anchor
4. **Analyse** the cardinalities of relationships
5. **Glue** tables together into a OBT

</div>

<!--
Before we go into the mechanics, let me give you the map.
When you're tasked with building an OBT, you rarely start from nothing.
You have upstream data — ERD-modeled datasets, time-series tables, maybe other data marts.
The dissection framework tells you how to read those inputs.
Then you re-normalize what needs normalizing, choose a modeling approach that fits your use case, and build the OBT as the final output — not the starting point.
Let's walk through the key pieces.
-->
