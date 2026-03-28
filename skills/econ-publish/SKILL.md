---
name: econ-publish
description: Prepare and submit economics papers to journals. Formats manuscripts, creates submission bundles (LaTeX/Word), writes cover letters, generates blinded versions for double-blind review, and guides through editorial manager portals. Activate for /submit-paper, /cover-letter, /blind-manuscript, /journal-target.
---

# Economics journal submission and publication

Routes:
- `/submit-paper [journal]` → Read `submit-paper.md`. Prepare complete submission package for a specific journal: formatted PDF, LaTeX bundle, cover letter, blinded manuscript (if double-blind). Provides portal URL and step-by-step upload instructions.
- `/cover-letter [journal]` → Read `cover-letter.md`. Draft a professional cover letter tailored to the target journal's scope and editorial preferences.
- `/blind-manuscript` → Read `blind-manuscript.md`. Create a version of the manuscript with all author-identifying information removed for double-blind review.
- `/journal-target [topic]` → Read `journal-target.md`. Analyze a paper's findings and recommend target journals with acceptance odds, reframing advice, and submission strategy for the author's career stage.

Author info loaded from memory (Richeng Piao, Northeastern University). Uses project's `_quarto.yml` and `references.bib`.
