---
citekey: CardKrueger1994
title: "Minimum Wages and Employment: A Case Study of the Fast-Food Industry in New Jersey and Pennsylvania"
authors: [David Card, Alan B. Krueger]
year: 1994
journal: "American Economic Review"
doi: "10.2307/2118030"
tags: [literature_note, minimum-wage, employment, diff-in-diff, natural-experiment, labor]
status: detailed
relevance: 5
methodology: diff-in-diff
jel_codes: [J31, J38, C21]
date_analyzed: 2026-03-22
---

# Minimum Wages and Employment: A Case Study of the Fast-Food Industry in New Jersey and Pennsylvania

**Card and Krueger (1994)** | American Economic Review 84(4): 772-793

---

## Executive Summary

Card and Krueger exploit New Jersey's 1992 minimum wage increase (from $4.25 to $5.05) as a natural experiment, comparing employment changes in fast-food restaurants in New Jersey (treated) with those in eastern Pennsylvania (control). Using a telephone survey of 410 restaurants conducted before and after the policy change, they find **no evidence that the minimum wage increase reduced employment** -- and point estimates suggest a small positive effect. This paper fundamentally challenged the competitive labor market consensus that minimum wage floors necessarily reduce employment, becoming one of the most cited and debated papers in labor economics.

**One-sentence finding**: A $0.80 (18.8%) increase in New Jersey's minimum wage produced no statistically significant decline in fast-food employment relative to neighboring Pennsylvania.

---

## Knowledge Distillation

### Paper Snapshot

