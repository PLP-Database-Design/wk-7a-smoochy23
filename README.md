SELECT
    OrderID,
    CustomerName,
    TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(Products, ',', n), ',', -1)) AS Product
FROM
    ProductDetail
CROSS JOIN
    (SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3) AS numbers
ON
    LENGTH(Products) - LENGTH(REPLACE(Products, ',', '')) >= numbers.n - 1;
    Okay, let's tackle these normalization challenges!

Question 1: Achieving 1NF

Here's the SQL query to transform the ProductDetail table into 1NF:

SQL

SELECT
    OrderID,
    CustomerName,
    TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(Products, ',', n), ',', -1)) AS Product
FROM
    ProductDetail
CROSS JOIN
    (SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3) AS numbers
ON
    LENGTH(Products) - LENGTH(REPLACE(Products, ',', '')) >= numbers.n - 1;
Explanation:

CROSS JOIN (SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3) AS numbers: This creates a temporary table with numbers 1, 2, and 3. We assume a maximum of 3 products per order in this example. You might need to adjust the number of UNION ALL statements if your data has more products per order.
SUBSTRING_INDEX(Products, ',', n): This extracts the first n products from the Products string, delimited by commas.
SUBSTRING_INDEX(..., ',', -1): This then extracts the last product from the result of the previous SUBSTRING_INDEX, effectively isolating the n-th product.
TRIM(...): This removes any leading or trailing whitespace from the extracted product name.
ON LENGTH(Products) - LENGTH(REPLACE(Products, ',', '')) >= numbers.n - 1: This condition ensures that we only generate a row for each product that actually exists in the Products string. It counts the number of commas to determine how many products are present.
Resulting Table (in 1NF):

OrderID	CustomerName	Product
101	John Doe	Laptop
101	John Doe	Mouse
102	Jane Smith	Tablet
102	Jane Smith	Keyboard
102	Jane Smith	Mouse
103	Emily Clark	Phone
Okay, let's tackle these normalization challenges!

Question 1: Achieving 1NF

Here's the SQL query to transform the ProductDetail table into 1NF:

SQL

SELECT
    OrderID,
    CustomerName,
    TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(Products, ',', n), ',', -1)) AS Product
FROM
    ProductDetail
CROSS JOIN
    (SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3) AS numbers
ON
    LENGTH(Products) - LENGTH(REPLACE(Products, ',', '')) >= numbers.n - 1;
Explanation:

CROSS JOIN (SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3) AS numbers: This creates a temporary table with numbers 1, 2, and 3. We assume a maximum of 3 products per order in this example. You might need to adjust the number of UNION ALL statements if your data has more products per order.
SUBSTRING_INDEX(Products, ',', n): This extracts the first n products from the Products string, delimited by commas.
SUBSTRING_INDEX(..., ',', -1): This then extracts the last product from the result of the previous SUBSTRING_INDEX, effectively isolating the n-th product.
TRIM(...): This removes any leading or trailing whitespace from the extracted product name.
ON LENGTH(Products) - LENGTH(REPLACE(Products, ',', '')) >= numbers.n - 1: This condition ensures that we only generate a row for each product that actually exists in the Products string. It counts the number of commas to determine how many products are present.
Resulting Table (in 1NF):

OrderID	CustomerName	Product
101	John Doe	Laptop
101	John Doe	Mouse
102	Jane Smith	Tablet
102	Jane Smith	Keyboard
102	Jane Smith	Mouse
103	Emily Clark	Phone

Export to Sheets
Question 2: Achieving 2NF

Here are the SQL queries to transform the OrderDetails table into 2NF:

Step 1: Create a new table for Customer Information:

SQL

CREATE TABLE Customers AS
SELECT DISTINCT OrderID, CustomerName
FROM OrderDetails;
Step 2: Create a new table for Order Items:

SQL

CREATE TABLE OrderItems AS
SELECT OrderID, Product, Quantity
FROM OrderDetails;
Explanation:

CREATE TABLE Customers AS SELECT DISTINCT OrderID, CustomerName FROM OrderDetails;: This query creates a new table named Customers. It selects the distinct OrderID and CustomerName combinations from the original OrderDetails table. Here, OrderID will be the primary key of the Customers table.

CREATE TABLE OrderItems AS SELECT OrderID, Product, Quantity FROM OrderDetails;: This query creates a new table named OrderItems. It selects the OrderID, Product, and Quantity from the original OrderDetails table. In this table, the primary key will be a composite key of (OrderID, Product). The OrderID here acts as a foreign key referencing the Customers table.

Resulting Tables (in 2NF):

Customers Table:

OrderID	CustomerName
101	John Doe
102	Jane Smith
103	Emily Clark

OrderItems Table:

OrderID	Product	Quantity
101	Laptop	2
101	Mouse	1
102	Tablet	3
102	Keyboard	1
102	Mouse	2
103	Phone	1

CREATE TABLE OrderItems AS
SELECT OrderID, Product, Quantity
FROM OrderDetails;
esulting Tables (in 2NF):

Customers Table:

OrderID	CustomerName
101	John Doe
102	Jane Smith
103	Emily Clark

OrderItems Table:

OrderID	Product	Quantity
101	Laptop	2
101	Mouse	1
102	Tablet	3
102	Keyboard	1
102	Mouse	2
103	Phone	1

