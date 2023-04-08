What issues will you address by cleaning the data?
1. In the Products table: Replace 1 row NULL sentiment score and sentiment magnitude values with averages for each category.
2. In sales_by_sku table: Remove 8 rows contain SKUs not on the products table.
3. After removing unknown SKUs, sales_by_sku table is identical to the first two columns from sales_report table. Drop sales_by_sku
4. In analytics table: Delete duplicate rows



Queries:
Below, provide the SQL queries you used to clean your data.
1.
UPDATE products_clean
SET sentimentscore = 
	(SELECT ROUND(AVG(sentimentscore)::numeric, 1) FROM products_clean)
    WHERE sentimentscore IS NULL;

UPDATE products_clean
SET sentimentmagnitude = 
	(SELECT ROUND(AVG(sentimentmagnitude)::numeric, 1) FROM products_clean)
	WHERE sentimentmagnitude IS NULL;

2.
CREATE TABLE sales_by_sku_clean AS (
	SELECT *
	FROM sales_by_sku
	WHERE productsku IN (SELECT sku FROM products_clean)
)

3.
DROP TABLE sales_by_sku_clean

4.
CREATE TABLE analytics_clean AS (SELECT DISTINCT * FROM analytics)

5.

