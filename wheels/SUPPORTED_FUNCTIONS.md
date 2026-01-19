# Supported M Functions

**python_m**: 44 functions
**m_runtime** (Fabric wheel): 41 functions (excludes TransformColumnTypes, UnpivotOtherColumns, Pivot)

## Table (14 in m_runtime, 17 in python_m)

- `Table.AddColumn` - Add a computed column
- `Table.ColumnNames` - Get column names
- `Table.Distinct` - Remove duplicate rows
- `Table.FillDown` - Fill nulls with previous value
- `Table.FirstN` - Get first N rows
- `Table.LastN` - Get last N rows
- `Table.PromoteHeaders` - Use first row as headers
- `Table.RemoveColumns` - Remove columns
- `Table.RenameColumns` - Rename columns
- `Table.RowCount` - Count rows
- `Table.SelectColumns` - Select columns
- `Table.SelectRows` - Filter rows
- `Table.Skip` - Skip first N rows
- `Table.Sort` - Sort rows
- `Table.TransformColumnTypes` - Change column types *(python_m only)*
- `Table.UnpivotOtherColumns` - Unpivot columns *(python_m only)*
- `Table.Pivot` - Pivot rows to columns *(python_m only)*

## List (8)

- `List.Average` - Average of items
- `List.Count` - Count items
- `List.Distinct` - Unique values
- `List.First` - First item
- `List.Last` - Last item
- `List.Max` - Maximum value
- `List.Min` - Minimum value
- `List.Sum` - Sum of items

## Text (7)

- `Text.Contains` - Check for substring
- `Text.End` - Last N characters
- `Text.Length` - String length
- `Text.Lower` - Lowercase
- `Text.Start` - First N characters
- `Text.Trim` - Trim whitespace
- `Text.Upper` - Uppercase

## Date (4)

- `#date` - Construct date
- `Date.Day` - Day component
- `Date.Month` - Month component
- `Date.Year` - Year component

## Number (3)

- `Number.Abs` - Absolute value
- `Number.Round` - Round number
- `Number.Sign` - Sign (-1, 0, 1)

## File/CSV (2)

- `Csv.Document` - Parse CSV to table
- `File.Contents` - Read file

## Constructors (2)

- `#duration` - Construct duration
- `#table` - Construct table

---

## Not Yet Supported in m_runtime

These functions exist in `python_m` but are not yet wired up for the Fabric wheel:

**Table Functions (need backend implementation):**
- `Table.TransformColumnTypes` - Change column types
- `Table.UnpivotOtherColumns` - Unpivot columns
- `Table.Pivot` - Pivot rows to columns

**Not Applicable for m_runtime:**
- `File.Contents` - Use pandas/Spark to load files instead
- `Csv.Document` - Use `pd.read_csv()` or `spark.read.csv()` instead
- `#table` constructor - Pass DataFrames via `tables={}` parameter

**Not Implemented Anywhere Yet:**
- `Table.Group` - Grouping and aggregation
- `Table.Join` / `Table.NestedJoin` - Table joins
- `Table.ExpandTableColumn` - Expand nested tables
- `Web.Contents` - HTTP requests
- `Json.Document` - JSON parsing
- `Sql.Database` / `Sql.Databases` - SQL connections
- `Lakehouse.*` - Fabric Lakehouse functions

---

## m_runtime Notes

The `m_runtime` wheel is optimized for Microsoft Fabric notebooks:

- Pass pandas/PySpark DataFrames directly via `tables={}` parameter
- No need for `File.Contents` or `Csv.Document` - load data with Spark/pandas first
- Most Table.* functions work with DataFrames (see exceptions above)
- PySpark support with automatic backend detection
