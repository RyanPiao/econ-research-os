### /dashboard — Build interactive Streamlit/Plotly dashboards

**Trigger**: "build a dashboard for [topic]", "create an interactive app", "Streamlit app for [data]"

**Workflow**:

1. **Generate Streamlit app scaffold**:
   ```python
   """
   Dashboard: [Description]
   Data: [Source]
   Run: streamlit run app.py
   """
   import streamlit as st
   import pandas as pd
   import plotly.express as px
   from fredapi import Fred

   st.set_page_config(page_title="[Title]", layout="wide")
   st.title("[Dashboard Title]")

   # --- Sidebar controls ---
   st.sidebar.header("Parameters")
   start_year = st.sidebar.slider("Start year", 1990, 2025, 2000)
   series = st.sidebar.multiselect(
       "FRED Series",
       ["GDP", "UNRATE", "CPIAUCSL", "FEDFUNDS"],
       default=["GDP", "UNRATE"]
   )

   # --- Data loading (cached) ---
   @st.cache_data(ttl=3600)
   def load_fred(series_list, start):
       fred = Fred(api_key=st.secrets["FRED_API_KEY"])
       data = {}
       for s in series_list:
           data[s] = fred.get_series(s, start=f"{start}-01-01")
       return pd.DataFrame(data)

   df = load_fred(series, start_year)

   # --- Visualization ---
   col1, col2 = st.columns(2)
   with col1:
       fig = px.line(df, title="Time Series")
       st.plotly_chart(fig, use_container_width=True)
   with col2:
       st.dataframe(df.describe())

   # --- Analysis section ---
   st.subheader("Correlation Analysis")
   if len(series) >= 2:
       corr = df.corr()
       fig_corr = px.imshow(corr, text_auto=".2f",
                            title="Correlation Matrix")
       st.plotly_chart(fig_corr)
   ```

2. **Save**: `./projects/[project]/apps/[app-name]/app.py`