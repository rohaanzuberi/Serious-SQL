# 👨‍💻 Serious SQL

# 🔎 Data Exploration

## 📌 Select & Sort Data

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
![](Images/Exercise_2.jpeg)![Excercise_2](https://user-images.githubusercontent.com/103615594/172679760-870ee5f6-30d8-4da1-bc44-eece570c7e82.jpeg)


### Question 3. Who was the manager of the store WITH the highest total_sales IN the dvd_rentals.sales_by_store table?

```SQL
/* Sorting total sales in DESC and LIMIT 1 to show only the manager with highest total_sales */
SELECT  manager
       ,total_sales
FROM dvd_rentals.sales_by_store
ORDER BY total_sales DESC
LIMIT 1 -- to show only the manager with highest total_sales;
```
![](Images/Exercise_3.jpeg)

### Question 4. What is the postal_code of the city WITH the 5th highest city_id IN the dvd_rentals.address table?

```SQL
/* Sorting results to show CITY_ID in DESC order and LIMIT 5 to identify the 5th highest CITY_ID */
SELECT  postal_code
       ,city_id
FROM dvd_rentals.address
ORDER BY city_id DESC
LIMIT 5 -- to show only the top 5 cities WITH highest city_id;
```
![](Images/Exercise_4.jpeg)![Excercise_4](https://user-images.githubusercontent.com/103615594/172679577-10253965-a1b2-4e46-b826-02e537a9fdf4.jpeg)

In this case, the postal_code is 31390.


## 📌 Record Counts & Distinct Values

### Question 1. Which actor_id has the most number of unique film_id records in the dvd_rentals.film_actor table?

```SQL
SELECT  actor_id
       ,COUNT(DISTINCT film_id) AS unique_film
FROM dvd_rentals.film_actor
GROUP BY  actor_id
ORDER BY unique_film DESC
LIMIT 1;
```
![](Images/S2_Q1.jpeg)

### Question 2. How many distinct fid values are there for the 3rd most common price value in the dvd_rentals.nicer_but_slower_film_list table?

```SQL
/* Counting the distinct fid values and grouping by price, followed by ordering by descending values to identify the 3rd most common fid value */
SELECT  price
       ,COUNT(DISTINCT fid) AS fid_values
FROM dvd_rentals.nicer_but_slower_film_list
GROUP BY  price
ORDER BY fid_values DESC;
```
![](Images/S2_Q2.jpeg)

### Question 3. How many unique country_id values exist in the dvd_rentals.city table?

