# System Architecture

econ-research-os is a modular Claude Code skill system for economics research. It organizes the full research lifecycle into eight sequential stages, each backed by purpose-built skills. This document describes the pipeline, what each stage does, the design philosophy, and how to install individual stages independently.

## Pipeline Overview

```
Think --> Discover --> Ingest --> Store --> Analyze --> Synthesize --> Write --> Verify --> Publish
```

The pipeline is designed to be traversed left-to-right for a complete project, but every stage can also be used independently. Data flows forward: Think produces research questions that guide Discover; Discover finds papers that feed Ingest; Ingest creates markdown snapshots that populate the Store; the Store feeds Analyze, Synthesize, and Write; and Verify audits everything before Publish compiles the final output.

## Stage Table

| Stage | What happens | Skills |
|-------|-------------|--------|
| **0. Think** | Brainstorm ideas, plan research, generate prompts for Gemini/ChatGPT/Claude | `/brainstorm` `/research-plan` `/prompt-gen` `/scope-and-outline` |
| **1. Discover** | Search for papers across Semantic Scholar, Google Scholar, NBER, SSRN, RePEc | `/paper-search` `/citation-chase` `/deep-search` `/reading-list` |
| **2. Ingest** | Read papers, chunk long documents, extract knowledge | `/read-paper` `/split-pdf` `/handbook-reader` `/corpus-qa` |
| **3. Store** | Git-tracked markdown knowledge base | `literature/` `notes/` `topics/` `references.bib` |
| **4. Analyze** | Regressions, data wrangling, visualizations, simulations, causal ML | `/analyze` `/data-wrangle` `/visualize` `/simulate` `/replicate` `/dashboard` `/causal-ml` |
| **5. Synthesize** | Lit reviews, debate maps, gap extraction | `/lit-review` `/debate-map` `/gap-finder` `/research-outline` |
| **6. Write** | Draft chapters, compile manuscripts, self-review prose | `/draft-section` `/book-chapter` `/compile-draft` `/self-review` |
| **7. Verify** | Adversarial review: math, logic, citations, code-output consistency, cross-model prompts | `/verify` `/cross-verify` `/verify-code` |
| **Util** | Project management, bibliography, scaffolding | `/status` `/bib` `/new-chapter` `/new-project` |

### Stage details

**Stage 0 -- Think.** Generates structured brainstorms, research plans, and cross-platform prompts. Output lands in `fleeting/` and project directories. This is the ideation layer: it produces questions, angles, and prompts but no final artifacts.

**Stage 1 -- Discover.** Searches academic databases via the Semantic Scholar MCP server. `/paper-search` runs keyword queries with year filters; `/citation-chase` traces forward and backward citations from a seed paper; `/deep-search` combines both into a comprehensive sweep; `/reading-list` aggregates and deduplicates results into a prioritized queue.

**Stage 2 -- Ingest.** Reads PDFs and produces structured markdown snapshots following the three-pass reading methodology (Keshav). `/split-pdf` handles long documents (80+ pages) by chunking into ~15-page sections. `/handbook-reader` is specialized for handbook chapters. `/corpus-qa` allows question-answering over a set of already-ingested papers.

**Stage 3 -- Store.** Not a skill per se but the organizational layer. Paper snapshots go to `literature/`, atomic notes to `notes/`, topic summaries to `topics/`, and all bibliographic entries to `references.bib`. Everything is plain markdown, Git-tracked.

**Stage 4 -- Analyze.** Python/Jupyter-first econometric analysis. Produces scripts in `code/`, outputs in `output/`. Covers OLS, IV, DiD, panel methods, causal ML (DML, causal forests via econml), Monte Carlo simulations, and publication-quality figures following AER style conventions.

**Stage 5 -- Synthesize.** Reads the knowledge base and produces literature reviews, debate maps, gap analyses, and research outlines. `/lit-review` follows economics conventions (Cochrane, Bellemare, JEL guidelines). `/debate-map` structures disagreements. `/gap-finder` identifies what the literature hasn't addressed.

**Stage 6 -- Write.** Drafts prose following McCloskey, Thomson, and AER style conventions. `/book-chapter` uses the Angrist-Pischke chapter template (hook, intuition, example, formal framework, implementation, extensions, exercises). `/self-review` runs 7 lenses on prose quality.

**Stage 7 -- Verify.** Adversarial checking across 6 substance lenses: math correctness, empirical consistency, causal logic, citation validity, code-output staleness, and overclaiming. `/cross-verify` generates targeted prompts for Gemini, ChatGPT, and Claude to triangulate correctness across models.

**Util.** Project scaffolding (`/new-project`, `/new-chapter`), status dashboards (`/status`), bibliography management (`/bib`), and session memory (`/context-save`).

## Using Individual Stages

You don't have to use the whole system. Each stage works independently. Install only what you need:

| I want to... | Install these skills | That's it? |
|---|---|---|
| Just read and take notes on papers | `econ-reader/` | Yes -- generates paper snapshots to any directory |
| Just search for papers | `econ-discover/` | Yes -- searches Semantic Scholar, NBER, SSRN |
| Just run regressions and make figures | `econ-analyze/` | Yes -- Python econometrics with publication output |
| Just write a lit review | `econ-reader/` + `econ-synthesize/` | Reader feeds synthesizer |
| Just draft and review a paper | `econ-write/` + `econ-verify/` | Write --> self-review --> verify |
| Just generate prompts for Gemini/ChatGPT | `econ-think/` | Yes -- `/prompt-gen` works standalone |
| **The full pipeline for a book project** | **All skills + `econ-utils/`** | Full system with project management |

To install a single skill, copy only that skill's folder into your Claude Code skills directory:

```bash
cp -r skills/econ-reader/ ~/.claude/skills/
```

Each skill directory is self-contained: a slim `SKILL.md` router, detailed methodology files, and templates.

## Key Design Decisions

**Plain markdown everywhere.** No proprietary formats. Your knowledge base is a Git repo of `.md` files. It works on GitHub, in VS Code, in any text editor, forever.

**Slim skills with reference files.** Each SKILL.md is <50 lines -- just metadata and routing. Detailed methodology lives in separate `.md` files that Claude loads on demand. This keeps context costs low.

**Separate code from prose.** Analysis code lives in `code/`, output in `output/`, chapters in `chapters/`. The book references code output via includes, not embedded execution. Keeps compilation fast and debugging simple.

**Author-date citations.** Everything uses `[@citekey]` pandoc syntax with a shared `references.bib`. Compatible with Quarto, pandoc, and LaTeX.

**Multi-model verification.** The system doesn't trust any single AI -- `/cross-verify` generates targeted prompts for Gemini (breadth), ChatGPT (fact-checking), and Claude (logic) to triangulate correctness.

## Methodology Sources

This system encodes conventions and best practices from:

- **Reading**: Keshav (Three-Pass), Angrist & Pischke (Credibility Revolution)
- **Literature reviews**: Cochrane (Writing Tips), Bellemare (Middle Bits), JEL editorial guidelines
- **Writing**: McCloskey (Economical Writing), Thomson (Guide for the Young Economist), AER Style Guide
- **Book structure**: Angrist & Pischke (MHE/Mastering Metrics), Cunningham (Mixtape), Huntington-Klein (The Effect)
- **Verification**: Sant'Anna (adversarial critic-fixer), Cunningham (Referee 2), Berk-Harvey-Hirshleifer (JEP 2017)
- **Workflow**: Sant'Anna (Claude Code academic workflow), Cunningham (MixtapeTools)
- **Causal ML**: Chernozhukov et al. (Double ML), Athey & Imbens (Causal Forests)
