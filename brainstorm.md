# Brainstorming: Master the Art of Schema Dissection: Operation Data Engineer

**Date**: 2026-04-06
**Status**: Research complete, ready for outline

**Note**: See `presentation-config.md` for presentation parameters (duration, audience, etc.)

---

## Abstract & Commitments

### Original Abstract

> Denormalized tables promise simplicity, but often hide complexity in plain sight. A single table may contain business entities, metrics, time semantics, and technical artifacts: all mixed together behind a deceptively flat schema.
>
> This talk introduces a practical framework for dissecting denormalized tables in 1NF by classifying columns into functional roles: keys, dimensions, facts, temporal granularities, technical columns, aggregates, and indices. This perspective turns tables into understandable systems instead of mysterious collections of fields.
>
> With this mental model, data engineers can spot leaky abstractions early and approach normalization as a deliberate, surgical process rather than a risky rewrite. If you work with Python-based data stacks and messy real-world schemas, this talk will change how you look at tables forever.

### Key Commitments (must deliver on these)

1. **Introduce the one-big-table problem**: Show audience the mess they've likely encountered — one big denormalized table hiding multiple entities, metrics, time semantics, and technical noise.
2. **Deliver the dissection framework**: Classify columns into functional roles — keys, dimensions, facts, temporal granularities, technical columns, aggregates, indices. This is the core content promise.
3. **Make normalization feel achievable**: Show that with the framework, normalization is surgical precision (deliberate, reversible), not a risky rewrite.
4. **Python-stack relevance**: Frame it for Python-based data stacks (such as pyspark, but also pandas, polars...) and real-world messy schemas.

### Tone/Approach

Storytelling-first: lead with a relatable scenario ("you get a giant wide table, what do you do?"), build tension, then deliver the framework as the resolution. Technical details introduced only where they reinforce the story. Light humor in breaker slides.

---

## Research Summary

### Local Materials Review

**Files reviewed**: `proposals/proposal-PyConLT.md`, `proposals/proposal-PyConIT.md`, `assets/mindmap.excalidraw.png`

**Full narrative arc (from mindmap)**:

#### ACT 1 — The Setup

1. **Title slide**
2. **About me** — who am I, what do I do
3. **The story hook**: Your task as a Big Data Engineer — create one big table (OBT). Ask the audience: how many of you have faced this?
4. **Show a wide table**: 90+ columns in denormalized form
5. **The uncomfortable truth**: To feed dashboards and stakeholders, they don't *have* to JOIN — OBT work. Yet, they can be tricky to build.

#### ACT 2 — Understanding the Beast

6. **DON'T PANIC** — let's take a step back and break down the problem
7. **OBT & entities**: OBTs often contain many entities. Horizontal redundancy is OK — but denormalization can hide data and generate duplicates.
8. **Tables are leaky abstractions**: A design purported to handle complexity, but fails to make things simpler and has pitfalls. Should show relations between entities, but requires extra work (indices, normalization, querying) to optimize.
9. **OLTP vs OLAP**:
   - OLTP captures transactions: write-optimized, produces well-normalized tables, almost 1:1 with objects
   - OLAP captures analytics: read-optimized, aggregates data, often denormalizes entities

#### ACT 3 — The Dissection Framework

10. **Columns are not all the same** — the core thesis:
    - **Dimensions**: describe attributes of an entity (subject to evolution over time); composite and business dimensions exist
    - **Primary Keys**: Business K., Candidate K., Composite Ks, Surrogate K. 
    - **Measures / Facts**: represent point-in-time metrics of an entity
    - **Technical Fields**: metadata or non-business attributes that support operations
    - **Temporal**: granularities — dimensions are the foundation, with a single row per unique granularity
    - **Aggregations**: integrate facts over dimensions together, reducing normality

11. **The DATA JOURNEY** — OBT is the outcome, no need to be created from scratch. Typically:
    - Input: ERD modeling & Time Series
    - If upstream datasets are denormalized → re-normalization is a good starting point
    - Create a star/fact with dimensional layer: Fact & Dimension Tables → Star / Snowflake / Data Vault / Anchor modeling

