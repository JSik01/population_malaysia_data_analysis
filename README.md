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

2. Handling boundary changes for Selangor and Sabah for most parts except population proportion

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

7. Visualising population proportion and trends via line plots, bar plots and pie chart

## Data source 
The data comes from <a href="https://open.dosm.gov.my/data-catalogue/population_state">Population Table: States (Department of Statistics Malaysia, 2025)</a>.

## Initial check
The initial data check is available in the *population_initial_data_check* file

## Future extensions
- Age pyramid
- Growth rate 
- Time series prediction for Malaysia

## Note 
- All code files are in Jupyter notebook format
- Ethnicity data is available from 1980 onwards
