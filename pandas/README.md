# Pandas Learning

A practical record of Pandas fundamentals learned using the READ → TRY → FAIL → LEARN → TRY AGAIN method.

## Topics Learned

### 1. What is Pandas?
- Python library for structured/tabular data.
- Think: Excel tables + SQL operations + Python.
- Used for data analysis, cleaning, ML preprocessing, CSV/Excel work, and SQL + Python workflows.

### 2. Series
```python
import pandas as pd
no = pd.Series([1, 2, 3, 4, 5])
print(no[2])
marks = pd.Series([90, 85, 88], index=["python", "sql", "ml"])
print(marks["ml"])
```
- One-dimensional labeled data.
- Default index starts at 0; custom labels can be provided.

### 3. DataFrame
```python
data = {"Name": ["Ashwin", "Rahul", "Priya"], "Age": [23, 25, 22], "City": ["Pune", "Mumbai", "Delhi"]}
df = pd.DataFrame(data)
```
- Two-dimensional table with rows, columns, index, and values.

### 4. Selecting columns
```python
df["Name"]
df[["Name", "Age"]]
```
- One column → Series; multiple columns → DataFrame.

### 5. Selecting rows: `loc` and `iloc`
```python
df.iloc[0]
df.iloc[1]
df.loc[1]
df.index = ["A", "B", "C"]
df.loc["B"]
```
- `loc` → label; `iloc` → integer position.

### 6. Inspection
```python
df.head()
df.head(2)
df.tail()
df.tail(2)
df.shape
df.shape[0]
df.shape[1]
df.columns
df.dtypes
df.info()
df.describe()
```
- `head()` / `tail()` → first/last rows.
- `shape` → `(rows, columns)`; `shape[0]` → rows; `shape[1]` → columns.
- `columns` → column names; `dtypes` → data types; `info()` → structure; `describe()` → numeric summary.
- Methods use `()`; attributes such as `shape`, `columns`, `dtypes` do not.

### 7. Filtering
```python
df[df["Age"] > 23]
df[df["Age"] >= 23]
df[df["Age"] < 24]
df[df["City"] == "Pune"]
df[(df["Age"] >= 23) & (df["City"] == "Pune")]
df[(df["City"] == "Pune") | (df["City"] == "Mumbai")]
df[df["City"].isin(["Pune", "Mumbai", "Delhi"])]
```
- `&` → AND; `|` → OR; use parentheses around individual conditions.

### 8. Sorting
```python
df.sort_values("Age")
df.sort_values("Age", ascending=False)
df.sort_values("Age", ascending=True)
```
- Default is ascending order.

### 9. Adding and updating columns
```python
df["Salary"] = [50000, 60000, 45000]
df["Age"] = df["Age"] + 1
df["Salary"] = df["Salary"] * 1.10
df["Age_After_5_Years"] = df["Age"] + 5
df["Double_Age"] = df["Age"] * 2
df["Salary_After_10_Percent"] = df["Salary"] * 1.10
```
Pattern: `df["NEW_COLUMN"] = df["OLD_COLUMN"] + calculation`.

### 10. Percentage calculation
For an increase of X%: multiplier = `1 + X/100`. For a decrease: multiplier = `1 - X/100`.

### 11. Removing columns
```python
df = df.drop("City", axis=1)
df = df.drop(["Age", "City"], axis=1)
```
- `axis=1` → columns; `axis=0` → rows.

### 12. Renaming columns
```python
df.rename(columns={"Age": "Employees_Age"})
df = df.rename(columns={"Age": "Employees_Age"})
df = df.rename(columns={"Name": "Employee_Name", "City": "Location"})
```
- Assignment is needed to keep the change permanently when not using `inplace=True`.

### 13. Missing values
```python
df.isnull()
df.isnull().sum()
df.isnull().sum().sum()
df["Age"].fillna(25)
df["Age"] = df["Age"].fillna(25)
df["Salary"] = df["Salary"].fillna(0)
df.dropna()
df = df.dropna()
df.dropna(axis=1)
```
- `isnull()` → locate missing values; `fillna()` → replace; `dropna()` → remove rows/columns containing missing values.

### 14. Duplicates
```python
df.duplicated()
df.drop_duplicates()
df = df.drop_duplicates(subset="Name")
df = df.drop_duplicates(subset="Name", keep="last")
df = df.drop_duplicates(subset="Name", keep=False)
```
- `duplicated()` → find; `drop_duplicates()` → remove. Default `keep="first"` keeps the first occurrence.

