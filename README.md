#   Restaurant Menu & Orders Analysis (SQL Project)

##  Project Overview
This project explores a restaurant's **menu data** and **customer orders** to understand how customers are reacting to a new menu.  

The analysis is structured around three key objectives:  
1. **Explore the `menu_items` table** → Understand what’s on the menu.  
2. **Explore the `order_details` table** → Review ordering trends and data collected.  
3. **Combine both tables** → Analyze customer behavior and spending patterns.  

---

##  Objective 1: Explore the `menu_items` Table
### Key Questions:
- How many items are on the menu?  
- What are the **least and most expensive items**?  
- How many **Italian dishes** are available?  
- What is the **price range** within Italian dishes?  
- How many dishes per category?  
- What is the **average price** per category?  

### Queries:
```sql
-- Count total menu items
SELECT COUNT(*) AS total_item
FROM menu_items;

-- Cheapest and most expensive items with names
SELECT item_name, price 
FROM menu_items
WHERE price = (SELECT MIN(price) FROM menu_items)   
   OR price = (SELECT MAX(price) FROM menu_items);

-- Count dishes per category
SELECT category, COUNT(menu_item_id) AS total_each_dish
FROM menu_items
GROUP BY category;

-- Average dish price per category
SELECT category, AVG(price) AS average_price
FROM menu_items
GROUP BY category;

## Objective 2: Explore the order_details Table
### Key Questions:
- What is the date range of the dataset?
- How many orders were made in this period?
- How many unique items were ordered?
- Which orders had the most items?
- How many orders had more than 12 items?

### Queries:
-- Date range of orders
SELECT MIN(order_date) AS start_date,
       MAX(order_date) AS end_date
FROM order_details;

-- Total number of unique orders
SELECT COUNT(DISTINCT order_id) AS num_orders
FROM order_details;

-- Orders with the most items
SELECT order_id, COUNT(item_id) AS most_order 
FROM order_details
GROUP BY order_id
ORDER BY most_order DESC;

-- Number of orders with more than 12 items
SELECT COUNT(*) AS num_order_morethan_12_items
FROM (
  SELECT order_id, COUNT(item_id) AS morethan_12_items
  FROM order_details
  GROUP BY order_id
  HAVING morethan_12_items > 12
) subquery;

## Objective 3: Analyze Customer Behavior

- By joining the menu_items and order_details tables, we can analyze customer reactions to the new menu.

### Key Questions
- What were the least and most ordered items?
- Which categories do these items belong to?
- What are the top 5 orders by spending?
- What insights can we gather from high spending orders?

### Queries

-- Most and least ordered items with categories
SELECT item_name, category, COUNT(order_details_id) AS num_purchases
FROM order_details od
LEFT JOIN menu_items mi
  ON od.item_id = mi.menu_item_id
GROUP BY item_name, category
ORDER BY num_purchases DESC;

-- Top 5 highest spending orders
SELECT order_id, SUM(price) AS top_5_orders
FROM order_details od
LEFT JOIN menu_items mi
  ON od.item_id = mi.menu_item_id
GROUP BY order_id
ORDER BY top_5_orders DESC
LIMIT 5;

-- Details of the highest spending order
SELECT order_date, category, item_name, SUM(price) AS highest_orders
FROM order_details od
LEFT JOIN menu_items mi
  ON od.item_id = mi.menu_item_id
GROUP BY order_date, category, item_name
ORDER BY highest_orders DESC 
LIMIT 1;

### Key Insights
- The restaurant menu is well-diversified across categories, with Italian dishes standing out.
- Some items are ordered much more frequently than others, suggesting popular favorites.
- A small number of high-value orders contribute significantly to revenue.
- Understanding order size distribution helps optimize staffing and inventory.

## Skills Demonstrated

- SQL querying: JOINs, GROUP BY, HAVING, subqueries, aggregations.
- Data exploration and business insight generation.
- Ability to structure and communicate findings clearly.

# Business Recommendations – Restaurant Menu & Orders Analysis

This document summarizes the **business recommendations** derived from the SQL analysis of the `menu_items` and `order_details` tables.  
The goal is to provide actionable insights that can guide **menu optimization, marketing strategy, and operations management**.  

---

## 1. Optimize Pricing Strategy
- The **most expensive dishes** are less frequently ordered compared to mid-priced items.  
- Introduce **combo offers, discounts, or bundles** on high-priced dishes to improve sales.  
- Monitor categories where the **average price is higher than others** to avoid customer drop-offs.  

---

## 2. Promote Best-Selling Items
- Certain dishes are consistently **top sellers**.  
- Recommendations:  
  - Highlight them on the menu (e.g., “Chef’s Special” or “Most Popular”).  
  - Build **marketing campaigns** around these items.  
  - Ensure they are **always in stock** to avoid missed sales.  

---

## 3. Review Underperforming Dishes
- A small number of items are ordered **very rarely**.  
- Options to consider:  
  - Adjust **price, recipe, or presentation**.  
  - Bundle them with popular items to increase visibility.  
  - Remove them from the menu if they consistently underperform.  

---

## 4. Leverage High-Value Customers
- A few orders contribute a **disproportionately large share of revenue**.  
- Retention strategies:  
  - Loyalty programs (e.g., points, rewards, or exclusive deals).  
  - Personalized offers or free add-ons for large orders.  
  - Encourage **repeat purchases** from these high-value customers.  

---

## 5. Manage Kitchen & Inventory Better
- Orders with **12+ items** are uncommon but logistically demanding.  
- Recommendations:  
  - Offer **pre-order options** for large groups.  
  - Train staff to handle high-volume orders efficiently.  
  - Use order data to **align kitchen prep and inventory levels**.  

---

## 6. Category Insights
- **Italian dishes** represent a strong portion of the menu and could be expanded further.  
- Categories with low order volumes may need **rebranding, promotions, or replacements**.  

---

## 7. Data-Driven Marketing
- Use insights on top categories and items to create **targeted campaigns**.  
- Examples:  
  - “Italian Night Specials” promotions.  
  - **Bundling popular and less-ordered items** together.  
  - Seasonal offers highlighting best sellers.  

---

##  Final Takeaway
- A **small number of items drive most sales** → focus on them.  
- **High-spending customers are crucial** → reward and retain them.  
- The menu could be **streamlined** by optimizing or removing underperforming dishes.  
- SQL analysis provides **clear direction** for **pricing, marketing, and operations improvements**.


## Next Steps
- Visualize insights in Power BI.

















