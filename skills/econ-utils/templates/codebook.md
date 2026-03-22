# Data Codebook: [Dataset Name]

**Source**: [Where the data comes from]
**Downloaded**: [YYYY-MM-DD]
**File**: [filename.csv / .dta / .xlsx]
**Observations**: [N rows]
**Time coverage**: [Start — End]
**Geographic coverage**: [Country / State / County / etc.]

## Variables

| Variable | Description | Type | Unit | Source | Notes |
|----------|------------|------|------|--------|-------|
| `id` | Unique identifier | int | — | — | Primary key |
| `year` | Year of observation | int | year | — | |
| `state` | US state FIPS code | int | — | Census | |
| `wage` | Average hourly wage | float | 2020 USD | BLS/CPS | Deflated using CPI-U |
| `emp_rate` | Employment-population ratio | float | proportion | BLS | Ages 16-64 |
| `min_wage` | State minimum wage | float | nominal USD | DOL | Effective Jan 1 of each year |
| `treat` | Post-treatment indicator | binary | 0/1 | Constructed | = 1 if year ≥ treatment_year |

## Sample restrictions
- [Restriction 1: e.g., "Dropped observations with missing wage data (N = 234)"]
- [Restriction 2: e.g., "Limited to ages 16-64"]
- [Restriction 3: e.g., "Excluded Alaska and Hawaii"]

## Known issues
- [Issue 1: e.g., "CPS redesign in 2014 creates a break in series"]
- [Issue 2: e.g., "State minimum wage data inconsistent for tipped workers"]

## Cleaning steps applied
1. [Step 1: what was done and why]
2. [Step 2]
3. [Step 3]
