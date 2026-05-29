<!-- Title slide -->
So let's start.

Welcome everybody uh to this first afternoon session and uh today I will talk to you about uh the art of schema dissection operation engineer. Uh

---
<!-- About me slide -->

First of all let me present myself.
I'm Nino. I'm an Italian and in the art finished uh big data engineer at Agile Lab.
We are a consultancy firm and so uh we're building data systems.
in this company I have the opportunity of uh wearing different hats and uh for my data stacks uh pipelines I've used mostly pispark pandas and polars
uh this talk is loosely based on this book by Bill Hinmon and uh Franchesco Bupini uh the unified star schema and uh
it applies to all these three frameworks
as
uh this is something that uh I measured with experience and uh this is the kind of mental framework that I wish somebody uh introduced me to when uh from day zero.
So let me explain.

---
<!-- The narrative hook slide -->

Uh picture this. You come back from your holidays.
Your previous project is done.
Then your managers comes at you and uh tells you
the business team wants to be able to see trends in data.
uh they want to see the old re the old record history and uh be able to analyze the finest details of uh weekly, monthly, uh quarterly reports.
Uh so we need to build a dashboard.

**YOU NEED TO BUILD A ONE BIG TABLE!**

---
<!-- OBT slide -->

one big table.

This is the kind of monster I'm talking about.
A table filled with data, lots of columns, quite opaque.
So, you're going to hit this a lot.

Raise hands if you find yourself in this situation.

Okay. Okay. I see a few hands raised!
For the others. I hope this is going to be useful whenever you're going to faced we're going to be faced with this task.

---
<!-- The pain slide -->

when they tell me, let's denormalize everything. This needs to be quick.
Uh I was like:
"""
**WHAT!?!**

A denormalized table!?

Aren't those supposed to be an anti-pattern??
What am I supposed to do here?
"""

---
<!-- Reasons slides  -->

Uh, so let me stop a second and let me explain why the customer might be wanting these kind of uh tables.
First of all, they are fast to be loaded in dashboards.
they retrieve data in milliseconds latency
and uh and second they they are optimized for the stakeholders.
So the no joints are required.
Maybe the stakeholders doesn't have the capabilities to do that or is just busy doing the analysis.
So it doesn't want to be messed with that.
So this make the stakeholder happy.

In a nutshell, the use case I'm talking about for one big tables.

Let me emphasize one thing: this is not a recipe for every table
only those that are very large and contain the normalized data.
Still these kind of tables are hard to build.

---
<!-- don't panic slide -->

not not
um hiding that but let's not parking and

---
<!-- why OBTs exists slide -->

uh let's try to break down things.
Uh
one thing that makes this kind of table confusing is that usually one row contains many entities 
and with many entities there is a source of confusion
and uh there is going to be a lot of uh redundant data.
So columns that looks the same horizontally but this is fine.
Uh the problem comes in when uh the repetition uh comes between different rows.
So duplicates uh in the in the rows could create confusion.
There might be multiple source of truths.
You don't know which one to choose.
But even worse, you could lose data because the normalization makes it so easy to do that.
Still this kind of use case is not
wrong.
Uh it's just opaque.

---
<!-- leaky abstraction slide -->

Another thing I want to talk about is that tables are leaky abractions.
what do I mean by that?

A leaky abraction is uh an abstraction that that promise simplicity
but still fail to do so for every detail.

It's designed to expose patterns but there is structure that there is hidden not uh in plain sight.

Uh the naive design model can leave you to lead you to to a lot of work later on
EG. normalization, change data capture, things like that.
And uh to optimize them you you might need different techniques that are not uh coming straight away at the creation time
talking about indices sharding partition use these kind of things.

---
<!-- the data journey slide -->

But whenever you faced with this task you have to understand that there is a journey of the data.
So you start with an online transactional process
uh one where uh uh things tables are optimized to to be written so to save the store records
where everything is nice and normalized and possibly onetoone with the objects.
Then you go through the dimensional modeling.
You choose a a schema for for modeling your data
then you end up in the realm of online analytical processes.

This this is a real where everything is read optimized
data is aggregated and they normalized and OBT are the outcome of the journey.

--- 
<!-- the dissection slide -->

So let's get down to business.
Uh let's pass me the scaple.

---
<!-- the gray table slide -->

I will uh introduce you to the table.
So that monster we saw in the first picture.
Uh it all looks the same but there are organs in there.
Not every column plays the same role even though they look alike.

--- 
<!-- primary keys slide -->

