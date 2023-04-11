What are your risk areas? Identify and describe them.

There appear to be many transactions in the analytics table that are unaccounted for on the all_sessions table. These transactions could not be linked to a country or city, therefore have been excluded from answers related to geography. There are also many records in the all_sessions table that are missing location data. This means any sort of trends noted regarding sales by city or country have to be understood as an incomplete picture.

Additionally, there were contradictions between different revenue columns that made it unclear which was the most reliable. For the sake of these questions, I used the totaltransactionrevenue column from the all_sessions table. Similarly, between different tables, it was unclear what the most accurate accouting of units sold. I focused on the unit_sold column from analytics and cross referenced it with all_sessions to pinpoint the transactions.

Relying on any of this data is high risk. If the company would like to use any of this analysis, I would want to better understand the source of the data and clarify inconsistency before redoing my analysis.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

For the provided questions 1 and 5, I confirmed all cities/countries with revenue were appearing in the query and the range of revenue per transaction from each location did not exceed the totals I calculated.

```SQL
SELECT	city,
		MAX(totaltransactionrevenue),
		MIN(totaltransactionrevenue)
FROM all_sessions_clean
GROUP BY city
ORDER BY MAX(totaltransactionrevenue) DESC;

SELECT	country,
		MAX(totaltransactionrevenue),
		MIN(totaltransactionrevenue)
FROM all_sessions_clean
GROUP BY country
ORDER BY MAX(totaltransactionrevenue) DESC;
```

For question 2. I confirmed that all visitors that appear on both the analytics and all sessions tables have their cities/countries represented in the answer.

```SQL
SELECT	COUNT(DISTINCT city) AS cities,
		COUNT(DISTINCT country) AS countries
FROM all_sessions_clean
INNER JOIN analytics_clean USING(fullvisitorid)
```

For question 3. I checked overall top selling categories to see if anything stood out as contradicting my answer. It confirmed apparel was by far the biggest seller.

```SQL
SELECT 	v2productcategory,
		COUNT(*) AS frequency
FROM all_sessions_clean
GROUP BY v2productcategory
ORDER BY frequency DESC
```


For question 4, I tried to check my work by comparing what I found with the skus and order totals from the sales report table. 

```SQL
WITH sold_items AS (
	SELECT 	fullvisitorid,
			city,
			country,
			v2productname,
			productsku,
			units_sold
	FROM all_sessions_clean
	INNER JOIN analytics_clean USING(fullvisitorid)
	WHERE units_sold >= 1
	ORDER BY city, v2productname
	)
	
SELECT	productsku,
		SUM(units_sold)
FROM sold_items
GROUP BY productsku
ORDER BY productSKU
```