### 15. Data type conversion
```python
df["Age"] = df["Age"].astype(int)
df["Salary"] = df["Salary"].astype(float)
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")
```
- `astype()` → specified type; `pd.to_numeric(..., errors="coerce")` → invalid values become `NaN`.

### 16. String operations
```python
df["Name"] = df["Name"].str.upper()
df["Name"] = df["Name"].str.lower()
df["Name"] = df["Name"].str.strip()
df["First_Name"] = df["Name"].str.split().str[0]
df["Last_Name"] = df["Name"].str.split().str[1]
```
- `.str` gives access to string operations.

### 17. `apply()`
```python
df["Salary"] = df["Salary"].apply(lambda x: x * 1.10)
df["Age"] = df["Age"].apply(lambda x: x + 5)
df["Age_Double"] = df["Age"].apply(lambda x: x * 2)
```
- Applies a function to each value.

### 18. `map()`
```python
df["Score"] = df["Grade"].map({"A": 90, "B": 80, "C": 70})
df["State"] = df["City"].map({"Pune": "Maharashtra", "Mumbai": "Maharashtra", "Delhi": "Delhi"})
```
- Think: `this value → that value`.

### 19. `lambda`
```python
def double(x):
    return x * 2

double = lambda x: x * 2
add_five = lambda x: x + 5
cube = lambda x: x ** 3
```
- Small one-line anonymous function; common with `apply()`, `map()`, and sorting.

### 20. GroupBy
```python
df.groupby("Department")["Salary"].sum()
df.groupby("Department")["Salary"].mean()
df.groupby("Department")["Salary"].max()
```
Common operations: `.sum()`, `.mean()`, `.count()`, `.min()`, `.max()`.

### 21. Multiple aggregations with `agg()`
```python
df.groupby("Department")["Salary"].agg(["sum", "mean", "max", "min"])
df.groupby("Department").agg({"Salary": "sum", "Age": "mean"})
```
- `.agg()` → multiple/different calculations in one operation.

### 22. `merge()`
```python
pd.merge(df1, df2, on="ID")
pd.merge(df1, df2, on="ID", how="left")
```
- `inner` → matching only; `left` → everything from left; `right` → everything from right; `outer` → everything from both.
- SQL connection: inner → INNER JOIN, left → LEFT JOIN, right → RIGHT JOIN, outer → FULL OUTER JOIN.

### 23. `concat()`
```python
pd.concat([df1, df2])
pd.concat([df1, df2], axis=0)
pd.concat([df1, df2], ignore_index=True)
pd.concat([df1, df2], axis=1)
```
- `axis=0` → rows; `axis=1` → columns; `ignore_index=True` → fresh continuous index.
- `merge()` uses a common key; `concat()` combines/stacks DataFrames.

### 24. Dates & Time
```python
df["Order_Date"] = pd.to_datetime(df["Order_Date"])
df["Year"] = pd.to_datetime(df["Order_Date"]).dt.year
df["Month"] = pd.to_datetime(df["Order_Date"]).dt.month
df["Day"] = pd.to_datetime(df["Order_Date"]).dt.day
df["Day_Name"] = pd.to_datetime(df["Order_Date"]).dt.day_name()
df["Month_Name"] = pd.to_datetime(df["Order_Date"]).dt.month_name()
df["Days"] = (df["End_Date"] - df["Start_Date"]).dt.days
```
- `pd.to_datetime()` converts dates; `.dt` accesses date components.

### 25. `pivot_table()`
```python
pd.pivot_table(df, values="Salary", index="Department", aggfunc="mean")
pd.pivot_table(df, values="Salary", index="Department", aggfunc="sum")
pd.pivot_table(df, values="Salary", index="Department", aggfunc=["sum", "mean", "max"])
```
- `values` → WHAT; `index` → GROUP BY what; `aggfunc` → HOW.

### 26. Exporting Data
```python
df.to_csv("employees.csv", index=False)
df.to_excel("employees.xlsx", index=False)
df.to_json("employees.json")
```
- `to_csv()` → CSV; `to_excel()` → Excel; `to_json()` → JSON.
- `index=False` prevents the DataFrame index from being saved for CSV/Excel exports.

## Learning Method
READ → TRY → FAIL → LEARN → TRY AGAIN

Every practice attempt is reviewed for syntax, indentation, capitalization, variable/column names, Pandas usage, logic, output, and Python conventions.
