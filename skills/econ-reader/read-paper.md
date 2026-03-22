# Skill: Economic Paper Reader & Knowledge Distillation

## Description
A two-mode skill for academic economics workflow:
- **`read` mode**: Reads economics papers from local PDFs, web URLs, or directories. Performs structured three-pass knowledge distillation. Outputs paper snapshot markdown files to a Git-tracked knowledge base.
- **`review` mode**: Synthesizes multiple paper snapshot files into a draft literature review — either a standalone JEL-style survey or an embedded contribution section for an empirical paper. Uses economics-specific conventions (Cochrane/Bellemare/Angrist-Pischke methodology).

Both modes share the same knowledge base directory structure and output plain markdown files ready for GitHub.

---

# MODE 1: READ

## Workflow

### Step 0: Input Resolution
1. **If local PDF path**: Verify the file exists. Extract text content.
2. **If web URL**: Use `curl -L -o /tmp/paper_download.pdf "[URL]"` to download. Then extract text.
3. **If directory path**: List all `.pdf` files with `ls [path]/*.pdf`. Process each sequentially. Generate one output per paper plus a batch index.

For text extraction from PDFs, attempt:
```bash
# Try pdftotext first (poppler-utils)
pdftotext "[input_path]" /tmp/paper_text.txt

# If unavailable, try python
python3 -c "
import sys
try:
    import PyPDF2
    reader = PyPDF2.PdfReader(sys.argv[1])
    text = '\n'.join(page.extract_text() or '' for page in reader.pages)
    print(text)
except ImportError:
    print('ERROR: No PDF extraction tool available.')
" "[input_path]" > /tmp/paper_text.txt
```

If extraction fails, inform the user:
> "I couldn't extract text from this PDF. Please install `poppler-utils` (`brew install poppler` or `sudo apt install poppler-utils`) or provide a plain-text version."

### Step 1: First Pass — Triage Scan (The "Five Cs")
Read ONLY: title, abstract, introduction (first 2–3 pages), all section headings, conclusion, figure/table captions, bibliography.

Extract:

| Parameter | What to Identify |
|---|---|
| **Category** | Theoretical model, empirical analysis, methodological contribution, policy evaluation, survey, structural estimation |
| **Context** | Foundational works it builds on; debate it enters; applicable JEL codes |
| **Correctness** | Do abstract assumptions appear logically sound? |
| **Contributions** | Novel addition: new dataset, identification strategy, mechanism, policy result? |
| **Clarity** | Structurally coherent? Sections logically ordered? |

Also extract:
- **Research question** (one sentence)
- **Object of study** (population, sample, context)
- **Method** (model type, identification, estimation)
- **Main claim** (one sentence)
- **Scope limits** ("in X context," "for Y period")

**Decision gate**: Default to **working understanding** unless user specifies "skim" or "deep."

### Step 2: Second Pass — Core Economics Reading Strategy
Read full paper body in this order (NOT linearly):

#### 2A: Introduction Deep Read
Extract: (a) precise research question, (b) why it matters, (c) core mechanism, (d) headline result with direction + significance.

#### 2B: Identification Strategy / Empirical Design
**MOST CRITICAL SECTION.** Extract:
- What variation identifies the causal claim?
- Design type: DiD, RD, IV, RCT, synthetic control, matching, structural, other
- Key assumption(s) for identification (parallel trends, exclusion restriction, CIA, etc.)
- Threats authors acknowledge
- Threats authors DO NOT acknowledge (your assessment)

#### 2C: Theoretical Model / Mechanism (if present)
- Rewrite every symbol in plain English
- Label parameters: "behavioral," "technology," "institution," "statistical"
- Identify exogenous vs. endogenous
- State what each assumption "buys" the model
- Test one special case (parameter → 0 or ∞)

#### 2D: Data Section — Dataset Card
Extract: unit of observation, N, sample restrictions, time period, key variable definitions (DV, treatment/key IV, controls), data source(s), missingness handling, summary statistics assessment.

#### 2E: Results — Regression Table Decode
For EACH key table:
- Dependent variable, key independent variable(s)
- Baseline coefficient + SE + significance
- Identification: what comparison generates the estimate
- Controls and fixed effects (what level)
- Standard errors: clustering level, robust, bootstrap
- Sample: N, subsample restrictions
- Robustness: does key estimate survive alternatives?

**Plain-language interpretation**: "A one-unit increase in [X] is associated with a [β] unit change in [Y], holding [controls] constant."

Flag stargazing risks. Note if paper relies on asterisks without discussing economic magnitude.

#### 2F: Robustness and Sensitivity
- List all checks (alternative controls, samples, placebos, clustering, functional forms)
- Assess survival of main result
- Flag p-hacking signs

