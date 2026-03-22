### /citation-chase — Expand from a seed paper via citation network

**Trigger**: "citation chase [paper]", "who cites [paper]", "expand from [paper]", "snowball from [paper]"

**Workflow**:
1. Identify the seed paper (from user input or from a file in ./literature/)
2. Extract: title, authors, year, DOI or Semantic Scholar ID

3. **Backward chasing** (what does this paper cite?):
   ```bash
   # Get references via Semantic Scholar
   curl -s "https://api.semanticscholar.org/graph/v1/paper/[PAPER_ID]/references?fields=title,authors,year,citationCount,journal&limit=50"
   ```
   - Filter for economics journals and working paper series
   - Rank by citation count
   - Flag papers that appear in multiple reference lists (canonical works)

4. **Forward chasing** (who cites this paper?):
   ```bash
   # Get citations via Semantic Scholar
   curl -s "https://api.semanticscholar.org/graph/v1/paper/[PAPER_ID]/citations?fields=title,authors,year,citationCount,journal&limit=50"
   ```
   - Sort by year (newest first) to find latest developments
   - Flag highly-cited recent papers (potential new canonical works)

5. **Network analysis**:
   - Identify papers that appear in BOTH backward and forward chains (core literature)
   - Find clusters: papers that cite each other frequently
   - Identify the "closest" papers: those sharing the most references with the seed

6. Generate citation map:

```markdown
## Citation Chase: [Seed Paper]
**Seed**: [Authors (Year)] — "[Title]"
**Date**: [YYYY-MM-DD]

### Most important backward references (this paper builds on)
| # | Authors | Year | Title | Why important |
|---|---------|------|-------|--------------|
| 1 | | | | Foundational theory |
| 2 | | | | Method source |
| 3 | | | | Closest prior empirical work |

### Most important forward citations (builds on this paper)
| # | Authors | Year | Title | What they add |
|---|---------|------|-------|--------------|
| 1 | | | | Extends to new setting |
| 2 | | | | Challenges finding |
| 3 | | | | Improves method |

### Core cluster (appear in both directions)
[Papers that both cite and are cited by papers in the network]

### Closest neighbors
[Papers sharing 5+ references with the seed — highest overlap = closest competitor/complement]

### Gaps identified
- [Topic/method/setting not yet explored in this network]

### Recommended reading order
1. [Paper] — read first because [reason]
2. [Paper] — read second because [builds on #1]
3. [Paper] — read third because [contrasts with #1-2]
```

7. Save to `./projects/[project-name]/citation-chase-[seed-author-year].md`
8. Optionally: auto-add Tier 1 papers to the reading list