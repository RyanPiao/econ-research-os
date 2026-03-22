### /book-chapter — Draft or revise a full book chapter

**Trigger**: "write chapter [N] on [topic]", "draft the [topic] chapter", "expand my outline for chapter [N]"

**Inputs**:
- Chapter entry from `./projects/[project]/outline.md`
- All paper snapshots tagged with this chapter's topic
- Any existing draft sections in `./projects/[project]/drafts/`
- `STYLE_GUIDE.md` and `NOTATION.md` if they exist

**Workflow**:

1. **Load chapter context**:
   - Read the outline for this chapter (argument, key papers, dependencies)
   - Read `NOTATION.md` for symbol consistency (create if doesn't exist)
   - Read `STYLE_GUIDE.md` for voice conventions (create if doesn't exist)
   - Check which previous chapters exist (for cross-reference consistency)

2. **Plan the chapter structure** following the practitioner guide template:

   ```markdown
   # Chapter [N]: [Title]

   ## [N].1 Opening hook
   [A real-world causal question, empirical puzzle, or policy dilemma
   that motivates the entire chapter. 1-2 paragraphs. Name specific
   people, places, numbers. Make the reader care.]

   ## [N].2 The intuition
   [Plain-language explanation of the core idea. Why does this method
   exist? What problem does it solve that simpler approaches can't?
   Use analogies, thought experiments, or causal diagrams. NO equations
   in this section.]

   ## [N].3 The canonical example
   [One empirical paper that perfectly illustrates the method.
   Tell the story: what was the question, what was the clever design,
   what did they find? Reference the specific tables/figures.]

   ## [N].4 The formal framework
   [Mathematical presentation. Start with the simplest version
   (two-period, two-group) before the general case. Every equation
   gets a full walk-through. Build from the intuition section —
   the reader should recognize the math as formalizing what they
   already understand.]

   ## [N].5 Implementation
   [How to actually do this. Code in R/Python/Stata showing the
   method applied to the canonical example's data (or simulated data).
   Walk through the code, explain the output, connect back to the
   formal framework.]

   ## [N].6 Extensions and practical issues
   [Variations, diagnostics, common mistakes, recent methodological
   advances. When does the method break? What assumptions are people
   most often wrong about?]

   ## [N].7 Further applications
   [2-3 additional empirical examples from different fields.
   Brief treatment — show the method's breadth.]

   ## [N].8 Summary and key takeaways
   [Numbered list of 5-7 key points. What must the reader remember?]

   ## [N].9 Exercises
   [3-5 conceptual questions + 2-3 applied problems with data]
   ```

3. **Draft each section sequentially**, following the /draft-section rules for prose quality.

4. **Cross-reference management**:
   - Use Quarto cross-ref syntax: `@fig-[label]`, `@eq-[label]`, `@sec-[label]`, `@tbl-[label]`
   - Reference previous chapters: "As we saw in @sec-regression, the OLS estimator..."
   - Check that all referenced figures, tables, equations actually exist
   - Maintain a running notation table and verify consistency with `NOTATION.md`

5. **Voice consistency checks** during drafting:
   - Re-read the opening hook of Chapter 1 before writing any new chapter — calibrate the voice
   - Check: Am I using the same level of formality? Same pronoun convention? Same humor register?
   - Check: Are my equation walks following the same pattern (intuition → equation → term-by-term → connection)?
   - If `STYLE_GUIDE.md` exists, verify compliance

6. **Save outputs**:
   ```bash
   # Chapter file
   ./projects/[project]/chapters/[NN]-[short-title].qmd

   # Update notation table
   ./projects/[project]/NOTATION.md

   # Update style guide if new conventions were established
   ./projects/[project]/STYLE_GUIDE.md
   ```

7. **Generate chapter metadata**:
   ```markdown
   ---
   chapter: [N]
   title: "[Chapter Title]"
   status: [first-draft | revised | review-ready | final]
   word_count: [N]
   equations: [N]
   figures: [N]
   tables: [N]
   code_blocks: [N]
   citations: [list of citekeys used]
   cross_refs_to: [list of other chapters referenced]
   cross_refs_from: [list of chapters that reference this one]
   last_edited: [YYYY-MM-DD]
   ---
   ```

### /compile-draft — Build the manuscript into PDF, HTML, or Word

**Trigger**: "compile my book", "build the manuscript", "render to PDF", "compile chapter [N]"

**Workflow**:

1. **Detect project type**:
   - If `_quarto.yml` exists → use Quarto
   - If only `.md` files → use pandoc directly
   - If `.tex` files → use LaTeX

2. **For Quarto book projects** (recommended):
   ```bash
   # Full book render
   quarto render

   # Single chapter preview
   quarto render chapters/04-did.qmd

   # Specific format
   quarto render --to pdf
   quarto render --to html
   quarto render --to docx
   ```

3. **For pandoc markdown projects**:
   ```bash
   # Concatenate chapters in order, then compile
   cat chapters/0*.md > /tmp/full-manuscript.md

   pandoc /tmp/full-manuscript.md \
     --citeproc \
     --bibliography=references.bib \
     --csl=chicago-author-date.csl \
     --number-sections \
     --toc \
     --toc-depth=3 \
     -V geometry:margin=1in \
     -V fontsize=12pt \
     -o manuscript.pdf
   ```

4. **For LaTeX projects**:
   ```bash
   # 3-pass compilation
   xelatex manuscript.tex
   bibtex manuscript
   xelatex manuscript.tex
   xelatex manuscript.tex
   ```

5. **Post-compilation checks**:
   - Verify no "??" unresolved references in output
   - Check bibliography rendered correctly (no missing entries)
   - Verify figure/table numbering is sequential
   - Check page count is reasonable for target
   - Report any compilation warnings or errors

6. **Scaffold a new Quarto book project** (if none exists):
   ```bash
   mkdir -p projects/[name]/{chapters,figures,data,code}

   # Generate _quarto.yml
   cat > projects/[name]/_quarto.yml << 'EOF'
   project:
     type: book
     output-dir: _book
   book:
     title: "[Book Title]"
     author: "Richeng"
     date: today
     chapters:
       - index.qmd
       - chapters/01-introduction.qmd
   bibliography: references.bib
   csl: chicago-author-date.csl
   format:
     html:
       theme: cosmo
       code-fold: true
       code-tools: true
     pdf:
       documentclass: scrreprt
       keep-tex: true
       number-sections: true
   execute:
     freeze: auto
   EOF

   echo "Quarto book project scaffolded at ./projects/[name]/"
   ```

## Supporting Files This Skill Creates/Manages

### STYLE_GUIDE.md (per project)
```markdown
# Style Guide: [Book Title]

## Voice
- Register: [Conversational / Semi-formal / Formal]
- Pronouns: [we / you / the reader / one]
- Humor: [Yes-frequent / Yes-occasional / Minimal]
- Stance: [Opinionated / Balanced / Neutral]

## Conventions
- Equation walks: [Always / For key equations / Never]
- Code language priority: [R > Python > Stata]
- Citation density: [Light ~5/section / Medium ~10 / Heavy ~20]
- Cross-ref style: [Quarto @ref / LaTeX \ref / Markdown links]

## Forbidden phrases
- "It is important to note that..."
- "In this section, we will..."
- "It is well known that..."
- "delve into"
- "it's worth noting"
- [Add project-specific bans]

## Required patterns
- Every chapter opens with: [empirical puzzle / causal question / story]
- Every equation must have: [preceding intuition + following walk-through]
- Every method chapter must include: [canonical example + code + exercises]
```

### NOTATION.md (per project)
```markdown
# Notation Table: [Book Title]

## Core variables
| Symbol | Meaning | First introduced |
|--------|---------|-----------------|
| Y_i | Outcome for individual i | Ch. 2 |
| D_i | Treatment indicator (0/1) | Ch. 2 |
| X_i | Vector of covariates | Ch. 2 |
| β | Treatment effect coefficient | Ch. 2 |
| ε_i | Error term | Ch. 2 |

## Method-specific notation
| Symbol | Meaning | Chapter |
|--------|---------|---------|
| τ_ATT | Average treatment effect on treated | Ch. 4 |
| δ_DiD | Difference-in-differences estimator | Ch. 5 |

## Conventions
- Subscript i: individual
- Subscript t: time period
- Subscript g: group
- Superscript (1), (0): potential outcomes
- Hat (^): estimated quantity
- Bar (ˉ): sample mean
- Bold: vectors and matrices
- Calligraphic: sets
```

## Git Workflow for Book Manuscripts
```bash
# Branch strategy
main        → stable, compiles cleanly
develop     → active writing
ch03-did    → feature branch per chapter
review-ch03 → review/revision branch

# Commit conventions
git commit -m "Ch 4: First draft of DiD identification section"
git commit -m "Ch 4: Add simulation code for parallel trends"
git commit -m "Ch 4: Address self-review issues (voice, eq walks)"
git commit -m "Notation: Add tau_ATT definition"
git commit -m "Bib: Add Callaway-SantAnna 2021"

# Tag milestones
git tag -a v0.1-outline -m "Complete outline"
git tag -a v0.5-first-draft -m "All chapters first draft"
git tag -a v1.0-review-ready -m "Full manuscript for review"

# .gitignore for book projects
_book/
*.pdf
*.docx
*.html
*_files/
.quarto/
```
