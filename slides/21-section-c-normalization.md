---
layout: center
class: text-center
---

# Normalization can be used to to stitch entities
Two levels are fine for OLAP.

**1NF**: one atomic value per column; no duplicate rows

**2NF**: every non-key attribute depends on the *entire* PK

`->` *Practical move*: replace composite keys with a **surrogate key**

These steps guarantee protection from **insertion** and **deletion** anomalies.


<!--
Normalization gets a bad reputation in analytics engineering. People think it means painful, risky table rewrites.
It doesn't have to be.
First Normal Form: one value per cell, no duplicate rows. Simple.
Second Normal Form: every attribute must depend on the whole primary key, not just part of it.
The practical implication: if you have a composite key, swap it for a surrogate key. This satisfies 2NF mechanically and makes your table safer.
And here's where the dissection framework pays off: instead of staring at 90 columns wondering where to start, you've already classified everything.
You know which columns belong to which entity. You can make precise, targeted changes.
That's not demolition. That's surgery.
-->
