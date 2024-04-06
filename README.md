
# SQL Practice 2: Answering Business Questions using MySQL 

## Overview: 

In this exercise, I'll be showcasing my SQL skills by answering a set of business questions, simulating the role of a data analyst.<br> This practice session is aimed at refining and expanding my proficiency in MySQL.

### Scenario 

As a newly hired data analyst at **Taste of Worlds Cafe**, renowned for its diverse culinary offerings, I'm tasked with delving into customer data to pinpoint the most and least popular items from our recently launched menu. My mission is to uncover the preferences of our top clientele by crafting SQL queries to analyze the restaurant's database.  



### Additional Information: 
Editor | Database Name | Source
:---:|:---:|:---:
MySQL Workbench (Version 8.031) | restaurant_db (2 tables) | Maven Analytics



## Questions

* View the menu_items table and write a query to find the number of items on the menu.

```sql
SELECT *
FROM menu_items;
```
```sql
SELECT
  COUNT(menu_items.menu_item_id) AS Total_Menu_Items
FROM menu_items;
```

* What are the least and most expensive items on the menu?

```sql
SELECT
  MIN(menu_items.price) AS Least_Expensive_Items,
  MAX(menu_items.price) AS Most_Expensive_Items
FROM menu_items;
```

* How many Italian dishes are on the menu? What are the least and most expensive Italian dishes on the menu?

```sql
SELECT
  menu_items.category,
  COUNT(menu_item_id) AS Total_Menu_Items,
  MIN(menu_items.price) AS Least_Expensive_Item,
  MAX(menu_items.price) AS Most_Expensive_Item
FROM menu_items
GROUP BY menu_items.category
HAVING menu_items.category = 'Italian';
```

* How many dishes are in each category? What is the average dish price within each category?

```sql
SELECT
  menu_items.category,
  COUNT(menu_item_id) AS Items_per_Category,
  AVG(menu_items.price) AS Avg_Price
FROM menu_items
GROUP BY menu_items.category;
```


* View the order_details table. What is the date range of the table?

```sql
SELECT *
FROM order_details;
```
```sql
SELECT
  MIN(order_details.order_date) AS 'Earliest_Date',
  MAX(order_details.order_date) AS 'Oldest_Date'
FROM order_details;
```


* How many orders were made within this date range? How many items were ordered within this date range?

```sql
SELECT
  COUNT(DISTINCT order_details.order_id) AS Total_Orders,
  COUNT(order_details.order_details_id) AS Total_Items_Ordered
FROM order_details
WHERE order_details.order_date BETWEEN '2023-01-01' AND '2023-03-31';
```

* Which orders had the most number of items?

```sql
SELECT
  order_details.order_id,
  COUNT(order_details.order_details_id) AS Total_Items_Ordered
FROM order_details
GROUP BY order_details.order_id
ORDER BY total_items_ordered DESC;
```

```sql
SELECT
  order_details.order_id,
  COUNT(order_details.order_details_id) AS Total_Items_Ordered
FROM order_details
GROUP BY order_details.order_id
HAVING total_items_ordered >= 14
ORDER BY total_items_ordered DESC;
```


* How many orders had more than 12 items?

```sql
SELECT COUNT(*)
FROM
  (SELECT
    order_details.order_id,
    COUNT(order_details.order_details_id) AS Total_Items_Ordered
  FROM order_details
  GROUP BY order_details.order_id
  HAVING total_items_ordered > 12) AS Total_Items;
```


* Combine the menu_items and order_details tables into a single table

```sql
SELECT *
FROM order_details
LEFT JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id;
```


* What were the least and most ordered items? What categories were they in?

```sql
SELECT
    menu_items.item_name,
    menu_items.category,
    COUNT(order_details.item_id) AS Num_Orders
FROM order_details
LEFT JOIN menu_items
    ON order_details.item_id = menu_items.menu_item_id
GROUP BY menu_items.item_name, menu_items.category
ORDER BY num_orders DESC;
```


* What were the top 5 orders that spent the most money?

```sql
SELECT
  order_details.order_id,
  SUM(menu_items.price) AS Total_Price
FROM order_details
LEFT JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
GROUP BY order_details.order_id
ORDER BY total_price DESC
LIMIT 5;
```


* View the details of the highest spend order. Which specific items were purchased?

```sql
SELECT
  order_details.order_id,
  menu_items.item_name,
  COUNT(menu_items.menu_item_id) AS Item_Qty,
  SUM(menu_items.price) AS Total_Price
FROM order_details
LEFT JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
GROUP BY order_details.order_id, menu_items.item_name
HAVING order_details.order_id = 440;
```


* View the details of the top 5 highest spend orders. 

```sql
SELECT
  order_details.order_id,
  menu_items.category,
  menu_items.item_name,
  COUNT(menu_items.menu_item_id) AS Item_Qty
FROM order_details
LEFT JOIN menu_items
  ON order_details.item_id = menu_items.menu_item_id
GROUP BY order_details.order_id, menu_items.category, menu_items.item_name
HAVING order_details.order_id IN (440,2075,1957,330,2675);
```


## Project Feedback
*To be updated*
