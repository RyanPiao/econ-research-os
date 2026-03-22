---
name: econ-synthesize
description: Synthesize paper snapshots into literature reviews, debate maps, gap analyses, and research outlines. Activate for /lit-review, /debate-map, /gap-finder, /research-outline.
---

# Economics literature synthesis

Routes:
- `/lit-review [--type embedded|standalone]` → Read `lit-review.md`. Embedded = 1-3 paragraph contribution section (Cochrane/Bellemare convention). Standalone = full JEL-style survey with thematic sections and comparison tables.
- `/debate-map [topic]` → Read `debate-map.md`. Structure disagreements: Position A vs B, sources of disagreement, resolution.
- `/gap-finder [--scan ./literature/]` → Scan paper snapshots. Identify boundary/mechanism/measurement/design gaps. Generate testable next steps.
- `/research-outline [topic]` → Generate paper or chapter outline from gap analysis + lit review positioning.

Input: reads from `./literature/` paper snapshots. Output: saves to `./projects/[name]/`.
