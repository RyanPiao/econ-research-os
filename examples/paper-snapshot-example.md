---
citekey: CardKrueger1994
title: "Minimum Wages and Employment: A Case Study of the Fast-Food Industry in New Jersey and Pennsylvania"
authors: [David Card, Alan B. Krueger]
year: 1994
journal: "American Economic Review"
doi: "10.1257/aer.84.4.772"
tags: [literature_note, labor, minimum-wage, diff-in-diff, natural-experiment, employment]
status: detailed
relevance: 5
methodology: diff-in-diff
jel_codes: [J23, J31, J38]
date_analyzed: 2026-03-22
---

# Card & Krueger (1994) — Minimum Wages and Employment

## Executive Summary

### Research Question
Does an increase in the minimum wage reduce employment in the fast-food industry?

### Main Answer
No — employment in New Jersey fast-food restaurants actually increased slightly relative to Pennsylvania restaurants after New Jersey raised its minimum wage, contradicting the standard competitive labor market prediction.

### Evidence Type
Quasi-experiment (natural experiment)

### Identification Strategy
New Jersey raised its minimum wage from $4.25 to $5.05 per hour on April 1, 1992; neighboring Pennsylvania did not. Card and Krueger exploit this policy discontinuity using a difference-in-differences design: they compare employment changes in fast-food restaurants in New Jersey (treatment) versus eastern Pennsylvania (control) before and after the NJ minimum wage increase. The key identifying assumption is that employment trends in NJ and PA fast-food restaurants would have been parallel in the absence of the minimum wage increase.

### Headline Result
The estimated effect of the NJ minimum wage increase on full-time equivalent (FTE) employment is +2.76 FTEs per restaurant (SE = 1.36), implying a 13% relative increase in employment in NJ versus PA. This result is statistically significant at the 5% level and is the opposite sign predicted by the textbook competitive model.

### Bottom Line
This paper fundamentally disrupted the consensus that minimum wage increases reduce employment, providing the most influential piece of evidence for the "new minimum wage research." Its credibility rests on the natural experiment design, but the result is limited to one state, one industry, and one modest minimum wage increase. It revived interest in monopsony models of labor markets and launched decades of follow-up research.

---

## Knowledge Distillation

