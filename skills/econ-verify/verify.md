Methodology: Sant'Anna adversarial critic pattern (read-only critic, no incentive to downplay), Cunningham Referee 2 (fresh context, hostile but constructive), Berk-Harvey-Hirshleifer referee framework (JEP 2017), AEA replication standards.

### /verify — Full adversarial verification of a manuscript or chapter

**Trigger**: "verify my chapter", "check this for errors", "referee this draft", "cross-check my analysis"

**Inputs**: A chapter file (.qmd/.md/.tex), or a directory containing chapter + code + output

**Workflow**:

Run 6 verification lenses sequentially. For each lens, adopt the posture of a hostile but fair Referee 2: assume nothing is correct until verified. Do NOT give the author the benefit of the doubt.

---

#### Lens 1: Mathematical verification

**Question**: Are all equations, derivations, and formal claims correct?

**Procedure**:
1. Extract every displayed equation from the manuscript
2. For each equation:
   - Verify notation matches NOTATION.md (if exists)
   - Check that each symbol is defined before use
   - Verify dimensional consistency (both sides of = have same units)
   - Work through the derivation step by step — does step N follow from step N-1?
   - Test boundary/special cases: set key parameters to 0, 1, or ∞ — does the equation produce sensible results?
   - Check signs: should the coefficient be positive or negative given the economic story?
3. For any equation taken from a cited paper:
   - Verify it matches the original (common error: transcription mistakes, dropped terms, changed notation without noting it)

**Output format**:
```markdown
### Lens 1: Mathematical verification
**Equations checked**: [N]
**Errors found**: [N]

| Eq # | Location | Issue | Severity | Fix |
|------|----------|-------|----------|-----|
| (3.2) | §3.4, p.12 | Sign error: β₂ should be negative given mechanism described in §3.2 | CRITICAL | Flip sign or revise mechanism discussion |
| (5.1) | §5.2, p.28 | Missing subscript i on Y — inconsistent with (5.3) | Minor | Add subscript |
```

---

#### Lens 2: Empirical claims vs. actual output

**Question**: Does the text accurately describe what the code actually produces?

**Procedure**:
1. Identify every quantitative claim in the manuscript (point estimates, standard errors, p-values, sample sizes, R², summary statistics)
2. For each claim, trace it to the source:
   - If code exists in `./code/`: run or read the code, verify the number matches
   - If a table exists in `./output/tables/`: cross-check the number against the table
   - If a figure exists: verify described patterns match the visual
3. Common errors to flag:
   - Coefficient reported in text differs from table (rounding, wrong column, stale output)
   - Sample size in text doesn't match N in regression table
   - "Significant at 5%" but actual p-value is 0.06
   - Figure described as "increasing" but the plot shows a flat or decreasing trend
   - Summary statistics don't match between text and Table 1

**Procedure for code-text consistency**:
```bash
# If Python code exists, attempt to verify key numbers
# Read the main analysis script
cat ./projects/[project]/code/[analysis].py

# Check if output files exist and are recent
ls -la ./projects/[project]/output/tables/
ls -la ./projects/[project]/output/figures/

# Read output tables and cross-reference with manuscript claims
```

**Output format**:
```markdown
### Lens 2: Empirical claims vs. actual output
**Claims checked**: [N]
**Mismatches found**: [N]

| Claim | Location | In text | In output | Match? | Issue |
|-------|----------|---------|-----------|--------|-------|
| Baseline β | §5.3, ¶2 | "0.15" | Table 5.1 col 3: 0.147 | CLOSE | Rounding acceptable but inconsistent — use 0.147 or round consistently |
| Sample N | §5.2, ¶1 | "12,450" | Code output: 12,387 | NO | Stale text — code drops 63 obs in cleaning step added after text was written |
```

---

#### Lens 3: Causal logic and identification

**Question**: Does the identification argument actually support the causal claims?

**Procedure**:
1. State the paper/chapter's causal claim in one sentence
2. State the identification strategy in one sentence
3. List the key assumptions required (parallel trends, exclusion restriction, CIA, etc.)
4. For EACH assumption:
   - Is it stated explicitly in the text?
   - Is it testable? If yes, is it tested? What do the tests show?
   - If not testable, is the argument for plausibility compelling?
   - What is the most plausible violation? Is it addressed?
