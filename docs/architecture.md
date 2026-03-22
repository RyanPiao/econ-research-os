# System Architecture

This document describes the design of econ-research-os: the pipeline stages, what each stage does, and the key decisions behind the system.

## Pipeline overview

The system follows a linear pipeline from idea to publication:

```
Think → Discover → Ingest → Store → Analyze → Synthesize → Write → Verify → Publish
```

Each stage feeds the next, but every stage can also be used independently. You can enter the pipeline at any point and exit when you have what you need.

## Stage table

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

## Using individual stages

**You don't have to use the whole system.** Each stage works independently. Install only what you need:

| I want to... | Install these skills | That's it? |
|---|---|---|
| Just read and take notes on papers | `econ-reader/` | Yes — generates paper snapshots to any directory |
| Just search for papers | `econ-discover/` | Yes — searches Semantic Scholar, NBER, SSRN |
| Just run regressions and make figures | `econ-analyze/` | Yes — Python econometrics with publication output |
| Just write a lit review | `econ-reader/` + `econ-synthesize/` | Reader feeds synthesizer |
| Just draft and review a paper | `econ-write/` + `econ-verify/` | Write → self-review → verify |
| Just generate prompts for Gemini/ChatGPT | `econ-think/` | Yes — `/prompt-gen` works standalone |
| **The full pipeline for a book project** | **All skills + `econ-utils/`** | Full system with project management |

To install a single skill, copy just its directory into your Claude Code skills folder:

```bash
# Example: install only the reader skill
cp -r skills/econ-reader ~/.claude/skills/
```

## Key design decisions

**Plain markdown everywhere.** No proprietary formats. Your knowledge base is a Git repo of `.md` files. It works on GitHub, in VS Code, in any text editor, forever.

**Slim skills with reference files.** Each SKILL.md is <50 lines — just metadata and routing. Detailed methodology lives in separate `.md` files that Claude loads on demand. This keeps context costs low.

**Separate code from prose.** Analysis code lives in `code/`, output in `output/`, chapters in `chapters/`. The book references code output via includes, not embedded execution. Keeps compilation fast and debugging simple.

**Author-date citations.** Everything uses `[@citekey]` pandoc syntax with a shared `references.bib`. Compatible with Quarto, pandoc, and LaTeX.

**Multi-model verification.** The system doesn't trust any single AI — `/cross-verify` generates targeted prompts for Gemini (breadth), ChatGPT (fact-checking), and Claude (logic) to triangulate correctness.

## Methodology sources

This system encodes conventions and best practices from:

- **Reading**: Keshav (Three-Pass), Angrist & Pischke (Credibility Revolution)
- **Literature reviews**: Cochrane (Writing Tips), Bellemare (Middle Bits), JEL editorial guidelines
- **Writing**: McCloskey (Economical Writing), Thomson (Guide for the Young Economist), AER Style Guide
- **Book structure**: Angrist & Pischke (MHE/Mastering Metrics), Cunningham (Mixtape), Huntington-Klein (The Effect)
- **Verification**: Sant'Anna (adversarial critic-fixer), Cunningham (Referee 2), Berk-Harvey-Hirshleifer (JEP 2017)
- **Workflow**: Sant'Anna (Claude Code academic workflow), Cunningham (MixtapeTools)
- **Causal ML**: Chernozhukov et al. (Double ML), Athey & Imbens (Causal Forests)
