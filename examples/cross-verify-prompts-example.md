# Cross-Verification Prompts: Chapter 5 — Difference-in-Differences Estimation of Minimum Wage Effects

**Generated**: 2026-03-22
**Source file**: `./projects/mw-textbook/chapters/05-did.qmd`
**Purpose**: Independent verification of substance and logic by external AI models and manual search

---

## For Google Gemini Deep Research

*Gemini strength: broad literature verification, finding contradicting evidence, methodology checks*

### Prompt 1 -- Claim verification against literature

```
I am writing an economics textbook chapter on difference-in-differences
estimation, using minimum wage effects as the running example. Please
verify each of the following claims against the published economics
literature and flag any that are (a) contradicted by well-known results,
(b) stated more strongly than the evidence supports, or (c) missing
important caveats or qualifications.

Claims to verify:

1. "Card and Krueger (1994) found that New Jersey's 1992 minimum wage
   increase from $4.25 to $5.05 produced no statistically significant
   decline in fast-food employment, with a point estimate suggesting a
   small *increase* of approximately 2.76 FTE workers per restaurant
   relative to Pennsylvania."

2. "The consensus from the time-series literature prior to Card and
   Krueger, as summarized by Brown, Gilroy, and Kohen (1982), was that
   a 10 percent increase in the minimum wage reduces teenage employment
   by 1 to 3 percent."

3. "Dube, Lester, and Reich (2010) extended the Card-Krueger border
   comparison design to the entire United States using contiguous county
   pairs straddling state borders, and found employment elasticities
   with respect to the minimum wage that were statistically
   indistinguishable from zero."

4. "Cengiz, Dube, Lindner, and Zipperer (2019) study 138 state-level
   minimum wage changes between 1979 and 2016 and find that the number
   of jobs paying below the new minimum falls sharply, but total
   employment shows no significant net decline."

5. "Neumark and Wascher (2000) challenged Card and Krueger's survey-based
   findings using BLS payroll data and found a negative employment effect
   of approximately 4 percent, reigniting the empirical debate."

6. "The Callaway and Sant'Anna (2021) estimator corrects for bias in
   two-way fixed effects models under staggered treatment timing by
   estimating group-time average treatment effects and aggregating them
   appropriately."

7. "Under a monopsonistic labor market, as modeled by Manning (2003),
   a minimum wage set between the monopsony wage and the competitive
   wage can simultaneously increase both wages and employment."

For each claim, provide:
- Whether the published literature supports, contradicts, or is mixed
- The 2-3 most relevant papers (with full citations)
- Any important caveats the author should add
```

### Prompt 2 -- Methodology check

```
I am using difference-in-differences to estimate the employment effect
of minimum wage increases in large US cities that adopted $15 minimum
wages after 2020. My setting involves staggered treatment timing (cities
adopted at different dates between 2020 and 2025), and I use the
Callaway and Sant'Anna (2021) estimator.

My identification assumptions are:
1. Parallel trends: absent the $15 minimum wage adoption, employment in
   treated cities would have followed the same trend as in not-yet-treated
   and never-treated cities.
2. No anticipation: employers did not adjust employment in advance of the
   minimum wage taking effect.
3. No spillovers: the minimum wage in one city does not affect employment
   in other cities (e.g., through cross-city competition or worker migration).
4. Irreversibility: once a city adopts the $15 minimum, it does not repeal it
   during the sample period.

My data covers quarterly employment counts from the QCEW for all US
metropolitan counties, 2017-2025.

Please evaluate:
1. Are these the correct assumptions for the Callaway-Sant'Anna estimator?
   Am I missing any (e.g., assumptions about the composition of the
   comparison group)?
2. Are there well-known papers showing that parallel trends fail in
   minimum wage settings -- particularly for large cities that may have
   different secular employment trends than smaller cities?
3. What diagnostic tests should I run? I currently have event-study plots
   and a pre-trend F-test. Am I missing anything (e.g., Roth 2022
   sensitivity analysis, Rambachan-Roth honest confidence intervals)?
4. Are there methodological advances since Callaway-Sant'Anna (2021) that
   I should consider (e.g., Borusyak, Jaravel, and Spiess 2024; Sun and
   Abraham 2021; de Chaisemartin and D'Haultfoeuille 2020)?
5. What would a skeptical referee's strongest objection be?
```

