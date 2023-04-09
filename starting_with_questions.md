Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT city,
		SUM(totaltransactionrevenue)
FROM all_sessions_clean
WHERE totaltransactionrevenue IS NOT NULL AND 
CITY != 'not available in demo dataset'
GROUP BY city
ORDER BY SUM(totaltransactionrevenue) DESC;

SELECT country,
		SUM(totaltransactionrevenue)
FROM all_sessions_clean
WHERE totaltransactionrevenue IS NOT NULL
GROUP BY country
ORDER BY SUM(totaltransactionrevenue) DESC
LIMIT 1;


Answer:
City: San Francisco - $1,564.32
Country: United States - $13,154.17




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



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







