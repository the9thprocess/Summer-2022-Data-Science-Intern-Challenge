# Summer-2022-Data-Science-Intern-Challenge
Shopify Data Science Internship Challenge
Summer 2022 Data Science Intern Challenge 

Please complete the following questions, and provide your thought process/work. You can attach your work in a text file, link, etc. on the application page. Please ensure answers are easily visible for reviewers!


Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

a.Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 
Answer:
Total revenue generated: 15725640
Count of items: 5000
Average order value: 3145.128
Therefore, the error in calculating total items results in error. Instead of summing the value of items, count was used.

b.What metric would you report for this dataset?
Answer:
To correctly calculate AOV (average order value) we need to divide the total revenue in the time period with the total number of items sold. That is, AOV = total revenue/total items.

c.What is its value?
Answer:
The correct value is 357.92


Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

a.How many orders were shipped by Speedy Express in total?

CREATE VIEW orders_shipped AS
SELECT Orders.OrderID, Orders.ShipperID, Shippers.ShipperName
FROM Orders 
INNER JOIN Shippers
ON Shippers.ShipperID=Orders.ShipperID;
SELECT COUNT(*) FROM [orders_shipped]
WHERE ShipperName = 'Speedy Express';

Answer= 54


b.	What is the last name of the employee with the most orders?
CREATE VIEW Employee_Orders AS 
SELECT Orders.EmployeeID, Employees.LastName, Orders.OrderID
FROM Orders
INNER JOIN Employees
ON Employees.EmployeeID=Orders.EmployeeID; 

SELECT LastName, COUNT(*)
FROM Employee_Orders
GROUP BY LastName
ORDER BY COUNT(*) desc;

Answer = Peacock, 40
c.	What product was ordered the most by customers in Germany?
CREATE VIEW Products_Ordered AS
SELECT Orders.OrderID, Customers.Country, OrderDetails.Quantity, Products.ProductName
FROM Orders, OrderDetails
JOIN Customers ON Orders.CustomerID=Customers.CustomerID
JOIN Products ON OrderDetails.ProductID=Products.ProductID
WHERE Country='Germany';

CREATE VIEW Product_Orders AS
SELECT ProductName, Quantity, COUNT(*) as 'Orders'
FROM Products_Ordered
GROUP BY ProductName;

SELECT ProductName, Quantity, Orders, (Quantity * Orders) AS TotalOrdered
FROM Product_Orders
ORDER BY TotalOrdered desc
LIMIT 1;

Answer:
ProductName: Camembert Pierrot
Quantity: 40
Orders: 300
TotalOrdered: 12000
