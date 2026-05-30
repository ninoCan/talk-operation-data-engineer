<!-- Title slide -->

It's after lunch, you might be fighting the urge to nod off... but stay with me.
Today I want to show you a new way of seeing a data structure you've almost certainly already encountered.
Welcome to the art of schema dissection: Operation Data Engineer.

---
<!-- About me slide -->

Let me introduce myself.
I'm Nino, an Italian big data engineer at Agile Lab.
We are a consultancy firm; we design and build data systems.
In this company I get to wear many different hats.
For my data pipeline work I've used mostly PySpark, pandas, and polars.
This talk is loosely based on the book by Bill Inmon and Francesco Puppini, "The Unified Star Schema"; it applies to all three frameworks.
This is something I built up through experience.
It's the kind of mental framework I wish someone had handed me on day zero.
So... let me explain.

---
<!-- The narrative hook slide -->

Picture this.
You come back from your holidays; your previous project is wrapped up.
Then your manager pulls you aside.
The business team wants to see trends in the data.
They want the full record history; they want to drill into weekly, monthly, quarterly reports.
So: we need a dashboard.

**YOU NEED TO BUILD A ONE BIG TABLE!**

---
<!-- OBT slide -->

One big table.

Look at the slide: customer name next to product SKU next to shipment date next to a rolling 30-day average... all in a single row.
No obvious grouping; no clear structure.
OBT... you are going to hit this a lot.

Raise your hand if you've been in this situation.

I see a few hands; good.
For the others: I hope this becomes useful the day your manager comes knocking.

---
<!-- The pain slide -->

When they told me "let's denormalize everything, this needs to be quick," I froze.

**WHAT!?!**

A denormalized table!?

Aren't those supposed to be an anti-pattern??
What am I supposed to do here?

---
<!-- Reasons slides -->

Let me take a step back and explain why the customer actually wants these tables.
First: they load fast in dashboards; we're talking millisecond latency.
Second: they are optimized for stakeholders.
No joins required; maybe the stakeholders don't have the SQL skills, or they simply don't want to be bothered with it.
Either way, the stakeholder is happy.

That is the use case for One Big Tables, in a nutshell.

One thing worth saying clearly: this is not a recipe for every table!
Only large, denormalized reporting tables warrant this treatment.
Still... these tables are genuinely hard to build.

---
<!-- don't panic slide -->

But let's not panic.

---
<!-- why OBTs exist slide -->

Let's try to break things down.
What makes these tables confusing is that one row often encodes many different entities.
Many entities means many sources of potential confusion; there will be a lot of redundant data.
In other words: columns that look the same, stacked horizontally... but that is not the real problem.

Horizontal repetition is totally fine!

The problem is when repetition appears across rows.
Row-level duplicates can create confusion; there might be multiple sources of truth.
Which one do you trust?
Even worse: denormalization makes it dangerously easy to silently lose data.
The use case itself is not wrong... it's just opaque.

---
<!-- leaky abstraction slide -->

I also want to talk about the fact that tables are leaky abstractions.
What do I mean?

A leaky abstraction promises simplicity; it hides the complexity underneath.
But an OBT hides something specific: it encodes multiple entity relationships inside a flat structure, with no trace of them in the schema itself.
You won't see those decisions... until you hit a performance problem or a data loss incident.

A naive design at this stage can generate a lot of rework later: normalization passes, change data capture pipelines, things like that.
Optimizing them will require techniques that are not obvious at creation time; indices, sharding, partitioning... none of those are visible in the original table.

---
<!-- the data journey slide -->

Whenever you face this task, there is something important to keep in mind: there is a journey the data has already taken.
It starts in an online transactional process; tables optimized for writes, nicely normalized, often one-to-one with domain objects.
Then it passes through dimensional modeling; you choose a schema to reshape the data.
Finally it lands in the realm of online analytical processes.

This is the realm where everything is read-optimized; data is aggregated, denormalized... and OBTs are the destination.

---
<!-- the dissection slide -->

