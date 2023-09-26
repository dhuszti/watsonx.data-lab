# watsonx.data-lab

## Download prerequisite files
Please download cars.csv from prerequsites folder. 

## Access the environment (6 people each)
team 1:
team 2:
team 3:
team 4:
team 5:

## Upload file via GUI
Upload a file and check how it is stored in Iceberg table format
1. Upload data via GUI - cars.csv
a.	Create new „my_schema” under „iceberg_catalog” (please use your name like "dhuszti")
b.	Create new „cars” table under this „my_schema”
c.	SQL (explain and run) - substitute "my_schema" with your name like "dhuszti": 
```
select car, avg(mpg) as avg_mpg from iceberg_data.**my_schema**.cars group by car order by car;
```
d.	Query history

## Federation
Unlike traditional database systems, Presto doesn’t have its own native database storage. Instead, Presto supports separation of compute and storage, with dozens of connectors that let Presto access data where it lives – which could be in relational databases, NoSQL databases, data warehouses, data lakes, data lakehouses, and more.
With Presto’s federated query capability (frequently referred to as data federation, or simply federation), enterprises can combine data from their existing, diverse sources with new data that they have in watsonx.data, rapidly unlocking insights without the complexity and cost of duplicating or moving data.
Although Presto supports a wide range of connectors, watsonx.data officially only supports a subset of them. This is because IBM wants to ensure quality, performance, and security of connectors before adding support (which may require updating connector source code to do this). More connectors will be added over time.
As of 3Q 2023, Presto in watsonx.data can currently connect to IBM Db2, Netezza, Apache Kafka, MySQL, PostgreSQL, and MongoDB, among others. The most current list of connectors and the types SQL statements supported can be found here. The list of supported connectors will grow over time.
In this section you will combine data from watsonx.data’s object storage with data in Db2 and PostgreSQL databases. To avoid you having to provision these databases yourself, they’ve been installed in the same VM as watsonx.data and are pre-populated with data.

### Write a query to get max expense for each employee (Db2 - fact table & Watsonx.data Hive table - dim table)
1. Copy the below SQL to SQL editor and run it
```
SELECT
  EMP_DIM.employee_name,
  MAX(EMP_FACT.expense_total) as max_expense
FROM
  "db2_catalog"."gosalesdw"."emp_employee_dim" as EMP_DIM
  JOIN "hive_data"."gosalesdw"."emp_expense_fact" as EMP_FACT ON EMP_DIM."employee_key" = EMP_FACT."employee_key"
GROUP BY EMP_DIM.employee_name
ORDER BY max_expense DESC
```

### This sample query could be used by the fictional business to determine which purchasing method is associated with the largest orders. (Db2 & Watsonx.data Hive table).
1. Copy the below SQL to SQL editor and run it
```
select
  pll.product_line_en as product,
  md.order_method_en as order_method,
  sum(sf.quantity) as total
from
  db2_catalog.gosalesdw.sls_order_method_dim as md,
  db2_catalog.gosalesdw.sls_product_dim as pd,
  db2_catalog.gosalesdw.sls_product_line_lookup as pll,
  hive_data.gosalesdw.sls_product_brand_lookup as pbl,
  hive_data.gosalesdw.sls_sales_fact as sf
where
  pd.product_key = sf.product_key
  and md.order_method_key = sf.order_method_key
  and pll.product_line_code = pd.product_line_code
  and pbl.product_brand_code = pd.product_brand_code
group by
  pll.product_line_en,
  md.order_method_en
order by
  product,
  order_method;
```
   
##  Move data from external Db2 database to Iceberg table
1. If not yet created then please create new „my_schema” under „iceberg_catalog” (please use your name like "dhuszti")
2. Copy the below SQL to SQL editor and replace "my_schema" to your schema like "dhuszti"
 
```
create table iceberg_data.**my_schema**.emp_employee_dim as
select
  *
from
  db2_catalog.gosalesdw.emp_employee_dim;
```
3. Run query

##  Timetravel query
1. If not yet created then please create new „my_schema” under „iceberg_catalog” (please use your name like "dhuszti")
2. Copy the below SQL to SQL editor and replace "my_schema" to your schema like "dhuszti"

```
-- create a sample table to show time travel
create table iceberg_data.**my_schema**.airport_id as select * from hive_data.ontime.airport_id;
```

```
-- get snapshot id
select * from iceberg_data.**my_schema**."airport_id$snapshots" order by committed_at;
```
```
insert into iceberg_data.my_schema.airport_id values (10000, 'North Pole: Reindeer Field');
```
```
select * from iceberg_data.my_schema.airport_id where code = 10000;
```
```
select count(*) from iceberg_data.my_schema.airport_id;
```
```
-- rollback_to_snapshot
call iceberg_data.system.rollback_to_snapshot ('**my_schema**', 'airport_id', <snapshotID>);
```   
