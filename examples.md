# Examples

## Basic Expressions

### Arithmetic
```python
from python_m.runtime import evaluate

result = evaluate('1 + 2 * 3')
print(result)  # 7

result = evaluate('10 / 2')
print(result)  # 5.0
```

### Let Expressions
```python
result = evaluate('''
let
    x = 10,
    y = 20,
    sum = x + y
in
    sum
''')
print(result)  # 30
```

### Text Operations
```python
result = evaluate('"Hello" & " " & "World"')
print(result)  # Hello World

result = evaluate('Text.Upper("hello")')
print(result)  # HELLO
```

## Working with Tables

### Creating Tables
```python
from python_m.runtime import evaluate

result = evaluate('''
#table(
    {"Name", "Age", "City"},
    {
        {"Alice", 30, "New York"},
        {"Bob", 25, "Los Angeles"},
        {"Carol", 35, "Chicago"}
    }
)
''')
print(result.to_pandas())
```

### Filtering Rows
```python
result = evaluate('''
let
    Source = #table({"Name", "Age"}, {{"Alice", 30}, {"Bob", 25}, {"Carol", 35}}),
    Filtered = Table.SelectRows(Source, each [Age] > 28)
in
    Filtered
''')
print(result.to_pandas())
# Output:
#     Name  Age
# 0  Alice   30
# 1  Carol   35
```

### Selecting Columns
```python
result = evaluate('''
let
    Source = #table({"Name", "Age", "City"}, {{"Alice", 30, "NYC"}, {"Bob", 25, "LA"}}),
    Selected = Table.SelectColumns(Source, {"Name", "City"})
in
    Selected
''')
print(result.to_pandas())
# Output:
#     Name City
# 0  Alice  NYC
# 1    Bob   LA
```

### Adding Columns
```python
result = evaluate('''
let
    Source = #table({"Name", "Age"}, {{"Alice", 30}, {"Bob", 25}}),
    WithCategory = Table.AddColumn(Source, "Category", each if [Age] >= 30 then "Senior" else "Junior")
in
    WithCategory
''')
print(result.to_pandas())
```

### Sorting
```python
result = evaluate('''
let
    Source = #table({"Name", "Age"}, {{"Alice", 30}, {"Bob", 25}, {"Carol", 35}}),
    Sorted = Table.Sort(Source, {"Age", Order.Descending})
in
    Sorted
''')
print(result.to_pandas())
```

## Using m_runtime with DataFrames

The `m_runtime` module provides an easy way to work with pandas DataFrames.

### Basic Usage
```python
import pandas as pd
from m_runtime import run_m

# Create sample data
sales = pd.DataFrame({
    'Product': ['Widget', 'Gadget', 'Widget', 'Gadget'],
    'Amount': [100, 200, 150, 250],
    'Region': ['East', 'West', 'West', 'East']
})

# Filter using M expression
result = run_m('''
    Table.SelectRows(Sales, each [Amount] > 150)
''', tables={'Sales': sales})

print(result)
# Output:
#   Product  Amount Region
# 0  Gadget     200   West
# 1  Gadget     250   East
```

### Multiple Tables
```python
import pandas as pd
from m_runtime import run_m

customers = pd.DataFrame({
    'CustomerID': [1, 2, 3],
    'Name': ['Alice', 'Bob', 'Carol']
})

orders = pd.DataFrame({
    'OrderID': [101, 102, 103],
    'CustomerID': [1, 2, 1],
    'Amount': [50, 75, 100]
})

result = run_m('''
let
    Merged = Table.NestedJoin(Orders, "CustomerID", Customers, "CustomerID", "CustomerData", JoinKind.Inner),
    Expanded = Table.ExpandTableColumn(Merged, "CustomerData", {"Name"})
in
    Expanded
''', tables={'Customers': customers, 'Orders': orders})

print(result)
```

### Aggregations
```python
import pandas as pd
from m_runtime import run_m

sales = pd.DataFrame({
    'Product': ['A', 'B', 'A', 'B', 'A'],
    'Amount': [100, 200, 150, 250, 50]
})

result = run_m('''
let
    Grouped = Table.Group(Sales, {"Product"}, {{"TotalAmount", each List.Sum([Amount]), type number}})
in
    Grouped
''', tables={'Sales': sales})

print(result)
# Output:
#   Product  TotalAmount
# 0       A          300
# 1       B          450
```

## Microsoft Fabric Notebooks

### Setup

1. Open your notebook in Fabric
2. Click "Resources" in the left sidebar (folder icon)
3. Upload the wheel files

### Complete Example

**Cell 1 - Install packages:**
```python
%pip install "builtin/python_m-0.1.0-py3-none-any.whl"
%pip install "builtin/m_runtime-0.1.0-py3-none-any.whl"
```

**Cell 2 - Run M queries:**
```python
import pandas as pd
from m_runtime import run_m

# Create test DataFrame
Sales = pd.DataFrame({
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'Amount': [100, 250, 150, 300],
    'Category': ['A', 'B', 'A', 'B']
})

code = '''
let
    Filtered = Table.SelectRows(Sales, each [Amount] > 100),
    WithBonus = Table.AddColumn(Filtered, "Bonus", each [Amount] * 0.15),
    HighValue = Table.SelectRows(WithBonus, each [Bonus] > 30)
in
    HighValue
'''

result = run_m(code, tables={'Sales': Sales})
display(result)
```
