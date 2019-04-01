# JOIN

## INNER JOIN - Intersection

![](../../.gitbook/assets/image%20%283%29.png)

The INNER JOIN clause appears after the FROM clause. The condition to match between table A and table B is specified after the ON keyword. This condition is called _join condition_ i.e., _`B.n = A.n`_

```sql
SELECT
  A.n
FROM A
INNER JOIN B ON B.n = A.n;
```

The INNER JOIN clause can join three or more tables as long as they have relationships, typically foreign key relationships.

```sql
SELECT
  A.n
FROM A
INNER JOIN B ON B.n = A.n
INNER JOIN C ON C.n = A.n;
```

## LEFT JOIN

![](../../.gitbook/assets/image%20%2821%29.png)

The **inner join** clause **eliminates the rows that do not match** with a row of the other table.

The **left join**, however, **returns all rows from the left table whether or not there is a matching row in the right table**.

### LEFT JOIN two tables

{% tabs %}
{% tab title="Query" %}
![](../../.gitbook/assets/image%20%289%29.png)

```sql
SELECT
    c.country_name,
    c.country_id,
    l.country_id,
    l.street_address,
    l.city
FROM
    countries c
LEFT JOIN locations l ON l.country_id = c.country_id
WHERE
    c.country_id IN ('US', 'UK', 'CN')
```
{% endtab %}

{% tab title="Results" %}
![](../../.gitbook/assets/image%20%285%29.png)

Because we use the LEFT JOIN clause, all rows that satisfy the condition in the WHERE clause of the countries table are included in the result set.

For each row in the countries table, the LEFT JOIN clause finds the matching rows in the locations table.

* If at least one matching row found, the database engine combines the data from columns of the matching rows in both tables.
* In case there is no matching row found e.g., with the country\_id CN, the row in the countries table is included in the result set and the row in the locations table is filled with NULL values.
* Because non-matching rows in the right table are filled with the NULL values, you can apply the LEFT JOIN clause to miss-match rows between tables.
{% endtab %}
{% endtabs %}

### LEFT JOIN three tables

{% tabs %}
{% tab title="Query" %}
![](../../.gitbook/assets/image%20%2811%29.png)

One region may have zero or many countries while each country is located in the one region. The relationship between countries and regions tables is one-to-many. The region\_id column in the countries table is the link between the countries and regions table.

```sql
SELECT
    r.region_name,
    c.country_name,
    l.street_address,
    l.city
FROM
    regions r
LEFT JOIN countries c ON c.region_id = r.region_id
LEFT JOIN locations l ON l.country_id = c.country_id
WHERE
    c.country_id IN ('US', 'UK', 'CN');
```
{% endtab %}

{% tab title="Results" %}
![](../../.gitbook/assets/image%20%284%29.png)
{% endtab %}
{% endtabs %}

## SELF JOIN

Sometimes, it is useful to join a table to itself. This type of join is known as the self-join.

