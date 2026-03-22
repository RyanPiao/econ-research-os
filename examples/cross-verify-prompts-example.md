# Cross-Verification Prompts: Chapter 5 — Difference-in-Differences

**Generated**: 2026-03-22
**Source file**: `./projects/mybook/chapters/05-did.qmd`
**Purpose**: Independent verification of substance, claims, and logic by external AI models
**Companion**: Use alongside `./projects/mybook/reviews/verify-05-did-2026-03-22.md`

---

## For Google Gemini Deep Research
*Gemini strength: broad literature verification, finding contradicting evidence, surfacing recent methodological advances*

### Prompt 1 — Literature and Claim Verification

```
I am writing an economics textbook chapter on difference-in-differences
that makes the following key claims. Please verify each claim against
the published economics literature and flag any that are:
(a) contradicted by well-known results
(b) stated more strongly than the evidence supports
(c) missing important caveats or qualifications

Claims to verify:

1. "Card and Krueger (1994) found that New Jersey's minimum wage
   increase from $4.25 to $5.05 led to a relative increase in
   fast-food employment of approximately 2.76 FTEs per store
   compared to Pennsylvania."

2. "The parallel trends assumption cannot be directly tested — it
   is fundamentally untestable because it concerns counterfactual
   outcomes."

3. "Goodman-Bacon (2021) shows that the two-way fixed effects
   estimator is a weighted average of all possible 2x2 DiD
   comparisons, including problematic comparisons that use
   already-treated units as controls."

4. "Under staggered treatment timing, the TWFE estimator can
   produce estimates with the wrong sign even when all individual
   treatment effects are positive — a result known as the
   'negative weighting' problem."

5. "Callaway and Sant'Anna (2021) propose a solution that
   restricts comparisons to not-yet-treated and never-treated
   units, avoiding the contamination identified by
   Goodman-Bacon."

6. "The Cengiz et al. (2019) bunching estimator finds that across
   138 state-level minimum wage changes, the number of jobs lost
   below the new minimum is almost exactly offset by jobs gained
   just above it, implying a near-zero aggregate employment
   effect."

7. "When treatment effects are heterogeneous across groups and
   time, the standard event-study regression with leads and lags
   can produce biased pre-trend coefficients even when parallel
   trends holds."

For each claim, provide:
- Whether the published literature supports, contradicts, or is
  mixed on this claim
- The 2-3 most relevant papers (with full citations)
- Any important caveats or qualifications the author should add
```

### Prompt 2 — Methodology and Recent Advances Check

```
I am writing a textbook chapter that teaches difference-in-differences
using a case study of city-level minimum wage increases. My empirical
application uses the following design:

Identification strategy: Difference-in-differences comparing
restaurant-level FTE employment in cities that raised the minimum
wage to $15/hour versus neighboring cities that did not, over
the period 2014-2023.

My identification assumptions are:
1. Parallel trends: Absent the minimum wage increase, employment
   trends in treated and control cities would have been identical.
2. No anticipation: Restaurants did not adjust employment before the
   minimum wage increase took effect.
3. No spillovers (SUTVA): The minimum wage increase in treated cities
   did not affect employment in control cities.
4. Treatment effect homogeneity: The effect of the minimum wage does
   not vary systematically across cities in a way that biases the
   TWFE estimator.

My data covers 12,387 restaurant-quarter observations across 18 cities
in 6 states from 2014Q1 to 2023Q4.

Please evaluate:
1. Are these the correct assumptions for this method? Am I missing any?
2. Are there well-known papers that show these assumptions fail in
   settings similar to mine (city-level minimum wages)?
3. What diagnostic tests should I run that I might have overlooked?
   Specifically: should I worry about the Roth (2022) pre-test bias
   critique? Should I use the honest DiD confidence intervals?
4. Are there methodological advances from 2022-2026 that improve on
   my approach for staggered city-level treatment timing?
5. Given that $15 minimum wages represent increases of 40-60% in some
   cities, should I be concerned about violations of SUTVA from
   commuting-zone spillovers?
6. What would a skeptical referee's strongest objection be?
```

---

## For ChatGPT Deep Research
*ChatGPT strength: finding specific facts, verifying numbers, checking institutional and policy details*

### Prompt 1 — Factual and Numerical Verification

