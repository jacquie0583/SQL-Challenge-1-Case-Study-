# SQL-Challenge---Case-Study-
A Customer needs help answering questions with regard to his business.  A sample of his overall customer data, 3 key datasets, were used to write fully functioning SQL queries to help the answer his questions! Datasets:  sales, menu, members. Inspection occurred of the entity relationship diagram as well.

<p align="center">
  <img width="400" height="400" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture1.png">
</p>

1.  TOTAL AMOUNT OF EACH CUSTOMER SPENT AT THE RESTAURANT
We use the SUM and GROUP BY functions to find out total spent for each customer and JOIN function because customer_id is from sales table and price is from menu table.
Answer:
•	Customer A spent $76.
•	Customer B spent $74.
•	Customer C spent $36.


<p align="center">
  <img width="200" height="200" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture2.png">
</p>

2. How many days has each customer visited the restaurant?
Use DISTINCT and wrap with COUNT function to find out number of days customer visited the restaurant.
If we do not use DISTINCT for order_date, the number of days may be repeated. For example, if customer A visited the restaurant twice on ‘2021–01–07’, then number of days may have counted as 2 instead of 1 day.
Answer:
•	Customer A visited 4 times.
•	Customer B visited 6 times.
•	Customer C visited 2 times.



<p align="center">
  <img width="200" height="200" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture3.png">
</p>

3.  What was the first item from the menu purchased by each customer?
First, we have to create a CTE using WITH function. In the summary CTE, we use DENSE_RANK and OVER(PARTITION BY ORDER BY) to create a new column rank based on order_date.
I chose to use DENSE_RANK instead of ROW_NUMBER or RANK as the order_date is not time stamped hence, we do not know which item is ordered first if 2 or more items are ordered on the same day.  Subsequently, we GROUP BY the columns to show rank = 1 only.
Answer:
•	Customer A’s first order are curry and sushi.
•	Customer B’s first order is curry.
•	Customer C’s first order is ramen.



<p align="center">
  <img width="200" height="200" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture4.png">
</p>


4.  What is the most purchased item on the menu and how many times was it purchased by all customers?
Most purchased item on the menu is ramen.

<p align="center">
  <img width="200" height="200" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture5.png">
</p>

5.  Which item was the most popular for each customer?
We create a CTE to rank the number of orders for each product by DESC order for each customer.  Then, we generate results where rank of product = 1 only as the most popular product for individual customer.  Customer A and C’s favourite item is ramen.
Customer B enjoys all items in the menu. He/she is a true foodie.



<p align="center">
  <img width="400" height="400" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture6.png">
</p>

6.  Which item was purchased first by the customer after they became a member?
In this CTE, we filter order_date to be on or after their join_date and then rank the product_id by the order_date.  After Customer A became a member, his/her first order is curry, whereas it’s sushi for Customer B.

<p align="center">
  <img width="400" height="300" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture7.png">
</p>


7.  Which item was purchased just before the customer became a member?
Create new column rank by partitioning customer_id by DESC order_date to find out the order_date just before the customer became member
Filter order_date before join_date.
Answer:
•	Customer A’s order before he/she became member is sushi and curry and Customer B’s order is sushi. That must have been a real good sushi!



<p align="center">
  <img width="400" height="300" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture8.png">
</p>

8.  What is the total items and amount spent for each member before they became a member?
First, filter order_date before their join_date. Then, COUNT unique product_id and SUM the prices total spent before becoming member.  Answer: Before becoming members,
•	Customer A spent $ 25 on 2 items.
•	Customer B spent $40 on 2 items.



<p align="center">
  <img width="400" height="300" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture9.png">
</p>


9.  If each $1 spent equates to 10 points and sushi has a 2x points multiplier — how many points would each customer have?
the question.
•	Each $1 spent = 10 points.
•	But, sushi (product_id 1) gets 2x points, meaning each $1 spent = 20 points
So, we use CASE WHEN to create conditional statements
•	If product_id = 1, then every $1 price multiply by 20 points
•	All other product_id that is not 1, multiply $1 by 10 points
So, you can see the table below with new column, points.  
Using the table above, we SUM the price, match it to the product_id and SUM the total_points.
Answer:
•	Total points for Customer A, B and C are 860, 940 and 360.



<p align="center">
  <img width="400" height="300" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture10.png">
</p>



10.  In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi — how many points do customer A and B have at the end of January? 
breakdown the question.
•	Find out customer’s validity date (which is 6 days after join_date and inclusive of join_date) and last day of Jan 2021 (‘2021–01–21’).
WITH dates_cte AS 
(
SELECT *, 
DATEADD(DAY, 6, join_date) AS valid_date, 
EOMONTH('2021-01-31') AS last_date
FROM members AS m
)
Then, use CASE WHEN to allocate points by dates and product_name.

assumptions are
•	Day -X to Day 1 (customer becomes member (join_date), each $1 spent is 10 points and for sushi, each $1 spent is 20 points.
•	Day 1 (join_date) to Day 7 (valid_date), each $1 spent for all items is 20 points.
•	Day 8 to last day of Jan 2021 (last_date), each $1 spent is 10 points and sushi is 2x points.
Answer:
•	Customer A has 1,370points.
•	Customer B has 820 points.




<p align="center">
  <img width="400" height="400" src="https://github.com/jacquie0583/SQL-Challenge---Case-Study-/blob/main/Picture11.png">
</p>
