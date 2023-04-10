Question 1: Is there any correlation between time on site or page views and untis sold?

SQL Queries:
WITH time_and_views AS (
	SELECT	visitid,
			timeonsite,
			pageviews, 
			SUM(units_sold) AS totalUnitsSold
	FROM analytics_clean
	GROUP BY visitid, timeonsite, pageviews
	)

SELECT 	CORR(timeonsite, totalUnitsSold) AS timeCorr,
		CORR(pageviews, totalUnitsSold) AS viewsCorr
FROM time_and_views 

Answer: 
Both time on site and number of page views have very weak correlation with number of units sold per visit. Page views has a slightly higher coefficient at 0.14, but it is still too weak to be considered a major factor in whether or not a sale is made.



Question 2: Is there any pattern to what days of the week or times of year get the most web traffic?

SQL Queries:
# Visits by day
WITH visit_by_date AS (
	SELECT	date,
		EXTRACT(DOW FROM date) AS day,
		EXTRACT(MONTH FROM date) AS month,
		COUNT(*) AS visits
	FROM all_sessions_clean
	GROUP BY date, day
	)
SELECT	CASE
			WHEN day = 0 THEN 'Sunday'
			WHEN day = 1 THEN 'Monday'
			WHEN day = 2 THEN 'Tuesday'
			WHEN day = 3 THEN 'Wednesday'
			WHEN day = 4 THEN 'Thursday'
			WHEN day = 5 THEN 'Friday'
			WHEN day = 6 THEN 'Saturday'
		END AS day,
		SUM(visits) AS totalvisits,
		AVG(visits) AS avgvisits
FROM visit_by_date
GROUP BY day
ORDER BY totalvisits;

# Visits by month
WITH visit_by_date AS (
	SELECT	date,
		EXTRACT(DOW FROM date) AS day,
		EXTRACT(MONTH FROM date) AS month,
		COUNT(*) AS visits
	FROM all_sessions_clean
	GROUP BY date, day
	)
SELECT	month,
		SUM(visits) AS totalvisits
FROM visit_by_date
GROUP BY month
ORDER BY totalvisits;

Answer:
Weekdays get over 50% more traffic than weekends. Saturdays have the fewest visits and Wednesdays have the most.
The month with the most visits is August with over twice as many visits as October, the month with the least traffic. September and August stick out as having significantly more traffic than the other months. With only one year's data, it is difficult to generalize this pattern beyond the given year.


Question 3: What percentage of the site traffic is repeat visitors? 

SQL Queries: 
WITH repeat_visitors AS (
		SELECT 	fullvisitorid,
				COUNT(DISTINCT visitid) AS visits
		FROM all_sessions_clean
		GROUP BY fullvisitorid
		HAVING COUNT(DISTINCT visitid) > 1
	),
	one_time_visitors AS (
		SELECT 	fullvisitorid,
				COUNT(DISTINCT visitid) AS visits
	FROM all_sessions_clean
	GROUP BY fullvisitorid
	HAVING COUNT(DISTINCT visitid) = 1
	)

SELECT (SELECT COUNT(*) FROM repeat_visitors) AS repeat,
(SELECT COUNT(*) FROM one_time_visitors) AS onetime,
((SELECT COUNT(*) FROM repeat_visitors)+(SELECT COUNT(*) FROM one_time_visitors)) AS total

Answer:
Repeat visitors make up 1.89% of traffic.


Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
