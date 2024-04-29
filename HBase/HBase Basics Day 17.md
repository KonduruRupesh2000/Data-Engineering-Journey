# NoSQL Databases We Have:
1. Hbase
2. MongoDB
3. Cassandra
4. Neo4j
5. couchDB

**Hbase**: Hadoop Based Database, it is a NoSQL database

## Things I'll Cover:
1. NoSQL vs SQL
2. Types of NoSQL
3. Hbase Data Model
4. Hbase Architecture
5. Hbase vs HDFS
6. Hbase vs Hive

## NoSQL vs SQL

**SQL**:
- Structured Query language used for RDBMS.
- Table-based schema.
- Follows ACID property (Atomicity, Consistency, Isolation, Durability).
- Allows Vertical Scaling (scaling in).
- Predefined Schema (Rigid schema like row and column).

**NoSQL**:
- Non-relational database (No traditional row and column form).
- Document-based schema, Graph-based schema, etc.
- Follows CAP (Consistency, Availability, Partitioning).
- Allows Horizontal Scaling (scaling out).
- Dynamic Schema.

### Types of NoSQL:
1. **Key-Value pair**: Data is stored in key-value form (e.g., keys = name, age, gender; Value = sunny, 24, male). Examples: Redis, DynamoDB, Riak.
2. **Column-oriented**: Involves RowKey, columns, and subColumns. Examples: HBASE, Cassandra, Hypertable.
3. **Document Based**: Data stored in the form of JSON or XML. Examples include:
   - Document1: `{ "height": 170, "Weight": 80, "BMI": 20 }`
   - Document2: `{ "height": 160, "Weight": 90, "BMI": 21 }`
   - CouchDB, MongoDB, Amazon SimpleDB.
4. **Graph Based**: Entities are defined along with their relationships. Examples: Neo4j, OrientDB, FlockDB.

### Data Model:
1. **RDMS**: (Row and column)

```
Name Gender
rupesh M
ram M
```

2. **Hive**: (Table with partitions)

```
Partition1     Partition2   Partition3
Bucket1 B2 B3   B1 B2  B3    B1 B2 B3
```

3. **Column-oriented Database**:

```
Personal Details Office Details
Name Gender       EmpId Email
ram M               1 alfja@
rupesh M            2 afjajl@
```


**Are RDMS and Hbase the same?** No. In RDMS, to fetch the value at the 4th row and 5th position, you need to scan each row to get there. In Hbase, you can check only the column you need to return.

### HDFS vs HBASE:
- **HDFS**: File system; Write once, read many times; Access: MapReduce, Hive, Pig; Storage with HDFS and processed with Map Reduce.
- **HBASE**: Database; Random read/write; Extract data: API, Server, RestAPI; Storage and processes are performed with HBASE commands.

### Hive:
- OLAP
- SQL(HQL)
- Rigid Schema

### Principles:

- **ACID** (Followed by RDBMS): Atomicity, Consistency, Isolation, Durability.
- **CAP** (Followed by NoSQL): Consistency, Availability, Partition.
- **BASE** (Followed by NoSQL): Basically Available, Soft state, Eventually consistent.