```
Please fact-check the following specific claims from my economics
textbook chapter on difference-in-differences. For each, verify
whether the stated number/fact is accurate by searching for the
original source:

1. "The federal minimum wage was last raised to $7.25 per hour
    on July 24, 2009."

2. "Card and Krueger (1994) surveyed 410 fast-food restaurants
    in New Jersey and eastern Pennsylvania, including Burger King,
    KFC, Wendy's, and Roy Rogers locations."

3. "New Jersey raised its minimum wage from $4.25 to $5.05 per
    hour on April 1, 1992."

4. "Neumark and Wascher (2000) used payroll data from a
    restaurant industry association to challenge the Card and
    Krueger findings."

5. "As of January 2026, 30 states and the District of Columbia
    have minimum wages above the federal level of $7.25."

6. "Seattle's minimum wage ordinance, passed in June 2014, phased
    in a $15 minimum wage over several years, with large employers
    reaching $15 by January 2017."

7. "The Goodman-Bacon (2021) decomposition theorem was published
    in the Journal of Econometrics."

8. "Cengiz et al. (2019) analyze 138 state-level minimum wage
    changes between 1979 and 2016."

9. "The 'credibility revolution' in economics, as described by
    Angrist and Pischke (2010), emphasizes research designs that
    exploit quasi-random variation."

10. "The Quarterly Census of Employment and Wages (QCEW) provides
    universe-of-employers data derived from unemployment insurance
    tax records."

For each:
- Is the stated fact/number correct?
- If not, what is the correct number and what is the source?
- If approximately correct, note the exact figure and source
```

### Prompt 2 — Citation Verification

```
I need to verify that the following citations in my economics textbook
chapter are real papers that actually exist. For each, please confirm:
(a) The paper exists with these authors, title, year, and journal
(b) The paper's actual finding matches my characterization

Citations to verify:

1. Card, David, and Alan B. Krueger. 1994. "Minimum Wages and
   Employment: A Case Study of the Fast-Food Industry in New
   Jersey and Pennsylvania." American Economic Review 84(4):
   772-793.
   — I characterize this as: "finding no negative employment
   effect of the NJ minimum wage increase"

2. Neumark, David, and William Wascher. 2000. "Minimum Wages
   and Employment: A Case Study of the Fast-Food Industry in
   New Jersey and Pennsylvania: Comment." American Economic
   Review 90(5): 1362-1396.
   — I characterize this as: "finding negative employment effects
   using payroll data, contradicting Card and Krueger"

3. Goodman-Bacon, Andrew. 2021. "Difference-in-Differences with
   Variation in Treatment Timing." Journal of Econometrics
   225(2): 254-277.
   — I characterize this as: "proving that TWFE is a weighted
   average of all 2x2 DiD estimates with potentially negative
   weights"

4. Callaway, Brantly, and Pedro H.C. Sant'Anna. 2021.
   "Difference-in-Differences with Multiple Time Periods."
   Journal of Econometrics 225(2): 200-230.
   — I characterize this as: "proposing a group-time ATT
   estimator that avoids negative weighting"

5. Cengiz, Doruk, Arindrajit Dube, Attila Lindner, and Ben
   Zipperer. 2019. "The Effect of Minimum Wages on Low-Wage
   Jobs." Quarterly Journal of Economics 134(3): 1405-1454.
   — I characterize this as: "finding near-zero net employment
   effects using a bunching estimator across 138 minimum wage
   events"

6. Dube, Arindrajit, T. William Lester, and Michael Reich. 2010.
   "Minimum Wage Effects Across State Borders." Review of
   Economics and Statistics 92(4): 945-964.
   — I characterize this as: "using contiguous border county
   pairs and finding no significant negative employment effects"

7. Roth, Jonathan. 2022. "Pretest with Caution: Event-Study
   Estimates After Testing for Parallel Trends." American
   Economic Review: Insights 4(3): 305-322.
   — I characterize this as: "showing that pre-testing for
   parallel trends biases event-study estimates toward finding
   an effect"

8. Sun, Liyang, and Sarah Abraham. 2021. "Estimating Dynamic
   Treatment Effects in Event Studies with Heterogeneous
   Treatment Effects." Journal of Econometrics 225(2): 175-199.
   — I characterize this as: "showing that standard event-study
   regressions produce biased coefficients under treatment effect
   heterogeneity"

Flag any citations that:
- Do not appear to exist (possible hallucination)
- Exist but the finding doesn't match my characterization
- Have incorrect journal, volume, or page numbers
- Have a more recent published version I should cite instead
```

