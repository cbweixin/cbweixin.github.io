title: multiple-solutions-for-a-sql-problems
date: 2023-05-06 12:10:17
categories: programming
tags: sql
---

# multiple solutions for a sql problem

## problem description
[problem](https://leetcode.cn/problems/running-total-for-different-genders/)


## sol 1, using variable and subquery to calculate
```sql
# Write your MySQL query statement below
SELECT
  gender,
  day,
  @total := @total * (@pre = (@pre := gender)) + score_points AS total
FROM
  scores,
  (
    SELECT
      @total := 0,
      @pre := 'F'
  ) init
ORDER BY
  gender,
  day;
```

in this query, we have 2 variables `@total` and `pre`. we compare pre and current gender, if it is same, then we accumulate, otherwise, score_points would be my new total. 

## sol 2, windows function
```sql
SELECT
  gender,
  day,
  SUM(score_points) OVER (PARTITION BY gender ORDER BY day) AS total
FROM
  scores
```
this is the best solution. so concise. 

we also can rewrite like this
```sql
select gender, day, sum(score_points) over w total
from scores
window w as (partition by gender order by day)
```

## sol 3, using join
intitially, I write the query as such
```sql
SELECT s1.gender, s1.day, SUM(s1.score_points) AS total
FROM Scores AS s1 JOIN Scores AS s2
ON s1.gender = s2.gender AND s1.day >= s2.day
GROUP BY s1.gender, s1.day
ORDER BY s1.gender, s1.day;
```

but it gives me result 
```sql
| gender | day        | total |
| ------ | ---------- | ----- |
| F      | 2019-12-30 | 17    |
| F      | 2019-12-31 | 46    |
| F      | 2020-01-01 | 51    |
| F      | 2020-01-07 | 92    |
| M      | 2019-12-18 | 2     |
| M      | 2019-12-25 | 22    |
| M      | 2019-12-30 | 39    |
| M      | 2019-12-31 | 12    |
| M      | 2020-01-07 | 35    |
```

this is wrong. this error likes at on syntax: `ON s1.gender = s2.gender AND s1.day >= s2.day`

how about change it as such:
```sql
SELECT s1.gender, s1.day, SUM(s1.score_points) AS total
FROM Scores AS s1 JOIN Scores AS s2
ON s1.gender = s2.gender AND s1.day <= s2.day
GROUP BY s1.gender, s1.day
ORDER BY s1.gender, s1.day;
```

still wrong
```sql
| gender | day        | total |
| ------ | ---------- | ----- |
| F      | 2019-12-30 | 68    |
| F      | 2019-12-31 | 69    |
| F      | 2020-01-01 | 34    |
| F      | 2020-01-07 | 23    |
| M      | 2019-12-18 | 10    |
| M      | 2019-12-25 | 44    |
| M      | 2019-12-30 | 39    |
| M      | 2019-12-31 | 6     |
| M      | 2020-01-07 | 7     |
```

in order to make it right, we need to understand how `join` works. 

let's check a left join example. 
```sql

SELECT CustomerName, OrderDate
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;
```
if I re-write it as python code, it looks like this:
```python
for customer in customers:
    order = orders.filter(orders.CustomerID == customer.CustomerID).first()
    if order is not None:
        print(customer.CustomerName, order.OrderDate)
    else:
        print(customer.CustomerName, None)
```
so table on the left side of join would be outer loop, table on the right side would be inner join.

inner join is similiar
```python
# Create two lists
customers = [
    {'CustomerID': 1, 'CustomerName': 'John Doe'},
    {'CustomerID': 2, 'CustomerName': 'Jane Doe'},
    {'CustomerID': 3, 'CustomerName': 'John Smith'}
]

orders = [
    {'CustomerID': 1, 'OrderDate': '2023-03-08'},
    {'CustomerID': 2, 'OrderDate': '2023-03-09'},
    {'CustomerID': 4, 'OrderDate': '2023-03-10'}
]

# Iterate through the lists
for customer in customers:
    order = next((order for order in orders if order['CustomerID'] == customer['CustomerID']), None)
    if order is not None:
        print(customer['CustomerName'], order['OrderDate'])
    else:
        print(customer['CustomerName'], None)
```