12. **ERDs define an ontology/taxonomy**:
    - Directionality defined via PK → FK
    - Time-series have composite keys
    - These tables describe evolution of a single entity, aggregated over a window to coarser granularities

13. **Normalization reduces redundancy**:
    - 1NF: each column holds a single value, no duplicate rows
    - 2NF: each non-key attribute depends on the entire PK (no need to split tables, move composite keys to a surrogate key?)
    - This should guarantees safety from insertion and deletion anomalies

#### ACT 4 — Star/Snowflake Modeling (Applied Framework)

14. **Star/Snowflake focus** (most common modeling):
    - **Dimension tables**: describe actors and their attributes (e.g., Customers, Products)
    - **Fact tables**: describe events involving actors (e.g., Purchases, Sales)
    - Often only Facts sit between Dimensions (Star), but Dimensions can connect multiple Facts (Snowflake)

15. **Cardinality**:
    - one-to-one (1→1)
    - one-to-many (1→n)

16. **PITFALL — M:N relationships are poorly modeled in RDBMS**:
    - Introduce "association tables" / "bridges" to reduce M:N to (1→n) & (n→1)
    - Bridges contain overlapping PKs from both tables as FKs → composite key

#### ACT 5 — Then We JOIN... and Things Go South

17. **JOIN anomalies** (the crux of the practical problem):
    - Inner/Left/Right JOINs can cause data loss when PK/FK is NULL
    - Full Outer JOINs can explode rows on one side or the other
    - Joining two tables loses the *directionality* of the relationship between entities

18. **UNION BRIDGES comes to the rescue**:
    - JOINS put columns next to each others while UNION ALL puts underneath each other.
    - Solve the problem because they are immune to duplicates
    - Build a Union Bridge, join other tables to it
    - Bridges come to solve what JOINs can't

19. **3 problems** solved:
    - FAN TRAP: one (or more) fact table with a many-to-one relation to another. Example: Sales (FK) >---+ (PK) Products 
    - CHASM TRAP: two dimension tables with a many-to-one relation to a third table. Example: Skills (FK) >---+ (PK) Users +---< (FK) Languages 
    - LOOPS: given n tables there can be at most n-1 relations without them forming a loop


#### ACT 6 — Closing

20. **Bonus: anti-Fact models & non-conformed granularities** (Target & Rules)
21. **Take-aways**
22. **Acknowledgements**

---

## Key Themes & Messages

### Core Themes

1. **One-big tables look simple**: but conceal multiple entities, metrics, time semantics, and technical noise.
    The frustration is valid — it's not obvious chaos, it's hidden complexity.

2. **Column classification is the first-step**: Once you know what each column *is* (dimension, fact, key, temporal, technical, aggregate), the table stops being a mystery. You gain a shared vocabulary and a surgical map.

3. **The data journey is the context**: OBTs don't exist in isolation — they sit on a journey from OLTP to OLAP.
    Understanding where your table lives on that journey helps you decide what to do with it.

4. **Normalization is surgery, not demolition**: With the framework, you can identify exactly which columns belong to which entity, how they relate, and what the safe refactoring path looks like. No more risky big-bang rewrites.

5. **JOINs have traps**: Even after dissection, JOINs are dangerous — NULL FKs, row explosion, lost directionality.
    UNION BRIDGES offer a path forward.

### Key Messages (What Audience Should Remember)

**Primary message**: Every one-big table has a hidden schema — learning to read it is part and parcel of a data engineer.

**Supporting messages**:
1. Classify columns before you touch them (the 7-role framework)
2. JOINs are lossy — design your bridge tables to preserve directionality
3. Intermediate modeling is the proper way to build a denormalized structure that does not create troubles

**Call to action**: Next time you open a OBT, don't scroll through columns in despair — pick up your scalpel.

