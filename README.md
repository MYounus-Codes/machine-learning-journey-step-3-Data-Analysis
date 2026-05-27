# ML Step 3 — Data Analysis with Pandas 🐼

> Hands-on pandas examples for data loading, exploration, filtering, and cleaning.
> Interactive Colab mirror → [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ete8uYibOi9axG4C357OQts94EzwUa7A?usp=sharing)

---

## Contents

| File | Description |
|------|-------------|
| `learning_pandas.ipynb` | Core examples — Series, DataFrame, filtering, CSV import, and data cleaning |
| `basics.ipynb` | Short supplementary snippets |

---

## Prerequisites

- **Python** ≥ 3.12
- **Dependencies:** `pandas`

```powershell
# Activate your virtual environment (Windows PowerShell)
& .\.venv\Scripts\Activate.ps1

# Install dependencies
python -m pip install --upgrade pip
pip install pandas
```

---

## API Reference

### Imports

```python
import pandas as pd
```

---

### Series

#### Creating a Series
```python
s = pd.Series([100, 101, 102], index=['a', 'b', 'c'])
```

#### Selecting values
```python
s.loc['a']          # by label
s.iloc[0]           # by position
s.loc[['a', 'c']]   # multiple labels
```

#### Updating values
```python
s.iloc[1] = 110
s.loc['a'] = 99
```

#### Aggregation
```python
s.min()
s.max()
s.sum()
s.mean()
```

---

### DataFrame

#### Creating a DataFrame
```python
df = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Carol'],
    'Age':  [14, 15, 13],
    'Grade': ['A+', 'B', 'A+']
}, index=['Student 1', 'Student 2', 'Student 3'])
```

#### Selecting rows & columns
```python
df.loc['Student 1']     # row by label
df.iloc[2]              # row by position
df['Age']               # column as Series
df[['Name', 'Grade']]   # multiple columns
```

#### Filtering rows
```python
# Single condition
df[df['Age'] > 13]

# Multiple conditions — always use & / | with parentheses
df[(df['Age'] > 13) & (df['Grade'] == 'A+')]
```

#### Adding columns & rows
```python
# New column
df['City'] = ['Karachi', 'Lahore', 'Islamabad']

# Derived column (vectorized)
df['final_bill'] = df['total_bill'] + df['tip']

# New row via .loc
df.loc['Student 4'] = {'Name': 'Dan', 'Age': 14, 'Grade': 'B+'}

# ✅ Preferred for multiple rows — concat is faster than repeated .loc
new_rows = pd.DataFrame([{'Name': 'Eve', 'Age': 15, 'Grade': 'A'}])
df = pd.concat([df, new_rows], ignore_index=True)
```

---

### Loading & Inspecting Data

```python
df = pd.read_csv('tips.csv')   # load from CSV

df.head(5)         # first 5 rows
df.shape           # (rows, columns)
df.info()          # dtypes, non-null counts, memory
df.describe()      # summary statistics for numeric columns
df.sample(5)       # 5 random rows
df.to_string()     # full DataFrame as a string (use carefully for large data)

# Iterate column names
for col in df.columns:
    print(col)
```

---

### Data Cleaning

```python
df.drop(columns=['Unnamed: 0'])     # remove unwanted columns
df.dropna()                         # drop rows with any NaN
df.fillna({'Age': 0, 'Grade': 'N/A'})  # fill NaN per column
df.replace('?', None)               # replace a specific value
df.drop_duplicates()                # remove duplicate rows
df.drop_duplicates(subset=['Name']) # duplicates based on specific columns

# String operations
df['Name'].str.lower()
df['Name'].str.strip()
df['Name'].str.replace('-', '_')

# Type casting
df['Age'].astype(int)    # ⚠️ truncates decimals
```

---

### Counting & Aggregating

```python
# Count rows matching a condition
df[df['sex'] == 'Male']['sex'].count()

# Vectorized sum (preferred over loops)
df['final_bill'].sum()

# ⚠️ Avoid Python loops on DataFrames — use pandas methods instead
# Slow:  total = sum(row['final_bill'] for _, row in df.iterrows())
# Fast:  total = df['final_bill'].sum()
```

---

## Tips & Best Practices

- **Vectorize everything** — prefer `.sum()`, `.mean()`, `.str.lower()` over `for` loops.
- **Batch row additions** — collect new rows in a list, then call `pd.concat()` once instead of appending in a loop.
- **Work on copies** — use `df.copy()` when exploring transformations to avoid mutating the original dataset.
- **Use `.loc` for label access, `.iloc` for positional access** — mixing them up is a common source of bugs.
- **Always use parentheses with `&` / `|`** — Python operator precedence will silently break multi-condition filters otherwise.

---

## Resources

- [pandas documentation](https://pandas.pydata.org/docs/)
- [10 Minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)