First of all we have principal keys.
So these are the business entities the business qualifiers that helps you identify the object of the research.
There might be more than one uh candidate and so they also get the name of candidate keys 
in fact you can use more than one together uh in a composite key.

Whenever this is the case, you can always uh reduce yourself
from many principal key columns to a single one using a surrogate key.
A surrogate key is just a random string or auto incrementing integer or even better a nash of the columns that form the composite key.

---
<!-- dimensions slide -->

Then we go to dimensions.
Dimensions describe the attributes of your data.
The who and the what.
uh they are subject to changes.
They slowly creep in
uh different forms and uh an example of this is customer country.

A customer might move to a different place uh
changes phone number or the product uh
that was first categorized as brand new becomes obsolete and uh so this could change.

---
<!-- measures and time grains slide -->

Then we have measures.
Measures as you what happened and uh they are point in time matrix numbers
they are also called facts
uh they might have more than one time granularity
so you could have a measure that that is associated to a weekly or a monthly or quarterly time grain

---
<!-- Tech fields and aggregates slide -->

and uh this is the thing measure gets measure and time granularities get aggregated together.
So they are joint uh the mathematical operations are going to be performed on them.
You might average them, sum them uh perform moving averages, moving sums and uh and technical fields.
These are metadata or columns you use to debug to join tables together.
And uh these kind of fields usually are on new.
So these are the two most uh let's say critical uh kind of structures columns.

---
<!-- Full colors slide -->

Um
so you see the the the table that the beginning was gray now it's colorful.
I hope you have a a new kind of way of seeing them.

---
<!-- now what slide -->
And you might say okay but so what?

---
<!-- action plan slide -->

Uh yes let me outline a plan.
So first of all speak with your customers stakeholders
try to understand what's their need
because the the wrong uh understanding the wrong assumption will lead you to uh head-aches downstream.
Then you will start with the entity relationship diagrams time series or other data marts.
So other big tables uh that could come as the normalized uh
I mean while denormalization is good for a dashboard it's bad for you.
So if you have a the denormalized tables upstream reormalize it uh then you choose a model
uh and you analyze this point is often uh undervalued uh and uh
it's very important though to to analyze the cardalities of your relationships
because these are going to affect the way you glue them together in the one big table at the end.


---
<!-- ERDs graphs -->

So let me pick uh the entity relationship diagrams case.
You have these many tables uh they might uh come
from the different applications different verticals of the company.
Uh
still they they have a graph embedded in them.
So the foreign keys uh in one table point to principal keys in another.
And this is a directionality that is important because
it tells you who acts on what uh who owns the relationship.
So you have to stitch them together.

---
<!-- normalization slide -->

Normalization is not just about dissecting table,
but it can be used to be can be used to join them as well.
there are a lot of levels like seven or so in the literature
but uh for uh online analytical processes you're going to need only two of those.
The first normalization form tells you that you should have just uh a single value in a column.
So no comma separated values in a in a field and no duplicated row.
So um composition of uh primary keys that uh identifies the the the grain.
The second normal form says you should have all the attributes depending on the entirety of the key.
When you have a composite uh key then they're probably sales products shipment stitched together.
Uh not everyone of these entities will depends on the entirety of the key,
but you can still surrogate it:
transform the composite key into a single principle key by using the technique I showed you earlier.
So the these steps will guarantee you safety from insertion and deletion.

Data might be updated and that's not going to be safe but in an allup setting you shouldn't be caring that much about that.

---
<!-- dimensional modeling slide -->

So let's come to the modeling side.
I will talk about the star schema only because it's one of the most widespread
maybe not the best uh for all the use cases.
In the star schema you have fact tables and dimension tables.
Facts sits in between dimensions and describe the events and collect the measures.
Dimensions are again the description of uh the actors the attributes uh they have.
Uh dimensions should be stable when connected to a fact.
Uh and by this I mean the columns shouldn't change.
If that is the case then consider separating them into separate u into satellite dimensions.
For example, take the email and campaign over there
those dimensions are probably going to change in the future.
You keep it in a separate table and then you just point to the most updated value.
This will move uh your schema modeling from start to snowflake schema
but that's not uh the topic of this talk.

Then again when a dimension links together multiple facts is called conformed.
This is a technicality but it might come up later in question.
So this is why I mention it here.
And you can see the date dimension here is shared between multiple facts.

---
<!-- cardinalities slide -->

So let's get down to the analysis of cardinalities.
uh some cardinalities are wonderfully well-behaved!
Take the one-to-one cardinality when your country has only one capital
then there is no uh there is no room for for uh disambiguity.
Uh again one-to-one or one-to-many are also okay in the sense that
uh everything should behave normally uh with a few exceptions.

