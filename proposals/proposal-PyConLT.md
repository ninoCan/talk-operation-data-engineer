# Master the Art of Schema Dissection: Operation Data Engineer

## Elevator pitch

Have you ever faced an enormous, wide table? Sometimes you need to cope with it because it's faster in this form, and that's what your stakeholders need. Anyway, it’s hiding entities, metrics, and time semantics. Learn how the dissection framework reveals schema structure and turns risky rewrites into surgical precision.

## Abstract

Denormalized tables promise simplicity, but often hide complexity in plain sight. A single table may contain business entities, metrics, time semantics, and technical artifacts: all mixed together behind a deceptively flat schema.

This talk introduces a practical framework for dissecting denormalized tables in 1NF by classifying columns into functional roles: keys, dimensions, facts, temporal granularities, technical columns, aggregates, and indices. This perspective turns tables into understandable systems instead of mysterious collections of fields.

With this mental model, data engineers can spot leaky abstractions early and approach normalization as a deliberate, surgical process rather than a risky rewrite. If you work with Python-based data stacks and messy real-world schemas, this talk will change how you look at tables forever.
