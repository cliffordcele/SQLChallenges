# Introduction

Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

# Problem Statement

Danny wants to use the data to answer a few simple questions about his customers. Having a deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

# Data

Danny has shared with you 3 key datasets for this case study:
* **SALES:** captures all [customer_id]() level purchases with an corresponding [order_date]() and [product_id]() information for when and what menu items were ordered.
* **MENU:** maps the [product_id]() to the actual [product_name]() and price of each menu item.
* **MEMBERS:** captures the [join_date]() when a [customer_id]() joined the beta version of the Danny’s Diner loyalty program.

# Entity Relationship Diagram

<p align="center">
  <img src="ER_Diagram.png" alt="ER_Diagram" width="600"/>
</p>


# Case Study Solutions

1. **What is the total amount each customer spent at the restaurant?**
* Left join the data from the menu table to the sales table using the product_id as the link between the two tables.
* Group the orders made in the sales table by the customer_id.
* Sum the price for each order by customer_id to obtain the total_spent by customer.
```SQL
SELECT s.customer_id, 
	SUM(m.price) AS total_spent
FROM dannys_diner.sales s
	LEFT JOIN dannys_diner.menu m 
	ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id;
```
| customer_id | total_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |
---


2. **How many days has each customer visited the restaurant?**
* Group the orders made in the sales table by the customer_id.
* Use the distinct count function on order_date to count the number of unique dates per customer.

```SQL
SELECT customer_id, 
       COUNT(DISTINCT order_date)
FROM dannys_diner.sales
GROUP BY customer_id
ORDER BY customer_id;
```
| customer_id | count |
| ----------- | ----- |
| A           | 4     |
| B           | 6     |
| C           | 2     |
---


3. **What was the first item from the menu purchased by each customer?**
* Left join the data from the menu table to the sales table using the product_id as the link between the two tables.
* Sort the table by the order_date and then by customer_id to find the earliest purchases by customer.
* Note one, customers A and C ordered more than one item during their first purchase.
* Note two, the query below will return all the orders made sorted by date and customer. The table below is a subset of the full table.
```SQL
SELECT s.customer_id, 
       s.order_date, 
       m.product_name
FROM dannys_diner.sales s
	LEFT JOIN dannys_diner.menu m 
	ON s.product_id = m.product_id
ORDER BY s.order_date, 
	 s.customer_id;
```
| customer_id | order_date               | product_name |
| ----------- | ------------------------ | ------------ |
| A           | 2021-01-01T00:00:00.000Z | sushi        |
| A           | 2021-01-01T00:00:00.000Z | curry        |
| B           | 2021-01-01T00:00:00.000Z | curry        |
| C           | 2021-01-01T00:00:00.000Z | ramen        |
| C           | 2021-01-01T00:00:00.000Z | ramen        |
---


4. **What is the most purchased item on the menu and how many times was it purchased by all customers?**
* Left join the data from the menu table to the sales table using the product_id as the link between the two tables.
* Count the number of times each product_id appears in the sales table 
* Sort the joined table by the purchase_count of each product_id from greatest to least
* Limit the output to the top product_name purchased
```SQL
SELECT s.product_id, 
       m.product_name, 
       COUNT(s.product_id) AS purchase_count
FROM dannys_diner.sales s
	LEFT JOIN dannys_diner.menu m 
	ON s.product_id = m.product_id
GROUP BY s.product_id, m.product_name
ORDER BY purchase_count DESC
LIMIT 1;
```
| product_id | product_name | purchase_count |
| ---------- | ------------ | -------------- |
| 3          | ramen        | 8              |
---


5. **Which item was the most popular for each customer?**
* Left join the data from the menu table to the sales table using the product_id as the link between the two tables.
* Parse this table and count the number of times each item was ordered by each customer  
* Sort thi
* Create a rank variable that w
* Ramen was the most popular item for customers A and C. Customer B ordered each item the same amount.
```SQl
SELECT
    tab.customer_id, tab.product_name, tab.cnt
FROM 
	(SELECT s.customer_id, 
		m.product_name, 
        	COUNT(*) AS cnt,
        	RANK() OVER (PARTITION BY s.customer_id ORDER BY COUNT(*) DESC) AS rnk
	FROM dannys_diner.sales s
		LEFT JOIN dannys_diner.menu m 
		ON s.product_id = m.product_id
	 GROUP BY s.customer_id, m.product_name
	 ) AS tab
WHERE rnk = 1;
```
| customer_id | product_name | cnt |
| ----------- | ------------ | --- |
| A           | ramen        | 3   |
| B           | ramen        | 2   |
| B           | curry        | 2   |
| B           | sushi        | 2   |
| C           | ramen        | 3   |
---



