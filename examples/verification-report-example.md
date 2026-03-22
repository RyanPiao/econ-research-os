# Verification Report: Chapter 5 — Difference-in-Differences

**File**: `./projects/mybook/chapters/05-did.qmd`
**Date**: 2026-03-22
**Verified by**: Claude Code /verify (adversarial mode)
**Verdict**: REVISE

---

## Score Summary

| Lens | Issues found | Critical | Important | Minor |
|------|-------------|----------|-----------|-------|
| 1. Math | 3 | 1 | 0 | 2 |
| 2. Empirical claims | 2 | 1 | 1 | 0 |
| 3. Causal logic | 2 | 0 | 1 | 1 |
| 4. Citations | 1 | 0 | 0 | 1 |
| 5. Code pipeline | 1 | 0 | 1 | 0 |
| 6. Overclaiming | 2 | 0 | 0 | 1 |
| **TOTAL** | **11** | **2** | **3** | **5** |

## Verdict
- [ ] **PASS**: 0 critical, ≤3 important — ready to publish/submit
- [x] **REVISE**: 1+ critical or 4+ important — fix before sharing
- [ ] **MAJOR REVISION**: 3+ critical — fundamental issues to address

---

## Critical Issues (must fix before any sharing)

### 1. Sign error in the DiD estimator derivation (Lens 1: Math)
**Location**: Section 5.3, Equation (5.4), page 18

The chapter presents the 2x2 DiD estimator as:

$$\hat{\delta}^{DiD} = (\bar{Y}_{T,post} - \bar{Y}_{T,pre}) + (\bar{Y}_{C,post} - \bar{Y}_{C,pre})$$

The second term should be **subtracted**, not added. The correct expression is:

$$\hat{\delta}^{DiD} = (\bar{Y}_{T,post} - \bar{Y}_{T,pre}) - (\bar{Y}_{C,post} - \bar{Y}_{C,pre})$$

As written, the estimator adds the control group change instead of subtracting it. This is a transcription error — the prose description and the code implementation are both correct (they subtract), but the displayed equation is wrong. Any reader attempting to learn DiD from this equation will get the wrong answer.

**Fix**: Change the `+` to `-` in Eq. (5.4). Verify that subsequent derivations starting from (5.4) propagate the correct sign.

### 2. Stale sample size — text says N = 12,450 but code produces N = 12,387 (Lens 2: Empirical)
**Location**: Section 5.5, paragraph 1, page 24

The text states: "Our sample consists of 12,450 restaurant-quarter observations." However, the analysis script `code/ch05_did_estimation.py` (line 47) drops 63 additional observations due to a cleaning step added on March 15 that removes establishments with missing zip codes. The output table `output/tables/did_main.md` reports N = 12,387.

The text was written before this cleaning step was added and was never updated.

**Fix**: Update the text to read "12,387 restaurant-quarter observations." Also update the sentence in Section 5.4 that describes the sample construction to mention the zip code restriction.

---

## Important Issues (fix before submission)

### 1. Parallel trends assumption asserted but not tested (Lens 3: Causal Logic)
**Location**: Section 5.3, pages 16–17

The chapter states: "The key identifying assumption is that treated and control counties would have followed parallel employment trends in the absence of the minimum wage increase." This is correct. However, the chapter never presents a formal test or visual check of this assumption.

The code directory contains `code/ch05_event_study.py`, which generates an event-study plot with pre-treatment coefficients — but this figure is never referenced in the chapter text and does not appear in the output directory.

**Fix**: (a) Run `ch05_event_study.py` to generate the event-study figure. (b) Add a subsection or figure in Section 5.3 showing the event-study plot and discussing whether pre-trends are flat. (c) Discuss what a violation of parallel trends would imply for the estimate. This is essential — any reviewer will request it.

### 2. Missing citation for the Callaway and Sant'Anna (2021) estimator (Lens 4/3: Citations + Logic)
**Location**: Section 5.6, page 28

The chapter uses a staggered DiD estimator and references it as "the group-time ATT estimator of Callaway and Sant'Anna (2021)." However:
- `@CallawySantAnna2021` does not appear in `references.bib` (the entry is spelled `@CallawaySantAnna2021` — missing an 'a' in Callaway).
- This will cause a compilation error (`pandoc-citeproc: reference CallawySantAnna2021 not found`).

**Fix**: Correct the citekey in the text from `@CallawySantAnna2021` to `@CallawaySantAnna2021` to match the .bib entry.

### 3. Code modified after output was generated — possible stale results (Lens 5: Pipeline)
**Location**: `code/ch05_did_estimation.py` → `output/tables/did_main.md`

The analysis script `ch05_did_estimation.py` was last modified on March 20 (the zip code cleaning step). The output file `did_main.md` was last generated on March 15. This means the published results may not reflect the current code.

