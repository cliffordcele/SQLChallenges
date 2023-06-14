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
```SQL
SELECT s.customer_id, 
	SUM(m.price) AS total_spent
FROM dannys_diner.sales s
	LEFT JOIN dannys_diner.menu m 
	ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id;
```

**Schema (PostgreSQL v13)**
---

| customer_id | total_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

---

[View on DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138)


3. **How many days has each customer visited the restaurant?**
4. **What was the first item from the menu purchased by each customer?**
5. **What is the most purchased item on the menu and how many times was it purchased by all customers?**
6. **Which item was the most popular for each customer?**
7. **Which item was purchased first by the customer after they became a member?**
8. **Which item was purchased just before the customer became a member?**
9. **What is the total items and amount spent for each member before they became a member?**
10. **If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**
11. **In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**


