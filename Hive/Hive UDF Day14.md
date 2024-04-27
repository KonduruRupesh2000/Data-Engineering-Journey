# UDF in Hive

**UDF** stands for User Defined Functions.

## Need for UDF:

1. Custom functions for runtime manipulation.
2. Example: Consider an Employee table with columns:

```
Name, Salary
Rupesh, 1000
Rahul, 200
```
If there is a need to add an increment of salary or bonus to the employee, then we need user-defined functions.

## Add UDF in Hive Shell

```sql
add file /tmp/hive_class/multiply_udf.py;
```

`multiply_udf.py` code:

```python
import sys

for line in sys.stdin:
    line = line.strip("\n\r")
    quantity = int(line)
    quantity = quantity * 10
    print(str(quantity))
```

### Statement to Execute Python UDF

```sql
SELECT TRANSFORM(year_id, quantityordered)
USING 'python multicol_python_udf.py'
AS (year_id STRING, square INT)
FROM sales_order_data_orc
LIMIT 5;
```
### Add Another UDF in Hive Shell

```sql
add file /tmp/hive_class/many_column_udf.py;
```

`many_column_udf.py` code:

```python
import sys

for line in sys.stdin:
    line = line.strip("\n\r")
    country, order_num, quantity = line.split("\t")  # Hive terminal prints data with tab separator although file is comma separated.
    order_num = int(order_num)
    quantity = int(quantity)
    quantity = quantity * 1000
    result = '\t'.join([country, str(order_num), str(quantity)])
    print(result)
```

### Statement to Execute Python UDF for Multiple Columns

```sql
SELECT TRANSFORM(country, ordernumber, quantityordered)
USING 'python many_column_udf.py'
AS (country STRING, ordernumber INT, multiplied_quantity INT)
FROM sales_order_data_orc LIMIT 10;
```

#### Bucketing - Optimizing Hive
Bucketing helps optimize join operations in Hive by reducing data shuffling over the network.

Note: Partitioning can be done independently, bucketing can be done independently, and both partitioning and bucketing can be done together.

#### Bucketing Explained:
Bucketing is used to create small chunks of data, or to create small buckets/clusters.

Learn with Example:
If, for instance, I have a big box containing Red, Blue, and Black balls, and I have 3 buckets - B0, B1, B2, with a mediator (one person).

Using the mediator, I will place the colored balls in their respective buckets (B0, B1, B2).

### Similarly, with data:

```
Name, Salary
ABC, 1000
RTYC, 2000
ER, 5000
```

Using a hash function - formula Len(name) % number of buckets determines the address of the field in a bucket.

Buckets: 3

So for ABC, Len(ABC) % 3 = 0. ABC field goes to Bucket B0.
Similarly, RTYC goes to B1, and ER goes to B2.

