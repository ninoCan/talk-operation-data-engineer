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
- `mindmap.excalidraw.svg` - Diagram containing the outline of the talk. This should be the basis of the generation of content.
- `pages/` - Folder containing the slides as separate markdown files
- `components/` — Custom Vue components usable directly in slides via their filename (e.g., `<Counter />`).
- `snippets/` — External code files that can be embedded into slides with the `<<< @/snippets/file.ts` syntax.
- `proposals/` — Talk proposal documents for each conference (not part of the slide build).
- `assets/` - Pictures and diagrams to be used in the slides

Deployment targets GitHub Pages (`.github` folder), serving the built `dist/` as a SPA.
