### Serious-SQL Course

### Course 1 Questions

#### What is the name of the category with the highest category_id in the dvd_rentals.category table?

````SQL
SELECT
  name,
  category_id
FROM dvd_rentals.category
ORDER BY category_id desc
````

#### For the films with the longest length, what is the title of the “R” rated film with the lowest replacement_cost in dvd_rentals.film table?

Order by length because we want to identify the longest length, and since we want lowest replacement_cost too, that is why also need to ```ORDER BY``` it too
````SQL
select title, replacement_cost, rating, length
from dvd_rentals.film
order by length DESC, replacement_cost
limit 10;
````
