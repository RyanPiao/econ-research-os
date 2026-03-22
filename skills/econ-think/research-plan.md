### /research-plan — Structure a research project from idea to execution

**Trigger**: "plan my research on [topic]", "help me plan a paper about [topic]", "research plan for [topic]"

**Workflow**:
1. Ask the user for (or infer from context):
   - Research question (one sentence)
   - Proposed identification strategy
   - Target output: journal paper, book chapter, course paper, consulting report
   - Timeline: weeks available

2. Generate a structured research plan:

```markdown
## Research Plan: [Title]
**Date**: [YYYY-MM-DD]
**Target output**: [paper/chapter/report]
**Timeline**: [X weeks]

### 1. Research question
[Precise one-sentence question]

### 2. Proposed answer / hypothesis
[What you expect to find and why]

### 3. Identification strategy
- **Design**: [DiD / IV / RD / RCT / structural / descriptive]
- **Key variation**: [What creates the identifying variation]
- **Key assumption**: [What must hold for this to be credible]
- **Biggest threat**: [Most likely failure mode]

### 4. Data requirements
- **Unit of observation**:
- **Time period needed**:
- **Key variables**:
- **Candidate datasets**: [with access notes]
- **Data gaps to resolve**:

### 5. Literature positioning (preliminary)
- **Strand 1**: [Topic — 3–5 key papers to read]
- **Strand 2**: [Topic — 3–5 key papers to read]
- **Your marginal contribution**: [One sentence]

### 6. Task breakdown
| Week | Task | Deliverable |
|---|---|---|
| 1 | Literature search + reading | Reading list + 10 paper snapshots |
| 2 | Data acquisition + cleaning | Clean dataset + summary stats |
| 3 | Baseline estimation | Main regression table |
| 4 | Robustness + writing | Draft paper |

### 7. Risk register
| Risk | Probability | Mitigation |
|---|---|---|
| [Data unavailable] | [Med] | [Alternative source] |
| [Identification fails] | [Low/Med/High] | [Backup design] |
| [Null result] | [Med] | [Frame as informative null] |

### 8. Immediate next actions
- [ ] [Action 1]
- [ ] [Action 2]
- [ ] [Action 3]
```

3. Save to `./projects/[project-name]/research-plan.md`

### /scope-and-outline — Define the scope and structure of a book or paper

**Trigger**: "outline my book on [topic]", "structure my paper on [topic]", "help me scope [project]"

**Workflow**:
1. Gather: topic, target audience, target length, number of chapters/sections
2. Generate hierarchical outline with:
   - Chapter/section titles
   - 2–3 sentence description of each section's argument
   - Key papers needed for each section (cross-reference with ./literature/ if files exist)
   - Estimated word count per section
   - Dependencies between sections (which must be written first)
3. Save to `./projects/[project-name]/outline.md`

## Output Conventions
- All outputs saved as markdown in the appropriate project directory
- Create `./projects/[project-name]/` automatically if it doesn't exist
- All prompts use economics-specific terminology and conventions
- Cross-reference with existing ./literature/ files when available
- Include JEL codes in all literature-related prompts

## Example Invocations
```bash
/research-plan "Effect of occupational licensing on labor mobility"
/scope-and-outline "Applied Econometrics: A Practitioner's Guide" --chapters 12
```
