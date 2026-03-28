# /submit-paper — Prepare journal submission package

## Trigger
"submit to [journal]", "prepare submission for EL", "get ready for JUE submission"

## Workflow

### Step 1: Identify journal requirements

**Known journals and their requirements:**

| Journal | Portal URL | Review type | File format | Article type | Max words | Fee |
|---|---|---|---|---|---|---|
| **Economics Letters** (EL) | editorialmanager.com/ecolet/ | Single-blind | LaTeX (.tex + .bib in .zip) OR Word | Short Communication | 2,000 (excl. refs) | $125 USD ($67.50 PhD students) |
| **Journal of Urban Economics** (JUE) | editorialmanager.com/jue/ | Double-blind | PDF + blinded PDF | Regular Submission | No strict limit | Free |
| **RAND Journal of Economics** | editorialmanager.com/rand/ | Double-blind | PDF | — | No strict limit | Free |
| **AEJ: Microeconomics** | editorialmanager.com/aejmi/ | Double-blind | PDF | — | No strict limit | Free |
| **Journal of Industrial Economics** (JIE) | editorialmanager.com/joie/ | Double-blind | Word or LaTeX | — | No strict limit | Free |
| **Marketing Science** | pubsonline.informs.org | Double-blind | PDF | — | No strict limit | Free |
| **JEBO** | editorialmanager.com/jebo/ | Double-blind | Word or LaTeX | — | No strict limit | Free |

### Step 2: Prepare submission files

**For single-blind journals (EL):**
1. Compile manuscript PDF with author name visible
2. Generate .tex file: set `keep-tex: true` in Quarto YAML, render
3. Bundle .tex + .bib into .zip
4. Upload: Manuscript PDF + LaTeX bundle (.zip) + Cover letter

**For double-blind journals (JUE, RAND, AEJ, JIE):**
1. Compile manuscript PDF with author name (item type: "Manuscript")
2. Compile **blinded** PDF without author name (item type: "Manuscript WITHOUT Author Identifiers"):
   - Temporarily remove `author:` line from `_quarto.yml`
   - Render to PDF
   - Restore original `_quarto.yml`
   - Also check manuscript body for self-identifying references
3. Upload: Manuscript PDF + Blinded PDF + Cover letter

### Step 3: Create blinded manuscript (double-blind journals)

```bash
# Backup
cp _quarto.yml _quarto.yml.bak

# Remove author line
sed '/  author:/d' _quarto.yml.bak > _quarto.yml

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

**EL item types:**
| Item type | What to upload |
|---|---|
| Manuscript (with Author names, affiliations & email) | `manuscript.pdf` |
| Editable Source Files (Word or LaTeX) | `submission-bundle.zip` (.tex + .bib) |
| Cover Letter | `cover-letter.pdf` |

**JUE item types:**
| Item type | What to upload |
|---|---|
| Manuscript | `manuscript.pdf` (with author name) |
| Manuscript WITHOUT Author Identifiers | `manuscript-blinded.pdf` (no author name) |
| Cover Letter | `cover-letter.pdf` |

---

## Step 6: COMPLETE FORM-FILLING GUIDE BY JOURNAL

### Economics Letters — Full Editorial Manager Walkthrough

**Page 1: Article type**
- Select: **Short Communication**

**Page 2: Request Editor**
- Pick an editor whose research matches your topic
- For algorithmic pricing / IO: **Joseph E. Harrington** (if available)
- For econometrics: **Brendan Kline**

**Page 3: Questions**

| Question | Answer |
|---|---|
| Word count (excl. refs) | Count body text only. Our EL paper was ~1,850 words |
| Funding confirmation | Select: "I confirm that I have mentioned all organizations..." (even if no funding — just means you confirmed) |
| Preprint (SSRN)? | **YES** — free visibility, early DOI, no effect on editorial decision |
| Data/code availability | Select public repository option, or "Other" and type: "Analysis code available at [GitHub URL]. Data publicly available from [source]." |
| Submission fee | Select: **Agreed** ($125 USD; $67.50 for PhD students) |
| Associate Editor | Pick someone in your field. For algorithmic pricing: look for IO economists. If Robert Clark is listed, choose him (co-author of Assad et al.) |

**Page 4: Classifications (select 1-3)**
- **L - Industrial Organization** (primary)
- **D - Microeconomics**
- **C - Mathematical and Quantitative Methods**

**Page 5: Manuscript metadata**
Provide manually if PDF uploaded (not auto-extracted):
- **Title**: From your manuscript
- **Abstract**: From your manuscript (keep under 150 words for EL)
- **Keywords**: 3-5 keywords separated by semicolons
- **Authors**: Name, department, university, email
- **Funding**: "No funding" if unfunded

**After submission:**
- Pay $125 fee (redirected to payment page)
- Confirmation email with manuscript number (e.g., ECOLET-D-26-XXXX)
- Expect desk decision in 2-3 weeks

---

### Journal of Urban Economics — Full Editorial Manager Walkthrough

**Page 1: Submission type**
- Select: **Regular Submission** (not "JUE Insight Short Paper")

**Page 2: Questions**

| Question | Answer |
|---|---|
| JUE Insight shorter paper? | **No** |
| Your Paper Your Way or Standard? | **Your Paper Your Way** (upload single PDF, no need to separate figures/tables) |
| Funding confirmation | Select: "I confirm..." |
| Preprint (SSRN)? | **YES** |
| JUE prior review policy? | **No** (unless resubmitting from another journal with referee reports) |
| Data/code posting policy read? | **Yes** (JUE requires data/code posted upon conditional accept) |
| Contains empirical work? | **Yes** |
| Can provide all code? | **Yes** |
| Data subject to access restrictions? | **No** (if using public data like Inside Airbnb) |
| Already conditionally accepted? | **No** |

**Page 3: Classifications (select 1-3)**
- **R - Urban, Rural, and Regional Economics** (primary — JUE's home field)
- **L - Industrial Organization**
- **D - Microeconomics**

**Page 4: Manuscript metadata**
- **Title**: From your manuscript
- **Abstract**: Reframe for urban/housing audience if different from IO version
- **Keywords**: Include urban-relevant terms (e.g., "short-term rentals", "urban housing markets")
- **Authors**: Name, department, university, email
- **Funding**: "No funding" if unfunded

**After submission:**
- No fee
- Confirmation email with manuscript number
- Expect desk decision in 3-4 weeks, full review 3-5 months
- JUE requires data/code posting upon conditional accept (GitHub repo satisfies this)

---

## Step 7: Dual-track protocol

**When submitting to two journals simultaneously:**
- Only ethical if papers are genuinely different (different length, scope, contribution)
- Short version (EL ~2,000 words) + full version (JUE ~10,000 words) = different papers
- **MUST disclose** in cover letter to the second journal:
  > "A shorter version focusing solely on [X] is under review at *[Journal]*. The present manuscript extends the analysis to include [Y, Z] and differs in scope and contribution."
- If the first journal accepts: cite it in the second paper and explain how the full version extends it

## Step 8: Post-submission checklist

- [ ] Confirmation email received with manuscript number
- [ ] Payment completed (if applicable — EL: $125)
- [ ] Save manuscript number to project memory
- [ ] Note expected timeline:
  - EL: desk decision 2-3 weeks, full review 6-8 weeks
  - JUE: desk decision 3-4 weeks, full review 3-5 months
- [ ] If dual-track: verify disclosure in second cover letter
- [ ] Calendar reminder for follow-up if no decision after expected timeline + 2 weeks
