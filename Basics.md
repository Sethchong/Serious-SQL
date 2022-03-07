### Course 1 Questions

#### Q1: What is the name of the category with the highest category_id in the dvd_rentals.category table?

````SQL
SELECT
  name,
  category_id
FROM dvd_rentals.category
ORDER BY category_id desc
````

#### Q2: For the films with the longest length, what is the title of the “R” rated film with the lowest replacement_cost in dvd_rentals.film table?

Order by length because we want to identify the longest length, and since we want lowest replacement_cost too, that is why also need to ```ORDER BY``` it too
````SQL
select title, replacement_cost, rating, length
from dvd_rentals.film
order by length DESC, replacement_cost
limit 10;
````

#### Q3: Who was the manager of the store with the highest total_sales in the dvd_rentals.sales_by_store table?

````SQL
select manager, total_sales 
from dvd_rentals.sales_by_store
order by total_sales DESC
````

#### Q4: What is the postal_code of the city with the 5th highest city_id in the dvd_rentals.address table?

````SQL
select postal_code, city_id 
from dvd_rentals.address
order by city_id DESC
limit 5;
````

##### When we want to explore our datasets, one handy tip is to find out how many rows are we dealing with: 
````SQL
SELECT
  COUNT(*) AS row_count
FROM dvd_rentals.film_list;
````

```GROUP BY```
One way to think of the GROUP BY is to imagine our dataset being divided into different groups based off the values of selected columns.
So for ```GROUP BY```, you have to use with aggregate functions such as COUNT, SUM, MEAN, MAX, MIN 
this also results in the SQL returning 1 row per group


### Course 2 Questions 

#### Q1:Which actor_id has the most number of unique film_id records in the dvd_rentals.film_actor table?

so note that you cannot rename a column and try to ORDER BY the new name. Unless its a CTE or temp table. 
````SQL
select actor_id, COUNT(DISTINCT film_id)
FROM dvd_rentals.film_actor
group by actor_id
order by COUNT(DISTINCT film_id) DESC
limit 10;
````

### with cte alternative

````SQL
WITH temp_table as (
select actor_id, COUNT(DISTINCT film_id) as unique
FROM dvd_rentals.film_actor
GROUP BY actor_id
order by 2 DESC
)

select * 
from temp_table
````

#### Q2: How many distinct fid values are there for the 3rd most common price value in the dvd_rentals.nicer_but_slower_film_list table?

````SQL
select price, count(distinct fid)
from dvd_rentals.nicer_but_slower_film_list
group by 1
order by 2 DESC
limit 10;
````

#### Q3: How many unique country_id values exist in the dvd_rentals.city table?

````SQL
select count(distinct country_id)
from dvd_rentals.city
limit 10;
````

#### Q4: What percentage of overall total_sales does the Sports category make up in the dvd_rentals.sales_by_film_category table?

so in this case, ROUND() is to round the value to 2 decimal places. 
Also, the ::NUMERIC is the short form of casting. 
The long form is ```CAST(column_name AS <new-data-type>)```


````SQL
select category, ROUND( 100* total_sales::NUMERIC / SUM(total_sales) OVER(), 2) as percentage
from dvd_rentals.sales_by_film_category
limit 10
````
![image](https://user-images.githubusercontent.com/67274884/152634615-cf03fd11-5b87-421d-8a10-455c046a54b8.png)


ok so to give a little bit more context as to why we are using OVER() here. 
if we remove OVER(), we have to use GROUP BY, and recall that GROUP BY will return 1 per row.
````SQL
select category, total_sales, ROUND( 100* total_sales::NUMERIC / SUM(total_sales), 2) as percentage
from dvd_rentals.sales_by_film_category
group by 1,2
order by 2 desc
````
![image](https://user-images.githubusercontent.com/67274884/152634604-7d391042-07b6-4b93-abf8-6f2658380339.png)


### <a href="https://www.youtube.com/watch?v=7yp_QYBCEP4"> Youtube link that explains more on OVER()</a>

  
#### Q5: What percentage of unique fid values are in the Children category in the dvd_rentals.film_list table?

````SQL
SELECT
  category,
  ROUND(
    100 * COUNT(DISTINCT fid)::NUMERIC / SUM(COUNT(DISTINCT fid)) OVER (),
    2
  ) AS percentage
FROM dvd_rentals.film_list
GROUP BY category
ORDER BY category;
````
ROUND () code is to round the numbers to 2 decimal places and the code within count is to calculate percentage 

#### Notes

Another quick tip: 
```` SQL
SELECT COUNT(DISTINCT ID)
FROM health.user_logs;

````
From the above code, Distinct ID means we want to know how many distinct ID and COUNT is to count it 

### How to deal with duplicates

Before we continue - we have to alwways check for duplicates - so we can write the most basic one such as COUNT(*)
However, there are times when we cannot write COUNT(DISTINCT (*)) so we can also write a subquery: 
```` SQL
SELECT COUNT(*)
FROM (
  SELECT DISTINCT * 
  FROM health.user_logs
) AS subquery

````
in the above code, we want to have all the distinct rows first then we can count it 

### Common Table Expression (CTE) 
The most basic idea for CTE is to imagine we are changing the existing data and storingthe data outputs as a new reference - very similar to storing data in a new temporary excel sheet

```` SQL
WITH deduped_logs AS (
  SELECT DISTINCT *
  FROM health.user_logs
)
SELECT COUNT(*)
FROM deduped_logs;

````


### Course 3 Questions

#### Q1 Highest Duplicate 
Which id value has the most number of duplicate records in the health.user_logs table?

Note that in this case - we cannot do COUNT(DISTINCT *) in Postgre SQL 

So we can use a subquery to get the same effect

```` SQL
WITH groupby AS(
  select id, log_date, measure, measure_value, systolic, diastolic, count(*) AS numbers
  from health.user_logs
  group by id, log_date, measure, measure_value, systolic, diastolic
)

select id, sum(numbers) as duplicate_rows 
from groupby
where numbers >1
group by id 
order by duplicate_rows desc
limit 10;
````

so in the above codes - we want to write a cte to count how many duplicates in a for each row 

then we want to sum the duplicates up and only select those > 1 and then we will be able to find out which id has the most duplicates 


#### Q2 Second Highest Duplicate 
Which log_date value had the most duplicate records after removing the max duplicate id value from question 1?

#### Q3 Highest Occuring Value
Which measure_value had the most occurences in the health.user_logs value when measure = 'weight'?

#### Q4 Single and Total Duplicated Rows 
How many single duplicated rows exist when measure = 'blood_pressure' in the health.user_logs? How about the total number of duplicate records in the same table?

#### Q5 Percentage of Table Records
What percentage of records measure_value = 0 when measure = 'blood_pressure' in the health.user_logs table? How many records are there also for this same condition?


#### Q6 Percentage of Duplicates
What percentage of records are duplicates in the health.user_logs table?











