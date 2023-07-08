# Introduction
Pizza Runner is a uber-style pizza delivery company. “Runners” deliver fresh pizza from Pizza Runner Headquarters 
(otherwise known as Danny’s house). Danny has also paid freelance developers to build a mobile app to accept orders from customers.

# Problem
Danny would like to clean the data he has collected, and apply some basic calculations so he can better direct his runners and 
optimise Pizza Runner’s operations.

# Data
Danny has shared five datasets for this case study:
*  **RUNNERS:** shows the registration_date for each new runner.
*  **CUSTOMER ORDERS:** each row represents a pizza ordered by a customer. Customer's can order multiple pizzas in a signle order. A [ingredient_id]() is used to determine which ingredients should be removed ([exclusions]()) or should be added ([extras]()).
*  **RUNNER ORDERS:** each order is assigned to a runner for delivery. This table contains delivery information such as the [pickup_time](), and how long ([duration]()) and how far ([distance]()) the runner had to travel to deliver the order.
*   **PIZZA NAMES:** Pizza Runner only has 2 pizzas available the Meat Lovers or Vegetarian.
*   **PIZZA RECIPES:** Each [pizza_id]() has a standard set of toppings which are used as part of the pizza recipe.
*   **PIZZA TOPPING:** This table contains all of the [topping_name]() values with their corresponding [topping_id]() value.

# Entity Relationship Diagram
<p align="center">
  <img src="Pictures/ER_Diagram.png" alt="ER_Diagram" width="600"/>
</p>

# Data Cleaning
** customer_orders table: The exclusions and extras columns will need to be cleaned up before using them in your queries. **

** runner_orders: There are some known data issues with this table so be careful when using this in your queries - make sure to check the data types for each column in the schema SQL! **

** null values and data types in the customer_orders and runner_orders tables. **



# Case Study Solutions


