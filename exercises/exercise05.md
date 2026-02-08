# Exercise 05: SQLDA Database - Dates, Data Quality, Arrays, and JSON

- Name: Caleb Sellinger
- Course: Database for Analytics
- Module: 5
- Database Used:  `sqlda` (Sample Datasets)
- Tools Used: PostgreSQL (pgAdmin or psql)

---

## Instructions

- Use the **sqlda** database from the "Loading the Sample Datasets" instructions.
- For each SQL task:
  - Include your SQL in a fenced code block
  - Execute it and include a **screenshot** showing the query and results
- Store screenshots in the `screenshots/` folder and embed them below each answer.
- For explanation questions:
  - Write your answer in complete sentences
  - Include a screenshot if requested

---

## Question 1

Using the `sqlda` database, write the SQL needed to show a **list of years** that emails were sent.

Your results should list years like this (order matters):

```
year
2011
2013
2014
2015
2016
2017
2018
2019
```

### SQL

```sql
SELECT distinct
	EXTRACT(year FROM sent_date) AS Year
FROM emails
ORDER By Year ASC;
```

### Screenshot

![A horse with no name](screenshots/M5_1.png)

---

## Question 2

Using the `sqlda` database, write the SQL needed to show the **number of messages sent by year**, ordered by year (as shown in the prompt).

Output should resemble:

```
count   year
...
```

### SQL

```sql
SELECT
	Count(email_id), EXTRACT(year FROM sent_date) AS year
From emails
GROUP BY sent_date
ORDER BY year ASC;
```

### Screenshot

![I have no mouth, but I must scream](screenshots/M5_2.png)

---

## Question 3

Using the `sqlda` database, write the SQL needed to show:
- the **sent date**
- the **opened date**
- the **interval** between the two

Only include emails that contain **both** a sent date and an opened date.

### SQL

```sql
SELECT
	sent_date, opened_date, opened_date - sent_date AS Interval
From emails
WHERE
	sent_date IS NOT null
	and
	opened_date IS NOT null
```

### Screenshot

![When the debate is lost, slander becomes the tool of the loser.](screenshots/M5_3.png)

---

## Question 4

Using the `sqlda` database, write the SQL needed to show emails that contain an **opened date BEFORE the sent date**.

### SQL

```sql
SELECT *
FROM emails
WHERE opened_date < sent_date
```

### Screenshot

![alt text](screenshots/M5_4.png)

---

## Question 5

Using the `sqlda` database: there are **over 100 emails** that contain an opened date **BEFORE** the sent date.

After looking at the data, **why is this the case?**

### Answer

No timezone. So someone in EST could send someone in PST an email and they could open it up earlier than it was sent.

### Screenshot (if requested by instructor)

![To live is to suffer, to survive is to find some meaning in the suffering](screenshots/M5_5.png)

---

## Question 6

Using the `sqlda` database, explain in your own words what the following code does:

```sql
CREATE TEMP TABLE customer_points AS (
    SELECT
        customer_id,
        point(longitude, latitude) AS lng_lat_point
    FROM customers
    WHERE longitude IS NOT NULL
    AND latitude IS NOT NULL
);

CREATE TEMP TABLE dealership_points AS (
    SELECT
        dealership_id,
        point(longitude, latitude) AS lng_lat_point
    FROM dealerships
);

CREATE TEMP TABLE customer_dealership_distance AS (
    SELECT
       customer_id,
       dealership_id,
       c.lng_lat_point <@> d.lng_lat_point AS distance
    FROM customer_points c
    CROSS JOIN dealership_points d
);
```

### Answer

It creates 3 temporary tables (customer_points, dealership_points, and customer_dealership_distance) using the data pulled from the subqueries. These tables are dropped after the session ends. Customer_points and Dealership_points are contain longitude and latitude of each respective dealer and customer id. The resulting table, customer_dealerships_distance, gives the Euclidean distance between every customer and dealership.

Customer_points has columns:
- customer_id
- lng_lat_point

Dealership_points has columns:
- dealership_id
- lng_lat_point

customer_dealership_distance
- customer_id
- dealership_id
-
---

## Question 7

Using the `sqlda` database, write SQL to display an **array of salespeople for each dealership**, sorted by dealership.

For example - dealership 1 is below:

```text
"{""Fidell,Granville"",""Onele,Jereme"",""Sheriff,Lelia"",""McSpirron,Massimiliano"",""Rennick,Nadia"",""Mace,Eveleen"",""Oxteby,Dukie"",""Spong,Marcos"",""Wogden,Quent"",""Duny,Sandye"",""Loraine,Englebert"",""Meere,Ira"",""Gibbens,Cristine"",""Prine,Lyda"",""McCoughan,Sheff"",""Schule,Giselbert"",""McAndie,Eleen"",""Dosedale,Dorie"",""Nafziger,Shay""}"
```

### SQL

```sql
SELECT dealership_id, ARRAY_AGG(last_name || ', ' ||first_name)
FROM salespeople
GROUP BY 1
ORDER BY dealership_id ASC;
```

### Screenshot

![alt text](screenshots/M5_7.png)

---

## Question 8

Using the `sqlda` database, write SQL to display:
- an **array of salespeople for each dealership**
- the **state** of the dealership
- the **number of salespeople** for the dealership

Sort by **state**.

Reference image:

![05-ExerciseArray](./instructions/05-ExerciseArray.jpg)

### SQL

```sql
SELECT d.dealership_id, d.state,
	ARRAY_AGG(s.last_name || ', ' || s.first_name) AS list_of_salespeople,
	COUNT(s.salesperson_id) AS Num_of_salespeople
FROM salespeople s
LEFT JOIN dealerships d
	ON d.dealership_id = s.dealership_id
GROUP BY d.dealership_id, d.state
ORDER BY d.state ASC;
```

### Screenshot

![A pessimist is an optimist in full possession of the facts.](screenshots/M5_8.png)

---

## Question 9

Using the `sqlda` database, write the SQL needed to convert the **customers** table to **JSON**.

### SQL

```sql
SELECT row_to_json(c, TRUE)
FROM customers c
```

### Screenshot

![The music is not in the notes, but in the silence between](screenshots/M5_9.png)

---

## Question 10

Using the `sqlda` database, write SQL to display:
- an **array of salespeople for each dealership**
- the **state**
- the **number of salespeople**
- sorted by **state**

Then **convert this result to JSON**.

Reference image:

![05-ExerciseArray-1](./instructions/05-ExerciseArray-1.jpg)

### SQL

```sql
SELECT row_to_json(s, TRUE)
FROM (
	SELECT d.dealership_id, d.state,
		ARRAY_AGG(s.last_name || ', ' || s.first_name) AS list_of_salespeople,
		COUNT(s.salesperson_id) AS Num_of_salespeople
	FROM salespeople s
	LEFT JOIN dealerships d
		ON d.dealership_id = s.dealership_id
	GROUP BY d.dealership_id, d.state
	ORDER BY d.state ASC)
AS s
```

### Screenshot

![Good people do not need laws to tell them to act responsibly, while bad people will find a way around the laws](screenshots/M5_10.png)
