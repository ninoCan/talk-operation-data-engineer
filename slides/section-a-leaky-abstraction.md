---

# Tables promise simplicity — and then complexity leaks through

Designed to hide structure, they transfer it instead:

- **Indices** to design and maintain
- **Query patterns** to optimize
- **Normalization** decisions that come back later

> Like a pandas DataFrame that silently hits memory limits — the execution model always leaks through.

<!-- TODO: Visual - MEDIUM PRIORITY
Type: Simple diagram — abstraction layer with arrows "leaking" through it
Why: Makes the "leaky abstraction" metaphor concrete before the technical content
Element count: 1 diagram + 1 quote = 2 elements ✓
-->

<!--
Joel Spolsky coined the phrase "leaky abstraction" — every abstraction, no matter how well designed, eventually leaks details of the underlying implementation.
Tables are a leaky abstraction.
They're supposed to give you a clean, simple interface to your data. Instead, they transfer the complexity to you in a different form.
You need indices to make them fast. You need to understand query patterns to use them correctly. And when you need to change them, all the normalization decisions you ignored come back to haunt you.
If you've written a pandas groupby on a wide DataFrame and watched your machine start sweating — you've felt this. The table looked flat and simple. The execution model was leaking through.
-->