**Fix**: Re-run `python code/ch05_did_estimation.py` to regenerate the output tables. Verify that the main estimate and sample size in the regenerated table match what the text reports (and update the text if they differ — see Critical Issue #2).

---

## Minor Issues (fix when convenient)

### 1. Notation inconsistency: subscript conventions (Lens 1: Math)
**Location**: Sections 5.2–5.3

Equation (5.1) uses $Y_{it}$ (individual $i$, time $t$), but Equation (5.7) switches to $Y_{gt}$ (group $g$, time $t$) without flagging the change in notation. While both are correct in context (the first is for the individual-level model, the second for the group-time estimator), a reader working through the chapter sequentially will be confused.

**Fix**: Add a sentence when introducing Eq. (5.7): "We now switch notation from individual $i$ to group $g$, where a group is defined as all units first treated at the same time."

### 2. Rounding inconsistency in reported coefficients (Lens 2: Empirical)
**Location**: Section 5.5, paragraphs 2–3, page 25

The baseline estimate is reported as "0.034" in the text but as "0.0342" in Table 5.2. The robustness estimate is reported as "0.031" (3 decimal places) but the standard error is reported as "0.0119" (4 decimal places). Pick a consistent number of decimal places — either 3 throughout or 4 throughout.

**Fix**: Standardize to 3 decimal places for coefficients and standard errors: 0.034 (0.012) in both text and tables.

### 3. Figure 5.1 axis label says "Employment" but should say "FTE Employment" (Lens 2: Empirical)
**Location**: Figure 5.1, page 20

The y-axis reads "Employment" but the variable being plotted is full-time equivalent employment, not headcount. Since the chapter discusses the distinction between FTE and headcount in Section 5.4, the axis label should be precise.

**Fix**: Update the y-axis label in `code/ch05_fig_trends.py` from `"Employment"` to `"FTE Employment"` and regenerate the figure.

### 4. Overclaiming: "robust to all specifications" is too strong (Lens 6: Overclaiming)
**Location**: Section 5.5, final paragraph, page 27

The text states: "The positive employment effect is robust to all alternative specifications." Table 5.3 shows 6 robustness checks, of which 5 produce positive and significant estimates but one (column 6, with county-specific linear trends) produces a positive but insignificant estimate (p = 0.14). Saying "robust to all" is inaccurate.

**Fix**: Change to: "The positive employment effect survives five of six alternative specifications. When we include county-specific linear trends (column 6), the point estimate remains positive but loses statistical significance."

### 5. Footnote 14 — hedging language could be tighter (Lens 6: Overclaiming)
**Location**: Footnote 14, page 26

Footnote reads: "These results suggest that minimum wage increases may never cause disemployment." This is an overreach from a single study in one setting. The design cannot support a universal claim.

**Fix**: Change to: "These results suggest that moderate minimum wage increases need not cause disemployment in the fast-food sector, consistent with the findings of Card and Krueger (1994) and Dube et al. (2010)."

---

## What Passed Cleanly

**Lens 1 (Math)**: Apart from the sign error in (5.4) and the notation switch, all other equations are correct. The derivation of the TWFE estimand decomposition in Section 5.6 (Equations 5.8–5.12) is mathematically sound and clearly presented. The Goodman-Bacon decomposition walkthrough is a particularly strong section — each weight is correctly derived and the numerical example is accurate.

**Lens 2 (Empirical)**: All coefficient magnitudes (other than the rounding issue) match between text and tables. Summary statistics in Table 5.1 are internally consistent. The description of the data construction pipeline in Section 5.4 is detailed and reproducible.

**Lens 3 (Causal Logic)**: The identification argument is clearly stated and the discussion of threats to validity in Section 5.3.2 is thorough. The chapter correctly distinguishes between what the parallel trends assumption requires and what it does not. The treatment of SUTVA violations (spillovers to neighboring counties) is well-handled.

**Lens 4 (Citations)**: 27 of 28 citations verified successfully against `references.bib` and paper snapshots. All characterizations of cited findings are accurate and fairly stated. No evidence of citation laundering or AI-hallucinated references.

**Lens 5 (Code Pipeline)**: All scripts have docstring headers, random seeds, and relative paths. Package versions are pinned in `requirements.txt`. The only issue is the stale output from the March 15 → March 20 code edit.

**Lens 6 (Overclaiming)**: The conclusion (Section 5.7) is appropriately hedged for the most part. External validity limitations are explicitly discussed. The chapter correctly notes that the results apply to the specific minimum wage magnitude and labor market studied.

---
*Generated by Econ Verify — Adversarial Mode*
*Methodology: Sant'Anna adversarial critic + Cunningham Referee 2 + AEA replication standards*
