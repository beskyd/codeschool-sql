
```sql

    SELECT title FROM movies WHERE id = 2;  
    SELECT title, duration FROM movies WHERE genre = 'Adventure';
    SELECT title FROM movies WHERE id = 1 AND genre = 'Comedy';
    SELECT title FROM movies WHERE id = 1 OR genre = 'Comedy';
    
    <!--select all-->
    SELECT * FROM movies WHERE title = 'The Kid';
    SELECT * FROM movies WHERE duration > 100;
    SELECT * FROM movies WHERE duration <= 95;
    SELECT * FROM movies WHERE genre <> 'Horror';
    
    <!--sort by duration (by default ascending)-->
    SELECT title FROM movies ORDER BY duration;
    
    <!--to sort descending-->
    SELECT title FROM movies ORDER BY duration DESC;
    
    <!--add entries to db-->
    INSERT INTO movies (id, title, genre, duration) VALUES (5, 'The Circus', 'Comedy', 71);
    
    <!--if add to all columns-->
    INSERT INTO movies VALUES (5, 'The Circus', 'Comedy', 71);
    
    
    <!--edit entries in db-->
    UPDATE movies SET genre = 'Romance', duration = 70 WHERE id = 5;
    
    <!--edit entries in several rows -->
    UPDATE movies SET genre = 'Romance' WHERE id = 5 OR id = 7;
    
    
    <!--delete entries-->
    DELETE FROM movies WHERE id = 5;
    DELETE FROM movies WHERE duration < 100;
    
    <!--WARNING!!! The following query will delete all data from the movies table-->
    DELETE FROM movies;
    
    
    <!--create new db-->
    CREATE DATABASE Chaplin Theaters;
    
    <!--delete db-->
    DROP DATABASE Chaplin Theaters;
    
    <!--create table inside db-->
    CREATE TABLE movies 
    (
    id int,
    title varchar(50),
    genre varchar(100),
    duration int
    );
    
    <!--delete table from db-->
    DROP TABLE movies;
    
    
    <!--add, modify or remove columns in the table-->
    ALTER TABLE movies ADD COLUMN ratings int;
    ALTER TABLE movies DROP COLUMN ratings;

    <!--rename column (for Oracle and PostgreSQL)-->
    ALTER TABLE table_name RENAME COLUMN old_name TO new_name;
    <!--rename column (for MySQL and MariaDB)-->
    ALTER TABLE table_name CHANGE COLUMN old_name TO new_name;
    
    <!--modify column (for Oracle, MySQL, MariaDB)-->
    ALTER TABLE table_name MODIFY column_name column_type;
    <!--modify column (for PostgreSQL)-->
    ALTER TABLE table_name ALTER COLUMN column_name TYPE column_definition;



```