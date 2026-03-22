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

### /data-wrangle — Clean and prepare economic data

**Trigger**: "clean this data", "pull data from FRED", "prepare the dataset", "merge these sources"

**Workflow**:

1. **Identify data sources and actions needed**

2. **For FRED API data**:
   ```python
   """
   Data acquisition: [Series names] from FRED
   """
   import pandas_datareader.data as web
   from fredapi import Fred
   import os

   fred = Fred(api_key=os.environ.get("FRED_API_KEY"))

   # Pull series
   gdp = fred.get_series("GDP", start="2000-01-01")
   cpi = fred.get_series("CPIAUCSL", start="2000-01-01")
   unrate = fred.get_series("UNRATE", start="2000-01-01")

   # Merge on date index
   df = pd.DataFrame({
       "gdp": gdp,
       "cpi": cpi,
       "unrate": unrate
   })

   # Compute derived variables
   df["gdp_growth"] = df["gdp"].pct_change(4) * 100  # YoY %
   df["real_gdp"] = df["gdp"] / (df["cpi"] / 100)
   df["inflation"] = df["cpi"].pct_change(12) * 100

   # Document and save
   print(df.describe())
   df.to_csv("data/processed/fred_macro_panel.csv", index=True)
   ```

3. **For Census / BLS / World Bank data**:
   ```python
   # Census API
   import requests
   census_url = "https://api.census.gov/data/2022/acs/acs5"
   params = {
       "get": "B19013_001E,B01003_001E",  # median income, population
       "for": "county:*",
       "in": "state:*",
       "key": os.environ.get("CENSUS_API_KEY")
   }

   # World Bank
   from pandas_datareader import wb
   gdp_data = wb.download(
       indicator="NY.GDP.PCAP.KD",
       country="all", start=2000, end=2025
   )
   ```

4. **Data cleaning checklist** (always execute):
   - [ ] Check dtypes: dates as datetime, numerics as float/int
   - [ ] Document missingness: `df.isnull().sum()` with interpretation
   - [ ] Check for duplicates: `df.duplicated().sum()`
   - [ ] Validate ranges: are values plausible? (negative wages? GDP > 100 trillion?)
   - [ ] Standardize variable names: lowercase, underscores, descriptive
   - [ ] Create a codebook: variable name, description, source, unit, coverage

5. **Save**: `./projects/[project]/code/data_[source]_cleaning.py`
   Also save codebook: `./projects/[project]/data/codebook.md`

### /replicate — Reproduce results from a published paper

**Trigger**: "replicate [paper]", "reproduce [author year]", "check if I can get the same results as [paper]"

**Workflow**:

1. **Setup replication structure** (AEA Data Editor standard):
   ```
   ./projects/[project]/replication/
   ├── README.md           # Replication instructions
   ├── code/
   │   ├── 01_data_prep.py
   │   ├── 02_main_results.py
   │   ├── 03_robustness.py
   │   └── 04_figures.py
   ├── data/
   │   ├── raw/            # Original data (or instructions to obtain)
   │   └── processed/      # Cleaned data
   ├── output/
   │   ├── tables/
   │   └── figures/
   └── logs/
       └── replication_log.md
   ```

2. **Replication log template**:
   ```markdown
   # Replication Log: [Authors (Year)] — [Short Title]
   **Original paper**: [Full citation]
   **Replication data source**: [OpenICPSR / Dataverse / author website / reconstructed]
   **Date started**: [YYYY-MM-DD]

   ## Target results
   | Table/Figure | Description | Original result | My result | Match? |
   |---|---|---|---|---|
   | Table 2, Col 3 | Baseline DiD | β = 0.15 (0.04) | | |

   ## Data notes
   - [How I obtained the data]
   - [Any differences from the original]

   ## Code notes
   - [Translating from Stata/R to Python]
   - [Any judgment calls or ambiguities]

   ## Discrepancies
   - [Document ANY differences and hypothesize why]
   ```

3. **Replication execution**:
   - Download original replication package if available (check OpenICPSR, Dataverse, author websites)
   - If Stata code → translate to Python, documenting each translation choice
   - Run original specification first, compare exact numbers
   - Then run your extensions/robustness
   - Document everything in the replication log

4. **Save**: `./projects/[project]/replication/`

### /dashboard — Build interactive Streamlit/Plotly dashboards

**Trigger**: "build a dashboard for [topic]", "create an interactive app", "Streamlit app for [data]"

**Workflow**:

