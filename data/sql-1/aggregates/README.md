---
description: calculates on a set of values and returns a single value.
---

# Aggregates

Because an aggregate function operates on a set of values, it is often used with the **GROUP BY** clause of the SELECT statement. 

The GROUP BY clause divides the result set into groups of values and the aggregate function return a single value for each group.

```sql
SELECT c1, aggregate_function(c2) 
FROM table 
GROUP BY c1;
```

AVG\(\), MAX\(\)/MIN\(\), SUM\(\), COUNT\(\)

Except for the `COUNT()` function, SQL aggregate functions ignore null.

You can use aggregate functions as expressions only in the following:

* The select list of a [`SELECT`](http://www.sqltutorial.org/sql-select/) statement, either a subquery or an outer query.
* A [`HAVING`](http://www.sqltutorial.org/sql-having/) clause

## AVG

## MIN /MAX

## COUNT

## SUM











