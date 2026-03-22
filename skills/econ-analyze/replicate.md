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