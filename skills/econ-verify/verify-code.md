### /verify-code — Focused verification of analysis code

**Trigger**: "check my code", "verify this script", "does this code do what I think it does"

**Workflow**:

1. Read the specified Python script
2. Check against 8 code-verification criteria:

   | Check | What to look for |
   |-------|-----------------|
   | **Specification match** | Does the code implement the regression/model described in the manuscript? Same variables, same sample, same clustering? |
   | **Data integrity** | Are there unexpected drops? Is the merge correct (check for duplicates, unexpected NaN)? |
   | **Standard errors** | Are SEs clustered at the right level? Robust? Never default homoskedastic? |
   | **Random seeds** | Every `np.random`, `sklearn`, `train_test_split` must have a seed set |
   | **Hardcoded values** | Flag any magic numbers without explanation. All thresholds should be named constants |
   | **Output paths** | All relative, not absolute. Output goes to `./output/`, not `/Users/richeng/...` |
   | **Package versions** | Are imports available in requirements.txt? Any deprecated functions? |
   | **Edge cases** | What happens with empty dataframes? Division by zero? Single-observation groups? |

3. Output a code review report with line-specific issues:

```markdown
### Code Verification: [script_name.py]
**Lines of code**: [N]
**Issues found**: [N]

| Line | Issue | Severity | Fix |
|------|-------|----------|-----|
| 45 | `model.fit()` uses default SEs (homoskedastic) — should be `cov_type="cluster"` | CRITICAL | Add `cov_type="cluster", cov_kwds={"groups": df["state"]}` |
| 72 | `np.random.randn(1000)` — no seed set | Important | Use `rng = np.random.default_rng(42)` |
| 103 | Merge on "id" without checking duplicates — could inflate N silently | Important | Add `assert df.merge(...).shape[0] == expected_n` |
```

4. Save to: `./projects/[project]/reviews/code-review-[script]-[date].md`
