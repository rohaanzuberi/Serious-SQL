# 🔎 Dataset Exploration

For context, this real world messy dataset captures data taken from individuals logging their measurements via an online portal throughout the day.

For example, multiple measurements can be taken on the same day at different times, but certain information is missing as the log_date column does not show timestamp values!

The dataset consists of two tables - `user_logs` and `users`. For this section the `user_log` table was used.

| Field | Type |
| --- | --- |
| id | varchar |
| log_date | date |
| measure | text |
| measure_value | numeric |
| systolic | numeric |
| diastolic | numeric |

Following are the activities carried out to explore the messy dataset:

## 📌 Data Inspection

Before dealing with any duplicate data it is important to understand the dataset by carrying out inspection.

```SQL
SELECT *
FROM health.user_logs
LIMIT 10;
```
![](Images/S3_P1.jpeg)

### Record Counts

Checking the count of the data

```SQL
SELECT COUNT(*)
FROM health.user_logs;
```
![](Images/S3_P2.jpeg)

### Unique Column Counts

Using the `COUNT (DISTINCT)` function to identify the unique ID values

```SQL
SELECT COUNT(DISTINCT id)
FROM health.user_logs;
```
![](Images/S3_P3.jpeg)

### Single Column Frequency Counts

Inspecting the `measure` column to understand the frequency of measure values

```SQL
SELECT
  measure,
  COUNT(*) AS frequency,
  ROUND(
    100 * COUNT(*) / SUM(COUNT(*)) OVER (), 2)
    AS percentage
  FROM health.user_logs
  GROUP BY measure
  ORDER BY frequency DESC;
  ```
  ![](Images/S3_P4.jpeg)
  
  Inspecting the `id` column as well
  
  ```SQL
  SELECT
    id,
    COUNT(*) AS frequency,
    ROUND(
      100 * COUNT(*) / SUM(COUNT(*)) OVER(), 2) AS percentage
    FROM health.user_logs
    GROUP BY id
    ORDER BY percentage DESC
    LIMIT 10;
  ```
  ![S3_P5](https://user-images.githubusercontent.com/103615594/172887349-7102b325-3997-4ba7-95eb-062e61d24703.jpeg)

  
  ## 📌 Individual Column Distributions
  
  Identifying the most recurring values within each column
  
  ### Measure Column
  
  ```SQL
  SELECT
  measure_value,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY measure_value
ORDER BY frequency DESC
LIMIT 5;
```
![](Images/S3_P6.jpeg)

### Systolic Column

```SQL
SELECT
  systolic,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY systolic
ORDER BY frequency DESC
LIMIT 5;
```
![](Images/S3_P7.jpeg)

### Diastolic Column

```SQL
SELECT
  diastolic,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY diastolic
ORDER BY frequency DESC
LIMIT 5;
```
![](Images/S3_P8.jpeg)

### Deeper dive into specific values

As it can be seen that there are many 0 values there for the `measure_value` field and null values for both `systolic` and `diastolic` columns.

To inspect these rows a bit further - a use of `WHERE` filter will be utilized to check if this only happens for certain measure values when the condition `measure_value = 0` is met and the `systolic` and `diastolic`columns are `NULL`.

```SQL
SELECT
  measure,
  COUNT(*)
FROM health.user_logs
WHERE measure_value = 0
GROUP BY measure
LIMIT 5;
```
![](Images/S3_P9.jpeg)

When comparing the results with the frequency percentage calculated above it can be seen that most `measure_value = 0` are occurring when the `measure = blood_pressure` as 562 out of 2417 entries are 0. While `measure = blood_glucose` has 8 `measure_value = 0` values out of 38692 entries and `measure = weight` has 2 entries with `measure_value = 0` out of 2782 frequency value.

Digging in further for `measure = blood_pressure` to inspect the values:

```SQL
SELECT *
FROM health.user_logs
WHERE measure_value = 0
AND measure = 'blood_pressure'
LIMIT 10;
```
![](Images/S3_P10.jpeg)

It is understood that when `measure_value = 0` then values for `systolic` and `diastolic` are captured with valid information.

But what if `measure_value != 0`, the query was altered to obtain the results for further inspection:

```SQL
SELECT *
FROM health.user_logs
WHERE measure_value != 0
AND measure = 'blood_pressure'
LIMIT 10;
```
![](Images/S3_P11.jpeg)

It is apparent that `systolic` values are sometimes recorded as `measure_value` which is sometimes recorded as 0.

The final step in this inspection stage would be to analyze the `NULL` values in `systolic` and `diastolic` columns using the WHERE statement:

```SQL
SELECT
  measure,
  COUNT(*)
FROM health.user_logs
WHERE systolic IS NULL
GROUP BY measure
LIMIT 10;
```
![](Images/S3_P12.jpeg)

Looking at the result it is confirmed that there are NO-NULL records for `systolic` only when `measure = blood_pressure`.

```SQL
SELECT
  measure,
  COUNT(*)
FROM health.user_logs
WHERE diastolic IS NULL
GROUP BY measure
LIMIT 10;
```
![](Images/S3_P12.jpeg)

The results confirm the observations.

## 🗣 TL;DR in the form of sharing insights with colleague through a short email

> ***"Rohaan, I need your help urgently - we just received this user_logs dataset, can you let me know what you find?"***

> Dear Colleague,
> 
> Following are the analysis findings on the `user_logs` dataset:
> - The data consists of the following fields:
> 	- Patient id as `id`
> 	- `log_date` for entries
> 	- `measure` records consisting of blood_glucose, blood_pressure and weight data
> 	- `measure_value` for the corresponding measure
> 	- Record of `systolic` value
> 	- Record of `diastolic` value
> - There are **43891 records** within the dataset with entries on **554 different patients**
> - The most recorded `measure` is **blood_glucose** with **38692 entries** i.e. **88.15%** of total entries
> - The most frequent `measure_value` is `0` at **572 entries**
> - `systolic` field has **26023** `NULL` and **15451** `0` value entries
> - `diastolic` field consists of **26023** `NULL` and **15449** `0` value entries
> - Findings indicate that most of the `measure_value = 0` happen when `measure = blood_pressure`.
> - For cases where **`measure_value != 0`** then `systolic` values are recorded within the `measure_value` field. This finding is substantiated with the fact that `systolic` and `diastolic` fields only have non-null records when `measure = blood_pressure`.
> 
> Best Regards,
> 
> Rohaan

  ## 📌 Detecting Duplicate Records
  
  ```SQL
  WITH deduped_log AS (
  SELECT DISTINCT *
  FROM health.user_logs)
SELECT COUNT(*)
FROM deduped_log;
```
![](Images/S3_P13.jpeg)

It can be seen that there are duplicate records as the original table had 43891 count of records, however only 31004 are distinct.


```SQL
WITH groupby_counts AS (
  SELECT
    id,
    log_date,
    measure,
    measure_value,
    systolic,
    diastolic,
    COUNT(*) AS frequency
  FROM health.user_logs
  GROUP BY
    id,
    log_date,
    measure,
    measure_value,
    systolic,
    diastolic)
SELECT *
FROM groupby_counts
WHERE frequency > 1 -- filtering for duplicate entries only
ORDER BY frequency DESC
LIMIT 10;
```
![](Images/S3_P14.jpeg)

From the outcome it can be seen that there are multiple entries that have been duplicated.

In the context of this exploration, the duplicates will not be removed as it may be possible that they are valid data points captured with certain missing information such as the timestamp within the `log_date` column.
