What are your risk areas? Identify and describe them.

There appear to be many transactions in the analytics table that are unaccounted for on the all_sessions table. These transactions could not be linked to a country or city, therefore have been excluded from answers related to geography.
Additionally, there were contradictions between different revenue columns that made it unclear which was the most reliable. For the sake of these questions, I used the totaltransactionrevenue column from the all_sessions table.




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


