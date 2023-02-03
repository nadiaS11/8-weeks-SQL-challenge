# 🧹 Data Cleaning for Case Study #2 - Pizza Runner

**I was solving this case study on Fiddle without data cleaning at first, but I couldn't continue with that messed-up data. So I finally switched to MySQL to re-prepare the data.**

Let's start with **runner_orders**. The table has:
- use of 'null' as string
- empty fields
- km/min/minutes in the distance and duration columns which will obstruct the analysis.

![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/216430799-84047be5-9e6e-4b4d-be9d-cc7363fbc35d.jpg)


To clean the data, I created a new table called **runner_orders_new**, which is a copy of runner_orders, so that no original data was lost.
In the runner_orders table, we need to remove 
- the string from the distance and duration columns
- fix any string 'nulls'.

```sql
#copying the table and cleaning the data

drop table if exists runner_orders_new;
create table runner_orders_new as 
(
select order_id, runner_id, 
case 
  when pickup_time like 'null' or '' then Null
  else pickup_time 
  end as pickup_time,
case
   when distance like '%km' then trim('km' from distance)
   when distance  like 'null' or '' then Null
   else distance 
   end as distance,
case
   when duration like '%minutes' then trim('minutes' from duration)
   when duration like '%mins' then trim('mins' from duration)
   when duration like '%minute' then trim('minute' from duration)
   when duration like 'null' or '' then Null
   else duration
	 end as duration, 
case 
   when cancellation in( '' , 'null') then Null
   else cancellation
   end as  cancellation
from runner_orders
)
```
This is how the new table looks like: 

![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/216521085-48bcacf5-443d-4885-b981-b72c82ef7eb4.jpg)


Now we need to copy and clean the **customers_order** in new table as **customers_order_new**

```sql
drop table if exists customer_orders_new2;
create table customer_orders_new2 as 
(
select customer_id, order_id,  
case 
when exclusions in( '' , 'null')  then Null
 else exclusions 
end as exclusion,
case
   when extras in( '' , 'null')  then Null
  else extras 
end as extras,
order_time
from customer_orders)
```

Result:

![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/216523815-98754871-cc85-472e-b15f-0ea185874d19.jpg)


Also, the datatypes for the distance and duration columns are varchar, but they are numbers, so we must convert them to **decimal** and **integer** data types, respectively. Also, the **pickup_time** column is varchar, but because it is a timestamp, the datatype must be changed to **DateTime**.

```sql
#update datatypes for runner table

 alter table runner_orders_new
 modify column pickup_time datetime null,
 modify column distance decimal(5,1) null,
 modify column duration int null;
```