5. Check the logic chain: assumption → design → estimate → interpretation
   - Does the estimate actually answer the stated question?
   - Could the result be driven by something other than the claimed mechanism?
   - Is the comparison group appropriate?
6. Check for common identification failures:
   - DiD: Are pre-trends actually flat? Is the treatment truly exogenous to the outcome trend?
   - IV: Is the instrument plausibly excludable? Is it strong (F > 10)?
   - RDD: Is there manipulation at the cutoff? Is the bandwidth choice sensitive?
   - Matching: Is overlap sufficient? Are the matched groups actually comparable on observables?

**Output format**:
```markdown
### Lens 3: Causal logic and identification
**Causal claim**: [one sentence]
**Identification**: [one sentence]

| Assumption | Stated? | Tested? | Test result | Plausible violation | Addressed? |
|------------|---------|---------|-------------|---------------------|------------|
| Parallel trends | Yes, §5.2 | Yes, event study | Pre-trends flat ✓ | Differential shocks to treated states | Partially — placebo test in App B but no discussion of [specific concern] |

**Logic chain assessment**:
- [ ] Claim matches what the design can identify
- [ ] No overclaiming (e.g., claiming general equilibrium effects from a partial equilibrium design)
- [ ] External validity limitations stated
- **Overall**: [SOUND / CONCERNS / CRITICAL FLAW]
```

---

#### Lens 4: Citation verification

**Question**: Are all citations real, accurate, and fairly represented?

**Procedure**:
1. Extract all `[@citekey]` references from the manuscript
2. Cross-check every citekey against `references.bib`:
   - Does the entry exist?
   - Is the entry complete (authors, year, title, journal/series)?
3. For key claims attributed to specific papers:
   - Does the cited paper actually say what the text claims it says?
   - Check using paper snapshots in `./literature/` if available
   - Flag any "citation laundering" (citing a secondary source for a claim that should be attributed to the primary)
4. AI hallucination check — these patterns suggest a fabricated citation:
   - Author names that don't appear together on any real paper
   - Journal names that don't exist
   - Years that don't match the claimed finding
   - Suspiciously round sample sizes or effects attributed to specific papers

**Procedure for automated checking**:
```bash
# Check all citekeys resolve in .bib
grep -oP '@\w+' manuscript.qmd | sort -u > /tmp/cited_keys.txt
grep -oP '@\w+\{(\w+),' references.bib | sed 's/.*{//' | sed 's/,//' | sort -u > /tmp/bib_keys.txt
comm -23 /tmp/cited_keys.txt /tmp/bib_keys.txt  # Keys cited but not in .bib
```

**Output format**:
```markdown
### Lens 4: Citation verification
**Citations checked**: [N]
**Issues found**: [N]

| Citekey | Issue | Severity |
|---------|-------|----------|
| @Smith2019 | Not in references.bib | CRITICAL — will fail compilation |
| @Jones2021 | Text says "find positive effect" but paper snapshot shows null result | CRITICAL — misrepresentation |
| @Brown2020 | Secondary citation — original finding is from @Lee2018 | Minor — cite primary source |
```

---

#### Lens 5: Code-output-text pipeline integrity

**Question**: If someone ran all the code from scratch, would they get the same output that the text describes?

**Procedure**:
1. Check that code files exist for every table and figure referenced in the manuscript
2. Verify the code → output → manuscript pipeline:
   - Does each script specify its input data file? Does that file exist?
   - Does each script save output to the expected path?
   - Are random seeds set for all stochastic operations?
   - Are package versions specified (requirements.txt)?
3. Check for "stale output" risk:
   - When was the code last modified? When was the output last generated?
   - If code is newer than output, the output may be stale
