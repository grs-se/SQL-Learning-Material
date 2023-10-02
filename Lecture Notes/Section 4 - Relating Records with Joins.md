# SECTION 4: Relating Records with Joins

## Queries with Joins and Aggregations:

The more tables we have, the more interesting questions we can answer

- Find all the comments for the photo with ID = 3, along with the username of the comment author.
- Find the photo with ID = 10 and get the number of comments attached to it
- Find the average number of comments per photo Find the user with the most activity (most comments + most photos)
- Find the photo with he most comments attached to it Calculate the average number of characters per comment

**Joins**:

- Produces values by merging together rows from different related tables
- Use a join most times that you’re asked to find data that involves multiple resources
  Aggregation:
- Looks at many rows and calculates a single value
- Words like ‘most’, ‘average’, ‘least’ are a sign that you need to use an aggregation
  Joining Data from Different Tables

So if we could write some sql to take a look at each comment, extract the contents, take that user_id, and look up that related user_Id in the users table, that would probably solve this query.

**Problem**: Find all the comments for the photo with ID = 3, along with the username of the comment author:

```sql
SELECT contents, username
FROM comments
JOIN users ON users.id = comments.user_id;
```

**Query Result**
| contents | username |
|-----------------------------------------------------------|---------------------|
| Quo velit iusto ducimus quos a incidunt nesciunt facilis. | Micah.Cremin |
| Non est totam. | Frederique_Donnelly |
| Fuga et iste beatae. | Alfredo66 |

We are trying to refer to some columns or information in both tables, so we want accesst o contents from comments, and username from users. Because we are trying to access information from 2 different tables, that is a sign we might want to use a join.
We can think of FROM as doing some initial selection of the rows we want to operate on.
When we say JOIN users, that means we want to somehow combine information from the rows we already selected during FROM, with some rows from the users table, so we want to somehow take these rows from comments and somehow match them together with rows from users.  
Behind the scenes whenever you see the JOIN statement you can imagine that our db is making a temporary copy of that initial table, in this case comments, and then imagine that this table gets renamed to comments_with_users, and then finally we can imagine that the db is gong to iterate through all these different rows and is going to match all of these rows together with the row from the users table using the matching statement we’ve put on the other side of ON, so : users.id = comments.user_id; This says that for every row of the users table take a look atht eid and match that up with eh row from the comments table, sepcifcally using the value out of the user id column.  
We end up with a temporary, imaginary table and we can refer to or select any of the different columns inside of here. In this case we selected only contents and username and we didn’t really care about any of the other columsn inside of here, so we can imagine that all the other columns fall away and we are left with just comments and username. If we wanted to we could adjust that SELECT statement and select other columns out of this table, for example photo_id.

**Another Use of the Join Expression**:
**Problem**:For each comment, list the contents of the comment and the URL of the photo the comment was added to:

```sql
SELECT contents, url
FROM comments
JOIN photos ON photos.id = comments.photo_id;
```

**Query Result**
| contents | url |
|-----------------------------------------------------------|------------------------|
| Quo velit iusto ducimus quos a incidunt nesciunt facilis. | http://marjolaine.name |
| Non est totam. | http://chet.net |
| Fuga et iste beatae. | https://kailyn.name |
| Molestias tempore est. | http://chet.net |

```sql
SELECT title, name
FROM books
JOIN authors ON authors.id = books.author_id;
```

**Alternative Forms of Syntax**
**Notes on Joins**

- Table order between ‘FROM’ and ‘JOIN’ frequently makes a difference
- We must give context if column names collide
- Tables can be renamed using the ‘AS’ keyword
- There are a few kinds of joins!

In some scenarios we can flip the order of tables and we get the exact same result, but other scenarios where we effects the ultimate result.

We must give context if column names collide.  
Duplicate column names = “column reference "id" is ambiguous”. Whenever you want to select a column that is listed twice or more inside of a JOIN table you have to be more explicit: you have to designate which of those different id columsn you want by putting the name of table it’s coming from in front of the selector, i.e. SELECT photos.id.

```sql
SELECT photos.id AS photo_id, comments.id
FROM comments
JOIN photos ON photos.id = comments.photo_id;
```

| photo_id | id  |
| -------- | --- |
| 4        | 1   |
| 5        | 2   |
| 3        | 3   |

Tables can be renamed using the ‘AS’ keyword:

```sql
SELECT photos.id AS photo_id, c.id
FROM comments AS c
JOIN photos ON photos.id = c.photo_id;
```

| photo_id | id  |
| -------- | --- |
| 4        | 1   |
| 5        | 2   |
| 3        | 3   |

You will see this shortened syntax with the abbreviation being used especially when you have to embed a lot of different references to your table.  
One other way of you’ll see this syntax shoretened up is to drop the AS keyword and write “FROM comments c”.

```sql
SELECT photos.id AS photo_id, c.id
FROM comments c
JOIN photos ON photos.id = c.photo_id;
```

But AS keyword adds clarity and is recommended.

