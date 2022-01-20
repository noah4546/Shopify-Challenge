# Summer 2022 Data Science Intern Challenge

> Noah Tomkins - 1tomkinsnoa@gmail.com

Shopify - January 19th 2022

---

## Question 1

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one
model of shoe. We want to do some analysis of the average order value (AOV). When
we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13.
Given that we know these shops are selling sneakers, a relatively affordable item,
something seems wrong with our analysis.

> a. Think about what could be going wrong with our calculation. Think about a better
way to evaluate this data.
---
> b. What metric would you report for this dataset?
---
> c. What is its value?

## Question 1 Answer

> a. The AOV of $3135.13 seems too high. The issue I found is that there seems to be some orders that have a very high `order_amount` this seems to cause the issue. What I did to get a better metric is to calculate the average item price, and store in the dataset as `avg_item_price` this was able to give me a better idea of what people are spending, but this is still done on the mean. When I calculated the mean of `avg_item_price` I got the value of what the average spent per shoe is across the whole dataset being $387.74
>
> I also then calculated the median of both the `avg_item_price` and the `order_amount` giving me the result better result. For the `avg_item_price` median I got $153.00, and for `order_amount` I got $284.00. Which I think better represents the data.
>
> In addition to that I thought that you could split the orders by their avg item price, and at the end of the `checkingData.ipynb` file you can see that with the group of `avg_item_price` being between 100 to 200, which is the most popular, the average customer spends $302.24 per purchase.
---
> b. Assuming that at the end of this evaluation of data you only want one single value to wrap up the whole dataset I would use the median of `order_amount`, but I think that having multiple filtered metrics would make better sense, to allow the shop owners to better understand their customer basis.
---
> c. The median `order_amount` is $284.00

---

## Question 2

For this question you’ll need to use SQL. Follow this link to access the data set
required for the challenge. Please use queries to answer the following questions. Paste your
queries along with your final numerical answers below.

> a. How many orders were shipped by Speedy Express in total?
---
> b. What is the last name of the employee with the most orders?
---
>c. What product was ordered the most by customers in Germany

## Question 2 Answer

> a. There are `54` orders shipped by `Speedy Express`

```sql
SELECT count(*) 
FROM orders o
JOIN shippers s
ON o.ShipperID = s.ShipperID
WHERE s.ShipperName = 'Speedy Express'
```

> b. The last name of the employee with the most orders is `Peacock`.

```sql
SELECT e.LastName 
FROM orders o
JOIN employees e
ON o.EmployeeID = e.EmployeeID
GROUP BY e.EmployeeID
ORDER BY count(*) DESC
Limit 1
```

> c. The product ordered most by customers in Germany is `Boston Crab Meat`

```sql
SELECT p.ProductName
FROM orders o
JOIN customers c ON o.CustomerID = c.CustomerID
JOIN orderDetails od ON o.OrderID = od.OrderID
JOIN products p ON od.ProductID = p.ProductID
WHERE c.Country = 'Germany'
GROUP BY p.ProductID
ORDER BY SUM(od.Quantity) DESC
Limit 1
```