| Parameter | Detail |
|---|---|
| **Research question** | Does an increase in the minimum wage reduce employment in the fast-food industry? |
| **Object of study** | Fast-food restaurants (Burger King, KFC, Wendy's, Roy Rogers) in NJ and eastern PA |
| **Method** | Difference-in-differences: compare employment changes in NJ restaurants (treated by the wage increase) vs. PA restaurants (control) before and after April 1, 1992 |
| **Identification** | The NJ minimum wage increase was a state-level legislative change unrelated to local labor market conditions; PA provides a geographically adjacent control |
| **Main claim** | The minimum wage increase did not reduce employment in NJ fast-food restaurants; if anything, employment rose slightly relative to PA |
| **Scope limits** | Fast-food industry only; one state; one wage increase ($4.25 to $5.05); short-run effects (8 months post-treatment) |
| **Key assumption** | Absent the minimum wage increase, employment trends would have been parallel in NJ and PA fast-food restaurants |

### Dataset Card

| Parameter | Detail |
|---|---|
| **Unit of observation** | Fast-food restaurant |
| **N** | 410 restaurants (331 NJ, 79 PA) in wave 1; 410 in wave 2 (with some attrition and replacement) |
| **Sample restrictions** | Burger King, KFC, Wendy's, and Roy Rogers restaurants within NJ and eastern PA (within a reasonable commuting distance of the NJ border) |
| **Time period** | Wave 1: February/March 1992 (before the April 1 increase); Wave 2: November/December 1992 (7-8 months after) |
| **Dependent variable** | Full-time equivalent (FTE) employment = full-time workers + 0.5 * part-time workers |
| **Treatment variable** | Location in New Jersey (binary: NJ = 1, PA = 0) |
| **Key controls** | Chain indicator (4 chains), company-owned vs. franchise, region within NJ |
| **Data source** | Original telephone survey conducted by the authors |
| **Missingness** | ~6% of wave 1 restaurants could not be reached in wave 2; replaced with new sample from same chains/areas; results robust to excluding replacements |
| **Summary statistics** | Pre-treatment mean FTE: NJ = 20.4, PA = 23.3; starting wage: NJ = $4.61, PA = $4.63 |

### Regression Table Decode: Main Employment Result (Table 3)

| Parameter | Detail |
|---|---|
| **Dependent variable** | Change in FTE employment (wave 2 minus wave 1) |
| **Key independent variable** | NJ dummy (= 1 if restaurant is in New Jersey) |
| **Baseline coefficient** | +2.76 FTE (SE = 1.36) in the simple difference-in-differences |
| **Significance** | Statistically significant at the 5% level (t = 2.03) |
| **Identification** | Comparison of mean employment changes: NJ restaurants gained 0.59 FTE on average while PA restaurants lost 2.16 FTE; the DiD estimate is the difference of these changes |
| **Controls** | Specifications with and without chain dummies and ownership status; results robust across specifications |
| **Fixed effects** | None (two-period, two-group design) |
| **Standard errors** | Conventional OLS standard errors; no clustering (single policy shock) |
| **Sample** | 410 restaurants in main specification; 357 in balanced panel (no replacements) |
| **Robustness** | Result survives: (a) balanced panel only, (b) controlling for chain and ownership, (c) using proportional rather than level changes, (d) excluding restaurants with imputed data, (e) comparing within-NJ variation by initial wage gap |

**Plain-language interpretation**: Fast-food restaurants in New Jersey, which were forced to raise wages by the minimum wage increase, added approximately 2.8 more FTE workers than comparable Pennsylvania restaurants over the same period. This contradicts the standard prediction that employment should fall when wages are forced above the market-clearing level.

**Note on economic vs. statistical significance**: The positive point estimate of 2.76 FTE represents roughly a 13% increase relative to the PA control group's employment decline. However, the confidence interval includes effects ranging from approximately +0.1 to +5.5, so the data are also consistent with a very small positive effect or near-zero effect. The key finding is the *absence* of the predicted negative effect, not necessarily the presence of a positive one.

---

## Critical Appraisal

### Claims and Scope

- **Exact claim**: No evidence that the NJ minimum wage increase reduced fast-food employment; point estimate is positive.
- **Scope**: Fast-food industry in one state (NJ), one wage increase, short-run (8 months).
- **Counterclaim ruled out**: The standard competitive model prediction that a binding minimum wage reduces employment is not supported in this setting.

### Assumptions and Logic

| Assumption | Explicit? | Testable? | Assessment |
|---|---|---|---|
| Parallel trends (absent treatment, NJ and PA employment would move together) | Implicit | Partially -- authors show similar pre-treatment levels and trends in wages | Plausible given geographic proximity, but no multi-period pre-trend test |
| No spillovers from NJ to PA | Implicit | Difficult | Some PA restaurants near the border could have gained workers fleeing NJ jobs or lost customers to cheaper NJ restaurants -- not discussed |
| SUTVA (no interference between units) | Implicit | No | Fast-food chains may reallocate employment across restaurants; franchise/corporate decisions may not be independent |
| Measurement validity (FTE captures true employment) | Explicit | Yes | Authors discuss and test alternative FTE definitions; results robust |

### Validity Assessment

- **Internal validity**: Reasonably strong for a natural experiment of its era. Geographic proximity of treatment and control groups limits confounding from regional shocks. However, the single policy change means the estimate captures one realization of the treatment effect, and there is no pre-trend analysis in the modern event-study sense.
- **Construct validity**: FTE employment is a standard measure; however, it does not capture hours adjustments within FTE categories, potential quality-of-hire changes, or substitution between worker types.
- **External validity**: Limited. Fast-food is a specific industry with high turnover, low margins, and price-setting power. Results may not generalize to other sectors, larger wage increases, or different labor market conditions.

### Reproducibility

- **Data**: Survey data collected by authors; later made available. The BLS conducted a follow-up survey that partially replicated the data collection.
- **Code**: Not available (pre-replication era). Methodology is simple enough to reproduce from published tables.
- **Replication**: Neumark and Wascher (2000) challenged results using BLS payroll data, finding negative employment effects. Card and Krueger (2000) responded with updated analysis showing original results hold.

### Citations and Positioning

- Appropriately cites Brown, Gilroy, and Kohen (1982) as the prior consensus survey.
- Cites Katz and Krueger (1992) as a precursor using the same NJ wage increase with different data.
- Does not cite any monopsony models (Manning's textbook came later in 2003), though the results are consistent with monopsonistic competition.

---

## Contribution Map

Card and Krueger (1994) positions itself within three strands of literature:

### Strand 1: Minimum Wage Employment Effects (broadest)

The dominant prior was Brown, Gilroy, and Kohen (1982), which surveyed time-series studies and established the "consensus" that a 10% minimum wage increase reduces teen employment by 1-3%. Card and Krueger challenge this using a quasi-experimental design rather than aggregate time series. Their contribution: **first large-scale natural experiment on minimum wages**, providing identification that avoids the confounding inherent in national time-series regressions.

### Strand 2: Natural Experiments and Credibility in Labor Economics

Part of the broader "credibility revolution" (later named by Angrist and Pischke 2010). Card and Krueger demonstrate that state-level policy variation can be exploited for credible causal inference. Precursors include Card (1990) on the Mariel boatlift and Meyer and Katz (1990) on unemployment insurance. Their contribution: **a clean diff-in-diff design using a sharp policy discontinuity** that became a template for subsequent minimum wage studies.

### Strand 3: Monopsony and Labor Market Power (implicit)

While Card and Krueger (1994) do not formally invoke monopsony models, their results are inconsistent with perfect competition and consistent with employer wage-setting power. This strand was formalized later by Manning (2003) and Burdett and Mortensen (1998). Their contribution (recognized ex post): **empirical evidence motivating renewed interest in monopsonistic labor market models**.

**Organizing principle**: Card and Krueger organize by methodology (case study vs. time series), arguing their natural experiment design is more credible than the prior regression-based literature.

---

## Gap Analysis

### Boundary Gaps

- **Long-run effects**: The study captures only 8 months post-treatment. Longer-run adjustments (entry/exit, capital-labor substitution, automation) are not observed. Does the null employment effect persist at 2, 5, or 10 years?
- **Larger wage increases**: The NJ increase was 18.8% ($0.80). Do results hold for larger increases (e.g., $15 minimum wage movements post-2012)?
- **Other industries**: Results are for fast food only. Do they generalize to retail, hospitality, or manufacturing?

### Mechanism Gaps

- **Why no employment decline?** The paper documents the effect (or lack thereof) but does not pin down the mechanism. Candidates include: (a) monopsony power, (b) efficiency wages, (c) reduced turnover, (d) price pass-through to consumers, (e) demand stimulus from higher wages. Card and Krueger provide some evidence of price increases but do not formally test channels.
- **Margin of adjustment**: If employment does not adjust, what does? Hours? Product quality? Non-wage benefits? Speed of service?

### Measurement Gaps

- **FTE vs. hours**: FTE employment masks within-category hours adjustments. A restaurant could maintain headcount but cut hours per worker.
- **Self-reported data**: Telephone surveys introduce measurement error. Neumark and Wascher (2000) argue payroll data gives different results, highlighting the sensitivity to data source.
- **Entry and exit**: The survey covers existing restaurants only. If the minimum wage caused marginal restaurants to close or deterred new entry, the surviving sample would show an upward bias.

### Design Gaps

- **Single treatment event**: One state, one time, one policy change. The DiD estimate captures one realization, making it hard to assess external validity or conduct inference about the general effect of minimum wages.
- **No pre-trend test**: Modern event-study designs would show multiple pre-treatment periods to validate parallel trends. Card and Krueger have only one pre-period.
- **Geographic spillovers**: PA restaurants near the NJ border may be affected by the NJ policy (cross-border shopping, worker migration), biasing the control group.

**Actionable next step**: Apply the bunching estimator from Cengiz et al. (2019) to CPS or QWI microdata for the NJ 1992 episode to test whether the null employment effect reflects genuine labor demand insensitivity or measurement limitations in the original survey data.

---

## Citation Seeds

### Backward (key references this paper builds on)

| Citekey | Relevance |
|---|---|
| Brown, Gilroy, and Kohen (1982) | Prior consensus survey: 10% MW increase reduces teen employment by 1-3%. Card and Krueger's main foil. |
| Katz and Krueger (1992) | Earlier study using the *same* NJ minimum wage increase with Texas fast-food data as control. Found similar null employment effects. Card and Krueger extend with a better control group (adjacent PA). |
| Card (1992) | Card's study of the 1988 California minimum wage increase using a similar natural experiment approach. Part of the same research agenda. |

### Forward (key papers that build on this work)

| Citekey | Relevance |
|---|---|
| Neumark and Wascher (2000) | Direct challenge using BLS payroll data instead of survey data. Find negative employment effects. Sparked a decade-long data debate. |
| Dube, Lester, and Reich (2010) | Extend the border-comparison design to the full US, using contiguous cross-state county pairs. Find minimal disemployment effects nationally, supporting Card and Krueger. |
| Cengiz, Dube, Lindner, and Zipperer (2019) | Use a bunching estimator across 138 state-level MW changes (1979-2016). Find employment effects near zero overall, with clear wage compression at the bottom. Most comprehensive test of the Card-Krueger finding. |

---

## Reading Metadata

| Parameter | Value |
|---|---|
| **Depth** | Working understanding (second-pass) |
| **Time spent** | ~45 minutes |
| **Confidence in summary** | High -- this is one of the most discussed papers in labor economics; claims are well-documented across the literature |
| **Follow-up needed** | Read Neumark and Wascher (2000) response and Card and Krueger (2000) reply; read Dube et al. (2010) for the extended border design |
