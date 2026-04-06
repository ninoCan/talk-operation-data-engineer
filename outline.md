# Master the Art of Schema Dissection: Operation Data Engineer

**Duration:** 20 minutes
**Target Slides:** 28 (23 contentful × 50s + 5 breakers × 5s ≈ 19.6 min)
**Audience:** Python developers, intermediate — PyCon LT 2026
**Slide types:** [C] = contentful (~50s), [B] = breaker/transition (~5s)

---

## Opening — The Setup (6 slides, ~3.6 min)

### Slide 1 [B]: Title
- "Master the Art of Schema Dissection: Operation Data Engineer"
- PyCon LT 2026 · Nino · no notes needed

### Slide 2 [C]: About me
- Who am I, what I do
- Brief personal framing to establish data engineering credibility
- **Python-stack anchor here**: "I work with PySpark, pandas, and polars — and OBTs are unavoidable in that world"
- Keep it tight — one slide, ~30 words

### Slide 3 [C]: The task
- Story hook: "You're a data engineer. Your stakeholders need a report — fast. Management says: build a One Big Table."
- Audience prompt: "Raise your hand if you've been here."
- Introduce the term **OBT** (One Big Table)

### Slide 4 [C]: The OBT in the wild
- Show a wide, real-feeling table: 90+ column names, no obvious structure
- Visual: color-coded table grid (all columns same color — undifferentiated chaos)
- Source: `assets/table.excalidraw` (starting state — no colors yet)

### Slide 5 [C]: The uncomfortable truth
- OBTs work. Stakeholders are happy. No JOINs needed.
- But: they're tricky to build correctly, with denormalized tables it's easy to lose or miscount
- Setup the tension: what is hiding inside?

### Slide 6 [B]: DON'T PANIC
- Single punchy line
- Light humor — resets the room before the technical content begins

---

## Section A — Why OBTs Exist (3 slides, ~2.5 min)

### Slide 7 [C]: OBTs and entities
- OBTs often contain many entities mixed together in the same row
- Horizontal redundancy is OK — but denormalization can hide data and silently generate duplicates
- Key insight: the table isn't wrong, it's opaque

### Slide 8 [C]: Tables are leaky abstractions
- A table is supposed to simplify, but it exposes us to new complexity anyway — and adds traps
- To optimize: indices, normalization, query patterns all leak through
- Visual: simple analogy diagram (abstraction layer that's not fully hiding what's below)

### Slide 9 [C]: The data journey
There are two systems at play:
- OLTP: write-optimized, normalized, 1:1 with objects
- OLAP: read-optimized, aggregated, denormalized
- OBT is an *outcome* of the journey — you don't build it from scratch, you arrive at it
- Visual: left-to-right flow diagram (Mermaid)

---

## Breaker

### Slide 10 [B]: The Dissection
- Section intro: "Let's open it up."
- Military/surgical metaphor — "time to pick up the scalpel"

---

## Section B — The Dissection Framework (7 slides, ~5.8 min)

### Slide 11 [C]: Columns are not all the same
- Core thesis: every column in an OBT plays a specific functional role
- Once you know the role, the table stops being a mystery
- Preview the **6 role categories**: Keys · Dimensions · Facts · Temporal · Technical · Aggregations
- Visual: the raw OBT grid — about to be color-coded, with a compact role legend alongside

### Slide 12 [C]: Keys
- Define the identity and relationships of your data
- **Business Key**: natural identifier (e.g., `order_id`)
- **Candidate Key**: could serve as PK
- **Composite Key**: multiple columns together form the PK
- **Surrogate Key**: system-generated PK (e.g., auto-increment, UUID)
- Color: blue — highlight key columns in the OBT grid

### Slide 13 [C]: Dimensions
- Describe the *attributes* of an entity
- Subject to evolution over time (SCD — Slowly Changing Dimensions)
- Types: simple (flat value), composite (multi-field), business (carries meaning)
- Color: green — highlight dimension columns in the OBT grid

### Slide 14 [C]: Facts & Temporal granularities
- **Facts/Measures**: point-in-time numeric metrics of an entity (e.g., `revenue`, `quantity`)
- **Temporal**: define the time grain — in OLTP "one row per unique granularity", in OLAP "multiple time granularities are possible" (think of weekly, monthly, quarterly sales)
- Together they define *what happened, when*
- Color: orange (facts), purple (temporal) — highlight in OBT grid

