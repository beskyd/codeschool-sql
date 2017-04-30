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