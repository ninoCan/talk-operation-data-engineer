---
layout: center
class: text-center
transition: none
---

<img src="../assets/table-dimensions.png" class="table-reveal">

# Dimensions *describe the attributes*

Attributes of an entity: the **who** and the **what**

Subject to change: Slowly Changing Dimensions (SCD)

*Examples: `customer_country`, `product_category`, `phone_number`*

<!--
TIMING: 40 seconds

"Dimensions describe the attributes of your data. The who and the what."

"They are subject to change — they slowly creep into different forms over time. That's why we call them Slowly Changing Dimensions, or SCDs."

"Think of customer_country. A customer might move. Their phone number changes. A product that was 'brand new' becomes 'obsolete'."

"Dimensions are never fully static. The trick is knowing HOW they change and designing for it — rather than discovering it six months later when a join starts returning nulls."
-->
