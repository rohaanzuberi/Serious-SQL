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
![](images/Exercise_1.jpg)
