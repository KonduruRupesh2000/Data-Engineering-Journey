# Hive Data Structures and Drop Tables



We have created:
- `Department_data` (Internal Table)
- `Department_data_from_hdfs` (Internal Table)
- `Department_data_external` (External Table)

Drop table `Department_data` | drop table `Department_data_from_hdfs`
- Deletes table from metadata and warehouse

Drop table `Department_data_external`
- It just unlinks the pointer that is locating the data in HDFS, data is not destroyed

So In conclusion, when dealing with a critical system where data is important, it is better to use External tables than Internal tables.

Hive gives flexibility to deal with Advanced Data Structures like Array, Map
Now we can check out how it can be done

Dataset: `arrayData.csv`
Sample:
- 101,Amit,HADOOP:HIVE:SPARK:BIG-DATA
- 102,Sumit,HIVE:OZZIE:HADOOP:SPARK:STORM
- 103,Rohit,KAFKA:CASSANDRA:HBASE

We will create a table for `arrayData.csv`
Create table `employee` (
  id int,
  Name String,
  Skills array<string>  # column array type
)
Row format delimited
Fields terminated by ','
Collection items terminated by ':';  # when dealing with data structures we use collections

```sql
SELECT * FROM employee;