So let's get down to business.
Pass me the scalpel.

---
<!-- the gray table slide -->

Look at the table again.
It all looks the same; just columns, gray and uniform.
But there are organs in there.
Not every column plays the same role, even if they look alike.

---
<!-- primary keys slide -->

First: principal keys.
These are your business entities; the qualifiers that identify the object you're researching.
There might be more than one candidate; hence the name candidate keys.
You can also combine more than one into a composite key.

Whenever that's the case, you can collapse those many columns into a single surrogate key.
A surrogate key is a random string, an auto-incrementing integer... or better yet, a deterministic hash of the columns that form the composite key.
Why a hash? Because it's idempotent; run the pipeline twice and you get the same key, which is critical for safe reruns.

---
<!-- dimensions slide -->

Then we have dimensions.
Dimensions describe the attributes of your data; the who and the what.
They are subject to change; they slowly creep into different forms over time.
Think of customer country.

A customer might move; their phone number might change.
A product that was first categorized as brand new can become obsolete.
Dimensions are never fully static.

---
<!-- measures and time grains slide -->

Then we have measures.
Measures describe what happened; they are point-in-time numeric values.
They are also called facts.
A measure can have more than one time granularity; weekly, monthly, quarterly... each is a different grain.

---
<!-- Tech fields and aggregates slide -->

Here is the key point: measures and time granularities get aggregated together.
Mathematical operations are going to be performed on them; averages, sums, moving averages, moving sums.
Then there are technical fields.
These are metadata; columns you use for debugging or for joining tables together.
They are often null in production records; they exist to serve the pipeline, not the analyst.
These two types are the most critical structures to identify before you start building.

---
<!-- Full colors slide -->

So now you see it: the table that looked uniformly gray... is actually colorful.
Each column type has a role; each role has implications.
I hope you have a new way of seeing them now.

---
<!-- now what slide -->

And you might say... okay, but so what?

---
<!-- action plan slide -->

Yes; let me walk you through a plan.
First: talk to your customers and stakeholders.
Understand their actual need, not the one you assume they have; a wrong assumption here will create headaches all the way downstream.
Then you survey your inputs: entity relationship diagrams, time series, or data marts... other large tables that may already arrive denormalized.
Denormalization is good for a dashboard; it is bad for you as an intermediate step.
So if you receive denormalized tables upstream... renormalize them.
Then you choose a model.
And then you analyze cardinalities; this step is often skipped, but it matters enormously.
The cardinalities of your relationships will determine exactly how you stitch everything together into the final One Big Table.

---
<!-- ERDs graphs -->

Let me walk through the entity relationship diagrams case.
You have many tables from different applications; different verticals of the same company.
But they carry a graph inside them.
Foreign keys in one table point to principal keys in another; that's a direction, not just a link.
That direction tells you who acts on what; who owns the relationship.
You have to stitch them together... respecting that direction.

---
<!-- normalization slide -->

Normalization is not just a tool for dissecting tables; it can also be used to join them safely.
There are many normal forms in the literature; seven or so.
For analytical processes you only need two.

The first normal form says: one value per column.
No comma-separated lists in a field; no duplicated rows.
Just a composition of primary keys that identifies the grain.

The second normal form says: every attribute must depend on the entirety of the key.
Take a composite key like sales + products + shipment stitched together.
Not every attribute will depend on all three; but you can surrogate it.
Transform the composite key into a single principal key using the technique from earlier.
These two steps protect you from insertion and deletion anomalies.

Data updates are another story; they are not fully safe even after this.
In an OLAP setting though... you typically don't care much about that.

---
<!-- dimensional modeling slide -->

Now let's talk modeling.
I will cover only the star schema; it's the most widespread, and it's enough for our purposes.
In a star schema you have fact tables and dimension tables.
Facts sit at the center; they describe events and collect the measures.
Dimensions radiate outward; they describe the actors and their attributes.

