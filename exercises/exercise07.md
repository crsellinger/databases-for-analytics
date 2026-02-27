# Exercise 07: Bringing It All Together

- Name: Caleb Sellinger
- Course: Database for Analytics
- Module: 7
- Tools Used: pgAdmin4

***

### Initial Data Source
***

Minimum requirements on dataset:
- at least 3 tables
- One table must have at least 1000 rows
- Other 2 must have at least 100 rows
- One *date*, one *numeric*, and one *string* data type

[Source Data Here](https://www.sqlservertutorial.net/getting-started/sql-server-sample-database/)

Converted sql to psql files in data folder


### Data Dictionary
***

Tables include:

 | Table | Column | Data Type | Description |
 | --- | --- | --- | --- |
 | production.brands (Rows: 9 Columns: 2) | brand_id | int | Primary key of unique IDs |
 || brand_name | varchar | VarChar of brand names |
 | production.categories (Rows: 7 Columns: 2) | category_id | int | Primary key of unique IDs |
 || category_name | varchar | VarChar of categories |
 | production.products (Rows: 321 Columns: 6) | product_id | int | Primary key of unique IDs |
 || product_name |  | Name of bike model |
 || brand_id | int | Foreign key for products.brands |
 || category_id | int | Foreign key for products.categories |
 || model_year |  | Year of bike model |
 || list_price |  | MSRP of bike model |
 | production.stocks (Rows: 939 Columns: 3) | store_id | int | Primary key of unique IDs, related to sales.stores |
 || product_id | int | Primary key of unique IDs |
 || quantity | int | Number of bikes on-hand at store |
 | sales.customers (Rows: 1445 Columns: 9) | customer_id | int | Primary key of unique customers IDs |
 || first_name || First name of customer |
 || last_name || Last name of customer |
 || phone || Phone number of customer |
 || email | | Email address of customer|
 || street || Street address of customer |
 || city || Customer's city location |
 || state || Customer's state location |
 || zip_code || Customer's zipcode (5 digit zip, no postal code) |
 | sales.order_items (Rows: 4722 Columns: 6) | order_id | int | Primary key of unique order IDs |
 || item_id | int | Primary key, ordered item's unique ID |
 || product_id | int | Foreign key related to sales.order_items |
 || quantity | int | Amount ordered |
 || list_price || MSRP of single unit |
 || discount || Percentage as decimal of discount applied to item_id |
 | sales.orders (Rows: 1615 Columns: 8) | order_id | int | Primary key of unique order IDs |
 || customer_id | int | Foreign key to sales.customers, unique customer ID number |
 || order_status || Order status code, 1 - 4|
 || order_date | datetime | Date order was put in |
 || required_date | datetime | Date customer was supposed to receive order |
 || shipped_date | datetime | Date order was shipped |
 || store_id | int | Foreign key to production.stocks and sales.stores |
 || staff_id | int | Foreign key to sales.staff |
 | sales.staffs (Rows: 10 Columns: 8) | staff_id | int | Primary key of unique staff IDs |
 || first_name || First name of staff member |
 || last_name || Last name of staff member|
 || email || Email address of staff member |
 || phone || Phone number of staff member |
 || active || Employment indicator of staff member |
 || store_id | int | Store ID of staff member's working location |
 || manager_id | int | Manager ID of staff member's manager |
 | sales.stores (Rows: 3 Columns: 8) | store_id | int | Primary key of unique store IDs |
 || store_name || Name of store |
 || phone || Phone number of store |
 || email || Email address of store |
 || street || Street address of store|
 || city || City that store is located in |
 || state || State that store is located in |
 || zip_code || Zip code store is located in |

 ### Obstacles
 ***

Finding usable relational datasets without having to create my own.

After that, converting the SQL scripts to work in PostgreSQL.


 ### Select statements
 ***

To get number of rows for each table:

```sql
SELECT COUNT(*)
FROM '[Schema]'.'[table]'
```

To get number of columns for each table:

```sql
SELECT COUNT(*)
FROM information_schema.columns
WHERE table_schema = 'table_schema'
  AND table_name = 'table_name
```

Examples:

![This image](image.png)

![That image](image-1.png)


 ### Queries
 ***

 Customers with more than 2 orders (Group By):

```sql
SELECT customer_id, COUNT(customer_id) as No_of_orders
FROM sales.orders
GROUP BY customer_id
HAVING COUNT(customer_id) > 2
```

What brands do customers order most?

```sql
SELECT s.order_id, MIN(p.list_price), b.brand_name, COUNT(b.brand_name)
FROM sales.order_items AS s
INNER JOIN production.products AS p
	ON s.product_id = p.product_id
JOIN production.brands as b
	ON p.brand_id = b.brand_id
GROUP BY b.brand_name, s.order_id
ORDER BY COUNT(b.brand_name) DESC;
```

This query finds the brand that each customer has ordered most.
