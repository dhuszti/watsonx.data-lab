# watsonx.data-lab

## Download prerequisite files
Please download cars.csv from prerequsites folder. 

## Access the environment (6 people each)
team 1:
team 2:
team 3:
team 4:
team 5:

## Infrastructure overview of Watsonx.data
1. Go to top left corner click on four lines
2. Select Infrastructure Manager
3. The Infrastructure manager page opens with a graphical canvas view of the different infrastructure components currently defined in this watsonx.data environment: Engines (blue layer), Catalogs (purple layer), Buckets (green layer), and Databases (also blue, but not shown).
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/e20d294f-1086-4a8e-998e-65768de68d21)
Each bucket is associated with a catalog (with a 1:1 mapping). When a bucket is added to watsonx.data, a catalog is created for it at the same time, based on input from the user. Likewise, if a database connection is added (for federation purposes), a catalog is created for that database connection as well. Both of these activities will be shown later in the lab. Each catalog is then associated with one or more engines. An engine can’t access data in a bucket or a remote database unless the corresponding catalog is associated with the engine.

4. Let us add a new bucket by clicking to top right corner
   ![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/6fde9e54-aad1-401a-a6b7-9f81dd6f2a18)
5. Click on bucket
   ![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/06eea249-2c1f-4c54-9316-212eee70af4b)
6. Fill values accordingly:
Bucketname: cos-watsonx-data-user1 or cos-watsonx-data-user2 or cos-watsonx-data-user3 or cos-watsonx-data-user4 or cos-watsonx-data-user5 or cos-watsonx-data-user6 (please select the one respective to your previosly selected username)
Url: https://s3.eu-de.cloud-object-storage.appdomain.cloud
Access key: 7830f4f34e514848ad3141e196ce4e79
Secret key: 6984cfb2ac8164dd1f290f32e525986b35776ff67566c9de
Catalog type: Apache Iceberg
Catalog name: iceberg_**your_name** (for instance iceberg_dhuszti)
Active: Maybe later
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/26733521-6a42-4d7b-8333-1e6407510a46)

8. Click on Add button
9. Now you need to associate your iceberg_**your_name** catalog to Presto engine. Move your cursor to your catalog and click on Manage Associations
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/d0878d9d-2024-40ee-a7e2-1e927cc2aaad)
10. Select Presto and click Restart engine
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/40d66d25-8ffb-4107-89ab-d8dba479049b)
11. You should be able to see that your iceberg_**your_name** catalog is now connected to engine. You can start using it.  

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

![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/39bcb637-d7bb-4f18-91d3-ef21c0116925)

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
##  Optimize performance - use partitioning

create table iceberg_data.my_schema.customer as select * from tpch.tiny.customer;

-- Run a simple scan query which selects customer names and market segment.
select name, mktsegment from iceberg_data.workshop.customer limit 3;

-- Run this EXPLAIN or show via Explain button
explain select name, mktsegment from iceberg_data.workshop.customer;

-- Create partitioned table (afterwards show the MinIO)
create table iceberg_data.workshop.part_customer 
  with (partitioning = array['mktsegment']) 
  as select * from tpch.tiny.customer;

select * from iceberg_data."workshop".part_customer where mktsegment='MACHINERY';

create table iceberg_data.workshop.orders as 
  select * from tpch.tiny.orders;

-- Use a Windowing function  
SELECT 
   orderkey, clerk, totalprice, 
   rank() OVER (PARTITION BY clerk ORDER BY totalprice DESC) AS rnk 
FROM 
   iceberg_data.workshop.orders 
ORDER BY 
   clerk, rnk;  
   
select * from iceberg_data.workshop.customer where mktsegment='FURNITURE';

select * from iceberg_data.workshop.part_customer where mktsegment='FURNITURE';
