# 👨‍💻 Serious SQL

## 🔎 Data Exploration

📌 Select & Sort Data

### Question 1. What is the name of the category WITH the highest category_id IN the dvd_rentals.category table?

```SQL
/* Using ORDER BY DESC to sort the data and LIMIT function to show only the highest outcome */
SELECT  name
       ,category_id
FROM dvd_rentals.category
ORDER BY category_id DESC
LIMIT 1;
```
![](Images/Exercise_1.jpeg)

### Question 2. For the films with the longest length, what is the title of the “R” rated film with the lowest replacement_cost in dvd_rentals.film table?

```SQL
/* Filter results by 'R' rating, sort results for longest to shortest length and least to most replacement cost */
SELECT  title
       ,replacement_cost
       ,length
FROM dvd_rentals.film
WHERE rating = 'R'
ORDER BY length DESC, replacement_cost
LIMIT 1;
```
![](Images/Exercise_2.jpeg)
