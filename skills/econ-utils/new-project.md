# /new-project — Scaffold a New Research Project

## Trigger
`/new-project "[Project Name]"` or `/new-project "[Project Name]" --type [paper|book|report]`

## Procedure

### Step 1: Create directory structure
```bash
PROJECT_DIR="./projects/$(echo "$NAME" | tr ' ' '-' | tr '[:upper:]' '[:lower:]')"
mkdir -p "$PROJECT_DIR"/{chapters,code,data/{raw,processed},output/{tables,figures},drafts,reviews,synthesis,apps}
```

### Step 2: Copy templates
```bash
# From skills/econ-utils/templates/new-project/
cp templates/new-project/_quarto.yml "$PROJECT_DIR/"
cp templates/new-project/.gitignore "$PROJECT_DIR/"
cp templates/new-project/index.qmd "$PROJECT_DIR/"
```

### Step 3: Generate project-specific files

**CLAUDE.md** — from template, fill in project name and defaults:
```bash
sed "s/\[PROJECT NAME\]/$NAME/g" templates/CLAUDE.md > "$PROJECT_DIR/CLAUDE.md"
```

**MEMORY.md** — fresh session log:
```bash
cp templates/MEMORY.md "$PROJECT_DIR/MEMORY.md"
```

**STYLE_GUIDE.md** — ask user or use defaults:
```markdown
# Style Guide: [Project Name]

## Voice
- Register: Semi-formal (conversational but precise)
- Pronouns: "we" for papers, "you" for textbook chapters
- Humor: Occasional (Angrist-Pischke level, not Cunningham level)
- Stance: Opinionated but fair

## Conventions
- Equation walks: Always for displayed equations
- Code language: Python (primary), with R alternatives noted
- Citation density: Medium (~8-12 per major section)
- Cross-ref style: Quarto @ref

## Forbidden phrases
- "It is important to note that..."
- "In this section, we will..."
- "delve into"
- "it's worth noting"
- "utilize" (use "use")
```

**NOTATION.md** — starter table:
```markdown
# Notation Table: [Project Name]

## Core variables
| Symbol | Meaning | First introduced |
|--------|---------|-----------------|
| Y_i | Outcome for individual i | Ch. 1 |
| D_i | Treatment indicator (0/1) | Ch. 1 |
| X_i | Vector of covariates | Ch. 1 |

## Conventions
- Subscript i: individual unit
- Subscript t: time period
- Hat (^): estimated quantity
- Bold: vectors and matrices
```

**references.bib** — empty starter:
```bash
echo "% Bibliography for $NAME" > "$PROJECT_DIR/references.bib"
echo "% Add entries with /bib add [DOI]" >> "$PROJECT_DIR/references.bib"
```

**outline.md** — starter:
```markdown
# Outline: [Project Name]
**Type**: [paper / book / report]
**Created**: [YYYY-MM-DD]

## Chapters / Sections
1. Introduction
2. [Topic]
3. [Topic]
...
```

### Step 4: Initialize git
```bash
cd "$PROJECT_DIR"
git init
git add .
git commit -m "Scaffold project: $NAME"
```

### Step 5: Confirm
```
Project scaffolded at [path]:
- CLAUDE.md (project constitution)
- MEMORY.md (session log)
- STYLE_GUIDE.md (writing conventions)
- NOTATION.md (symbol table)
- _quarto.yml (book configuration)
- references.bib (bibliography)
- outline.md (project outline)
- chapters/, code/, data/, output/ directories

Start writing with: /book-chapter 1 "Introduction"
Or start reading with: /paper-search [topic]
```
