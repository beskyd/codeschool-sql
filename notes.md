### `postgresql` on Cloud9

---

Start the server
```bash
    sudo service postgresql start
```
Create password
```bash
    sudo sudo -u postgres psql
```
Set the password
```postgresql
    \password postgres
```
then quit
```postgresql
    \q
```
Log into the client
```bash
    psql -U postgres -h localhost
```
