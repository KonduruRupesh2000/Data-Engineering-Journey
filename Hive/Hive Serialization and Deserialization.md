# Hive Serialization and Deserialization Implementation



## Steps to create Parquet file:
1. Create table for CSV
2. Load CSV file from local to table (which is created)
3. Create a table for parquet (by explicitly mentioning it)
4. Now load the `csv_Table` to new `Pq_Table` (newly created table)

### First load data as CSV

```sql
CREATE TABLE sales_data_v2 (
  p_type STRING,
  total_sales INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

```sql
LOAD DATA LOCAL INPATH 'file:///tmp/hive_class/sales_data_raw.csv' INTO TABLE sales_data_v2;
```

### Command to create an identical table

```sql
CREATE TABLE sales_data_v2_bkup AS
SELECT * FROM sales_data_v2;
```

### Describe command for a table

```sql
DESCRIBE EXTENDED sales_data_v2;
```