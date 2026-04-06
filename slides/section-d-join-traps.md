---
layout: center
class: text-center
---

# And then... we JOIN

*(Things go south.)*

---

# JOINs are lossy — they destroy relationship directionality

- **INNER JOIN + NULL FK/PK**: rows silently disappear
- **OUTER JOIN**: row count explodes unexpectedly
- **All JOINs**: the PK → FK direction is gone from the result

Rule: **know your cardinality before you JOIN**.

<!--
Here's the painful truth about JOINs: they're lossy.
The moment you JOIN two tables, you lose something.
If you use an inner join and a foreign key is null — which happens more often than you think in real-world data — that row simply disappears. No error. No warning. Just missing data.
If you use an outer join and the cardinality isn't what you expected, your row count can quietly explode.
And in both cases, the directionality you carefully defined in your ERD — the PK to FK arrow — is gone. The joined table doesn't know which entity owns the relationship anymore.
The rule is simple: understand your cardinality before you write the JOIN. If you don't, you're flying blind.
-->

---

# Fan and Chasm traps silently corrupt your aggregations

**FAN TRAP** — facts fan out and double-count:

`Sales (FK) >---+ Products (PK)` → joining fans product data across all sales rows → sum double-counts

**CHASM TRAP** — rows vanish on both sides:

`Skills (FK) >---+ Users (PK) +---< Languages (FK)` → users without skills AND languages disappear entirely

<!-- TODO: Visual - HIGH PRIORITY
Type: Mermaid ER diagram — two subgraphs showing FAN and CHASM patterns
-->

<!--
The Fan Trap and the Chasm Trap are the two most common join disasters in analytics engineering.
The Fan Trap happens when you join a fact table through a one-to-many relationship.
Take Sales joined to Products. Products has one row per product. Sales has many rows per product.
When you join them, the product attributes get fanned out across all the sales rows.
Now if you sum the sales amount, it looks correct. But if you try to sum any product-level metric, you'll double-count every single time.
The Chasm Trap is subtler. You have Users in the middle, with Skills on one side and Languages on the other.
Users without skills survive the first join. Users without languages survive the second join.
But when you join all three, the users without both simply vanish.
Both traps produce wrong numbers silently. No errors. No warnings. Just incorrect analytics that nobody catches until a stakeholder asks the right question.
-->

---

# Loops in JOIN chains make results unpredictable

Given **n tables**: at most **n − 1 relationships** without forming a loop.

Extra relations create **ambiguous JOIN paths**:
- SQL engines choose a path — you might not like which one
- Often introduced silently when building OBTs with n+1 relations

*Map the full graph before you JOIN.*

<!--
The Loop trap is less famous than Fan and Chasm, but just as dangerous.
Here's the rule: if you have n tables in your query, you can have at most n minus one relationships between them before you create a cycle.
When a loop forms, there are multiple valid JOIN paths between your tables. The SQL engine will pick one, but it might not be the one that gives you the answer you expect.
This happens surprisingly often when building OBTs. You add one more join to enrich the data, and without realizing it, you've created a cycle.
The fix is to map the full graph before you start joining. Draw it out. Make sure it's a directed acyclic graph — no cycles.
The dissection framework helps here too: once you know which columns are keys and which are foreign keys, you can trace the relationships and spot potential loops before they cause problems.
-->

---

# UNION BRIDGES solve what JOINs cannot

JOIN → columns side-by-side (loses direction)

UNION ALL → rows stacked underneath (preserves direction)

**The pattern**:
1. Build a **bridge table** with `UNION ALL`
2. JOIN other tables to the bridge — not to each other

Solves **FAN TRAP** · **CHASM TRAP** · **LOOPS**

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw — side-by-side comparison of JOIN result vs UNION BRIDGE result
Show: same two source tables, different approach, different row counts and structure
-->

<!--
So how do we escape the JOIN traps? UNION BRIDGES.
Here's the key insight: JOIN and UNION ALL are fundamentally different operations.
JOIN puts columns side by side. It creates new combinations of rows. It loses directionality.
UNION ALL stacks rows underneath each other. It preserves every row from every source. It maintains structure.
The UNION BRIDGE pattern builds a bridge table using UNION ALL first, then joins other tables to it — the bridge becomes your single anchor point.
This preserves the directionality of your relationships. It's immune to the Fan Trap because you're not fanning through a join.
It avoids the Chasm Trap because every row from every source is preserved.
And it breaks loops because the bridge is the one stable connection point.
It's a simple pattern. And it works.
-->
