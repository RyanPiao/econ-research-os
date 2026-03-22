---
name: econ-utils
description: Project management utilities for economics research. Dashboard, bibliography management, project/chapter scaffolding, session management. Activate for /status, /bib, /new-chapter, /new-project, /context-save.
---

# Economics research project utilities

Routes:
- `/status` → Read `status.md`. Scan project directory. Report: chapters (drafted/reviewed/verified/compiled), papers (read/queued), stale outputs, next actions. Display as compact dashboard.
- `/bib [action]` → Read `bib.md`.
  - `/bib add [DOI|URL|title]` — Add entry to references.bib (fetch metadata from Semantic Scholar/Crossref)
  - `/bib check [chapter]` — Verify all [@citekey] in chapter exist in .bib
  - `/bib dedup` — Find and merge duplicate entries
  - `/bib update-wp` — Check if any working papers now have published versions
- `/new-chapter [N] [title]` → Scaffold: .qmd file with YAML frontmatter, code/ and output/ subdirs, outline entry, notation table update.
- `/new-project [name]` → Scaffold: full Quarto book project from templates/new-project/. Initialize git. Create CLAUDE.md, MEMORY.md, STYLE_GUIDE.md, NOTATION.md.
- `/context-save` → Snapshot current working state to MEMORY.md. Record: what was done this session, decisions made, next actions, open questions.

Templates in `templates/` directory.
