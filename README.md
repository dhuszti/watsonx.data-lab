# watsonx.data-lab

1. Download prerequisite files
2. Access the environment
3. Upload a file and check how it is stored in Iceberg table format
Upload data via GUI - cars.csv
a.	Create new „my_schema” under „iceberg_catalog”
b.	Create new „cars” table under this „my_schema”
c.	SQL (explain and run): 
i.	select car, avg(mpg) as avg_mpg from iceberg_data.my_schema.cars group by car order by car;
d.	Query history
![image](https://github.com/dhuszti/watsonx.data-lab/assets/11091479/c3ededfc-6fb2-4e52-a248-17e050940968)

4. 
