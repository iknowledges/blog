# Install TimescaleDB

## Install Postgres

1. Download [Postgres 18](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads), then run the installer.

- unckecked the option of "Stack Builder may be used to download and install additional tools, drivers and applications to complement your PostgreSQL installation." than click finish button.

2. Add the path `F:\Program Files\PostgreSQL\18\bin` to system environment variables. Then run following command from the command line:

```
pg_config
```

## Install TimescaleDB

1. Download [timescaledb-postgresql-18-windows-amd64.zip](https://www.tigerdata.com/docs/self-hosted/latest/install/installation-windows#supported-platforms) and unzip to a directory such as `F:\Program Files\timescaledb`. Then right-click `setup.exe`, choose `Run as Administrator`.

- Do you want to run timescaledb-tune.exe now? [(y)es / (n)o]: yes
- Please enter the path to your postgresql.conf: `F:\Program Files\PostgreSQL\18\data`
- shared_preload_libraries needs to be updated... Is this okay? [(y)es / (n)o]: yes
- Tune memory/parallelism/WAL and other settings? [(y)es / (n)o]: no

2. Restart the Postgres service

```
# Run PowerShell as Administrator
Restart-Service postgresql-x64-18
```

## Add the TimescaleDB extension to your database

1. Connect to a database on your Postgres instance

```
# psql -d "postgres://<username>:<password>@<host>:<port>/<database-name>"
psql -d "postgres://postgres@localhost:5432/postgres"
```

2. Add TimescaleDB to the database

```
CREATE EXTENSION IF NOT EXISTS timescaledb;
```

3. Check that TimescaleDB is installed

```
\dx
# exit
\q
```

#### Reference

- [Install TimescaleDB on Windows](https://www.tigerdata.com/docs/self-hosted/latest/install/installation-windows)