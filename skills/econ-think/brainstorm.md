### /brainstorm — Generate research ideas from a seed topic

**Trigger**: "brainstorm [topic]", "help me think about [topic]", "what could I study about [topic]"

**Workflow**:
1. Take the user's seed topic or question
2. Generate a structured brainstorm with 5 dimensions:

```markdown
## Brainstorm: [Topic]

### Empirical puzzles
[3–5 unexplained patterns or contradictions in the data]

### Identification opportunities
[3–5 natural experiments, policy shocks, or quasi-experimental designs
that could credibly identify causal effects in this area]

### Mechanism questions
[3–5 "why does this happen?" questions where theory is underdeveloped
or competing theories make different predictions]

### Data opportunities
[3–5 underexploited datasets, new data sources, or novel measurement
approaches for this topic — include FRED series, Census products,
admin data, or scraping targets where applicable]

### Cross-pollination
[3–5 ideas that import methods or insights from adjacent fields —
IO applied to health, labor applied to development, ML applied to
causal inference, etc.]
```

3. For each idea, append a one-line feasibility note: data availability, identification plausibility, novelty relative to existing work
4. Save to `./fleeting/brainstorm-[topic]-[YYYY-MM-DD].md`

## Output Conventions
- All outputs saved as markdown in the appropriate project directory
- All prompts use economics-specific terminology and conventions
- Cross-reference with existing ./literature/ files when available

## Example Invocation
```bash
/brainstorm minimum wage effects on small business formation
```
