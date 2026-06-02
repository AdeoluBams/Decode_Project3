# Decode_Project3

## Project Overview
The task given in this project is to perform analysis using the provided to get business insight and the data information

## Data Sources
The data is provided by Decode_labs, it is in format "  Dataset for Data Analytics.xlsx" in the repository

## Tools
- Excel - Data Cleaning 
  - [Download Here](https://microsoft.com)
- PostgreSQl - Data Analysis
  - [Download Here](https://www.postgresql.org/download)

## Data Cleaning Process
- Data Loading and Inspection
- Checking for duplicates
- Columns standardisation
- Date format transformation (yyyy-MM-dd)
- Coverting null values in the coupon column to "No Coupon"

## Query Highlight

```sql
-- Average Order Value
WITH average_order_calc AS(
	SELECT
		SUM("TotalPrice") AS total_amount,
		COUNT("OrderID") AS total_orders
	FROM public."Decode_Labs"
)
SELECT
	ROUND(
		(total_amount / total_orders),0) AS average_order_value
FROM average_order_calc;
```
```sql
-- Order Cancelling Rate
WITH cancelled_calc AS (
	SELECT
		COUNT("OrderStatus") AS cancelled_order_status
	FROM public."Decode_Labs"
	WHERE "OrderStatus" = 'Cancelled'
)
SELECT
	ROUND(
	c.cancelled_order_status * 100.0 / COUNT(o."OrderStatus"),2) AS Cancelling_rate
FROM public."Decode_Labs"o
CROSS JOIN cancelled_calc c
GROUP BY cancelled_order_status;
```
```sql
-- Yearly Growth rate
WITH yearly_revenue AS(
	SELECT
		EXTRACT(YEAR FROM "Date") AS year_,
		SUM("TotalPrice") AS total_amount
	FROM public."Decode_Labs"
	GROUP BY year_
	ORDER BY year_ ASC
),
Preceding_rev_calc AS(
	SELECT
		year_,
		total_amount,
		LAG(total_amount) OVER(ORDER BY year_) AS prev_revenue
	FROM yearly_revenue
)
SELECT
	year_,
	total_amount,
	ROUND(
		(total_amount - prev_revenue) * 100.0 / prev_revenue,2) revenue_growth_perc
FROM Preceding_rev_calc;
```
- Full Query link is provided in the Repository

# Findings
- The business handled a total of 1200 orders
- Average order value is 1054
- Printer was the most ordered product, it was purchased 181 times
- Chair generated the most revenue, a revenue of $195,620.11 and was sold 562 times
- Leads generated through INSTAGRAM generated the most revenue, a total of $275,285.45
- Online Payment_method, it was used 258 times
- [Download Full report here]()
- Full Report is also provided in the repository

## References
[Stack Overflow](https://stackoverflow.com/)



