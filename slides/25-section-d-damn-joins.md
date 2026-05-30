---
layout: center
class: text-center
---

# JOINs are tricky

<div style="text-align: left">

#### **INNER/LEFT/RIGHT JOIN** can both:
- lose records when one between the FK or PK is absent `→ missing data`
- explode rows when modelling n-m relationships `→ duplicates`
#### **OUTER JOIN**: can mitigate these problems but...
#### **All JOINs**: erase the PK → FK direction from the result

</div>
