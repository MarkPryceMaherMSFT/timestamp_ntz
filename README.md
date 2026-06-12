# timestamp_ntz

Demonstrates how to work with Delta table data types in Microsoft Fabric, specifically handling types that the **SQL Analytics Endpoint** cannot query directly.

## Notebooks

### 1. `create_delta_table_datatypes.ipynb`

Creates a Delta table (`all_datatypes_demo`) with **100 rows** of randomized data covering all major Spark data types:

| Data Type | Column |
|-----------|--------|
| BYTE (TINYINT) | `byte_col` |
| SHORT (SMALLINT) | `short_col` |
| INT | `int_col` |
| LONG (BIGINT) | `long_col` |
| FLOAT | `float_col` |
| DOUBLE | `double_col` |
| DECIMAL(38,18) | `decimal_col` |
| STRING | `string_col` |
| BOOLEAN | `boolean_col` |
| DATE | `date_col` |
| TIMESTAMP | `timestamp_col` |
| TIMESTAMP_NTZ | `timestamp_ntz_col` |
| BINARY | `binary_col` |
| ARRAY\<INT\> | `array_col` |
| MAP\<STRING,INT\> | `map_col` |

Both `TIMESTAMP` and `TIMESTAMP_NTZ` use full **microsecond precision** (6 decimal places) to demonstrate the difference:
- **TIMESTAMP** — stored as UTC; Spark adjusts for the session timezone on read.
- **TIMESTAMP_NTZ** — stored as-is with no timezone conversion.

### 2. `convert_for_sql_endpoint.ipynb`

Reads `all_datatypes_demo` and creates a new table (`all_datatypes_demo_sql_compatible`) with unsupported types converted so the Fabric SQL Analytics Endpoint can query them:

| Original Type | Converted To | Method |
|---------------|-------------|--------|
| TIMESTAMP_NTZ | TIMESTAMP | `.cast(TimestampType())` |
| ARRAY\<INT\> | STRING | `to_json()` — JSON array |
| MAP\<STRING,INT\> | STRING | `to_json()` — JSON object |

All other columns pass through unchanged.

## Usage

1. Import both notebooks into a Fabric workspace attached to a Lakehouse.
2. Run `create_delta_table_datatypes.ipynb` first to create the source table.
3. Run `convert_for_sql_endpoint.ipynb` to produce the SQL-compatible table.
4. Query `all_datatypes_demo_sql_compatible` from the SQL Analytics Endpoint.
