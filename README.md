https://colab.research.google.com/drive/1ete8uYibOi9axG4C357OQts94EzwUa7A?usp=sharing

# ML Step 3 — Data Analysis (Course README)

This repository contains hands-on examples exploring pandas basics in `learning_pandas.ipynb`. The Colab notebook mirror is linked above for quick interactive use.

**Contents**
- `learning_pandas.ipynb`: pandas examples covering `Series`, `DataFrame`, filtering, importing CSVs, and data cleaning.
- `basics.ipynb`: (if present) additional short examples.

**Prerequisites**
- Python 3.10+ (noted project requires >=3.12 in `pyproject.toml`).
- Install dependencies in a virtual environment:

```powershell
& .\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install pandas numpy
```

---

## Definitions and examples of functions and code used

This section documents every pandas/Numpy function and common Python pattern used in the notebook, with a short definition and example.

- `import pandas as pd`
  - Imports the pandas library under the alias `pd`.
  - Example: `import pandas as pd`

- `pd.Series(data, index=...)`
  - Creates a one-dimensional labeled array. `data` can be list, ndarray, or dict.
  - Example: `pd.Series([100,101], index=['a','b'])`

- `series.loc[label]` and `series.loc[[labels]]`
  - Label-based indexer for selecting rows by index label (works for Series and DataFrame).
  - Example: `s.loc['a']`

- `series.iloc[position]` and `series.iloc[[positions]]`
  - Position-based indexer (zero-based integer positions).
  - Example: `s.iloc[0]`

- `series[index] = value` and `series.iloc[pos] = value`
  - Assign/update a value in a Series by label or position.
  - Example: `s.iloc[1] = 110`

- Boolean indexing (masking)
  - Use boolean expressions to filter Series/DataFrame rows: `data[data >= 6]` selects values >= 6.
  - Combine conditions for DataFrames with `&` and `|` (use parentheses).
  - Example: `df[(df['Age'] > 13) & (df['Grade'] == 'A+')]`

- Aggregation: `min()`, `max()`
  - Compute the minimum/maximum of Series or along DataFrame columns.
  - Example: `data.min()`

- `pd.DataFrame(dict, index=...)`
  - Create a two-dimensional labeled table from a dict of lists (columns).
  - Example: `pd.DataFrame({'Name':['A'],'Age':[20]})`

- `df.loc[row_label]` and `df.iloc[row_pos]`
  - Row selection by label or position.
  - Example: `df.loc['Student 1']`, `df.iloc[2]`

- Column selection `df['ColName']`
  - Select a column as a Series.
  - Example: `df['Age']`

- Row filtering `df[df['Col'] == value]`
  - Return a DataFrame consisting of rows where condition is True.

- Adding a new column `df['NewCol'] = values`
  - Assign values (list/Series/array) to create a new column.
  - Example: `df['City'] = ['K','L','I']`

- Adding/appending rows with `.loc` assignment
  - You can assign a dict to a new index label: `df.loc['Student 4'] = {...}`. This will add a new row, aligning columns by keys.
  - For robust appends, prefer `pd.DataFrame([...], index=[...])` + `pd.concat()`.

- `pd.concat([df, new_df])`
  - Concatenate DataFrames along rows (default) or columns.
  - Example: `df = pd.concat([df, student_7])`

- `pd.read_csv(path)`
  - Read a CSV file into a DataFrame.
  - Example: `df = pd.read_csv('tips.csv')`

- `df.head(n)`
  - Return the first `n` rows (default 5).

- `df.shape`
  - Tuple (rows, columns) describing DataFrame size.

- `df.info()`
  - Summary of DataFrame: index dtype, column dtypes, non-null counts and memory usage.

- `df.describe()`
  - Summary statistics for numeric columns (count, mean, std, min, 25%, 50%, 75%, max).

- Column arithmetic and creation of derived columns
  - You can perform vectorized arithmetic: `final_bill = df['total_bill'] + df['tip']`
  - Assigning a computed Series to a new column: `df['final_bill'] = final_bill`

- Filtering with multiple conditions and `.count()`
  - Example shown: selecting male customers with particular attributes and counting rows via `male_customers['sex'].count()`.

- Looping and aggregating (Python loop)
  - Example in notebook: summing `final_bill` values via a for-loop. Prefer `male_customers['final_bill'].sum()` for efficiency.

- `df.to_string()`
  - Render the entire DataFrame to a single string (use with caution for large DataFrames).

- `df.sample(n)`
  - Return a random sample of `n` rows from the DataFrame.

- Iterating columns: `for col in df.columns:`
  - Useful to print or inspect column names.

--- Data Cleaning helpers ---

- `df.drop(columns=[...])`
  - Drop one or more columns and return a new DataFrame (or use `inplace=True`).

- `df.dropna()`
  - Drop rows containing any NaN values.

- `df.fillna(value_or_dict)`
  - Fill NaN values with a scalar or dict mapping columns to fill values.

- `df.replace(old, new)`
  - Replace specific values in the DataFrame.

- String methods `.str` (e.g., `df['col'].str.lower()`)
  - Vectorized string operations for Series of dtype object/string.

- `df.astype(dtype)`
  - Cast a Series to a different dtype (e.g., float -> int). Use carefully (may truncate).

- `df.drop_duplicates()`
  - Remove duplicate rows. Optionally supply `subset` to consider specific columns.

---

## Tips and best practices

- Prefer vectorized pandas operations over Python loops for speed (e.g., use `.sum()` instead of a `for` loop).
- When adding rows repeatedly, collect them into a list and `pd.concat()` once — avoid many single-row `.loc` assignments inside loops.
- Use copies (`df.copy()`) when demonstrating transformations to avoid accidentally mutating the original dataset during examples.

---

If you'd like, I can also:
- generate a `requirements.txt` or update `pyproject.toml` with explicit pinned versions used in the examples, or
- open the Colab link and port the notebook to Colab-compatible paths.
