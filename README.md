### Serious-SQL Course

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







