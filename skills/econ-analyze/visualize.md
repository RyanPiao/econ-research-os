### /visualize — Create publication-quality figures

**Trigger**: "make a figure of [topic]", "plot [relationship]", "create a chart", "visualize [data]"

**Workflow**:

1. **Set publication style globally**:
   ```python
   import matplotlib.pyplot as plt
   import matplotlib as mpl

   # AER-compatible style
   plt.rcParams.update({
       "figure.figsize": (8, 5),
       "figure.dpi": 300,
       "font.family": "serif",
       "font.size": 11,
       "axes.labelsize": 12,
       "axes.titlesize": 13,
       "xtick.labelsize": 10,
       "ytick.labelsize": 10,
       "legend.fontsize": 10,
       "axes.spines.top": False,
       "axes.spines.right": False,
       "axes.grid": False,
       "figure.facecolor": "white",
       "savefig.bbox": "tight",
       "savefig.dpi": 300,
   })

   # Color palette (colorblind-friendly)
   COLORS = ["#1f77b4", "#d62728", "#2ca02c", "#ff7f0e",
             "#9467bd", "#8c564b", "#e377c2"]
   ```

2. **Standard econometrics figure types**:

   **Coefficient plot** (for regression results):
   ```python
   def coef_plot(model, vars_to_plot, labels=None, title=""):
       coefs = model.params[vars_to_plot]
       ci = model.conf_int().loc[vars_to_plot]
       fig, ax = plt.subplots(figsize=(8, len(vars_to_plot) * 0.6 + 1))
       y_pos = range(len(vars_to_plot))
       ax.errorbar(coefs, y_pos, xerr=[coefs - ci[0], ci[1] - coefs],
                   fmt="o", color=COLORS[0], capsize=4, linewidth=1.5)
       ax.axvline(0, color="gray", linestyle="--", linewidth=0.8)
       ax.set_yticks(y_pos)
       ax.set_yticklabels(labels or vars_to_plot)
       ax.set_xlabel("Coefficient estimate (95% CI)")
       ax.set_title(title)
       return fig
   ```

   **Event study plot** (for DiD):
   ```python
   def event_study_plot(leads_lags_df, title=""):
       """leads_lags_df: columns = [period, coef, ci_lower, ci_upper]"""
       fig, ax = plt.subplots(figsize=(10, 6))
       ax.fill_between(leads_lags_df["period"],
                       leads_lags_df["ci_lower"],
                       leads_lags_df["ci_upper"],
                       alpha=0.2, color=COLORS[0])
       ax.plot(leads_lags_df["period"], leads_lags_df["coef"],
               "o-", color=COLORS[0], linewidth=1.5, markersize=5)
       ax.axhline(0, color="gray", linestyle="--", linewidth=0.8)
       ax.axvline(-0.5, color="red", linestyle=":", linewidth=0.8,
                  label="Treatment onset")
       ax.set_xlabel("Periods relative to treatment")
       ax.set_ylabel("Estimated effect")
       ax.set_title(title)
       ax.legend()
       return fig
   ```

   **Binscatter** (for relationships):
   ```python
   def binscatter(df, x, y, n_bins=20, controls=None, title=""):
       """Binned scatterplot, optionally residualized on controls."""
       if controls:
           from statsmodels.formula.api import ols
           resid_x = ols(f"{x} ~ {' + '.join(controls)}", df).fit().resid
           resid_y = ols(f"{y} ~ {' + '.join(controls)}", df).fit().resid
       else:
           resid_x, resid_y = df[x], df[y]
       bins = pd.qcut(resid_x, n_bins, duplicates="drop")
       binned = pd.DataFrame({"x": resid_x, "y": resid_y, "bin": bins})
       means = binned.groupby("bin").mean()
       fig, ax = plt.subplots()
       ax.scatter(means["x"], means["y"], s=40, color=COLORS[0])
       ax.set_xlabel(x + (" (residualized)" if controls else ""))
       ax.set_ylabel(y + (" (residualized)" if controls else ""))
       ax.set_title(title)
       return fig
   ```

3. **For interactive figures** (Plotly/Streamlit):
   ```python
   import plotly.express as px
   import plotly.graph_objects as go

   # Interactive time series
   fig = px.line(df, x="date", y=["gdp_growth", "inflation"],
                 title="GDP Growth vs Inflation",
                 labels={"value": "Percent", "variable": "Series"})
   fig.write_html("output/figures/macro_timeseries.html")
   fig.write_image("output/figures/macro_timeseries.pdf")  # for book
   ```

4. **Export rules**:
   - Save BOTH vector (PDF/SVG) and raster (PNG 300dpi) formats
   - Vector for book/paper, raster for web/slides
   - File naming: `fig_[chapter]_[number]_[description].pdf`
   - Every figure must have a self-contained caption (set via title or separate caption file)

5. **Save**: `./projects/[project]/code/fig_[name].py`
   Output: `./projects/[project]/output/figures/`
