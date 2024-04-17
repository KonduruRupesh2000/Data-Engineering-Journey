# Hive Usecase - Create tables

**Sunday, April 14, 2024**  
*12:19 AM*

This page focuses on the creation of Hive Internal and External Tables and understanding the concepts which we learned about tables in previous articles.

## hive 
Explanation: starts hive CLI  

```sql
Create database `hadoop_class_b1`  
Explanation: Creates Database  
Use `hadoop_class_b1`  
```
Explanation: use the database you wish to work on.

```sql
create table department_data (
    dept_id int,
    dept_name string,
    manager_id int,
    salary int
) 
row format delimited 
fields terminated by ',';
```

Explanation: Creates a table along with column names where rows are separated by NEW LINE (default is '\n') and fields are separated by ','.

```sql
describe department_data;
```
Explanation: Provides details about the table

```sql
describe formatted department_data;
```
Explanation: Deeper information about the table (OWNER Name, Table Type, Location, etc)

```sql
load data local inpath 'file:///home/cloudera/data/depart_data.csv' into table department_data;
``
Explanation: For data load from local.

```sql
Select * from department_data;
```
Explanation: Show contents of the table after loading from local.

```sql
set hive.cli.print.header = true;
```
Explanation: Display column name

```sql
load data inpath '/tmp/hive_data_class_2/' into table department_data_from_hdfs;
```
Explanation: Load data from HDFS location

```sql
create external table department_data_external (
    dept_id int,
    dept_name string,
    manager_id int,
    salary int
) 
row format delimited 
fields terminated by ',' 
location '/tmp/hive_data_class_2/'; 
```


## Learning points:
Read data from local to Hive
Read data from HDFS to Hive
Read data from HDFS to Hive (external table) (it just points the Hive engine to HDFS folder location where data is stored)
Difference between Internal Tables and External Tables
Internal Tables: Two things will be created

Metadata in Hive
Original / raw data in Hive
External Tables:

Only metadata in Hive is created