---

## For ChatGPT Deep Research

*ChatGPT strength: finding specific facts, checking numbers, verifying policy details*

### Prompt 1 -- Factual and numerical verification

```
Please fact-check the following specific claims from my economics
textbook chapter on minimum wages and difference-in-differences. For
each, verify whether the stated number or fact is accurate by searching
for the original source:

1. "New Jersey raised its minimum wage from $4.25 to $5.05 per hour
   on April 1, 1992."

2. "Card and Krueger surveyed 410 fast-food restaurants — 331 in
   New Jersey and 79 in eastern Pennsylvania."

3. "The pre-treatment mean FTE employment was 20.4 in New Jersey
   restaurants and 23.3 in Pennsylvania restaurants."

4. "The federal minimum wage was last raised to $7.25 per hour in
   July 2009 and has not been increased since."

5. "As of 2025, 30 states and the District of Columbia have minimum
   wages above the federal level of $7.25."

6. "Seattle's $15 minimum wage ordinance, adopted in June 2014, was
   the first major US city to legislate a $15 minimum."

7. "Dube, Lester, and Reich (2010) use all contiguous county pairs
   straddling a state border where the minimum wage differs, covering
   the period 1990 to 2006."

8. "Cengiz et al. (2019) study 138 prominent state-level minimum wage
   events between 1979 and 2016."

9. "The Earned Income Tax Credit was expanded significantly in 1993
   under the Clinton administration, complicating the interpretation
   of employment trends for low-wage workers in the mid-1990s."

10. "Restaurant industry employment as a share of total nonfarm
    employment was approximately 8.3 percent in 2019 according to
    the BLS."

For each:
- Is the stated fact/number correct?
- If not, what is the correct number and what is the source?
- If approximately correct, note the exact figure and source
```

### Prompt 2 -- Citation verification

```
I need to verify that the following citations in my economics textbook
chapter are real papers that actually exist. For each, please confirm:
(a) The paper exists with these authors, title, year, and journal
(b) The paper's actual finding matches my characterization

Citations to verify:

1. Card, David, and Alan B. Krueger. 1994. "Minimum Wages and
   Employment: A Case Study of the Fast-Food Industry in New Jersey
   and Pennsylvania." American Economic Review 84(4): 772-793.
   — I cite this as finding "no significant disemployment effect and
   a point estimate of +2.76 FTE per restaurant."

2. Neumark, David, and William Wascher. 2000. "Minimum Wages and
   Employment: A Case Study of the Fast-Food Industry in New Jersey
   and Pennsylvania." American Economic Review 90(5): 1362-1396.
   — I cite this as "using BLS payroll data to challenge Card and
   Krueger, finding a negative employment effect of about 4 percent."

3. Dube, Arindrajit, T. William Lester, and Michael Reich. 2010.
   "Minimum Wage Effects Across State Borders." Review of Economics
   and Statistics 92(4): 945-964.
   — I cite this as "finding employment elasticities indistinguishable
   from zero using contiguous county pairs."

4. Cengiz, Doruk, Arindrajit Dube, Attila Lindner, and Ben Zipperer.
   2019. "The Effect of Minimum Wages on Low-Wage Jobs." Quarterly
   Journal of Economics 134(3): 1405-1454.
   — I cite this as "finding near-zero net employment effects across
   138 state minimum wage events using a bunching estimator."

5. Callaway, Brantly, and Pedro H.C. Sant'Anna. 2021. "Difference-
   in-Differences with Multiple Time Periods." Journal of Econometrics
   225(2): 200-230.
   — I cite this as "proposing an estimator that avoids negative
   weighting bias in staggered DiD settings."

6. Manning, Alan. 2003. Monopsony in Motion: Imperfect Competition in
   Labor Markets. Princeton University Press.
   — I cite this as "formalizing the modern monopsony framework showing
   minimum wages can increase employment when firms have wage-setting
   power."

7. Azar, José, Ioana Marinescu, and Marshall Steinbaum. 2022. "Labor
   Market Concentration." Journal of Human Resources 57(S): S167-S199.
   — I cite this as "documenting that more concentrated labor markets
   exhibit 5-17% lower posted wages."

Flag any citations that:
- Do not appear to exist
- Exist but the finding does not match my characterization
- Have a different journal or year than I state
- Have been superseded by a more recent or definitive version
```

