### /paper-search — Find papers on a topic across multiple databases

**Trigger**: "find papers on [topic]", "search for papers about [topic]", "what's been published on [topic]"

**Workflow**:
1. Parse the user's topic into search components:
   - **Core keywords** (2–4 terms)
   - **Method keywords** (if specified: DiD, IV, RCT, structural, etc.)
   - **JEL codes** (infer from topic — see JEL mapping below)
   - **Date range** (default: 2015–2026 unless specified)
   - **Field filter** (labor, IO, public finance, development, macro, etc.)

2. Execute searches across available platforms IN THIS ORDER (most reliable first):

   **a. Semantic Scholar API** (free, programmatic):
   ```bash
   curl -s "https://api.semanticscholar.org/graph/v1/paper/search?query=[KEYWORDS]&fields=title,authors,year,citationCount,journal,externalIds,abstract&limit=20&fieldsOfStudy=Economics" \
     | python3 -c "import sys,json; data=json.load(sys.stdin); [print(f\"{p['title']} ({p['year']}) — citations: {p.get('citationCount',0)} — {p.get('journal',{}).get('name','')}\") for p in data.get('data',[])]"
   ```

   **b. Web search for Google Scholar** (broader coverage):
   ```
   Search: site:scholar.google.com "[keyword1]" "[keyword2]" economics
   ```

   **c. Web search for NBER Working Papers**:
   ```
   Search: site:nber.org "[keyword]" working paper
   ```

   **d. Web search for SSRN**:
   ```
   Search: site:ssrn.com "[keyword1]" "[keyword2]" economics
   ```

   **e. Web search for RePEc/IDEAS**:
   ```
   Search: site:ideas.repec.org "[keyword]"
   ```

3. Deduplicate results (match on title similarity)
4. Rank by: citation count × recency weight (papers after 2020 get 2x boost)
5. Generate a structured reading list:

```markdown
## Paper Search Results: [Topic]
**Date**: [YYYY-MM-DD]
**Query**: [keywords used]
**JEL codes**: [codes searched]
**Sources checked**: Semantic Scholar, Google Scholar, NBER, SSRN, RePEc

### Tier 1: High-priority reads (highly cited + recent)
| # | Authors | Year | Title | Journal/Series | Citations | Method |
|---|---------|------|-------|---------------|-----------|--------|
| 1 | | | | | | |

### Tier 2: Important background
| # | Authors | Year | Title | Journal/Series | Citations | Method |
|---|---------|------|-------|---------------|-----------|--------|

### Tier 3: Exploratory / recent working papers
| # | Authors | Year | Title | Series | Method |
|---|---------|------|-------|--------|--------|

### Suggested next steps
- [ ] Download and /read-paper on Tier 1 papers
- [ ] /citation-chase on the top-cited paper
- [ ] Check [specific database] for [specific gap]

### Search strings for manual follow-up
**Scopus**: `TITLE-ABS-KEY([terms]) AND SUBJAREA(ECON) AND PUBYEAR > 2019`
**Web of Science**: `TS=([terms]) AND WC=(Economics)`
**EconLit**: `SU [JEL code] AND KW [terms]`
```

6. Save to `./projects/[project-name]/search-results-[topic]-[date].md`
