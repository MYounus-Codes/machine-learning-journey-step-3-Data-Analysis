# ML Step 3 — Data Analysis with Pandas 🐼

> Hands-on pandas & NumPy examples for data loading, exploration, filtering, and cleaning.
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
- **Dependencies:** `pandas`, `numpy`

```powershell
# Activate your virtual environment (Windows PowerShell)
& .\.venv\Scripts\Activate.ps1

# Install dependencies
python -m pip install --upgrade pip
pip install pandas numpy
```

---

## API Reference

### Imports

```python
import pandas as pd
import numpy as np
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
df.replace('?', np.nan)             # replace a specific value
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

### NumPy

NumPy powers the numeric engine underneath pandas. These are the most common patterns used alongside DataFrames.

#### Arrays
```python
import numpy as np

a = np.array([1, 2, 3, 4, 5])          # 1-D array
b = np.array([[1, 2], [3, 4]])          # 2-D array
np.zeros((3, 4))                        # 3×4 array of 0s
np.ones((2, 3))                         # 2×3 array of 1s
np.arange(0, 10, 2)                     # [0, 2, 4, 6, 8]
np.linspace(0, 1, 5)                    # 5 evenly spaced values between 0–1
```

#### Array properties
```python
a.shape       # dimensions, e.g. (5,)
a.dtype       # data type, e.g. int64
a.ndim        # number of dimensions
a.size        # total number of elements
```

#### Arithmetic (element-wise)
```python
a + 10        # add scalar
a * 2         # multiply scalar
a + b         # element-wise addition (shapes must be compatible)
a ** 2        # element-wise power
np.sqrt(a)    # square root
```

#### Aggregation
```python
np.sum(a)
np.mean(a)
np.median(a)
np.std(a)         # standard deviation
np.min(a)
np.max(a)
np.argmin(a)      # index of minimum value
np.argmax(a)      # index of maximum value
```

#### Useful constants & functions
```python
np.nan            # Not a Number — used to represent missing values
np.inf            # infinity

np.isnan(a)       # boolean array — True where NaN
np.isinf(a)       # boolean array — True where inf

# Replacing NaN in a NumPy array before passing to pandas
a = np.array([1.0, np.nan, 3.0])
a[np.isnan(a)] = 0   # replace NaN with 0
```

#### Working with pandas
```python
# pandas Series wraps a NumPy array
s = pd.Series(np.array([10, 20, 30]))

# Convert a column to a NumPy array
arr = df['Age'].to_numpy()

# Create a column from a NumPy computation
df['score_scaled'] = np.log1p(df['score'])   # log(1 + x), safe for 0 values
df['price_rounded'] = np.round(df['price'], 2)
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
- [NumPy documentation](https://numpy.org/doc/)
- [10 Minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)