---

## For Claude Deep Research

*Claude strength: logical consistency, argument structure, identifying subtle flaws in reasoning*

### Prompt 1 -- Logic and argument audit

```
I am writing a textbook chapter arguing that difference-in-differences is
a powerful tool for causal inference in labor economics, using minimum wage
effects as the primary example. Here is my argument structure:

1. The competitive labor market model predicts that a binding minimum wage
   reduces employment (standard supply-and-demand).

2. Testing this prediction requires a credible counterfactual: what would
   employment have been without the minimum wage increase?

3. Simple before-after comparisons are insufficient because other factors
   (e.g., macroeconomic conditions) change simultaneously.

4. Difference-in-differences solves this by using a control group
   (Pennsylvania) to proxy for the counterfactual employment path in the
   treated group (New Jersey).

5. Card and Krueger (1994) implement this design and find no employment
   decline, challenging the competitive model prediction.

6. The key identification assumption is parallel trends: absent the
   treatment, NJ and PA employment would have moved in parallel.

7. This assumption is inherently untestable but can be assessed by
   examining pre-treatment trends.

8. Subsequent literature (Dube et al. 2010, Cengiz et al. 2019) extends
   the Card-Krueger approach with better designs and larger samples,
   broadly confirming the null employment result.

9. The monopsony model (Manning 2003) provides a theoretical explanation:
   when firms have wage-setting power, minimum wages can increase
   employment by forcing firms closer to the competitive equilibrium.

10. Therefore, the DiD evidence on minimum wages illustrates both the
    power of quasi-experimental methods and the limitations of the
    simple competitive model.

Please perform a rigorous logical audit:
1. Does each step follow from the previous one?
2. Are there hidden assumptions I have not stated?
3. Where is the argument weakest -- which step would a skeptic attack?
4. Am I overclaiming at any point? Specifically, does Step 10's broad
   conclusion ("limitations of the simple competitive model") follow from
   evidence in one industry in one state?
5. Am I fairly representing the other side? Neumark and Wascher have
   published extensively arguing for negative employment effects -- am I
   strawmanning the competitive model by not engaging with their evidence?
6. If you were Referee 2 at a top economics journal, what would be your
   single most devastating criticism of this chapter's narrative?

Be adversarial. Do not give me the benefit of the doubt.
```

### Prompt 2 -- Equation and derivation check

```
Please verify the following mathematical derivation from my economics
textbook chapter on difference-in-differences. Check each step for
correctness.

Starting point — the potential outcomes framework:

   Y_it(0) = alpha_i + lambda_t + epsilon_it     ... (5.1)

where alpha_i is a unit fixed effect, lambda_t is a time fixed effect,
and epsilon_it is an idiosyncratic error.

Step 1: Define the treatment effect for unit i at time t:
   tau_it = Y_it(1) - Y_it(0)                     ... (5.2)

Step 2: The observed outcome is:
   Y_it = Y_it(0) + D_it * tau_it                 ... (5.3)
   Y_it = alpha_i + lambda_t + D_it * tau_it + epsilon_it

where D_it = 1 if unit i is treated at time t.

Step 3: In the 2x2 case (two groups, two periods), assume a constant
treatment effect tau_it = tau. Then:
   Y_it = alpha_i + lambda_t + tau * D_it + epsilon_it

Step 4: Taking expectations for the four group-time cells:
   E[Y|NJ, Post] = alpha_NJ + lambda_Post + tau
   E[Y|NJ, Pre]  = alpha_NJ + lambda_Pre
   E[Y|PA, Post] = alpha_PA + lambda_Post
   E[Y|PA, Pre]  = alpha_PA + lambda_Pre

Step 5: The DiD estimator is:
   tau_DiD = (E[Y|NJ,Post] - E[Y|NJ,Pre]) - (E[Y|PA,Post] - E[Y|PA,Pre])
           = (alpha_NJ + lambda_Post + tau - alpha_NJ - lambda_Pre)
             - (alpha_PA + lambda_Post - alpha_PA - lambda_Pre)
           = (lambda_Post - lambda_Pre + tau) - (lambda_Post - lambda_Pre)
           = tau

For each step:
1. Is the algebraic manipulation correct?
2. Is the economic assumption valid?
3. Does the notation remain consistent?
4. What happens when tau = 0? Does the estimator correctly return zero?
5. What happens when we relax the constant treatment effect assumption
   (tau_it varies by unit)? Does Step 5 still identify a meaningful
   parameter, or do we get a weighted average with potentially negative
   weights (the Goodman-Bacon 2021 decomposition problem)?
6. Are there any sign errors or dropped terms?
```

