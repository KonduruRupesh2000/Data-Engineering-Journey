# Hive Tables



## Hive
- Create DB
- Create Table
- Load the data to HIVE

## Tables
### Internal Table (Managed Table):
- Metadata will be maintained by HIVE
- Actual data will be stored in HIVE warehouse
  - Example: `HDFS` ---> `Load in HIVE` ---> `dump in warehouse`

### External Table:
- Metadata will be maintained by HIVE
- Actual data will be at source location (means Point to source data i.e. HDFS, here we don’t load to HIVE)
  - Example: `HDFS` ---> `HIVE`

## Load data in HIVE
Two ways:
1. Load from Local
2. Load from HDFS

HIVE takes raw data and stores it in the data warehouse (rows and cols).
