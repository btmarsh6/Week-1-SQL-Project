What issues will you address by cleaning the data?
1. In the Products table: Missing sentiment score and sentiment magnitude values for 1 row.




Queries:
Below, provide the SQL queries you used to clean your data.
1.
UPDATE products_clean
SET sentimentscore = 
	(SELECT AVG(sentimentscore) FROM products_clean)
    WHERE sentimentscore IS NULL;

UPDATE products_clean
SET sentimentmagnitude = 
	(SELECT AVG(sentimentmagnitude) FROM products_clean)
	WHERE sentimentmagnitude IS NULL;
