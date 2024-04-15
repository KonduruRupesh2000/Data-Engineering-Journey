
# Hive Basics and Architecture


- Hive is built to develop distributed computing applications with ease (No prior knowledge of complex programming is needed)
- Hive is a Framework, and basically, it is a Data warehousing service (stores data in a meaningful manner)
- Hive is not a database, it acts like a database but typically it is a processing Engine

## Why HIVE

- Simple to use
- Built on top of Hadoop
- Typical SQL kind of framework (whatever logic we wrote in Hive will be converted into Map_Reduce code to access HDFS data)

## Where Hive fits in Architecture:

1. HDFS - File Storage
2. Map-Reduce - Processing
3. YARN - Resource Manager

## Explanation on Hive communication:

- We need connectors to communicate with HIVE (JDBC/ODBC connectors)
- Communication medium to HIVE: CLI, Thrift, Web Interface
- Driver block (parser, planner, execution, optimizer) begins the requests that have been passed.
  - Driver acts like Controller for HQL statements
  - Creates sessions for query
  - Maintain lifecycle of query of HQL
  - Maintain metadata for execution
  - Collect output and display

### Parsing/compilation/planner:

- Syntax check, creates execution plan, raises compile-time errors

### Optimize:

- Compare execution plans, calculate the cost, try to place or combine transformations together

### Execution:

- Execute Hive

### Metastore:

- Maintains information about tables, data, etc.

### Flow:

- Hive -- Planning/Parsing -- Optimize -- Execution

### Database:

- By default, HIVE uses the Derby Database (a type of RDBMS)
- Drawback of using Derby DB: We cannot create more than one connection to HIVE (here connection means if Prog1 has successfully connected with HIVE, if Prog2 tries to connect concurrently then it does not work).

So, we can use other DB like MYSQL, PostgreSQL, etc., for MetaStore.

#### Uses:

- External DB
- Metadata Backup, if HIVE is uninstalled, it does not cause any issue with the metadata as we are not using the default DB
- More concurrent connections
- Expose to external use cases.
