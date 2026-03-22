Stack: Python (pandas, numpy, statsmodels, linearmodels, sklearn, scipy, matplotlib, seaborn, plotly), Jupyter notebooks, Streamlit. Causal ML: econml, doubleml. Data: FRED API, Census, BLS, World Bank.

### /analyze — Run econometric analysis from specification to output

**Trigger**: "run a regression on [topic]", "estimate [model]", "analyze this data", "test [hypothesis]"

**Workflow**:

1. **Clarify the specification** (infer or ask):
   - Dependent variable (Y)
   - Key independent variable / treatment (D or X)
   - Controls (X vector)
   - Fixed effects (entity, time, entity×time)
   - Clustering level for standard errors
   - Sample restrictions
   - Model type: OLS, IV/2SLS, DiD, panel FE/RE, logit/probit, tobit, quantile

2. **Generate analysis script** following these conventions:

   ```python
   """
   Analysis: [Description]
   Date: [YYYY-MM-DD]
   Data: [Source]
   Specification: Y = β₀ + β₁D + β₂X + ε
   """
   import pandas as pd
   import numpy as np
   import statsmodels.api as sm
   import statsmodels.formula.api as smf
   from linearmodels.panel import PanelOLS, FirstDifferenceOLS
   from linearmodels.iv import IV2SLS
   import matplotlib.pyplot as plt
   import seaborn as sns

   # ============================================================
   # 1. DATA LOADING AND CLEANING
   # ============================================================
   df = pd.read_csv("data/[filename].csv")

   # Document every cleaning step
   print(f"Raw observations: {len(df)}")
   df = df.dropna(subset=["key_var"])  # Document why
   print(f"After dropping missing key_var: {len(df)}")

   # ============================================================
   # 2. VARIABLE CONSTRUCTION
   # ============================================================
   # [Create treatment indicators, interactions, transformations]
   # Comment: explain economic rationale for each variable

   # ============================================================
   # 3. SUMMARY STATISTICS
   # ============================================================
   summary = df[["Y", "D", "X1", "X2"]].describe()
   # Export for book/paper
   summary.to_latex("output/tables/summary_stats.tex")
   summary.to_markdown("output/tables/summary_stats.md")

   # ============================================================
   # 4. MAIN SPECIFICATION
   # ============================================================
   # OLS with robust/clustered SEs
   model = smf.ols("Y ~ D + X1 + X2", data=df).fit(
       cov_type="cluster", cov_kwds={"groups": df["cluster_id"]}
   )
   print(model.summary())

   # ============================================================
   # 5. ROBUSTNESS CHECKS
   # ============================================================
   # [Alternative specifications, subsamples, placebo tests]

   # ============================================================
   # 6. EXPORT RESULTS
   # ============================================================
   # Save regression table
   from statsmodels.iolib.summary2 import summary_col
   results_table = summary_col(
       [model, model_robust, model_alt],
       stars=True,
       info_dict={"N": lambda x: f"{int(x.nobs)}",
                  "R²": lambda x: f"{x.rsquared:.3f}"}
   )
   # Export to multiple formats
   with open("output/tables/main_results.tex", "w") as f:
       f.write(results_table.as_latex())
   ```

3. **Code quality rules**:
   - Every script has a docstring header: description, date, data source, specification
   - Every data cleaning step is documented with before/after observation counts
   - Every variable construction includes economic rationale in comments
   - Standard errors are ALWAYS clustered or robust — never default homoskedastic
   - Results export to BOTH LaTeX (.tex) and markdown (.md) for flexibility
   - Use `linearmodels` for panel data and IV, `statsmodels` for cross-section
   - Use `sklearn` for ML tasks, `econml`/`doubleml` for causal ML

4. **Save**: `./projects/[project]/code/[descriptive-name].py`

## Example Invocations
```bash
/analyze "OLS regression of wages on education with state FE and clustered SEs"
/data-wrangle "Pull GDP, unemployment, and CPI from FRED 2000-2025"
/visualize "Event study plot for DiD estimates from my panel regression"
/simulate "Monte Carlo showing OLS bias under endogeneity vs IV consistency"
/replicate "Card and Krueger 1994 minimum wage"
/dashboard "FRED macroeconomic indicators explorer with Streamlit"
/causal-ml "Double ML estimate of treatment effect with high-dimensional controls"
```