---

## Visual Opportunities

**Diagrams needed**:

- **Wide table "before"**: A grid/table showing many mixed columns color-coded by role (keys=blue, dimensions=green, facts=orange, technical=grey, temporal=purple, aggregates=red). Already partially designed in `assets/table.excalidraw`.
  - Type: Excalidraw
  - Why: Spatial — shows the chaos at a glance

- **Column classification taxonomy**: A simple hierarchy or legend showing the 7 column roles with descriptions
  - Type: Mermaid / Excalidraw
  - Why: Reference card for the framework

- **OLTP → OLAP journey**: A left-to-right flow showing where wide tables fit
  - Type: Mermaid flowchart
  - Why: Contextualizes the problem in the data pipeline

- **Star schema diagram**: Classic star schema — fact table in center, dimension tables radiating out
  - Type: Mermaid ER or graph
  - Why: Immediately recognizable; shows the target state

- **M:N → Bridge table**: Before/after showing M:N relationship resolved via bridge
  - Type: Mermaid ER
  - Why: Concrete depiction of a common pitfall

- **JOIN anomalies**: Show row explosion / NULL loss visually
  - Type: Simple table diff (Excalidraw or inline table)
  - Why: Makes the abstract concrete

---

## Rough Structure Ideas

**Opening** (~4 slides):
- Hook: "You have to build a many-columns, **denormalized** table. What do you do?" — relatable anxiety
- Audience poll: how many of you have seen this?
- Show the table — overwhelming, flat, no obvious structure
- The uncomfortable truth: it works for dashboards, but it's hiding something

**Main content** (~20 slides, 4 sections):
- **Section A — Why this exists** (3 slides): 1NF basics, OLTP vs OLAP, where CBTs come from in the data journey
- **Section B — The framework** (7 slides): 7 column roles, one per slide (or grouped), with the color-coded table visual evolving as each role is revealed
- **Section C — Applying it: Star/Snowflake** (5 slides): ERDs, cardinality, normalization, bridge tables, M:N pitfall
- **Section D — JOIN traps & solutions** (5 slides): NULL loss, row explosion, directionality loss, UNION BRIDGES

**Closing** (~4 slides):
- Summary: the framework in one slide (the labeled color-coded table)
- Bonus mention: anti-Fact models, non-confirmed granularities
- Key takeaways (3 bullets max)
- Call to action + acknowledgements

**Breaker/transition slides** (~6, sprinkled throughout):
- After hook: "DON'T PANIC" (humor)
- Between sections: short punchy title cards
- Before JOINs section: something like "And then we JOIN... 😬"

---

## Raw Notes & Ideas

- The `assets/table.excalidraw` file likely contains a draft of the color-coded wide table — use and extend it as the anchor visual for the framework
- "Operation Data Engineer" in the title has military/surgical connotations — lean into this metaphor (scalpel, dissection, precision) for the narrative frame
- The column classification could be presented as a progressive reveal: start with the raw colorless table, then color-code columns one category at a time
- The mindmap lists ~23 numbered points — this maps well to ~28 slides with breakers
- Consider a "cheat sheet" slide at the end: 7 roles, one line each — screenshot-able takeaway

---

## References

**Local sources**:
- `proposals/proposal-PyConLT.md` — primary abstract and elevator pitch
- `proposals/proposal-PyConIT.md` — nearly identical abstract (same talk)
- `assets/mindmap.excalidraw.png` — full narrative arc and detailed talking points
- `assets/table.excalidraw` — draft visual of wide table with column color-coding

**Keywords**: denormalized tables, 1NF, star schema, snowflake schema, fact tables, dimension tables, bridge tables, OLAP vs OLTP, data modeling, normalization anomalies

---

## Next Steps

1. ✅ Framing done (`presentation-config.md`)
2. ✅ Brainstorm done (`brainstorm.md`)
3. → Run `/slidev:outline` to create structured outline
4. → Run `/slidev:generate` to build slides from outline
