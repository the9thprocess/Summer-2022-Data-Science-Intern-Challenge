# Summer-2022-Data-Science-Intern-Challenge
Shopify Data Science Internship Challenge
Summer 2022 Data Science Intern Challenge 

Please complete the following questions, and provide your thought process/work. You can attach your work in a text file, link, etc. on the application page. Please ensure answers are easily visible for reviewers!


Question 1: Given some sample data, write a program to answer the following: click here to access the required data set <br />

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis.  <br />

a.Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.  <br />
<b>Answer:</b> <br />
Total revenue generated: 15725640 <br />
Count of items: 5000 <br />
Average order value: 3145.128 <br />
Therefore, the error in calculating total items results in error. Instead of summing the value of items, count was used. <br />

b.What metric would you report for this dataset? <br />
Answer: <br />
To correctly calculate AOV (average order value) we need to divide the total revenue in the time period with the total number of items sold. That is, AOV = total revenue/total items. <br />

c.What is its value? <br />
Answer: <br />
The correct value is 357.92 <br />


Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below. <br />

a.How many orders were shipped by Speedy Express in total? <br />

CREATE VIEW orders_shipped AS <br />
SELECT Orders.OrderID, Orders.ShipperID, Shippers.ShipperName <br />
FROM Orders  <br />
INNER JOIN Shippers <br />
ON Shippers.ShipperID=Orders.ShipperID; <br />
SELECT COUNT(*) FROM [orders_shipped] <br />
WHERE ShipperName = 'Speedy Express'; <br />

Answer= 54 <br />


b.	What is the last name of the employee with the most orders? <br />
CREATE VIEW Employee_Orders AS  <br />
SELECT Orders.EmployeeID, Employees.LastName, Orders.OrderID <br />
FROM Orders <br />
INNER JOIN Employees <br />
ON Employees.EmployeeID=Orders.EmployeeID;  <br />

SELECT LastName, COUNT(*) <br />
FROM Employee_Orders <br />
GROUP BY LastName <br />
ORDER BY COUNT(*) desc; <br />

Answer = Peacock, 40 <br />
c.	What product was ordered the most by customers in Germany? <br />
CREATE VIEW Products_Ordered AS <br />
SELECT Orders.OrderID, Customers.Country, OrderDetails.Quantity, Products.ProductName <br />
FROM Orders, OrderDetails <br />
JOIN Customers ON Orders.CustomerID=Customers.CustomerID <br />
JOIN Products ON OrderDetails.ProductID=Products.ProductID <br />
WHERE Country='Germany'; <br />

CREATE VIEW Product_Orders AS <br />
SELECT ProductName, Quantity, COUNT(*) as 'Orders' <br />
FROM Products_Ordered <br />
GROUP BY ProductName; <br />

SELECT ProductName, Quantity, Orders, (Quantity * Orders) AS TotalOrdered <br />
FROM Product_Orders <br />
ORDER BY TotalOrdered desc <br />
LIMIT 1; <br />
Answer: <br />
ProductName: Camembert Pierrot <br />
Quantity: 40 <br />
Orders: 300 <br />
TotalOrdered: 12000 <br />
