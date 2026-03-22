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