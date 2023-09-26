# watsonx.data-lab

1. Download prerequisite files
2. Access the environment
3. Upload a file and check how it is stored in Iceberg table format
Upload data via GUI - cars.csv
a.	Create new „my_schema” under „iceberg_catalog”
b.	Create new „cars” table under this „my_schema”
c.	SQL (explain and run): 
```select car, avg(mpg) as avg_mpg from iceberg_data.my_schema.cars group by car order by car;```
d.	Query history

4. Federation - Copy this 
```SELECT
  EMP_DIM.employee_name,
  MAX(EMP_FACT.expense_total) as max_expense
FROM
  "db2_catalog"."gosalesdw"."emp_employee_dim" as EMP_DIM
  JOIN "hive_data"."gosalesdw"."emp_expense_fact" as EMP_FACT ON EMP_DIM."employee_key" = EMP_FACT."employee_key"
GROUP BY EMP_DIM.employee_name
ORDER BY max_expense DESC```

5. Move data from external Db2 database to Iceberg table
```create table iceberg_data.wxgosalesdw.emp_employee_dim as
select
  *
from
  db2_catalog.gosalesdw.emp_employee_dim;```

   
