# KNOWLEDGE BASE MANAGEMENT

## Repository Structure
Both modes share this directory structure (created automatically on first run):

```
econ-knowledge-base/
├── README.md                    # Dashboard: topic map + reading stats
├── references.bib               # BibTeX bibliography (append on each read)
├── literature/                  # One file per paper (Read Mode output)
│   ├── Card1994-MinimumWages.md
│   ├── Chetty2009-SalienceTaxation.md
│   └── _index.md               # Auto-generated table of all papers
├── notes/                       # Atomic permanent notes (your ideas)
│   ├── monopsony-explains-mw-puzzle.md
│   └── did-requires-parallel-trends.md
├── topics/                      # Hub/structure notes by theme
│   ├── labor-economics.md
│   └── public-finance.md
├── projects/                    # Writing projects
│   └── lit-review/
│       ├── contribution-section-draft.md
│       ├── survey-draft.md
│       └── literature-table.md
├── templates/                   # Reusable note templates
│   ├── literature-note-template.md
│   └── permanent-note-template.md
└── fleeting/                    # Quick captures to process later
```

## Conventions
- **Filenames**: `AuthorYear-ShortTitle.md` (e.g., `Card1994-MinimumWages.md`)
- **Links**: Standard markdown `[text](./relative/path.md)` — NOT wiki-links (GitHub doesn't render them)
- **Tags**: In YAML frontmatter, use lowercase with hyphens: `[labor, minimum-wage, diff-in-diff]`
- **Citations in drafts**: Use pandoc-citeproc format `[@Card1994]` for compilation to PDF/Word
- **BibTeX**: Append entries to `references.bib` on each paper read; use `techreport` type for working papers

## Useful Commands
```bash
# Search across all notes
rg "minimum-wage" ./literature/

# Find all papers using DiD
rg "methodology: diff" ./literature/

# Find high-relevance papers
rg "relevance: [45]" ./literature/

# Compile a draft to PDF with citations
pandoc ./projects/lit-review/survey-draft.md \
  --citeproc --bibliography=references.bib \
  --csl=chicago-author-date.csl -o survey-draft.pdf

# Update the paper index
ls ./literature/*.md | grep -v _index | while read f; do
  head -10 "$f" | grep -E "^(title|citekey|methodology):"
done > ./literature/_index.md
```
