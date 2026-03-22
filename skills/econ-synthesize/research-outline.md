# /research-outline — Generate Paper or Chapter Outline from Synthesis

## Inputs
- Gap analysis from `/gap-finder`
- Lit review positioning from `/lit-review`
- Research plan from `/research-plan` (if exists)
- User's research question and contribution

## Procedure

### Step 1: Determine output type
- **Journal paper outline**: Introduction (hook, question, contribution, roadmap) → Literature → Model/Theory → Data → Empirical Strategy → Results → Robustness → Conclusion
- **Book chapter outline**: Hook → Intuition → Canonical Example → Formal Framework → Implementation → Extensions → Exercises
- **Consulting report outline**: Executive Summary → Background → Method → Findings → Implications → Recommendations

### Step 2: Map content to sections
For each section in the outline:
- Which paper snapshots provide the content?
- Which notes from `./notes/` are relevant?
- Which analysis output (tables/figures) goes here?
- What still needs to be written from scratch?

### Step 3: Generate the outline

```markdown
## Outline: [Title]
**Type**: [paper / chapter / report]
**Date**: [YYYY-MM-DD]

### Section map
| Section | Content source | Status | Estimated words |
|---------|---------------|--------|-----------------|
| 1. Introduction | brainstorm + lit-review draft | needs writing | 1,500 |
| 2. Background | paper snapshots @A, @B, @C | notes exist | 2,000 |
| 3. Data | /data-wrangle output + codebook | code exists | 1,000 |
| 4. Method | /analyze specification | code exists, prose needed | 2,000 |
| 5. Results | /analyze output tables/figures | output exists | 2,500 |
| 6. Robustness | /analyze robustness checks | partially done | 1,500 |
| 7. Conclusion | gap analysis + own synthesis | needs writing | 1,000 |

### Detailed outline
#### 1. Introduction (~1,500 words)
- 1.1 Hook: [The specific puzzle or question — 1 paragraph]
- 1.2 What we do: [Research question + method + preview of results — 1 paragraph]
- 1.3 What we find: [Main result + magnitude + significance — 1 paragraph]
- 1.4 Contribution: [2-3 strands from /lit-review draft — 1-3 paragraphs]
- 1.5 Roadmap: [1 paragraph]

#### 2. Background (~2,000 words)
- 2.1 [Institutional context — from paper snapshots + ChatGPT fact-check]
- 2.2 [Related literature — from /lit-review, organized by theme]
...

### Dependencies
[Which sections must be written before others?
E.g., Data before Results; Introduction last (after results are known)]

### Writing order (recommended)
1. Data section (straightforward, grounds everything)
2. Method section (specification is known)
3. Results section (tables exist)
4. Robustness (extension of results)
5. Background/literature (positioning known after results)
6. Introduction (write last — you now know what the paper actually says)
7. Conclusion (summary + implications)
```

### Step 4: Save
`./projects/[project]/outline.md`
