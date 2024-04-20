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

### Create a table which will store data in parquet

```sql
CREATE TABLE sales_data_pq_final (
  product_type STRING,
  total_sales INT
)
STORED AS PARQUET;
```

### Load data in parquet file

```sql
FROM sales_data_v2
INSERT OVERWRITE TABLE sales_data_pq_final SELECT *;
```

`STORED AS PARQUET` - Indicates HIVE engine that the file format is Parquet, so deserialization should happen in parquet.

## In general, we have two kinds:
- **Schema read**
- **Schema write**

**Schema read** is used in data formats like Parquet, ORC. It just reads the data without any schema checks. For example, when CSV data is loaded into a parquet table, it just dumps the data from the CSV table to the parquet table; only when a SELECT query is used does it check the schema and prints out data.

### Why garbage data occurs - due to CSV to PQ (miscompatibility)
In general, the data format should be the same during serialization and deserialization; if it is not, then garbage/non-readable data will be shown.

For example, in the earlier example of `sales_data`, it throws garbage data because while serialization it uses a CSV file and for deserialization it uses a parquet file.

The correct sequence should be:
1. CSV to Parquet
2. Serialize the parquet file
3. Deserialize the file - Parquet
