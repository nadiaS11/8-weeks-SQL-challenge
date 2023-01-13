# 🍜 Case Study #1: Danny's Diner

## Solution
***
### 1. What is the total amount each customer spent at the restaurant?
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



### 2. How many days has each customer visited the restaurant?
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


### 3. What was the first item from the menu purchased by each customer?
````sql
WITH cte_customers AS
(
   SELECT customer_id, order_date, product_name,
      DENSE_RANK() OVER(PARTITION BY s.customer_id
      ORDER BY s.order_date) AS rank
   FROM dannys_diner.sales AS s
   JOIN dannys_diner.menu AS m
      ON s.product_id = m.product_id
)

SELECT customer_id, product_name
FROM cte_customers
WHERE rank = 1
GROUP BY customer_id, product_name;
````

#### Answer:
| customer_id | product_name | 
| ----------- | -----------  |
| A           | curry        | 
| A           | sushi        | 
| B           | curry        | 
| C           | ramen        |

### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?
````sql
-- most purchased item on the menu
select Distinct m.product_name, count(s.order_date) as most_purchased
from dannys_diner.menu as m
join dannys_diner.sales as s
on m.product_id=s.product_id
group by m.product_id, product_name
order by most_purchased desc
limit 1;
````
#### Answer:
| product_name| most_purchased | 
| ----------- | -----------    |
| ramen       | 8              |

-- Most popular item is ramen.

````sql
-- how many times was "ramen" purchased by all customers?
Select ss.customer_id,  mm.product_name, Count(ss.product_id) as order_count
from dannys_diner.sales as ss
join dannys_diner.menu as mm
on ss.product_id=mm.product_id
where mm.product_name like '%ramen%'
group by ss.customer_id,  mm.product_name;
````

#### Answer:
| customer_id | product_name | order_count | 
| ----------- | -----------  | -----------  |
| A           | ramen        | 3            | 
| A           | ramen        | 2            | 
| B           | ramen        | 3            | 


### 5. Which item was the most popular for each customer?
````sql
with cte_popular_item as
(
  Select s.customer_id,  m.product_name, Count(s.product_id) as order_count,
  DENSE_RANK() over(partition by s.customer_id  
                    ORDER BY COUNT(s.customer_id) DESC) as rank
from dannys_diner.sales as s
join dannys_diner.menu as m
on s.product_id=m.product_id
GROUP BY s.customer_id, m.product_name
)
  
SELECT customer_id, product_name, order_count
FROM cte_popular_item 
WHERE rank = 1;

````
#### Answer:
| customer_id | product_name | order_count |
| ----------- | ----------   |------------ |
| A           | ramen        |  3          |
| B           | sushi        |  2          |
| B           | curry        |  2          |
| B           | ramen        |  2          |
| C           | ramen        |  3          |
### 6. Which item was purchased first by the customer after they became a member?
````sql
with cte_first_order_after_join as
(
select s.customer_id, s.product_id, s.order_date,
dense_rank() over(partition by s.customer_id 
                  order by(s.order_date) ) as rank
from dannys_diner.sales as s
join dannys_diner.members as m
on s.customer_id=m.customer_id
where s.order_date>=m.join_date
  )
  
Select c.customer_id, mm.product_name, c.order_date
from cte_first_order_after_join as c
join dannys_diner.menu as mm
on c.product_id=mm.product_id
where rank=1;
````
#### Answer:
| customer_id | product_name | order_date |
| ----------- | ---------- |----------    |
| B           | sushi      | 2021-01-11   |
| A           | curry      |  2021-01-07  |

### 7. Which item was purchased just before the customer became a member?
````sql
with cte_last_order_before_join as
(
select s.customer_id, s.product_id, s.order_date,
dense_rank() over(partition by s.customer_id 
                  order by(s.order_date) desc) as rank
from dannys_diner.sales as s
join dannys_diner.members as m
on s.customer_id=m.customer_id
where s.order_date<m.join_date
  )
  
