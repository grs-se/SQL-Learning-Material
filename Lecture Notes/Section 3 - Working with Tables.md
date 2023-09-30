* If we use type integer we would have to write code to manually increment integer each time a row is inserted. 
* SERIAL datatype tells postgres to generate values automatically. We don't provide an id at all, only other fields, postgres fills in id for us. 
* After SERIAL we add on PRIMARY KEY, which adds special perfomrance benefits to looking up records when we are using the id

~~~~sql
CREATE TABLE users (
id SERIAL PRIMARY KEY, 
username VARCHAR(50)
);
~~~~