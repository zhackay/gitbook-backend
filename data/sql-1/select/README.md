---
description: 'Quering, Sorting, Filtering'
---

# SELECT

## SELECT

## SELECT DISTINCT

SELECT DISTINCT \(...cols\) FROM \(Table\)

```sql
SELECT DISTINCT Country FROM Customers;
SELECT COUNT(DISTINCT Country) FROM Customers;
SELECT Count(*) AS DistinctCountries FROM 
    (SELECT DISTINCT Country FROM Customers);
```

## AS \[column\_alias\]

allows you to assign a table or a column a temporary name during the execution of a [query](http://www.sqltutorial.org/sql-select/). There are two types of aliases: **table alias** and **column alias**.

```sql
SELECT
    inv_no AS invoice_no,
    amount,
    due_date AS 'Due date',
    cust_no 'Customer No'
FROM
    invoices;
```



* The `invoice_no` is the alias of the `inv_no` column
* The `'Due date'` is the column alias of the `due_date` column. Because the alias contains space, we have to use either sing quote \(‘\) or double quotes \(“\) to surround the alias.
* The `'Customer no'` is the alias of the `cust_no` column. You notice that we did not use the AS keyword. The AS keyword is optional so you can omit it.

We often use the column aliases for the expressions in the select list.

```sql
SELECT
    department_id,
    count(employee_id) headcount
FROM
    employees
GROUP BY
    department_id
HAVING
    headcount >= 5;
```

## AS \[table\_alias\]

We often assign a table a different name temporarily in a `SELECT` statement. We call the new name of the table is the table alias. A table alias is also known as a **correlation name**.

As keyword can be omitted

```sql
SELECT employee_id, first_name, last_name,
    e.department_id, department_name
FROM employees e
INNER JOIN departments d ON d.department_id = e.department_id
ORDER BY first_name;
```

## ORDER BY

ORDER BY _column1, column2, ..._ ASC\|DESC;

```sql
SELECT * FROM Customers ORDER BY Country, CustomerName;
SELECT * FROM Customers ORDER BY Country ASC, CustomerName DESC;
```

## LIMIT OFFSET

used to specify the number of records to return.

{% hint style="warning" %}
 MySQL supports the **LIMIT** clause to select a limited number of records, 

Oracle uses **ROWNUM**.

MS SQL uses **SELECT TOP**
{% endhint %}

#### MySQL

```sql
SELECT column_name(s) FROM table_name
WHERE condition
LIMIT number;
```

```sql
SELECT * FROM Customers
LIMIT 3;
```

#### OFFSET 

The OFF SET value allows us to specify which row to start from retrieving data

```sql
# from 2nd row, get 3 results
SELECT * FROM Customers
LIMIT 3 OFFSET 2; 
```

Or, without OFFSET keyword

```sql
# from 2nd row, get 1 results - get 2nd top result
SELECT * FROM Customers
LIMIT 1, 2;
```

## CASE WHEN

## NULL

If a field in a table is optional, it is possible to insert a new record or update a record without adding a value to this field. Then, the field will be saved with a **NULL** value.

### How to Test for NULL Values?

It is not possible to test for NULL values with comparison operators, such as =, &lt;, or &lt;&gt;.

We will have to use the **IS NULL** and **IS NOT NULL** operators instead.

#### IS NULL Syntax

```sql
SELECT column_names FROM table_name
WHERE column_name IS NULL;
```

#### IS NOT NULL Syntax

```sql
SELECT column_names FROM table_name
WHERE column_name IS NOT NULL;
```











