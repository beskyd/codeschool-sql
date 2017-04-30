###### Gist shortlink: https://git.io/vV0xB

#### Official Living Doc for Postgres in C9: https://community.c9.io/t/setting-up-postgresql/1573

##### @mikeumus' Postgres in Cloud9 Notes:
 - http://stackoverflow.com/questions/28177912/postgres-database-setup-on-cloud9-asks-for-sudo-password
 - http://stackoverflow.com/questions/11919391/postgresql-error-fatal-role-username-does-not-exist
 - `$ sudo -u postgres -i` or `sudo sudo -u postgres psql` <-- use this one in C9
 - `$ sudo -s` then `sudo -u postgres psql` or `sudo su - postgres`
 - `$ createuser -d -P -s starseed`
 - `$ createdb -O starseed starseeddb`
 - Run Postgres Server `$ sudo service postgresql start` 
 - Set Postgres User `pg_hba.conf`: http://stackoverflow.com/a/18664239/1762493
 - Log into db `psql -U starseed -d starseeddb -W`

##### @mashcode's Cloud 9 recipe:
 - In the c9 Project screen select Django template
 - Clone from Git copy/paste `https://github.com/NewSpaceNYC/starseed.git`
 - Create workspace
 - In the workspace click Open to launch the project into a new window
 - `sudo pip install -r requirements.txt `
 - `$ sudo -s` then `sudo -u postgres psql` or `sudo su - postgres`
 - `$ createuser -d -P -s starseed` 
 - Enter password
 - `$ createdb -O starseed starseeddb` 
 - `\q`
 - Run Postgres Server (see above)

Install psycopg2 for postgres
 - http://stackoverflow.com/a/5450183
 - `$ sudo apt-get install libpq-dev python-dev`
 - `$ sudo pip install psycopg2`

PSQL Commands
 - `sudo -u postgres psql phishingdb` Enter phishingdb in psql: http://www.postgresql.org/docs/8.4/static/tutorial-accessdb.html
 - `\l` List Databases
 - `\?` List Available Commands
 - `\d+ <table_name>` List columns
 - `\c postgres postgres` Change user and database
 - `\q` or `Ctrl+D` Exit PSQL
 - `\dt`, `\dS` or `SELECT * FROM pg_catalog.pg_tables` List Tables
 - `DROP DATABASE [ IF EXISTS ] name` http://www.postgresql.org/docs/8.2/static/sql-dropdatabase.html
 - `ALTER DATABASE one RENAME TO "one-two";` http://stackoverflow.com/a/3942826/1762493
 - `ALTER USER "user_name" WITH PASSWORD "new_password";` http://stackoverflow.com/a/12721095
 - `UPDATE Asset SET my_field = 0;` or `$ UPDATE Asset SET my_field = 0 WHERE my_field = 1` http://stackoverflow.com/questions/20944603/reset-a-boolean-field-in-all-rows-in-django-db 
 - `pg_dump dbname | gzip > filename.gz` download db dump: http://www.postgresql.org/docs/9.1/static/backup-dump.html#BACKUP-DUMP-ALL
 - Fix Django complaining about ghost migrations/models: http://stackoverflow.com/questions/28531889/how-to-resolve-programmingerror-column-does-not-exist-after-adding-model-fiel


Notes for running Posgres in C9 from the open-source Starseed project: 
 - https://ide.c9.io/mikeumus/starseed
 - https://github.com/NewSpaceNYC/Starseed