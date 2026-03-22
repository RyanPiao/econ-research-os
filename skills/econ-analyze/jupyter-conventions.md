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