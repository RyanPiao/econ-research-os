### /deep-search — Comprehensive multi-source topic search with AI assistance

**Trigger**: "deep search [topic]", "comprehensive search on [topic]", "find everything on [topic]"

This mode combines automated API searches with web searches to build the most comprehensive picture possible. It's the "leave no stone unturned" mode for when you're starting a major project.

**Workflow**:
1. Parse topic into search dimensions:
   - **Theoretical** keywords: model names, mechanism terms
   - **Empirical** keywords: method names, data types
   - **Applied** keywords: policy names, population descriptors
   - **JEL codes**: primary + adjacent

2. Execute layered search:

   **Layer 1 — Structured API search**:
   - Semantic Scholar: topic search + field filter
   - Semantic Scholar: method-specific search

   **Layer 2 — Web search across repositories**:
   - NBER working papers
   - SSRN economics papers
   - RePEc/IDEAS
   - Google Scholar (recent 3 years)

   **Layer 3 — Survey/handbook detection**:
   - Search for: "[topic] survey" OR "[topic] handbook" OR "[topic] review"
   - These are gold — a single survey paper contains curated references

   **Layer 4 — Institutional/policy sources**:
   - Search for government reports, CBO analyses, World Bank working papers
   - Search for data documentation (codebooks, technical reports)

3. Compile and deduplicate all results
4. Classify each paper by role in the literature:
   - **Foundational**: high citations, older, establishes the framework
   - **Best design**: strongest identification strategy
   - **Best data**: richest or most novel dataset
   - **Best critique**: challenges conventional wisdom
   - **Latest**: most recent working papers
   - **Survey**: comprehensive review of the area

5. Generate a comprehensive search report:

```markdown
## Deep Search Report: [Topic]
**Date**: [YYYY-MM-DD]
**Scope**: [Description of what was searched]

### Literature landscape summary
[3–5 paragraph synthesis of what was found: how many papers,
what time period, which journals dominate, what methods are used,
what the main debates are]

### Papers by role
#### Foundational works
[Table with papers]

#### Best identification designs
[Table]

#### Best data / novel measurement
[Table]

#### Key critiques and debates
[Table]

#### Recent working papers (2024–2026)
[Table]

#### Survey articles and handbook chapters
[Table — these are the most valuable for getting oriented]

### Coverage assessment
- **Well-covered**: [Topics/methods with many papers]
- **Sparse**: [Topics/methods with few papers — potential gaps]
- **Missing**: [Topics I expected to find but didn't — could be genuine gaps or search failure]

### Recommended reading plan
**Week 1**: Read [2–3 surveys] for landscape
**Week 2**: Deep read [3–5 best design papers]
**Week 3**: Read [critiques and debates]
**Week 4**: Read [recent working papers] for frontier

### Database-specific search strings for follow-up
[Ready-to-paste queries for each platform]
```

6. Save to `./projects/[project-name]/deep-search-[topic]-[date].md`