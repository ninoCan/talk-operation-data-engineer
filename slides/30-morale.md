---
layout: center
---

# The dissection framework: your new lens 🤓

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — fully labeled cheat sheet version)
Same color-coded OBT as the full-reveal slide, now with role labels and legend
Keys: blue · Dimensions: green · Facts: orange · Temporal: purple · Technical: grey · Aggregations: red
Make it clean, legible, screenshot-worthy — this is the takeaway slide.
-->

1. **Classify** before you touch —> 6 roles
2. **Model** with Star/Snowflake —> let the dissection guide the structure
3. **Bridge** before *JOIN* to preserve proper cardinality & hierarchy

<!--
Here's your cheat sheet.
The same color-coded table from earlier — but now fully labeled with the role of every column.
Three rules to take home:
One: classify before you touch. Before you write a single line of transformation code, know what every column is. Keys first, then dimensions, then facts.
Two: let the dissection guide your modeling. Your key columns show you where the entities are. Your dimension columns tell you what dimension tables to create. Your fact columns tell you what to measure.
Three: when your JOINs behave badly — rows disappearing, counts doubling — reach for UNION BRIDGES. They solve what JOINs can't.
And if you want to go deeper, Anti-Fact models handle the cases where the absence of an event is what you need to capture. Churn detection, gap analysis. Ask me in Q&A.
-->

