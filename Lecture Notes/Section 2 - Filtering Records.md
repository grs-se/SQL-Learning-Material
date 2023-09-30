# SECTION 2: Filtering Records

### Filter Rows with “WHERE”

~~~~sql
SELECT
  name,
  area
FROM
  cities
WHERE
  area > 4000;
~~~~

**Syntax**:
- Best not to think of these queries as being read from Left to Right , i.e. SELECT > FROM > WHERE
- Best order is: FROM cities, WHERE area, > 4000 SELECT name
- Internally first of all Postgres takes a look at a datra source
- After it gets source of data it then second applies that filtering criteria. 
- Being able to picture this entire process makes making more complex queries down the road a lot easier to understand 
- Many ways to use ‘WHERE’

---
### More on the WHERE keyword:

- The where keyword allows us to filter down the query.
- Many comparison operators. 

~~~~sql
SELECT
  name,
  area
FROM
  cities
WHERE
  area = 8223;


SELECT
  name,
  area
FROM
  cities
WHERE
  area != 8223;
~~~~

---
### Compound Where Clauses

~~~~sql
SELECT
  name,
  area
FROM
  cities
WHERE
  area BETWEEN 2000 AND 4000;

SELECT
  name,
  area
FROM
  cities
WHERE
  name IN ('Delhi', 'Shanghai');

SELECT
  name,
  area
FROM
  cities
WHERE
  area NOT IN (8223, 3043);

SELECT
  name,
  area
FROM
  cities
WHERE
  area NOT IN (8223, 3043) AND name = 'Delhi';
~~~~

---
### WHERE with Lists

**Syntax**:
- Order of operations = do mathematical operations first

~~~~sql
SELECT name, manufacturer FROM phones
WHERE manufacturer
IN ('Apple', 'Samsung');
~~~~

---
### Updating Rows

~~~~sql
UPDATE cities
SET population = 39505000
WHERE name = 'Tokyo';
~~~~

---
### Deleting Rows

**BE CAREFUL and ACCURATE**

~~~~sql
DELETE FROM cities
WHERE name = 'Tokyo';
~~~~