6. **Which item was purchased first by the customer after they became a member?**
*
*
*
```SQL
SELECT
    tab.customer_id, tab.order_date, tab.product_name, tab.join_date
FROM (
	SELECT s.customer_id, s.order_date, m.product_name, mbr.join_date,
	RANK() OVER (PARTITION BY s.customer_id ORDER BY s.order_date ASC) AS rnk
	FROM dannys_diner.sales s
	JOIN dannys_diner.menu m
  		ON s.product_id = m.product_id
	JOIN dannys_diner.members mbr
  		ON s.customer_id = mbr.customer_id
	WHERE order_date > join_date 
    GROUP BY s.customer_id, s.order_date, m.product_name, mbr.join_date
	ORDER BY s.order_date, s.customer_id
	) AS tab
WHERE rnk = 1;
```


| customer_id | order_date               | product_name | join_date                |
| ----------- | ------------------------ | ------------ | ------------------------ |
| A           | 2021-01-10T00:00:00.000Z | ramen        | 2021-01-07T00:00:00.000Z |
| B           | 2021-01-11T00:00:00.000Z | sushi        | 2021-01-09T00:00:00.000Z |
---


7. **Which item was purchased just before the customer became a member?**
* This is the same code that was used to answer Question 6. The only changes made are 
* 	1) The WHERE statement is used to filter out orders made after a customer became a member 
* 	2) that in the RANK() function, the ordering is reversed to DESC so that the order_date closest to the join_date has a rank of 1. 
```SQL
SELECT
    tab.customer_id, tab.order_date, tab.product_name, tab.join_date
FROM (
	SELECT s.customer_id, s.order_date, m.product_name, mbr.join_date,
	RANK() OVER (PARTITION BY s.customer_id ORDER BY s.order_date DESC) AS rnk
	FROM dannys_diner.sales s
		JOIN dannys_diner.menu m ON s.product_id = m.product_id
		JOIN dannys_diner.members mbr ON s.customer_id = mbr.customer_id
	WHERE order_date < join_date 
    GROUP BY s.customer_id, s.order_date, m.product_name, mbr.join_date
	ORDER BY s.order_date, s.customer_id
	) AS tab
WHERE rnk = 1;
```
| customer_id | order_date               | product_name | join_date                |
| ----------- | ------------------------ | ------------ | ------------------------ |
| A           | 2021-01-01T00:00:00.000Z | curry        | 2021-01-07T00:00:00.000Z |
| A           | 2021-01-01T00:00:00.000Z | sushi        | 2021-01-07T00:00:00.000Z |
| B           | 2021-01-04T00:00:00.000Z | sushi        | 2021-01-09T00:00:00.000Z |
---



8. **What is the total items and amount spent for each member before they became a member?**

* Join tall tables together using the product_id and customer_id as the link between tables.
* Use WHERE statement to filter out orders made before a customer became a member
* Use GROUP BY statement to perform calculations by customer
* COUNT(*) function determines the number of orders made by customer
* SUM(m.price) calculates the total spent by customer before membership
```SQL
SELECT s.customer_id, COUNT(*) AS total_items, SUM(m.price) AS amnt_spent
FROM dannys_diner.sales s
	JOIN dannys_diner.menu m ON s.product_id = m.product_id
	JOIN dannys_diner.members mbr ON s.customer_id = mbr.customer_id
WHERE s.order_date < mbr.join_date 
GROUP BY s.customer_id
ORDER BY s.customer_id;
```
| customer_id | total_items | amnt_spent |
| ----------- | ----------- | ---------- |
| A           | 2           | 25         |
| B           | 3           | 40         |
---

[View on DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/4716)

11. **If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**
12. **In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**


