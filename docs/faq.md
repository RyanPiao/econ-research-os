# Frequently Asked Questions

## How do I install just one skill?

Each skill directory is fully independent. You do not need to install the entire system. To install a single skill, copy its directory to your Claude Code skills folder:

```bash
# Example: install only the paper reader
cp -r skills/econ-reader ~/.claude/skills/

# Example: install only the analysis tools
cp -r skills/econ-analyze ~/.claude/skills/
```

The skill will work on its own. Some combinations are natural pairs (e.g., `econ-reader` + `econ-synthesize` for lit reviews), but nothing will break if you only install one.

## What if I don't use Python?

The analysis skill (`econ-analyze`) is Python-first: it generates scripts using pandas, statsmodels, linearmodels, and matplotlib. However, most other skills are language-agnostic:

- **Reading and ingestion** (`econ-reader`) outputs plain markdown — no code involved.
- **Writing and synthesis** (`econ-write`, `econ-synthesize`) produce prose and structured outlines.
- **Verification** (`econ-verify`) reviews text and checks logic — it works on any content.
- **Discovery** (`econ-discover`) searches databases and returns structured results.

If you use R, you can still use the analysis skill as a starting point. Ask Claude to adapt the generated code blocks to R equivalents (e.g., `fixest` instead of `linearmodels`, `ggplot2` instead of `matplotlib`). The econometric logic and specification choices transfer directly.

## How do I handle long PDFs?

Use `/split-pdf`, which chunks long documents into 10-15 page sections to avoid hitting context window limits. This is especially useful for handbook chapters and book-length readings:

```
> /split-pdf ./papers/handbook-econometrics-ch22-did.pdf
```

Claude will read each chunk sequentially and produce a consolidated summary. For very long documents (100+ pages), this is significantly more reliable than trying to process the entire file at once.

## Do I need all the MCP servers?

No. Only the **Semantic Scholar** MCP server is strongly recommended, as it powers the paper search capabilities (`/paper-search`, `/citation-chase`, `/deep-search`).

The other two are optional conveniences:

- **GitHub** — useful if you manage your research repo through Claude Code, but you can always use `git` commands directly.
- **Filesystem** — useful if your papers, data, and projects live in separate directory trees. Not needed if everything is under one project folder.

See [mcp-setup.md](mcp-setup.md) for installation details.

## Can I use this for a journal paper instead of a book?

Yes. The system works for papers, dissertations, practitioner guides, and any other long-form economics writing. A few skill substitutions to keep in mind:

- Use `/draft-section` instead of `/book-chapter` for paper sections.
- Use `/research-plan` to plan the paper's structure, identification strategy, and data requirements.
- Use `/scope-and-outline` to define the paper's contribution and section flow before drafting.
- The `/lit-review`, `/verify`, and `/cross-verify` skills work identically for papers and books.

The pipeline is the same — Think, Discover, Ingest, Analyze, Synthesize, Write, Verify — the output format just changes.
