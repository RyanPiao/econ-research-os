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
