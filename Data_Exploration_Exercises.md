# 👨‍💻 Serious SQL

## 🔎 Data Exploration

📌 Select & Sort Data

```SQL
/* Q1. What is the name of the category WITH the highest category_id IN the dvd_rentals.category table? */
SELECT  name
       ,category_id
FROM dvd_rentals.category
ORDER BY category_id DESC
LIMIT 1;
```
