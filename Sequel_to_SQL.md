#### Aggregate functions

Using the `COUNT` function
```sql
    SELECT COUNT(*) FROM movies;
    SELECT COUNT(column_name) FROM table_name;
```
Other aggregate functions (work only with numbers)
```sql
    SELECT SUM(column_name) FROM table_name;
    SELECT AVG(column_name) FROM table_name;
    SELECT MIN(column_name) FROM table_name;
    SELECT MAX(column_name) FROM table_name;
```
Using the `GROUP BY` clause
```sql
    SELECT genre, sum(cost) FROM Movies GROUP BY genre;
```
Using the `HAVING` clause to only include genres that have more than 1 movie.
```sql
    SELECT genre, sum(cost) FROM Movies GROUP BY genre HAVING COUNT(*) > 1;
```
#### Constraints

Column constraints
```sql
    CREATE TABLE Promotions (
        id int,
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15)
    );
```
Table constraints (same effect as column constraint above)
```sql
    CREATE TABLE Promotions (
        id int,
        name varchar(50) NOT NULL,
        category varchar(15),
        CONSTRAINT unique_name UNIQUE (name, category)
    );
```
`Primary key` constraint can only be set once per table
```sql
    CREATE TABLE Promotions (
        id int PRIMARY KEY,
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15)
    );
```
`Foreign key` constraint
```sql
    CREATE TABLE Promotions (
        id int PRIMARY KEY,
        movie_id int REFERENCES movies(id),
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15)
    );
```
You can also omit id, because its the default value
```sql
    CREATE TABLE Promotions (
        id int PRIMARY KEY,
        movie_id int REFERENCES movies,
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15)
    );
```
`Foreign key` table constraint
```sql
    CREATE TABLE Promotions (
        id int PRIMARY KEY,
        movie_id int,
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15),
        FOREIGN KEY (movie_id) REFERENCES movies
    );
```
`check` constraint
```sql
    CREATE TABLE Movies (
        id int PRIMARY KEY,
        title varchar(50) NOT NULL UNIQUE,
        genre varchar(20),
        duration int CHECK(duration > 0)
    );
```
---
#### Normalization

> Normalization is the process of reducing duplication in database tables

**First Normal Form Rule:** *Tables must not contain repeating groups of data in 1 column*.

**Second Normal Form Rule:** *Tables must not contain redundancy (unnecessary repeating information)*

---

Join table creation
```sql
    CREATE TABLE Actors_Movies (
        actor_id int REFERENCES Actors,
        movie_id int REFERENCES Movies
    );
```
One-to-one relationship
```sql
    CREATE TABLE Rooms (
        id int PRIMARY KEY,
        seats int,
        movie_id int UNIQUE,
        FOREIGN KEY (movie_id) REFERENCES Movies
    );
```
#### Inner Join
```sql
    SELECT * FROM Movies INNER JOIN Reviews ON Movies.id = Reviews.movie_id;
```
or (same rusult)
```sql
    SELECT * FROM Reviews INNER JOIN Movies ON Reviews.movie_id = Movies.id;
```
Extract only some columns
```sql
    SELECT Movies.title, Reviews.review 
    FROM Movies 
    INNER JOIN Reviews 
    ON Movies.id = Reviews.movie_id;
```
`Inner Join` on multiple tables
```sql
    SELECT Movies.title, Genres.name 
    FROM Movies 
    INNER JOIN Movies_Genres 
    ON Movies.id = Movies_Genres.movie_id 
    INNER JOIN Genres 
    ON Movies_Genres.genre_id = Genres.id
    WHERE Movies.title = "Peter Pan"
    ORDER BY Movies.title ASC;
```
#### Aliases

Using column aliases
```sql
    SELECT Movies.title AS Films, Reviews.review AS Reviews 
    FROM Movies 
    INNER JOIN Reviews 
    ON Movies.id = Reviews.movie_id;
```
or simply omitting `AS`
```sql
    SELECT Movies.title Films, Reviews.review Reviews 
    FROM Movies 
    INNER JOIN Reviews 
    ON Movies.id = Reviews.movie_id;
```
For multiple words aliases or capitalization use quotes
```sql
    SELECT Movies.title "Weekly Movies", Reviews.review "Weekly Reviews" 
    FROM Movies 
    INNER JOIN Reviews 
    ON Movies.id = Reviews.movie_id;
```
Using table aliases
```sql
    SELECT m.title "Weekly Movies", r.review "Weekly Reviews" 
    FROM Movies m
    INNER JOIN Reviews r
    ON m.id = r.movie_id;
```
#### Outer Join

Left Outer Join : *All movies info and only the reviews associated with them*
```sql
    SELECT * 
    FROM Movies 
    LEFT OUTER JOIN Reviews 
    ON Movies.id = Reviews.movie_id;
```
or finding matching columns with aliases
```sql
    SELECT m.title, r.review 
    FROM Movies m
    LEFT OUTER JOIN Reviews r 
    ON m.id = r.movie_id 
    ORDER BY r.id;
```
Right Outer Join : *All reviews and only movies that are associated with reviews*
```sql
    SELECT * 
    FROM Movies 
    RIGHT OUTER JOIN Reviews 
    ON Movies.id = Reviews.movie_id;
```
or finding matching columns with aliases
```sql
    SELECT m.title, r.review 
    FROM Movies m
    RIGHT OUTER JOIN Reviews r 
    ON m.id = r.movie_id 
    ORDER BY r.id;
```

#### Subqueries
A query in a query. Notice `IN` clause
```sql
    SELECT Sum(sales) 
    FROM Movies 
    WHERE id IN 
        (SELECT movie_id 
        FROM Promotions 
        WHERE category = "Non-cash");
```
Same result is achieved by using join
```sql
    SELECT Sum(m.sales) 
    FROM Movies m 
    INNER JOIN Promotions p 
    ON m.id = p.movie_id 
    WHERE p.category = 'Non-cash';
```

* **Subquery**: easier to read
* **JOIN query**: better for performance

Subquery syntax
```sql
    WHERE <field> IN (<subquery>) 
    WHERE <field> NOT IN (<subquery>)
```
Using correlated subqueries
```sql
    SELECT * FROM Movies WHERE duration > 
    (SELECT avg(duration) FROM Movies);
```
**Note!!!**: *aggregate function are not allowed in WHERE*

That's why we have to use a subquery in the case above