### Paper Snapshot
- **Question:** Does an increase in the state minimum wage reduce employment in the fast-food industry?
- **Answer:** No. Employment in New Jersey fast-food restaurants increased relative to Pennsylvania after the NJ minimum wage rise, contradicting the competitive model prediction.
- **Method:** Difference-in-differences comparing FTE employment at fast-food restaurants in NJ (treatment) and eastern PA (control) before (Feb/Mar 1992) and after (Nov/Dec 1992) NJ's minimum wage increase from $4.25 to $5.05. Data collected via phone surveys of 410 Burger King, KFC, Wendy's, and Roy Rogers restaurants. Specifications include OLS with and without controls for chain, ownership type, and region.
- **Key Table/Figure:** Table 3 — Shows the main DiD result. Row 1 reports the change in mean FTE employment: NJ increased by +0.59, PA decreased by -2.16, yielding a DiD estimate of +2.76. This is the paper's headline result.
- **Limitations (authors'):** (1) Short time horizon — only 8 months between surveys; long-run effects could differ. (2) Phone survey data may introduce measurement error in employment counts. (3) Results are specific to fast food; may not generalize to other industries. (4) PA may not be a perfect control if local economic conditions diverged.
- **Limitations (mine):** (1) No formal pre-trends test — only two time periods, so parallel trends is assumed, not tested. (2) FTE is constructed from part-time and full-time headcounts using a specific formula; results may be sensitive to this construction. (3) Attrition between survey waves (about 7%) is not extensively analyzed. (4) Potential for Hawthorne effects — managers knew they were being surveyed. (5) The magnitude (+13%) is large for such a modest minimum wage increase, which raises questions about whether the PA control group was experiencing an independent negative shock.
- **Use for me:** Canonical example of a natural experiment / DiD in labor economics. Essential citation for any minimum wage literature review. Useful teaching example for identification strategy, and for illustrating how an empirical result can challenge established theory.

### Dataset Card
- **Unit of observation:** Fast-food restaurant (store level)
- **N:** 410 restaurants (331 in NJ, 79 in eastern PA) in wave 1; 410 re-interviewed in wave 2 (371 successfully re-contacted)
- **Sample restrictions:** Burger King, KFC, Wendy's, and Roy Rogers restaurants within NJ and a set of eastern PA counties (within reasonable commuting distance of the NJ border). Franchisee-owned and company-owned included.
- **Time period:** Wave 1: February–March 1992 (before NJ increase); Wave 2: November–December 1992 (7–8 months after)
- **Key variables:**
  - Dependent: FTE employment = full-time workers + 0.5 * part-time workers (constructed from manager-reported headcounts)
  - Treatment/Key IV: NJ indicator (= 1 if restaurant is in New Jersey)
  - Controls: chain dummies (BK, KFC, Wendy's, Roy Rogers), company-owned indicator, region within NJ (shore, central, north, south)
- **Data source:** Original phone survey conducted by the authors. Managers were called and asked about current employment, wages, prices, and hours.
- **Missingness handling:** 39 restaurants (9.5%) could not be re-contacted in wave 2 due to closures, refusals, or inability to reach a manager. Authors conduct attrition analysis and find no differential attrition between NJ and PA.

### Regression Table Decode
**Table 3: Average Employment Per Store Before and After the Rise in NJ Minimum Wage**
| Element | Detail |
|---|---|
| Dependent variable | FTE employment per store (full-time + 0.5 * part-time) |
| Key IV | NJ indicator (treatment state) |
| Baseline estimate | DiD = +2.76 FTE, SE = 1.36, p < 0.05 |
| Plain-language | New Jersey restaurants gained roughly 2.8 more full-time equivalent employees per store relative to Pennsylvania restaurants after the minimum wage increase, holding pre-treatment levels constant. This represents approximately a 13% relative employment increase. |
| Controls / FE | None in baseline specification (Table 3 is a simple 2x2 DiD of means). Table 4 adds chain dummies, company-ownership indicator, and NJ region controls. |
| SE clustering | Standard errors not clustered in Table 3 (simple comparison of means). Regression specifications in Table 4 report heteroskedasticity-robust SEs. |
| N | 410 stores (wave 1), 371 stores (wave 2 panel) |
| R² | Not reported for the means comparison; R² = 0.07–0.10 for regression specifications in Table 4 |

**Table 4, Column (iii): Regression-Adjusted DiD**
| Element | Detail |
|---|---|
| Dependent variable | Change in FTE employment (wave 2 minus wave 1) |
| Key IV | NJ dummy |
| Baseline estimate | β = +2.30 FTE, SE = 1.19, p ≈ 0.054 |
| Plain-language | After controlling for chain type, ownership, and region, NJ restaurants still gained about 2.3 more FTEs than PA restaurants, though this estimate is marginally significant. |
| Controls / FE | Chain dummies (4), company-owned indicator, region within NJ (4 categories) |
| SE clustering | Robust standard errors |
| N | 357 stores with complete data |
| R² | 0.10 |

### Robustness Assessment
| Check | Result | Survives? |
|---|---|---|
| Within-NJ variation (low- vs. high-wage stores) | Stores initially at or near the old minimum wage (most affected by increase) show larger employment gains | Yes — strengthens the result |
| Balanced sample (only stores in both waves) | DiD estimate remains positive and similar in magnitude | Yes |
| Alternative FTE definitions | Using full-time only or total headcount instead of the 0.5 weighting gives qualitatively similar results | Yes (magnitudes vary) |
| Controlling for chain, ownership, region | Estimate changes from 2.76 to 2.30 but remains positive | Yes (marginally significant) |
| Prices (pass-through test) | NJ restaurants raised prices of a standard meal by ~4% relative to PA — consistent with cost pass-through rather than employment reduction | Yes — supports monopsony / price pass-through interpretation |
| BLS administrative data (follow-up) | Neumark and Wascher (2000) challenged results using payroll data, finding negative effects; Card and Krueger (2000) responded with BLS data showing null/positive effects. Disagreement persists. | Contested |

### Atomic Notes

- **Claim/Concept:** A modest minimum wage increase need not reduce employment in low-wage industries.
- **Evidence:** Card & Krueger 1994, Table 3 — DiD estimate of +2.76 FTEs (SE = 1.36).
- **My Commentary:** The result directly contradicts the textbook supply-and-demand prediction. The key question is whether this reflects monopsony power, measurement error, or an idiosyncratic PA downturn.
- **Tags:** [minimum-wage, employment, natural-experiment]

- **Claim/Concept:** Phone survey data can be used to construct credible employment measures for quasi-experimental analysis.
- **Evidence:** Card & Krueger 1994, Section II — detailed description of survey instrument, response rates, and FTE construction.
- **My Commentary:** The phone survey approach was innovative but generated controversy. Neumark and Wascher (2000) argued payroll data told a different story. This debate highlights how measurement choices can drive empirical conclusions.
- **Tags:** [measurement, survey-data, employment-measurement]

- **Claim/Concept:** Within-treatment-group variation can strengthen a DiD design by providing a dose-response check.
- **Evidence:** Card & Krueger 1994, Table 5 — comparison of initially low-wage vs. high-wage NJ stores. Stores most affected by the minimum wage increase (those previously paying near $4.25) show the largest employment gains.
- **My Commentary:** This is a powerful placebo-like test within the treatment group. It is more consistent with a causal minimum wage effect than with a generic NJ economic boom.
- **Tags:** [diff-in-diff, dose-response, identification]

- **Claim/Concept:** Fast-food restaurants may pass minimum wage costs through to consumer prices rather than reducing employment.
- **Evidence:** Card & Krueger 1994, Table 6 — NJ restaurants increased meal prices by approximately 4% relative to PA.
- **My Commentary:** Price pass-through is consistent with both competitive and monopsony models. The combination of no employment decline plus price increase is more consistent with monopsony or a model with product market power.
- **Tags:** [minimum-wage, price-pass-through, monopsony]

- **Claim/Concept:** The parallel trends assumption in a two-period DiD cannot be directly tested — it must be argued on substantive grounds.
- **Evidence:** Card & Krueger 1994 implicitly assumes NJ and PA fast-food employment would have trended identically absent the minimum wage change. With only two time periods, no pre-trend test is possible.
- **My Commentary:** This is the paper's most vulnerable assumption. Later work (e.g., Dube, Lester, and Reich 2010) uses longer panels with event-study designs to directly assess pre-trends in contiguous border counties.
- **Tags:** [parallel-trends, diff-in-diff, identification-assumption]

---

## Literature Review Integration

### Critical Appraisal Summary
- **Internal validity:** Strong for a natural experiment of its era. The NJ/PA comparison is plausible given geographic proximity. The within-NJ dose-response check strengthens the case. However, the two-period design prevents formal pre-trend testing, and the lack of clustered SEs by region is a weakness by modern standards.
- **Construct validity:** FTE employment constructed from phone surveys is reasonable but imperfect. The 0.5 weighting for part-time workers is an assumption, and managers' phone-reported headcounts may contain measurement error. The Neumark-Wascher payroll data challenge directly questions construct validity.
- **External validity:** Limited to fast food, one state, one time period, and one modest ($0.80) minimum wage increase. Results may not generalize to larger increases, other industries, other labor markets, or periods with different macroeconomic conditions.
- **Reproducibility:** Original survey data have been made available. The analysis is straightforward (means comparisons, simple OLS). Multiple replications and extensions exist. However, the BLS data reanalysis in Card and Krueger (2000) partly supersedes the original survey data results.

### Contribution Map
- **Strands claimed:**
  1. **Minimum wage and employment** — the paper directly enters the decades-old debate about whether minimum wages reduce employment, providing the first large-scale natural experiment evidence against the competitive model prediction.
  2. **Natural experiments in economics** — alongside Angrist (1990) and other early natural experiment papers, Card and Krueger helped establish the "credibility revolution" methodology of using policy variation as quasi-random treatment assignment.
  3. **Monopsony in labor markets** — the results are consistent with monopsonistic labor markets where firms have wage-setting power, reviving a theoretical strand that had been largely dormant since Robinson (1933).
- **Closest comparisons:** Brown, Gilroy, and Kohen (1982) — the canonical pre-1994 survey of minimum wage studies finding consensus negative employment effects; Katz and Krueger (1992) — earlier Card/Krueger study of the 1991 federal minimum wage increase in Texas fast food.
- **Marginal contribution:** First study to use a clean state-border natural experiment with establishment-level data to estimate minimum wage employment effects. Previous studies relied on time-series variation or cross-state correlations, which are more vulnerable to confounding.
- **Organizing principle used:** By finding — the paper positions itself against the prevailing "consensus" of negative employment effects and frames the contribution as a credible challenge to that consensus.

### Gap Analysis
| Gap Type | Description | Testable Next Step |
|---|---|---|
| Boundary | Results are specific to fast food in NJ in 1992. Do they hold for other industries, other states, larger minimum wage increases, or different macroeconomic conditions? | Replicate the design in other state border pairs and industries using longer panels (see Dube, Lester, and Reich 2010). |
| Mechanism | The paper documents the employment effect (or lack thereof) but does not definitively distinguish between monopsony, price pass-through, efficiency wages, or reduced turnover as the mechanism. Why don't firms cut employment? | Test for monopsony directly by estimating firm-level labor supply elasticities (see Manning 2003; Azar, Marinescu, and Steinbaum 2022). |
| Measurement | FTE employment from phone surveys may contain measurement error. The Neumark-Wascher challenge using payroll data raises the possibility that the null result is an artifact of noisy data. | Re-estimate using administrative payroll or unemployment insurance records, which are less subject to reporting error. |
| Design | Two-period DiD prevents testing parallel trends and limits analysis of dynamic effects. No ability to assess whether the employment effect is immediate or develops over time. | Use event-study designs with multiple pre- and post-periods (as later papers do). Apply modern staggered DiD methods (Callaway and Sant'Anna 2021) to multi-state minimum wage variation. |

### Citation Seeds
**Backward (this paper cites — I should read):**
1. Brown, Gilroy, and Kohen (1982), "The Effect of the Minimum Wage on Employment and Unemployment" — **why:** the canonical pre-1994 survey establishing the "consensus" that minimum wages reduce employment by 1–3%. Card & Krueger are explicitly challenging this consensus.
2. Katz and Krueger (1992), "The Effect of the Minimum Wage on the Fast-Food Industry" — **why:** earlier companion study using the 1991 federal minimum wage increase in Texas; establishes the survey methodology that Card & Krueger extend.
3. Neumark and Wascher (1992), "Employment Effects of Minimum and Subminimum Wages: Panel Data on State Minimum Wage Laws" — **why:** contemporaneous study finding negative employment effects using state panel data; represents the opposing view in the minimum wage debate.

**Forward (who cites this — search for):**
- Google Scholar: `cites:CardKrueger1994` or `"Card and Krueger" minimum wage employment` — over 6,000 citations as of 2025
- Key follow-ups to prioritize:
  - Neumark and Wascher (2000) — direct challenge using payroll data
  - Card and Krueger (2000) — rebuttal using BLS data
  - Dube, Lester, and Reich (2010) — contiguous border county design
  - Cengiz, Dube, Lindner, and Zipperer (2019) — bunching estimator across all US minimum wage changes
  - Harasztosi and Lindner (2019) — Hungarian minimum wage increase with admin data

### Position in Literature Map
- **Cluster:** Minimum wage and labor demand; credibility revolution / natural experiments in labor economics
- **Role:** Foundational — this is the single most influential paper in the modern minimum wage debate and a canonical example of natural experiment methodology in economics
- **Builds on:** Brown, Gilroy, and Kohen (1982); Stigler (1946) competitive model; Robinson (1933) monopsony theory; Katz and Krueger (1992)
- **Extended by:** Dube, Lester, and Reich (2010); Cengiz et al. (2019); Neumark and Wascher (2000, 2008); Manning (2003); Autor, Manning, and Smith (2016)

---

## Follow-Up Actions
- [ ] Read: Neumark and Wascher (2000) — "The Effect of New Jersey's Minimum Wage Increase on Fast-Food Employment: A Reevaluation Using Payroll Records"
- [ ] Read: Dube, Lester, and Reich (2010) — "Minimum Wage Effects Across State Borders"
- [ ] Read: Cengiz, Dube, Lindner, and Zipperer (2019) — "The Effect of Minimum Wages on Low-Wage Jobs"
- [ ] Read: Manning (2003) — *Monopsony in Motion* (for theoretical framework explaining the result)
- [ ] Check: Download Card and Krueger's original dataset and replicate Table 3 as a DiD exercise
- [ ] Idea: Use modern staggered DiD methods (Callaway-Sant'Anna, Sun-Abraham) to revisit the NJ/PA setting with longer panel data
- [ ] Data: Explore Quarterly Census of Employment and Wages (QCEW) for NJ/PA county-level fast-food employment around 1992

---
*Generated by Econ Paper Reader & Lit Review Tool — Read Mode*
*Methodology: Keshav Three-Pass + Angrist/Pischke Design-Based Credibility*
