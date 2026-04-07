---
layout: center
class: text-center
---


# JOINs are tricky

<div style="text-align: left">

#### **INNER/LEFT/RIGHT JOIN** can both:
- lose records when one between the FK or PK is absent `-> missing data`
- explode rows when modelling n-m relationships `-> duplicates`
#### **OUTER JOIN**: can mitigate these problems but...
#### **All JOINs**: erase the PK → FK direction from the result

</div>

<!--
Here's the painful truth about JOINs: they're lossy.
The moment you JOIN two tables, you lose something.
If you use an inner join and a foreign key is null — which happens more often than you think in real-world data — that row simply disappears. No error. No warning. Just missing data.
If you use an outer join and the cardinality isn't what you expected, your row count can quietly explode.
And in both cases, the directionality you carefully defined in your ERD — the PK to FK arrow — is gone. The joined table doesn't know which entity owns the relationship anymore.
The rule is simple: understand your cardinality before you write the JOIN. If you don't, you're flying blind.
-->
