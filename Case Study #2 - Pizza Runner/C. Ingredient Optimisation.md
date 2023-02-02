# 🍕 Case Study #2 Pizza Runner

## Solution - C. Ingredient Optimisation

### 1. What are the standard ingredients for each pizza?
````sql

````

#### Answer:

### 2. What was the most commonly added extra?
````sql
```sql

FROM toppings_cte t
INNER JOIN pizza_runner.pizza_toppings pt
  ON t.topping_id = pt.topping_id
GROUP BY t.topping_id, pt.topping_name
ORDER BY topping_count DESC;
```
````

#### Answer:


### 3. What was the most common exclusion?
````sql

````

#### Answer:


### 4. Generate an order item for each record in the customers_orders table in the format of one of the following:
- Meat Lovers
- Meat Lovers - Exclude Beef
- Meat Lovers - Extra Bacon
- Meat Lovers - Exclude Cheese, Bacon - Extra Mushroom, Peppers
````sql

````

#### Answer:


### 5. Generate an alphabetically ordered comma separated ingredient list for each pizza order from the customer_orders table and add a 2x in front of any relevant ingredients
-  For example: "Meat Lovers: 2xBacon, Beef, ... , Salami"
  
````sql

````

#### Answer:


### 7. What is the total quantity of each ingredient used in all delivered pizzas sorted by most frequent first?
````sql

````

#### Answer:

