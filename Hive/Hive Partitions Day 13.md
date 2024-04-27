## Big Data Processing and Partitioning

Partitions have become a very important part of Big data processing.

**Use Case:**  
If I had 1TB of employee data and needed to process the rows whose fields are in the IT department, it would take more processing time. This is because our engine is not aware of where IT fields are present, so it would have to check line by line to return the IT department.

**Example:**

```
Name, salary, country
Shashank, 1, India
Ravi, 1000, UK
Ram, 2000, India
Rahul, 800, China
Kapil, 900, USA
Var, 1000, UK
```

### Which column should be used for partitioning?

The decision should be based on:
1. Columns used to filter the data frequently
2. Avoid using primary key type of columns for partitioning (because each record will be unique so it results in a million of partitions).
3. Less Uniqueness (repeated multiple times)

### How partitions are stored in HDFS:


/user/hive/warehouse/db_name/tb_name/country_India/
/user/hive/warehouse/db_name/tb_name/country_USA/
/user/hive/warehouse/db_name/tb_name/country_UK/


### Types of Partitioning in Hive

1. **Static Partitioning:**  
   We know in which partition data will get loaded (we will hard code on where partition should happen, how it should store, etc.). The system does not interfere.

2. **Dynamic Partitioning:**
   - If data is big and we are not aware of distinct values present in it, then dynamic partitioning comes into play.
   - Hive will scan the partition column and will create a partition in HDFS for each distinct value.

**Note:** Dynamic partitioning performs slower because the Hive engine must go through all the rows to find distinct values and create partitions upon them.

### After creating the table, check the number of files in the HDFS location.

To ensure whole data won't be traversed during static partitioning:

```sql
set hive.mapred.mode=strict;
```

## Create Table Commands for Partition Tables
### For Static Partitioning:

```sql
CREATE TABLE sales_data_static_part
(
    ORDERNUMBER int,
    QUANTITYORDERED int,
    SALES float,
    YEAR_ID int
)
PARTITIONED BY (COUNTRY string);
```

### Load Data in Static Partition:

```sql
INSERT OVERWRITE TABLE sales_data_static_part PARTITION(country = 'USA') SELECT
ordernumber, quantityordered, sales, year_id FROM sales_order_data_orc WHERE country = 'USA';
```

### For Dynamic Partitioning:

Set this property:

```sql
set hive.exec.dynamic.partition.mode=nonstrict;
```

Create the dynamic partition table:

```sql
CREATE TABLE sales_data_dynamic_part
(
    ORDERNUMBER int,
    QUANTITYORDERED int,
    SALES float,
    YEAR_ID int
)
PARTITIONED BY (COUNTRY string);
```

### Load Data in Dynamic Partition Table:

```sql
INSERT OVERWRITE TABLE sales_data_dynamic_part PARTITION(country) SELECT
ordernumber, quantityordered, sales, year_id, country FROM sales_order_data_orc;
```

### Multilevel Partitioning
#### Create the table:

```sql
CREATE TABLE sales_data_dynamic_multilevel_part_v1
(
    ORDERNUMBER int,
    QUANTITYORDERED int,
    SALES float
)
PARTITIONED BY (COUNTRY string, YEAR_ID int);
```

### Load Data in Multilevel Partitions:

```sql
INSERT OVERWRITE TABLE sales_data_dynamic_multilevel_part_v1 PARTITION(country, year_id) SELECT
ordernumber, quantityordered, sales, country, year_id FROM sales_order_data_orc;
```