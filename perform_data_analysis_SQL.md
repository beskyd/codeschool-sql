### PostgreSQL

---

In Cloud9 start the **PostgreSQL Server**
```bash
    sudo service postgresql start
```
Connect to the service
```bash
    psql
```
List all databases
```postgresql
    \l
```
Create a PostgreSQL db
```postgresql
   CREATE DATABASE "groceries" 
```
Connect to db
```postgresql
    \connect db_name
```
Display tables
```postgresql
    \dt
```
Display columns of the table called `groceries`
```postgresql
    \d groceries
```
Copy entries from a `csv` file to the db
```sql
    COPY spending FROM '/home/ubuntu/workspace/codeschool/spending.csv' DELIMITER ',' CSV;
```
In case there is headers, add `HEADER` keyword
```sql
    COPY spending FROM '/home/ubuntu/workspace/codeschool/spending.csv' HEADER DELIMITER ',' CSV;
```