---

## For Claude Deep Research
*Claude strength: logical consistency, argument structure, identifying subtle reasoning flaws*

### Prompt 1 — Logic and Argument Audit

```
I am writing a textbook chapter arguing that difference-in-differences
is a powerful tool for causal inference when properly implemented, but
that naive TWFE estimation under staggered treatment timing can produce
misleading results.

Here is my argument structure:

1. The fundamental problem of causal inference is that we cannot
   observe counterfactual outcomes for treated units.

2. DiD solves this by using a control group's change over time as
   a proxy for the treated group's counterfactual change, under
   the assumption of parallel trends.

3. In the canonical 2x2 case (two groups, two periods), the DiD
   estimator is unbiased for the ATT under parallel trends and
   no anticipation.

4. I demonstrate this with the Card and Krueger (1994) minimum
   wage example, showing that the NJ vs. PA comparison yields a
   positive employment effect of about 2.8 FTEs per restaurant.

5. I then extend to the staggered case: multiple groups treated
   at different times. I claim that researchers routinely
   estimate this using two-way fixed effects (TWFE) — unit and
   time fixed effects — which appears to be the natural
   generalization of the 2x2 DiD.

6. However, Goodman-Bacon (2021) shows that TWFE is actually a
   weighted average of ALL 2x2 comparisons, including comparisons
   that use already-treated units as controls. These comparisons
   can receive negative weight when treatment effects change over
   time.

7. Therefore, TWFE can produce a negative estimate even when all
   individual treatment effects are positive.

8. I present solutions: Callaway and Sant'Anna (2021) and Sun and
   Abraham (2021) restrict comparisons to clean control groups.

9. I conclude that researchers should use these newer estimators
   for staggered designs, and that TWFE should only be used as a
   benchmark or when there is strong reason to believe treatment
   effects are homogeneous.

Please perform a rigorous logical audit:
1. Does each step follow from the previous one?
2. Are there hidden assumptions I haven't stated?
3. Where is the argument weakest — which step would a skeptic
   attack first?
4. Am I presenting the Card and Krueger result too uncritically
   in Step 4, given the Neumark-Wascher controversy? Does this
   undermine the pedagogical argument?
5. In Step 6, am I correctly characterizing the negative weighting
   result? Is it specifically "already-treated as controls" that
   causes the problem, or is my characterization too simplified?
6. Does my conclusion (Step 9) follow from what I've actually
   shown, or am I overclaiming? Specifically: should I also
   discuss settings where TWFE is still perfectly fine?
7. If you were Referee 2 at a top economics journal reviewing this
   as a survey/pedagogy piece, what would be your single most
   devastating criticism?

Be adversarial. Don't give me the benefit of the doubt.
```

### Prompt 2 — Equation and Derivation Check

