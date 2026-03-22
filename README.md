# econ-research-os

A complete Claude Code skill system for economics research — from brainstorming to publication. Built for graduate students, academic economists, and economic consultants writing papers, practitioner guides, and textbooks.

**27 skills across 8 stages. Python/Jupyter first. Plain markdown knowledge base. Git-tracked everything.**

> ⚠️ **Work in progress.** This system is actively used and evolving. Contributions, issues, and feedback welcome.

## What this does

```
Think → Discover → Ingest → Store → Analyze → Synthesize → Write → Verify → Publish
```

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

## Quickstart (5 minutes)

### 1. Clone and install

```bash
git clone https://github.com/RyanPiao/econ-research-os.git
cd econ-research-os

# Copy skills to your Claude Code skills directory
cp -r skills/* ~/.claude/skills/

# Install system dependencies
brew install poppler ripgrep uv        # macOS
# sudo apt install poppler-utils ripgrep  # Linux

# Install Quarto (for book compilation)
brew install --cask quarto             # macOS

# Install Python dependencies
pip install -r requirements.txt
```

### 2. Install MCP servers

```bash
# Semantic Scholar — academic paper search (works without API key)
claude mcp add semantic-scholar -s user -- \
  uvx --from git+https://github.com/akapet00/semantic-scholar-mcp \
  semantic-scholar-mcp

# GitHub — push repos, manage PRs
claude mcp add github -s user -- \
  npx -y @modelcontextprotocol/server-github

# Filesystem — work across directories
claude mcp add filesystem -s user -- \
  npx -y @modelcontextprotocol/server-filesystem ~/projects ~/papers
```

### 3. Create your first project

```bash
cd ~/your-project-directory
claude

# In Claude Code:
/new-project "My Research Paper on Minimum Wages"
```

That's it. You're ready. See the [Tutorial](#tutorial) below for a complete walkthrough.

## Repo structure

```
econ-research-os/
├── README.md                         # This file
├── LICENSE                           # MIT
├── requirements.txt                  # Python dependencies
├── TUTORIAL.md                       # Step-by-step walkthrough with examples
│
├── skills/                           # All Claude Code skills (copy to ~/.claude/skills/)
│   ├── econ-think/
│   │   ├── SKILL.md                  # Slim router: /brainstorm /research-plan /prompt-gen /scope-and-outline
│   │   ├── brainstorm.md             # Detailed brainstorm methodology
│   │   ├── research-plan.md          # Research planning template
│   │   ├── prompt-gen.md             # Cross-platform prompt generation
│   │   └── templates/
│   │       └── research-plan-template.md
│   │
│   ├── econ-discover/
│   │   ├── SKILL.md                  # Slim router: /paper-search /citation-chase /deep-search /reading-list
│   │   ├── paper-search.md           # Multi-database search methodology
│   │   ├── citation-chase.md         # Forward/backward snowballing
│   │   ├── deep-search.md            # Comprehensive topic search
│   │   └── templates/
│   │       ├── search-results.md
│   │       └── reading-list.md
│   │
│   ├── econ-reader/
│   │   ├── SKILL.md                  # Slim router: /read-paper /split-pdf /handbook-reader /corpus-qa
│   │   ├── read-paper.md             # Three-pass reading methodology
│   │   ├── split-pdf.md              # Long document chunking
│   │   └── templates/
│   │       └── paper-snapshot.md
│   │
│   ├── econ-analyze/
│   │   ├── SKILL.md                  # Slim router: /analyze /data-wrangle /visualize /simulate /replicate /dashboard /causal-ml
│   │   ├── analyze.md                # Econometric analysis methodology
│   │   ├── visualize.md              # Publication figure conventions
│   │   ├── simulate.md               # Monte Carlo methodology
│   │   ├── causal-ml.md              # DML, causal forests, econml
│   │   └── templates/
│   │       ├── analysis-script.py
│   │       └── replication-log.md
│   │
│   ├── econ-synthesize/
│   │   ├── SKILL.md                  # Slim router: /lit-review /debate-map /gap-finder /research-outline
│   │   ├── lit-review.md             # Economics lit review conventions
│   │   ├── debate-map.md             # Structured disagreement mapping
│   │   └── templates/
│   │       ├── contribution-section.md
│   │       └── debate-map-template.md
│   │
│   ├── econ-write/
│   │   ├── SKILL.md                  # Slim router: /draft-section /book-chapter /compile-draft /self-review
│   │   ├── draft-section.md          # Prose writing rules (McCloskey/Cochrane/AER)
│   │   ├── book-chapter.md           # Chapter structure (Angrist-Pischke template)
│   │   ├── self-review.md            # 7-lens quality review
│   │   └── templates/
│   │       ├── chapter-template.qmd
│   │       ├── style-guide.md
│   │       └── notation-table.md
│   │
│   ├── econ-verify/
│   │   ├── SKILL.md                  # Slim router: /verify /cross-verify /verify-code
│   │   ├── verify.md                 # 6-lens adversarial verification
│   │   ├── cross-verify.md           # External AI prompt generation
│   │   └── templates/
│   │       ├── verification-report.md
│   │       └── cross-verify-prompts.md
│   │
│   └── econ-utils/
│       ├── SKILL.md                  # Slim router: /status /bib /new-chapter /new-project
│       ├── status.md                 # Project dashboard logic
│       ├── bib.md                    # Bibliography management
│       └── templates/
│           ├── CLAUDE.md             # Project-level Claude instructions template
│           ├── MEMORY.md             # Session memory template
│           ├── new-project/          # Scaffolding templates
│           │   ├── _quarto.yml
│           │   ├── index.qmd
│           │   └── .gitignore
│           └── codebook.md
│
├── examples/                          # Example outputs for the tutorial
│   ├── paper-snapshot-example.md
│   ├── lit-review-example.md
│   ├── verification-report-example.md
│   └── cross-verify-prompts-example.md
│
└── docs/                              # Additional documentation
    ├── architecture.md                # System design and stage descriptions
    ├── best-practices.md              # Daily workflow patterns and tips
    ├── mcp-setup.md                   # Detailed MCP server installation
    └── faq.md                         # Common issues and solutions
```

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

## Tutorial

See [TUTORIAL.md](TUTORIAL.md) for a complete walkthrough: brainstorming a research idea → searching for papers → reading and distilling → running analysis → drafting a chapter → verifying → compiling to PDF.

## Contributing

This is an open-source project. If you're an economist using Claude Code, your contributions make the system better for everyone:

- **Found a bug?** Open an issue.
- **Have a workflow improvement?** Open a PR.
- **Built a new skill?** Share it — the system is modular by design.
- **Using this for your research?** I'd love to hear about it.

## License

MIT — use it however you want.

## Acknowledgments

Built on the shoulders of:
- [Pedro Sant'Anna](https://psantanna.com/claude-code-my-workflow/) — the most comprehensive Claude Code academic workflow
- [Scott Cunningham](https://github.com/scunning1975/MixtapeTools) — split-pdf skill, Referee 2 pattern, and the original inspiration
- [Anthropic's skills documentation](https://code.claude.com/docs/en/skills) — skill architecture best practices
