# watsonx.data-lab

## Download prerequisite files
Please download cars.csv from prerequsites folder. 

## Access the environment (6 people each)
• team 1: https://na4.techzone-services.com:31554/
• team 2: https://na4.techzone-services.com:40236/
• team 3: https://na4.techzone-services.com:44077/
• team 4: https://na4.techzone-services.com:26615/
• team 5: https://na4.techzone-services.com:30707/

## Infrastructure overview of Watsonx.data
1. Go to top left corner click on four lines
   
3. Select Infrastructure Manager
   
4. The Infrastructure manager page opens with a graphical canvas view of the different infrastructure components currently defined in this watsonx.data environment: Engines (blue layer), Catalogs (purple layer), Buckets (green layer), and Databases (also blue, but not shown).
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/e20d294f-1086-4a8e-998e-65768de68d21)

Each bucket is associated with a catalog (with a 1:1 mapping). When a bucket is added to watsonx.data, a catalog is created for it at the same time, based on input from the user. Likewise, if a database connection is added (for federation purposes), a catalog is created for that database connection as well. Both of these activities will be shown later in the lab. Each catalog is then associated with one or more engines. An engine can’t access data in a bucket or a remote database unless the corresponding catalog is associated with the engine.

6. Let us add a new bucket by clicking to top right corner
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/6fde9e54-aad1-401a-a6b7-9f81dd6f2a18)

7. Click on bucket
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/06eea249-2c1f-4c54-9316-212eee70af4b)

8. Fill values accordingly:
• Bucketname: cos-watsonx-data-user1 or cos-watsonx-data-user2 or cos-watsonx-data-user3 or cos-watsonx-data-user4 or cos-watsonx-data-user5 or cos-watsonx-data-user6 (please select the one respective to your previosly selected username)
• Url: https://s3.eu-de.cloud-object-storage.appdomain.cloud
• Access key: 7830f4f34e514848ad3141e196ce4e79
• Secret key: 6984cfb2ac8164dd1f290f32e525986b35776ff67566c9de
• Catalog type: Apache Iceberg
• Catalog name: iceberg_**your_name** (for instance iceberg_dhuszti)
• Active: Activate it now

8. Click on Add button
   
9. Now you need to associate your iceberg_**your_name** catalog to Presto engine. Move your cursor to your catalog and click on Manage Associations
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/d0878d9d-2024-40ee-a7e2-1e927cc2aaad)

10. Select Presto and click Restart engine
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/40d66d25-8ffb-4107-89ab-d8dba479049b)

12. You should be able to see that your iceberg_**your_name** catalog is now connected to engine. You can start using it.  

## Upload file via GUI
Upload a file and check how it is stored in Iceberg table format
a.	Go to the top of the left navigation pane and click the Create dropdown menu. Select Create schema. Create new „my_schema” under „iceberg_catalog” (please use your name like "dhuszti")
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/bfa1611f-b862-4ceb-a867-bb94f44b7e8d)
In the Create schema pop-up window, select/enter the following information, and then click the Create button.
• Catalog: iceberg_data
• Name: **my_schema**
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/c65f032c-f51c-4d3e-b6e0-fad17430c0c8)
Expand the iceberg_data catalog. The new schema should be listed (but contains no tables).
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/d916b032-eaf8-4bd1-b971-5e462906023d)
Click the Create dropdown menu again but this time select Create table from file.
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/a6d491ee-ccb1-4614-9558-d653befd9f52)
The Create table from file workflow allows you to upload a small (maximum 2 MB file size) .csv, .parquet, .json, or .txt file to define and populate a new table.

Download the sample cars.csv file to your desktop (link to file: https://ibm.box.com/v/data-cars-csv).

For the Source, click Drag and drop a file or click to upload. Locate the cars.csv file you downloaded in the previous step and select it for upload (or simply drag and drop the file into this panel).
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/514d5117-c5da-453c-bb61-b1a8c86ea10f)
Scroll down to view a sample of the data uploaded. The schema of the table is inferred from the data in the file. Click Next.
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/074ef713-e018-4c5a-8918-ca7fb46a7625)
For the Target, select/enter the following information (some fields are pre-populated and cannot be changed). Once filled in, click Next.
• Engine: presto-01
• Catalog: iceberg_data
• Schema: **my_schema**
• Table name: cars
• Table format: Apache Iceberg
• Data format: Parquet

![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/7c9607fa-f2de-4da6-ab1d-ac24d35c1c80)

Scroll down to review the Summary, which includes the Data Definition Language (DDL) that will be used to create the table. You have an opportunity to alter the DDL statement if you wish, but do not change anything for this lab. Click Create to create the table.

Navigate to your new table: iceberg_data > my_schema > cars.
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/09a040c1-a2d5-4eed-9ab6-a19d2e86cb0b)

