# 🍕 Case Study #2 Pizza Runner

## Solution - B. Runner and Customer Experience

### 1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
````sql
SELECT 
  WEEK(registration_date) + 1 AS registration_week,
  COUNT(runner_id) AS registered_runners
FROM  runners
GROUP BY registration_week
ORDER BY registration_week;

````
#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214924900-6b29c81a-09e3-4827-ac60-3dab28541b5c.jpg)

- I used +1 to get the registration week number because the WEEK() function starts from 0 for the first week of the year.
- 
### 2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?

````sql
SELECT
  AVG(TIMESTAMPDIFF(MINUTE, order_time, pickup_time)) AS avg_minutes,
  runner_id
FROM customer_orders_new as c
join runner_orders_new as r
on c.order_id=r.order_id
group by runner_id;
````

#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214927908-550aac64-66d4-4bd5-b433-6cc8a220e210.jpg)

### 3. Is there any relationship between the number of pizzas and how long the order takes to prepare?

````sql
with Prep_time as (
select c.order_id, count(pizza_id) as no_of_pizza, TIMESTAMPDIFF(MINUTE, order_time, pickup_time) AS minutes_taken 
FROM customer_orders_new as c
join runner_orders_new as r
on c.order_id=r.order_id
 WHERE r.distance != 0
 GROUP BY c.order_id, c.order_time, r.pickup_time
)
select no_of_pizza, avg(minutes_taken) as avg_prep_time
from Prep_time
group by no_of_pizza;
````

#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214934671-8d1cd22c-38fe-48c9-8892-5e940b7becef.jpg)

- As we can see, a single pizza takes 12 minutes, two take 20 minutes, and three take 29 minutes.

### 4. What was the average distance travelled for each customer?

````sql
select customer_id, avg(r.distance) as avg_distance
from customer_orders_new as c
join runner_orders_new as r
on c.order_id=r.order_id
group by customer_id;
````

#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214938369-a8d875a3-ee69-4faa-9ef1-72ec907a5f9c.jpg)

### 5. What was the difference between the longest and shortest delivery times for all orders?

````sql
select Max(duration)-Min(duration) as Duration_difference
from runner_orders_new
where duration is not null;
````

#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214940265-c451435d-269c-48e2-8be7-3cddbd30463e.jpg)
### 6. What was the average speed for each runner for each delivery and do you notice any trend for these values?

````sql
with cte_speed as (
select runner_id, order_id, round(distance *60/duration,1) as speed_in_KMPH
from runner_orders_new
where distance is not null
group by runner_id, order_id)
select * from cte_speed
order by runner_id;
````

#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214942305-35cb99f2-8a5d-46a8-aff0-a9cde7f04f94.jpg)

### 7. What is the successful delivery percentage for each runner?

````sql
with cte_delivery_count as
(
select runner_id, 
		sum(case when distance != 0 then 1
			else 0
			end) as delivered_orders, 		
        count(order_id) as total_orders
from runner_orders_new
group by runner_id
)
select runner_id,round((delivered_orders/total_orders)*100) as Percentage_of_successful_delivery 
from cte_delivery_count
order by runner_id;
````

#### Answer:
![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/214943562-e2108be1-982e-4def-b4d0-341bfda2be25.jpg)

- I didn't factor in order cancellation as it is always out of runners control.
