# Case Study Solution
### 1. What is the total amount each customer spent at the restaurant?

    SELECT customer_id, sum(price) as Amount_Spent
    FROM sales s
    INNER JOIN menu m
    ON s.product_id = m.product_id
    GROUP BY customer_id

 | customer_id | Amount_spent |
|-------------|--------------|
| A           | 76           |
| B           | 74           |
| C           | 36           |


### 2. How many days has each customer visited the restaurant?

    SELECT customer_id, count (distinct(order_date)) as Days_visited
    FROM sales
    GROUP BY customer_id

| customer_id | Days_visited |
|-------------|--------------|
| A           | 4            |
| B           | 6            |
| C           | 2            |

### 3. What was the first item from the menu purchased by each customer?

    WITH cte_first_date AS (
    SELECT customer_id, product_name, order_date,
    RANK() OVER(PARTITION BY customer_id ORDER BY order_date) AS rank
    FROM sales
    INNER JOIN menu
    ON sales.product_id = menu.product_id
    )

    SELECT DISTINCT customer_id, product_name, order_date
    FROM cte_first_date
    WHERE rank = 1


| customer_id | product_name | order_date |
|-------------|--------------|------------|
| A           | curry        | 2021-01-01 |
| A           | sushi        | 2021-01-01 |
| B           | curry        | 2021-01-01 |
| C           | ramen        | 2021-01-01 |

### 4.  What is the most purchased item on the menu and how many times was it purchased by all customers?

    WITH Most_purchased AS (SELECT product_name, COUNT (product_name) as No_of_times_purchased
    FROM Sales s
    INNER JOIN menu m
    ON s.product_id = m.product_id
    GROUP BY product_name)

    SELECT product_name, No_of_times_purchased
    FROM Most_purchased
    WHERE No_of_times_purchased = (
    SELECT MAX(No_of_times_purchased)
    FROM Most_purchased
    );

| product_name | No_of_times_purchased |
|--------------|-----------------------|
| ramen        | 8                     |

### 5. Which item was the most popular for each customer?

    WITH Most_Popular AS (
    SELECT customer_id, product_name, COUNT (product_name)as Times_Purchased
    FROM Sales s
    INNER JOIN Menu m
    ON s.product_id = m.product_id
    GROUP BY customer_id, product_name
    )
    SELECT customer_id, product_name, Times_Purchased
    FROM ( 
    SELECT *, RANK() OVER (PARTITION BY customer_id ORDER BY Times_Purchased DESC) as Popularity_Rank
    FROM Most_Popular
    ) subquery
    WHERE Popularity_Rank = 1;

 | customer_id | product_name | Times_Purchased |
|-------------|--------------|-----------------|
| A           | ramen        | 3               |
| B           | sushi        | 2               |
| B           | curry        | 2               |
| B           | ramen        | 2               |
| C           | ramen        | 3               |

### 6. Which item was purchased first by the customer after they became a member?

    WITH date_joined AS (SELECT s.customer_id, s.product_id, m.join_date
    FROM members m
    INNER JOIN sales s
    ON m.customer_id = s.customer_id
    WHERE join_date = order_date 
    OR order_date > join_date)

    SELECT customer_id, mm.product_name, join_date
    FROM ( SELECT *, RANK() OVER(PARTITION BY customer_id ORDER BY product_id) AS first_item
    FROM date_joined
    )subquery
    INNER JOIN Menu mm
    ON subquery.product_id = mm.product_id
    WHERE first_item = 1;

| customer_id | product_name | join_date  |
|-------------|--------------|------------|
| B           | sushi        | 2021-01-09 |
| A           | curry        | 2021-01-07 |

### 7. Which item was purchased just before the customer became a member?

    WITH date_joined AS (SELECT s.customer_id, s.product_id, m.join_date, s.order_date
    FROM members m
    INNER JOIN sales s
    ON m.customer_id = s.customer_id
    WHERE order_date < join_date)

    SELECT customer_id, mm.product_name, join_date, order_date
    FROM (SELECT *, DENSE_RANK() OVER(PARTITION BY customer_id ORDER BY order_date DESC) as R_N
    FROM date_joined
    )subquery
    INNER JOIN Menu mm
    ON subquery.product_id = mm.product_id
    WHERE R_N = 1;

| customer_id | product_name | join_date  | order_date |
|-------------|--------------|------------|------------|
| A           | sushi        | 2021-01-07 | 2021-01-01 |
| A           | curry        | 2021-01-07 | 2021-01-01 |
| B           | sushi        | 2021-01-09 | 2021-01-04 |

### 8. What is the total items and amount spent for each member before they became a member?

    WITH first_table AS (SELECT m.product_name, s.customer_id, s.product_id, s.order_date, mm.join_date,m.price
    FROM Sales s
    INNER JOIN Menu m
    ON s.product_id = m.product_id
    INNER JOIN Members mm
    ON s.customer_id = mm.customer_id
    WHERE order_date < join_date)

    SELECT customer_id, count (product_id) as total_items, sum(price) as amount_spent
    FROM first_table
    GROUP BY customer_id

| customer_id | total_items | amount_spent |
|-------------|-------------|--------------|
| A           | 2           | 25           |
| B           | 3           | 40           |

### 9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

    WITH cte_spent AS (SELECT customer_id, product_name, sum(price) as amount_spent
    FROM Sales s
    INNER JOIN Menu m
    ON s.product_id = m.product_id
    GROUP BY customer_id, product_name)

    SELECT  
    customer_id, 
    SUM(CASE 
		WHEN product_name = 'sushi' THEN amount_spent * 20
	    ELSE amount_spent * 10
	    END) as points
    FROM cte_spent
    GROUP BY customer_id; 

| customer_id | points |
|-------------|--------|
| A           | 860    |
| B           | 940    |
| C           | 360    |

### 10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

    WITH Jan_record AS (SELECT s.customer_id, product_name, order_date, join_date, sum(price) as price
    FROM Sales s
    INNER JOIN Members mm
    ON s.customer_id = mm.customer_id
    INNER JOIN Menu m
    ON s.product_id = m.product_id
    GROUP BY s.customer_id, m.product_name, s.order_date, mm.join_date)

    SELECT customer_id,
    SUM(CASE
    WHEN product_name = 'sushi' THEN price * 20
    WHEN order_date < join_date THEN price * 10
    WHEN order_date = join_date THEN price * 20
    WHEN order_date < '2021-01-31' THEN price * 20
    END) as points
    FROM Jan_record
    GROUP BY customer_id

| customer_id | points |
|-------------|--------|
| A           | 1370   |
| B           | 940    |












