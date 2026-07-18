# Zomato-Order-Restaurant-Analysis-Using-Power-BI

End-to-end analytics project covering SQL-based data cleaning/modeling and an interactive Power BI dashboard, built to analyze Zomato's restaurant and order data and surface business-ready insights.

Domain: Food & Beverage · Business Analytics
Tools: MySQL · Power BI Desktop · Power Query · DAX


📌 Problem Statement

Zomato generates large volumes of restaurant and order data every day. Without structured analysis, it's difficult to know which restaurants perform best, how pricing affects orders, and how demand differs across cities. This project transforms that raw transactional data into actionable business intelligence covering customer preferences, restaurant performance, pricing impact, and location-based trends.

🎯 Business Objectives


Identify top-performing restaurants by rating & order volume
Analyze how pricing impacts customer orders
Understand customer preferences across cities & areas
Evaluate delivery time efficiency
Surface demand trends for future growth



🗂️ Project Structure

Zomato_Project/
├── SQL/
│   ├── 0_create_database.sql
│   ├── 01_zomato_orders_table.sql
│   ├── 02_zomato_restaurant_table.sql
│   ├── 03_Data_Profiling_&_Cleaning.sql
│   ├── 04_Data_Exploration.sql
│   ├── 05_Data_Aggregation.sql
│   └── 06_Data_Joins_&_View.sql
├── Data/
│   ├── Zomato_Orders_cleaned.csv
│   └── Zomato_Restaurants_cleaned.csv
├── PowerBI/
│   └── Zomato_Order_Restaurant_Analysis.pbix
└── Documentation/
    ├── Zomato_Project_Documentation.docx
    └── Zomato_PowerBI_10Tasks_Presentation.pptx


🛠️ Approach

1. Database Design (MySQL)

Two relational tables created in a dedicated database (ZomatoDB):

TableKey ColumnsZomato_Restaurantsrestaurant_id (PK), restaurant_name, city, area, cuisine, avg_rating, total_ratings, price_range, delivery_availableZomato_Ordersorder_id (PK), restaurant_id (FK), customer_id, order_date, order_time, delivery_time, total_cost, item_count, payment_method, customer_rating

A foreign key (fk_restaurant) enforces referential integrity — every order must map to a valid restaurant.

2. Data Profiling & Cleaning


Row counts and duplicate-key checks (GROUP BY … HAVING COUNT(*) > 1) on both tables
Null audits across every column via conditional SUM(column IS NULL) queries
Text standardization using TRIM() on payment_method, restaurant_name, city, area, cuisine
Verification via post-cleanup aggregation checks


3. Data Exploration & Aggregation


Restaurant count per city
Top 5 cities by order volume (JOIN Orders + Restaurants)
Total revenue per restaurant
Average order value per city
Top 5 restaurants by total sales


4. Data Joins & View

A consolidated view (vw_Zomato_Analysis) joins Orders + Restaurants on restaurant_id, used for validation and ad-hoc analysis alongside the two-table Power BI model.

5. Power BI Data Modeling


Cleaned Zomato_Orders and Zomato_Restaurants tables imported and connected via a Many-to-One relationship on restaurant_id
Single-direction cross-filtering (Restaurants → Orders)


Key DAX Measures:

DAXTotal Revenue            = SUM(Zomato_Orders[total_cost])
Average Order Value      = AVERAGE(Zomato_Orders[total_cost])
Total Orders              = COUNTROWS(Zomato_Orders)
Total Restaurants        = DISTINCTCOUNT(Zomato_Restaurants[restaurant_id])
Average Delivery Time    = AVERAGE(Zomato_Orders[delivery_time])
Average Customer Rating  = AVERAGE(Zomato_Orders[customer_rating])


📊 Dashboard Pages

PageContentsMarket OverviewRestaurant distribution by city, order distribution by city (%), daily sales trend, correlation scatter plots (delivery time, order value, total ratings vs. rating)Restaurant PerformanceTop 5 restaurants by total sales, revenue by area (tree map), order density by city × area (conditional-formatted heat map)KPI & Restaurant SummaryKPI cards (Total Revenue, AOV, Total Orders, Avg Rating, Total Restaurants, Avg Delivery Time), filterable restaurant-wise sales summary tableExecutive DashboardCombined view of all visuals with synchronized city/area/price-range/cuisine/payment-method slicers


✅ Task Checklist (per project brief)

#TaskVisual1Number of Restaurants per CityBar chart2Percentage of Orders from Different CitiesPie chart3Order Amount Trends Over TimeLine chart4Correlation of Factors Affecting Average RatingScatter plots5Top 5 Restaurants by Total SalesColumn chart6Revenue by Area Using a Tree MapTree map7Order Density by City Using a Heat MapConditional-formatted matrix8KPI Cards — Total Revenue & Average Order ValueCards9Restaurant-Wise Sales Summary in a TableSortable/filterable table10Power BI Dashboard — Combining All VisualsExecutive Dashboard page


💡 Key Insights


Supply vs. demand: Mumbai (115 restaurants) and Bangalore (109) lead both restaurant count and order volume (235 and 215 orders respectively); Delhi trails on both fronts.
Revenue driver: Revenue differences across areas are driven by order volume, not order value — average order value is nearly flat (₹1,006–₹1,080) across all 5 areas, while Area_A leads on order count (247).
Ratings: No strong correlation between customer rating and price, delivery time, or total ratings — satisfaction is likely driven by other factors (e.g. food quality, order accuracy).
Delivery time: Average of 51.94 minutes is on the higher side; Mumbai and Bangalore (specifically their Area_A zones) are the densest order pockets and the first place to investigate logistics.
Overall KPIs: ₹1.04M total revenue · 1,000 orders · ₹1.04K average order value · 500 restaurants · 3.03 average rating.


📈 Business Recommendations


Prioritize restaurant onboarding and delivery capacity in Delhi and Chennai, where supply and order volume lag the top cities.
Focus growth on driving more orders per area rather than raising order value — pricing has limited room to move revenue.
Prioritize food-quality and order-accuracy audits over price-based promotions to lift average rating.
Run a root-cause logistics review starting in Mumbai/Bangalore Area_A, the highest order-density zones.
Track the dashboard on a recurring (weekly/monthly) cadence to confirm rating and delivery-time trends move in the right direction.



📦 Deliverables


MySQL Database Export — cleaned Zomato_Orders and Zomato_Restaurants CSVs + all SQL scripts
Power BI Report (.pbix) — 4-page interactive dashboard
Project Documentation — Word doc + PowerPoint covering approach, dashboard walkthrough, insights, and recommendations
