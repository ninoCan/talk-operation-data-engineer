---
layout: center
---

# Outline: Action Plan

<div style="text-align: left">

0. **Collect** the business requirements
1. **Start** with ERD datasets, time-series, and data marts as inputs
2. **Re-normalize** if upstream is denormalized, dissect first
3. **Choose a model**: Star · Snowflake · Data Vault · Anchor
4. **Analyse** the cardinalities of relationships
5. **Glue** tables together into a OBT

<!--
TIMING: 50 seconds

ENERGY HOOK: "Alright — this is the part you take home. Five steps, in order."

Step 0 — Collect requirements: "First: talk to your stakeholders. Actually understand their need, not the one you assume they have. A wrong assumption here creates headaches all the way downstream. Uncomfortable step? Yes. Non-negotiable? Also yes."

Step 1 — Survey inputs: "Then you survey your inputs: entity relationship diagrams, time series, data marts. Understand what's already there and what shape it's in."

Step 2 — Re-normalize: "If you receive denormalized tables upstream, re-normalize them before doing anything else. Denormalization is great for dashboards. It is terrible as an intermediate processing step. Dissect first, then rebuild."

Step 3 — Choose a model: "Star, Snowflake, Data Vault, Anchor. We'll cover star schema in a minute."

Step 4 — Analyze cardinalities: "This step is often skipped. Don't skip it. The cardinalities of your relationships will determine exactly how you stitch everything together in the final step."

Step 5 — Glue: "Then — and only then — you build the OBT."

TRANSITION TO NEXT: "Let's walk through the inputs, starting with ERDs."
-->

</div>
