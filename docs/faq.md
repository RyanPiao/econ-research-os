# Frequently Asked Questions

## 1. How do I install just one skill?

Copy only that skill's folder to `~/.claude/skills/`. Each skill directory is self-contained -- it includes a slim `SKILL.md` router, detailed methodology reference files, and any templates it needs. For example, to install only the paper reading skill:

```bash
cp -r skills/econ-reader/ ~/.claude/skills/
```

No other skills or configuration are required. See the [Using Individual Stages](architecture.md#using-individual-stages) table for which skills to install for common use cases.

## 2. What if I don't use Python?

The analyze skill (`econ-analyze/`) is Python-first -- it generates scripts using pandas, statsmodels, linearmodels, and econml. However, the reading, writing, synthesis, and verification skills are language-agnostic. They produce and consume plain markdown, so they work regardless of your analysis toolchain. The templates and methodology references (McCloskey's writing rules, Keshav's three-pass reading, the verification lenses) are universal and apply to any language.

If you use R or Stata for analysis, you can still use every other stage of the pipeline. You would just skip `/analyze` and write your own analysis scripts, then pick up again at `/synthesize` or `/write`.

## 3. How do I handle long PDFs?

Use `/split-pdf`, which chunks documents into 10-15 page sections to avoid context window limits. This is essential for handbook chapters, dissertations, and survey papers that exceed 50-80 pages. Claude reads each chunk independently and produces a consolidated summary. Example:

```
> /split-pdf ./papers/handbook-econometrics-ch22-did.pdf
```

For shorter papers (under ~40 pages), `/read-paper` handles them directly without splitting.

## 4. Do I need all the MCP servers?

No. Only Semantic Scholar is strongly recommended -- it powers `/paper-search` and `/citation-chase`, which are central to the Discover stage. Without it, you would need to find papers manually and provide them as local PDFs.

GitHub MCP and Filesystem MCP are optional conveniences. GitHub MCP is useful if you want Claude to push commits or create PRs directly. Filesystem MCP is useful if your papers and projects live in different directories. Neither is required for the core research workflow.

See [docs/mcp-setup.md](mcp-setup.md) for installation details.

## 5. Can I use this with Cursor/Windsurf instead of Claude Code?

Skills are Claude Code specific -- they use the `SKILL.md` routing format that Claude Code's skill system recognizes. The slash commands (`/brainstorm`, `/read-paper`, etc.) will not work directly in Cursor, Windsurf, or other AI coding tools.

However, the methodology, templates, and prompts work in any AI coding tool. The valuable content is in the reference `.md` files (e.g., `read-paper.md`, `verify.md`, `draft-section.md`). You can copy these into your tool's system prompt or rules files and use them as methodology guides. The templates in each skill's `templates/` directory are plain markdown and work anywhere.

## 6. How do I add RAG to the system?

See [docs/mcp-setup.md](mcp-setup.md) for knowledge-RAG installation guidance. RAG becomes valuable when your `literature/` directory exceeds ~30 paper snapshots -- at that point, loading all snapshots into the context window becomes expensive and slow. A RAG server indexes your markdown knowledge base and lets Claude retrieve only the relevant snippets on demand.

This is an area of active development. The system works well without RAG for smaller projects, and the knowledge-RAG section of the MCP setup guide will be updated as recommended implementations mature.

## 7. What's the difference between /self-review and /verify?

They check different things:

- **`/self-review`** (econ-write skill) checks **prose quality**: argument structure, pedagogy, math presentation, prose clarity, citation formatting, cross-references, and authorial voice. It runs 7 lenses and scores each out of 10. Think of it as a writing coach.

- **`/verify`** (econ-verify skill) checks **substance**: mathematical correctness (step-by-step equation checking), causal logic (identification assumptions), citation validity (do citekeys resolve and are papers described accurately), code-output consistency (is the text stale relative to code output), and overclaiming (do conclusions follow from evidence). Think of it as an adversarial referee.

The recommended workflow is to run `/self-review` first to polish the prose, fix any issues, then run `/verify` to check that the content is correct. This order avoids wasting verification effort on prose that will be rewritten.

## 8. How many papers can the system handle?

Without RAG, ~30 paper snapshots work comfortably in the context window. Each snapshot is typically 1-3 pages of markdown, so 30 snapshots plus a chapter draft fit within Claude's context with room to spare. Skills like `/lit-review` and `/corpus-qa` work well at this scale.

With a knowledge-RAG MCP server, the system scales to hundreds of papers. RAG indexes the full knowledge base and retrieves only relevant snippets per query, so the context window is no longer the bottleneck. If you are working on a book with a large literature base or managing multiple research projects, RAG is the recommended path. See [docs/mcp-setup.md](mcp-setup.md) for setup guidance.
