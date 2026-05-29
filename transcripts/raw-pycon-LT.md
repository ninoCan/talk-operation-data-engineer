So let's start. Welcome everybody uh to
0:06
this first afternoon session and uh
0:09
today I will talk to you about uh the
0:11
art of schema dissection operation
0:14
engineer. Uh
0:17
so first of all let me present myself.
0:20
I'm Nino. I'm an Italian and in the art
0:23
finished
0:24
uh big data engineer at Agile Lab. We
0:27
are a consultancy firm and so uh we're
0:30
building data systems in this company I
0:33
have the opportunity of uh wearing
0:35
different hats and uh for my data stacks
0:38
uh pipelines I've used mostly pispark
0:41
pandas and polars
0:44
uh this talk is loosely based on this
0:46
book by Bill Hinmon and uh Franchesco
0:49
Bupini uh the unified star schema and uh
0:54
it applies to all these three frameworks
0:56
as
0:57
uh this is something that uh I measured
1:01
with experience and uh this is the kind
1:04
of mental framework that I wish somebody
1:07
uh introduced me to when uh from day
1:09
zero. So let me explain. Uh picture
1:13
this. You come back from your holidays.
1:16
Your previous project is done. Then your
1:18
managers comes at you and uh tells you
1:21
the business team wants to be able to
1:23
see trends in data. uh they want to see
1:26
the old re the old record history and uh
1:30
be able to analyze the finest details of
1:33
uh weekly, monthly, uh quarterly
1:36
reports. Uh so we need to build a
1:38
dashboard. You need to build a one big
1:41
table,
1:43
one big table. This is the kind of
1:45
monster I'm talking about. Uh table
1:50
filled with data, lots of columns, quite
1:53
opaque. So, you're going to hit this a
1:56
lot. Raise hands if you find yourself in
1:59
this situation. Okay. Okay. I see a few
2:03
hands raised for the others. I hope this
2:05
is going to be useful whenever you're
2:07
going to faced we're going to be faced
2:09
with this task. So,
2:13
when they tell me, let's denormalize
2:15
everything. This needs to be quick. Uh I
2:20
was what the the normalized table.
2:23
Aren't those an empty button? Uh, and
2:26
then 100 columns. What what what am I
2:30
supposed to do here? Uh, so let me stop
2:34
a second and let me explain why the
2:37
customer might be wanting these kind of
2:39
uh tables. First of all, they are fast
2:43
to be loaded in dashboards. they
2:46
retrieve data in milliseconds latency
2:49
and uh and second they they are
2:52
optimized for the stakeholders. So the
2:54
no joints are required. Maybe the
2:57
stakeholders doesn't have the
2:58
capabilities to do that or is just busy
3:01
doing the analysis. So it doesn't want
3:04
to be messed with that. So this make the
3:06
stakeholder happy and uh this is the use
3:09
case I'm talking about for one big
3:10
tables. So again this is not a recipe
3:13
for every table only those that are very
3:16
large and contain the normalized data.
3:18
Still these kind of tables are hard to
3:20
build. not not
3:23
um hiding that but let's not parking and
3:27
uh let's try to break down things. Uh
3:32
one thing that makes this kind of table
3:34
confusing is that usually one row
3:36
contains many entities and with many
3:39
entities there is a source of confusion
3:42
and uh there is going to be a lot of uh
3:45
redundant data. So columns that looks
3:48
the same horizontally but this is fine.
3:51
Uh the problem comes in when uh the
3:55
repetition uh comes between different
3:58
rows. So duplicates uh in the in the
4:02
rows could create confusion. There might
4:04
be multiple source of truths. You don't
4:08
know which one to choose. But even
4:10
worse, you could lose data because the
4:12
normalization makes it so easy to do
4:15
that. Still this kind of use case is not
4:19
wrong. Uh it's just opaque.
4:22
Another thing I want to talk about is
4:24
that tables are leaky abractions. What
4:26
what do I mean by that? A leaky
4:29
abraction is uh an abstraction that that
4:31
promise simplicity but still fail to do
4:35
so for every detail. So it's designed to
4:38
expose patterns but then the structure
4:41
there is structure that there is hidden
4:44
not uh in plain sight. Uh the naive
4:48
design model can leave you to lead you
4:51
to to a lot of work later on or so
4:56
normalization change data capture things
4:59
like that. And uh to optimize them you
5:01
you might need different techniques that
5:04
are not uh coming straight away at the
5:07
creation time talking about indices
5:09
sharding partition use these kind of
5:12
things. But whenever you faced with this
5:15
task you have to understand that there
5:17
is a journey of the data. So you start
5:19
with an online transactional process
5:23
uh one where uh uh things tables are
5:27
optimized to to be written so to save
5:31
the store records uh where everything is
5:34
nice and normalized and possibly
5:36
onetoone with the objects. Then you go
5:38
through the dimensional modeling. You
5:40
choose a a
5:43
schema for for modeling your data and
5:46
then you end up in the realm of online
5:48
analytical processes. This this is a
5:52
real where everything is read optimized
5:56
data is aggregated and they normalized
5:58
and OBT are the outcome of the journey.
6:03
So let's get down to business. Uh let's
6:08
pass me the scaple. I will uh introduce
6:11
you to the table. So that monster we saw
6:14
in the first picture. Uh it all looks
6:17
the same but there are organs in there.
6:21
Not every column plays the same role
6:23
even though they look alike. First of
6:26
all we have principal keys. So these are
6:29
the business entities the the the
6:32
business qualifiers that helps you
6:35
identify the the the object of the
6:38
research. There might be more than one
6:42
uh candidate and so they also get the
6:45
name of candidate keys and in fact you
6:47
can use more than one together uh in a
6:50
composite key. Whenever this is the
6:52
case, you can always uh reduce yourself
6:55
from many principal key columns to a
6:58
single one using a surrogate key. A
7:01
surrogate key is just a random string or
7:05
auto incrementing integer or even better
7:08
a nash of the columns that form the
7:11
composite key.
7:13
Then we go to dimensions. Dimensions
7:15
describe the attributes of your data.
7:18
The who and the what. uh they are
7:22
subject to changes. They slowly creep in
7:25
uh different forms and uh an example of
7:29
this is customer country. A customer
7:32
might move to a different place uh
7:35
changes phone number or the product uh
7:38
that was first categorized as brand new
7:41
becomes obsolete and uh so this could
7:44
change. Then we have measures. measured
7:46
as you what happened and uh they are
7:50
point in time matrix numbers they are
7:53
also called facts
7:55
uh they might have more than one time
7:58
granularity so you could have a measure
8:02
that that is associated to a weekly or a
8:05
monthly or quarterly uh time grain
8:09
and uh this is the thing measure gets
8:13
measure and time granularities get
8:15
aggregated together. So they are
8:19
joint uh the mathematical operations are
8:23
going to be performed on them. You might
8:25
average them, sum them uh perform moving
8:29
averages, moving sums and uh and
8:32
technical fields. These are metadata or
8:37
columns you use to debug to join tables
8:42
together. And uh these kind of fields
8:45
usually are on new. So these are the two
8:49
most uh let's say
8:52
critical uh kind of structures columns.
8:55
Um
8:57
so you see the the the table that the
9:00
beginning was gray now it's colorful. I
9:02
hope you have a a new kind of way of
9:07
seeing them. And you might say okay but
9:10
so what?
9:12
Uh yes let me outline a plan. So first
9:16
of all speak with your customers
9:19
stakeholders try to understand what's
9:21
their need because the the wrong uh
9:24
understanding the wrong assumption will
9:26
lead you to uh edex downstream. Then you
9:30
will start with the entity relationship
9:32
diagrams time series or other data
9:35
marks. So other big tables
9:38
uh that could come as the normalized uh
9:43
I mean while the normalization is good
9:45
for a dashboard it's bad for you. So if
9:48
you have a den the normalized tables
9:50
upstream reormalize it uh then you
9:53
choose a model
9:55
uh and you analyze this point is often
9:59
uh undervalued uh and uh it's very
10:03
important though to to analyze the
10:04
cardalities of your relationships
10:07
because these are going to affect the
10:10
way you glue them together in the one
10:12
big table at the end.
10:14
So let me pick uh the entity
10:16
relationship diagrams case. You have
10:19
these many tables uh they might uh come
10:23
from the different applications
10:25
different verticals of the company. Uh
10:29
still they they have a graph embedded in
10:33
them. So the foreign keys uh in one
10:37
table point to principal keys in
10:39
another. And this is a directionality
10:41
that is important because it tells you
10:44
who acts on what uh who owns the
10:46
relationship.
10:49
So you have to stitch them together.
10:51
Normalization is not just about
10:53
dissecting table but it can be used to
10:55
be can be used to join them as well. So
10:58
there are a lot of levels like seven or
11:02
so in the literature but uh for uh
11:05
online analytical processes you're going
11:08
to need only two of those. The first
11:10
normalization form tells you that you
11:12
should have just uh a single value in a
11:15
column. So no comma separated values in
11:17
a in a field and no duplicated row. So
11:21
um composition of uh primary keys that
11:24
uh identifies the the the grain. The
11:28
second number form tells you that that
11:30
you should have all the attributes
11:32
depending on the entirety of the key.
11:35
When you have a composite uh key then
11:39
they're probably sales products shipment
11:42
stitched together. Uh not everyone of
11:45
these entities will depends on the
11:47
entirety of the key but you can still
11:50
surrogate it transform the composite key
11:53
into a single principle key by by using
11:57
the technique I showed you earlier. So
12:00
the these steps will guarantee you
12:03
safety from insertion and deletion.
12:07
Data might be updated and that's not
12:10
going to be safe but in an allup setting
12:14
you shouldn't be caring that much about
12:16
that.
12:17
So let's come to the modeling side. I
12:21
will talk about the star schema only
12:24
because it's one of the most widespread
12:26
maybe not the best uh for all the use
12:29
cases. In the star schema you have fact
12:32
tables and dimension tables. Facts sits
12:36
in between dimensions and describe the
12:39
events and collect the measures.
12:42
Dimensions are again the description of
12:46
uh the actors the attributes uh they
12:50
have. Uh dimensions should be stable
12:54
when connected to a fact. Uh and by this
12:58
I mean the columns shouldn't change. If
13:02
that is the case then consider
13:04
separating them into separate u into
13:07
satellite dimensions. So for example the
13:10
email and campaign over there uh those
13:13
dimensions are probably going to change
13:15
in the future. You keep it in a separate
13:17
table and then you just point to the
13:19
most updated value.
13:22
This will move uh your schema dimen
13:25
modeling from start to snowflake schema
13:28
but that's not uh the topic of this
13:30
talk. Then again when a dimension links
13:35
together multiple facts is called
13:37
conformed. This is a technicality but it
13:40
might come up later in question. So this
13:44
is why I mention it here. And you can
13:47
see the date dimension here is shared
13:49
between multiple facts.
13:54
So let's get down to the analysis of
13:56
cardalities. uh some card analysis are
13:59
wonderful like the one to one cardality
14:02
when your country has only one capital
14:05
then there is no uh there is no room for
14:09
for uh disambiguity.
14:12
Uh again one to end card analysis or one
14:16
to many are also okay in the sense that
14:21
uh everything should
14:24
behave normally uh with a few
14:27
exceptions.
14:29
Uh but many to many relationship are
14:32
going to be troublesome. So whenever you
14:35
have a table that has many other
14:40
entities connected to it, I think a
14:42
countries have many citizens and one
14:45
citizen can have multiple nationalities.
14:48
So then you introduce a separate entity
14:51
to remodel this many to many
14:54
relationship into a link of one to many
14:58
relationship. So the passport uh gives
15:01
each citizen a well- definfined uh
15:05
nationality well uh when traveling.
15:09
Uh this is called the bridge uh table or
15:13
or association table because it
15:15
describes these kind of things. It's
15:17
formed by uh stitching together
15:19
principle and foreign keys uh of the two
15:23
databases uh into a new one the two
15:26
tables into a new one. So now we join.
15:29
Fantastic. And this is where things was
15:32
out. Uh so the problem is that join the
15:36
join operation is a tricky one. Uh join
15:40
have side effects. inner left right
15:43
joints can both lose records whenever
15:47
a foreign key is not pointing to a
15:50
primary key of another table or can
15:53
create uh duplicates we are going to see
15:55
later uh when whenever there is a
15:58
multiple cardality
16:01
uh you can if you if you're experienced
16:04
with with joints then you think okay but
16:06
outer joints can uh can alleviate this
16:10
problem it's true But all joins have one
16:13
big problem that they lose cardality. By
16:16
joining two rows together, you put
16:19
columns side by side and that loss is
16:21
the directionality.
16:24
This is when union bridges come into
16:27
play.
16:28
So a union bridge uh is a kind of table
16:33
that stack columns together uh on the
16:38
side but rows are just laid underneath
16:41
each other. In this way you preserve the
16:44
directionality of uh of the relationship
16:47
and uh you uh are ensure that all the
16:51
data is present here. So for example,
16:55
let's have a look at these two uh very
16:57
simplified tables. The sales and the
16:59
product table. The sale contain only one
17:02
uh entity. Uh so there has been only one
17:07
transaction here. There is
17:10
involve two products and uh the third
17:13
one was left outside. So whenever a join
17:16
is going to merge these two tables uh
17:20
not talking about outer joints but other
17:22
joints are going to lose out uh the
17:25
third uh row in the product uh table.
17:28
Whereas in the union table everything is
17:32
going to be there and the yellow column
17:36
is not actually part of it but it shows
17:38
you that no values is going to be lost.
17:44
So let's let's deep dive into how uh
17:49
duplicates uh come creeps in. This is
17:53
the the the fun trap. Uh so whenever you
17:57
have uh multiple cardality
18:01
uh there is the chance that uh when
18:04
joining things together you're going to
18:06
and measures are involved you're going
18:08
to duplicate uh rows. If you average
18:12
over them, if you sum them, then it's
18:14
highly likely that you're going to get
18:16
the the wrong numeric.
18:18
Uh funnemonic
18:22
for for this kind of problem is straight
18:24
in the diagram. Oops.
18:27
You see the
18:31
Oops.
18:35
Right.
18:37
You see the bright part uh as this
18:40
crowoot uh kind of ending and that
18:44
that's reminiscent of a fun the the kind
18:48
of gizmo that your grandmas used to keep
18:51
cool in the summertime.
18:54
Uh so many spokes uh can create
18:58
duplicates.
19:01
Another kind of trap is the chasm.
19:05
Uh the chasm trap of course whenever two
19:08
dimensions two satellite dimensions are
19:10
joined together into the initial one. So
19:15
think of a social media like linking or
19:19
whatever else. there there might be
19:21
users that have several skills uh and
19:25
they might have had several jobs. If you
19:27
want to create a reporting table that
19:29
then normalizes these three into one.
19:33
If you use join in the wrong form that
19:36
the worst case scenario is that you're
19:38
going to have a um
19:42
cartisian product of the do values. So
19:46
if a user has n skills and m jobs, the
19:50
outcome is n * m rows. A quadratic
19:54
explosion. Whereas with unions, you only
19:57
have one occurrence of them each time.
20:00
So at most you get the sum of these two
20:04
rows and the growth is linear.
20:08
The last kind of uh trap is uh a
20:13
scenario in which for example you have
20:16
uh three tables uh that are horizontally
20:20
connected together. So, so think uh of a
20:24
sale like like your customer has bought
20:26
three products, then they all shipped
20:29
together into one uh shipment and and
20:33
then you might want to join the shipment
20:36
back to the products to get more details
20:39
about uh the the all uh outcome. You
20:44
might want to know maybe here is not
20:46
shown but
20:48
products might have tags and so you
20:51
might want to know how many shipments of
20:54
a certain category has been produced.
20:58
This kind of joints are tricky because
21:01
you can have two different path. You can
21:05
use that to
21:07
uh relationship of shipment to sales to
21:10
product or you can join the shipment uh
21:13
directly to the product table.
21:16
This ambiguity can lead to errors and uh
21:21
more uh some SQL engines are not able to
21:25
choose the the correct path
21:28
uh which might be business dependent or
21:30
might as well uh fail to compile a query
21:34
plan altogether.
21:36
And so
21:38
this kind of problem is one of the most
21:41
common because it occurs every time
21:43
there are
21:45
more than uh there are even nine uh the
21:49
the same number of relationship as your
21:52
tables.
21:54
With uh three tables you can have at
21:56
most two relationship
21:59
that doesn't create
22:02
a loop. with uh three or more uh then
22:05
you're going to have a loop.
22:08
But let's wrap it up. This was the the
22:12
session framework. The three takeaway
22:16
message you should bring with you after
22:18
this talk are that classify your columns
22:22
uh know them uh so so that uh no
22:25
ambiguities will come. model your uh
22:31
journey into uh
22:35
internal uh
22:38
hierarchy of tables that then lead to
22:40
one big table and then use bridges to
22:43
preserve cardalities.
22:46
So use them as a support to join them
22:50
tables together.
22:52
And I hope with this next time you're
22:55
going to face with this problem, you're
22:58
going to have a framework that won't
23:00
make you scared. Thank you. I want to
23:04
thank