#### 2G: Conclusion + External Validity
- What this changes about understanding
- Where result likely fails to generalize
- Whether policy implications are warranted by design

### Step 3: Deep Mastery Pass (only if user requests "deep")
- Reconstruct identification argument from scratch
- Verify one key derivation or robustness check
- Identify hidden unstated assumptions
- Produce a concrete extension idea

### Step 4: Critical Appraisal

**Claims and Scope**: Exact claim, scope (population/context/time), counterclaim ruled out.

**Assumptions and Logic**: Required assumptions, implicit assumptions, what happens if weakened.

**Validity**: Internal (does design support inference?), Construct (defensible measurement?), External (where won't it generalize?).

**Reproducibility**: Data/code available? Sufficient detail for replication? Preregistration?

**Citations and Positioning**: Primary sources? Accurate representation? Missing key works?

### Step 5: Gap Extraction

| Gap Type | Question |
|---|---|
| **Boundary** | Where does the claim stop? |
| **Mechanism** | Effect exists — but WHY? What's under-argued? |
| **Measurement** | Key concept measured well, or better proxy available? |
| **Design** | Could identification be cleaner? Better natural experiment or data? |

Translate ONE gap into a testable next step.

### Step 6: Literature Review Positioning (NEW — feeds into Review Mode)
Extract how THIS paper positions itself relative to existing work:
- What "strands of literature" does it claim to contribute to? (usually 2–3)
- For each strand: which papers are cited, what gap is claimed, what's the marginal contribution?
- What organizing principle does the paper use: by identification strategy, by mechanism, by finding?
- What papers does it cite as the "closest" comparisons?

Record this in the output as the **Contribution Map** — this becomes raw material for Review Mode.

### Step 7: Generate Output File

Save to `./literature/` directory (create if needed):

Output template: see `templates/paper-snapshot.md`

### Step 8: Save and Confirm

```bash
# Create literature directory if needed
mkdir -p ./literature

# Filename: AuthorYear-ShortTitle.md
# Example: Card1994-MinimumWages.md
cat > "./literature/[filename].md" << 'PAPER_EOF'
[generated content]
PAPER_EOF

echo "Paper snapshot saved to ./literature/[filename].md"
```

If batch processing, also generate `./literature/_index.md` with a table of all papers.

---

## Accepted Input Formats (Read Mode)
- **Local PDF**: Absolute or relative path to a `.pdf` file (e.g., `./papers/angrist2010.pdf`)
- **Web URL**: Direct link to a paper (e.g., `https://www.nber.org/papers/w12345.pdf`)
- **Local directory**: Path to a folder containing multiple `.pdf` files for batch processing (e.g., `./papers/`)

## When to Use (Read Mode)
- "Read this paper" / "Analyze this paper" / "Distill this paper"
- "Run the paper reader on [path/URL]"
- "Knowledge distill [path/URL]"
- "Summarize this economics paper"
- Process any academic PDF with structured output

---

# EXAMPLE INVOCATIONS (Read Mode)

```bash
# Single local PDF
"Read and distill ./papers/angrist_krueger_1991.pdf"

# Web URL
"Analyze this paper: https://www.nber.org/papers/w28726.pdf"

# Directory batch
"Run the paper reader on all PDFs in ./reading_list/"

# With depth override
"Deep read ./papers/heckman_1979.pdf"

# Skim only
"Quick skim ./papers/survey_paper.pdf"
```

---

# IMPORTANT METHODOLOGY NOTES

## For Read Mode
- Always write coefficients in PLAIN LANGUAGE, not just numbers
- Always distinguish statistical significance from economic significance
- Always note what authors claim vs. what design actually supports
- Flag p-hacking risks explicitly
- Theoretical papers: interrogate assumptions, not math validity
- Empirical papers: interrogate identification, not prose quality
- Default depth is "working understanding"

---

## Authorities Encoded in This Skill
- **Keshav (2007)**: Three-pass reading framework
- **Angrist & Pischke (2010)**: Credibility revolution, design-based identification
- **Cochrane (2005)**: Writing tips — focus on 2–3 closest papers, cut citation padding
- **Bellemare (2020)**: Middle bits formula — 5–7 studies, one contribution paragraph
- **Head (UBC)**: Introduction formula — Hook → Question → Antecedents → Value-Added → Roadmap
- **JEL Editorial Guidelines**: Synthesis as crucial ingredient, point of view, selectivity
- **JES Author Guidelines**: 10,000–15,000 word surveys, creative synthesis not catalogue
- **ASA Statement on p-Values**: Effect sizes and uncertainty over binary significance
- **AEA Data Policy**: Reproducibility and transparency standards
