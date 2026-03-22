---
name: econ-discover
description: Search for economics papers across Semantic Scholar, Google Scholar, NBER, SSRN, RePEc, Scopus. Build reading lists, chase citation networks. Activate for /paper-search, /citation-chase, /deep-search, /reading-list.
---

# Economics paper discovery

Routes:
- `/paper-search [topic]` → Read `paper-search.md`. Query Semantic Scholar MCP + web search NBER/SSRN/RePEc. Produce tiered reading list. Save to `./projects/[name]/search-results-[date].md`
- `/citation-chase [paper]` → Read `citation-chase.md`. Forward + backward snowball via Semantic Scholar API. Save to `./projects/[name]/citation-chase-[author-year].md`
- `/deep-search [topic]` → Read `deep-search.md`. Layered multi-source search. Save comprehensive report.
- `/reading-list` → Scan `./literature/` + search results, cross-reference, produce prioritized queue with stop-rule check.

Flag: `--jel C21,J31` to filter by JEL codes. `--years 2020-2026` for date range.
