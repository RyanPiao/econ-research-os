### /simulate — Monte Carlo simulations and power analysis

**Trigger**: "simulate [method]", "Monte Carlo for [estimator]", "power analysis for [design]", "show what happens when [assumption] fails"

**Workflow**:

1. **Define the data generating process (DGP)**:
   ```python
   """
   Monte Carlo Simulation: [What we're testing]
   DGP: [Mathematical specification]
   Purpose: [Show bias/consistency/coverage of estimator X under condition Y]
   """
   import numpy as np
   import pandas as pd
   from joblib import Parallel, delayed
   from tqdm import tqdm

   def dgp(n=1000, beta_true=0.5, seed=None):
       """Generate one dataset from the DGP.

       Y_i = β₀ + β₁ * D_i + β₂ * X_i + ε_i
       where D_i is endogenous: D_i = γ * Z_i + η_i
       """
       rng = np.random.default_rng(seed)
       Z = rng.normal(0, 1, n)           # instrument
       eta = rng.normal(0, 1, n)          # unobserved
       epsilon = rng.normal(0, 1, n) + 0.5 * eta  # endogeneity
       X = rng.normal(0, 1, n)            # exogenous control
       D = 0.8 * Z + eta                  # treatment (endogenous)
       Y = 1.0 + beta_true * D + 0.3 * X + epsilon
       return pd.DataFrame({"Y": Y, "D": D, "X": X, "Z": Z})
   ```

2. **Run the simulation**:
   ```python
   def one_iteration(i, n=1000):
       """Run one MC iteration. Returns dict of estimates."""
       df = dgp(n=n, seed=i)

       # OLS (biased)
       ols = smf.ols("Y ~ D + X", data=df).fit()

       # IV/2SLS (consistent)
       from linearmodels.iv import IV2SLS
       iv = IV2SLS.from_formula("Y ~ 1 + X + [D ~ Z]", data=df).fit()

       return {
           "ols_beta": ols.params["D"],
           "ols_se": ols.bse["D"],
           "iv_beta": iv.params["D"],
           "iv_se": iv.std_errors["D"],
       }

   # Parallel execution
   N_SIMS = 2000
   results = Parallel(n_jobs=-1)(
       delayed(one_iteration)(i) for i in tqdm(range(N_SIMS))
   )
   sim_df = pd.DataFrame(results)
   ```

3. **Analyze and visualize results**:
   ```python
   # Bias
   print(f"OLS bias: {sim_df['ols_beta'].mean() - 0.5:.4f}")
   print(f"IV bias:  {sim_df['iv_beta'].mean() - 0.5:.4f}")

   # Coverage (does 95% CI contain true value?)
   sim_df["ols_covers"] = (
       (sim_df["ols_beta"] - 1.96 * sim_df["ols_se"] <= 0.5) &
       (sim_df["ols_beta"] + 1.96 * sim_df["ols_se"] >= 0.5)
   )
   print(f"OLS coverage: {sim_df['ols_covers'].mean():.3f}")

   # Distribution plot
   fig, axes = plt.subplots(1, 2, figsize=(12, 5))
   axes[0].hist(sim_df["ols_beta"], bins=50, alpha=0.7, label="OLS")
   axes[0].axvline(0.5, color="red", linestyle="--", label="True β")
   axes[1].hist(sim_df["iv_beta"], bins=50, alpha=0.7, label="IV")
   axes[1].axvline(0.5, color="red", linestyle="--", label="True β")
   ```

4. **For power analysis**:
   ```python
   def power_curve(effect_sizes, n_range, alpha=0.05, n_sims=1000):
       """Compute power for each (effect_size, n) combination."""
       results = []
       for es in effect_sizes:
           for n in n_range:
               rejections = 0
               for i in range(n_sims):
                   df = dgp(n=n, beta_true=es, seed=i)
                   model = smf.ols("Y ~ D + X", data=df).fit()
                   if model.pvalues["D"] < alpha:
                       rejections += 1
               results.append({"effect_size": es, "n": n,
                               "power": rejections / n_sims})
       return pd.DataFrame(results)
   ```

5. **Save**: `./projects/[project]/code/sim_[name].py`