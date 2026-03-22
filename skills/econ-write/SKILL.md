---
name: econ-write
description: Draft academic economics prose, write book chapters for practitioner guides, compile manuscripts via Quarto/pandoc, and self-review for prose quality. Activate for /draft-section, /book-chapter, /compile-draft, /self-review.
---

# Economics writing and book chapter drafting

Routes:
- `/draft-section [topic]` → Read `draft-section.md`. Turn notes + outline into polished prose. Follows McCloskey/Cochrane/AER rules. Integrates equations with walks. Author-date citations.
- `/book-chapter [N] [title]` → Read `book-chapter.md`. Full chapter using Angrist-Pischke template: hook → intuition → example → formal → code → extensions → exercises. Manages cross-refs and notation consistency.
- `/compile-draft [--format pdf|html|docx]` → Detect Quarto/pandoc/LaTeX. Compile. Check for unresolved references. Scaffold new Quarto project if none exists.
- `/self-review [file]` → Read `self-review.md`. 7-lens prose quality review (argument, pedagogy, math presentation, prose, citations, cross-refs, voice). Score /70 with quality gate.

Reads `STYLE_GUIDE.md` and `NOTATION.md` from project directory if they exist. Creates them on first book-chapter run.