### Slide 15 [C]: Technical fields & Aggregations
- **Technical Fields**: metadata that supports operations, not business meaning (e.g., `created_at`, `batch_id`, `_source`)
- **Aggregations**: pre-computed summaries of facts over dimensions — they reduce normality
- Color: grey (technical), red (aggregations) — highlight in OBT grid

### Slide 16 [C]: The full picture
- The OBT grid fully color-coded: keys (blue), dimensions (green), facts (orange), temporal (purple), technical (grey), aggregations (red)
- Now you can *see* the hidden entities, metrics, and time semantics
- This is the dissection framework — your cheat sheet
- Visual: completed color-coded OBT (Excalidraw, from `assets/table.excalidraw`)

---

## Breaker

### Slide 17 [B]: Now let's model it
- "You can see the structure. Now what do you do with it?"

---

## Section C — Modeling (5 slides, ~3.8 min)

### Slide 18 [B]: Sneak peek
- You'll have ERD-modelled datasets, Time-series and/or other data mart 
- If the upstream datasets are denormalized, re-normalizing could be a good starting point
- Compose your own data model (eg Star, Snowflake, Data Vault, Anchor) 
- Bring them together in the OBT


### Slide 19 [C]: ERDs & ontology
- ERDs define the ontology/taxonomy of your data
- Directionality: PK → FK tells you who owns the relationship
- Time-series tables typically use composite keys (entity + timestamp)
- These tables describe the evolution of a single entity over time
- Visual: minimal ERD (Mermaid)

### Slide 20 [C]: Normalization
- Normalization reduces redundancy → reduces inconsistency risk
- **1NF**: each column holds a single atomic value; no duplicate rows
- **2NF**: every non-key attribute depends on the *entire* PK
- Practical move: replace composite keys with a surrogate key
- Benefit: protection from insertion and deletion anomalies
- **Frame it as surgery**: "This isn't a big-bang rewrite — it's a precise incision. Your dissection map tells you exactly where to cut."

### Slide 21 [C]: Star schema
- The most common modeling target for OBT dissection
- **Dimension tables**: describe actors and their attributes (e.g., Customers, Products)
- **Fact tables**: describe events involving actors (e.g., Purchases, Sales)
- Facts sit between Dimensions; Dimensions can connect multiple Facts (Snowflake variant)
- Visual: classic star diagram (Mermaid graph)

### Slide 22 [C]: Cardinality & M:N pitfall
- Cardinality: 1→1, 1→n — straightforward in RDBMS
- **M:N is a pitfall**: poorly modeled directly in relational databases
- Solution: **Bridge tables** (association tables) — reduce M:N to two 1→n relationships
- Bridge contains overlapping PKs from both tables as FKs → forms a composite key
- Visual: before/after M:N → Bridge (Mermaid ER)

---

## Breaker

### Slide 23 [B]: And then... we JOIN
- Humor beat: "Everything looks great. Time to JOIN."
- Sets up the tension drop before the traps section

---

## Section D — JOIN Traps & Resolution (4 slides, ~3.3 min)

### Slide 24 [C]: JOIN anomalies
- JOINs are lossy by design — they destroy relationship directionality
- **NULL FK/PK**: inner joins silently drop rows; outer joins may explode them
- **Row explosion**: Full Outer JOIN on unmatched keys multiplies rows unexpectedly
- Rule: always understand your cardinality before you JOIN

### Slide 25 [C]: FAN TRAP & CHASM TRAP
- **FAN TRAP**: one (or more) Fact tables with a many-to-one relation to another — facts get fanned out and double-counted
  - Example: `Sales (FK) >---+ (PK) Products`
- **CHASM TRAP**: two Dimension tables with a many-to-one relation to a shared third table — rows vanish when outer records have no match on both sides
  - Example: `Skills (FK) >---+ (PK) Users +---< (FK) Languages`
- Visual: ER diagram showing both traps (Mermaid)

