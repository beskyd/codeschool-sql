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
