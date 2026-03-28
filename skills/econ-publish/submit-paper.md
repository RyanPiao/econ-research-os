# /submit-paper — Prepare journal submission package

## Trigger
"submit to [journal]", "prepare submission for EL", "get ready for JUE submission"

## Workflow

### Step 1: Identify journal requirements

**Known journals and their requirements:**

| Journal | Portal | Review type | File format | Max words | Fee |
|---|---|---|---|---|---|
| **Economics Letters** (EL) | editorialmanager.com/ecolet/ | Single-blind | LaTeX (.tex + .bib in .zip) OR Word | 2,000 (excl. refs) | $125 USD |
| **Journal of Urban Economics** (JUE) | editorialmanager.com/jue/ | Double-blind | PDF + blinded PDF | No strict limit | Free |
| **RAND Journal of Economics** | editorialmanager.com/rand/ | Double-blind | PDF | No strict limit | Free |
| **AEJ: Microeconomics** | editorialmanager.com/aejmi/ | Double-blind | PDF | No strict limit | Free |
| **Journal of Industrial Economics** (JIE) | editorialmanager.com/joie/ | Double-blind | Word or LaTeX | No strict limit | Free |
| **Marketing Science** | pubsonline.informs.org | Double-blind | PDF | No strict limit | Free |
| **JEBO** | editorialmanager.com/jebo/ | Double-blind | Word or LaTeX | No strict limit | Free |

### Step 2: Prepare submission files

**For single-blind journals (EL):**
1. Compile manuscript PDF with author name visible
2. Generate .tex file: set `keep-tex: true` in Quarto YAML, render
3. Bundle .tex + .bib into .zip
4. Upload: Manuscript PDF + LaTeX bundle + Cover letter

**For double-blind journals (JUE, RAND, AEJ, JIE):**
1. Compile manuscript PDF with author name (item type: "Manuscript")
2. Compile **blinded** PDF without author name (item type: "Manuscript WITHOUT Author Identifiers"):
   - Temporarily remove `author:` line from `_quarto.yml`
   - Render to PDF
   - Restore original `_quarto.yml`
   - Also check manuscript body for self-identifying references ("the author's previous work", university name, acknowledgements naming colleagues)
3. Upload: Manuscript PDF + Blinded PDF + Cover letter

### Step 3: Create blinded manuscript (double-blind journals)

```bash
# Backup
cp _quarto.yml _quarto.yml.bak

# Remove author line
sed 's/  author: "Richeng Piao"//' _quarto.yml.bak > _quarto.yml

# Compile
quarto render --to pdf

# Copy blinded PDF
cp _book/[output].pdf submission/[journal]/manuscript-blinded.pdf

# Restore
cp _quarto.yml.bak _quarto.yml && rm _quarto.yml.bak

# Recompile with author
quarto render --to pdf
```

**Also check for identifying info in the manuscript body:**
- Self-citations: "@piao2023" or "Piao (2023)" → replace with "Author (2023)" or "[removed for review]"
- University name in acknowledgements
- "our previous work" or "in earlier work, we showed..."
- GitHub URLs containing the author's name
- Dataset descriptions mentioning the author's lab or institution

### Step 4: Generate LaTeX bundle (if required)

```bash
# Set keep-tex: true in _quarto.yml (under pdf format)
quarto render manuscript.qmd --to pdf

# Bundle
zip submission-bundle.zip manuscript.tex references.bib [any .sty files]
```

### Step 5: Editorial Manager upload guide

**Common item types across Elsevier journals:**
| Item type | What to upload |
|---|---|
| Manuscript | Your main PDF (with author info) |
| Manuscript WITHOUT Author Identifiers | Blinded PDF (double-blind only) |
| Cover Letter | Cover letter PDF |
| Editable Source Files (Word or LaTeX) | .zip with .tex + .bib (LaTeX journals) |
| Figure | Separate figure files if required |
| Table | Usually embedded in manuscript |
| Supplementary Files | Data, code, appendices |

**Common form fields:**
| Field | What to enter |
|---|---|
| Article Type | "Short Communication" (EL) or "Research Paper" (most others) |
| Word count | Body text only, excluding references/tables |
| JEL codes | From your manuscript frontmatter |
| Keywords | From your manuscript frontmatter |
| Suggested editor | Pick someone whose research matches your topic |
| Suggested reviewers | Optional but helpful — pick 2-3 active researchers in your area |
| Funding | "No funding" if unfunded |
| Data availability | "Code available at [GitHub URL]. Data from [source]." |
| Preprint | Usually YES — free visibility on SSRN |
| Submission fee | EL: $125. Most others: free |

### Step 6: Post-submission checklist

- [ ] Confirmation email received with manuscript number
- [ ] Payment completed (if applicable)
- [ ] Save manuscript number to project memory
- [ ] Note expected timeline (desk decision: 2-4 weeks; full review: 2-4 months)
- [ ] If dual-track: disclose in cover letter to second journal
