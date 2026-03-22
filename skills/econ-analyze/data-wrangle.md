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