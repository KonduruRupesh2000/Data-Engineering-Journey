# Hive Project - Sales Data- ORC format

**Date**: Saturday, April 20, 2024  
**Time**: 6:04 PM

Now we can work on a new dataset i.e., Sales Data:

Sales data contains the following columns:
- ORDERNUMBER
- QUANTITYORDERED
- PRICEEACH
- ORDERLINENUMBER
- SALES
- STATUS
- QTR_ID
- MONTH_ID
- YEAR_ID
- PRODUCTLINE
- MSRP
- PRODUCTCODE
- PHONE
- CITY
- STATE
- POSTALCODE
- COUNTRY
- TERRITORY
- CONTACTLASTNAME
- CONTACTFIRSTNAME
- DEALSIZE

## Create CSV Table for Sales Data

```sql
CREATE TABLE sales_order_data_csv_v1 (
  ORDERNUMBER INT,
  QUANTITYORDERED INT,
  PRICEEACH FLOAT,
  ORDERLINENUMBER INT,
  SALES FLOAT,
  STATUS STRING,
  QTR_ID INT,
  MONTH_ID INT,
  YEAR_ID INT,
  PRODUCTLINE STRING,
  MSRP INT,
  PRODUCTCODE STRING,
  PHONE STRING,
  CITY STRING,
  STATE STRING,
  POSTALCODE STRING,
  COUNTRY STRING,
  TERRITORY STRING,
  CONTACTLASTNAME STRING,
  CONTACTFIRSTNAME STRING,
  DEALSIZE STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
TBLPROPERTIES("skip.header.line.count"="1");
```

## Load sales_order_data.csv Data into Above Mentioned Tables
## ORC Table Creation

```sql
CREATE TABLE sales_order_data_orc (
  ORDERNUMBER INT,
  QUANTITYORDERED INT,
  PRICEEACH FLOAT,
  ORDERLINENUMBER INT,
  SALES FLOAT,
  STATUS STRING,
  QTR_ID INT,
  MONTH_ID INT,
  YEAR_ID INT,
  PRODUCTLINE STRING,
  MSRP INT,
  PRODUCTCODE STRING,
  PHONE STRING,
  CITY STRING,
  STATE STRING,
  POSTALCODE STRING,
  COUNTRY STRING,
  TERRITORY STRING,
  CONTACTLASTNAME STRING,
  CONTACTFIRSTNAME STRING,
  DEALSIZE STRING
)
STORED AS ORC;
```

## Copy Data from sales_order_data_csv_v1 to sales_order_data_orc

```sql
SELECT year_id, SUM(sales) AS total_sales FROM sales_order_data_orc GROUP BY year_id;
```

Here only 1 map and 1 reduce task will get created
To change the average load for a reducer (in bytes):

```sql
SET hive.exec.reducers.bytes.per.reducer=<number>;
```

To limit the maximum number of reducers:

```sql
SET hive.exec.reducers.max=<number>;
```

To set a constant number of reducers:


```sql
SET mapreduce.job.reduces=<number>;
```

Change Number of Reducers to 3

```sql
SET mapreduce.job.reduces=3;
CREATE TABLE sales_order_grouped_orc_v1 STORED AS ORC AS 
SELECT year_id, SUM(sales) AS total_sales FROM sales_order_data_orc GROUP BY year_id;
```


After creating the table, check the number of files in HDFS location
Change Number of Reducers to 2

```sql
SET mapreduce.job.reduces=2;
CREATE TABLE sales_order_grouped_orc_v2 STORED AS ORC AS 
SELECT year_id, SUM(sales) AS total_sales FROM sales_order_data_orc GROUP BY year_id;
```

### Difference between Sort By and Order By in Hive
Hive sort by and order by commands are used to fetch data in sorted order. The main differences between sort by and order by commands are given below.

Sort by

```sql
SELECT E.EMP_ID FROM Employee E SORT BY E.empid;
```

- May use multiple reducers for the final output.
- Only guarantees ordering of rows within a reducer.
- May give partially ordered result.
Performance: Because it forces data through a single reducer, ORDER BY can be very slow and inefficient for large amounts of data.
Scalability: As data grows, the single reducer will take longer to process all the data, limiting scalability.
Resource Utilization: It may not utilize the cluster resources effectively as it does not parallelize the sorting process.

## OrderBy

```sql
SELECT E.EMP_ID FROM Employee E ORDER BY E.empid;
```

- Uses a single reducer to guarantee total order in output.
- LIMIT can be used to minimize sort time.
- Partial Ordering: With SORT BY, the total ordering of the output is not guaranteed. If you need a globally ordered list, SORT BY won't suffice.
- Redundancy: Sometimes SORT BY may not be useful if a subsequent operation like JOIN does not preserve the sorted order, potentially requiring another sort operation.
- Less Intuitive: For those accustomed to SQL from traditional RDBMS, the behavior of SORT BY not providing a total sort can be less intuitive and lead to misunderstandings about the output.