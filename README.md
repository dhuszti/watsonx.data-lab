# watsonx.data-lab

1. Download prerequisite files
2. Access the environment
3. Upload a file and check how it is stored in Iceberg table format
Upload data via GUI - cars.csv
a.	Create new „my_schema” under „iceberg_catalog”
b.	Create new „cars” table under this „my_schema”
c.	SQL (explain and run) - substitute "my_schema" with your name like "dhuszti": 
```
select car, avg(mpg) as avg_mpg from iceberg_data.my_schema.cars group by car order by car;
```
d.	Query history

*** Federation
Unlike traditional database systems, Presto doesn’t have its own native database storage. Instead, Presto supports separation of compute and storage, with dozens of connectors that let Presto access data where it lives – which could be in relational databases, NoSQL databases, data warehouses, data lakes, data lakehouses, and more.
With Presto’s federated query capability (frequently referred to as data federation, or simply federation), enterprises can combine data from their existing, diverse sources with new data that they have in watsonx.data, rapidly unlocking insights without the complexity and cost of duplicating or moving data.
Although Presto supports a wide range of connectors, watsonx.data officially only supports a subset of them. This is because IBM wants to ensure quality, performance, and security of connectors before adding support (which may require updating connector source code to do this). More connectors will be added over time.
As of 3Q 2023, Presto in watsonx.data can currently connect to IBM Db2, Netezza, Apache Kafka, MySQL, PostgreSQL, and MongoDB, among others. The most current list of connectors and the types SQL statements supported can be found here. The list of supported connectors will grow over time.
In this section you will combine data from watsonx.data’s object storage with data in Db2 and PostgreSQL databases. To avoid you having to provision these databases yourself, they’ve been installed in the same VM as watsonx.data and are pre-populated with data.

Get max expense for each employee (Db2 & Hive table)

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

6. Federation complex example (Db2 & Hive table): 
-- This sample query could be used by the fictional business to determine 
-- which purchasing method is associated with the largest orders.
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

5. Move data from external Db2 database to Iceberg table
Create a new schema under Iceberg 
```
create table iceberg_data.wxgosalesdw.emp_employee_dim as
select
  *
from
  db2_catalog.gosalesdw.emp_employee_dim;
```
6. 

   
