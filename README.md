# Shopify-Fall-2022-Data-Science-Challenge

# Question 1: 
On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

**a.	Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.**

The high average order value of $3145.13 is calculated by taking the mean (average) of the ‘order_amount’ column over the given 30 day dataset. Since it includes outliers such as 704000, it skews the AOV value to be much higher than it should be. A better way to evaluate the AOV would be to omit the outliers by filtering them out using IQR methods, then calculating the AOV.

As the Shopify blog below suggests, looking at AOV only gives a partial picture of customer purchase behavior. It is best to consider all three measures of central tendency using mean, median, and mode (when logical and possible).

Reference: https://www.shopify.com.ng/blog/average-order-value#averageorder 

**b.	What metric would you report for this dataset?**

I would use the mean of the IQR dataset (middle 50%) since this symmetrically omits outliers from both sides (mins and maxs) that may otherwise skew the average order value (AOV).

**c.	What is its value?**

275.4 

(see .ipynb file for method)

# Question 2: 
For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

**a.	How many orders were shipped by Speedy Express in total?**

SELECT COUNT(OrderID) as 'Speedy Express Count' 

FROM Orders o

INNER JOIN Shippers s

ON o.ShipperID = s.ShipperID
   
WHERE s.ShipperName = 'Speedy Express'

**b.	What is the last name of the employee with the most orders?**

SELECT e.LastName, COUNT(*) AS NumOfOrders

FROM Orders o 

JOIN Employees e

ON o.EmployeeID = e.EmployeeID

GROUP BY e.LastName

ORDER BY NumOfOrders DESC

LIMIT 1

**c.	What product was ordered the most by customers in Germany?**

SELECT p.ProductName,

SUM(d.Quantity) AS OrderTotal

FROM Orders o

JOIN Customers c

ON c.CustomerID = o.CustomerID

JOIN OrderDetails d

ON d.OrderID = o.OrderID

JOIN Products p

ON p.ProductID = d.ProductID

WHERE c.Country = 'Germany'

GROUP BY d.ProductID

ORDER BY OrderTotal DESC

