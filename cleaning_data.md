What issues will you address by cleaning the data?
1. In the Products table: Replace 1 row NULL sentiment score and sentiment magnitude values with averages for each category.
2. In sales_by_sku table: Remove 8 rows contain SKUs not on the products table.
3. After removing unknown SKUs, sales_by_sku table is identical to the first two columns from sales_report table. Drop sales_by_sku
4. In analytics table: Delete duplicate rows
5. In analytics table: userid column only has NULL values. Delete this column
6. In analytics table: Convert revenue and unit price to dollars.
7. In analytics table: units_sold, bounces and revenue column change NULL values to 0. 
8. In all_sessions table: productrefundamount, itemquantity, itemrevenue, and searchkeyword columns are all NULL. The currencycode is only USD. Delete these columns.
9. In all_sessions table: Convert totaltransactionrevenue, productprice, productrevenue, transactionrevenue to dollars
10. In all_sessions table: 24 rows have no country and 8656 rows have no city. Change all these values to 'unknown'
11. In all_sessions table: v2productcategories is inconsistent. Consolidate and sort products


Queries:
Below, provide the SQL queries you used to clean your data.
1.
```SQL
UPDATE products_clean
SET sentimentscore = 
	(SELECT ROUND(AVG(sentimentscore)::numeric, 1) FROM products_clean)
    WHERE sentimentscore IS NULL;

UPDATE products_clean
SET sentimentmagnitude = 
	(SELECT ROUND(AVG(sentimentmagnitude)::numeric, 1) FROM products_clean)
	WHERE sentimentmagnitude IS NULL;
```
2.
```SQL
CREATE TABLE sales_by_sku_clean AS (
	SELECT *
	FROM sales_by_sku
	WHERE productsku IN (SELECT sku FROM products_clean);
)
```
3.
```SQL
DROP TABLE sales_by_sku_clean;
```

4.
```SQL
CREATE TABLE analytics_clean AS (SELECT DISTINCT * FROM analytics);
```
5.
```SQL
ALTER TABLE analytics_clean
DROP COLUMN userid;
```

6.
```SQL
UPDATE analytics_clean SET
	unit_price = (unit_price/1000000),
	revenue = (revenue/1000000);
```

7.
```SQL
UPDATE analytics_clean SET units_sold = 0
WHERE units_sold IS NULL;

UPDATE analytics_clean SET
	bounces = 0
	WHERE bounces IS NULL;

UPDATE analytics_clean SET
	revenue = 0
	WHERE revenue IS NULL;
```

8.
```SQL
ALTER TABLE all_sessions_clean
	DROP COLUMN productrefundamount,
	DROP COLUMN itemquantity,
	DROP COLUMN itemrevenue,
    DROP COLUMN searchkeyword,
    DROP COLUMN currencycode;
```

9.
```SQL
UPDATE all_sessions_clean SET
	totaltransactionrevenue = totaltransactionrevenue/1000000,
	productprice = productprice/1000000,
	productrevenue = productrevenue/1000000,
	transactionrevenue = transactionrevenue/1000000;
```

10.
```SQL
UPDATE all_sessions_clean
SET country = 'unknown'
WHERE country = '(not set)';

UPDATE all_sessions_clean
SET city = 'unknown'
WHERE city = '(not set)'
OR city = 'not available in demo dataset';
```

11.
```SQL
UPDATE all_sessions_clean SET v2productcategory = 
	CASE 
		WHEN v2productcategory ILIKE '%apparel%' THEN 'Apparel'
		WHEN v2productcategory ILIKE '%electronics%' THEN 'Electronics'
		WHEN v2productcategory ILIKE '%drinkware%' THEN 'Drinkware'
		WHEN v2productcategory ILIKE '%bags%' THEN 'Bags'
        WHEN v2productcategory ILIKE '%pet%' THEN 'Pet'
        WHEN v2productcategory ILIKE '%houseware%' THEN 'Housewares'
        WHEN v2productcategory ILIKE '%sport%' THEN 'Sports & Fitness'
		WHEN v2productname ILIKE '%bottle%' OR v2productname ILIKE '%mug%' OR v2productname ILIKE '%tumbler%' THEN 'Drinkware'
		WHEN v2productname ILIKE '%pen%' THEN 'Writing Instruments' 
		WHEN v2productname ILIKE '%journal%' OR v2productname ILIKE '%notebook%' THEN 'Notebooks & Journals'
		WHEN v2productname ILIKE '%bag%' OR v2productname ILIKE '%backpack%' OR v2productname ILIKE '%rucksack%' OR v2productname ILIKE '%tote%' THEN 'Bags'
		WHEN v2productname ILIKE '%sticker%' OR v2productname ILIKE '%decal%' THEN 'Stickers'
		WHEN v2productname ILIKE '%cap%' OR v2productname ILIKE '%hat%' THEN 'Apparel'
		WHEN v2productname ILIKE '%Men''s%' OR v2productname ILIKE '%toddler%' OR v2productname ILIKE '%infant%' THEN 'Apparel'
		WHEN v2productname ILIKE '%nest%' THEN 'Nest'
		WHEN v2productname ILIKE '%socks%' OR v2productname ILIKE '%hoodie%' OR v2productname ILIKE '%onesie%' OR v2productname ILIKE '%shirt%' OR v2productname ILIKE '%tee%' THEN 'Apparel'
		WHEN v2productname ILIKE '%speaker%' OR v2productname ILIKE '%headphone%' THEN 'Electronics'
        WHEN v2productname ILIKE '%charger%' OR v2productname ILIKE '%flashlight%' OR v2productname ILIKE '%power%' THEN 'Electronics'
        WHEN v2productname ILIKE '%dog%' OR v2productname ILIKE '%pet%' THEN 'Pet'
		WHEN v2productname ILIKE '%luggage%' OR v2productname ILIKE '%lunch kit%' THEN 'Housewares'
        WHEN v2productname ILIKE '%sunglas%' THEN 'Apparel'
		WHEN v2productname ILIKE '%ball%' OR v2productname ILIKE '%yoga%' THEN 'Sports & Fitness'
        WHEN v2productname ILIKE '%windup%' THEN 'Accessories'
		WHEN v2productname ILIKE '%selfie%' THEN 'Accessories'
   		WHEN v2productname ILIKE '%holder%' OR v2productname ILIKE '%stand%' OR v2productname ILIKE '%mount%' THEN 'Accessories'

		ELSE v2productcategory
	END
```