Dimensions connected to a fact should be stable; the columns shouldn't change underneath you.
If they do change... consider splitting them into satellite dimensions.
Take email and campaign columns, for example; those are likely to change in the future.
You park them in a separate table and point to the most recent value.
This moves your design from a star to a snowflake schema; but that's a topic for another talk.

One more term worth knowing: when a dimension is shared across multiple fact tables, it's called a conformed dimension.
It's a technicality... but it may come up in questions, so I mention it here.
The date dimension in the diagram is a classic example.

---
<!-- cardinalities slide -->

Now let's analyze cardinalities.
Some cardinalities are wonderfully well-behaved!
One-to-one is clean; one country, one capital... no ambiguity.
One-to-many is also generally fine; things behave as expected, with a few edge cases.

Many-to-many relationships are where things get troublesome.
Think of citizens and countries; a citizen can hold multiple nationalities, a country has many citizens.
To handle this, you introduce a separate entity that breaks the many-to-many into a chain of one-to-many relationships.
The passport is the analogy here; it gives each citizen a well-defined nationality in a specific context.
This is called a bridge table, or association table.
It's built by stitching together the principal and foreign keys of both tables into a new one.

---
<!-- and now joins slide -->

So now... we join.
Fantastic.
And this is where things get complicated.

---
<!-- damn joins slide -->

Joins are tricky; they have side effects.
Inner, left, and right joins can all lose records; whenever a foreign key has no matching primary key in the other table.
Or they can create duplicates; whenever a key matches multiple rows.
You might think: "outer joins can fix this, right?"
And it's true... partially.
But all joins share one fundamental problem: they lose directionality.
When you join two rows, you place columns side by side; the original relationship between the rows disappears.

---
<!-- union bridges slide -->

This is where union bridges come in.
A union bridge stacks rows on top of each other; rather than placing columns side by side.
This preserves the directionality of the relationship; and ensures no data goes missing.

Take these two simplified tables: sales and products.
The sale record references two products; the third product has no matching sale.
A standard join would drop that third row entirely.
In a union bridge, every row is present; the structure makes data loss visible rather than silent.

---
<!-- fan trap slide -->

Let's look at how duplicates sneak in.
This is the fan trap.
Whenever you have a one-to-many cardinality and measures are involved, joining can silently duplicate rows.
Average or sum over those duplicates... and you'll get the wrong number, every time.

Here's a mnemonic: look at the diagram.
See that crowfoot-style ending on the relationship line?
It looks like a fan; the kind your grandma used to keep cool in the summer.
Many spokes... many duplicates.

---
<!-- chasm trap slide -->

The second trap is the chasm.
It occurs when two satellite dimensions are both joined back to a central fact.
Think of a LinkedIn-style profile; a user has several skills and several jobs.
If you want a single reporting table that denormalizes all three... and you use plain joins, the worst case is a Cartesian product.
If a user has n skills and m jobs, you end up with n × m rows; quadratic growth.
With unions, each row appears exactly once; at most you get n + m rows, and growth stays linear.

---
<!-- loops slide -->

The last trap is the loop: three or more tables connected in a cycle.
Think of a sale; a customer buys three products, all shipped together in one shipment.
Now you want to join the shipment back to the products for more detail... but two paths exist.
You can go shipment → sales → products; or you can join shipment directly to products.

This ambiguity can lead to silent errors.
Some SQL engines cannot determine the correct path; some fail to compile a query plan at all.
The rule is simple: with n tables, you can have at most n − 1 relationships before you create a loop.
Three tables; two safe relationships.
Add a third relationship... and you have a loop.

---
<!-- morale slide -->

Let's wrap up.
This was the dissection framework; and here are the three things to take with you.

First: classify your columns and know them well; ambiguity in column roles is the root of most OBT bugs.
Second: model the journey; build an internal hierarchy of tables that leads deliberately to the One Big Table.
Third: use bridges to preserve cardinalities; reach for a union bridge before you reach for a join.

Next time you open a schema that looks opaque... you'll have a framework.
And it won't feel so scary.

---
<!-- thanks slide -->

Thank you.
I want to thank...