Uh but many-to-many relationship are going to be troublesome.
So whenever you have a table that has many other entities connected to it,
I think a countries have many citizens and one citizen can have multiple nationalities.
So then you introduce a separate entity to remodel this many to many relationship
into a link of one to many relationship.
So the passport uh gives each citizen a well-defined uh nationality well uh when traveling.
Uh this is called the bridge uh table or or association table
because it describes these kind of things.
It's formed by uh stitching together principle and foreign keys uh of the two databases uh into a new one the two tables into a new one.

---
<!-- and now joins slide -->

So now we join.
Fantastic.
And this is where things was out.

---
<!-- damn joins slide -->

Uh so the problem is that join the join operation is a tricky one.
Uh join have side effects.
inner left right joints can both lose records
whenever a foreign key is not pointing to a primary key of another table
or can create uh duplicates
we are going to see later that whenever there is a multiple cardinality
uh you can if you if you're experienced with with joints then you think okay but
but outer joints can uh can alleviate this problem
it's true!
But all joins have one big problem that they lose directionality.
By joining two rows together,
you put columns side by side
and that loss is the directionality.

---
<!-- union bridges slide -->

This is when union bridges come into play.
So a union bridge uh is a kind of table
that stack columns together uh on the
side but rows are just laid underneath each other.
In this way you preserve the directionality of uh of the relationship
and uh you uh are ensure that all the data is present here.
So for example, let's have a look at these two uh very simplified tables.
The sales and the product table.
The sale contain only one uh entity.

Uh so there has been only one transaction here.
There is involve two products and uh the third one was left outside.

So whenever a join is going to merge these two tables uh
not talking about outer joints
but other joints are going to lose out uh the third uh row in the product uh table.
Whereas in the union table everything is going to be there
and the yellow column is not actually part of it
but it shows you that no values is going to be lost.

---
<!-- fan trap slide -->

So let's let's deep dive into how uh duplicates uh come creeps in.
This is the fun trap.
Uh so whenever you have uh multiple cardinality
uh there is the chance that uh when joining things together
you're going to and measures are involved you're going to duplicate uh rows.
If you average over them,
if you sum them,
then it's highly likely that you're going to get the wrong numeric.

Uh fun mnemonic for for this kind of problem is straight in the diagram.
Oops.
You see the
Oops.
Right.
You see the bright part uh as this crowfoot uh kind of ending
and that that's reminiscent of a fan,
the kind of gizmo that your grandmas used to keep cool in the summertime.
Uh so many spokes uh can create duplicates.

---
<!-- chasm trap slide -->

Another kind of trap is the chasm.
The chasm trap occurs whenever two satellite dimensions are joined together into the initial one.
So
think of a social media like linking or whatever else.
There there might be users that have several skills uh and they might have had several jobs.
If you want to create a reporting table that then normalizes these three into one.
If you use join in the wrong form that the worst case scenario
is that you're going to have a cartesian product of the do values.
So
if a user has n skills and m jobs, the outcome is n * m rows: A quadratic explosion!
Whereas with unions, you only have one occurrence of them each time.
So at most you get the sum of these two rows and the growth is linear.

---
<!-- loops slide  -->


The last kind of trap is a scenario in which you have three tables connected together in a cycle.
Think uh of a sale like like your customer has bought three products,
then they all shipped together into one uh shipment and
then you might want to join the shipment back to the products to get more details about all outcome.

You might want to know maybe here is not shown but
products might have tags and so you might want to know
how many shipments of a certain category has been produced.
This kind of joints are tricky because you can have two different path.
You can use that to relationship of shipment to sales to product
or you can join the shipment uh directly to the product table.

This ambiguity can lead to errors,
even more some SQL engines are not able to choose the correct path
which might be business dependent or might as well fail to compile a query plan altogether.
This kind of problem is one of the most common because it occurs every time
there are more than uh there are even nine uh the same number of relationship as your tables.
With three tables you can have at more than two relationship that doesn't create a loop.
with uh three or more uh then you're going to have a loop.

---
<!-- morale slide -->

But let's wrap it up.
This was the dissection framework.
The three takeaway message you should bring with you after this talk are:
- that classify your columns uh know them uh so so that uh no ambiguities will come.
- model your uh journey into uh internal uh hierarchy of tables that then lead to one big table
- and then use bridges to preserve cardinalities. Use them as a support to join them tables together.

And I hope with this next time you're going to face with this problem,
you're going to have a framework that won't make you scared.

---
<!-- thanks slide -->

Thank you.
I want to thank
