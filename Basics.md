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


#### Course 2 Questions 

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









