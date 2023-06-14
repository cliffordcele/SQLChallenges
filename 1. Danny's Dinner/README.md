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

* Left join the data from the menu table to the sales table using the product_id as the link between the two tables
* Group the orders made in the sales table by the customer_id
* Sum the price for each order by customer_id to obtain the total_spent by customer
```SQL
SELECT s.customer_id, 
	SUM(m.price) AS total_spent
FROM dannys_diner.sales s
	LEFT JOIN dannys_diner.menu m 
	ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id;
```
---
| customer_id | total_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

---



3. **How many days has each customer visited the restaurant?**

```SQL
SELECT customer_id, COUNT(DISTINCT order_date)
FROM dannys_diner.sales
GROUP BY customer_id
ORDER BY customer_id;
```
**Schema (PostgreSQL v13)**

    CREATE SCHEMA dannys_diner;
    SET search_path = dannys_diner;
    
    CREATE TABLE sales (
      "customer_id" VARCHAR(1),
      "order_date" DATE,
      "product_id" INTEGER
    );
    
    INSERT INTO sales
      ("customer_id", "order_date", "product_id")
    VALUES
      ('A', '2021-01-01', '1'),
      ('A', '2021-01-01', '2'),
      ('A', '2021-01-07', '2'),
      ('A', '2021-01-10', '3'),
      ('A', '2021-01-11', '3'),
      ('A', '2021-01-11', '3'),
      ('B', '2021-01-01', '2'),
      ('B', '2021-01-02', '2'),
      ('B', '2021-01-04', '1'),
      ('B', '2021-01-11', '1'),
      ('B', '2021-01-16', '3'),
      ('B', '2021-02-01', '3'),
      ('C', '2021-01-01', '3'),
      ('C', '2021-01-01', '3'),
      ('C', '2021-01-07', '3');
     
    
    CREATE TABLE menu (
      "product_id" INTEGER,
      "product_name" VARCHAR(5),
      "price" INTEGER
    );
    
    INSERT INTO menu
      ("product_id", "product_name", "price")
    VALUES
      ('1', 'sushi', '10'),
      ('2', 'curry', '15'),
      ('3', 'ramen', '12');
      
    
    CREATE TABLE members (
      "customer_id" VARCHAR(1),
      "join_date" DATE
    );
    
    INSERT INTO members
      ("customer_id", "join_date")
    VALUES
      ('A', '2021-01-07'),
      ('B', '2021-01-09');

---

**Query #1**

    SELECT customer_id, COUNT(DISTINCT order_date)
    FROM dannys_diner.sales
    GROUP BY customer_id
    ORDER BY customer_id;

| customer_id | count |
| ----------- | ----- |
| A           | 4     |
| B           | 6     |
| C           | 2     |

---

[View on DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138)

5. **What was the first item from the menu purchased by each customer?**
6. **What is the most purchased item on the menu and how many times was it purchased by all customers?**
7. **Which item was the most popular for each customer?**
8. **Which item was purchased first by the customer after they became a member?**
9. **Which item was purchased just before the customer became a member?**
10. **What is the total items and amount spent for each member before they became a member?**
11. **If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**
12. **In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**


