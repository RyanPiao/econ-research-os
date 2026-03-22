### /prompt-gen — Generate optimized prompts for external deep research tools

**Trigger**: "generate prompts for [topic]", "make me deep research prompts", "prep prompts for Gemini/ChatGPT about [topic]"

This is the key bridge to external AI tools. Each platform has different strengths and the prompts should be tailored accordingly.

**Workflow**:
1. Take the user's research topic/question
2. Identify what information is needed: background literature, current debates, data sources, methodological precedents, institutional context
3. Generate platform-specific prompts:

```markdown
## Deep Research Prompts: [Topic]
**Generated**: [YYYY-MM-DD]
**Purpose**: [What you need to learn]

---

### For Google Gemini Deep Research
[Gemini excels at broad literature sweeps, finding recent papers,
and synthesizing across many sources. Optimize for breadth.]

**Prompt 1 — Literature landscape**:
```
I am an economics graduate student researching [TOPIC]. I need a
comprehensive survey of the academic economics literature on [SPECIFIC
QUESTION]. Please:

1. Identify the 15-20 most influential papers on this topic, organized
   by methodology (theoretical, reduced-form empirical, structural).
2. For each paper, provide: authors, year, journal, identification
   strategy (if empirical), and the main finding in one sentence.
3. Identify the 3 main debates or unresolved questions in this
   literature.
4. List any recent working papers (2023-2026) from NBER, CEPR, or
   IZA that extend this work.
5. Suggest JEL codes I should use to search EconLit for this topic.

Focus on papers published in top-5 economics journals (AER, QJE,
JPE, Econometrica, ReStud) plus top field journals in [SUBFIELD].
```

**Prompt 2 — Data and identification**:
```
I am planning an empirical economics paper on [SPECIFIC QUESTION]
using [PROPOSED DESIGN]. Please:

1. What datasets have other researchers used to study this question?
   For each: name, coverage, unit of observation, access method,
   and any known limitations.
2. What natural experiments or policy changes could provide
   identifying variation for this question? Include specific dates,
   jurisdictions, and which papers have exploited them.
3. What are the main identification threats for [PROPOSED DESIGN]
   in this context, and how have other researchers addressed them?
4. Are there any recent methodological advances (2022-2026) in
   [DiD/IV/RD/etc.] that would be relevant?
```

---

### For ChatGPT Deep Research
[ChatGPT with browsing excels at finding specific documents, recent
news, policy details, and institutional facts. Optimize for precision.]

**Prompt 1 — Institutional and policy context**:
```
I need detailed institutional background for an economics research
paper on [TOPIC]. Please search for and compile:

1. The specific policy/regulation/institutional change I am studying:
   [DESCRIPTION]. Find the exact dates, legislative history, and
   implementation details.
2. Any government reports, GAO studies, or CBO analyses related to
   this policy.
3. Data sources: search for microdata from [Census/BLS/FRED/admin
   data] that covers [population] during [time period]. Provide
   exact URLs for data access pages.
4. Find any replication packages or code repositories on Dataverse,
   OpenICPSR, or GitHub associated with key papers on this topic.
5. Check for any recent (2024-2026) news articles, policy briefs,
   or think tank reports that discuss this issue.
```

**Prompt 2 — Methodology deep dive**:
```
I am using [SPECIFIC ECONOMETRIC METHOD] for my paper. Please find:

1. The original methodological paper(s) that introduced this approach.
2. The most cited practical guide or handbook chapter explaining
   implementation.
3. Any recent methodological critiques or improvements (2022-2026),
   especially papers in Econometrica, ReStud, or Journal of
   Econometrics.
4. Software implementations: R packages, Stata commands, or Python
   libraries for this method, with links to documentation.
5. Any "best practices" papers or journal editor guidance on
   reporting standards for this method.
```

---

### For Claude Deep Research
[Claude excels at analytical synthesis, identifying connections
between ideas, and structured reasoning. Optimize for depth.]

**Prompt 1 — Theoretical synthesis**:
```
I am writing a literature review section for an economics paper on
[TOPIC]. My paper's contribution is [SPECIFIC CONTRIBUTION].

Please help me:

1. Identify the 2-3 "strands of literature" my paper should position
   itself within, following economics convention.
2. For each strand, name the 3-5 most important papers and summarize
   the state of evidence in 2-3 sentences.
3. Identify where these strands intersect or conflict — this is where
   my contribution fits.
4. Draft a contribution paragraph using the economics convention:
   "This paper contributes to X strands of literature. First, ..."
5. Identify the single most closely related paper and articulate
   precisely how my paper differs.
```

**Prompt 2 — Critical appraisal**:
```
I have read the following paper: [TITLE, AUTHORS, YEAR].
The paper claims [MAIN FINDING] using [METHOD].

Please help me critically appraise this paper:
1. What assumptions are required for the identification strategy
   to be valid?
2. What are the 3 most serious threats to internal validity?
3. Where would the results likely fail to replicate or generalize?
4. What robustness checks should the authors have run but didn't?
5. What is the most productive way to build on this paper?
```

---

### Search queries for academic databases
[Ready-to-paste search strings for each platform]

**Google Scholar**:
- `"[keyword1]" "[keyword2]" [method] economics`
- `allintitle: [key phrase] [method]`

**Semantic Scholar**:
- Topic: `[keyword]` | Fields: Economics | Year: 2020-2026

**NBER Working Papers**:
- https://www.nber.org/papers?page=1&perPage=50&q=[keyword]

**SSRN**:
- https://papers.ssrn.com/sol3/results.cfm?txtKey_Words=[keyword]

**EconLit (via Northeastern library)**:
- Subject: `[JEL code]` AND Keywords: `[term]`

**Scopus**:
- `TITLE-ABS-KEY([keyword1] AND [keyword2]) AND SUBJAREA(ECON)`

**RePEc/IDEAS**:
- https://ideas.repec.org/cgi-bin/htsearch?q=[keyword]
```

4. Save to `./projects/[project-name]/deep-research-prompts.md`
5. Inform user: "Prompts saved. Copy-paste into each platform. Results can be fed back into /read-paper or /paper-search."

## Example Invocation
```bash
/prompt-gen IO market concentration and consumer welfare
```
