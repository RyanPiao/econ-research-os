---
name: econ-analyze
description: Run econometric analysis, data wrangling, visualizations, Monte Carlo simulations, replications, dashboards, and causal ML in Python/Jupyter. Activate for /analyze, /data-wrangle, /visualize, /simulate, /replicate, /dashboard, /causal-ml.
---

# Economics quantitative analysis

Stack: Python (pandas, numpy, statsmodels, linearmodels, sklearn, econml, doubleml, plotly, matplotlib).

Routes:
- `/analyze [specification]` → Read `analyze.md`. Generate analysis script with documented cleaning, specification, robustness. Export tables to LaTeX + markdown.
- `/data-wrangle [source]` → Pull from FRED/Census/BLS/World Bank APIs. Clean with documented steps. Save codebook.
- `/visualize [description]` → Read `visualize.md`. Publication-quality figures (AER style). Save PDF + PNG.
- `/simulate [method]` → Read `simulate.md`. Monte Carlo DGP with parallel execution. Power analysis curves.
- `/replicate [paper]` → AEA Data Editor folder structure. Replication log. Code translation if needed.
- `/dashboard [topic]` → Streamlit scaffold with cached data loading and Plotly charts.
- `/causal-ml [method]` → Read `causal-ml.md`. Double ML, causal forests, CATE via econml/doubleml.

All code saves to `./projects/[name]/code/`. Output to `./projects/[name]/output/`.
