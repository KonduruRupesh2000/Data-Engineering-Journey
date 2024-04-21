# Recap

Serialization: Converting objects into bytes.
Deserialization: Converting bytes into objects.

## Working of MapReduce for HQL in Serialization and Deserialization

### Flow of Execution
HDFS -> Map -> Reduce -> HDFS

In Hdfs, a Parquet file is stored, so when HQL is executed, internally MapReduce will begin. Since the file stored in Hdfs is Parquet, Mapper and Reducer functions will use libraries like SerDe to read and output the data in Parquet.

To know about the input and output format, implement this query:

```sql
DESCRIBE FORMATTED sales_data_pq_final;
```

# Dealing with CSV Files with Column Names and Custom Column Values
### Sample Data:
- name,location
- Amit,"Bangalore,India"
- Shashank,"Gurgaon,India"

Here, the location contains values separated with ','.

### To load this data:

```sql
CREATE TABLE csv_table (
    name STRING,
    location STRING
) ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
    "separatorChar" = ",",
    "quoteChar" = "\"",
    "escapeChar" = "\\"
)
STORED AS TEXTFILE
TBLPROPERTIES ("skip.header.line.count" = "1");
```

Here, the SerDe org.apache.hadoop.hive.serde2.OpenCSVSerde is a library that deals with raw data (data like nested quotes, nested delimiter). These SerDes provide properties which can be defined with serdeproperties.

### Loading Data from Local

```sql
LOAD DATA LOCAL INPATH 'file:///tmp/hive_class/csv_file.csv' INTO TABLE csv_table;
```

### Download Hive Catalog JAR File

If SerDe libraries are not imported, download the Hive catalog jar file:

```
https://repo1.maven.org/maven2/org/apache/hive/hcatalog/hive-hcatalog-core/0.14.0/
```

Add the JAR file into your Hive shell:

ADD JAR /tmp/hive_class/hive-hcatalog-core-0.14.0.jar;

## Create JSON Table

```sql
CREATE TABLE json_table (
    name STRING,
    id INT,
    skills ARRAY<STRING>
) ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'
STORED AS TEXTFILE;
```

The advantage of using SerDe for JSON is that SerDe only takes care of JSON parsing (e.g., array, map, etc.), so we just need to create the table and the rest will be taken by the package itself.
