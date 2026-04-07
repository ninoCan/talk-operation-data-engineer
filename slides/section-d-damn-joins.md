---
layout: center
class: text-center
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
