# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About

This is a [Slidev](https://sli.dev/) presentation for the talk "Master the Art of Schema Dissection: Operation Data Engineer", delivered at:
- PyCon LT (Lithuania) 09/04/2026 — 20 minutes
<!-- - PyCon IT (Italy) 30/05/2026 — 25 minutes --><!-- TBD -->

## Commands

```bash
pnpm install       # Install dependencies
pnpm dev           # Start dev server at http://localhost:3030
pnpm build         # Build static output to dist/
pnpm export        # Export slides to PDF/images
```

## Architecture

- `slides.md` — The entire presentation deck. Slides are separated by `---` frontmatter blocks. Slide-level config (transitions, layouts) goes in each block's frontmatter.
- `mindmap.excalidraw.svg` — Starting reference for the talk's narrative structure. Use it to understand topic flow; it is not a strict constraint.
- `pages/` — Modular slide files imported into `slides.md` via `src: ./pages/<file>.md`. Use for logically distinct sections (e.g., a chapter of the talk) to keep `slides.md` navigable. New sections should default to a separate page file unless trivially short.
- `components/` — Custom Vue components usable directly in slides via their filename (e.g., `<Counter />`).
- `snippets/` — External code files embeddable with `<<< @/snippets/file.ts` syntax.
- `proposals/` — **Read-only.** Talk proposals submitted to and accepted by conference steering committees. Use as authoritative context for title, abstract, key messages, and scope. Do not modify.
- `assets/` — Images and diagrams used in slides.

Deployment targets GitHub Pages (`.github` folder), serving the built `dist/` as a SPA.

## Content Constraints

- **Slide budget:** 25–30 slides total for the 20-minute version.
- **Timing rule of thumb:**
  - Contentful slides: ~50 seconds each
  - Transition/breaker slides: ~5 seconds each (used to create rhythm and re-engage attention)
- **Speaker notes:** Include speaker notes for every contentful slide. Notes should be written as natural spoken prose — what the speaker would actually say, not bullet summaries. Transition slides do not need notes.
- **Slide density:** Prefer one idea per slide. If a slide needs more than 5 bullet points or 2 code blocks, split it.

## Workflow Rules

- **Source of truth for structure:** The narrative arc in `mindmap.excalidraw.svg` is the starting reference. Deviate only when the user explicitly instructs it.
- **Never modify** global frontmatter (theme, transition, duration) in `slides.md` unless explicitly asked.
- **Preserve existing slides** unless the user asks for edits or regeneration. When in doubt, append rather than overwrite.
- When adding a new logical section (3+ slides on a single topic), create a file under `pages/` and import it in `slides.md` with `src: ./pages/<section-name>.md`.

## Audience & Tone

- **Audience:** Python developers attending PyCon LT. Assume solid programming literacy; do not assume data engineering expertise.
- **Tone:** Storytelling-driven with a dash of technicality. Lead with narrative and analogy; introduce technical details only when they reinforce the story.
- **Code:** Use sparingly and only when a snippet makes a concept significantly clearer than prose alone. Prefer pseudocode or simplified examples over production-grade code.
- **Humor:** Light and situational. Transition/breaker slides are the primary vehicle for it.

## Tooling Notes

- **`@mermaid-js/mermaid-cli`** — Used to render Mermaid diagrams in the build pipeline. Prefer Mermaid for any flow or sequence diagrams authored in slides.
- **`excalidraw-brute-export-cli`** — Used to export Excalidraw diagrams (e.g., `mindmap.excalidraw.svg`) to static assets for embedding in slides.
- **`cc-slidev` MCP** — Claude Code integration for Slidev (see repo: https://github.com/rhuss/cc-slidev). When available, prefer MCP tools over direct file edits for slide generation and preview operations.