Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT city,
		SUM(totaltransactionrevenue)
FROM all_sessions_clean
WHERE totaltransactionrevenue IS NOT NULL AND 
CITY != 'unknown'
GROUP BY city
ORDER BY SUM(totaltransactionrevenue) DESC
LIMIT 3;

SELECT country,
		SUM(totaltransactionrevenue)
FROM all_sessions_clean
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY country
ORDER BY SUM(totaltransactionrevenue) DESC
LIMIT 3;


Answer:
City:
[San Francisco, 1564.32
 Sunnyvale,     992.23
 Atlanta,       854.44]

Country:
[United States, 13154.17
Israel,         602
Australia,      358]



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
WITH unit_sales AS (
	SELECT	fullvisitorid,
			city,
			country,
			SUM(units_sold) AS productTotal
	FROM all_sessions_clean AS al
	INNER JOIN analytics_clean AS an USING(fullvisitorid)
	GROUP BY fullvisitorid, city, country
	ORDER BY producttotal DESC
)

SELECT city, ROUND(AVG(producttotal), 2)
FROM unit_sales
GROUP BY city
ORDER by city;


WITH unit_sales AS (
	SELECT	fullvisitorid,
			city,
			country,
			SUM(units_sold) AS productTotal
	FROM all_sessions_clean AS al
	INNER JOIN analytics_clean AS an USING(fullvisitorid)
	GROUP BY fullvisitorid, city, country
	ORDER BY producttotal DESC
)

SELECT country, ROUND(AVG(producttotal), 2) AS averageunitssold
FROM unit_sales
GROUP BY country
ORDER by country;


Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







