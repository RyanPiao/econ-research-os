# Best Practices

This document collects daily workflow patterns and tips for getting the most out of econ-research-os. These patterns have been refined through actual research use and reflect how the system is designed to be used day-to-day.

## Daily workflow patterns

### Pattern A: Morning reading (1-2 hours)

A focused reading session for surveying and distilling papers.

```
> /reading-list               # What's next?
> /read-paper [next paper]    # Deep read
> /bib add [DOI]              # Add to bibliography
> /context-save               # Save progress
```

### Pattern B: Writing sprint (2-3 hours)

A dedicated writing block. Start by reloading context from the previous session, then draft and self-review.

```
> # Start by loading context
> "Read MEMORY.md and my outline for Chapter 6"
> /draft-section "Section 6.2: The IV estimator"
> /self-review ./drafts/6.2-iv-estimator.qmd
> /context-save
```

### Pattern C: Analysis session (2-3 hours)

Data work: wrangling, estimation, and visualization in a single focused block.

```
> /data-wrangle "Pull CPS state-year panel from FRED"
> /analyze "IV regression of wages on education, instrument with quarter of birth"
> /visualize "First-stage F-statistic across specifications"
> /context-save
```

### Pattern D: Verification day (before submission)

A full quality-assurance pass. Run this before submitting a paper or finalizing a chapter.

```
> /verify ./chapters/05-did.qmd
> /verify-code ./code/ch05_did_estimation.py
> /cross-verify ./chapters/05-did.qmd
> # Paste prompts into Gemini + ChatGPT
> # Compare external results with /verify report
> /compile-draft --format pdf
> /status
```

## Tips

1. **Use `/clear` between different task types.** Don't mix reading and writing in the same context window. Switching tasks mid-session degrades output quality.

2. **Run `/context-save` at the end of every session.** Your future self will thank you. This updates MEMORY.md with what was done, decisions made, and next actions.

3. **Use `--brief` for batch operations.** Save tokens when surveying many papers. Full deep reads should be reserved for your highest-priority papers.

4. **Commit daily** with descriptive messages. Your git log becomes a research diary that you can search and reference later.

5. **Always verify citations.** AI hallucination of references is real and dangerous in academic work. The `/verify` skill checks that every `[@citekey]` resolves and is accurately described, but a manual spot-check is still good practice.

6. **Start each session by reading MEMORY.md.** Type: "Read MEMORY.md and tell me where I left off." This restores context from your previous session without wasting tokens on re-exploration.
