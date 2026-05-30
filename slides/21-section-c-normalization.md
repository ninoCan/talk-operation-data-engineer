---
layout: center
class: text-center
---

# Normalization can be used to stitch entities
Two levels are fine for OLAP.

**1NF**: one atomic value per column; no duplicate rows

**2NF**: every non-key attribute depends on the *entire* PK

→ *Practical move*: replace composite keys with a **surrogate key**

These steps guarantee protection from **insertion** and **deletion** anomalies.
