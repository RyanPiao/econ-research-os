# Best Practices & Daily Workflows

This guide collects practical workflow patterns and tips for getting the most out of econ-research-os. The patterns below come from real usage across book projects, research papers, and practitioner guides. Each pattern is a self-contained session template you can follow as-is or adapt to your rhythm.

## Daily Workflow Patterns

### Pattern A: Morning reading (1-2 hours)
```
> /reading-list               # What's next?
> /read-paper [next paper]    # Deep read
> /bib add [DOI]              # Add to bibliography
> /context-save               # Save progress
```

Start by checking your reading queue, then do a deep read of the highest-priority paper. Add it to your bibliography and save session state before switching tasks. One or two papers per morning session keeps the knowledge base growing steadily without burning out.

### Pattern B: Writing sprint (2-3 hours)
```
> # Start by loading context
> "Read MEMORY.md and my outline for Chapter 6"
> /draft-section "Section 6.2: The IV estimator"
> /self-review ./drafts/6.2-iv-estimator.qmd
> /context-save
```

Always begin a writing session by loading context from your previous session. Draft one section at a time, then immediately self-review it while the logic is fresh. Save session state so tomorrow's sprint picks up cleanly.

### Pattern C: Analysis session (2-3 hours)
```
> /data-wrangle "Pull CPS state-year panel from FRED"
> /analyze "IV regression of wages on education, instrument with quarter of birth"
> /visualize "First-stage F-statistic across specifications"
> /context-save
```

Follow the data-wrangle-analyze-visualize sequence. Each step produces versioned output in `code/` and `output/`. The analysis skill generates fully documented scripts with proper clustering, robustness checks, and multiple export formats.

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

Dedicate a full session to verification before any submission or sharing. Run the internal verify pass first, then generate cross-verify prompts and send them to external AI models. Compare all reports, fix issues, then compile the final output and check project status.

## Tips

1. **Use `/clear` between different task types.** Don't mix reading and writing in the same context window. Switching between very different tasks (e.g., reading papers vs. drafting prose) in one context window leads to muddled output. Clear and reload for each task type.

2. **Run `/context-save` at the end of every session.** Your future self will thank you. This updates MEMORY.md with what was done, decisions made, and next actions. Starting a new session without this context means wasted time re-orienting.

3. **Use `--brief` for batch operations.** Save tokens when surveying many papers. When you are reading 15+ papers to get a landscape view rather than deep understanding, the brief flag produces compact 1-page snapshots that cost a fraction of the tokens.

4. **Commit daily** with descriptive messages. Your git log becomes a research diary. Descriptive commit messages like "Ch 5: added staggered DiD section with Callaway-Sant'Anna estimator" are searchable and tell a story when you look back months later.

5. **Always verify citations.** AI hallucination of references is real and dangerous in academic work. The `/verify` skill checks that every `[@citekey]` resolves in your bibliography and that the paper is accurately described, but you should also spot-check a few citations manually in each chapter.

6. **Start each session by reading MEMORY.md.** Type: "Read MEMORY.md and tell me where I left off." This single habit eliminates the most common productivity drain in AI-assisted research: losing track of where you were and re-doing work that was already completed.