### Slide 26 [C]: LOOPS
- Given n tables, at most n−1 relationships can exist without forming a loop
- Loops in JOIN chains create ambiguous paths — SQL engines resolve them unpredictably
- Often introduced silently when building OBTs with n+1 relations
- Hard to detect without mapping the full graph first

### Slide 27 [C]: UNION BRIDGES to the rescue
- Key insight: JOIN puts columns side-by-side; UNION ALL stacks rows underneath each other
- **UNION BRIDGES**: build a bridge table with UNION ALL, then JOIN other tables to it
- Immune to duplicate explosion; preserves directionality
- Solves FAN TRAP, CHASM TRAP, and LOOPS in one pattern
- Visual: before/after JOIN vs UNION BRIDGE (Excalidraw table diff)

---

## Closing (2 slides, ~1.7 min)

### Slide 28 [C]: The cheat sheet
- The fully labeled color-coded OBT one more time
- Three takeaways overlaid:
  1. Classify before you touch (6 roles)
  2. Model with Star/Snowflake — let the dissection guide you
  3. JOINs are lossy — bridge tables preserve directionality
- Bonus mention: anti-Fact models & non-conformed granularities exist for edge cases
- This slide is meant to be screenshot-able

### Slide 29 [C]: Thank you
- Key CTA: "Next time you open an OBT — don't scroll in despair. Pick up your scalpel."
- Links / QR code for further reading
- Acknowledgements
- Questions

---

## Notes

**Transition points:**
- Slides 1–6 → Sec A: tension established ("what's hiding inside?")
- Sec A → Sec B (slide 10): pivot from "why" to "how to see it"
- Sec B → Sec C (slide 17): pivot from "seeing" to "doing"
- Sec C → Sec D (slide 23): humor beat before the trap reveal
- Sec D → Closing: resolution — the framework works, here's your map

**Visual inventory:**
| Slide | Visual | Type |
|-------|--------|------|
| 4 | Raw OBT grid (undifferentiated) | Excalidraw |
| 9 | OLTP → OLAP flow | Mermaid flowchart |
| 11 | Raw OBT grid (preview) | Excalidraw |
| 12–15 | OBT grid progressive color reveal | Excalidraw (incremental) |
| 16 | Fully color-coded OBT | Excalidraw |
| 18 | Minimal ERD | Mermaid ER |
| 20 | Star schema | Mermaid graph |
| 21 | M:N → Bridge table | Mermaid ER |
| 24 | FAN + CHASM traps | Mermaid ER |
| 26 | JOIN vs UNION BRIDGE diff | Excalidraw |
| 27 | Labeled color-coded OBT (cheat sheet) | Excalidraw |

**Anchor visual:** The OBT color-coded grid from `assets/table.excalidraw` appears on slides 4, 11–16, and 27. It is the visual backbone of the talk — start blank, reveal progressively, end fully labeled.

**Time allocation:**
| Section | Slides | Time |
|---------|--------|------|
| Opening | 6 (4C + 2B) | ~3.5 min |
| Sec A: Why OBTs | 3C | ~2.5 min |
| Sec B: Framework | 7 (6C + 1B) | ~5.1 min |
| Sec C: Modeling | 5 (4C + 1B) | ~3.8 min |
| Sec D: JOINs | 5 (4C + 1B) | ~3.8 min |
| Closing | 2C | ~1.7 min |
| **Total** | **28 slides (23C + 5B)** | **~19.6 min** |

---

## Backup Slides (not counted in 28 — for Q&A)

### Backup 1: UNION BRIDGE in PySpark
- Concrete code example: building a UNION bridge in PySpark or pandas
- Addresses the Python-stack audience directly
- Good for Q&A on "how do I actually do this?"

### Backup 2: Multifacts, Anti-Fact models & non-conformed granularities
- Expanded version of the "bonus mention" on slide 27
- Anti-Fact: records absence of an event (e.g., no purchase made)
- Non-conformed granularities: when Fact tables use incompatible time grains
- Target & Rules modeling pattern

### Backup 3: Full 7-role reference card
- Complete taxonomy including "Indices" (partition keys, sort keys in warehouse systems like Parquet/BigQuery/Redshift)
- One line per role with a short description and a concrete column name example
- The "indices" role was in the original proposal — here for completeness and reference
