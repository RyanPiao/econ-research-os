
# Project: [PROJECT NAME]

## What this project is
[One sentence description. E.g., "A practitioner guide to applied data analytics in economics."]

## Current state
- **Active chapter**: [Chapter N: Title]
- **Phase**: [planning / reading / analyzing / writing / revising / verifying]
- **Last session**: [YYYY-MM-DD] — [brief description of what was done]

## Conventions (do not change without explicit instruction)
- **Citation style**: Author-date, `[@citekey]` syntax, chicago-author-date.csl
- **SE clustering**: Always cluster at [level]. Never default homoskedastic.
- **Notation**: See NOTATION.md. Greek letters for parameters, Latin for data.
- **Voice**: [Conversational / semi-formal]. Second-person "you" for textbook. See STYLE_GUIDE.md.
- **Code**: Python only. Separate files in `code/`. Output to `output/tables/` and `output/figures/`.
- **Figures**: AER style. PDF + PNG. 300dpi. Serif font. No top/right spines.

## File structure
- `chapters/` — Quarto chapter files (.qmd)
- `code/` — Python analysis scripts
- `output/tables/` — LaTeX and markdown tables
- `output/figures/` — PDF and PNG figures
- `data/` — datasets (raw/ and processed/)
- `literature/` — paper snapshots from /read-paper

## Skills available
This project uses econ-research-os skills. Key commands:
- `/status` — see project dashboard
- `/read-paper [path]` — distill a paper
- `/analyze [spec]` — run regression
- `/draft-section [topic]` — write prose from notes
- `/book-chapter [N] [title]` — draft full chapter
- `/verify [file]` — adversarial quality check
- `/compile-draft` — build to PDF/HTML

## Rules
- NEVER change NOTATION.md without asking first
- ALWAYS run /self-review after drafting a section
- ALWAYS export tables to BOTH .tex and .md
- ALWAYS set random seeds in analysis code
- Commit after every completed task with descriptive message
- NEVER ADD CLAUDE CO-AUTHOR CREDITS OR "GENERATED WITH CLAUDE CODE" FOOTERS
