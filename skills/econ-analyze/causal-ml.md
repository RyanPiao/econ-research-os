### /causal-ml — Run causal machine learning methods

**Trigger**: "run double ML", "causal forest", "LASSO for variable selection", "heterogeneous treatment effects"

**Workflow**:

1. **Method selection guide**:
   | Method | Package | Use when |
   |---|---|---|
   | Double/Debiased ML | `doubleml` | High-dimensional controls, want ATE/ATTE |
   | Causal Forest | `econml.dml.CausalForestDML` | Want CATE (heterogeneous effects) |
   | LASSO first stage | `sklearn.linear_model.LassoCV` | Variable selection for IV/controls |
   | Meta-learners | `econml.metalearners` | Simple CATE with any base learner |
   | DML + IV | `doubleml.DoubleMLIIVM` | IV setting with many instruments |

2. **Double ML example**:
   ```python
   """
   Causal ML: Double/Debiased Machine Learning
   Method: Chernozhukov et al. (2018)
   """
   import doubleml as dml
   from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier

   # Setup
   data = dml.DoubleMLData(
       df, y_col="Y", d_cols="D",
       x_cols=["X1", "X2", "X3", "X4", "X5"]  # high-dim controls
   )

   # ML learners for nuisance parameters
   ml_l = RandomForestRegressor(n_estimators=500, max_depth=5)  # E[Y|X]
   ml_m = RandomForestClassifier(n_estimators=500, max_depth=5)  # E[D|X]

   # Estimate
   dml_model = dml.DoubleMLPLR(data, ml_l, ml_m, n_folds=5)
   dml_model.fit()
   print(dml_model.summary)

   # Compare with OLS
   ols = smf.ols("Y ~ D + X1 + X2 + X3 + X4 + X5", data=df).fit()
   print(f"OLS: {ols.params['D']:.4f} ({ols.bse['D']:.4f})")
   print(f"DML: {dml_model.coef[0]:.4f} ({dml_model.se[0]:.4f})")
   ```

3. **Causal forest for heterogeneous effects**:
   ```python
   from econml.dml import CausalForestDML

   cf = CausalForestDML(
       model_y=RandomForestRegressor(n_estimators=200),
       model_t=RandomForestClassifier(n_estimators=200),
       n_estimators=1000,
       random_state=42
   )
   cf.fit(Y=df["Y"], T=df["D"],
          X=df[["X1", "X2"]],       # effect modifiers
          W=df[["X3", "X4", "X5"]])  # confounders only

   # Individual treatment effects
   cate = cf.effect(df[["X1", "X2"]])

   # Visualize heterogeneity
   fig, ax = plt.subplots()
   ax.scatter(df["X1"], cate, alpha=0.3, s=10)
   ax.set_xlabel("X1 (effect modifier)")
   ax.set_ylabel("Estimated CATE")
   ax.set_title("Treatment effect heterogeneity")
   ax.axhline(cate.mean(), color="red", linestyle="--",
              label=f"ATE = {cate.mean():.3f}")
   ax.legend()
   ```

4. **Save**: `./projects/[project]/code/causal_ml_[name].py`