```
Please verify the following mathematical derivation from my economics
textbook chapter on difference-in-differences. Check each step for
correctness.

Starting point — The potential outcomes framework for 2x2 DiD:

Step 1: Define potential outcomes
  Y_it(0) = outcome for unit i at time t if never treated
  Y_it(1) = outcome for unit i at time t if treated
  Observed: Y_it = D_it * Y_it(1) + (1 - D_it) * Y_it(0)
  where D_it = 1 if unit i is treated at time t.

Step 2: Define the ATT (average treatment effect on the treated)
  ATT = E[Y_it(1) - Y_it(0) | D_it = 1]

Step 3: The identification problem
  E[Y_it(0) | D_it = 1] is unobserved — we cannot observe
  the untreated potential outcome for treated units.

Step 4: The parallel trends assumption (PTA)
  E[Y_i1(0) - Y_i0(0) | D_i = 1] = E[Y_i1(0) - Y_i0(0) | D_i = 0]
  "The expected change in the untreated potential outcome is the
  same for treated and control groups."

Step 5: DiD estimator derivation
  ATT = E[Y_i1(1) - Y_i0(0) | D_i = 1]
      = E[Y_i1(1) | D_i = 1] - E[Y_i0(0) | D_i = 1]

  By PTA, substitute for the unobserved counterfactual:
  E[Y_i0(0) | D_i = 1] = E[Y_i1(0) | D_i = 1] - E[Y_i1(0) - Y_i0(0) | D_i = 0]
                        ... wait, this isn't right. Let me re-derive.

  Actually:
  E[Y_i1(0) | D_i = 1] = E[Y_i0(0) | D_i = 1]
                         + E[Y_i1(0) - Y_i0(0) | D_i = 1]
                         = E[Y_i0(0) | D_i = 1]
                         + E[Y_i1(0) - Y_i0(0) | D_i = 0]  [by PTA]

  So: ATT = E[Y_i1 | D_i = 1] - E[Y_i0 | D_i = 1]
           - [E[Y_i1 | D_i = 0] - E[Y_i0 | D_i = 0]]

Step 6: Sample analog — the DiD estimator
  delta_hat = (Y_bar_T_post - Y_bar_T_pre) - (Y_bar_C_post - Y_bar_C_pre)

Step 7: TWFE regression representation
  Y_it = alpha + gamma * D_i + lambda * Post_t + delta * (D_i * Post_t) + epsilon_it
  Claim: OLS estimate of delta equals delta_hat from Step 6.

For each step:
1. Is the algebra correct?
2. Is the economic assumption valid and correctly stated?
3. Does the notation remain consistent across all steps?
4. In Step 5, I got confused and re-derived mid-step — is my
   final expression correct?
5. In Step 7, does the OLS equivalence claim hold exactly, or
   only under specific conditions (balanced panel, etc.)?
6. What happens if parallel trends is violated — specifically,
   if E[Y_i1(0) - Y_i0(0) | D_i = 1] = E[Y_i1(0) - Y_i0(0) | D_i = 0] + bias?
   What does delta_hat converge to?
```

---

## Manual Search Queries for Spot-Checking

### Google Scholar (verify citations exist and check characterizations)

```
"Minimum Wages and Employment" author:Card author:Krueger
```
```
"Difference-in-Differences with Variation in Treatment Timing" author:Goodman-Bacon
```
```
"Difference-in-Differences with Multiple Time Periods" author:Callaway author:Sant'Anna
```
```
"The Effect of Minimum Wages on Low-Wage Jobs" author:Cengiz author:Dube
```
```
"Pretest with Caution" author:Roth
```

### Semantic Scholar (verify citation counts and impact)

Search each title above and check:
- Year and journal match the chapter's citation
- Citation count is consistent with the paper's claimed importance
- Any errata or corrections published

### FRED (verify macroeconomic claims)

- Federal minimum wage history: https://fred.stlouisfed.org/series/FEDMINNFRWG
- State minimum wages: https://www.dol.gov/agencies/whd/minimum-wage/state
- Verify: "The federal minimum wage was last raised to $7.25 on July 24, 2009"

### Department of Labor (verify policy details)

- Federal minimum wage history: https://www.dol.gov/agencies/whd/minimum-wage/history
- Verify NJ minimum wage history: https://www.nj.gov/labor/wageandhour/tools-resources/minimumwage/history.shtml
- Verify: "New Jersey raised its minimum wage from $4.25 to $5.05 on April 1, 1992"

### BLS / QCEW (verify data source claims)

- QCEW overview: https://www.bls.gov/cew/
- Verify: "The QCEW provides universe-of-employers data derived from unemployment insurance tax records"

### Seattle Minimum Wage Study (verify policy timeline)

- Seattle ordinance details: https://www.seattle.gov/laborstandards/ordinances/minimum-wage
- Verify: "Seattle's minimum wage ordinance phased large employers to $15 by January 2017"

---

## Usage Instructions

1. **Copy-paste each prompt** into the corresponding AI platform (Gemini, ChatGPT, Claude).
2. **Compare responses** across all three models. Pay particular attention to:
   - Claims where models disagree (suggests uncertainty — investigate further)
   - Specific numbers or dates where a model provides a correction
   - Missing caveats or qualifications that multiple models flag
3. **Run the manual search queries** for any claim that an AI model flags as potentially incorrect.
4. **Cross-reference** against the verification report (`verify-05-did-2026-03-22.md`) to see if external models catch the same issues or find new ones.
5. **Document results** in the verification report's appendix.

---
*Generated by Econ Verify — Cross-Verification Mode*
*Purpose: Multi-model adversarial review for substance and accuracy*