We join a table to itself to evaluate the rows with other rows in the same table. To perform the self-join, we use either an [inner join](http://www.sqltutorial.org/sql-inner-join/) or [left join](http://www.sqltutorial.org/sql-left-join/) clause.

Because the same table appears twice in a single query, we have to use the table aliases. The following statement illustrates how to join a table to itself.

{% tabs %}
{% tab title="Setup" %}
![](../../.gitbook/assets/image%20%2819%29.png)
{% endtab %}

{% tab title="Get employee with his manager" %}
```sql
SELECT 
    e.first_name || ' ' || e.last_name AS employee,
    concat(m.first_name, m.last_name) AS manager
FROM
    employees e
        INNER JOIN
    employees m ON m.employee_id = e.manager_id
ORDER BY manager;
```
{% endtab %}

{% tab title="Results" %}
![](../../.gitbook/assets/image%20%2818%29.png)

The president does not have any manager. 

In the employees table, the manager\_id of the row that contains the president is NULL.

Because the inner join clause only includes the rows that have matching rows in the other table, therefore the president did not show up in the result set of the query above.
{% endtab %}

{% tab title="Get employee including president" %}
To include the president in the result set, we use the LEFT JOIN clause instead of the INNER JOIN clause as the following query.

```sql
SELECT 
    e.first_name || ' ' || e.last_name AS employee,
    m.first_name || ' ' || m.last_name AS manager
FROM
    employees e
        LEFT JOIN
    employees m ON m.employee_id = e.manager_id
ORDER BY manager;
```
{% endtab %}

{% tab title="Results" %}


![](../../.gitbook/assets/image%20%2817%29.png)
{% endtab %}
{% endtabs %}

## FULL OUTER JOIN

![](../../.gitbook/assets/image%20%2814%29.png)

In theory, a full outer join is the combination of a [left join](http://www.sqltutorial.org/sql-left-join/) and a right join. 

The full outer join includes all rows from the joined tables whether or not the other table has the matching row.

If the rows in the joined tables **do not match**, the result set of the full outer join **contains NULL values** for every column of the table that lacks a matching row. **For the matching rows**, a **single row** that has the columns populated from the joined table is included in the result set.

{% tabs %}
{% tab title="Setup" %}
```sql
CREATE TABLE fruits (
    fruit_id INTEGER PRIMARY KEY,
    fruit_name VARCHAR (255) NOT NULL,
    basket_id INTEGER
);
CREATE TABLE baskets (
    basket_id INTEGER PRIMARY KEY,
    basket_name VARCHAR (255) NOT NULL
);
INSERT INTO baskets (basket_id, basket_name)
VALUES
    (1, 'A'),
    (2, 'B'),
    (3, 'C');
INSERT INTO fruits (
    fruit_id,
    fruit_name,
    basket_id
)
VALUES
    (1, 'Apple', 1),
    (2, 'Orange', 1),
    (3, 'Banana', 2),
    (4, 'Strawberry', NULL);
```
{% endtab %}

{% tab title="Query 1" %}
The following query returns 

* each fruit that is in a basket and 
* each basket that has a fruit, but 
* also returns each fruit that is not in any basket and 
* each basket that does not have any fruit.

```sql
SELECT
    basket_name,
    fruit_name
FROM
    fruits
FULL OUTER JOIN baskets ON baskets.basket_id = fruits.basket_id;
```

```text
basket_name  | fruit_name
-------------+------------
 A           | Apple
 A           | Orange
 B           | Banana
 (null)      | Strawberry
 C           | (null)
```

As you see, the basket `C` does not have any fruit and the `Strawberry` is not in any basket.
{% endtab %}

{% tab title="Query 2" %}
You can add a [WHERE clause](http://www.sqltutorial.org/sql-where/) to the statement that uses the `FULL OUTER JOIN` clause to get more specific information.

For example, to find the empty basket, which does not store any fruit, you use the following statement:

```sql
SELECT
    basket_name,
    fruit_name
FROM
    fruits
FULL OUTER JOIN baskets ON baskets.basket_id = fruits.basket_id
WHERE
    fruit_name IS NULL;
```

```sql
 basket_name | fruit_name
-------------+------------
 C           | (null)
(1 row)
```
{% endtab %}

{% tab title="Query 3" %}
Similarly, if you want to see which fruit is not in any basket, you use the following statement:

```sql
SELECT
    basket_name,
    fruit_name
FROM
    fruits
FULL OUTER JOIN baskets ON baskets.basket_id = fruits.basket_id
WHERE
    basket_name IS NULL;
```

```text
 basket_name | fruit_name
-------------+------------
(null)       | Strawberry
(1 row)
```
{% endtab %}
{% endtabs %}

## CROSS JOIN

A cross join is a join operation that produces the Cartesian product of two or more tables.

In Math, a Cartesian product is a mathematical operation that returns a product set of multiple sets.

For example, with two sets A {x,y,z} and B {1,2,3}, the Cartesian product of A x B is the set of all ordered pairs \(x,1\), \(x,2\), \(x,3\), \(y,1\) \(y,2\), \(y,3\), \(z,1\), \(z,2\), \(z,3\).

The following picture illustrates the Cartesian product of A and B:

![](../../.gitbook/assets/image%20%2816%29.png)

Similarly, in SQL, a Cartesian product of two tables A and B is a result set in which each row in the first table \(A\) is paired with each row in the second table \(B\). Suppose the A table has n rows and the B table has m rows, the result of the cross join of the A and B tables have n x m rows.

```sql
SELECT column_list
FROM A
CROSS JOIN B;
```

The following picture illustrates the result of the cross join between the table A and table B. In this illustration, the table A has three rows 1, 2 and 3 and the table B also has three rows x, y and z. As the result, the Cartesian product has nine rows:

![](../../.gitbook/assets/image%20%288%29.png)

Note that unlike the `CROSS JOIN` clause does not have a join condition.

```sql
SELECT 
    column_list
FROM
    A,
    B;
```



