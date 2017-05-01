Using the COUNT function
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
Using the GROUP BY clause
```sql
    SELECT genre, sum(cost) FROM Movies GROUP BY genre;
```
Using the HAVING clause to only include genres that have more than 1 movie.
```sql
    SELECT genre, sum(cost) FROM Movies GROUP BY genre HAVING COUNT(*) > 1;
```
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
Primary key constraint can only be set once per table
```sql
    CREATE TABLE Promotions (
        id int PRIMARY KEY,
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15)
    );
```
Foreign key constraint
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
Foreign key table constraint
```sql
    CREATE TABLE Promotions (
        id int PRIMARY KEY,
        movie_id int,
        name varchar(50) NOT NULL UNIQUE,
        category varchar(15),
        FOREIGN KEY (movie_id) REFERENCES movies
    );
```
Check constraint
```sql
    CREATE TABLE Movies (
        id int PRIMARY KEY,
        title varchar(50) NOT NULL UNIQUE,
        genre varchar(20),
        duration int CHECK(duration > 0)
    );
