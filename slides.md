---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: "Master the Art of Schema Dissection: Operation Data Engineer"
info: |
  ## Master the Art of Schema Dissection: Operation Data Engineer
  PyCon LT · April 2026

  A practical framework for classifying columns in denormalized tables
  and approaching normalization as surgical precision.
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 35min
---

# Master the Art of Schema Dissection

## Operation Data Engineer

<div class="pt-4 text-gray-400">
PyCon LT · April 2026
</div>

---
layout: default
---

# Data engineer, PySpark practitioner

I build pipelines in **PySpark**, **pandas**, and **polars**.

In that world, **OBTs are unavoidable**.

*This talk is what I wish someone had told me earlier.*

<!--
Good morning, PyCon Lithuania. I'm Nino, a data engineer.
I spend most of my working hours in PySpark, pandas, and occasionally polars — the Python data stack.
And in that world, One Big Tables are everywhere. You build them, you inherit them, you struggle with them.
This talk is the mental framework I wish someone had handed me on day one.
Let me show you what I mean.
-->

---

# The assignment arrives on a Monday

> "We need a dashboard. Build a **One Big Table**."

**OBT** — One Big Table — you'll hear this a lot today.

*Raise your hand if you've been here.*

<!--
Let me set a scene. It's Monday morning. Your stakeholder pings you — they need a new report by Friday.
"Just build a One Big Table," they say. "Denormalize everything, make it fast. The BI tool will handle the rest."
How many of you have received some version of this request? Go ahead, raise your hand.
[pause for audience]
That's a lot of hands. Good — because everything I'm about to show you applies directly to what you're building right now.
-->

---
layout: center
---

# 90 columns. No documentation.

<!-- TODO: Visual - HIGH PRIORITY
Type: Excalidraw (assets/table.excalidraw — starting state, all columns same neutral color)
Suggestion: Dense grid of column headers, ~90 columns, undifferentiated.
Names like: customer_id, order_date, product_sku, ytd_revenue, channel_flag,
batch_insert_ts, surrogate_key, customer_segment_code, avg_order_value, _etl_version, ...
Why: Visceral. Audience immediately recognizes it. One image, no words needed.
Element count: 1 visual = 1 element ✓
-->

<!--
Here it is. The One Big Table.
Ninety columns. Maybe more. Names like customer_segment_code, ytd_revenue, batch_insert_ts, surrogate_key_product.
What does order_channel_flag mean? Is it a dimension? A fact? A filter? A legacy artifact from 2019?
You have no idea. And often the person who built it has left the company.
This is what you're handed. This is where you start.
-->

---

# OBTs work — and that's exactly the problem

- Dashboards load fast ✓
- No JOINs required ✓
- Stakeholders are happy ✓

But they are **hiding something**.

<!--
Here's the uncomfortable truth about One Big Tables: they work.
Dashboards load in milliseconds. No complex joins, no query planning, no foreign key headaches.
Stakeholders love it. Your manager loves it.
But that ease of use comes at a cost — and that cost is completely invisible.
OBTs conceal multiple business entities, metrics, time semantics, and technical metadata
inside one flat, undifferentiated structure.
With denormalized tables it's surprisingly easy to lose rows or double-count facts without realizing it.
That's what we're going to fix today.
-->

---
layout: center
class: text-center
---

# DON'T PANIC

*Let's break this down.*

---
src: ./pages/section-a-why-obts-exist.md
---

---
src: ./pages/section-b-dissection-framework.md
---

---
src: ./pages/section-c-modeling.md
---

---
src: ./pages/section-d-join-traps.md
---

---
src: ./pages/closing.md
---

---
src: ./pages/backup.md
---
