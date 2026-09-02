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

### 3. DataFrame
```python
data = {
    "Name": ["Ashwin", "Rahul", "Priya"],
    "Age": [23, 25, 22],
    "City": ["Pune", "Mumbai", "Delhi"]
}

df = pd.DataFrame(data)
```

### 4. Selecting columns
```python
df["Name"]
df[["Name", "Age"]]
```
- One column → Series
- Multiple columns → DataFrame

### 5. Selecting rows
```python
df.iloc[0]       # first row by position
df.iloc[1]       # second row by position
df.loc[1]        # row by label when index labels are 0,1,2
```
If the index is custom:
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
Important distinction:
- Methods: `head()`, `tail()`, `info()`, `describe()`
- Attributes: `shape`, `columns`, `dtypes`

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

### 8. Sorting
```python
df.sort_values("Age")
df.sort_values("Age", ascending=False)
df.sort_values("Age", ascending=True)
```

### 9. Adding and updating columns
```python
df["Salary"] = [50000, 60000, 45000]
df["Age"] = df["Age"] + 1
df["Salary"] = df["Salary"] * 1.10
```

Creating a new column does not require a special `add()` function:
```python
df["Age_Next_Year"] = df["Age"] + 1
df["Age_After_5_Years"] = df["Age"] + 5
df["Double_Age"] = df["Age"] * 2
df["Salary_After_10_Percent"] = df["Salary"] * 1.10
```
Pattern:
```python
df["NEW_COLUMN"] = df["OLD_COLUMN"] + calculation
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
One column:
```python
df = df.drop("City", axis=1)
```
Multiple columns:
```python
df = df.drop(["Age", "City"], axis=1)
```
- `axis=1` → column
- `axis=0` → row

### 12. Renaming columns
```python
df.rename(columns={"Age": "Employees_Age"})
```
Permanent change:
```python
df = df.rename(columns={"Age": "Employees_Age"})
```
Multiple columns:
```python
df = df.rename(columns={"Name": "Employee_Name", "City": "Location"})
```

## Learning Method
READ → TRY → FAIL → LEARN → TRY AGAIN

Every practice attempt is reviewed for syntax, indentation, capitalization, variable/column names, Pandas usage, logic, output, and Python conventions.
