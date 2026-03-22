# /debate-map — Structured Disagreement Mapping

## When to use
When papers on the same topic reach conflicting conclusions. The goal is to structure WHY they disagree — not just THAT they disagree.

## Inputs
- Topic or specific claim where literature is divided
- Paper snapshots from `./literature/` (auto-scan by tag, or user specifies files)

## Procedure

### Step 1: Identify the positions
Read relevant snapshots and classify each paper's conclusion:
- Position A: [Finding — e.g., "Minimum wage reduces employment"]
- Position B: [Opposing finding — e.g., "No significant employment effect"]
- Position C: [Nuanced — e.g., "Depends on monopsony power"] (if applicable)

### Step 2: Analyze sources of disagreement
For each pair of opposing papers, check these 6 dimensions:

| Dimension | Paper A | Paper B | Could this explain the disagreement? |
|-----------|---------|---------|--------------------------------------|
| **Data source** | [e.g., BLS payroll] | [e.g., Phone survey] | [Yes/No + explanation] |
| **Identification strategy** | [e.g., DiD state-level] | [e.g., Bordering counties] | |
| **Population/setting** | [e.g., US national] | [e.g., NJ/PA fast food] | |
| **Time period** | [e.g., 1990-2000] | [e.g., 2010-2020] | |
| **Variable definitions** | [e.g., Total employment] | [e.g., Hours worked] | |
| **Functional form** | [e.g., Log-linear] | [e.g., Semi-parametric] | |

### Step 3: Assess which dimension matters most
Usually one or two dimensions drive the disagreement. Rank them by explanatory power.

### Step 4: State what would resolve the debate
- What data would settle it?
- What identification strategy would be most convincing?
- Has anyone done the definitive study? If not, what would it look like?

### Step 5: Generate output

```markdown
## Debate Map: [Topic]
**Date**: [YYYY-MM-DD]

### The disagreement
[1-2 sentences describing the core tension]

### Position A: [Finding]
**Supported by**: [@Author1Year], [@Author2Year]
**Core evidence**: [Brief description of strongest result]
**Identification**: [Design used]

### Position B: [Opposing finding]
**Supported by**: [@Author3Year], [@Author4Year]
**Core evidence**: [Brief description]
**Identification**: [Design used]

### Sources of disagreement
| Dimension | Most likely explanation? | Evidence |
|-----------|------------------------|----------|
| Data | [Yes/No] | [Why] |
| Identification | [Yes/No] | [Why] |
| Population | [Yes/No] | [Why] |
| Time period | [Yes/No] | [Why] |
| Variable definitions | [Yes/No] | [Why] |
| Functional form | [Yes/No] | [Why] |

**Primary driver**: [The 1-2 dimensions that most plausibly explain the disagreement]

### Resolution
[What would settle this — specific data + design + population]
[Does your paper contribute to resolving this? How?]
```

### Step 6: Save
`./projects/[project]/synthesis/debate-map-[topic]-[date].md`
