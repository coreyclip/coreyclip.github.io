---
layout: post
title: SQL Tricks: Mimicing Crosstabs 
published:True 
---

## Transforming Rows into Columns 

To get the sample database I'll be demonstrating with follow the instructions in this [link](https://dev.mysql.com/doc/sakila/en/sakila-installation.html)

SQL is pretty straight forward to get into yet despite the ubiquity of SQL Databases, there is comparatively few online resources on how to think about and troubleshoot complex queries. 
While it's maybe not the most alluring aspect of data science, it's pretty darn near foundational. In my current job, basically my entire job in one shape or form goes back to queries to our MySQL Database. While generally you want to design your database in a manner that makes reading and writing to the database easier. You'll often find yourself in situations where you'll have to use more sophisticated SQL queries. One common case basically involves transforming data that is held in rows into columns. This can be achieved in a crosstab query. 

Say for example you have table with payment records organized with a date column like this. 
```sql
mysql> SELECT * FROM payment LIMIT 10;
+------------|-------------|----------|-----------|--------|---------------------|---------------------+
| payment_id | customer_id | staff_id | rental_id | amount | payment_date        | last_update         |
+------------|-------------|----------|-----------|--------|---------------------|---------------------+
|          1 |           1 |        1 |        76 |   2.99 | 2005-05-25 11:30:37 | 2006-02-15 22:12:30 |
|          2 |           1 |        1 |       573 |   0.99 | 2005-05-28 10:35:23 | 2006-02-15 22:12:30 |
|          3 |           1 |        1 |      1185 |   5.99 | 2005-06-15 00:54:12 | 2006-02-15 22:12:30 |
|          4 |           1 |        2 |      1422 |   0.99 | 2005-06-15 18:02:53 | 2006-02-15 22:12:30 |
|          5 |           1 |        2 |      1476 |   9.99 | 2005-06-15 21:08:46 | 2006-02-15 22:12:30 |
|          6 |           1 |        1 |      1725 |   4.99 | 2005-06-16 15:18:57 | 2006-02-15 22:12:30 |
|          7 |           1 |        1 |      2308 |   4.99 | 2005-06-18 08:41:48 | 2006-02-15 22:12:30 |
|          8 |           1 |        2 |      2363 |   0.99 | 2005-06-18 13:33:59 | 2006-02-15 22:12:30 |
|          9 |           1 |        1 |      3284 |   3.99 | 2005-06-21 06:24:45 | 2006-02-15 22:12:30 |
|         10 |           1 |        2 |      4526 |   5.99 | 2005-07-08 03:17:05 | 2006-02-15 22:12:30 |
+------------|-------------|----------|-----------|--------|---------------------|---------------------+
10 rows in set (0.00 sec)
```


You want to get a total of all the payments made after a certain date broken down by another column, in our case address. 
You could use a where statement like this:
```sql 
mysql> SELECT 
    -> address.address,
    -> SUM(payment.amount) AS 'Payments made after 2006'
    -> FROM store 
    -> LEFT JOIN address on address.address_id = store.address_id
    -> LEFT JOIN city on address.city_id = city.city_id
    -> LEFT JOIN customer on customer.store_id = store.store_id
    -> LEFT JOIN payment on payment.customer_id = customer.customer_id
    -> where payment.payment_date >= '2006-01-01'
    -> GROUP BY address.address;
+--------------------|--------------------------+
| address            | Payments made after 2006 |
+--------------------|--------------------------+
| 28 MySQL Boulevard |                   231.16 |
| 47 MySakila Drive  |                   283.02 |
+--------------------|--------------------------+
2 rows in set (0.01 sec)
```

But what if you want to create multiple date filtered columns? You can't adjust the where statement, since that filters globally. You could set up multiple subqueries but that'll quickly get overly complex and difficult to debug. We can do the following: 

```sql
mysql> SELECT 
    -> CONCAT(address.address,' ', city.city) AS 'Full Address',
    ->     SUM(if(DATE(payment.payment_date) < '2006-01-01', coalesce(payment.amount, 0), 0)) AS 'Revenue Prior 2006',
    ->     SUM(if(DATE(payment.payment_date) >= '2006-01-01', coalesce(payment.amount, 0), 0)) AS 'Revenue Post 2006'
    -> FROM store 
    -> LEFT JOIN address on address.address_id = store.address_id
    -> LEFT JOIN city on address.city_id = city.city_id
    -> LEFT JOIN customer on customer.store_id = store.store_id
    -> LEFT JOIN payment on payment.customer_id = customer.customer_id
    -> group by city.city, address.address;
+------------------------------|--------------------|-------------------+
| Full Address                 | Revenue Prior 2006 | Revenue Post 2006 |
+------------------------------|--------------------|-------------------+
| 47 MySakila Drive Lethbridge |           36718.50 |            283.02 |
| 28 MySQL Boulevard Woodridge |           30183.83 |            231.16 |
+------------------------------|--------------------|-------------------+
2 rows in set (0.05 sec)
```

The SUM(IF(conditional, True , False)) essentially tells SQL to evaluate if a row meets a certain condition and then performs the SUM function if and only if that condition is met. 

The Coalesce function makes sure that SQL doesn't inadvertently sum up any NULL values that it may run into. 

This formula is pretty flexible in terms of the conditional that you set up and which column you decide to aggregate and you can repeat this pattern multiple times within the select statement. Note this will also work with the AVG function and any other aggregation functions. 
