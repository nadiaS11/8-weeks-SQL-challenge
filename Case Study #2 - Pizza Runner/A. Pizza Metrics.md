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

````
#### Answer:
### 4. How many of each type of pizza was delivered?
````sql

````
#### Answer:
### 5. How many Vegetarian and Meatlovers were ordered by each customer?
````sql

````

#### Answer:
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
