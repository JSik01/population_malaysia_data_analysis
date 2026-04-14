# Population in Malaysia Data Analysis

## Overview
This project is a personal project to analyze population pattern in Malaysia across state, age groups, gender and year using Python libraries including pandas, numpy, matplotlib and seaborn.

## Features
1. Filtering population data by state, year, ethnicity, gender, and age groups
    - For testing purpose, the analysis is first conducted on Johor
    - The filter is handled in function
    For example, for ethnicity,

    ```python
    ethnicity_group_data = df[(df['ethnicity_group'] == ethnicity_group) & (df['sex'] == 'both')].copy()
    ```

2. Handling boundary changes for Selangor and Sabah for most parts except population proportion and growth rate

```python
BOUNDARY_CHANGES = {
    "Selangor": [2010],
    "Sabah": [1991]
}
```

3. Remapping age groups into young, working, and old groups

```python
AGE_MAP = {
    "0-4": "young",
    "5-9": "young",
    "10-14": "young",
    "15-19": "working",
    "20-24": "working",
    "25-29": "working",
    "30-34": "working",
    "35-39": "working",
    "40-44": "working",
    "45-49": "working",
    "50-54": "working",
    "55-59": "working",
    "60-64": "working",
    "65-69": "old",
    "70-74": "old",
    "75-79": "old",
    "80+": "old"
}
```

4. Remapping ethnicity into Bumiputera, Chinese, Indian, and Other

```python
ETHNICITY_MAP = {
    'bumi' : 'Bumiputera',
    'bumi_malay': 'Bumiputera',
    'bumi_other': 'Bumiputera',
    'chinese': 'Chinese',
    'indian': 'Indian',
    'other': 'Other',
    'other_citizen': 'Other',
    'other_noncitizen': 'Other'
}
```

5. Unstacking data for multi-line plots
For example, ethnicity proportion.

```python
df_norm = df_norm.groupby('year')[[col for col in df_norm.columns]].apply(norm_total)
df_norm = df_norm.reset_index(drop=True)
df_unstacked = df_norm.set_index(["year", "ethnicity_group"])["normed_population"].unstack()
```

6. Allowing plotting by proportion via the `norm_total` function

```python
def norm_total(df):
    df["normed_population"] = df["population"] / df["population"].sum()
    return df
```

7. Allowing plotting by percentage change via `pandas.DataFrame.pct_change`

```python
malaysia_ethnicity_data_pt = malaysia_ethnicity_data_pt.pct_change()
```

8. Visualising population proportion and trends via line plots, bar plots and pie chart

9. Compared Malaysia population data using Kalman filter (local linear trend) and exponential smoothing (ETS (A,A,N))

# Time series summary
1. Kalman filter, being a stochastic model, captured structural break in 2020, thus producing a more realistic forecast
2. ETS forecasts a higher population than Kalman filter
3. Kalman filter residuals show no autocorrelation (Ljung-Box p-value of close to 1) and stable variance
4. There is a minor disturbance in 2010, possibly a small irregular shock or revised methodology

## Data source 
The data comes from <a href="https://open.dosm.gov.my/data-catalogue/population_state">Population Table: States (Department of Statistics Malaysia, 2025)</a>.

## Initial check
The initial data check is available in the *population_initial_data_check* file

## Limitations as noted in data source except the last one (The last is from observation after plotting population growth)
- Ethnicity data is available from 1980 onwards
- There are spikes due to intercensal adjustments (1991 and years divisible by 10 except 1990), govenrnment policies and COVID-19 pandemic (2020, 2021)
- This project does not consider migration, socioeconomic factors, social expectations, and other factors which might explain:
    - Growth rate for others fluctuates widely 
    - Population growth rate decreases if spikes are removed

## Closing note
After five months of development including delays due to personal reasons, the author is pleased to conclude the analysis of data with the addition of time series analysis extension. The full analysis is available in accompanying Jupyter notebooks for reference or future analysis. Annual minor update might be make to account for updated population data; otherwise please refer to the link in the data source section for the latest figures. 
The author hopes that this project will be useful for anyone intending to explore Malaysia demographic or data analysis methods.