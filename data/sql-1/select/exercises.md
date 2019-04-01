# exercises

## Combine Two Tables

{% embed url="https://leetcode.com/problems/combine-two-tables/" %}

{% tabs %}
{% tab title="Problem" %}
### [175. Combine Two Tables](https://leetcode.com/problems/combine-two-tables/)

Difficulty: **Easy**

Table: `Person`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId is the primary key column for this table.
```

Table: `Address`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.
```

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

```text
FirstName, LastName, City, State
```
{% endtab %}

{% tab title="Hints" %}

{% endtab %}

{% tab title="Solution" %}
```sql
select FirstName, LastName, City, State from Person
Left Join Address on Person.PersonId = Address.PersonId;
```
{% endtab %}
{% endtabs %}