---

## Manual Search Queries for Spot-Checking

### Google Scholar (verify citations exist and check characterizations)

```
"Minimum Wages and Employment" author:Card author:Krueger
```
*Expected: AER 1994, 84(4), 772-793. Verify the 2.76 FTE point estimate appears in the paper.*

```
"Minimum Wages and Employment" author:Neumark author:Wascher 2000
```
*Expected: AER 2000, 90(5). Verify they report negative employment effects using BLS data.*

```
"Minimum Wage Effects Across State Borders" author:Dube author:Lester author:Reich
```
*Expected: Review of Economics and Statistics 2010, 92(4). Verify contiguous county pair design.*

```
"The Effect of Minimum Wages on Low-Wage Jobs" author:Cengiz author:Dube
```
*Expected: QJE 2019, 134(3). Verify 138 state events and bunching estimator.*

```
"Difference-in-Differences with Multiple Time Periods" author:Callaway author:Sant'Anna
```
*Expected: Journal of Econometrics 2021, 225(2). Verify staggered DiD estimator.*

### FRED (verify macroeconomic facts)

- **Federal minimum wage history**: https://fred.stlouisfed.org/series/FEDMINNFRWG
  *Verify: last increase to $7.25 in July 2009*

- **Restaurant employment share**: https://fred.stlouisfed.org/series/CES7072200001
  *Cross-reference with total nonfarm: https://fred.stlouisfed.org/series/PAYEMS*
  *Verify: ~8.3% share claim for 2019*

- **New Jersey unemployment rate, 1991-1993**: https://fred.stlouisfed.org/series/NJUR
  *Check whether NJ had unusual labor market conditions around the 1992 MW increase*

### BLS (verify labor statistics)

- **Quarterly Census of Employment and Wages (QCEW)**: https://data.bls.gov/cew/
  *Verify restaurant-sector employment counts if referenced in the chapter*

- **Current Population Survey (CPS) data**: https://www.bls.gov/cps/
  *Verify any wage distribution statistics cited*

- **State minimum wage history**: https://www.dol.gov/agencies/whd/minimum-wage/state
  *Verify the count of "30 states above $7.25 as of 2025"*

### Census / ACS

- **City-level demographic data**: https://data.census.gov/
  *If the chapter cites demographic characteristics of treated vs. control cities, verify against ACS 5-year estimates*

---

## How to Use These Prompts

1. **Copy-paste** each prompt into the respective platform (Gemini, ChatGPT, Claude).
2. **Compare responses** across models. Pay special attention to:
   - Claims where models *disagree* -- these need manual verification
   - Citations that any model flags as potentially non-existent
   - Methodological concerns raised by 2+ models
3. **Cross-reference** with the `/verify` report (see `verification-report-example.md`). The verification report catches text-level errors; these prompts catch knowledge-level errors.
4. **Run the manual searches** for any claim flagged by the AI models. Google Scholar and FRED are ground truth; AI model responses are helpful but not authoritative.
5. **Document** what you changed based on cross-verification in a changelog or commit message.
