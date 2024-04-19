# Hive Data Formats

Wednesday, April 17, 2024  
12:48 PM

File formats in general we have are:  
1. Parquet  
2. ORC  
3. AVRO  
These file formats are widely used in the Hadoop ecosystem.

In memory data usually stores in  
1. Row columnar format  
2. Columnar format  

## Row columnar Format:  
Example:  
ID   Name      Salary  
1    Rupesh    100  
2    Varshith  200  
3    ravi      300  

Memory storage for Row columnar format: It stores in contiguous memory locations  
1 Rupesh 100 2 Varshith 200 3 ravi 300  

If I need to perform operation like: `SELECT sum(salary) from salary;`  
Then Disk reader reads from 1 and move to 100 (once salary value is found it aggregates) then move to 200 and 300 in sequence.

## Columnar Format:  
Example:  
ID     1  2  3  
Name   Rupesh Varshith Ravi  
Salary 100   200      300  

This format is efficient way for: `SELECT sum(salary) from salary;` Instead of scanning every memory location it just reads salary values.

For frequent or Heavy analytical queries preferred are Columnar formats or columnar NoSQL (Key:value format, Documents) DBs

So now we can discuss about:  
1. Parquet (Columnar)  
2. ORC (Columnar) (optimized Row Columner)  
3. AVRO (Row-Columnar)  

Spark Engine Prefers Parquet.  
Hive Framework Prefers ORC.  
AVRO - For Frequent write or update cases.

These File Formats provides Compression as well. Which is good while we are working with large data.  
For Instance CSV=1GB then parquet can be = 200MB  

Pros of Parquet, ORC, AVRO  
1. Fast Analytical Queries  
2. No manual conversion is needed (in compression scenario)  

ORC>Parquet>AVRO (highest level of compression is achieved )  

## Serialization and Deserialization:

**Serialization**: Converting original data or object or data types to bytes.  
Use case: In distributed computing data will be stored in different nodes, if for particular preprocessing one node needs data of another node then in that case data should be moved from one node to another. In this case we need to serialize data to transfer it.

**Deserialization**: Converting something bytes to its original form.

Advantage of serialization  
- Data in bytes  
- Fast transmission over network  
- Less data size  

Wherever we are querying the data (Serialized form) we need mechanism/package to deserialize it  
For example:

If I have two systems ie S1 and S2  
S1 - Serialize the data using CSV  
S2 - Deserialization should happen using CSV Only (not with any other data formats)  
