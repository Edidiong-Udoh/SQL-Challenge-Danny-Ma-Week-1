# SQL-Challenge-Danny-Ma-Week-1

![](Images/Danny_Ma.png)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

*This repository hosts my solutions to the 1st challenge (Week 1) of the 8 Weeks SQL Challenge by DannyMa. [Click here to view the full challenge](https://8weeksqlchallenge.com/case-study-1/)*

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 🗒️Table of Content
* [Business Case](https://github.com/Edidiong-Udoh/SQL-Challenge-Danny-Ma-Week-1/tree/main)
* [Entity Relationship Diagram](https://github.com/Edidiong-Udoh/SQL-Challenge-Danny-Ma-Week-1/tree/main)
* [Available Data](https://github.com/Edidiong-Udoh/SQL-Challenge-Danny-Ma-Week-1/tree/main)
* [Case Study Questions](https://github.com/Edidiong-Udoh/SQL-Challenge-Danny-Ma-Week-1/tree/main)
* [Case Study Solutions](Case_Study_Solutions.md)
* [Insight](https://github.com/Edidiong-Udoh/SQL-Challenge-Danny-Ma-Week-1/tree/main)

------------------------------------------------------------------------------------------------------------------------------------------------------------

## Business Case
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Entity Relationship Diagram

![](dataset_schema.jpeg)

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Available Data

<details> 
<summary>Dataset Used for the Challenge</summary>
<br>

Table 1: sales

| customer_id | order_date | product_id |
|-------------|------------|------------|
| A           | 2021-01-01 | 1          |
| A           | 2021-01-01 | 2          |
| A           | 2021-01-07 | 2          |
| A           | 2021-01-10 | 3          |
| A           | 2021-01-11 | 3          |
| A           | 2021-01-11 | 3          |
| B           | 2021-01-01 | 2          |
| B           | 2021-01-02 | 2          |
| B           | 2021-01-04 | 1          |
| B           | 2021-01-11 | 1          |
| B           | 2021-01-16 | 3          |
| B           | 2021-02-01 | 3          |
| C           | 2021-01-01 | 3          |
| C           | 2021-01-01 | 3          |
| C           | 2021-01-07 | 3          |


Table 2: menu

| product_id | product_name | price |
|------------|--------------|-------|
| 1          | sushi        | 10    |
| 2          | curry        | 15    |
| 3          | ramen        | 12    |


Table 3: members

| product_id | product_name | price |
|------------|--------------|-------|
| 1          | sushi        | 10    |
| 2          | curry        | 15    |
| 3          | ramen        | 12    |

</details>


-----------------------------------------------------------------------------------------------------------------------------------------

## Case Study Solutions
1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10.In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

## Introduction
This project contains the SQL query functions I used in solving the problem statements attached to this case study. It was fun and challenging at the same time. I love SQL because of its versatility. There are many ways to solve a problem and I applied my understanding of certain SQL functions in arriving at my answers.

## Case Study
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

## Dataset
Danny shared 3 key datsets for this case:
* sales: The sales table captures all customer_id level purchases with a corresponding order_date and product_id information for when and what menu items were ordered.
* menu: The menu table maps the product_id to the actual product_name and price of each menu item.
* members: The final members table captures the join_date when a customer_id joined the beta version of the Danny’s Diner loyalty program. 

     ![](dataset_schema.jpeg)

## Case Questions
1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Case Study Solution
* Click [here](Case_Study_Solutions.md) to view the Case Study Solution
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Insights
Based on my analysis, the following insights would be useful for Danny:
* Customer A has spent more money in the store than the others. His favorite menu is ramen.
  
| customer_id | % of Total Income     |
|-------------|--------|
| A           | 40.86% |
| B           | 39.78% |
| C           | 19.36% |

* Customer B has visited the store more often than others and likes the 3 menus equally.

* Customer C is the least performing customer. He has only been to the store twice and bought ramen only. Also, he did not sign up for the members' program.

* Although Customer A spent more money than others, Customer B actually spent a total of $40 before joining the members' program while Customer A spent $25. This means that Customer A was motivated to spend more because of the benefits the program offers. This is an indicator for the program to keep running because it has the potential of increasing sales. 

