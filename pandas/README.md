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
data = {
    "Name": ["Ashwin", "Rahul", "Priya"],
    "Age": [23, 25, 22],
    "City": ["Pune", "Mumbai", "Delhi"]
}

df = pd.DataFrame(data)
```
- Two-dimensional table with rows, columns, index, and values.

### 4. Selecting columns
```python
df["Name"]
df[["Name", "Age"]]
```
- One column → Series
- Multiple columns → DataFrame

### 5. Selecting rows: `loc` and `iloc`
```python
df.iloc[0]       # first row by position
df.iloc[1]       # second row by position
df.loc[1]        # row by label when index labels are 0,1,2
```
Custom index:
```python
df.index = ["A", "B", "C"]
df.loc["B"]
```
- `loc` → label
- `iloc` → integer position

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
- `head()` / `tail()` → first/last rows
- `shape` → `(rows, columns)`
- `shape[0]` → rows; `shape[1]` → columns
- `columns` → column names
- `dtypes` → data types
- `info()` → structural summary
- `describe()` → numeric summary
- Methods use `()`; attributes such as `shape`, `columns`, `dtypes` do not.

### 7. Filtering
```python
df[df["Age"] > 23]
df[df["Age"] >= 23]
df[df["Age"] < 24]
df[df["City"] == "Pune"]
```
Multiple conditions:
```python
df[(df["Age"] >= 23) & (df["City"] == "Pune")]
df[(df["City"] == "Pune") | (df["City"] == "Mumbai")]
```
Using `isin()`:
```python
df[df["City"].isin(["Pune", "Mumbai", "Delhi"])]
```
- `&` → AND
- `|` → OR
- Put parentheses around individual conditions.

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
```
New column pattern:
```python
df["NEW_COLUMN"] = df["OLD_COLUMN"] + calculation
```
Examples:
```python
df["Age_After_5_Years"] = df["Age"] + 5
df["Double_Age"] = df["Age"] * 2
df["Salary_After_10_Percent"] = df["Salary"] * 1.10
```

### 10. Percentage calculation
For an increase of X%:
- multiplier = `1 + X/100`
- 10% → `1.10`
- 20% → `1.20`
- 33% → `1.33`
- 50% → `1.50`
- 100% → `2.00`

For a decrease of X%:
- multiplier = `1 - X/100`
- 33% decrease → `0.67`

### 11. Removing columns
```python
df = df.drop("City", axis=1)
df = df.drop(["Age", "City"], axis=1)
```
- `axis=1` → columns
- `axis=0` → rows

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
- `isnull()` → locate missing values
- `isnull().sum()` → count missing values per column
- `isnull().sum().sum()` → total missing values
- `fillna()` → replace missing values
- `dropna()` → remove rows containing missing values
- `dropna(axis=1)` → remove columns containing missing values

### 14. Duplicates
```python
df.duplicated()
df.drop_duplicates()
df = df.drop_duplicates()
df = df.drop_duplicates(subset="Name")
df = df.drop_duplicates(subset="Name", keep="last")
df = df.drop_duplicates(subset="Name", keep=False)
```
- `duplicated()` → find duplicate rows
- `drop_duplicates()` → remove duplicates
- Default `keep="first"` keeps the first occurrence.

### 15. Data type conversion
```python
df["Age"] = df["Age"].astype(int)
df["Salary"] = df["Salary"].astype(float)
df["Age"] = df["Age"].astype(float)
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")
```
- `astype()` → convert to a specified type; can fail on invalid values.
- `pd.to_numeric(..., errors="coerce")` → invalid values become `NaN`.
- Check types with `df.dtypes` or `df.info()`.

### 16. String operations
```python
df["Name"] = df["Name"].str.upper()
df["Name"] = df["Name"].str.lower()
df["Name"] = df["Name"].str.strip()
df["First_Name"] = df["Name"].str.split().str[0]
df["Last_Name"] = df["Name"].str.split().str[1]
```
- `.str` gives access to string operations.
- `upper()` / `lower()` → change case
- `strip()` → remove leading/trailing spaces
- `split()` → split text into parts
- `.str[0]`, `.str[1]` → select split parts

