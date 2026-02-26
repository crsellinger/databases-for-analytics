# Exercise 07: Bringing It All Together

- Name: Caleb Sellinger
- Course: Database for Analytics
- Module: 7
- Tools Used:

***

### Initial Data Source
***

Minimum requirements on dataset:
- at least 3 tables
- One table must have at least 1000 rows
- Other 2 must have at least 100 rows
- One *date*, one *numeric*, and one *string* data type

[Source Data Here](https://www.sqlservertutorial.net/getting-started/sql-server-sample-database/)

Converted sql to psql in files data folder

### Format of Data
**Include count of column and rows**
***

- Production
   - brands:
     - Rows: 9
     - Columns:
   - categories
   - products
   - stocks
 - Sales
   - customers
   - order_items
   - orders
   - staffs
   - stores


### Data Dictionary
***

Tables include:

 | Table | Column | Description |
 | --- | --- | --- |
 | production.brands | brand_id | Primary key of unique IDs |
 || brand_name | VarChar of brand names |
 | production.categories | category_id | Primary key of unique IDs |
 || category_name | VarChar of categories |
 | production.products | product_id | Primary key of unique IDs |
 || product_name | Name of bike model |
 || brand_id | Foreign key for products.brands |
 || category_id | Foreign key for products.categories |
 || model_year | Year of bike model |
 || list_price | MSRP of bike model |
 | production.stocks | store_id | Primary key of unique IDs, related to sales.stores |
 || product_id | Primary key of unique IDs |
 || quantity | Number of bikes on-hand at store |
 | sales.customers | customer_id | Primary key of unique customers IDs |
 || first_name | First name of customer |
 || last_name | Last name of customer |
 || phone | Phone number of customer |
 || email | Email address of customer|
 || street | Street address of customer |
 || city | Customer's city location |
 || state | Customer's state location |
 || zip_code | Customer's zipcode (5 digit zip, no postal code) |
 | sales.order_items | order_id | Primary key of unique order IDs |
 || item_id | Primary key, ordered item's unique ID |
 || product_id| Foreign key related to sales.order_items |
 || quantity | Amount ordered |
 || list_price | MSRP of single unit |
 || discount | Percentage as decimal of discount applied to item_id |
 | sales.orders | order_id | Primary key of unique order IDs |
 || customer_id | Foreign key to sales.customers, unique customer ID number |
 || order_status | Order status code, 1 - 4|
 || order_date | Date order was put in |
 || required_date | Date customer was supposed to receive order |
 || shipped_date | Date order was shipped |
 || store_id | Foreign key to production.stocks and sales.stores |
 || staff_id | Foreign key to sales.staff |
 | sales.staffs | staff_id | Primary key of unique staff IDs |
 || first_name | First name of staff member |
 || last_name | Last name of staff member|
 || email | Email address of staff member |
 || phone | Phone number of staff member |
 || active | Employment indicator of staff member |
 || store_id | Store ID of staff member's working location |
 || manager_id | Manager ID of staff member's manager |
 | sales.stores | store_id | Primary key of unique store IDs |
 || store_name ||
 || phone ||
 || email ||
 || street ||
 || city ||
 || state ||
 || zip_code ||

 ### Obstacles
 ***



 ### Select statements
 ***

To get rows for each table:

```sql
SELECT COUNT(*)
FROM '[Schema]'.'[table]'
```


 ### Queries
 ***

```sql

```
