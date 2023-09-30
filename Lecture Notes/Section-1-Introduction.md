# SECTION 1

## What is Postgres?

- Databases used to store information, take data from some source put it inside db where it gets persisted in either memory or on hard-disk, and eventually we retrieve that data.
- Retrieve data using a client, connect to db using client: a piece of software, an api, or server, a utility programme, or Wide variety of different clients.
- When we connect a client to the db we are going to write some SQL, sql is a programming language of sorts, it tells our db some info that we want to put inside of it, retrieve, update or delete.
- SQL is very different / separate to postgres itself, sql is a communciation language, it is how we interface or interact with our db. SQL is supported by many other databases as well such as oracle, mssql, mysql, - generally all the same, not 100% but in general.

### 4 big challenges in writing SQL:

1. How to write efficient queries ito retrieve ifnimration - though writing sql to retrieve is by far the most important and in general most complicated as well.
2. Designing the schema, or structure of the database.
3. Understanding when to use advanced features.
4. Managing the database in a production environment: running backups, scaling db to serve more users and traffic.

### Database Design Process

- What kind of things are we storing?
- What properties does this thing have?
- What type of data does each of thse properties contain?
- Tables
- A table is something that stis inside of our database that is going to store a collection of records that all have some related meaning. We are only ever going to store data inside ouf our db inside of tbales. V important how to properly generate, name, and design the different tbales inside our db.

---

### Tables

A table is something that stis inside of our database that is going to store a collection of records that all have some related meaning. We are only ever going to store data inside ouf our db inside of tbales. V important how to properly generate, name, and design the different tbales inside our db.

### Columns

Table: Cities
Columns:
Name | country | population | area
Each of the columsn stores a very different type or kind of information, strings, numbers.

- Once we set up the table and the 4 columsn inside of it, we can start inserting data into the table itself, so we can start adding lists of cities. Each of these cities will have a name, country, pop, n darea, each of these cindividual cities we add into his table we refer to as rows.
- So we have tables, tables have many colums, and each record inside od a table is referred to as a row.


### Creating Tables:

**Syntax**:
* Some words are keywords, others identifiers. Keywords are special words that tell SQL we want to do a certain thing. 
* CREATE TABLE is a keyword, capitalizaed
* Identifiers: cities, all lowercase
* CREATE TABLE cities – creates the table itself, then inside the parentehses we list out all othe columns this table should contain. 
* Varchar = variable length characters -= essentially string
* Integer = -2 billion to +2billion. 


~~~~sql
CREATE TABLE cities (
name VARCHAR(50),
country VARCHAR(50),
population INTEGER,
area INTEGER
);
~~~~

---

### Insert Data Into Table

~~~~sql
INSERT INTO cities (name, country, population, area)
VALUES ('Tokyo', 'Japan', 38505000, 8223),
  ('Delhi', 'India', 28125000, 2240),
  ('Shanghai', 'China', 22125000, 4015),
  ('Sao Paulo', 'Brazil', 20935000, 3043);
~~~~
---
### Retrieving Data with Select:

~~~~sql
SELECT * FROM cities;
~~~~

Query Result
name	country	population	area
Tokyo	Japan	38505000	8223
Delhi	India	28125000	2240
Shanghai	China	22125000	4015
Sao Paulo	Brazil	20935000	3043

---
### Calculated Columns:

~~~~sql
SELECT name, population / area AS population_density
FROM cities;
~~~~

---
### String Operators and Functions

**Syntax**:
Join two strings using || pipe operator or Concat() function.

~~~~sql
SELECT name || country FROM cities;
SELECT name || ', ' || country FROM cities;
SELECT name || ', ' || country AS location FROM cities;
SELECT CONCAT(name, ', ', country) AS location FROM cities;
~~~~
Query Result:
Tokyo, Japan
Delhi, India
Shanghai, China
Sao Paulo, Brazil