###Query Workspace Page
Databases and query engines such as Presto have multiple ways that users can interact with the data. For example, there is usually an interactive command line interface (CLI) that lets users run SQL statements from a command terminal. Also, remote applications can use JDBC (Java Database Connectivity) to connect to the data store and run SQL statements.
The watsonx.data user interface includes an SQL interface for building and running SQL statements. This is called the Query workspace. Users can write or copy in their own SQL statements, or they can use templates to assist in building new SQL statements.

Select the Query workspace () icon from the left-side menu. The Query workspace page opens with a data objects navigation pane on the left side and a SQL editor (workspace) pane on the right side.
Like in the Data manager page, the top of the navigation pane directs you to select an engine to use. It is this engine that will be used to run the SQL statements entered here. The only engine that exists in this lab environment is the presto-01 Presto engine, which is selected by default.
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/b3af7044-e58a-4944-95ea-f8de52f2a052)

Copy and paste below SQL (replace my_schema to your own schema) and run it
```
select car, avg(mpg) as avg_mpg from iceberg_data.**my_schema**.cars group by car order by car;
```

## Federation
Unlike traditional database systems, Presto doesn’t have its own native database storage. Instead, Presto supports separation of compute and storage, with dozens of connectors that let Presto access data where it lives – which could be in relational databases, NoSQL databases, data warehouses, data lakes, data lakehouses, and more.
With Presto’s federated query capability (frequently referred to as data federation, or simply federation), enterprises can combine data from their existing, diverse sources with new data that they have in watsonx.data, rapidly unlocking insights without the complexity and cost of duplicating or moving data.
Although Presto supports a wide range of connectors, watsonx.data officially only supports a subset of them. This is because IBM wants to ensure quality, performance, and security of connectors before adding support (which may require updating connector source code to do this). More connectors will be added over time.
As of 3Q 2023, Presto in watsonx.data can currently connect to IBM Db2, Netezza, Apache Kafka, MySQL, PostgreSQL, and MongoDB, among others. The most current list of connectors and the types SQL statements supported can be found here. The list of supported connectors will grow over time.
In this section you will combine data from watsonx.data’s object storage with data in Db2 and PostgreSQL databases. To avoid you having to provision these databases yourself, they’ve been installed in the same VM as watsonx.data and are pre-populated with data.

![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/39bcb637-d7bb-4f18-91d3-ef21c0116925)

To make this tutorial faster there is an already configured Db2 (running in IBM Cloud) added to Presto as federated database.

Select the Query workspace () icon from the left-side menu. The Query workspace page opens with a data objects navigation pane on the left side and a SQL editor (workspace) pane on the right side.

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
   
##  Offloading Data from a Data Warehouse
The previous section described the federated query capability of watsonx.data and Presto. In addition to it making it easy to access data across the enterprise, federation also opens the door to significant cost-savings for clients.

Many enterprises maintain expensive data warehouses, whether they are on-premises or in the cloud. These warehouses support mission-critical workloads with low-latency and high-performance requirements that justify their high costs. However, there is also often data in these warehouses that are supporting less important or less stringent workloads. This could be infrequently accessed (cold) data being kept for audit purposes, or data used for business intelligence, reporting, and data science. It makes financial sense for this data to be offloaded to a lower-cost solution, but keeping it in the warehouse has just been the easier path to take. The result, though, is periodic scaling of the data warehouse to support growing workloads, which can be very expensive.

With watsonx.data, enterprises can offload data that really doesn’t need to be in the warehouse to lower-cost object storage. With fit-for-purpose engines, data warehousing costs can be reduced by offloading workloads from a data warehouse to watsonx.data. Specifically, applications that need to access this data can query it through Presto (or Spark). This includes being able to combine the offloaded data with the data that remains in the warehouse. This section will show an example of how this can be done with Db2 (but the steps are equivalent for other data warehouse products).

Additionally, external engines that support the Iceberg open table format can also work directly with data in watsonx.data’s object storage. For example, Db2 and Netezza have been enhanced to read from and write to the Iceberg table format (not all deployment options for Db2 and Netezza currently include this support; as of 3Q 2023 it is limited to Db2 Warehouse Gen3 on AWS and Netezza Performance Server as a Service on AWS). This means that Db2 and Netezza can participate in the watsonx.data ecosystem as well, accessing the lakehouse data in object storage directly (just as Presto and Spark can). This, however, is beyond the scope of this lab.
In the previous section you registered an existing Db2 environment with watsonx.data (calling it Db2DW) and created a catalog for it called db2catalog.
In this example scenario, you’re going to “move” the gosalesdw.sls_sales_fact table from Db2 to watsonx.data’s object storage. It will be created as an Iceberg table, in a new schema you create called wxgosalesdw, managed by the iceberg_data catalog.

To create a new table with the same table definition as the original table, you can use the CREATE TABLE AS SELECT (CTAS) SQL statement.

1. Copy the below SQL to SQL editor and replace "my_schema" to your schema like "dhuszti"
 
```
create table iceberg_data.**my_schema**.emp_employee_dim as
select
  *
from
  db2_catalog.gosalesdw.emp_employee_dim;
```
2. Run query

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
