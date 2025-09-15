## Relational

- **Schema:** The blueprint of the database. It defines the tables, the columns in those tables,
  data types, and the relationships between tables.
- **Table:** A collection of related data held in a structured format within a database. It consists
  of columns and rows.
- **Row (or Tuple):** A single record in a table.
- **Column (or Attribute):** A vertical entity in a table that contains all information associated
  with a specific field.
- **Keys:**
  - **Primary Key:** A column (or set of columns) that **uniquely identifies** each row in a table.
    It cannot contain NULL values.
  - **Foreign Key:** A key used to **link two tables together**. It's a field in one table that
    refers to the Primary Key in another table.
- **Index:** A special data structure used to improve the speed of data retrieval operations on a
  database table. Think of it like the index in the back of a book.

### Normalization

Normalization is the process of _organizing columns and tables_ in a relational database to minimize
data redundancy and improve data integrity.

- **First Normal Form (1NF):**
  - Has rows & columns
  - Each cell is not null
- **Second Normal Form (2NF):**
  - 1NF
  - Non-key attributes dependent on primary key, some may dependent on each other
- **Third Normal Form (3NF):**
  - 2NF
  - All attributes dependent JUST on primary key

## SQL

- create - create db, table, view
- alter - modify structure of a db object
- drop - delete db objects
- insert - add new rows
- update - modify existing data
- delete - remove rows
- select
- grant - give user access privileges
- revoke - take away user access privileges

- count() - no rows
- sum()
- avg()
- min()
- max()

Find the number of customers in each country, but only show countries with more than 10 customers.

```sql
select country, count(customerID) as numberOfCustomers
from customers
group by country
having count(customerID) > 10
order by numberOfCustomers desc
```

## Joins

- Inner join: only matching part

```sql
select
from tableA
inner join tableB
on tableA.id = tableB.id
```

- Left join: left + inner join

```sql
select
from tableA
left join tableB
on tableA.id = tableB.id
-- where tableB.id = null -> no matching, just left
```

- Right join: right + inner join

```sql
select
from tableA
right join tableB
on tableA.id = tableB.id
-- where tableA.id = null -> no matching, just right
```

- Full outer join: all

```sql
select
from tableA
full outer join tableB
on tableA.id = tableB.id
-- where tableA.id = null or tableB.id = null -> no matching, just left + right
```
