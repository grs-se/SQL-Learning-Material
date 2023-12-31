# SECTION 3: Working with Tables

### Approaching Database Design:

**What tables should we make?**
-	Common features (like authentication ,comments, etc) are freuqnelty built with conventional table names and cilumns
-	What type of resources exist in your app? Create a separate table for each of these features
-	Features that seem to indicate a relationship or ownership between two types of resoices need to be reflected in our table design. 

Google search: sql schema upvote system – tonnes of resources online about how to structure this system.
What does your UI do? List of photos? Followers, followings?

---
### One-to-many and many-to-one relationships:

4 different ways of relating together different kinds of records

**One to many:**
One user has many photos that are tied to them.
A user has many photos
  

**Many to one:**
A photo has one user


One to many and many to one are really just the exact opposite but it depends on which perspective you are looking at this relationship from.
  
Many comments can refer to one photo
  
School has many students
Boat has many crew members
Company has many employees
  
A student has one school
An employee has one company (though some people have many jobs – but we generalize this here)
  
Any time you want to refer to a relationship using the terms has many or has one, that’s always a sign that you have a one-to-many or many-to-one relationship. 

---

### One-to-one and Many-to-many Relationships:

One boat has one captain  
One company has one CEO  
One Capitol has one Country  
One Student has one desk  
One person has one driver’s license  

---

### Many-to-many Relationships:

Many Students have many classes  
Many Tasks have many engineers  
Many players play in a single match, and matches that have many different players playi gin them  
Many movies in the world, each movie has many actors, actors may have many movies they’ve acted in.  

---

### Primary Keys and Foreign Keys:  

Primary keys and foreign keys are additional columns that we are going to add to a lot of different tables inside all of the different tables inside all the different databases we create. Every single table we ever make is going to have a primary key.  
The goal of the primary key is to identify an individual row inside of the table. Every value inside of this column is goin got be some kind of unique value in that column. For example the first row might have an id of 1, so this is this rows primary key, and id of 1, there will never be any of row inside of this table with an id of 1, so if I ever reach into the photos table and ask for the photo with an id of 1 I will always 100% of the time get that row right there, even if we sort the values inside this table, if I ask for the row with the id of 1 we wil always get exactly that row. So The goal of the primary key is to identify this row inside of a table. The id is always suniqe and it’s never ever going to change.   

Now to actually set up a relationship between two different records, this is the important part, we’re going to use something called a foreign key. The goal of a foreign key is to somehow relate this record to some record in possibly another table or even the same table.  
So in this case we have a table of photos and users, and we said that a user has many photos, so we need to somehow set upa  relationship between these different photos and a user. To set up this relationship we are going to give our photos table a foreign key column, we’re going to call that column user_Id, and inside this clumn we’re going to store the id of the user that this particular photo belongs to or are somehow owned by or somehow related to the user with id 4.    
So in the users table we have a primary key column called id, so we would look up the user with id 4 inside that table. So we’ve now formed  a relationship, we have somehow related these two photos with the user with id 4. That is how we implement a one to many and many to one relationship. Remember those two are very dsimilar in nature it just depends on the perpsictve you are viewing this from.   
Sp now we could write a query that might take a look at our photos table we could take a look at all the user_id values inside there, we could say give me all the photos we user id of 4, and that would give me all the difffent photos that are associated with this user.   
  
So that is the basics of setting up a one to many or many to one relationship. We’re going to make sure that all our tables always have a primary key column, in general we’re gojgn to store numbers in there, the value is alwya sgoing to be a 100% unique and it’s never going to change, then to set up a relationship with a row inside anoterh table, we’re going to add in a foreign key and inside that column we’re going to say that here is the id of the user associated with this photo. 
This can be confusing the first time you see this set up.  

--- 

### Understanding Foreign Keys
So we are going to use this idea of a foreign key to set up all four kinds of those different relationships. Whenever we talk about a one-to-many a many-to-one a many-to-many or a one-to-one we are always going to somehow involve a foreign key column. That is how we set up relationships. This idea of foreign keys is a bit challenging.   
How are comments related to users and comments are related to photos?
Comments have one user and comments have one photo.   
If we consider that relatiosnhop from the opposite direction then we say that a user has many comments and  a cphoot has many comments.   
So to decide which table gets the foreign key columns here’s what we do. 
We are going to say that the many  side of our relationship is always going to get the foreign key column, so in our case for photos and comments it is a one to mny relationship, comments are the many side  so the comments table is going to get a foreign key column pointing at the photo that each comment belongs to.   

  
Thi sis the only way to set up relationships – so we really hav to understand hwo shtis works. It looks really confusing the first time, but the good news is that it’s a very mechanical process, setting up relationhsips is always sgoing to be done the exact same way. So you just have to learn it correctly one time and then it makes sense forever. 

|     Primary Keys                                                                                                            |     Foreign Keys                                                    |
|-----------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
|     Each row in every table has one   primary key                                                                           |     Rows only have this is they belong   to another record          |
|     No other row in the same table can   have the same value                                                                |     Many rows in the same table can   have the same foreign keys    |
|     99% of the time called ‘id’ – bad   idea to set ‘name’ as primary key (i.e. Shanghai in China and also in   America)    |     Name varies, usually called   something like ‘xyd_id’           |
|     Either an integer of a UUID                                                                                             |     Exactly equal to the primary key   of the referenced row        |
|     Will never change                                                                                                       |     Will change if the relationship   changes.                      |

---

### Auto-Generated ID's


