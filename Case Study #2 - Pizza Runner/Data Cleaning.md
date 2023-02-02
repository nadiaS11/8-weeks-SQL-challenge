# 🧹 Data Cleaning for Case Study #2 - Pizza Runner

**I was solving this case study on Fiddle without data cleaning at first, but I couldn't continue with that messed-up data. So I finally switched to MySQL to re-prepare the data.**

Let's start with **runner_orders**. The table has:
- use of null as 'string'
- empty fields
- km/min/minutes in the distance and duration columns which will obstruct the analysis.

![Screenshot 2023-01-23 202024](https://user-images.githubusercontent.com/110742273/216430799-84047be5-9e6e-4b4d-be9d-cc7363fbc35d.jpg)


To clean the data, I created a new table called **runner_orders_new**, which is a copy of runner orders, so that no original data was lost. 