---

## Missing Data in Joins

**Problem**: list all photos

```sql
SELECT url, username
FROM photos
JOIN users ON users.id = photos.user_id;

INSERT INTO photos(url, user_id)
VALUES ('https://banner.jpg', NULL);

SELECT url, username
FROM photos
JOIN users ON users.id = photos.user_id;
```

We don’t see the photo we just added, so the goal of the query is now no longer satisfied: we are not getting all photos. Instead, we are only listing all photos which are somehow related to a user. This is a problem. We might be trying to pull some information about every photo to get some idea about how many photos in total there are, or maybe the total storage size of all photos inside of the db. It’s always critical to make sure we fulfill each query we are trying to answer exactly, and right now we’re not because we’re not listing all photos.

**Why wasn’t it included?**

What is happening behind the scenes with the JOIN logic?

So inside the query itself we are first saying take all the different rows from photos, so that’s going to include all 4 rows inside the photos table, so we can imagine that this imaginary, tempoaray table photos_with_users is going to have all the rows from photos inside there including the one with the user_id NULL, and then the JOIN is gojgn to be done, so we merge in all the rows from users, and our joining relation, or the criteria for doing the join, is defined a user_id = photos.user.id, so we take a look at user.id 2, but when we get to user_id = NULL, so in this case there is no user inside the users table with the id of NULL, so no match, nothing to match up to this row, the join we are doing right now is one of a particular set of joins, several different kinds of joins, with the join we are currently using, if there is ever a row in our source table of photos that does not match up with the row from users, then that row gets dropped from the overall results set, so because we cannot find a match for that row we don’t see it in the final result.  
But there are other types of JOIN which will allow us to include that photo.

---

## Four Kinds of Join

### Inner Join

Whenever you use the keyword JOIN by itself inside of a query, that is by default an Inner Join. So it’s going to join these two tables together in a very particular way. You can write out JOIN or alternatively INNER JOIN, either one 100% equivalent.
The way Inner Joins: it will merge these two tables together. Whenever there are rows or values that don’t match up to a row in the other table, that row will be dropped from the overall result set.

With an INNER JOIN we only keep rows which match up to a row in the other table.
The term inner join comes from the overlapping sets of rows, we only take the inner section where we have a perfect match between them.

### Left Outer Join

In a Left Outer Join, if anything in the photos table does not match up to the users table, we are not going to drop it off.

```sql
SELECT url, username
FROM photos
LEFT JOIN users ON users.id = photos.user_id;
```

So once again, in the imaginary table, if a row is not matched we do not throw it away, instead we put in a row which contains empty or null values.

### Right Outer Join:

Essentially exact opposite of Left Outer Join, so we take all users and include them, even if they don’t match up to something from photos, and of course we still keep all the records that match up as well.

```sql
SELECT url, username
FROM photos
RIGHT JOIN users ON users.id = photos.user_id;
```

The interesting part: any unmatched rows from the left hand side are going to be dumped, so in the Right Outer Join we dump any rows on the left hand side, from photos that don’t match up, but then we include any rows from Users, even if they didn’t have any photos. The actual column values here will be set to NULL for the relevant post.

### Full Join:

In a full join we just say return everything, we don’t care whether or not there is any correct merging going on, just try to merge as many rows as possible, and if we can’t just include all the others.  
Rather than throw away the row which has nothing matching up in the users table, and we set all the relevant columns for some related user to NULL and then in addition we’re also going ot include this user here, even though there is no matching photo for them, and we would give htme some values also set to NULL.

Joins: This can become a very complicated topic.

So to solve the query we would use a LEFT Outer Join:

```sql
SELECT url, username
FROM photos
LEFT JOIN users ON users.id = photos.user_id;
```

---

## Each Join in Practice

**Right outer Join example**
List all users and their associated photos and even list users who do not have a photo.
So add in a new user to the users table, and add them without a photo associated with them. We’ll then try to do a right outer join and we’ll see that even though this user does not have any photos we wills till see them listed.

```sql
INSERT INTO users(username)
VALUES ('Nicole');

SELECT url, username
FROM photos
RIGHT JOIN users ON users.id = photos.user_id;
```

Query Result
| url | username |
|------------------------|--------------|
| http://johnson.info | Reyna.Marvin |
| http://kolby.org | Reyna.Marvin |
| http://marjolaine.name | Reyna.Marvin |
| null | Nicole |

### Full Join

```sql
SELECT url, username
FROM photos
FULL JOIN users ON users.id = photos.user_id;
```

Query Result
| url | username |
|------------------------|---------------------|
| http://johnson.info | Reyna.Marvin |
| http://kolby.org | Reyna.Marvin |
| http://marjolaine.name | Reyna.Marvin |
| https://elinore.name | Micah.Cremin |
| https://alayna.net | Frederique_Donnelly |
| null | Nicole |
| https://banner.jpg | null |

No matching photo, and no matching user. FULL JOIN gives us all results even if they don’t match up on either rside.
