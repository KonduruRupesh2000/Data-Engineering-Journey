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
```

Output:

101 Amit ["Hadoop","HIVE","SPARK","BIG-DATA"]
102 …
103 …
Operations:

Indexing in Array

```sql
SELECT id, name, skills[0] AS prime_skill FROM employee;
```
Output:

101 Amit Hadoop
102 …
103 …
Size of array, Find string, sort the array


```sql
SELECT id,
       name,
       size(skills) AS size_of_each_array,
       array_contains(skills, "HADOOP") AS knows_hadoop,
       sort_array(skills) AS sorted_array
FROM employee;
```

Maps Data Structure (key:value)

Dataset: map_data.csv
Sample:

101,Amit,age:21|gender:M
102,Sumit,age:24|gender:M
103,Mansi,age:23|gender:F
Create table employee_map_data (
id int,
name string,
details map<string,string>
)
Row format delimited
Fields terminated by ','
Collection items terminated by '|'
Map keys terminated by ':';

Operations: Indexing

```sql
SELECT id,
       name,
       details["gender"] AS employee_gender
FROM employee_map_data;
```
Output:

101 Amit M
102 Sumit M
103 …
## Map functions

```sql
SELECT id,
       name,
       details,
       size(details) AS size_of_each_map,
       map_keys(details) AS distinct_map_keys,
       map_values(details) AS distinct_map_values
FROM employee_map_data;
```
