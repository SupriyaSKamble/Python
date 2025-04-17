
# 📊Data Analysis in Python

---

## 📁 Dataset: GDP (nominal) per Capita

This dataset includes GDP per capita estimates by:
- IMF (International Monetary Fund)
- UN (United Nations)
- World Bank

The data was analyzed using **Pandas**, **NumPy**, **Matplotlib**, and **Seaborn**.

---

## ✅ Day 4: Task 1 - Basic Exploration

- **Loaded data** into DataFrame:
```python
df = pd.read_excel('GDP (nominal) per Capita.xlsx')
```

- **Displayed rows**:
```python
df.head(10)  # First 10 rows
df.tail(5)   # Last 5 rows
```

- **Selected columns**:
```python
df[['Country/Territory', 'UN_Region']]
```

---

## 📈 Day 4: Task 2 - Group and Analyze

### 🌍 Countries Per Region
```python
df.UN_Region.value_counts()
```

### 🇪🇺 Special Case: European Union
```python
df[df["Country/Territory"] == "European Union[n 1]"]
```

### 📉 European Countries Below Global Average (World Bank)
```python
global_avg = df['WorldBank_Estimate'].mean()
europe_below_avg = df[
    (df['UN_Region'].str.contains('Europe')) & 
    (df['WorldBank_Estimate'] < global_avg)
]
```

### 📈 Countries in Europe with GDP Above UK
```python
UK_GDP = df[df["Country/Territory"] == "United Kingdom"]["WorldBank_Estimate"].values[0]
df[(df["UN_Region"].str.contains("Europe")) & (df["WorldBank_Estimate"] > UK_GDP)]
```

---

## 📊 GroupBy Analysis

- **Average IMF estimate per region**:
```python
df.groupby("UN_Region")["UN_Estimate"].mean()
```

- **Total GDP by region**:
```python
df.groupby("UN_Region")["UN_Estimate"].sum()
```

---

## 🛠 IMF Estimate Data Cleaning

- **Replace 0s with NaN**:
```python
df["IMF_Estimate"].replace(0, np.nan)
```

- **Fill NaNs with avg of World Bank and UN**:
```python
df["IMF_Estimate"].fillna(
    df[["WorldBank_Estimate", "UN_Estimate"]].mean(axis=1)
)
```

---

## 🧪 Insights and Highlights

- **Highest GDP per Capita (UN)**: `df[df['UN_Estimate'] == df['UN_Estimate'].max()]`
- **Highest GDP per Capita (World Bank)**: `df[df['WorldBank_Estimate'] == df['WorldBank_Estimate'].max()]`
- **Highest GDP per Capita (IMF)**: `df[df['IMF_Estimate'] == df['IMF_Estimate'].max()]`

- **Countries below average (IMF)**:
```python
avg_imf = df["IMF_Estimate"].mean()
df[df["IMF_Estimate"] < avg_imf]
```

---

## 📊 Visualizations

- Histograms
  ![image](https://github.com/user-attachments/assets/ddbdb5db-0902-4afd-b54c-76988e4f0941)

- Correlation heatmaps (strong agreement across sources)
  ![image](https://github.com/user-attachments/assets/83542f1b-249f-4588-8d2e-8b6d519aeed4)

- Barplots by region
  ![image](https://github.com/user-attachments/assets/1a4cce83-a9fc-4d3b-b22d-6a218deaa5bd)

- Scatter plots
  ![image](https://github.com/user-attachments/assets/139cb5d0-7e1c-420d-8430-f55fde1f32eb)

- Boxplots for detecting outliers
  ![image](https://github.com/user-attachments/assets/17e1accc-0f8e-4a4c-ae7e-f346e8f05c08)

---

## 🔍 Outlier Handling

- Removed top 5 outliers by UN Estimate
  
- Applied IQR method to filter extreme values

  lower_q = df["UN_Estimate"].quantile(0.25)
  higher_q = df["UN_Estimate"].quantile(0.75)
  iqr = higher_q - lower_q
  upper_boundary = higher_q + 1.5 * iqr
  lower_boundary = lower_q - 1.5 * iqr
  df_filtered = df[(df["UN_Estimate"] < upper_boundary) & (df["UN_Estimate"] > lower_boundary)] df_filtered.head()
  ![image](https://github.com/user-attachments/assets/77b3635a-1dc6-4fda-ac2e-9f1cb4aee687)

---

**End of Summary**

