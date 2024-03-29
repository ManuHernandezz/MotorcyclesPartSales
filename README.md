<img src="https://github.com/ManuHernandezz/MotorcyclesPartSales/assets/130862652/490a414b-f110-4681-b0af-40c478f0fd6b" width="220" height="200">

# Analyzing Motorcycle Part Sales

You're working for a company that sells motorcycle parts, and they've asked for some help in analyzing their sales data!

They operate three warehouses in the area, selling both retail and wholesale. They offer a variety of parts and accept credit cards, cash, and bank transfer as payment methods. However, each payment type incurs a different fee.

The board of directors wants to gain a better understanding of wholesale revenue by product line, and how this varies month-to-month and across warehouses. You have been tasked with calculating net revenue for each product line and grouping results by month and warehouse. The results should be filtered so that only `"Wholesale"` orders are included.

They have provided you with access to their database, which contains the following table called `sales`:

## Sales
| Column | Data type | Description |
|--------|-----------|-------------|
| `order_number` | `VARCHAR` | Unique order number. |
| `date` | `DATE` | Date of the order, from June to August 2021. |
| `warehouse` | `VARCHAR` | The warehouse that the order was made from&mdash; `North`, `Central`, or `West`. |
| `client_type` | `VARCHAR` | Whether the order was `Retail` or `Wholesale`. |
| `product_line` | `VARCHAR` | Type of product ordered. |
| `quantity` | `INT` | Number of products ordered. | 
| `unit_price` | `FLOAT` | Price per product (dollars). |
| `total` | `FLOAT` | Total price of the order (dollars). |
| `payment` | `VARCHAR` | Payment method&mdash;`Credit card`, `Transfer`, or `Cash`. |
| `payment_fee` | `FLOAT` | Percentage of `total` charged as a result of the `payment` method. |

## TASK
Create a query to return product_line, the month from date, displayed as 'June', 'July', and 'August', the warehouse, and net_revenue.

  net_revenue is calculated by getting the sum of total and multiplying by 1 - payment_fee, rounding to two decimal places.
  You will need to filter client_type so that only 'Wholesale' orders are returned.
  The results should first be sorted by product_line and month in ascending order, then by net_revenue in descending order.


### QUERY
    
    SELECT product_line,
		  CASE 
		  WHEN EXTRACT (MONTH from DATE ) = 6 THEN 'June'
		  WHEN EXTRACT (MONTH from DATE) = 7 THEN 'July'
		  WHEN EXTRACT (MONTH from DATE) = 8 THEN 'August'
	    END AS Month,
	    Warehouse,
	    ROUND (SUM (total * (1-payment_fee))::numeric,2) AS net_revenue
    FROM sales
    WHERE client_type = 'Wholesale'
    GROUP BY product_line, EXTRACT(MONTH from date), warehouse
    ORDER BY product_line DESC, Month ASC, net_revenue DESC;

| product_line | month | warehouse | net_revenue |
|--------|-----------|-------------|-------------|
|Suspension & traction|August|Central|5362.59|
|Suspension & traction|August|North|4874.51|
|Suspension & traction|August|West|1069.99|
|Suspension & traction||July|Central|6392.23|
|Suspension & traction|July|North|3677.21|
|Suspension & traction|July|West|2909.98|
|Suspension & traction|June|North|7985.17|

    