4. Check for path dependencies:
   - Does the code use absolute paths? (It shouldn't — all paths should be relative)
   - Are API keys handled via environment variables, not hardcoded?

**Output format**:
```markdown
### Lens 5: Code-output-text pipeline integrity
**Scripts checked**: [N]
**Pipeline breaks found**: [N]

| Script | Output | Issue |
|--------|--------|-------|
| code/ch05_did.py | output/tables/main_results.tex | Code modified Mar 20, output dated Mar 15 — STALE |
| code/fig_event_study.py | output/figures/fig_05_01.pdf | Missing random seed — results may not reproduce |
| code/sim_iv_bias.py | N/A | Script references data/raw/panel.csv which does not exist |
```

---

#### Lens 6: Overclaiming and conclusion validity

**Question**: Do the conclusions follow from what was actually shown?

**Procedure**:
1. Extract every conclusion or policy implication from the manuscript
2. For each, ask:
   - Is this supported by the specific evidence presented?
   - Does the claim go beyond what the identification strategy can support?
   - Internal vs external validity: does a result in one setting justify a general claim?
   - Statistical vs economic significance: is a statistically significant but tiny effect presented as important?
   - Does the conclusion acknowledge limitations, or does it read as unconditional?
3. Common overclaiming patterns to flag:
   - "X causes Y" when the design only shows correlation or association
   - "Policy A should be adopted" when the paper only estimates partial equilibrium effects
   - "The effect is large" without benchmarking against relevant magnitudes
   - Generalizing from a specific population/time/place to universal claims
   - Ignoring competing explanations that weren't ruled out
   - Framing a null result as "no effect" rather than "not enough power to detect"

**Output format**:
```markdown
### Lens 6: Overclaiming and conclusion validity
**Claims checked**: [N]
**Overclaiming instances**: [N]

| Claim | Location | Issue | Suggested revision |
|-------|----------|-------|--------------------|
| "Minimum wage increases do not harm employment" | §5.7, ¶3 | Null result in one state/period ≠ universal claim; design cannot rule out small negative effects | "We find no statistically significant employment effects in [setting], though we cannot rule out effects smaller than [MDE]" |
```

---

### Verification Summary Report

After all 6 lenses, generate a consolidated report:

```markdown
# Verification Report: [Manuscript/Chapter]
**Date**: [YYYY-MM-DD]
**Verified by**: Claude Code /verify (adversarial mode)

## Score Summary
| Lens | Issues found | Critical | Important | Minor |
|------|-------------|----------|-----------|-------|
| 1. Math | | | | |
| 2. Empirical claims | | | | |
| 3. Causal logic | | | | |
| 4. Citations | | | | |
| 5. Code pipeline | | | | |
| 6. Overclaiming | | | | |
| **TOTAL** | | | | |

## Verdict
- [ ] **PASS**: 0 critical, ≤3 important → ready to publish/submit
- [ ] **REVISE**: 1+ critical or 4+ important → fix before sharing
- [ ] **MAJOR REVISION**: 3+ critical → fundamental issues to address

## Critical issues (must fix before any sharing)
1. [Issue + exact location + fix]

## Important issues (fix before submission)
1. [Issue + location + fix]

## Minor issues (fix when convenient)
1. [Issue + location + fix]

## What passed cleanly
[Note what IS correct — the author needs positive feedback too]
```

Save to: `./projects/[project]/reviews/verify-[filename]-[date].md`

---

## When to Use Each Verification Mode

```
After /draft-section or /book-chapter:
  → /self-review (econ-write.md)     # Check prose quality, voice, structure
  → /verify                          # Check substance, math, logic, claims
  → /cross-verify                    # Generate prompts for Gemini/ChatGPT

After /analyze or /simulate:
  → /verify-code                     # Check code correctness
  → /verify (on the chapter)         # Check that text matches output

Before /compile-draft:
  → /verify --full                   # Run all 6 lenses on full manuscript
  → /cross-verify                    # Final external check

Before git push to public repo:
  → /verify-code (all scripts)       # Ensure reproducibility
  → /verify Lens 4 (citations)       # No fabricated references
```

## Example Invocations
```bash
# Full adversarial verification of a chapter
/verify ./projects/mybook/chapters/05-did.qmd

# Code-specific verification
/verify-code ./projects/mybook/code/ch05_did_estimation.py

# Generate cross-verification prompts
/cross-verify ./projects/mybook/chapters/05-did.qmd

# Verify just the math
/verify --lens math ./projects/mybook/chapters/05-did.qmd

# Verify just citations
/verify --lens citations ./projects/mybook/chapters/05-did.qmd

# Verify after revisions (only re-check what changed)
/verify --diff ./projects/mybook/chapters/05-did.qmd
```
