# TheLook-eCommerce-customer-behavior SQL
## 📌 project introduce
Exploring the thelook_ecommerce dataset on Google BigQuery to practice SQL basics: filtering data, joining tables, and deriving business insights through metric calculation
---

## 🔍 task 1: Filtering
**analysis target：** identify female user in China。
**SQL code：**
```sql
SELECT 
    id, first_name, last_name, city, country
FROM 
    `bigquery-public-data.thelook_ecommerce.users`
WHERE 
    country = 'China' 
    AND gender = 'F';
```
💡 Key Insight: By filtering specific demographics, the marketing team can create localized promotional campaigns for the female segment in China, optimizing ad spend.

## 🔍 task 2: Joining & Grouping
**analysis target：** join orders and users tables to identify the top 10 cities by order volume and evaluate logistics prioritization.
**SQL code：**
```sql
SELECT 
    u.city, 
    COUNT(o.order_id) AS order_count
FROM 
    `bigquery-public-data.thelook_ecommerce.orders` AS o
JOIN 
    `bigquery-public-data.thelook_ecommerce.users` AS u
    ON o.user_id = u.id
GROUP BY 
    u.city
ORDER BY 
    order_count DESC
LIMIT 10;
```
💡 Key Insight: Identifying high-volume cities allows the operations team to prioritize warehouse stock levels and negotiate better rates with local courier services in these specific hubs.

## 🔍 task 3: Business Insights
**analysis target：** Calculate category return rates to identify high-risk products for quality control.
**SQL code：**
```sql
SELECT 
    p.category,
    COUNT(oi.id) AS total_items,
    SUM(CASE WHEN oi.status = 'Returned' THEN 1 ELSE 0 END) AS returned_items,
    ROUND(SUM(CASE WHEN oi.status = 'Returned' THEN 1 ELSE 0 END) / COUNT(oi.id) * 100, 2) AS return_rate
FROM 
    `bigquery-public-data.thelook_ecommerce.order_items` AS oi
JOIN 
    `bigquery-public-data.thelook_ecommerce.products` AS p
    ON oi.product_id = p.id
GROUP BY 
    p.category
ORDER BY 
    return_rate DESC;
```
💡 Key Insight: Categories with abnormally high return rates (e.g., Swimwear or Intimates) may indicate issues with sizing guides or product descriptions. This data-driven insight prompts a review of the product detail pages (PDP) to reduce future returns.

---

## 🛠️ Skills Demonstrated
* **SQL Proficiency:** Multi-table JOINS, Aggregate Functions (`COUNT`, `SUM`), Data Filtering (`WHERE`, `AND`), and Sorting (`ORDER BY`).
* **Cloud Data Warehousing:** Hands-on experience with **Google BigQuery** and navigating large-scale public datasets.
* **Business Intelligence:** Translating raw data into actionable insights to improve operational efficiency and product quality.
