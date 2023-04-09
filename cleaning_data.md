What issues will you address by cleaning the data?
1. In the Products table: Replace 1 row NULL sentiment score and sentiment magnitude values with averages for each category.
2. In sales_by_sku table: Remove 8 rows contain SKUs not on the products table.
3. After removing unknown SKUs, sales_by_sku table is identical to the first two columns from sales_report table. Drop sales_by_sku
4. In analytics table: Delete duplicate rows
5. In analytics table: userid column only has NULL values. Delete this column
6. In analytics table: Convert revenue and unit price to dollars.
7. In analytics table: units_sold, bounces and revenue column change NULL values to 0. 
8. In all_sessions table: productrefundamount, itemquantity, itemrevenue, and searchkeyword columns are all NULL. The currencycode is only USD. Delete these columns.
9. in all_sessions table: Convert totaltransactionrevenue, productprice, productrevenue, transactionrevenue to dollars



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
	WHERE productsku IN (SELECT sku FROM products_clean);
)

3.
DROP TABLE sales_by_sku_clean;

4.
CREATE TABLE analytics_clean AS (SELECT DISTINCT * FROM analytics);

5.
ALTER TABLE analytics_clean
DROP COLUMN userid;

6.
UPDATE analytics_clean SET
	unit_price = (unit_price/1000000),
	revenue = (revenue/1000000);

7.
UPDATE analytics_clean SET units_sold = 0
WHERE units_sold IS NULL;

UPDATE analytics_clean SET
	bounces = 0
	WHERE bounces IS NULL;

UPDATE analytics_clean SET
	revenue = 0
	WHERE revenue IS NULL;

8.
ALTER TABLE all_sessions_clean
	DROP COLUMN productrefundamount,
	DROP COLUMN itemquantity,
	DROP COLUMN itemrevenue,
    DROP COLUMN searchkeyword,
    DROP COLUMN currencycode

9.
UPDATE all_sessions_clean SET
	totaltransactionrevenue = totaltransactionrevenue/1000000,
	productprice = productprice/1000000,
	productrevenue = productrevenue/1000000,
	transactionrevenue = transactionrevenue/1000000