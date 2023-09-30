# SECTION 3: Working with Tables

### Approaching Database Design:

**What tables should we make?**
-	Common features (like authentication ,comments, etc) are freuqnelty built with conventional table names and cilumns
-	What type of resources exist in your app? Create a separate table for each of these features
-	Features that seem to indicate a relationship or ownership between two types of resoices need to be reflected in our table design. 



- If we use type integer we would have to write code to manually increment integer each time a row is inserted. 
- SERIAL datatype tells postgres to generate values automatically. We don't provide an id at all, only other fields, postgres fills in id for us. 
- After SERIAL we add on PRIMARY KEY, which adds special perfomrance benefits to looking up records when we are using the id

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

  
Primary keys a easier to understand, but foreign keys are a bit harder to remember what it does. 
Primary Keys	Foreign Keys
Each row in every table has one primary key	Rows only have this is they belong to another record
No other row in the same table can have the same value	Many rows in the same table can have the same foreign keys
99% of the time called ‘id’ – bad idea to set ‘name’ as primary key (i.e. Shanghai in China and also in America)	Name varies, usually called something like ‘xyd_id’
Either an integer of a UUID	Exactly equal to the primary key of the referenced row
Will never change	Will change if the relationship changes.
  
	
	




~~~~sql
CREATE TABLE users (
id SERIAL PRIMARY KEY, 
username VARCHAR(50)
);
~~~~