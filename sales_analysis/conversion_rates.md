# Conversion rates for customers in 2022

# Description
Get an overview on conversion rates for customers in 2022 broken down by different timeline requirements

```sql
SELECT
 DATE_PART('month', initiation_date) AS month,
 channel_name,
 COUNT(*) AS registered_totals,
 COUNT(CASE
       WHEN order_date IS NULL
       THEN customer_id
       END) AS no_sale,
 COUNT(CASE
       WHEN order_date - initiation_date < INTERVAL '30' day
       THEN customer_id
       END) AS within_first_month,
 COUNT(CASE
       WHEN order_date - initiation_date >= INTERVAL '30' day
       	AND order_date - initiation_date < INTERVAL '90' day
       THEN customer_id
       END) AS within_quarter,
 COUNT(CASE
       WHEN order_date - initiation_date >= INTERVAL '90' day
       THEN customer_id
       END) AS after_one_quarter
FROM customers cu
JOIN sales_category sc
 ON sc.id = cu.sales_category.id
WHERE DATE_PART('year', initiation_date) = '2022'
GROUP BY channel_name, DATE_PART('month', initiation_date)
ORDER BY DATE_PART('month', initiation_date)

```