1. **Generate Streamlit app scaffold**:
   ```python
   """
   Dashboard: [Description]
   Data: [Source]
   Run: streamlit run app.py
   """
   import streamlit as st
   import pandas as pd
   import plotly.express as px
   from fredapi import Fred

   st.set_page_config(page_title="[Title]", layout="wide")
   st.title("[Dashboard Title]")

   # --- Sidebar controls ---
   st.sidebar.header("Parameters")
   start_year = st.sidebar.slider("Start year", 1990, 2025, 2000)
   series = st.sidebar.multiselect(
       "FRED Series",
       ["GDP", "UNRATE", "CPIAUCSL", "FEDFUNDS"],
       default=["GDP", "UNRATE"]
   )

   # --- Data loading (cached) ---
   @st.cache_data(ttl=3600)
   def load_fred(series_list, start):
       fred = Fred(api_key=st.secrets["FRED_API_KEY"])
       data = {}
       for s in series_list:
           data[s] = fred.get_series(s, start=f"{start}-01-01")
       return pd.DataFrame(data)

   df = load_fred(series, start_year)

   # --- Visualization ---
   col1, col2 = st.columns(2)
   with col1:
       fig = px.line(df, title="Time Series")
       st.plotly_chart(fig, use_container_width=True)
   with col2:
       st.dataframe(df.describe())

   # --- Analysis section ---
   st.subheader("Correlation Analysis")
   if len(series) >= 2:
       corr = df.corr()
       fig_corr = px.imshow(corr, text_auto=".2f",
                            title="Correlation Matrix")
       st.plotly_chart(fig_corr)
   ```

2. **Save**: `./projects/[project]/apps/[app-name]/app.py`

## Jupyter Notebook Conventions

### Structure for reproducible notebooks
```
./projects/[project]/notebooks/
├── 01_data_exploration.ipynb
├── 02_main_analysis.ipynb
├── 03_robustness.ipynb
├── 04_simulations.ipynb
└── 05_figures_for_book.ipynb
```

### Notebook best practices
- **First cell**: imports + configuration (matplotlib style, random seeds, paths)
- **Second cell**: data loading with inline documentation
- **Use markdown cells extensively**: explain the economic logic, not just the code
- **Number notebooks sequentially**: they should be runnable in order
- **Set random seeds everywhere**: `np.random.default_rng(42)` for reproducibility
- **Clear all outputs before committing** (use `nbstripout` pre-commit hook):
  ```bash
  pip install nbstripout
  nbstripout --install  # adds git filter
  ```
- **For book companion notebooks**: each notebook maps to one chapter; heading structure matches the chapter's sections

### Jupytext for clean git diffs
```bash
pip install jupytext
# Pair notebooks with .py files for readable diffs
jupytext --set-formats ipynb,py:percent notebooks/*.ipynb
# Now git tracks the .py file; .ipynb is auto-generated
```

## Python Package Requirements

```
# requirements.txt for econ analysis
pandas>=2.2
numpy>=1.26
scipy>=1.12
statsmodels>=0.14
linearmodels>=6.0
matplotlib>=3.8
seaborn>=0.13
plotly>=5.18
scikit-learn>=1.4
econml>=0.15
doubleml>=0.7
fredapi>=0.5
pandas-datareader>=0.10
streamlit>=1.30
jupyter>=1.0
jupytext>=1.16
nbstripout>=0.7
joblib>=1.3
tqdm>=4.66
```

Install: `pip install -r requirements.txt`

## Output Directory Structure
```
./projects/[project]/output/
├── tables/
│   ├── summary_stats.tex          # LaTeX for book
│   ├── summary_stats.md           # Markdown for GitHub
│   ├── main_results.tex
│   └── main_results.md
├── figures/
│   ├── fig_05_01_event_study.pdf   # Vector for book
│   ├── fig_05_01_event_study.png   # Raster for web
│   └── fig_05_02_coef_plot.pdf
└── data/
    └── processed/                  # Analysis-ready datasets
```

## Integration with Book Chapters

Since you chose separate code files referenced from the book, use this pattern in your Quarto chapters:

```markdown
## 5.4 Estimation results

The baseline difference-in-differences estimate appears in Table 5.1.
The full code for reproducing this analysis is available in
`code/ch05_did_estimation.py` in the companion repository.

::: {#tbl-did-baseline}
{{< include ../output/tables/main_results.md >}}

Baseline DiD estimates with clustered standard errors.
:::

Figure @fig-event-study shows the event study coefficients,
confirming the parallel pre-trends assumption.

![Event study estimates](../output/figures/fig_05_01_event_study.pdf){#fig-event-study}
```

This keeps code separate while allowing Quarto to include the output directly.

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
