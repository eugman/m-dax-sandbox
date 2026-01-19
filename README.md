# m-dax-sandbox

Power Query M and DAX runtimes for Python. Evaluate M and DAX expressions directly without Power BI.

**Web sandbox**: https://vibes.sqlgene.com/m-dax-sandbox/

## Install

Download wheels from [Releases](https://github.com/eugman/m-dax-sandbox/releases) or the `wheels/` folder.

**Local:**
```bash
pip install python_m-0.1.0-py3-none-any.whl
pip install m_runtime-0.1.0-py3-none-any.whl  # optional, for Fabric
```

**Microsoft Fabric Notebook:**

1. Open your notebook in Fabric
2. Click "Resources" in the left sidebar (folder icon)
3. Upload the wheel files
4. In the first cell:

```python
%pip install "builtin/python_m-0.1.0-py3-none-any.whl"
%pip install "builtin/m_runtime-0.1.0-py3-none-any.whl"
```

Then use `run_m()` with your DataFrames:

```python
from m_runtime import run_m

result = run_m('''
    Table.SelectRows(Sales, each [Amount] > 100)
''', tables={"Sales": df_sales})

display(result)
```

Dependencies: pandas (Python 3.10+). Polars/pyarrow optional.

## Usage

```python
from python_m.runtime import evaluate

result = evaluate('let x = 1 + 2 in x')
print(result)  # 3

# Tables
result = evaluate('''
let
    Source = #table({"Name", "Age"}, {{"Alice", 30}, {"Bob", 25}}),
    Filtered = Table.SelectRows(Source, each [Age] > 26)
in
    Filtered
''')
print(result.to_pandas())
```

## Issues

Report bugs and feature requests: https://github.com/eugman/m-dax-sandbox/issues

## License

Copyright 2026. All rights reserved.
