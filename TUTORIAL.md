# Tutorial: From Research Idea to Published Chapter

This walkthrough takes you through the complete econ-research-os pipeline using a real example: writing a chapter on difference-in-differences for a practitioner guide. Each section can be used independently — skip to whatever stage you need.

## Prerequisites

- Claude Code installed and running
- Skills installed per the [README quickstart](README.md#quickstart-5-minutes)
- Semantic Scholar MCP server connected

## Stage 0: Think — "What should I write about?"

### Start a new project

```bash
cd ~/my-book
claude

> /new-project "Applied Data Analytics in Economics"
```

Claude scaffolds the full directory structure: `_quarto.yml`, `CLAUDE.md`, `MEMORY.md`, `STYLE_GUIDE.md`, `NOTATION.md`, chapter templates, `.gitignore`, and initializes git.

### Brainstorm the chapter

```
> /brainstorm difference-in-differences recent advances
```

**What you get** — a structured brainstorm saved to `./fleeting/brainstorm-did-2026-03-22.md`:

```markdown
## Brainstorm: Difference-in-Differences

### Empirical puzzles
- Why do TWFE estimates flip sign with heterogeneous treatment effects?
- Staggered adoption creates negative weighting — how widespread is this in published work?
- Pre-trend tests have low power — what fraction of DiD papers would fail with better tests?
...

### Identification opportunities
- State-level policy rollouts with clear timing variation
- Minimum wage increases staggered across cities (post-2020 data now available)
...
```

### Generate prompts for external AI tools

```
> /prompt-gen DiD staggered treatment heterogeneous effects
```

**What you get** — 6 ready-to-paste prompts saved to `./projects/did-chapter/deep-research-prompts.md`:

- 2 for Gemini Deep Research (broad literature sweep + data sources)
- 2 for ChatGPT (policy details + methodology deep dive)
- 2 for Claude Deep Research (theoretical synthesis + critical appraisal)

**What you do next**: Open Gemini in another tab, paste Prompt 1, let it run. Open ChatGPT, paste Prompt 1, let it run. Continue working in Claude Code while they process.

---

## Stage 1: Discover — "What papers exist?"

### Search across databases

```
> /paper-search difference-in-differences staggered treatment --years 2018-2026
```

**What you get** — a tiered reading list saved to `./projects/did-chapter/search-results-did-2026-03-22.md`:

```markdown
### Tier 1: High-priority reads
| # | Authors | Year | Title | Citations | Method |
|---|---------|------|-------|-----------|--------|
| 1 | Callaway & Sant'Anna | 2021 | DiD with Multiple Time Periods | 2,847 | DiD |
| 2 | Goodman-Bacon | 2021 | Difference-in-Differences with Variation in Treatment Timing | 2,103 | DiD |
| 3 | Sun & Abraham | 2021 | Estimating Dynamic Treatment Effects... | 1,456 | DiD |
...
```

### Citation chase from a seed paper

```
> /citation-chase "Callaway Sant'Anna 2021"
```

Claude uses the Semantic Scholar MCP to trace backward (what they built on) and forward (who built on them). Identifies the core cluster and closest neighbors.

### Build your reading list

```
> /reading-list
```

Aggregates all search results, deduplicates, cross-references with any papers you've already read in `./literature/`, and produces a prioritized queue.

---

## Stage 2: Ingest — "What do these papers say?"

### Read a paper

```
> /read-paper ./papers/callaway-santanna-2021.pdf
```

**What you get** — a full paper snapshot saved to `./literature/CallawayaSantAnna2021-DiDMultiplePeriods.md`:

```markdown
---
citekey: CallawayaSantAnna2021
title: "Difference-in-Differences with Multiple Time Periods"
authors: [Brantly Callaway, Pedro H.C. Sant'Anna]
year: 2021
journal: "Journal of Econometrics"
tags: [literature_note, did, staggered, panel]
methodology: diff-in-diff
relevance: 5
---

# Callaway & Sant'Anna (2021) — DiD with Multiple Time Periods

## Executive Summary
### Research Question
How should researchers estimate treatment effects when units adopt
treatment at different times?

### Identification Strategy
Group-time ATTs estimated separately for each cohort-period pair,
then aggregated. Requires parallel trends conditional on covariates
for each group relative to not-yet-treated or never-treated units.

### Headline Result
TWFE can produce biased estimates under heterogeneous treatment effects;
their disaggregated approach avoids negative weighting...
```

Plus Dataset Card, Regression Table Decode, Contribution Map, Gap Analysis, and Citation Seeds.

### For a long handbook chapter (80+ pages)

```
> /split-pdf ./papers/handbook-econometrics-ch22-did.pdf
```

Claude chunks the PDF into ~15-page sections, reads each without crashing the session, and produces a consolidated summary.

### Brief mode for batch reading

```
> /read-paper ./papers/reading-list/ --brief
```

Produces compact 1-page snapshots for each paper in the directory — saves tokens when you're surveying 15+ papers and don't need deep analysis on every one.

---

## Stage 3: Analyze — "Let me run the numbers"

### Run a regression

```
> /analyze "DiD estimation of minimum wage on teen employment, 
  state-year panel, cluster SEs at state level, include state 
  and year FE"
```

**What you get** — a fully documented Python script saved to `./projects/did-chapter/code/ch05_did_estimation.py`:

```python
"""
Analysis: DiD estimation of minimum wage effects
Date: 2026-03-22
Data: CPS state-year panel
Specification: emp_rate = β₀ + β₁*min_wage + state_FE + year_FE + ε
"""
import pandas as pd
import statsmodels.formula.api as smf
from linearmodels.panel import PanelOLS
...
```

With proper clustering, multiple export formats, and robustness checks.

### Create a publication figure

```
> /visualize "Event study plot showing leads and lags around 
  minimum wage increase"
```

Generates an event study figure following AER style conventions, saved as both PDF (for the book) and PNG (for the web).

### Monte Carlo simulation

```
> /simulate "Show OLS bias under parallel trends violation vs 
  Callaway-Sant'Anna estimator robustness"
```

Generates a parallelized simulation with 2,000 iterations comparing estimator performance, with distribution plots.

---

## Stage 4: Synthesize — "What does the literature say?"

### Draft the contribution section

```
> /lit-review --type embedded --question "Modern DiD estimators" 
  --contribution "unified tutorial comparing all major estimators 
  on the same dataset"
```

**What you get** — a 2-3 paragraph contribution section following economics conventions:

```markdown
This chapter contributes to two strands of the applied econometrics
literature. First, we engage with the rapidly growing body of work
on difference-in-differences under staggered treatment adoption...

Second, we contribute to the practitioner-oriented methods literature...

Relative to the most closely related work by @CallawayaSantAnna2021,
our chapter differs in providing side-by-side Python implementations
of all major estimators applied to the same dataset...
```

### Map a debate

```
> /debate-map "TWFE bias under treatment effect heterogeneity"
```

Structures the disagreement: who argues what, why they disagree, and what would resolve it.

---

## Stage 5: Write — "Time to draft"

### Draft a full chapter

```
> /book-chapter 5 "Difference-in-Differences"
```

Claude drafts each section following the Angrist-Pischke chapter template:

1. **Opening hook**: The Card & Krueger minimum wage story
2. **Intuition**: Why comparing before-and-after isn't enough
3. **Canonical example**: NJ vs PA fast food employment
4. **Formal framework**: The DiD estimator with equation walks
5. **Implementation**: Python code with `linearmodels`
6. **Extensions**: Staggered adoption, modern estimators
7. **Exercises**: Conceptual + applied problems

### Self-review the prose

```
> /self-review ./projects/mybook/chapters/05-did.qmd
```

Runs 7 lenses (argument structure, pedagogy, math presentation, prose quality, citations, cross-refs, voice) and scores each /10.

---

## Stage 6: Verify — "Is this actually correct?"

### Adversarial verification

```
> /verify ./projects/mybook/chapters/05-did.qmd
```

Runs 6 substance-checking lenses:
- Math: checks every equation step-by-step, tests boundary cases
- Empirical: cross-references numbers in text against code output
- Causal logic: audits identification assumptions
- Citations: verifies every `[@citekey]` resolves and is accurately described
- Code pipeline: checks for stale output, missing seeds
- Overclaiming: checks if conclusions follow from evidence

**What you get** — a scored report with PASS/REVISE/MAJOR REVISION verdict:

```markdown
## Verdict: REVISE (2 critical, 4 important, 6 minor)

### Critical issues
1. Eq (5.3): sign error on δ — should be negative given the
   mechanism described in §5.2. [Line 147]
2. Table 5.1 reports N=12,450 but code output shows N=12,387.
   Code was updated after text was written. [Line 203]
```

### Cross-verify with other AI models

```
> /cross-verify ./projects/mybook/chapters/05-did.qmd
```

Generates targeted prompts for Gemini, ChatGPT, and Claude Deep Research. Copy-paste into each platform for independent verification.

---

## Stage 7: Publish — "Ship it"

### Compile the manuscript

```
> /compile-draft --format pdf
> /compile-draft --format html
```

### Check project status

```
> /status
```

```markdown
## Project Status: Applied Data Analytics in Economics
Last updated: 2026-03-22

### Chapters
| Ch | Title | Draft | Reviewed | Verified | Compiled |
|----|-------|-------|----------|----------|----------|
| 1  | Introduction | ✅ | ✅ | ✅ | ✅ |
| 2  | Regression | ✅ | ✅ | ⬜ | ⬜ |
| 5  | DiD | ✅ | ✅ | ✅ | ✅ |
| 6  | IV | ⬜ | ⬜ | ⬜ | ⬜ |

### Knowledge base
- Papers read: 34
- Papers queued: 12
- Atomic notes: 67

### Stale outputs (code newer than output)
- code/ch02_regression.py → output/tables/reg_results.tex (STALE)

### Next actions
1. Verify Chapter 2
2. Start drafting Chapter 6 (IV)
3. Update stale output for Chapter 2
```

### Save session state and commit

```
> /context-save
```

Updates MEMORY.md with what was done, decisions made, and next actions.

```bash
git add .
git commit -m "Ch 5: DiD chapter verified and compiled"
git push origin develop
```

---

## Daily workflow patterns

### Pattern A: Morning reading (1-2 hours)
```
> /reading-list               # What's next?
> /read-paper [next paper]    # Deep read
> /bib add [DOI]              # Add to bibliography
> /context-save               # Save progress
```

### Pattern B: Writing sprint (2-3 hours)
```
> # Start by loading context
> "Read MEMORY.md and my outline for Chapter 6"
> /draft-section "Section 6.2: The IV estimator"
> /self-review ./drafts/6.2-iv-estimator.qmd
> /context-save
```

### Pattern C: Analysis session (2-3 hours)
```
> /data-wrangle "Pull CPS state-year panel from FRED"
> /analyze "IV regression of wages on education, instrument with quarter of birth"
> /visualize "First-stage F-statistic across specifications"
> /context-save
```

### Pattern D: Verification day (before submission)
```
> /verify ./chapters/05-did.qmd
> /verify-code ./code/ch05_did_estimation.py
> /cross-verify ./chapters/05-did.qmd
> # Paste prompts into Gemini + ChatGPT
> # Compare external results with /verify report
> /compile-draft --format pdf
> /status
```

---

## Tips

1. **Use `/clear` between different task types.** Don't mix reading and writing in the same context window.
2. **Run `/context-save` at the end of every session.** Your future self will thank you.
3. **Use `--brief` for batch operations.** Save tokens when surveying many papers.
4. **Commit daily** with descriptive messages. Your git log becomes a research diary.
5. **Always verify citations.** AI hallucination of references is real and dangerous in academic work.
6. **Start each session by reading MEMORY.md.** Type: "Read MEMORY.md and tell me where I left off."