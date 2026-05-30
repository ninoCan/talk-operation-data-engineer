---
layout: center
class: text-center
---

# Tables are Leaky Abstractions

## Tables promise simplicity... yet, complexity leaks through

<span class="big-emoji">🔎</span> Designed to expose patterns, but hide structure

<span class="big-emoji">📐</span> Naive modeling choices generate extra work later (`normalization`, `CDC`, ...)

<span class="big-emoji">🤸</span>♀️ Extra gymnastics to optimize (`Indices`, `sharding`, ...)

<!--
TIMING: 50 seconds

The leaky abstraction concept — make it tangible before showing the bullets.

"You've probably heard the term 'leaky abstraction'. The idea is that a system promises to hide complexity, but the complexity leaks through at the worst possible moment."

"An OBT hides something specific: it encodes multiple entity relationships inside a flat structure, with no trace of those decisions in the schema itself."

Walk the bullets:
- "You won't see those relationships... until you hit a performance problem or a data loss incident."
- "A naive design here can generate enormous rework: extra normalization passes, change data capture pipelines..."
- "And optimizing them requires techniques that aren't obvious at creation time — indices, sharding, partitioning. None of those appear in the original CREATE TABLE."

LIKELY QUESTION: "What is a leaky abstraction?"
A: It's a term coined by Joel Spolsky: a layer designed to hide complexity always exposes some of that complexity at the boundary. OBTs are leaky because their schema looks simple but hides entity relationships that surface later as bugs.

TRANSITION TO NEXT: "But there's something important to keep in mind when you face this task."
-->
