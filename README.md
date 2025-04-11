# 🍔 Customer Insights & Order Behavior Analysis for a Food Delivery App Launch
## Project Overview

In this project, I analyzed customer and order data from a food delivery app that launched in January 2025. Using SQL, I explored customer acquisition trends, order behaviors, promo usage, and engagement patterns to generate actionable business insights. The analysis was focused on supporting growth and marketing strategies with sample user data for 3 months.

## Dataset Description

The dataset consists of order-level information with the following key columns:

ORDER_ID: Unique identifier for each order

CUSTOMER_CODE: Unique identifier for each customer

PLACED_AT: Timestamp when the order was placed

RESTAURANT_ID: ID of the restaurant fulfilling the order

CUISINE: Type of cuisine ordered

ORDER_STATUS: Status of the order (e.g., Delivered, Cancelled)

PROMO_CODE_NAME: Name of the promo code used (if any)

Sample row:

ORDER_ID           CUSTOMER_CODE        PLACED_AT                 RESTAURANT_ID   CUISINE   ORDER_STATUS   PROMO_CODE_NAME

OF1900191801       UFDDN1991918XUY1     01-JAN-25 03.30.20 PM     KMKMH6787       Lebanese  Delivered       Tasty50

## ❓ Business Questions & SQL Insights

1. Top Outlet by Cuisine Type (Without LIMIT or TOP)
Goal: Identify the most popular outlet for each cuisine.
```sql
with cuisine as (select cuisine, RESTAURANT_ID, count(order_id) order_cnt 
from orders_test
group by cuisine, RESTAURANT_ID)

select cuisine,
       RESTAURANT_ID
from 
(select cuisine,
       RESTAURANT_ID,
       order_cnt,
       row_number()over(partition by cuisine order by order_cnt desc) rn
from cuisine)
where rn=1
;
```
Insight: Outlet "BURGER99" leads in American cuisine
Outlet "PIZZA123" leads in Italian cuisine
Outlet "SUSHI456" leads in Japanese
Outelt "KMKMH6787" leads in Lebanese
Outlet "TACO789" leads in TACO789

2. Daily New Customer Count Since Launch

Goal: Understand daily new user acquisition trends.

```sql
with cust_orders as (
select customer_code,
       min(trunc(placed_at)) as order_date
from orders_test
group by customer_code
)
select order_date
       , count(customer_code)
from cust_orders
group by order_date
order by order_date

```

Insight: Peak acquisition occurred on Jan 31, 2025 with 4 new customers.

3. One-Time Customers in Jan 2025

Goal: Identify users who ordered once in Jan and never returned.

```sql
with cust_orders as (
select customer_code
       , min(trunc(placed_at)) as order_date
       , extract(month from placed_at) order_month
       , extract(year from placed_at) order_year
       , order_id
from orders_test
group by customer_code
         , extract(month from placed_at)
         , extract(year from placed_at)
         , order_id
),
jan_cust as (
select distinct customer_code
       , count(order_id)
from cust_orders 
where (order_month=1 and order_year=2025)
group by customer_code
having count(order_id) =1
),
not_jan_cust as (
select distinct customer_code
from cust_orders where not order_month=1 and order_year=2025
)
select * 
from jan_cust  
where customer_code  not in (select customer_code from not_jan_cust);

```
Insight: 32 customers ordered only once in January 2025.

4. Dormant Customers Acquired a Month Ago with First Promo Order

Goal: Detect users who are inactive but were acquired via promo.

```sql
with promo_cust as (
select customer_code
       , min((placed_at)) first_order_date
       , max((placed_at)) latest_order_date
from orders_test
group by customer_code)

select pc.*, ot.promo_code_name
from promo_cust pc
inner join orders_test ot
on pc.customer_code = ot.customer_code
and pc.first_order_date = ot.placed_at
where latest_order_date < (sysdate-10)-7 -- the data is till 31-march and as i am doing this on 10 Apr so subtracting those days
and first_order_date < add_months(sysdate-10,-1)
and ot.promo_code_name is not null

```

Insight: 24 customers meet the reactivation criteria.

5. Trigger Campaign Post Third Order

Goal: Help the Growth Team personalize engagement.

```sql
with cust as (select customer_code
       , order_id
       , row_number()over(partition by customer_code order by placed_at) rn
       , placed_at
from orders_test)
select 
customer_code, order_id, rn
from cust 
where mod(rn,3) = 0 
and trunc(placed_at) = trunc(sysdate)-10 -- so that they can run the query every day and get the customer on that day

```

Insight: 2 customers are ready for personalized follow-up.

6. Customers with All Orders on Promo Only

Goal: Identify price-sensitive users.

```sql
with customers_with_orders as (
select customer_code
       , count(order_id) as order_cnt
       , count(case when promo_code_name is not null then 1 end) promo
       , count(case when promo_code_name is null then 1 end) no_promo
from orders_test
group by customer_code
having count(order_id)>1)
select customer_code
from customers_with_orders where no_promo=0

```

Insight: 2 customers always use promos for their orders.

7. Organic Acquisition % in Jan 2025

Goal: Measure the share of organic (non-promo) customer acquisition.

```sql
with cust as (
select customer_code
       , placed_at
       , promo_code_name
       , row_number()over(partition by customer_code order by placed_at) rn
from orders_test
where extract(month from placed_at) =1
)
select count(case when promo_code_name is null then 1 end)/count(*) *100 pct
from cust 
where rn =1
```

Insight: 43.9% of customers in January were acquired organically.

## 🎯 Final Takeaways

This SQL-driven analysis provided insights into:

Customer acquisition behavior

Promo effectiveness

Churn and re-engagement opportunities

Personalization triggers

These findings can support targeted marketing campaigns, improve retention strategies, and help product teams refine customer journeys.
