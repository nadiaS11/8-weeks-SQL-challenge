# 🍕 Case Study #2 - Pizza Runner

## 🍝 Solution - A. Pizza Metrics

### 1. How many pizzas were ordered?

````sql
SELECT COUNT(*) AS pizza_count
FROM pizza_runner.customer_orders;
````
#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/212493890-49dca672-b624-49c1-9194-4465d735df79.jpg)

### 2. How many unique customer orders were made?
````sql
SELECT COUNT(distinct order_id) AS unique_order
FROM pizza_runner.customer_orders;
````
#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/212494089-884f8e24-a6b1-494e-ad31-413c81fb6a8d.jpg)

### 3. How many successful orders were delivered by each runner?
````sql
select runner_id, count(order_id) as successful_orders
from pizza_runner.runner_orders
where  distance != 'null'
group by runner_id;
````
#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/212494822-b62f533c-c049-40c7-8ee6-c7baa95981a8.jpg)

### 4. How many of each type of pizza was delivered?
````sql
select runner_id, count(order_id) as successful_orders
from pizza_runner.runner_orders
where  distance != 'null'
group by runner_id;
--How many of each type of pizza was delivered?
select p.pizza_name, count(r.order_id)
from pizza_runner.pizza_names as p
join pizza_runner.customer_orders as c
on p.pizza_id=c.pizza_id
join pizza_runner.runner_orders as r
on c.order_id=r.order_id
where r.distance != 'null'
group by p.pizza_name;
````
#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/212495399-affbd78f-9356-4043-9790-6fe1e45383cc.jpg)

### 5. How many Vegetarian and Meatlovers were ordered by each customer?
````sql
select c.customer_id, p.pizza_name, count(c.order_id)
from pizza_runner.pizza_names as p
join pizza_runner.customer_orders as c
on p.pizza_id=c.pizza_id
group by c.customer_id, p.pizza_name
ORDER BY c.customer_id;
````

#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/212495645-034e82d0-03f9-4143-913b-c7f07e0ea9a3.jpg)

### 6. What was the maximum number of pizzas delivered in a single order?
````sql

````
#### Answer:
### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
````sql

````
#### Answer:
### 8. How many pizzas were delivered that had both exclusions and extras?
````sql

````

#### Answer:
### 9. What was the total volume of pizzas ordered for each hour of the day?
````sql

````
#### Answer:
### 10.What was the volume of orders for each day of the week?
````sql

````

#### Answer:
