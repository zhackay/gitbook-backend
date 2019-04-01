# WHERE expression

## WHERE

```sql
SELECT | UPDATE | DELETE 
    WHERE Country='Mexico';
    WHERE CustomerID=1;
```



| Operator | Description |
| :--- | :--- |
| = | Equal |
| &lt;&gt; | Not equal. **Note:** In some versions of SQL this operator may be written as != |
| &gt; | Greater than |
| &lt; | Less than |
| &gt;= | Greater than or equal |
| &lt;= | Less than or equal |
| BETWEEN | Between a certain range |
| LIKE | Search for a pattern |
| IN | To specify multiple possible values for a column |

#### Literal Values Used in expression

|  |  |  |
| :--- | :--- | :--- |
| Number | 100, 200.5 | use a number that can be an integer or a decimal without any formatting |
| Character | “100”, “John Doe” | use characters surrounded by either single or double quotes |
| Date | `'yyyy-mm-dd'` format | use the format that the database stores. It depends on the database system |
| Time | `'HH:MM:SS'` | use the format that the database system uses to store the time |

### IN

```sql
WHERE expression IN (value1,value2,...)
WHERE expression = value1 OR expression = value2 OR ...
WHERE expression NOT IN (value1, value2,...)
```

```sql
SELECT
    employee_id,
    first_name,
    last_name,
    job_id
FROM
    employees
WHERE
    job_id IN (8, 9, 10)
ORDER BY
    job_id;
```

### BETWEEN \(Inclusive\)

```sql
WHERE expression BETWEEN low AND high;
-- equivalent to 
WHERE low <= expression AND expression <= high
```

```sql
expression NOT BETWEEN low AND high
-- equivalent to
WHERE low > expression AND expression > high
```

```sql
SELECT employee_id, first_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '1999-01-01' AND '2000-12-31'
ORDER BY hire_date;
```

### LIKE

```sql
WHERE expression LIKE pattern
WHERE expression NOT LIKE pattern
```

* `%` zero or more character
* `_` a single character.

```sql
SELECT employee_id, first_name, last_name
FROM employees
WHERE   first_name LIKE 'S%'
    AND first_name NOT LIKE 'Sh%'
ORDER BY first_name;
```

### AND, OR, NOT

```sql
WHERE condition1 AND condition2 AND condition3 ...;
WHERE condition1 OR condition2 OR condition3 ...;
WHERE NOT condition;
```

