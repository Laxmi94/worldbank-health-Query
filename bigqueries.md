
### 1. Expenditure ($) on health by countries in the year 2018.
```sql
SELECT 
    country_name AS Country,
    ROUND(value, 2) AS Health_Expenditure_Dollar
FROM
    `bigquery-public-data.world_bank_health_population.health_nutrition_population`
WHERE
    indicator_name = 'Current health expenditure per capita, PPP (current international $)'
        AND year = 2018
ORDER BY Health_Expenditure_Dollar DESC
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/70090019/131223420-40d33e6f-3055-4f67-ba83-83d62a11a99e.png)

### 2. List of ten countries in the world with high unemployment rate.
```sql
SELECT 
    country_name AS Country,
    AVG(value) AS Unemployment_percent_avg
FROM
    `bigquery-public-data.world_bank_health_population.health_nutrition_population`
WHERE
    indicator_name = 'Unemployment, total (% of total labor force)'
GROUP BY country_name , indicator_name
ORDER BY Unemployment_percent_avg DESC
LIMIT 10;
```
![image](https://user-images.githubusercontent.com/70090019/131223448-f43e785b-66cb-449f-b605-f80f47c20b9e.png)

### 3. List of countries had external health expenditure is greater than the average of the external health expenditure in the year 2018.
```sql
SELECT DISTINCT
    country_name, ROUND(value, 4) AS external_health_exp
FROM
    `bigquery-public-data.world_bank_health_population.health_nutrition_population`
WHERE
    indicator_name = 'External health expenditure per capita (current US$)'
        AND value > (SELECT 
            AVG(value)
        FROM
            `bigquery-public-data.world_bank_health_population.health_nutrition_population`
        WHERE
            indicator_name = 'External health expenditure per capita (current US$)') AND year = 2018
ORDER BY external_health_exp DESC;
```
![image](https://user-images.githubusercontent.com/70090019/131223603-916ffcba-b520-448f-800d-330dd0c0bf08.png)

### 4. Explanation of Indicator names with what aggregation method is used to calculate the value of indicator.
```sql
SELECT DISTINCT
    (A.country_name),
    A.indicator_name,
    B.aggregation_method,
    B.short_definition
FROM
    `bigquery-public-data.world_bank_health_population.health_nutrition_population` A
        INNER JOIN
    `bigquery-public-data.world_bank_health_population.series_summary` B ON A.indicator_name = B.indicator_name;
 ```
 ![image](https://user-images.githubusercontent.com/70090019/131223681-e3ac813f-8f71-4cf9-9238-0b03a48be801.png)

### 5. Source for the collected data.
```sql
SELECT DISTINCT
    (A.indicator_name), A.year, S.source
FROM
    `bigquery-public-data.world_bank_health_population.health_nutrition_population` A
        JOIN
    `bigquery-public-data.world_bank_health_population.series_summary` S ON A.indicator_name = S.indicator_name
ORDER BY year DESC;
```
![image](https://user-images.githubusercontent.com/70090019/131223883-62e32cea-0031-43df-8457-cbc29f25f914.png)







