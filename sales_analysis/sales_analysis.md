
# Narrow sales down to specific state

# Description
Query to get list of invoiced items by state and identify selling percentages 

```sql
--generate timeseries in hourly increments
WITH state_sold AS
    (
        SELECT
            i.billing_state,
            i.invoice_id,
            il.product_id

        FROM invoice i
        JOIN invoice_line il on i.invoice_id = il.invoice_id
        WHERE billing_state = 'TX'
    )
    
SELECT
    g.name as field,
    count(listed.invoice_id) as total,
    
--subquery to get total invoice amount in order to divide by each item by this total to get proper percentages
    CAST(count(listed.invoice_id) as float) / (
        SELECT
        COUNT(invoice_id) 
        FROM state_sold
    ) * 100 as percentage
    
FROM field f
JOIN sub_prod_id spid ON f.field_id = spid.field_id
JOIN state_sold us ON us.product_id = t.product_id
GROUP BY 1
ORDER BY 2 DESC
```
