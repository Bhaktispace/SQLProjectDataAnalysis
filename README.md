# 🍔 Customer Insights & Order Behavior Analysis for a Food Delivery App Launch
## 📚 Project Overview

In this project, I analyzed customer and order data from a food delivery app that launched in January 2025. Using SQL, I explored customer acquisition trends, order behaviors, promo usage, and engagement patterns to generate actionable business insights. The analysis was focused on supporting growth and marketing strategies with sample user data for 3 months.

## 📂 Dataset Description

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

1. 🍽️ Top Outlet by Cuisine Type (Without LIMIT or TOP)

Goal: Identify the most popular outlet for each cuisine.

-- SQL logic used to rank outlets per cuisine without LIMIT

Insight: Outlet "KMKMH6787" leads in Lebanese cuisine.

2. 📅 Daily New Customer Count Since Launch

Goal: Understand daily new user acquisition trends.

-- SQL to calculate number of new users by day

Insight: Peak acquisition occurred on Jan 15, 2025 with 550 new customers.

3. 🤔 One-Time Customers in Jan 2025

Goal: Identify users who ordered once in Jan and never returned.

-- SQL to find customers with one order in Jan only

Insight: 350 customers ordered only once in January 2025.

4. ❌ Dormant Customers Acquired a Month Ago with First Promo Order

Goal: Detect users who are inactive but were acquired via promo.

-- SQL to find inactive users who haven't ordered in the last 7 days

Insight: 42 customers meet the reactivation criteria.

5. 🎉 Trigger Campaign Post Third Order

Goal: Help the Growth Team personalize engagement.

-- SQL to find users who just completed their third order

Insight: 178 customers are ready for personalized follow-up.

6. 🤑 Customers with All Orders on Promo Only

Goal: Identify price-sensitive users.

-- SQL to find users with only promo-based orders

Insight: 73 customers always use promos for their orders.

7. ✨ Organic Acquisition % in Jan 2025

Goal: Measure the share of organic (non-promo) customer acquisition.

-- SQL to calculate percent of users with no promo code on first order

Insight: 62% of customers in January were acquired organically.

📊 Optional Visuals

Visuals can help tell the story better. Suggested charts:

Line chart: Daily new customers

Pie chart: Promo vs. Organic acquisition

Bar chart: Top cuisine types

Tools you can use:

Excel / Google Sheets

Tableau / Power BI

Python (matplotlib, seaborn)

🎯 Final Takeaways

This SQL-driven analysis provided insights into:

Customer acquisition behavior

Promo effectiveness

Churn and re-engagement opportunities

Personalization triggers

These findings can support targeted marketing campaigns, improve retention strategies, and help product teams refine customer journeys.

🔍 About This Project

This project showcases my ability to derive insights from raw transactional data using SQL. It simulates a real-world scenario of launching a new product and needing actionable data to support business growth and customer retention strategies.

