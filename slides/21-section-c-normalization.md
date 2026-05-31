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

<!--
TIMING: 50 seconds

"Normalization isn't just a tool for breaking tables apart — it can also be used to put them together safely."

"There are seven or so normal forms in the literature. For analytical processes, two are enough

"1NF one atomic value per column. No comma-separated lists in a field. No duplicate rows. Just a composition of primary keys that defines the grain of each row."

"Second normal form: every non-key attribute depends on the ENTIRETY of the primary key. 

Take a composite key like sales + products + shipment. Not every attribute will depend on all three. The fix: hash the composite key into a single surrogate key, using the technique from the keys slide."

"These two steps protect you from insertion and deletion anomalies — the classic data corruption bugs." 

"More on this later"

Aside: "Data update anomalies are another story, but in OLAP you typically don't care much about those — you're usually appending, not updating."

TRANSITION TO NEXT: "Now: the modeling step."
-->
