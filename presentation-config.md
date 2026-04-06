---
# Presentation Framing Configuration
# Generated: 2026-04-06

# Core Parameters
duration_minutes: 20
time_per_contentful_slide_seconds: 50
time_per_transition_slide_seconds: 5
target_slide_count: 28
slide_count_range: [25, 30]

# Slide breakdown
contentful_slides: 22
transition_breaker_slides: 6
# Timing: (22 × 50) + (6 × 5) = 1100 + 30 = 1130 sec ≈ 18.8 min

# Audience
audience: "Python developers"
skill_level: "intermediate"
context: "conference"
venue: "PyCon LT 2026, Lithuania"
date: "2026-04-09"

# Style
style: "technical"
theme: "default"  # do not change unless explicitly asked
tone: "storytelling-driven with a dash of technicality"
humor: "light and situational — primarily in transition/breaker slides"
visuals: "diagrams-only"  # Mermaid diagrams, Excalidraw; no stock photos

# Timing Details
# Rule of thumb:
#   contentful slides: ~50 seconds each
#   transition/breaker slides: ~5 seconds each
backup_slides_recommended: 3

# Calculation Notes
# Formula: (contentful × 50) + (transition × 5) ≈ target_duration_seconds
# (22 × 50) + (6 × 5) = 1130 sec ≈ 18.8 min ✓
# Flexibility: ±2 slides = 25–30 slide range
---

# Presentation Scope

**Title**: Master the Art of Schema Dissection: Operation Data Engineer

## Duration & Pacing

- **Total time**: 20 minutes
- **Contentful slides**: ~50 seconds each
- **Transition/breaker slides**: ~5 seconds each
- **Target slides**: ~28 slides (22 contentful + 6 breakers)
- **Acceptable range**: 25–30 slides

## Audience

- **Who**: Python developers
- **Skill level**: Intermediate — solid programming literacy; no assumed data engineering expertise
- **Context**: Conference talk (PyCon LT 2026, Lithuania)
- **Expectations**: Leave with a practical mental model for reading and dissecting denormalized schemas; understand normalization as deliberate surgery, not risky rewrite

## Style Preferences

- **Tone**: Storytelling-driven with technicality introduced only when it reinforces the story
- **Humor**: Light and situational — reserved for transition/breaker slides
- **Slidev theme**: default (do not change unless asked)
- **Visual richness**: Diagrams only — Mermaid for flows/sequences, Excalidraw for conceptual diagrams; no stock photos

## Code Usage

- Use sparingly — only when a snippet makes a concept significantly clearer than prose
- Prefer pseudocode or simplified examples over production-grade code

## Structure Guidance

Recommended distribution (3-act, ~28 slides):
- **Opening** (15% → ~4 slides): Hook, the problem of wide tables, talk promise
- **Main content** (70% → ~20 slides): The dissection framework — column classification, each role (keys, dimensions, facts, temporal, technical, aggregates, indices), leaky abstraction detection, normalization as surgery
- **Closing** (15% → ~4 slides): Summary, key takeaways, call to action
- **Backup** (not counted): 3 slides — edge cases, extra detail on specific column types

## Key Messages (from proposal)

1. Denormalized tables hide complexity — entities, metrics, time semantics, and technical artifacts mixed together
2. Classifying columns into functional roles (keys, dimensions, facts, temporal, technical, aggregates, indices) turns tables into understandable systems
3. With this mental model, normalization becomes surgical precision, not a risky rewrite

## Next Steps

1. Review / adjust this config
2. Create outline: `/slidev:outline`
3. Generate slides: `/slidev:generate`
