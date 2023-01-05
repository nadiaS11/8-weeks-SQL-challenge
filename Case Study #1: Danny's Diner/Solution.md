# 🍜 Case Study #1: Danny's Diner

## Solution
***
### What is the total amount each customer spent at the restaurant?
````sql
select customer_id, sum(price) as total_sales
from  dannys_diner.sales as s
join  dannys_diner.menu as m
on s.product_id=m.product_id
group by customer_id 
order by customer_id;
````

#### Answer:
| customer_id | total_sales |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |



### How many days has each customer visited the restaurant?
````sql
select customer_id, count(DISTINCT order_date)
from dannys_diner.sales
group by customer_id;
````
#### Answer:
| customer_id | total_sales |
| ----------- | ----------- |
| A           | 4           |
| B           | 6           |
| C           | 2           |


### What was the first item from the menu purchased by each customer?
````sql

````
### What is the most purchased item on the menu and how many times was it purchased by all customers?
````sql

````
### Which item was the most popular for each customer?
````sql

````
### Which item was purchased first by the customer after they became a member?
````sql

````
### Which item was purchased just before the customer became a member?
````sql

````
### What is the total items and amount spent for each member before they became a member?
````sql

````
### If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
````sql

````
### In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
