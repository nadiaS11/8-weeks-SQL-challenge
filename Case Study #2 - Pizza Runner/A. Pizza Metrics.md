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
select r.order_id, count(c.pizza_id) as pizzas_per_order,
		DENSE_RANK() OVER(ORDER BY COUNT(c.pizza_id) DESC) as rank
from pizza_runner.runner_orders as r
join pizza_runner.customer_orders as c
on r.order_id=c.order_id
where r.distance  IS NOT NULL
group by r.order_id
order by count(c.pizza_id) desc;
````
#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/213010344-934f21a0-d9d6-4cea-8a12-d27f8ef0089a.jpg)
- Order ID 4 has the maximum number of pizza delivered in a single order with 3 pizzas

### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
- This is where i switched to MySQL and made necessary changes such copying the table and cleaning the data.
````sql
select c.customer_id, 
	sum(
	case when exclusions is not null or extras is not null then 1
	else 0
    end)  as at_least_one_change,
	sum(
	case when exclusions is null AND  extras is null then 1
	else 0
    end) as no_change

from customer_orders_new as c
join runner_orders_new as r on c.order_id=r.order_id
where distance is not null
group by customer_id
order by customer_id;
````
#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/213940702-d06f2a0c-7adf-4142-8143-ec3f819a40ba.jpg)

### 8. How many pizzas were delivered that had both exclusions and extras?
````sql
select c.customer_id, 
	sum(
	case when exclusions is not null AND extras is not null then 1
	else 0
    end)  as both_exclusions	
from customer_orders_new as c
join runner_orders_new as r on c.order_id=r.order_id
where distance is not null
group by customer_id
order by customer_id;
````

#### Answer:
![Screenshot 2023-01-15 015654](https://user-images.githubusercontent.com/110742273/213941148-b2d39c3b-3b55-4e9c-8eb3-122b0b2bc3d0.jpg)


### 9. What was the total volume of pizzas ordered for each hour of the day?
````sql
 select extract(hour from order_time) as Hour_data, count(order_id) as pizza_count
from pizza_runner.customer_orders_new
group by Hour_data
order by Hour_data
````
#### Answer:
![Screenshot 2023-01-23 152030](https://user-images.githubusercontent.com/110742273/214004266-7cd13f33-ca31-4858-ad03-ff41eb17ce28.jpg)

### 10.What was the volume of orders for each day of the week?
````sql
 select dayname( order_time) as day_of_the_week, count(order_id) as pizza_count
from pizza_runner.customer_orders_new
group by day_of_the_week
order by day_of_the_week
````

#### Answer:
![Screenshot 2023-01-23 152030](https://user-images.githubusercontent.com/110742273/214004916-9459df04-7384-4396-a18a-7d8661f800f7.jpg)