### 17. `apply()`
```python
df["Salary"] = df["Salary"].apply(lambda x: x * 1.10)
df["Age"] = df["Age"].apply(lambda x: x + 5)
df["Age_Double"] = df["Age"].apply(lambda x: x * 2)
```
Pattern:
```python
df["New_Column"] = df["Column"].apply(lambda x: calculation)
```
- Applies a function to each value.

### 18. `map()`
```python
df["Score"] = df["Grade"].map({"A": 90, "B": 80, "C": 70})
df["State"] = df["City"].map({"Pune": "Maharashtra", "Mumbai": "Maharashtra", "Delhi": "Delhi"})
```
- `map()` is useful when known values should map to other known values.
- Think: `this value → that value`.

### 19. `lambda`
Normal function:
```python
def double(x):
    return x * 2
```
Lambda:
```python
double = lambda x: x * 2
add_five = lambda x: x + 5
cube = lambda x: x ** 3
```
- Small one-line anonymous function.
- Commonly used with `apply()`, `map()`, and sorting.
- Pattern: `lambda input: calculation`

### 20. GroupBy
Example:
```python
df.groupby("Department")["Salary"].sum()
df.groupby("Department")["Salary"].mean()
df.groupby("Department")["Salary"].max()
```
Common operations:
```python
.sum()
.mean()
.count()
.min()
.max()
```
Pattern:
```python
df.groupby("GROUP_COLUMN")["VALUE_COLUMN"].operation()
```
- `groupby()` → puts similar rows into groups, then lets us calculate something for each group.

### 21. Multiple aggregations with `agg()`
Same column:
```python
df.groupby("Department")["Salary"].agg(["sum", "mean", "max", "min"])
```
Different columns:
```python
df.groupby("Department").agg({
    "Salary": "sum",
    "Age": "mean"
})
```
- `.agg()` → perform multiple/different calculations in one operation.

### 22. `merge()`
Basic:
```python
pd.merge(df1, df2, on="ID")
```
With join type:
```python
pd.merge(df1, df2, on="ID", how="left")
```
Common joins:
```python
how="inner"  # matching rows only
how="left"   # everything from left
how="right"  # everything from right
how="outer"  # everything from both
```
SQL connection:
- `inner` → INNER JOIN
- `left` → LEFT JOIN
- `right` → RIGHT JOIN
- `outer` → FULL OUTER JOIN

### 23. `concat()`
Stack rows:
```python
pd.concat([df1, df2])
pd.concat([df1, df2], axis=0)
pd.concat([df1, df2], ignore_index=True)
```
Stack columns:
```python
pd.concat([df1, df2], axis=1)
```
- `axis=0` → rows
- `axis=1` → columns
- `ignore_index=True` → creates a fresh continuous index
- `merge()` uses a common key; `concat()` combines/stack DataFrames.

### 24. Dates & Time
Convert:
```python
df["Order_Date"] = pd.to_datetime(df["Order_Date"])
```
Extract date parts:
```python
df["Year"] = pd.to_datetime(df["Order_Date"]).dt.year
df["Month"] = pd.to_datetime(df["Order_Date"]).dt.month
df["Day"] = pd.to_datetime(df["Order_Date"]).dt.day
df["Day_Name"] = pd.to_datetime(df["Order_Date"]).dt.day_name()
df["Month_Name"] = pd.to_datetime(df["Order_Date"]).dt.month_name()
```
Date difference:
```python
df["Days"] = df["End_Date"] - df["Start_Date"]
```
Integer number of days:
```python
df["Days"] = (df["End_Date"] - df["Start_Date"]).dt.days
```
- `pd.to_datetime()` makes dates datetime values.
- `.dt` gives access to date components and methods.

### 25. `pivot_table()`
Basic:
```python
pd.pivot_table(
    df,
    values="Salary",
    index="Department",
    aggfunc="mean"
)
```
Total:
```python
pd.pivot_table(
    df,
    values="Salary",
    index="Department",
    aggfunc="sum"
)
```
Multiple calculations:
```python
pd.pivot_table(
    df,
    values="Salary",
    index="Department",
    aggfunc=["sum", "mean", "max"]
)
```
Key idea:
- `values` → WHAT are we calculating?
- `index` → GROUP BY what?
- `aggfunc` → HOW do we calculate?

## Learning Method
READ → TRY → FAIL → LEARN → TRY AGAIN

Every practice attempt is reviewed for syntax, indentation, capitalization, variable/column names, Pandas usage, logic, output, and Python conventions.