Select c.customer_id, mm.product_name, c.order_date
from cte_last_order_before_join as c
join dannys_diner.menu as mm
on c.product_id=mm.product_id
where rank=1;
````
#### Answer:
| customer_id | product_name | order_date |
| ----------- | ---------- |----------    |
| B           | sushi      | 2021-01-04   |
| A           | sushi      | 2021-01-01   |
| A           | curry      | 2021-01-01   |
### 8. What is the total items and amount spent for each member before they became a member?
````sql
select s.customer_id, count(distinct s.product_id) as total_item, sum(m.price) as total_amount
from dannys_diner.sales as s
join dannys_diner.menu as m
on s.product_id=m.product_id
join dannys_diner.members as mm
on s.customer_id=mm.customer_id
where order_date<join_date
group by s.customer_id;
````
#### Answer:
| customer_id | total_item | total_amount |
| ----------- | ---------- |----------    |
| A           | 2          |  25          |
| B           | 2          |  40          |
### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
````sql
with cte_points as
(
Select *,
		CASE WHEN product_id=1 then price*20
        Else price*10 
  		end as price_ponts
  from dannys_diner.menu
  )
 
 select s.customer_id, sum(c.price_ponts) as total_points
 from  dannys_diner.sales as s       
 join cte_points as c
 on s.product_id=c.product_id
 group by s.customer_id
 order by s.customer_id;
````
#### Answer:
| customer_id | total_points   | 
| ----------- | ----------     |
| A           | 860            |
| B           | 940            |
| C           | 360            |


### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
````sql
WITH dates_cte AS (
   SELECT m.*, 
      join_date + INTERVAL '6 DAY' AS valid_date, 
      DATE_TRUNC('month', '2023-01-31'::DATE) + INTERVAL '1 MONTH - 1 DAY' AS last_date
   FROM dannys_diner.members m
)

SELECT d.customer_id,
   SUM(CASE
      WHEN m.product_name = 'sushi' THEN 2 * 10 * m.price
      WHEN s.order_date BETWEEN d.join_date AND d.valid_date THEN 2 * 10 * m.price
      ELSE 10 * m.price
      END) AS points
FROM dates_cte d
JOIN dannys_diner.sales s ON d.customer_id = s.customer_id
JOIN dannys_diner.menu m ON s.product_id = m.product_id
WHERE s.order_date < d.last_date
GROUP BY d.customer_id;
````
#### Answer:
| customer_id | points         | 
| ----------- | ----------     |
| A           | 1370           |
| B           | 820            |



## Bonus Questions

### Join All The Things(Recreating the following table)
|customer_id | order_date| product_name	| price	member
| --------- | ---------| ----------- | ----------- |
|A	| 2021-01-01	 | sushi	 | 10	 | N
|A	| 2021-01-07	 | curry	 | 15	 | Y
|A	| 2021-01-10	 | ramen	 | 12	 | Y
|A	| 2021-01-11	 | ramen	 | 12	 | Y
|A	| 2021-01-11	 | ramen	 | 12	 | Y
|B	| 2021-01-01	 | curry	 | 15	 | N
|B	| 2021-01-02	 | curry	 | 15	 | N
|B	| 2021-01-04	 | sushi	 | 10	 | N
|B	| 2021-01-11	 | sushi	 | 10	 | Y
|B	| 2021-01-16	 | ramen	 | 12	 | Y
|B	| 2021-02-01	 | ramen	 | 12	 | Y
|C	| 2021-01-01	 | ramen	 | 12	 | N
|C	| 2021-01-01	 | ramen	 | 12	 | N
|C	| 2021-01-07	 | ramen	 | 12	 | N

````sql
select s.customer_id, s.order_date, m.product_name,m.price, 
		case when s.order_date>=mm.join_date then 'Y'
        else 'N'
        end as membership
from dannys_diner.sales as s
join dannys_diner.menu as m
on s.product_id=m.product_id
join dannys_diner.members as mm
on s.customer_id=mm.customer_id;

````
### Rank All The Things












