# SECTION 1

## What is Postgres?
* Databases used to store information, take data from some source put it inside db where it gets persisted in either memory or on hard-disk, and eventually we retrieve that data.
* Retrieve data using a client, connect to db using client: a piece of software, an api, or server, a utility programme, or Wide variety of different clients.
* When we connect a client to the db we are going to write some SQL, sql is a programming language of sorts, it tells our db some info that we want to put inside of it, retrieve, update or delete. 
* SQL is very different / separate to postgres itself, sql is a communciation language, it is how we interface or interact with our db. SQL is supported by many other databases as well such as oracle, mssql, mysql, - generally all the same, not 100% but in general.

### 4 big challenges in writing SQL:
1. How to write efficient queries ito retrieve ifnimration - though writing sql to retrieve is by far the most important and in general most complicated as well. 
2. Designing the schema, or structure of the database. 
3. Understanding when to use advanced features. 
4. Managing the database in a production environment: running backups, scaling db to serve more users and traffic. 

### Database Design Process
* What kind of things are we storing? 
* What properties does this thing have?
* What type of data does each of thse properties contain?
* Tables
* A table is something that stis inside of our database that is going to store a collection of records that all have some related meaning. We are only ever going to store data inside ouf our db inside of tbales. V important how to properly generate, name, and design the different tbales inside our db. 
