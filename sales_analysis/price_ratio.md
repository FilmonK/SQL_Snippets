
# Price ratio

# Description
Get the price of an order, as well as average price based on several orders placed before and after said order.
Finally, find the ratio between the price of the order and average price.


```sql
SELECT
    order_date,
    total_price,
    AVG(total_price) OVER(ORDER BY order_date ROWS BETWEEN 5 PRECEDING
                         AND 5 FOLLOWING) AS average_total_price,
    total_price / AVG(total_price) OVER(ORDER BY order_date ROWS BETWEEN 5 PRECEDING AND 5 FOLLOWING) AS price_ratio
FROM ordered_items
```
