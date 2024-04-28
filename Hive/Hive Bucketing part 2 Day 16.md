# Hive Bucketing Part 2


## Bucket Map join:

**Users**

- B1: 
  - 1, Shashank
  - 2, Amit
- B2: 
  - 3, Yadav
  - 4, Sunil
- B3: 
  - 5, Kranti
  - 6, Manohar
- B4: 
  - 8, Chandra

**Location**

- B1: 
  - 3, MP
  - 2, Bihar
  - 1, UP
  - 4, AP
- B2: 
  - 7, GOA
  - 5, Maharastra
  - 6, Jharkand

So my mapper will have B1.user (M1) and B2.user(M2).

B1.location can be sent to M1 ( B1.user ) which performs required join operation for me. Since B1.location data is small, that need to be sent to mapper class.

**Condition:** Dataset should be disturbed evenly, i.e., number of buckets should be multiple of each other in both datasets.

**Example:**

- D1 = 4 buckets
- D2 = 2 or 4 buckets

### Implementation:

```sql
SET hive.optimize.bucketmapjoin=true;
SET hive.auto.convert.join=true;

SELECT * FROM buck_users u INNER JOIN buck_locations l ON u.id = l.id;
```


### Sorted merge Bucket Map join:
#### Sort Location side buckets

Users

B1:
1, Shashank
2, Amit
B2:
3, Yadav
4, Sunil
B3:
5, Kranti
6, Manohar
B4:
8, Chandra
Location

B1:
1, UP
2, Bihar
3, MP
4, AP
B2:
5, Maharastra
6, Jharkand
7, GOA

### Implementation:

```sql

SET hive.enforce.sortmergebucketmapjoin=false;
SET hive.auto.convert.sortmerge.join=true;
SET hive.optimize.bucketmapjoin = true;
SET hive.optimize.bucketmapjoin.sortedmerge = true;

SET hive.auto.convert.join=false;
SELECT * FROM buck_users u INNER JOIN buck_locations l ON u.id = l.id;
```