- If we use type integer we would have to write code to manually increment integer each time a row is inserted. 
- SERIAL datatype tells postgres to generate values automatically. We don't provide an id at all, only other fields, postgres fills in id for us. Increment by 1.
- After SERIAL we add on PRIMARY KEY, which adds special perfomrance benefits to looking up records when we are using the id

~~~~sql
CREATE TABLE users (
id SERIAL PRIMARY KEY, 
username VARCHAR(50)
);

-- note only username, no id
INSERT INTO
  users (username)
VALUES
  ('monohan93'),
  ('pfeffer'),
  ('si93onis'),
  ('99stroman');

---
SELECT * FROM users;
~~~~

**Query Result**

| **id** | **username** |
|--------|--------------|
| 1      | monohan93    |
| 2      | pfeffer      |
| 3      | si93onis     |
| 4      | 99stroman    |

Do not intend to change these ids at any point in time.

---

### Creating Foreign Key Columns:

- We don't use Serial for Foreign Keys because otherwise Postgres will try to assing a value to it automatically, we want to be able to specify exactly what user provided a given photo. So we use Integer and then add one more piece of syntax which will turn it into a Forieng Key Column = REFERENCES table(column)
- Marking a column as a FK enforces some level of data consistency. 

~~~~sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id)
);
---
INSERT INTO
  PHOTOS (url, user_id)
VALUES('http://one.jpg', 4);
---
SELECT * FROM photos;
~~~~

**Query Result**

| **id** | **url**        | **user_id** |
|--------|----------------|-------------|
| 1      | http://one.jpg | 4           |

---

### Running Queries on Associated Data:

~~~~sql
INSERT INTO
  photos (url, user_id)
VALUES
  ('http://two.jpg', 1),
  ('http://24two.jpg', 1),
  ('http://365.jpg', 1),
  ('http://11.jpg', 2),
  ('http://five.jpg', 3),
  ('http://1000.jpg', 4);
---
SELECT * FROM photos;
~~~~

| **id** | **url**          | **user_id** |
|--------|------------------|-------------|
| 1      | http://one.jpg   | 4           |
| 8      | http://two.jpg   | 1           |
| 9      | http://24two.jpg | 1           |
| 10     | http://365.jpg   | 1           |
| 11     | http://11.jpg    | 2           |
| 12     | http://five.jpg  | 3           |
| 13     | http://1000.jpg  | 4           |

Some users have multiple photos. 

---

### Foreign Key Constraints Around Insertion:

**Data Consistency**

**3 scenarios**:
-	We insert a photo that is tied to a user that exists – everything works ok. 
-	We insert a photo that refers to a user that does not exist – what would happen here? This scenario doesn’t make sense, so we get an error. “insert or update on table “photos” violates foreign key constraint “Photos_user_id_fkey” – the term foreign key constraint means that Postgres wants to make sure that whenever we set up this foreign key it tries to reference a record that actually exists inside the user table. This is a very good feature to have. It makes sure that you don’t accidentally insert data into the database that doesn’t make sense at all. 
-	We insert a photo that isn’t tied to any user – like an “image of the day”, a photo that isn’t intended to be associated with any user whatsoever. What would we do in that case? Instead of an id integer, we can put in NULL – the value NULL is very special in SQL and Postgres, it means there’s no value, nothing, so we can run this and are able to insert that record successfully. 

~~~~sql
INSERT INTO photos (url, user_id)
VALUES ('http://3333.jpg', NULL);
~~~~

---

### Constraints Around Deletion

In photos table we have 3 photos with user with id of 1. What would happen if we deleted the user with id of 1? If that would happen we would have some ‘dangling keys / dangling references’ , in other words these photos are now trying to reference a user that doesn’t exist and never will exist. Remember our ids are using the SERIAL data type, whenever we use that SERIAL data type no id ever gets reused even if a record with some given id gets deleted. So when we delete the user with id of 1 there will never be another user with id of 1. And so these photos are never going to be referencing any user again, so we would have to add some code inside our app to somehow detect that we have some photos that aren’t referencing any user that exists, and that would be annoying. 
   
So when we make use of FOREIGN Keys with can specify some options, for exactly what we want to have happen whenever we try to delete a record that some other rows are dependent upon. In this case we would say that these 3 photos are dependent upon that user with id of 1. 
   
**On Delete Option**
| DELETE OPTIONS                                  | RESULT                                                                         |
|-------------------------------------------------|--------------------------------------------------------------------------------|
|     ON DELETE RESTRICT (Default   behaviour)    |     Throw an error                                                             |
|     ON DELETE NO ACTION                         |     Throw an error                                                             |
|     ON DELETE CASCADE                           |     Delete the photo too!                                                      |
|     ON DELETE SET NULL                          |     Set the ‘user_id’ of the photo to NULL                                     |
|     ON DELETE SET DEFAULT                       |     Set the ‘user_Id’ of the photo to   a default value, if one is provided    |

update or delete on table "users" violates foreign key constraint "photos_user_id_fkey" on table "photos"

---
### Testing Deletion Constraints:

~~~~sql
CREATE TABLE photos (
  id SERIAL PRIMARY KEY,
  url VARCHAR(200),
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);

DELETE FROM users
WHERE id = 1;

SELECT * FROM photos;
~~~~

A discussion forum has many posts and many replies inside of a post, if you delete a post or discussion thread then you will probably want to delete the replies inside of it as well. Same thing to a blog post with comments because there is no expectation that anyone will want to read those comments.

---
